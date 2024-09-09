### 浏览器事件-键盘事件监听
``` go
package main

import (
	"embed"
	"fmt"
	"github.com/energye/energy/v2/cef"
	"github.com/energye/energy/v2/consts"
	"github.com/energye/energy/v2/pkgs/assetserve"
	"github.com/energye/golcl/lcl"
)

//go:embed resources
var resources embed.FS

func main() {
	//全局初始化 每个应用都必须调用的
	cef.GlobalInit(nil, &resources)
	//创建应用
	cefApp := cef.NewApplication()
	//指定一个URL地址，或本地html文件目录
	cef.BrowserWindow.Config.Url = "http://localhost:22022/key-event.html"
	cef.BrowserWindow.Config.Title = "Energy - Key Event"
	cef.BrowserWindow.Config.IconFS = "resources/icon.ico"
	//在主窗口初始化回调函数里设置浏览器事件
	cef.BrowserWindow.SetBrowserInit(func(event *cef.BrowserEvent, browserWindow cef.IBrowserWindow) {
		event.SetOnKeyEvent(func(sender lcl.IObject, browser *cef.ICefBrowser, event *cef.TCefKeyEvent, osEvent *consts.TCefEventHandle, result *bool) {
			fmt.Printf("%s  KeyEvent:%+v osEvent:%+v\n", string(rune(event.Character)), event, osEvent)
		})
		browserWindow.Chromium().SetOnPreKeyEvent(func(sender lcl.IObject, browser *cef.ICefBrowser, event *cef.TCefKeyEvent, osEvent *consts.TCefEventHandle) (isKeyboardShortcut, result bool) {
			fmt.Printf("%s  PreKeyEvent:%+v osEvent:%+v\n", string(rune(event.Character)), event, osEvent)
			return false, false
		})
	})
	//内置http服务链接安全配置
	cef.SetBrowserProcessStartAfterCallback(func(b bool) {
		fmt.Println("主进程启动 创建一个内置http服务")
		//通过内置http服务加载资源
		server := assetserve.NewAssetsHttpServer()
		server.PORT = 22022
		server.AssetsFSName = "resources" //必须设置目录名
		server.Assets = &resources
		go server.StartHttpServer()
	})
	//运行应用
	cef.Run(cefApp)
}


```

### html示例代码
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>cookie</title>
    <style>
    </style>
    <script type="application/javascript">

    </script>
</head>
<body style="overflow: hidden;margin: 0px;padding: 0px;">
<textarea style="width: 100%;height: 500px;"></textarea>
</body>
</html>
```

### 说明
#### 事件监听
```go
Chromium().SetOnPreKeyEvent
```
在此事件中监听页面中键盘事件以及键盘组合键
