Mac下Charles使用

> 使用场景：用于将线上页面在本地进行调试，将线上文件指向本地，本地修改以后可看到效果并调试。解决App Web View调试问题。

#### 1、下载Char

首先，你需要下载一个`Charles` [https://www.charlesproxy.com/latest-release/download.do](https://www.charlesproxy.com/latest-release/download.do)

#### 2、手机配置代理到Charles

a、保证手机和Mac处于同一局域网下

b、打开\`charles -&gt; help -&gt; SSL Proxying -&gt; Install Charles Root Certificate on a Mobile Device or Remote Browser\`，会弹出对话框，提示你在手机端需要输入的代理并在手机访问chls.pro/ssl下载证书

c、配置完成后，手机端访问页面或者发出请求时，即可在Charles上获得数据信息

#### 3、将线上文件代理到本地

那么问题来了，怎样将线上文件代理到本地，使我们能够实时调试呢？

点开Tools下面的Map Local，点击新增。

也可以在拉取到的资源中右键，选择Map Local，自动添加数据，仅需要配置Map to的路径。

> 批量代理看面板解释应该是通过\*替换，具体还需要再操作试试。

#### 4、在手机端访问，并查看结果

如果成功了，那notes里面会提示出代理信息。此时可以在本地修改该文件，在手机查看效果了~

##### 参考资料

[http://blog.csdn.net/geelong2010/article/details/49150237](http://blog.csdn.net/geelong2010/article/details/49150237)

[http://blog.csdn.net/s0228g0228/article/details/52778895](http://blog.csdn.net/s0228g0228/article/details/52778895)

[https://www.cnblogs.com/wonyun/p/5586746.html](https://www.cnblogs.com/wonyun/p/5586746.html)

[https://www.cnblogs.com/gabin/p/6999686.html](https://www.cnblogs.com/gabin/p/6999686.html)

##### 部分问题

[http://www.jb51.net/article/102620.htm](http://www.jb51.net/article/102620.htm)

