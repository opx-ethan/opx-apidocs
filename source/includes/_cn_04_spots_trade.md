# 现货交易

现货交易API可用于现货交易，包括下单、撤单、查看当前活动订单以及查询历史订单。

## 查询账户余额

API描述：查询用户现货账户余额。

API路径：GET /v2/spots/accounts

API请求参数：

- 无

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "BTC": [
      199997.999,
      0
    ],
    "ETH": [
      200000,
      0
    ],
    "USDT": [
      200000,
      0
    ]
  }
}
```

## 查询用户费率

API描述：查询用户现货交易费率。

API路径：GET /v2/spots/fee/rate

API请求参数(Query Param)：

| 参数       | 类型     | 说明                                  |
| :--------- | -------- | :------------------------------------ |
| **symbol* | **string** | **必填**<br>交易对名称,例如`BTC_USDT` |




```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "taker": 0.002,
    "maker": 0.001,
    "timestamp": 1692788103022
  }
}
```

## 下单

API描述：创建一个新订单。

API路径：POST /v2/spots/orders

API请求参数(Request Json Body)：

| 参数          | 类型        | 说明                                                   |
| :------------ | ----------- | ------------------------------------------------------ |
| **symbol**    | **string**  | **必填**<br>交易对名称,例如`BTC_USDT`                  |
| **type**      | **enum**    | **必填**<br/>订单类型：限价单="LIMIT"，市价单="MARKET" |
| **direction** | **enum**    | **必填**<br/>订单方向：买入="LONG"，卖出="SHORT"       |
| **price**     | **decimal** | **仅限限价单**<br/>订单价格，例如`7123.5`              |
| **quantity**  | **decimal** | **必填**<br/>订单数量，例如`1.02`                      |
| **fillOrKill**  | **boolean** | **非必填**<br/>是否FOK订单，例如`true`                      |
| **immediateOrCancel**  | **boolean** | **非必填**<br/>是否IOC订单，例如`true`                      |
| **postOnly**  | **boolean** | **必填**<br/>是否被动委托订单，例如`true`                      |
| **hidden**  | **boolean** | **必填**<br/>是否冰山委托订单，例如`true`，冰山委托订单手续费率使用taker费率      |
| **clientOrderId** | **string** | **选填**<br/>用户自定义OrderId，可用于订单查询、撤单。自定义OrderId 24小时内可用。 |



```
API响应样例：订单信息
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "orderId": 169927679934528
  }
}
```

## 撤单

API描述：撤销一个订单。

API路径：POST /v2/spots/orders/:order_id/cancel

API请求参数(Path Param)：

| 参数        | 类型     | 说明               |
| :---------- | -------- | :----------------- |
| **order_id** | **path** | **必填**<br>订单ID |

```
API响应样例：同上
```

API错误响应：

- `ORDER_NOT_FOUND`: 订单不存在（ID无效或已完全成交或已被取消）

说明：

如果撤单成功，返回的订单状态是`FULLY_CANCELLED`（全部取消）或`PARTICIAL_CANCELLED`（部分取消）。

如果撤单失败，返回错误响应。



## 撤单（通过自定义订单ID）

API描述：通过clientOrderId撤销一个订单。

API路径：POST /v2/spots/orders/client/:client_order_id/cancel

API请求参数(Path Param)：

| 参数              | 类型     | 说明                         |
| :---------------- | -------- | :--------------------------- |
| **clientOrderId** | **path** | **必填**<br>用户自定义订单ID |

```
API响应样例：同上
```

API错误响应：

- `ORDER_NOT_FOUND`: 订单不存在（ID无效或已完全成交或已被取消）

说明：

如果撤单成功，返回的订单状态是`FULLY_CANCELLED`（全部取消）或`PARTICIAL_CANCELLED`（部分取消）。

如果撤单失败，返回错误响应。



## 撤销所有订单

API描述：撤销所有订单。

API路径：POST /v2/spots/orders/cancel

API请求参数(Request Json Body)：

```json
{
    "symbol":"BTC_USDT"
}
```

| 参数       | 类型       | 说明                                                         |
| :--------- | ---------- | :----------------------------------------------------------- |
| **symbol** | **string** | **选填**<br>交易对名称,例如`BTC_USDT`，为空则撤销所有交易对活动订单 |

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "cancelled": 0 // 成功取消订单的数量
  }
}
```

