# 简单版本webpack编译

## 1. 分析各个模块之间的依赖关系 （包括node_modules模块中的require依赖模块） depTree

      espirma将js代码转换成AST (parse.js)， 再进行深度遍历(buildDeps.js)， 找出依赖关系， 并找出模块地址（resolve.js 绝对路径）

      其中模块地址思路：
         1. 给的是绝对路径、相对路径  ---  在路径下寻找
         2. 给的模块名称  ---  入口js文件下寻找同名的js文件，返回绝对路径
         3. 在入口js文件同级的node_module文件下寻找
      （nodejs实现的是逐层向上查找 --- 默认的模块查找算法）

## 2. 将output中的每个模块的名字 -- 替换成模块的ID (writeSource.js)

## 3. 根据depTree对象，将output.js文件的拼接 （writeChunk.js）

## 4. commonjs规范的模块代码转换成一个js自执行表达式  --->  浏览器执行模块代码

*** 查找出来的依赖关系如下 ***
```
  {
      "modules": {
          "/Users/youngwind/www/fake-webpack/examples/simple/example.js": {
              "id": 0,
              "filename": "/Users/youngwind/www/fake-webpack/examples/simple/example.js",
              "name": "/Users/youngwind/www/fake-webpack/examples/simple/example.js",
              "requires": [
                  {
                      "name": "a",
                      "nameRange": [
                          16,
                          19
                      ],
                      "id": 1
                  }],
              "source": "let a = require('a');\nlet b = require('b');\nlet c = require('c');\na();\nb();\nc();\n"
          },
          "/Users/youngwind/www/fake-webpack/examples/simple/a.js": {
              "id": 1,
              "filename": "/Users/youngwind/www/fake-webpack/examples/simple/a.js",
              "name": "a",
              "requires": [],
              "source": "// module a\n\nmodule.exports = function () {\n    console.log('a')\n};"
          }
      },
      "mapModuleNameToId": {
          "/Users/youngwind/www/fake-webpack/examples/simple/example.js": 0,
          "a": 1,
          "b": 2,
          "c": 3
      }
  }
```


## 启动：
    npm link
    进入对应需要编译的目录下  re-webpack ./main.js
