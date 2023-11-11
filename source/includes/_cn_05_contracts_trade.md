

# 合约交易

交易API可用于合约交易，包括下单、撤单、查看当前活动订单以及查询历史订单。

交易API必须附带用户的API签名，以便验证用户身份。

## 查询账户余额

API描述：查询用户现货账户余额。

API路径：GET /v2/contracts/accounts

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
      197.499,
      0,        
      0     
    ],
    "ETH": [
      20000,
      0,
      0
    ],
    "USDT": [
      0.462939518740007584,
      0,
      32.055269516849660528
    ]
  }
}
```
```
数据格式：

`[available, frozen, position]`

`[可用，冻结，仓位]`
```



## 查询用户费率

API描述：查询用户合约交易费率(期货、合约)。

API路径：GET /v1/contracts/fee/rate

API请求参数(Query Param)：

| 参数       | 类型       | 说明                                                         |
| :--------- | ---------- | :----------------------------------------------------------- |
| **symbol** | **string** | **选填**<br>交易对名称,例如`XBTCUSD_PERP`|


```
API响应样例:
```

```json
{
    "taker": 0.000200000000000000, // taker费率
    "maker": 0.000100000000000000, // maker费率
    "timestamp": 1595297714823 // 时间戳
}
```



## 创建新订单

API描述：创建一个新的订单。

API路径：POST /v2/contracts/orders

API请求参数：JSON Object:

| 字段名称 | 字段类型 | 是否必须 | 默认值 | 描述 |
| :------ | ---------|--- |-- | :------ |
| symbol | string | Y | | 合约代码，例如"XBTCUSD_PERP" |
| type | enum | Y | | 订单类型，限价单LIMIT与市价单MARKET |
| direction | enum | Y | | 订单方向，LONG与SHORT |
| source | string | N| "" | 订单来源标识，例如"WEB", "APP"，字母和数字组合，不超过20个字符 |
| price | decimal | 仅限价单 | | 限价单报价 |
| quantity | long | Y | | 订单数量，至少为1 |
| fillOrKill | boolean | N| false | 是否设置FOK订单 |
| immediateOrCancel | boolean | N| false | 是否设置IOC订单 |
| postOnly | boolean | N| false | 是否设置PostOnly订单 |
| hidden | boolean | N| false | 是否设置Hidden订单 |
| reduceOnly | boolean | N | false | 是否设置ReduceOnly订单 |
| clientOrderId | string | N | | 用户自定义订单ID，可用于查询、撤销订单，24小时内可用 |

请注意：

订单类型如果为LIMIT，则必须填写price；

若fillOrKill=true，则无法设置immediateOrCancel、postOnly、hidden和reduceOnly；

若immediateOrCancel=true，则无法设置fillOrKill、postOnly、hidden和reduceOnly；

若postOnly=true，则无法设置fillOrKill、immediateOrCancel和reduceOnly；

若hidden=true，则无法设置fillOrKill和immediateOrCancel；

只有MARKET订单与IOC订单可以设置reduceOnly=true。

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "orderId": 227698471338052
  }
}
```

## 查询活动订单

API描述：查询当前用户的所有活跃订单。

API路径：GET /v2/contracts/orders/open

API请求参数：无

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      {
        "id": 227141752979524,
        "clientOrderId": null,
        "features": 0,
        "price": 2000.0,
        "fee": 3.080000000000000000,
        "fillPrice": 2000.0,
        "quantity": 1000,
        "unfilledQuantity": 230,
        "makerFeeRate": 0.001,
        "takerFeeRate": 0.002,
        "type": "LIMIT",
        "status": "PARTIAL_FILLED",
        "direction": "SHORT",
        "triggerDirection": "LONG",
        "triggerOn": 0,
        "trailingBasePrice": 0,
        "trailingDistance": 0,
        "createdAt": 1699608607796,
        "updatedAt": 1699608607796,
        "frozenMargin": 4.600000000000000532,
        "frozenQuantity": 230,
        "symbol": "ETHUSDT_PERP",
        "trailing": false
      },
      {
        "id": 227698471338052,
        "clientOrderId": null,
        "features": 0,
        "price": 2000.0,
        "fee": 0,
        "fillPrice": 0.0,
        "quantity": 1000,
        "unfilledQuantity": 1000,
        "makerFeeRate": 0.001,
        "takerFeeRate": 0.002,
        "type": "LIMIT",
        "status": "PENDING",
        "direction": "SHORT",
        "triggerDirection": "LONG",
        "triggerOn": 0,
        "trailingBasePrice": 0,
        "trailingDistance": 0,
        "createdAt": 1699674973299,
        "updatedAt": 1699674973299,
        "frozenMargin": 20.000000000000000000,
        "frozenQuantity": 1000,
        "symbol": "ETHUSDT_PERP",
        "trailing": false
      }
    ]
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



