## Gitbook的使用


### 一. 快速上手
* node环境
* 全局安装gitbook命令行工具 `npm i gitbook-cli -g `
* 初始化一个文件夹 `gitbook init`
* 开始写作

### 二. 常用指令
* 启动服务 `gitbook serve [-p][port]`
* 打包

```
 gitbook build [文件路径] [目标路径]
 // 如
 gitbook build ./ ./docs
```

### 三. 部署到github page
* 创建一个github名.github.io 的项目仓库
* 设置为主站点
* 把代码推到另一个仓库
* 把打包生成的文件推到 gh-pages分支的根目录
* 打开 https://githubname.github.io/仓库名 即可看到

### 四. 插件拓展
* 根目录添加配置文件 book.json

* 添加内容

```
{
  "author": "qiuxiaori",
  "plugins": [ // 开启的插件
    "disqus",
    "baidu"
  ],
  "pluginsConfig": { // 插件配置
    "disqus": {
      "shortName": "gitbook"
    },
    "baidu": {
      "token": "f2aa21e56274fea731f801e573a3f23a"
    }
  }
}
```

* 安装插件 `gitbook install ./`