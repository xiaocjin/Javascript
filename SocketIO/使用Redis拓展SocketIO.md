#使用Redis拓展SocketIO
##封装Redis
      
      var redis = require('redis')
         , config = require('../config');
   
      function Redis(db) {
          this.port = config.get("redis:port");
          this.host = config.get("redis:host");
          this.password = config.get("redis:pass");
          this.db = config.get("redis:data_db");
          this.client = redis.createClient(this.port, this.host);
          if (this.password) this.client.auth(this.password, function () {});
          if (db) this.client.select(this.db, function () {});
      }
         
      module.exports = Redis;

##封装SocketIO

      var config = require('../config')
          , db = config.get("redis:session_db")
          , RedisStore = require('socket.io/lib/stores/redis')
          , redis = require('socket.io/node_modules/redis')
          , Redis = require('../cache/redis')
          , pub = new Redis(db).client
          , sub = new Redis(db).client
          , client = new Redis(db).client;
      
      function Socket(server) {
      
          var socketio = require('socket.io').listen(server);
      
          if (config.get('sockets:browserclientminification')) socketio.enable('browser client minification');
          if (config.get('sockets:browserclientetag')) socketio.enable('browser client etag');
          if (config.get('sockets:browserclientgzip')) socketio.enable('browser client gzip');
      
          socketio.set("polling duration", config.get('sockets:pollingduration'));
          socketio.set('log level', config.get('sockets:loglevel'));
          socketio.set('transports', [
              'websocket'
              , 'flashsocket'
              , 'htmlfile'
              , 'xhr-polling'
              , 'jsonp-polling'
          ]);
      
          socketio.set('store', new RedisStore({
              redis: redis, redisPub: pub, redisSub: sub, redisClient: client
          }));
      
          return {io: socketio,
              redis: client
          };
      };
   
      module.exports = Socket;
      
##SocketIO Room

###加入房间

   socket.join(room)
      
###离开房间

      socket.on('disconnect', function () {
         /**
          * 离开房间
          */
         var rooms = io.sockets.manager.roomClients[socket.id];
         for (var room in rooms) {
             if (room.length > 0) {
                 console.log('unsubscribing from ', room);
                 room = room.substr(1);
                 socket.leave(room);
             }
         }
      });

###向房间推送消息

   socket.broadcast.to(room).emit(event, data);
      
###向指定的socket推送消息

   1. 记录socketId与user的关联关系（内存/Redis）
   2. 根据socketId查找socket并推送消息，io.sockets.socket(socketId).emit(event, data);

##注意

   * 在使用Redis实现SocketIO集群时，需要确保使用的SocketIO版本一致，否则数据共享的时候可能出现一些莫名其妙的问题。



      
      
      
