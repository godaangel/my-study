# Express+Websocket实现即时通讯

> 通过`Express+Websocket+Vue`来实现了一个简单的在线即时聊天室，演示地址如下[http://flashchat.godaangel.com](http://flashchat.godaangel.com)

##### 1、开发前准备

* express
* websocket/ws插件

##### 2、关键代码

此处作为简单演示，主要修改了通过`express-cli`构建的工程下的`bin/www`文件，并且将`http://ip:3001/chat`作为websocket接口，实现了即时通讯。代码如下：

```js
#!/usr/bin/env node

/**
 * Module dependencies.
 */

var app = require('../app');
var debug = require('debug')('wstest:server');
var http = require('http');

const url = require('url');
const WebSocket = require('ws');

/**
 * Get port from environment and store in Express.
 */

var port = normalizePort(process.env.PORT || '3001');
app.set('port', port);

/**
 * Create HTTP server.
 */

var server = http.createServer(app);

/**
 * 统一向消息添加用户数量
 * @Author   Warrenyang
 * @DateTime 2018-01-31
 */
function addUserNumber(wss, message){
  var userNumber = 0;
  wss.clients.forEach(function each(client) {
    userNumber ++;
  });
  message = JSON.parse(message);
  message.userNumber = userNumber;
  message = JSON.stringify(message);
  return message;
}

const wss = new WebSocket.Server({ server, path: '/chat'});
wss.on('connection', function connection(ws, req) {
  const location = url.parse(req.url, true);
  /**
   * 如果用户主动刷新页面断开连接，那么需要捕获error事件，不然就会报错导致程序退出
   * @Author   Warrenyang
   * @DateTime 2018-01-30
   */
  console.log("connection sussess"); 
  ws.on('error',function(e){
    console.log('出错了',e);
    var message = {
      username: '系统消息',
      msg: '某用户出错了'
    };
    message = addUserNumber(wss, JSON.stringify(message));

    wss.clients.forEach(function each(client) {
      try{
        client.send(message);
      }catch(err){
        // console.log(err);
      }
    });
  });
  ws.on('close',function(e){
    console.log('用户主动关闭连接',e);
    var message = {
      username: '系统消息',
      msg: '某用户下线了'
    };
    message = addUserNumber(wss, JSON.stringify(message));

    wss.clients.forEach(function each(client) {
      try{
        client.send(message);
      }catch(err){
        // console.log(err);
      }
    });
  });
  ws.on('message', function incoming(message) {
    console.log('received: %s', message);
    // console.log(wss.clients);
    message = addUserNumber(wss, message);
    wss.clients.forEach(function each(client) {
      client.send(message);
    });
  });
  // ws.send('something');
});


/**
 * Listen on provided port, on all network interfaces.
 */

server.listen(port);
server.on('error', onError);
server.on('listening', onListening);

/**
 * Normalize a port into a number, string, or false.
 */

function normalizePort(val) {
  var port = parseInt(val, 10);

  if (isNaN(port)) {
    // named pipe
    return val;
  }

  if (port >= 0) {
    // port number
    return port;
  }

  return false;
}

/**
 * Event listener for HTTP server "error" event.
 */

function onError(error) {
  if (error.syscall !== 'listen') {
    throw error;
  }

  var bind = typeof port === 'string'
    ? 'Pipe ' + port
    : 'Port ' + port;

  // handle specific listen errors with friendly messages
  switch (error.code) {
    case 'EACCES':
      console.error(bind + ' requires elevated privileges');
      process.exit(1);
      break;
    case 'EADDRINUSE':
      console.error(bind + ' is already in use');
      process.exit(1);
      break;
    default:
      throw error;
  }
}

/**
 * Event listener for HTTP server "listening" event.
 */

function onListening() {
  var addr = server.address();
  var bind = typeof addr === 'string'
    ? 'pipe ' + addr
    : 'port ' + addr.port;
  debug('Listening on ' + bind);
}
```

3、运行

提供了可运行的demo，开箱即用。

后端代码地址为：[https://github.com/godaangel/flashchat.godaangel.com](https://github.com/godaangel/flashchat.godaangel.com)

前端代码地址为：[https://github.com/godaangel/flashchat.godaangel.com\_fe](https://github.com/godaangel/flashchat.godaangel.com_fe)



