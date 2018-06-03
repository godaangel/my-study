# weex环境搭建和ios运行

#### 1、配置基础开发环境

按照官网文档给的环境先安装好，可以先忽略npm4的警告，[http://weex.apache.org/cn/guide/index.html](http://weex.apache.org/cn/guide/index.html)

#### 2、初始化项目工程

运行 `weex create awesome-app`，初始化完成以后进入工程项目，（新版本会自动运行npm）

```
cd awesome-app

npm install

npm start
```

#### 3、在ios运行

在项目根目录下运行 `weex platform add ios` \(如果不运行这个命令，run ios会报错，找不到ios工程，这个命令初始运行一次就行\)

命令执行完以后运行 `weex run ios`

### &gt;&gt;&gt;注意了&lt;&lt;&lt;

此时可能会报错

【1】首先我遇到的是

```
Error:Error: Command failed: pod update

/bin/sh: pod: command not found
```

这个问题是没有安装 `cocoapods` 造成的，需要运行 `sudo gem install cocoapods`

【2】再运行 `weex run ios`

又报了下面这个错，醉了

```
 Command failed: npm i ios-deploy --save  --unsafe-perm=true --allow-root
```

这个问题是权限不够，如果直接运行 `sudo weex run ios` 则会报下面的错

```
You cannot run CocoaPods as root
```

【解决方法】这个地方需要分开运行，首先用root权限运行`sudo npm i ios-deploy --save  --unsafe-perm=true --allow-root`，然后再运行`weex run ios`

