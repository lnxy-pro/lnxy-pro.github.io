# Nodejs



**NodeJS**

> node 版本管理工具 n Permission denied \*\*\*. 要sudo 权限 which no 查看node路径 sudo n x.x.x 安装某个版本 n latest 安装最新版本 n stable 安装稳定版本lts n rm x.x.x 删除某个版本 n use 7.10.0 index.js 使用指定版本执行脚本 n 切换版本 n ls 查看已经安装的版本 n ls-remote --all 查看服务器上所有可用的版本



**Babel工具**

*   安装

    ```
    nnpm install --save-dev @babel/core
    ```
*   配置文件 .babelrc

    ```
    {
        "presets":[]
        "plugins":[]
    }
    ```
*   使用presets字段根据不同语言设置转码规则

    ```
    npm install --save-dev @babel/preset-env    //最新转码规则
    npm install --save-dev @babel/preset-ract    //    react转码规则

    将规则加入 .babelrc :

    {
      "presets":[
                "@babel/env",
                "@babel/preset-react"
      ],
      "plugins":[]
    }
    ```
*   命令行转码

    ```
    Babel提供命令工具@babel/cli
    ```
*   babel-node模块(babel/node)

    ```
    npx babel-node xxx.js   // 执行命令行代码,babel/node模块的命令提供es6的repl环境,支持node的repl功能
    ```
*   @babel/register模块

    该模块改写require命令,为它加上一个钩子,每当加载.js .jsx .es 和.es6后缀名的文件就会先用babel进行转码

    ```
    require("@babel/register")    // 该模块只会对require加载的文件转码
    require("./es6.js")
    ```
*   polyfill

    babel默认转换新语法,不转换新api.如果需要对api解码,需要用到不同方法,例: Array.from 就可以使用core-js和regenerator-runtime
*   浏览器环境

    使用@babel/standalone 浏览器版本,插入网页

    ```
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script type="text/babel">
      // Your ES6 code
    </script>
    ```
