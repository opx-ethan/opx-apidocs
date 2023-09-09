

# WebSocket API

WebSocket API是指连接到WebSocket后推送的数据。

## 连接信息

WebSocket只支持wss协议，地址是ws://dev.server.opx.pro:8081/v2/market/notification

## 用户认证

要以认证用户连接wss，必须提供一个有效的wss token。获取wss token的方式是：

以API方式请求wss token，可以访问如下API地址（需要API签名）：

用户API请求：GET http://dev.server.opx.pro/v2/users/notification/token

如果用户未登录，返回400错误，如果用户已登录，返回包含Token的JSON如下：

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "token": "SGo2QXRoMDAwMDAwMDAwMDExOGEyMjVhNGE4MDllNDI0Njg0YzE0ZjFhY2NjN2M1MGJhMDM3ZmU4ZjNiMTM3NzExMmFjNmFmNTgxZmY2ZWJlNTU0ZjkyYmM2ZTQ="
  }
}
```

此Token默认有效期1分钟。

2. 立刻连接WSS：

将获取的token作为参数附加到wss连接：

```
ws://dev.server.opx.pro:8081/v2/market/notification?token=SGo2QXRoMDAwMDAwMDAwMDExOGEyMjVhNGE4MDllNDI0Njg0YzE0ZjFhY2NjN2M1MGJhMDM3ZmU4ZjNiMTM3NzExMmFjNmFmNTgxZmY2ZWJlNTU0ZjkyYmM2ZTQ
```

WSS连接成功后，服务器会立刻推送一条status信息。如果WSS服务器验证用户成功，推送消息如下：

```json
{
    "type":"CONNECTED",
    "message":"connected as signed user",
    "userId":12345
}
```



如果WSS服务器验证用户失败，推送的消息不含userId：

```json
{
  "type": "CONNECTED",
  "message": "connected as anonymous user"
}
```



3. 认证成功的WSS连接会推送用户的订单成交、取消信息等用户相关信息。

## 订阅

连接WSS后，需要订阅指定topic，发送消息如下：

```javascript
socket.send(JSON.stringify({
    action: "subscribe",
    topic: "tick.BTC_USDT"
});
```

返回格式：

```json
{
    "type":"TOPICS",
    "data":[ "tick.btc_usdt"] // 所有已经订阅的topic
}
```

目前支持的topic

1. ORDERBOOK.{symbol_name}
2. TICK.{symbol_name}
3. TICKER.{symbol_name}
4. ALL-TICKER
5. BAR.{MIN,MIN5,MIN15,MIN30,HOUR,HOUR4,DAY,WEEK,MONTH}.{symbol_name}
6. BBO.{symbol_name}
7. SPOTS.ORDER_STATUS_CHANGED
8. SPOTS.ORDER_MATCHED
9. SPOTS.ACCOUNT_CHANGED


目前服务端返回消息的type list

1. CONNECTED
2. ORDERBOOK
3. TICK
4. TICKER
5. ALL-TICKER
6. BAR
7. BBO
8. SPOTS.ORDER_STATUS_CHANGED
9. SPOTS.ORDER_MATCHED
10. SPOTS.ACCOUNT_CHANGED


##  取消订阅

已经订阅的topic可以通过`unsubscribe`的action取消，发送消息如下：

```javascript
socket.send(JSON.stringify({
    action: "unsubscribe",
    topic: "tick.BTC_USDT"
});
```

支持的topic与订阅相同

## 接收消息

服务器向客户端推送的消息均为JSON文本，格式如下：

```json
{
    "type": "message-type", // 消息类型
    "sequenceId": 12345, // 序列ID
    "symbol": "BTC_USDT", // 可选，仅限已订阅的symbol
    "userId": 34567, // 可选，仅限针对单用户的消息，例如order_filled
    "resolution": "MIN", // 可选，仅限针对bar类型的消息，表示bar的类型
    "data": ... // 消息其他字段
}
```

JavaScript代码如下：

```javascript
socket.onmessage = function (event) {
    var message = JSON.parse(event.data);
    if (message.type === "orderbook") {
        processOrderBook(message);
    } else if (message.type === "bar") {
        processBar(message);
    } else ...
};
```



## 心跳

客户端需要每`15秒`发送ping消息来维持连接心跳，格式如下

```json
{
    "action":"ping",
    "ts":1595844075067 // client端时间戳, 单位ms
}
```

服务器响应：

```json
{
    "type":"PONG",
    "ts":1595844075403, // 服务器端时间戳
    "gap":336 // 服务器时间与client时间的差值，可以用来判断连接网络状态
}
```



## TICKER消息

服务器以固定频率推送市场价格摘要消息如下：

```json
{
  "type": "TICKER",
  "symbol": "BTC_USDT",
  "data": [
    1693574940000,
    27152.7,
    27205.2,
    25739.24,
    26031.52,
    133609.57,
    3504106410.368,
    -0.041291657919839
  ],
  "sequenceId": 794752,
  "ts": 1693574966918
}
```

说明：

- 消息类型：TICKER
- 只有24小时内有交易的symbol会出现，没有交易的symbol不会出现
- 数据格式：与Bar数据类型相同，可看作最近24小时的一条K线图（但不同于日K，因为日K的起始时间是0:00）



## ALL_TICKER消息

服务器以固定频率推送全交易对市场价格摘要。首次订阅将返回所有交易对的价格摘要，后续会只推送有数据更新的交易对价格摘要。

订阅：

```json
{
    "action":"subscribe",
    "topic":"ALL-TICKER"
}
```

消息格式：

```json
{
  "type": "ALL-TICKER",
  "timestamp": 1693575018105,
  "data": [
    {
      "type": "TICKER",
      "symbol": "BTC_USDT",
      "data": [
        1693575000000,
        27163.76,
        27205.2,
        25739.24,
        26032.64,
        133643.71,
        3504875464.9052,
        -0.041640774325793
      ],
      "sequenceId": 794931,
      "ts": 1693575016899
    },
    {
      "type": "TICKER",
      "symbol": "ETH_USDT",
      "data": [
        1693575000000,
        1705.54,
        1708.4,
        1632.28,
        1644.84,
        66586.38,
        110361200.734,
        -0.035589901145678
      ],
      "sequenceId": 794932,
      "ts": 1693575016912
    }
  ]
}
```



## Websocket Bar信息

服务器会根据实际成交以最低0.5s的速度推送bar（candle）信息：

```json
{
  "type": "BAR",
  "symbol": "BTC_USDT",
  "resolution": "MIN15",
  "sequenceId": 795097,
  "data": [
    1693575000000,
    26032.34,
    26051.16,
    26030.78,
    26050.7,
    234.92,
    6116784.4598
  ]
}
```
说明：

- 消息类型：bar
- 数据格式：[1594973040000,9100.8,9109.4,9099.7,9109.4,0.2004] --- [timestamp, open, high, low, close, amount,volume]


## Websocket BBO消息

服务器以固定频率推送BBO消息如下：

```json
{
  "type": "BBO",
  "symbol": "BTC_USDT",
  "data": {
    "sequenceId": 793396,
    "timestamp": 1693574535172,
    "bidPrice": 26039.02,
    "bidVolume": 7.24,
    "askPrice": 26039.06,
    "askVolume": 0.42
  }
}
```




## Websocket OrderBook消息

服务器以固定频率增量推送OrderBook消息如下：

```json
{
  "type": "orderbook",
  "symbol": "BTC_USDT",
  "timestamp": "1693629379104",
  "lastSequenceId": 963061,
  "sequenceId": 963066,
  "data": {
    "sellOrders": [
      [
        25770.8,
        1.95
      ]
    ],
    "direction": "LONG",
    "symbol": "BTC_USDT",
    "price": 25768.16,
    "buyOrders": [
      [
        25765.52,
        8.39
      ]
    ],
    "symbolId": 100105,
    "sequenceId": 963066
  }
}
```

说明：

- 消息类型：orderbook
- 是否需要订阅：需要
- 深度数据格式：`[price, amount]`
- 需要自己维护订单薄，按照sequenceId递增更新订单薄
- 维护订单薄规则:推送按照价格匹配，如果存在当前价格 ，那么amount更新为最新值（直接替换），如果amount = 0 将该价格移除订单薄，如果当前价格不存在，按照价格排序插入到订单薄

## Websocket Tick消息

服务器不定期推送最新成交的Tick消息如下：

```json
{
  "type": "TICK",
  "symbol": "BTC_USDT",
  "sequenceId": 794534,
  "data": [
    [
      1693574906883,
      1,
      26033.940000,
      2.000000,
      52067.880000,
      0
    ],
    [
      1693574906883,
      1,
      26033.960000,
      0.950000,
      24732.262000,
      0
    ],
    [
      1693574906883,
      1,
      26034.100000,
      2.360000,
      61440.476000,
      0
    ],
    [
      1693574906883,
      1,
      26034.180000,
      1.540000,
      40092.637200,
      0
    ]
  ]
}
```

说明：

- 消息类型：tick
- 是否需要订阅：需要
- 一个tick消息包含一个或多个成交信息
- tick数据格式：`[timestamp, dir, price, amount, flag]`
  - timestamp: 时间戳
  - dir: 1=主动买入, 0=主动卖出
  - price: 成交价格
  - amount: 成交数量
  - volume: 成交额
  - flag: 0=普通成交（后续增加爆仓标志）


## 订单成交消息

当用户订单成交后，获得订单成交消息：

```json
{
  "type": "SPOTS.ORDER_MATCHED",
  "sequenceId": 795536,
  "data": {
    "symbol": "BTC_USDT",
    "orderId": 176530000970114,
    "matchType": "TAKER",
    "price": 26029.42,
    "fee": 0.0032,
    "matchedQuantity": 1.6,
    "feeCurrency": "BTC",
    "direction": "LONG",
    "updatedAt": 1693575216940
  }
}
```

说明：

- 消息类型：`SPOTS.ORDER_MATCHED`
- 是否需要认证：需要



## 订单状态变更消息

当用户订单状态发生变动后，获得订单状态变化消息：

```json
{
  "type": "SPOTS.ORDER_STATUS_CHANGED",
  "sequenceId": 795290,
  "data": {
    "symbol": "BTC_USDT",
    "triggerOn": 0,
    "quantity": 4.54,
    "makerFeeRate": 0.001,
    "trailingDistance": 0,
    "fee": 118.1781068,
    "marginTrade": false,
    "chargeQuote": false,
    "trailingBasePrice": 0,
    "type": "LIMIT",
    "fillPrice": 26030.42,
    "triggerDirection": "LONG",
    "createdAt": 1693575145085,
    "features": 2,
    "trailing": false,
    "unfilledQuantity": 0.0,
    "price": 26030.42,
    "takerFeeRate": 0.002,
    "failReason": "",
    "id": 176529405378626,
    "direction": "SHORT",
    "status": "FULLY_FILLED",
    "updatedAt": 1693575146891
  }
}
```

说明：

- 消息类型：`SPOTS.ORDER_STATUS_CHANGED`
- 是否需要认证：需要


## 现货账户余额变更消息

当用户某个currency余额发生变动后，推送最新的currency余额：

```json
{
  "type": "SPOTS.ACCOUNT_CHANGED",
  "sequenceId": 2837623,
  "data": {
    "available": 9.994861380499118E10,
    "frozen": 5039418.923,
    "currency": {
      "userId": 10010001017,
      "accountId": 0,
      "currencyId": 105,
      "currencyName": "USDT",
      "available": 9.994861380499118E10,
      "spotsFrozen": 5039418.923,
      "updatedAt": 1694228357108
    },
    "updatedAt": 1694228357108
  }
}
```

说明：

- 消息类型：`SPOTS.ACCOUNT_CHANGED`
- 是否需要认证：需要
- 按照currencyId 更新余额