说明：

该接口是服务端代为执行的批处理操作，并非所有订单同时尝试取消。所以不能保证所有订单取消的时间有效性。

## 查询订单详情

API描述：查询某个指定订单的详情。

API路径：GET /v2/spots/orders/:order_id

API请求参数(Path Param)：

| 参数        | 类型     | 说明               |
| :---------- | -------- | :----------------- |
| **order_id** | **path** | **必填**<br>订单ID |



```
API响应样例：订单详细信息
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "id": 169927679934528,
    "clientOrderId": null,
    "features": 0,
    "price": 8400.000000000000000000,
    "fee": 0.000000000000000000,
    "fillPrice": 0.0,
    "marginTrade": false,
    "chargeQuote": false,
    "quantity": 0.010000000000000000,
    "unfilledQuantity": 0.010000000000000000,
    "makerFeeRate": 0.001000000000000000,
    "takerFeeRate": 0.002000000000000000,
    "type": "LIMIT",
    "status": "FULLY_CANCELLED",
    "direction": "LONG",
    "triggerDirection": "LONG",
    "triggerOn": 0.000000000000000000,
    "trailingBasePrice": 0.000000000000000000,
    "trailingDistance": 0.000000000000000000,
    "createdAt": 1692788158183,
    "updatedAt": 1692788229144,
    "symbol": "BTC_USDT",
    "trailing": false
  }
}
```

OrderStatus：订单状态说明

- PENDING：正在等待成交的活动单；
- FAILED：订单执行失败（无足够保证金等原因），最终状态；
- FULLY_FILLED：全部成交，最终状态；
- PARTIAL_FILLED：已部分成交；
- PARTIAL_CANCELLED：部分成交后被用户取消，最终状态；
- FULLY_CANCELLED：订单尚未成交就被用户取消，最终状态；


## 查询订单详情（通过自定义订单ID）

API描述：通过自定义订单ID查询订单的详情。

API路径：GET /v2/spots/orders/client/:client_order_id

API请求参数(Path Param)：

| 参数                | 类型     | 说明                         |
| :------------------ | -------- | :--------------------------- |
| **client_order_id** | **path** | **必填**<br>用户自定义订单ID |



```
API响应样例：订单详细信息 同上
```



## 查询订单成交详情

API描述：查询当前用户的某个订单成交详情。

API路径：GET /v2/spots/orders/:order_id/matches

API请求参数(Path Param)：


| 参数   | 类型     | 说明                            |
| :----- | -------- | :------------------------------ |
| **order_id** | **path** | **必填**<br>URL地址参数，订单ID |



```
API响应样例：订单成交详细信息，按时间排序
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      {
        "taker": true,
        "price": 29421.710000000000000000,
        "quantity": 0.010000000000000000,
        "fee": 0.000020000000000000,
        "createdAt": 1692788965761
      }
    ]
  }
}
```

如果一个订单的成交记录超过1000条，则最多返回前1000条记录。

## 查询清算结果

API描述：查询当前用户的某个订单成交详情。

API路径：GET /v2/spots/match/clearings

API请求参数(Request Param)：

| 参数         | 类型       | 说明                                                         |
| :----------- | ---------- | :----------------------------------------------------------- |
| **symbol**   | **string** | **选填**<br>交易对名称，例如`BTC_USDT`，默认空               |
| **limit**    | **long**   | **选填**<br/>返回结果集的最大记录数量，范围1～100，默认为100 |
| **range**    | **string** | **选填**<br/>查询月份，格式为YYYYMM，例如"201907"，默认为""，表示当前月份 |
| **offsetId** | **long**   | **选填**<br/>传入当前页的起始id，默认为0，表示第一页         |