## 查询活动订单（通过订单ID）

API描述：通过订单ID查询当前用户活跃订单。

API路径：GET /v2/contracts/orders/open/:order_id

API请求参数(Path Param)：

| 参数         | 类型       | 说明                                 |
| :----------- | ---------- |:-----------------------------------|
| **order_id** | **string** | **必填**<br>订单Id,例如"227141752979524" |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "id": 227141752979524,
    "clientOrderId": null,
    "features": 0,
    "price": 2000.0,
    "fee": 3.080000000000000000,
    "fillPrice": 2000.0,
    "quantity": 1000,
    "unfilledQuantity": 230,
    "makerFeeRate": 0.001,
    "takerFeeRate": 0.002,
    "type": "LIMIT",
    "status": "PARTIAL_FILLED",
    "direction": "SHORT",
    "triggerDirection": "LONG",
    "triggerOn": 0,
    "trailingBasePrice": 0,
    "trailingDistance": 0,
    "createdAt": 1699608607796,
    "updatedAt": 1699608607796,
    "frozenMargin": 4.600000000000000532,
    "frozenQuantity": 230,
    "symbol": "ETHUSDT_PERP",
    "trailing": false
  }
}
```



## 查询活动订单（通过自定义订单ID）

API描述：通过订单ID查询当前用户活跃订单。

API路径：GET /v2/contracts/orders/open/client/:client_order_id

API请求参数(Path Param)：

| 参数                | 类型       | 说明                                  |
| :------------------ | ---------- |:------------------------------------|
| **client_order_id** | **string** | **必填**<br>自定义订单Id,例如"clientOrderId" |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "id": 227700610433092,
    "clientOrderId": "123456789",
    "features": 0,
    "price": 2000.0,
    "fee": 0,
    "fillPrice": 0.0,
    "quantity": 1000,
    "unfilledQuantity": 1000,
    "makerFeeRate": 0.001,
    "takerFeeRate": 0.002,
    "type": "LIMIT",
    "status": "PENDING",
    "direction": "SHORT",
    "triggerDirection": "LONG",
    "triggerOn": 0,
    "trailingBasePrice": 0,
    "trailingDistance": 0,
    "createdAt": 1699675228528,
    "updatedAt": 1699675228528,
    "frozenMargin": 20.000000000000000000,
    "frozenQuantity": 1000,
    "symbol": "ETHUSDT_PERP",
    "trailing": false
  }
}
```





## 查询订单（通过订单ID）

API描述：通过订单ID查询当前用户订单。

API路径：GET /v2/contracts/orders/:order_id

API请求参数(Path Param)：

