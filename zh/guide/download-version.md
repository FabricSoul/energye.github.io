# 版本下载

> 使用 [energy](/course/install-env) 命令行工具自动安装开发环境

### Energy 对 CEF 版本支持
- Windows
  - Windows 10, 11 `energy-latest`
  - Windows 7, 8/8.1 and Windows Server 2012  `CEF 109.1.18`
  - Windows XP SP3  `CEF 49.0.2623`
- MacOSX
  - 默认 : `energy-latest`
- Linux
  - GTK3 默认 : `energy-latest`
  - GTK2 可选 : `CEF 106.1.1`
- Flash 版本
  - CEF 89.0.18
  - 取决于你使用的 `liblcl` 动态链接库
  - 例如: `liblcl-87` 只能在 `CEF 89.0.18` 使用

### Energy 的 CEF 版本更新

``` text
当前energy落后于CEF最新稳定版本
```

### CEF 和 Liblcl 版本匹配

|liblcl|CEF|Desc|OS|
|-|-|-|-|
|liblcl|CEF|Current energy-latest version|ALL|
|liblcl-49|CEF-49|Support Windows XP SP3|Windows|
|liblcl-87|CEF-87|Support Flash|ALL|
|liblcl-106|CEF-106|Support Linux GTK2|Linux|
|liblcl-109|CEF-109|Support Windows 7, 8/8.1 and Windows Server 2012|Windows|

<span style="color:red;">注:</span> 不同发行版本严格区分 liblcl 和 cef 版本号内的匹配

### Windows XP
Windows XP SP3 仅能使用 Go1.11.xx 版本开发

|LIBLCL Download |CEF Download |Golang |
|-|-|-|
| [WindowsXPSP3-64](https://sourceforge.net/projects/liblcl/files/v2.3.7/liblcl-49.WindowsXP_SP3_64.zip) <br> [WindowsXPSP3-32](https://sourceforge.net/projects/liblcl/files/v2.3.7/liblcl-49.WindowsXP_SP3_32.zip) | [Windows64](https://gitee.com/energye/assets/releases/download/cef/cef_binary_49.0.2623%20chromium-49.0.2623.110_windows64.zip) [Windows32](https://gitee.com/energye/assets/releases/download/cef/cef_binary_49.0.2623%20chromium-49.0.2623.110_windows32.zip) | [Windows64](https://studygolang.com/dl/golang/go1.11.13.windows-amd64.msi)  [Windows32](https://studygolang.com/dl/golang/go1.11.13.windows-386.msi) |


<script setup>
import DownloadVersionComponent from '../../components/download-version.vue'
</script>

## 版本下载
<DownloadVersionComponent />