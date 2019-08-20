# 运行Mix

因为Mix运行在webpack上，所以在使用它之前需要设置一些工具：

1. 首先，你需要安装Node.js，访问[Node website](https://nodejs.org/en/)学习如何运行它,一旦Node\(内置NPM\)安装完毕,你就不需要为每个工程在执行一遍安装了。现在你已经准备安装工程的依赖
2. 在命令行内进入到你的项目根目录，然后运行 npm install 来安装依赖的包（Laravel附带一个mix ready package.json文件，用于指导NPM）

现在你已经准备好了，你可以运行npm run dev来运行Webpack / Mix一次，npm运行watch来监听相关的文件更改并运行响应，或者npm运行prod以使用生产设置运行Mix一次（例如压缩输出）。 如果npm run watch在你的环境中不起作用，你也可以运行npm run watch-poll，或者npm run hot for Hot Module Replacement（HMR;在下一节中讨论）