| 参数         | 类型       | 说明                                    |
| :----------- | ---------- |:--------------------------------------|
| **order_id** | **string** | **必填**<br>自定义订单Id,例如"193871510241346" |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "id": 193871510241346,
    "clientOrderId": null,
    "features": 0,
    "price": 26001.000000000000000000,
    "fee": 0.026001000000000000,
    "fillPrice": 26001.0,
    "quantity": 10,
    "unfilledQuantity": 0,
    "makerFeeRate": 0.001000000000000000,
    "takerFeeRate": 0.002000000000000000,
    "type": "LIMIT",
    "status": "FULLY_FILLED",
    "direction": "LONG",
    "triggerDirection": "LONG",
    "triggerOn": 0.000000000000000000,
    "trailingBasePrice": 0.000000000000000000,
    "trailingDistance": 0.000000000000000000,
    "createdAt": 1695642485763,
    "updatedAt": 1695642523777,
    "frozenMargin": null,
    "frozenQuantity": 0,
    "symbol": "BTCUSDT_PERP",
    "trailing": false
  }
}
```



## 查询订单（通过自定义订单ID）

API描述：通过订单ID查询当前用户订单。

API路径：GET /v2/contracts/orders/client/:client_order_id

API请求参数(Path Param)：

| 参数                | 类型       | 说明                                         |
| :------------------ | ---------- | :------------------------------------------- |
| **client_order_id** | **string** | **选填**<br>自定义订单Id,例如"clientOrderId" |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "id": 227701952610372,
    "clientOrderId": "123456789",
    "features": 0,
    "price": 2000.0,
    "fee": 0,
    "fillPrice": 0.0,
    "quantity": 1000,
    "unfilledQuantity": 1000,
    "makerFeeRate": 0.001,
    "takerFeeRate": 0.002,
    "type": "LIMIT",
    "status": "PENDING",
    "direction": "SHORT",
    "triggerDirection": "LONG",
    "triggerOn": 0,
    "trailingBasePrice": 0,
    "trailingDistance": 0,
    "createdAt": 1699675388023,
    "updatedAt": 1699675388023,
    "frozenMargin": 20.000000000000000000,
    "frozenQuantity": 1000,
    "symbol": "ETHUSDT_PERP",
    "trailing": false
  }
}
```





## 查询历史订单

API描述：查询当前用户的历史订单记录。

API路径：GET /v2/contracts/orders/closed

API请求参数(Request Param)：

| 参数         | 类型       | 说明                                                         |
| :----------- | ---------- | :----------------------------------------------------------- |
| **symbol**   | **string** | **选填**<br>交易对名称，例如`XBTCUSD_PERP`，默认空           |
| **limit**    | **long**   | **选填**<br/>返回结果集的最大记录数量，范围1～100，默认为100 |
| **range**    | **string** | **选填**<br/>查询月份，格式为YYYYMM，例如"201907"，默认为""，表示当前月份 |
| **offsetId** | **long**   | **选填**<br/>传入当前页的起始id，默认为0，表示第一页         |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "range": "202309",
    // 是否有下一页:           
    "hasMore": true,
    // 下一页查询的起始ID:            
    "nextOffsetId": 193757794271300,
    "results": [
      {
        "id": 193871510241346,
        "clientOrderId": null,
        "features": 0,
        "price": 26001.000000000000000000,
        "fee": 0.026001000000000000,
        "fillPrice": 26001.0,
        "quantity": 10,
        "unfilledQuantity": 0,
        "makerFeeRate": 0.001000000000000000,
        "takerFeeRate": 0.002000000000000000,
        "type": "LIMIT",
        "status": "FULLY_FILLED",
        "direction": "LONG",
        "triggerDirection": "LONG",
        "triggerOn": 0.000000000000000000,
        "trailingBasePrice": 0.000000000000000000,
        "trailingDistance": 0.000000000000000000,
        "createdAt": 1695642485763,
        "updatedAt": 1695642523777,
        "frozenMargin": null,
        "frozenQuantity": 0,
        "symbol": "BTCUSDT_PERP",
        "trailing": false
      }
    ]
  }
}
```

说明：

历史订单总是按页查询，第一页的offsetId传入0或者不传；

如果API响应样例返回的hasMore=true，则可以根据nextOffsetId构造下一页查询的URL。

## 取消订单

API描述：取消一个活动订单。

API路径：GET /v2/contracts/orders/:order_id/cancel

API请求参数(Path Param)：

| 参数         | 类型     | 说明               |
| :----------- | -------- | :----------------- |
| **order_id** | **path** | **必填**<br>订单ID |

```
API响应样例:
```

```json
{
    "code": 200,
    "msg": "success",
    "data": {
        "orderId": 227141752979524
    }
}
```



## 取消订单（通过自定义订单ID）

API描述：通过自定义订单ID取消一个活动订单。

API路径：GET /v2/contracts/orders/client/:client_order_id/cancel

API请求参数(Path Param)：

| 参数                | 类型     | 说明                     |
| :------------------ | -------- | :----------------------- |
| **client_order_id** | **path** | **必填**<br>自定义订单ID |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "orderId": 227714661351492
  }
}
```



