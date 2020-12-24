---
title: "原生Websocket 实现即时聊天"
tags: ""
---

# 原生Websocket实现即时聊天

因为项目需求，需要适用websocket构建长链接完成即时通讯和站内信通知的功能。

## websocket实现

### websocket server

```python
import websockets
import asyncio
import ujson


async def recv_connect(websocket):
    await websocket.send("Connected")


async def recv_disconnect(websocket):
    await websocket.send("Disconnected")


# 接收客户端消息并处理，这里只是简单把客户端发来的返回回去
async def recv_msg(websocket):
    while True:
        recv_text = await websocket.recv()
        response_text = f"your submit context: {recv_text}"
        await websocket.send(response_text)


# 服务器端主逻辑
# websocket和path是该函数被回调时自动传过来的，不需要自己传
async def main_logic(websocket, path):
    try:
        await recv_connect(websocket)
        await recv_msg(websocket)
    except Exception as err:
        # shutdown connection
        print(err)


start_server = websockets.serve(main_logic, '127.0.0.1', 8080)
asyncio.get_event_loop().run_until_complete(start_server)
asyncio.get_event_loop().run_forever()
```

### websocket client

```html
<html>

<script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/3.0.3/socket.io.js"></script>

<body>
    <div id="im-displayer">
    </div>
    <input id="input" type="text" />
    <button id="send-btn" onclick="javascript:sendMessage()">
        发送
    </button>
</body>
<script>
    function renderMessage(message) {
        const displayer = document.getElementById("im-displayer");
        displayer.innerHTML = displayer.innerHTML + "<p>" + message + "</p>";
    };
</script>
<script>
    const websocket = new WebSocket("ws://localhost:8080");

    // 监听消息
    websocket.addEventListener('message', function (event) {
        console.log('Message from server ', event.data);
        renderMessage(event.data);
    });

    // 断开
    websocket.addEventListener('error', function (event) {
        console.log('WebSocket error: ', event);
        websocket.close();
    });

    // 连接
    websocket.addEventListener('open', function (event) {
        console.log("Websocket Connected ", event.data);
        renderMessage("Connected");
    })

    function sendMessage() {
        console.log("Sending");
        const input = document.getElementById("input");
        websocket.send(input.value);
    }
</script>

</html>
```

## 效果

![浏览器+wscat](/home/ling/BoostNote/images/websocket-im.png)

## Socket.io 框架

Socket.IO is a library that enables real-time, bidirectional and event-based communication between the browser and the server.

有server,client.还有多种语言client版本的社区实现。

## 展望

希望写一个websocket框架，实现自己的server, client。
