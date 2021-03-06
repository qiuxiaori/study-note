## Gitbook的使用

### 一. 快速上手
1. node环境：使用gitbook需要基于 node 环境 npm 包管理器，[node官网](https://nodejs.org/en/)。
2. 安装：全局安装gitbook命令行工具 `npm i gitbook-cli -g `
3. 初始化：在你要写作的文件目录下执行命令 `gitbook init`
4. 开始写作

### 二. 常用指令
1. 启动服务： `gitbook serve [-p][port]`
2. 打包：

```js
 gitbook build [文件路径] [目标路径]
 // 如
 gitbook build ./ ./docs
```

### 三. 部署到github page
1. 创建一个github名.github.io 的项目仓库
2. 设置为主站点
3. 把代码推到另一个仓库
4. 把打包生成的文件推到 gh-pages分支的根目录
5. 打开 https://githubname.github.io/仓库名 即可看到

### 四. 插件拓展
1. 根目录添加配置文件 book.json
2. 添加内容

```json
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

### 六. 一键发布