## 取消所有订单

API描述：取消所有活动订单。

API路径: POST /v2/contracts/orders/cancel

API请求参数(Request Json Body)：

```json
{
    "symbol":"XBTCUSD_PERP"
}
```

| 参数       | 类型       | 说明                                                         |
| :--------- | ---------- | :----------------------------------------------------------- |
| **symbol** | **string** | **选填**<br>交易对名称,例如`XBTCUSD_PERP`，为空则撤销所有交易对活动订单 |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "cancelled": 2 // 成功取消订单的数量
  }
}

```
说明：
该接口是服务端代为执行的批处理操作，并非所有订单同时尝试取消。所以不能保证所有订单取消的时间有效性。

## 查询订单成交详情

API描述：查询当前用户的某个订单成交详情。

API路径：GET /v2/contracts/orders/:order_id/matches

API请求参数(Path Param)：

| 参数         | 类型       | 说明               |
| :----------- | ---------- | :----------------- |
| **order_id** | **string** | **必填**<br>订单ID |

API响应样例：订单成交详细信息，按时间排序

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      {
        "taker": true, // 是否是taker
        "price": 9801.5, // 成交价
        "quantity": 120, // 成交数量
        "fee": 0.0012845, // 手续费
        "createdAt": 1564558783608 // 创建时间
      },
      {
        "taker": false, // 是否是taker
        "price": 9801.0, // 成交价
        "quantity": 26, // 成交数量
        "fee": -0.0006175, // 手续费，负数表示反佣
        "createdAt": 1564558790832 // 创建时间
      },
      ...
    ]
  }
}
```

-如果一个订单的成交记录超过1000条，则最多返回前1000条记录。

## 查询当前活动仓位列表

API描述：获取当前活动仓位列表。

API路径：GET /v2/contracts/positions

API请求参数：无

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      {
        "id": "10010001017_16",
        "leverage": 0,
        "riskLevel": 0,
        "maxQuantity": 200000,
        "margin": 26.339618214529659493,
        "realizedPNL": 37.917438947358001298,
        "takerFeeRate": 0.002,
        "symbol": "BTCUSDT_PERP",
        "bankruptcyPrice": 0.0,
        "liquidationPrice": 0.0,
        "updatedAt": 1699675200428,
        "direction": "LONG",
        "quantity": 38,
        "entryPrice": 25000.0,
        "minimumMaintenanceMarginRate": 0.005,
        "closed": false
      },
      {
        "id": "10010001017_17",
        "leverage": 0,  
        "riskLevel": 0,
        "maxQuantity": 200000,
        "margin": 6.580000000000000959,
        "realizedPNL": -1.444967749999999060,
        "takerFeeRate": 0.002,
        "symbol": "ETHUSDT_PERP",
        "bankruptcyPrice": 0.0,
        "liquidationPrice": 0.0,
        "updatedAt": 1699675200504,
        "direction": "LONG",
        "quantity": 470,
        "entryPrice": 2000.0,
        "minimumMaintenanceMarginRate": 0.005,
        "closed": false
      }
    ]
  }
}
// leverage = 0 代表该仓位为全仓  否则为逐仓对应的杠杆倍数
```

## 查询仓位清算列表

API描述：获取仓位清算列表。

API路径：GET /v2/contracts/clearings/positions

API请求参数(Request Param)：

| 参数         | 类型       | 说明                                                         |
| :----------- | ---------- | :----------------------------------------------------------- |
| **symbol**   | **string** | **选填**<br>交易对名称，例如`XBTCUSD_PERP`，默认空           |
| **limit**    | **long**   | **选填**<br/>返回结果集的最大记录数量，范围1～100，默认为100 |
| **range**    | **string** | **选填**<br/>查询月份，格式为YYYYMM，例如"201907"，默认为""，表示当前月份 |
| **offsetId** | **long**   | **选填**<br/>传入当前页的起始id，默认为0，表示第一页         |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "range": "202309",
    "hasMore": true,
    "nextOffsetId": 10003,
    "results": [
      {
        "id": 10004,
        "orderId": 193871510241346,
        "symbol": "BTCUSDT_PERP",
        "sequenceId": 32,
        "direction": "LONG",
        "type": "OPEN",
        "clearingPrice": 26001.000000000000000000,
        "rate": 0.001000000000000000,
        "fee": 0.026001000000000000,
        "quantityChanged": 10,
        "quantityAfterClearing": 10,
        "realizedPNLChanged": -0.026001000000000000,
        "positionMargin": 0.234009000000000074,
        "createdAt": 1695642523777
      }
    ]
  }
}
```
## 设置仓位杠杠

