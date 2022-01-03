# ESLint Demo

安装: ```npm i -D eslint```

初始化配置文件 ```npx eslint -init```

常用的代码风格
- airbnb 
- standard

### 一、ESLint 文件配置参数介绍


- root：将其设置为 true 之后，ESLint 就不会再向上去查找文件检测（后续两份rc有贴测试结论）
- impliedStrict：ecmaFeatures 中的 impliedStrict 设置为 true 之后就可以在整个项目中开启严格模式
- env：指定启用的环境，通常开启的是 browser 环境和 node 环境
- plugins：引入的相关插件名，可以直接省略 eslint-plugin- 前缀
- extends：extends 中可以直接引入可共享配置包，其中也可以直接省略包名前缀 eslint-config-，其中还可以直接引入基本配置文件的绝对路径或相对路径
- rules：参数 rules 中可以设置很多 eslint 内置的规则，官方文档中规则前面有个修补图标
    - rules 属性中的规则都接收一个参数，参数的值如下：
        - "off" 或 0：关闭规则
        - "warn" 或 1：开启规则，黄色警告 (不会导致程序退出)
        - "error" 或 2：开启规则，红色错误 (程序执行结果为0表示检测通过；执行结果为1表示有错误待修复)

### 二、工程化 

#### 方法一：husky + pre-commit 钩子代码格式检测

- (1) 安装 husky ```npm i -D husky```

- (2) 修改 package.json

**检测所有的代码**

```js
// package.json
{ 
  "husky": { 
    "hooks": { 
      "pre-commit": "npm run lint", // 在commit之前先执行npm run lint命令 
    } 
  } 
  "scripts": { 
    "lint": "eslint . --ext .js,.vue", 
  } 
}
```

#### 方法二： lint-staged + pre-commit 钩子代码格式检测

**只检测暂存区的代码**

```js
// package.json
// package.json 文件

  "husky": {
    "hooks": {
      "pre-commit": "lint-staged",
    }
  },
  "lint-staged": {
    "*.vue": [
      "eslint --cache --fix"
    ],
    "*.js": [
      "eslint --cache --fix"
    ]
  },
```

###  三、package.json 与 node_modules 中依赖版本不同时处理

使用 check-dependencies 库

```js
// package.json 文件

"scripts": {
  "predev": "node check.js"
}
```

```js
// check.js
const output = require('check-dependencies')
  .sync({
    verbose: true, 
  })

if (output.status === 1) {
  throw new Error('本地依赖与package.json中不同步，请先npm install再执行')
}
```

# TODO：待完善

