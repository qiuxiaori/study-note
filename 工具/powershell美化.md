## powershell美化教程

### 一.字体

> powershell对字体的要求hin严格，只有更纱黑体这一等宽字体支持 PowerShell 下使用中文。

* 地址：[github](https://github.com/be5invis/Sarasa-Gothic)

* 下载：使用ssh或http方式拉取源码

* node环境：源码需要使用npm安装依赖，因此需要node环境，没有的同学自己谷歌

### 二.配色

> 微软官方提供了更换 PowerShell 配色的工具：ColorTool.exe，该工具支持 iTerm 主题(以 .itermcolors 结尾的主题文件)。

* 安装
  * 使用scoop包管理工具：
    * 执行`scoop install colortool`即可。没有scoop环境的本站[scoop教程](./工具/scoop.md)

  * github下载安装：
    [地址](https://github.com/Microsoft/console/releases),解压后在解压路径执行`colortool -b campbell`应用环境变量。

* 查看已安装的主题: `colortool -s`

* 预览配色：`colortool [schmea name]` 如 `colortool cmd-legacy.ini`

* 保存配色：powershell 顶部右键 -> 属性 -> 确定 就能保存预览的配色方案啦。

* 更多主题：
  * 在[iterm2colorschemes](https://iterm2colorschemes.com/)找到喜欢的主题
  * 点击主题将打开的页面的内容复制下来
  * 新建 .itermcolors 文件：
    * 在安装目录(scoop安装) C:\Users\username\scoop\apps\colortool\current\schemes 目录新建同名主题文件，内容粘贴进去
  * 执行 `colortool [schmea name]` 并保存主题即可

### 三.主题

> 咱也没用过linux，听说oh-my-posh很好用，windows有个差不多的，咱们就把它拿来用用。

* github地址： [oh-my-posh](https://github.com/JanDeDobbeleer/oh-my-posh)

* 管理员模式启动 PowerShell

* 安装模块
```js
Install-Module posh-git -Scope CurrentUser
Install-Module oh-my-posh -Scope CurrentUser
```

* 新增修改配置文件
```js
// 如果之前没有配置文件，就新建一个 PowerShell 配置文件
if (!(Test-Path -Path $PROFILE )) { New-Item -Type File -Path $PROFILE -Force }
// 用记事本打开配置文件
// 路径 C:\Users\<用户名>\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1
notepad $PROFILE
```

* 添加内容
```js
Import-Module posh-git
Import-Module oh-my-posh
Set-Theme Paradox // 主题名
```

* 英文环境 `Set-Culture en-US`

* 修改文件中的主题名为自己想要的主题更换主题