API描述：为指定仓位设置杠杠倍数。如果设置成功，仓位保证金会随之调整。

API路径：POST /v2/contracts/positions/:symbol/leverage

API请求参数(Path Param)：

| 参数       | 类型       | 说明                                      |
| :--------- | ---------- | :---------------------------------------- |
| **symbol** | **string** | **必填**<br>交易对名称,例如`XBTCUSD_PERP` |

API请求参数(Request Json Body)：

```json
{
    "leverage": 10 // leverage: integer, 必填，0～最大允许值，0=全仓，1～最大允许值=逐仓杠杠倍数。
}
```

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "leverage": 0,
    "symbol": "ETHUSDT_PERP",
    "margin": 6.580000000000001,
    "quantity": 470,
    "riskLevel": 0,
    "maxQuantity": 200000,
    "bankruptcyPrice": 0.0,
    "minimumMaintenanceMarginRate": 0.005,
    "liquidationPrice": 0.0,
    "entryPrice": 2000.0,
    "realizedPNL": -1.444967749999999,
    "takerFeeRate": 0.002,
    "closed": false,
    "id": "10010001017_17",
    "direction": "LONG",
    "updatedAt": 1699675200504
  }
}
```

API错误响应：

- `PARAMETER_INVALID`：无效的leverage参数：null，负数或高于该Symbol允许的最大杠杆倍数。
- `ACCOUNT_NO_ENOUGH_AVAILABLE`: 没有足够的可用保证金。

## 设置仓位保证金

API描述：为指定仓位设置新的保证金额度，仅限逐仓仓位。为全仓仓位设置保证金将返回错误。

API路径：POST /v2/contracts/positions/:symbol/margin

API请求参数(Path Param)：

| 参数       | 类型       | 说明                                      |
| :--------- | ---------- | :---------------------------------------- |
| **symbol** | **string** | **必填**<br>交易对名称,例如`XBTCUSD_PERP` |

- API请求参数(Request Json Body)：

```json
{
    "margin": 1.05
}
```

| 参数       | 类型        | 说明                   |
| :--------- | ----------- |:---------------------|
| **margin** | **decimal** | **必填**<br>设置的新的仓位保证金 |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "leverage": 10,
    "symbol": "ETHUSDT_PERP",
    "margin": 205.0,
    "quantity": 1000,
    "riskLevel": 0,
    "maxQuantity": 200000,
    "bankruptcyPrice": 1798.5971943887776,
    "minimumMaintenanceMarginRate": 0.005,
    "liquidationPrice": 1807.6535750251762,
    "entryPrice": 2000.0,
    "realizedPNL": -2.0,
    "takerFeeRate": 0.002,
    "closed": false,
    "id": "10010001017_17",
    "direction": "LONG",
    "updatedAt": 1699531601621
  }
}
```

API错误响应：

- `PARAMETER_INVALID`：无效的保证金额度（null、0或负数）。
- `POSITION_NOT_FOUND`：未找到活动仓位。
- `POSITION_CHANGE_FAILED`: 不能调整全仓仓位的保证金。
- `ACCOUNT_NO_ENOUGH_AVAILABLE`: 没有足够的可用保证金。

## 设置仓位风险等级

API描述：为指定仓位设置风险等级。如果设置成功，继续增加仓位时最小维持保证金率可能会增加。

API路径：POST /v2/contracts/positions/:symbol/riskLevel

API请求参数：

| 参数       | 类型       | 说明                                      |
| :--------- | ---------- | :---------------------------------------- |
| **symbol** | **string** | **必填**<br>交易对名称,例如`XBTCUSD_PERP` |

- POST参数：JSON Object：

```json
{
    "riskLevel": 1
}
```

