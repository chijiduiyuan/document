# socket.io

### 客户端api
``` js
const socket = io('ws://localhost:8000');
//connect:连接成功

socket.on('connection',()=>{
    //使用send
    socket.send('hello!');
    //或者使用emit
    socket.emit('salutations','Hello',{'mr':'john'},Unit8Array.from([1,2,3,4]))
})

//处理send数据
socket.on('message',data=>{
    console.log(data);
})

//处理emit
socket.on('greetings',(elem1,elem2,elem3)=>{
    console.log(elem1,elem2,elem3)
})

//监听与服务器断开连接事件
socket.on('disconnect',function(){
    console.log('断开连接');
})

//重连
socket.on('reconnect',function(){
    console.log('重新连接到服务器');
})
```

### 服务端api
``` js
const app = require('express')();
const server = require('http').createServer();
const io = require('socket.io')(server);

io.on('connection',socket=>{
    console.log('客户端连接到服务器');
    //使用send
    socket.send('hello');
    //或者使用emit
    socket.emit('greetings','hey',{'ms':'jane'},Buffer.from([4,3,2,1]));
    //监听send
    socket.on('message',(data)=>{
        console.log(data);
    })
    //监听emit
    socket.on('salutations',(elem1,elem2,elem3)=>{
        console.log(elem1,elem2,elem3);
    })
    socket.on('disconnect',function(){
        console.log('断开连接')
    })
    socket.on('error',()=>{
        console.log('连接错误');
    })
})

server.listen(8000);
```


### 划分命名空间
``` js
//可以把服务分成多个命名空间，默认/，不同空间内不能通信
io.on('connection',socket=>{});
io.of('/news').on('connection',socket=>{});

//客户端链接命名空间
let socket = io('http://localhost/');
let socket = io('http://localhost/news')
```

### 房间
> 可以把一个命名空间分成多个房间,一个客户端可以进入多个房间
> 如果在大厅里广播，那么多有在大厅里的客户端和任何房间内的客户端都能收到消息
> 所有在房间里的广播和通信都不会影响到房间以外的客户端
``` js
//进入房间
socket.join('group1');
//离开房间
socket.leave('group1');

//服务端
socket.on('chat',function(data){
    socket.join('chat');
})
socket.on('leave_chat',function(data){
    socket.leave('chat');
})
//客户端
socket.emit('chat');
socket.emit('leave_chat');

//获取所有分组信息
io.sockets.manager.rooms

//获取此socketid进入的房间信息
io.sockets.manager.roomClients[socket.id]

//获取chat中的客户端，返回所有在此房间的socket实例
io.sockets.clients('chat');
```

### 广播
``` js
//向大厅和房间广播
io.sockets.emit('message','全局广播');
//向除了自己之外的其他客户端广播
socket.broadcast.emit('message',msg);
//给指定的客户端发送消息
io.sockets.socket(socketid).emit('message',msg);
//给该socket的客户端发送消息
socket.emit('message',msg);

//向房间内广播,从服务器的角度来提交事件，提交者会包含在内
io.in('chat').emit('message',msg);
io.of('/news').in('chat').emit('msaage',msg);
//向房间内广播,从客户端的角度来提交事件，提交着会排除在外
socket.broadcast.to('chat').emit('message',msg);
```

### 获取连接的客户端socket
``` js
io.sockets.clients().forEach(function(socket){
    //...
})
```