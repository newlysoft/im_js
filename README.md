#IM JS SDK


## IMService Methods

- **constructor**
    - Initializes the client

- observer
    - handle messages that received from server
- accessToken
    - property must be set before start

- start
    - Start im service

- stop
    - Stop im service

- sendPeerMessage
    - Send a message to peer
    - **Parameters**
      - `msg` (`Object`): message property(sender, receiver, content, msgLocalID)

- sendGroupMessage
    - Send a message to peer
    - **Parameters**
      - `msg` (`Object`): message property(sender, receiver, content, msgLocalID)


## IMService Observer
- onConnectState
    - callback when im service connection state changed
    - **Parameters**
      - `state`:im service's connect state

- handlePeerMessage
    - callback when im service received a peer message
    - **Parameters**
      - `msg` (`Object`): message property(sender, receiver, content, timestamp)

- handleMessageACK
    - callback when im service received an ack of message
    - **Parameters**
      - `msg` (`Object`): message property(sender, receiver, content, timestamp)

- handleMessageFailure
    - callback when im service can't send out the message
    - **Parameters**
      - `msg` (`Object`): message property(sender, receiver, content, timestamp)

- handleGroupMessage
    - callback when im service received a peer message
    - **Parameters**
      - `msg` (`Object`): message property(sender, receiver, content, timestamp)

- handleGroupMessageACK
    - callback when im service received an ack of message
    - **Parameters**
      - `msg` (`Object`): message property(sender, receiver, content, timestamp)

- handleGroupMessageFailure
    - callback when im service can't send out the message
    - **Parameters**
       - `msg` (`Object`): message property(sender, receiver, content, timestamp)



##example


    <script src="/im.js"></script>
    <script>
      var observer = {
        handlePeerMessage: function (msg) {
            console.log("msg sender:", msg.sender, " receiver:", msg.receiver, " content:", msg.content, " timestamp:", msg.timestamp)
        },
        handleMessageACK: function(msg) {
            console.log("message ack local id:", msgLocalID, " receiver:", receiver)
        },
        handleMessageFailure: function(msg) {
            console.log("message fail local id:", msgLocalID, " receiver:", receiver)
        },
        onConnectState: function(state) {
            if (state == IMService.STATE_CONNECTED) {
               console.log("im connected");
            } else if (state == IMService.STATE_CONNECTING) {
               console.log("im connecting");
            } else if (state == IMService.STATE_CONNECTFAIL) {
               console.log("im connect fail");
            } else if (state == IMService.STATE_UNCONNECTED) {
               console.log("im unconnected");
            }
        },

        handleGroupMessage: function(msg) {
            console.log("group msg sender:", msg.sender, " receiver:", msg.receiver, " content:", msg.content, " timestamp:", msg.timestamp)
        },
        handleGroupMessageACK: function(msg) {
            console.log("group message ack:", msg);
        },
        handleGroupMessageFailure: function(msg) {
            console.log("group message fail:", msg);
        },
        handleGroupNotification(notification) {
            var obj = JSON.parse(notification);
            console.log("group notification:", obj);
        },
      }
      
      var im = new IMService();
      im.observer = observer;
      im.accessToken = "????";
      im.start()

      var msg = {sender:100, receiver:200, content:"11", msgLocalID:1000}

      //connectState == STATE_CONNECTING
      var r = im.sendPeerMessage(msg);
      //r == false
      console.log("send message result:", r);


      function send() {
          var msg = {sender:100, receiver:100, content:"11", msgLocalID:1000}
          if (im.connectState == IMService.STATE_CONNECTED) {
              im.sendPeerMessage(msg);
          }
      }

      setTimeout(send, 2000)
    </script>



