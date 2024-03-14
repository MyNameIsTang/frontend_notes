## Vite + Antd 实现按需加载

使用 babel-plugin-import 和@rollup/plugin-babel 实现

1. 安装插件

   ```
     npm i babel-plugin-import @rollup/plugin-babel -D
   ```

2. 在根目录中创建 babel.config.js 文件

   ```
     module.exports = {
       plugins: [
         "import",
         {
           libraryName: "antd",
           libraryDirectory: "es",
           style: "css"
         },
         "antd"
       ]
     }
   ```

3. vite 配置文件 vite.config.js

   ```
     import babel from "@rollup/plugin-babel"

     export default {
       plugins: [
         babel({
           babelHelpers: "bundled",
           exclude: "node_modules/**" // 防止把node_modules里的其他库也转译
         })
       ]
     }
   ```
