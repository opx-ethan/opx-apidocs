

# WebSocket API

WebSocket API是指连接到WebSocket后推送的数据。

## 连接信息

WebSocket只支持wss协议，地址是wss://dev-ws.server.opx.pro/v2/market/notification

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
wss://dev-ws.server.opx.pro/v2/market/notification?token=SGo2QXRoMDAwMDAwMDAwMDExOGEyMjVhNGE4MDllNDI0Njg0YzE0ZjFhY2NjN2M1MGJhMDM3ZmU4ZjNiMTM3NzExMmFjNmFmNTgxZmY2ZWJlNTU0ZjkyYmM2ZTQ
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
    topic: "ORDERBOOK.BTCUSDT_PERP"
});
```

返回格式：

```json
{
  "type": "TOPICS",
  "data": [
    "ORDERBOOK.BTCUSDT_PERP"
  ]
}
```

目前支持的topic

1. ORDERBOOK.{symbol_name}
2. TICK.{symbol_name}
3. TICKER.{symbol_name}
4. ALL-TICKER
5. BAR.{MIN,MIN5,MIN15,MIN30,HOUR,HOUR4,DAY,WEEK,MONTH}.{symbol_name}
6. BBO.{symbol_name}
7. CONTRACTS.ORDER_STATUS_CHANGED
8. CONTRACTS.ORDER_MATCHED
9. CONTRACTS.ACCOUNT_CHANGED
10. CONTRACTS.POSITION_CHANGED


目前服务端返回消息的type list

1. CONNECTED
2. ORDERBOOK
3. TICK
4. TICKER
5. ALL-TICKER
6. BAR
7. BBO
8. CONTRACTS.ORDER_STATUS_CHANGED
9. CONTRACTS.ORDER_MATCHED
10. CONTRACTS.ACCOUNT_CHANGED
11. CONTRACTS.POSITION_CHANGED


##  取消订阅

已经订阅的topic可以通过`unsubscribe`的action取消，发送消息如下：

```javascript
socket.send(JSON.stringify({
    action: "unsubscribe",
    topic: "ORDERBOOK.BTCUSDT_PERP"
});
```

支持的topic与订阅相同

## 接收消息

服务器向客户端推送的消息均为JSON文本，格式如下：

```json
{
    "type": "message-type", // 消息类型
    "sequenceId": 12345, // 序列ID
    "symbol": "BTCUSDT_PERP", // 可选，仅限已订阅的symbol
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

服务器推送市场价格摘要消息如下：

```json
{
  "type": "TICKER",
  "symbol": "BTCUSDT_PERP",
  "data": [
    1699523040000,
    25000,
    25000,
    25000,
    25000,
    43,
    107.5,
    0
  ],
  "sequenceId": 157154,
  "ts": 1699523058351
}
```

说明：
- 消息类型：TICKER
- 只有24小时内有交易的symbol会出现，没有交易的symbol不会出现
- 数据格式：与Bar数据类型相同，可看作最近24小时的一条K线图（但不同于日K，因为日K的起始时间是0:00）
- change: 24小时涨跌幅
- 数据格式：
  `[1594973040000,9100.8,9109.4,9099.7,9109.4,0.2004,1843.68, 0]`
  `[timestamp, open, high, low, close, volume，amount,change]`



## ALL_TICKER消息

服务器推送全交易对市场价格摘要。首次订阅将返回所有交易对的价格摘要，后续会只推送有数据更新的交易对价格摘要。

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
  "timestamp": 1699680695758,
  "data": [
    {
      "type": "TICKER",
      "symbol": "ETH_USDT",
      "data": [
        1699618980000,
        1787.41,
        1787.41,
        1639.22,
        1639.22,
        1,
        1639.22,
        -0.082907670875736
      ],
      "sequenceId": 97623185,
      "ts": 1699619021105
    },
    {
      "type": "TICKER",
      "symbol": "BTCUSDT_PERP",
      "data": [
        1699523040000,
        25000,
        25000,
        25000,
        25000,
        43,
        107.5,
        0
      ],
      "sequenceId": 157154,
      "ts": 1699523058351
    },
    {
      "type": "TICKER",
      "symbol": "BTC_USDT",
      "data": [
        1699680660000,
        36677.06,
        37519.9,
        36340.02,
        37108.86,
        148735.16,
        5513377587.616,
        0.011773026518483
      ],
      "sequenceId": 97703325,
      "ts": 1699680687065
    },
    {
      "type": "TICKER",
      "symbol": "ETHUSDT_PERP",
      "data": [
        1699608600000,
        2000,
        2000,
        2000,
        2000,
        3271,
        6542,
        0
      ],
      "sequenceId": 205836,
      "ts": 1699608607796
    }
  ]
}
```
说明：
- 消息类型：ALL-TICKER
- 数据格式：与TICKER 一样



## Websocket Bar信息

服务器会根据实际成交以最低0.5s的速度推送bar（candle）信息：

```json
{
  "type": "BAR",
  "symbol": "BTCUSDT_PERP",
  "resolution": "MIN",
  "sequenceId": 157154,
  "data": [
    1699680780000,
    25000,
    25000,
    25000,
    25000,
    0,
    0
  ]
}
```
说明：

- 消息类型：bar
- 数据格式：
  `[1594973040000,9100.8,9109.4,9099.7,9109.4,0.2004,1843.68]`
  `[timestamp, open, high, low, close, volume，amount]`


## Websocket BBO消息

服务器以固定频率推送BBO消息如下：

```json
{
  "type": "BBO",
  "symbol": "BTCUSDT_PERP",
  "data": {
    "sequenceId": 236709,
    "timestamp": 1699685654818,
    "bidPrice": 27000.0,
    "bidVolume": 1.0,
    "askPrice": 29000.0,
    "askVolume": 1.0
  }
}
```




## Websocket OrderBook消息

服务器以固定频率增量推送OrderBook消息如下：

```json
{
  "type": "orderbook",
  "symbol": "BTCUSDT_PERP",
  "timestamp": "1699680530904",
  "lastSequenceId": 234562,
  "sequenceId": 234638,
  "data": {
    "sellOrders": [
      [
        25000,
        0
      ]
    ],
    "direction": "SHORT",
    "symbol": "BTCUSDT_PERP",
    "price": 25000,
    "buyOrders": {},
    "symbolId": 16,
    "sequenceId": 234638
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
  "symbol": "BTCUSDT_PERP",
  "sequenceId": 235903,
  "data": [
    [
      1699683670464,
      1,
      25000,
      1,
      2.5,
      0
    ]
  ]
}
```

说明：

- 消息类型：tick
- 是否需要订阅：需要
- 一个tick消息包含一个或多个成交信息
- tick数据格式：`[timestamp, dir, price, volume, amount, flag]`
  - timestamp: 时间戳
  - dir: 1=主动买入, 0=主动卖出
  - price: 成交价格
  - volume: 成交数量
  - amount: 成交额
  - flag: 0=普通成交（后续增加爆仓标志）


## 订单成交消息

当用户订单成交后，获得订单成交消息：

```json
{
  "type": "CONTRACTS.ORDER_MATCHED",
  "sequenceId": 235903,
  "data": {
    "symbol": "BTCUSDT_PERP",
    "orderId": 227771427061828,
    "matchType": "TAKER",
    "price": 25000,
    "fee": 0.005,
    "matchedQuantity": 1,
    "feeCurrency": "USDT",
    "updatedAt": 1699683670464,
    "direction": "LONG"
  }
}
```

说明：

- 消息类型：`CONTRACTS.ORDER_MATCHED`
- 是否需要认证：需要



## 订单状态变更消息

当用户订单状态发生变动后，获得订单状态变化消息：

```json
{
  "type": "CONTRACTS.ORDER_STATUS_CHANGED",
  "sequenceId": 235903,
  "data": {
    "symbol": "BTCUSDT_PERP",
    "quantity": 1,
    "triggerOn": 0,
    "makerFeeRate": 0.001,
    "trailingDistance": 0,
    "fee": 0.0025,
    "trailingBasePrice": 0,
    "type": "LIMIT",
    "fillPrice": 25000,
    "triggerDirection": "LONG",
    "features": 0,
    "createdAt": 1699683660415,
    "trailing": false,
    "unfilledQuantity": 0,
    "price": 25000,
    "takerFeeRate": 0.002,
    "failReason": "",
    "id": 227771343175748,
    "status": "FULLY_FILLED",
    "updatedAt": 1699683670464,
    "direction": "SHORT"
  }
}
```

说明：

- 消息类型：`CONTRACTS.ORDER_STATUS_CHANGED`
- 是否需要认证：需要


## 合约账户余额变更消息

当用户某个currency余额发生变动后，推送最新的currency余额：

```json
{
  "type": "CONTRACTS.ACCOUNT_CHANGED",
  "sequenceId": 235709,
  "data": {
    "currenName": {
      "id": 105,
      "name": "USDT",
      "derivative": false,
      "iconUrl": "https://static.opx.pro/icon/usdt.svg",
      "displayScale": 8,
      "depositOpenTime": 0,
      "withdrawOpenTime": 0,
      "hidden": false,
      "displayOrder": 0,
      "displayName": "TetherUS"
    },
    "available": 100000,
    "frozen": 0,
    "position": 0,
    "currencyId": 105,
    "userId": 10010001050,
    "updatedAt": 1699683204049
  }
}
```

说明：

- 消息类型：`CONTRACTS.ACCOUNT_CHANGED`
- 是否需要认证：需要
- 按照currencyId 更新余额


## 合约仓位变更消息

当用户仓位发生变动后，推送最新的仓位信息：

```json
{
  "type": "CONTRACTS.POSITION_CHANGED",
  "sequenceId": 236087,
  "data": {
    "symbolId": 16,
    "symbol": {
      "id": 16,
      "name": "BTCUSDT_PERP",
      "openTime": 1546300800000,
      "endTime": 5000000000000,
      "type": "PERPETUAL",
      "marginCurrency": "USDT",
      "sizeCurrency": "BTC",
      "inverse": false,
      "liquidateBy": "INDEX_PRICE",
      "multiplier": 1.0E-4,
      "minimumPriceIncrement": 0.5,
      "priceStep": 5,
      "priceScale": 1,
      "maximumQuantityPerOrder": 2000,
      "riskLimit": {
        "id": 102,
        "initialMarginRate": 0.01,
        "maintenanceMarginRateStep": 0.005,
        "maxLeverage": 100,
        "riskLimitBase": 200000,
        "riskLimitStep": 100000,
        "maxRiskLimitSteps": 9,
        "createdAt": 1546956010600
      },
      "settlementFeeRate": 0.0,
      "hidden": false,
      "referencedIndexes": {
        "SPOT": 30010,
        "FAIR": 30030,
        "PREMIUM_1M": 30031,
        "PREMIUM_AGGREGATE": 30032,
        "MAX_PREMIUM_RATE": 30021,
        "FUNDING_RATE_1M": 30033,
        "FUNDING_RATE_AGGREGATE": 30034,
        "LENDING_RATE_1D": 30020
      }
    },
    "price": 25000.0,
    "type": "INCREASE",
    "updatedAt": 1699684127769,
    "quantityChanged": 1
  }
}
```

说明：

- 消息类型：`CONTRACTS.POSITION_CHANGED`
- 是否需要认证：需要