```
API响应样例：订单成交详细信息，按时间排序
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "range": "202308",
    "hasMore": false,
    "nextOffsetId": 0,
    "results": [
      {
        "id": 10066,
        "orderId": 169934449541184,
        "userId": 2,
        "symbol": "BTC_USDT",
        "sequenceId": 401,
        "direction": "LONG",
        "type": "TAKER",
        "matchPrice": 29421.710000000000000000,
        "matchQuantity": 0.010000000000000000,
        "orderStatusAfterClearing": "FULLY_FILLED",
        "orderUnfilledQuantityAfterClearing": 0.000000000000000000,
        "feeCurrency": "BTC",
        "fee": 0.000020000000000000,
        "rate": 0.002000000000000000,
        "createdAt": 1692788965761
      },
      {
        "id": 10065,
        "orderId": 169934365655104,
        "userId": 2,
        "symbol": "BTC_USDT",
        "sequenceId": 401,
        "direction": "SHORT",
        "type": "MAKER",
        "matchPrice": 29421.710000000000000000,
        "matchQuantity": 0.010000000000000000,
        "orderStatusAfterClearing": "FULLY_FILLED",
        "orderUnfilledQuantityAfterClearing": 0.000000000000000000,
        "feeCurrency": "USDT",
        "fee": 0.294217100000000000,
        "rate": 0.001000000000000000,
        "createdAt": 1692788965761
      }
    ]
  }
}
```

## 查询所有活跃订单

API描述：查询当前用户活跃订单。

API路径：GET /v2/spots/orders/open

API请求参数：

- 无

```
API响应样例：订单详细信息，按时间排序
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      {
        "id": 169932469829696,
        "clientOrderId": null,
        "features": 0,
        "price": 8400.00,
        "fee": 0,
        "fillPrice": 0.0,
        "marginTrade": false,
        "chargeQuote": false,
        "quantity": 0.01,
        "unfilledQuantity": 0.01,
        "makerFeeRate": 0.001,
        "takerFeeRate": 0.002,
        "type": "LIMIT",
        "status": "PENDING",
        "direction": "LONG",
        "triggerDirection": "LONG",
        "triggerOn": 0,
        "trailingBasePrice": 0,
        "trailingDistance": 0,
        "createdAt": 1692788729879,
        "updatedAt": 1692788729879,
        "symbol": "BTC_USDT",
        "trailing": false
      }
    ]
  }
}
```

## 查询交易对活跃订单

API描述：查询当前用户活跃订单。

API路径：GET /v2/spots/orders/open/:symbol

API请求参数(Path Param)：


| 参数   | 类型     | 说明                            |
| :----- | -------- | :------------------------------ |
| **symbol**   | **string** | **必填**<br>交易对名称，例如`BTC_USDT`，默认空               |


```
API响应样例：订单详细信息，按时间排序
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      {
        "id": 169932469829696,
        "clientOrderId": null,
        "features": 0,
        "price": 8400.00,
        "fee": 0,
        "fillPrice": 0.0,
        "marginTrade": false,
        "chargeQuote": false,
        "quantity": 0.01,
        "unfilledQuantity": 0.01,
        "makerFeeRate": 0.001,
        "takerFeeRate": 0.002,
        "type": "LIMIT",
        "status": "PENDING",
        "direction": "LONG",
        "triggerDirection": "LONG",
        "triggerOn": 0,
        "trailingBasePrice": 0,
        "trailingDistance": 0,
        "createdAt": 1692788729879,
        "updatedAt": 1692788729879,
        "symbol": "BTC_USDT",
        "trailing": false
      }
    ]
  }
}
```

