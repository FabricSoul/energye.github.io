### 弹出子窗口
``` go
package main

import (
	"embed"
	"fmt"
	"github.com/energye/energy/v2/cef"
	"github.com/energye/energy/v2/pkgs/assetserve"
	"github.com/energye/golcl/lcl"
	"strings"
)

//go:embed resources
var resources embed.FS

func main() {
	//全局初始化 每个应用都必须调用的
	cef.GlobalInit(nil, nil)
	//创建应用
	cefApp := cef.NewApplication()
	//指定一个URL地址，或本地html文件目录
	cef.BrowserWindow.Config.Url = "http://localhost:22022/index.html"
	cef.BrowserWindow.Config.IconFS = "resources/icon.ico"
	cef.BrowserWindow.SetBrowserInit(func(event *cef.BrowserEvent, window cef.IBrowserWindow) {
		//弹出子窗口
		event.SetOnBeforePopup(func(sender lcl.IObject, browser *cef.ICefBrowser, frame *cef.ICefFrame, beforePopupInfo *cef.BeforePopupInfo, popupWindow cef.IBrowserWindow, noJavascriptAccess *bool) bool {
			fmt.Println("beforePopupInfo-TargetUrl", beforePopupInfo.TargetUrl, strings.Index(beforePopupInfo.TargetUrl, "popup_1"), strings.Index(beforePopupInfo.TargetUrl, "popup_2"))
			if strings.Index(beforePopupInfo.TargetUrl, "popup_1") > 0 {
				popupWindow.SetSize(800, 600)
				popupWindow.HideTitle()
			} else if strings.Index(beforePopupInfo.TargetUrl, "popup_2") > 0 {
				popupWindow.SetSize(300, 300)
			}
			return false
		})
	})
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
``` 
resources
  index.html
  popup_1.html
  popup_2.html
```

### 说明

#### 窗口弹出事件
```go
cef.BrowserWindow.SetBrowserInit(func(event *cef.BrowserEvent, window cef.IBrowserWindow) {
    //chromium 弹出窗口事件
    event.SetOnBeforePopup(func(sender lcl.IObject, browser *cef.ICefBrowser, frame *cef.ICefFrame, beforePopupInfo *cef.BeforePopupInfo, popupWindow cef.IBrowserWindow, noJavascriptAccess *bool) bool {
        return false // 返回false执行框架默认逻辑
    })
})
```

在弱出窗口事件里，我们可以自定义弹出逻辑
