## 小日的第一个npm包

## _教程_
#### 一. 配置eslint

- 安装eslint: `npm i eslint -g`
- 初始化eslint配置: `eslint --init` 生成 .eslintrc.js 文件
- 忽略目录校验: 创建 .eslintignore.js 文件把不需要校验的目录放进去
- 配置scripts: 在 package.json 文件中添加scripts命令,`"lint": "eslint --fix -src"`

#### 二. 编写代码

- 在src目录下编写代码

#### 三. babel编译
> 在低版本的 node 可能还不支持某些 es6 语法，比如对象解构等，所以需要使用 babel 编译成低版本也能识别的语法。
> 我们把 src 目录里的代码编译到 lib 目录，然后我们在 package.json 里，把 "main" 改为 "lib/index.js"，这样对外暴露出去的代码就不会出现兼容性问题。

- 安装babel依赖： `npm install --save-dev babel-cli`
- 下载预设：`npm install --save-dev babel-preset-es2015`
- 配置 .babelrc 文件:
```json
  {
    "presets": ["es2015"]
  }
```
- 配置 scripts:
```json
  // package.json
  "scripts": {
    "build": "babel src -d lib",
    "build:watch": "npm run build -- --watch"
  }
```

#### 四. 编写测试
#### 五. 持续集成
#### 六. 发布

- 注册npm账号并验证
- 添加npm用户信息,执行`npm adduser`命令输入用户名和密码
- 发布 `npm publish`

#### 七. 版本升级

- 升级补丁版本号：` npm version patch`
- 升级小版本号：`npm version minor`
- 升级大版本号：`npm version major`

#### 八. 删除包

- 删除已发布的包: `npm unpublish [packageName] -force`