## 查询历史订单

API描述：查询当前用户历史订单。

API路径：GET /v2/spots/orders/closed

API请求参数(Request Param)：

| 参数         | 类型       | 说明                                                         |
| :----------- | ---------- | :----------------------------------------------------------- |
| **symbol**   | **string** | **选填**<br>交易对名称，例如`BTC_USDT`，默认空               |
| **limit**    | **long**   | **选填**<br/>返回结果集的最大记录数量，范围1～100，默认为100 |
| **range**    | **string** | **选填**<br/>查询月份，格式为YYYYMM，例如"201907"，默认为""，表示当前月份 |
| **offsetId** | **long**   | **选填**<br/>传入当前页的起始id，默认为0，表示第一页         |

```
API响应样例：订单详细信息，按时间排序
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "range": "202308",
    "hasMore": false,
    "nextOffsetId": 0,
    "results": [
      {
        "id": 169927679934528,
        "clientOrderId": null,
        "features": 0,
        "price": 8400.000000000000000000,
        "fee": 0.000000000000000000,
        "fillPrice": 0.0,
        "marginTrade": false,
        "chargeQuote": false,
        "quantity": 0.010000000000000000,
        "unfilledQuantity": 0.010000000000000000,
        "makerFeeRate": 0.001000000000000000,
        "takerFeeRate": 0.002000000000000000,
        "type": "LIMIT",
        "status": "FULLY_CANCELLED",
        "direction": "LONG",
        "triggerDirection": "LONG",
        "triggerOn": 0.000000000000000000,
        "trailingBasePrice": 0.000000000000000000,
        "trailingDistance": 0.000000000000000000,
        "createdAt": 1692788158183,
        "updatedAt": 1692788229144,
        "symbol": "BTC_USDT",
        "trailing": false
      }
    ]
  }
}
```



## 币币账户划转到资产中心

API描述：从币币账户划转资产到资产中心

API路径：POST /v2/spots/transfer/out/request

API请求参数：JSON Object

| 参数             | 类型        | 说明                               |
| :--------------- | ----------- | :--------------------------------- |
| **currency**     | **string**  | **必填**<br/>currency名称，如“BTC” |
| **amount**       | **decimal** | **必填**<br/>划转数量              |
| **transferFrom** | **string**  | **必填**<br/>"SPOTS"               |
| **transferTo**   | **string**  | **必填**<br/>"WALLET"              |

返回：

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "transferFrom": "SPOTS",
    "transferTo": "WALLET",
    "userId": 2,
    "amount": 2,
    "transferId": "CT00009a8e52800040",
    "currencyName": "BTC",
    "currencyId": 100
  }
}
```

## 币币账户之间划转

API描述：币币账户之间的划转, 默认账户为0，普通用户开设1，2两个子账户，做市商用户开设1～9 共9个子账户， 子账户使用方式为在原有的接口上增加 Account Header，填写使用的账户，默认为0

API路径：POST /v2/spots/transfer/account/request

API请求参数：JSON Object

| 参数             | 类型        | 说明                               |
| :--------------- | ----------- | :--------------------------------- |
| **currency**     | **string**  | **必填**<br/>currency名称，如“BTC” |
| **amount**       | **decimal** | **必填**<br/>划转数量              |
| **transferFrom** | **string**  | **必填**<br/>"ACCOUNT"               |
| **transferTo**   | **string**  | **必填**<br/>"ACCOUNT"              |
| **fromAccount** | **string**  | **必填**<br/>0～2               |
| **toAccount**   | **string**  | **必填**<br/>0～2              |

请求样例：

```json
{
    "currency":"BTC",
    "transferFrom":"ACCOUNT",
    "transferTo":"ACCOUNT",
    "fromAccount":0,
    "toAccount":1,
    "amount":0.001
}

```


返回：

```json
{
    "result": true
}
```