| 参数          | 类型        | 说明                      |
| :------------ | ----------- | :------------------------ |
| **riskLevel** | **integer** | **必填**<br>0～最大允许值 |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "leverage": 0,
    "symbol": "BTCUSDT_PERP",
    "margin": 1.08,
    "quantity": 54,
    "riskLevel": 0,
    "maxQuantity": 200000,
    "bankruptcyPrice": 0.0,
    "minimumMaintenanceMarginRate": 0.005,
    "liquidationPrice": 0.0,
    "entryPrice": 25000.0,
    "realizedPNL": -6.8593473082,
    "takerFeeRate": 0.002,
    "closed": false,
    "id": "10010001017_16",
    "direction": "LONG",
    "updatedAt": 1699360257781
  }
}
```

API错误响应：

- `PARAMETER_INVALID`：无效的riskLevel参数：null，负数或高于该Symbol允许的最大风险等级。
- `ACCOUNT_NO_ENOUGH_AVAILABLE`: 没有足够的可用保证金。



## 合约账户划转到资产中心

API描述：从合约账户划转资产到资产中心

API路径：POST /v2/contracts/transfer/out/request

API请求参数：JSON Object

| 参数             | 类型        | 说明                               |
| :--------------- | ----------- | :--------------------------------- |
| **currency**     | **string**  | **必填**<br/>currency名称，如“BTC” |
| **amount**       | **decimal** | **必填**<br/>划转数量              |
| **transferFrom** | **string**  | **必填**<br/>"CONTRACTS"           |
| **transferTo**   | **string**  | **必填**<br/>"WALLET"              |

返回：

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "transferFrom": "CONTRACTS",
    "transferTo": "WALLET",
    "userId": 10010001017,
    "amount": 9.208214471259992313,
    "transferId": "CT0000ce02ab800044",
    "currencyName": "USDT",
    "currencyId": 105
  }
}
```
## 查询划转状态

API描述：从合约账户划转资产到资产中心划转状态

API路径：GET /v2/contracts/transfer/out/:transfer_id/status

API请求参数(Path Param)：

| 参数         | 类型       | 说明             |
| :----------- | ---------- |:---------------|
| **transfer_id** | **string** | **必填**<br>转账ID |



返回：

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "id": 10000,
    "transferId": "CT0000b03d5c800042",
    "transferFrom": "CONTRACTS",
    "transferTo": "WALLET",
    "userId": 10010001017,
    "amount": 1.000000000000000000,
    "done": true,
    "createdAt": 1695631289512,
    "updatedAt": 1695631289639,
    "error": false,
    "currency": "BTC"
  }
}
```

## 查询转账历史

API描述：获取内部划账历史

API路径：GET /v2/contracts/transfer/logs

API请求参数(Request Param)：

| 参数           | 类型       | 说明                                     |
|:-------------| ---------- |:---------------------------------------|
| **currency** | **string** | **选填**<br>币种名称，例如`USDT`，默认空            |
| **limit**    | **long**   | **选填**<br/>返回结果集的最大记录数量，范围1～100，默认为100 |
| **offsetId** | **long**   | **选填**<br/>传入当前页的起始id，默认为0，表示第一页       |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "hasMore": true,
    "nextOffsetId": 10029,
    "results": [
      {
        "id": 10031,
        "transferId": "CT0000ce02ab800044",
        "transferFrom": "CONTRACTS",
        "transferTo": "WALLET",
        "userId": 10010001017,
        "amount": 9.208214471259992313,
        "done": true,
        "createdAt": 1699533399819,
        "updatedAt": 1699533399854,
        "error": false,
        "fromAccount": null,
        "toAccount": null,
        "currency": "USDT"
      },
      {
        "id": 10030,
        "transferId": "38dea7c5108f498a8c42342wq2",
        "transferFrom": "WALLET",
        "transferTo": "CONTRACTS",
        "userId": 10010001017,
        "amount": 10.000000000000000000,
        "done": true,
        "createdAt": 1699532737771,
        "updatedAt": 1699532737951,
        "error": false,
        "fromAccount": null,
        "toAccount": null,
        "currency": "USDT"
      }
    ]
  }
}
```
