# npm i --save和--save-dev的区别

npm install 在安装 npm 包时，有两种命令参数可以把它们的信息写入 package.json 文件，一个是npm install--save另一个是 npm install –save-dev，他们表面上的区别是--save 会把依赖包名称添加到 package.json 文件 dependencies 键下，--save-dev 则添加到 package.json 文件 devDependencies 键下。

不过这只是它们的表面区别。它们真正的区别是，npm自己的文档说dependencies是运行时依赖，devDependencies是开发时的依赖。即devDependencies 下列出的模块，是我们开发时用的，比如 我们安装 js的压缩包gulp-uglify 时，我们采用的是 “npm install –save-dev gulp-uglify ”命令安装，因为我们在发布后用不到它，而只是在我们开发才用到它。dependencies 下的模块，则是我们发布后还需要依赖的模块，譬如像jQuery库或者Angular框架类似的，我们在开发完后后肯定还要依赖它们，否则就运行不了。

另外需要补充的是：正常使用npm install时，会下载dependencies和devDependencies中的模块，当使用npm install –production或者注明NODE\_ENV变量值为production时，只会下载dependencies中的模块。

> 参考文献：http://blog.csdn.net/juzipchy/article/details/65653683



