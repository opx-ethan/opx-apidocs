# 公开市场

公开市场API是指无需登录，即可直接访问的API。

## 获取交易信息

API描述：获取交易所当前所有交易。

API路径：GET [/v2/market/meta](https://defiapi.876ex.com/v2/market/meta)

API请求参数：

- 无

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "spotsSymbols": [
      {
        "id": 100105,
        "name": "BTC_USDT",
        "openTime": 0,
        "endTime": 5000000000000,
        "baseName": "BTC",
        "baseMinimumIncrement": 0.01,
        "baseScale": 2,
        "baseMinimumQuantity": 0.01,
        "baseMaximumQuantity": 10000.0,
        "quoteName": "USDT",
        "quoteMinimumIncrement": 0.01,
        "quoteScale": 2,
        "supportMarginTrade": true,
        "alwaysChargeQuote": false,
        "displayOrder": 0,
        "hidden": false,
        "derivative": false,
        "referencedIndexIdMap": {},
        "zone": null
      },
      {
        "id": 101105,
        "name": "ETH_USDT",
        "openTime": 0,
        "endTime": 5000000000000,
        "baseName": "ETH",
        "baseMinimumIncrement": 0.01,
        "baseScale": 2,
        "baseMinimumQuantity": 0.01,
        "baseMaximumQuantity": 10000.0,
        "quoteName": "USDT",
        "quoteMinimumIncrement": 0.01,
        "quoteScale": 2,
        "supportMarginTrade": true,
        "alwaysChargeQuote": false,
        "displayOrder": 0,
        "hidden": false,
        "derivative": false,
        "referencedIndexIdMap": {},
        "zone": null
      }
    ],
    "contractsCurrencies": [],
    "contractsSymbols": [],
    "marginSymbols": [
      {
        "id": 100105,
        "name": "BTC_USDT",
        "openTime": 0,
        "endTime": 5000000000000,
        "baseName": "BTC",
        "baseMinimumIncrement": 0.01,
        "baseScale": 2,
        "baseMinimumQuantity": 0.01,
        "baseMaximumQuantity": 10000.0,
        "quoteName": "USDT",
        "quoteMinimumIncrement": 0.01,
        "quoteScale": 2,
        "supportMarginTrade": true,
        "alwaysChargeQuote": false,
        "displayOrder": 0,
        "hidden": false,
        "derivative": false,
        "referencedIndexIdMap": {},
        "zone": null
      },
      {
        "id": 101105,
        "name": "ETH_USDT",
        "openTime": 0,
        "endTime": 5000000000000,
        "baseName": "ETH",
        "baseMinimumIncrement": 0.01,
        "baseScale": 2,
        "baseMinimumQuantity": 0.01,
        "baseMaximumQuantity": 10000.0,
        "quoteName": "USDT",
        "quoteMinimumIncrement": 0.01,
        "quoteScale": 2,
        "supportMarginTrade": true,
        "alwaysChargeQuote": false,
        "displayOrder": 0,
        "hidden": false,
        "derivative": false,
        "referencedIndexIdMap": {},
        "zone": null
      }
    ],
    "indexes": [],
    "spotsCurrencies": [
      "BTC",
      "ETH",
      "USDT"
    ],
    "riskLimits": [],
    "currencies": [
      {
        "id": 100,
        "name": "BTC",
        "derivative": false,
        "iconUrl": null,
        "displayScale": 0,
        "depositOpenTime": 0,
        "withdrawOpenTime": 0,
        "hidden": false,
        "displayOrder": 0,
        "displayName": null
      },
      {
        "id": 101,
        "name": "ETH",
        "derivative": false,
        "iconUrl": null,
        "displayScale": 0,
        "depositOpenTime": 0,
        "withdrawOpenTime": 0,
        "hidden": false,
        "displayOrder": 0,
        "displayName": null
      },
      {
        "id": 105,
        "name": "USDT",
        "derivative": false,
        "iconUrl": null,
        "displayScale": 0,
        "depositOpenTime": 0,
        "withdrawOpenTime": 0,
        "hidden": false,
        "displayOrder": 0,
        "displayName": null
      }
    ]
  }
}
```

## 获取时间

API描述：获取服务器当前时间，以毫秒为单位。

API路径：GET [/v2/market/timestamp](https://defiapi.876ex.com/v2/market/timestamp)

API请求参数：

- 无

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "timestamp": 1691203331433
  }
}
```

## 获取汇率

API描述：获取当前市场汇率。

API路径：GET [/v2/market/fex](https://defiapi.876ex.com/v2/market/fex)

API请求参数：

- 无

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "from": "USD",
    "tos": [
      "AUD",
      "CAD",
      "CNY",
      "EUR",
      "GBP",
      "HKD",
      "JPY",
      "KRW",
      "RUB",
      "SGD",
      "TWD"
    ],
    "exchanges": {
      "USD_GBP": 0.77924,
      "USD_CAD": 1.31809,
      "USD_JPY": 142.258,
      "USD_HKD": 7.7977,
      "USD_TWD": 31.474,
      "USD_EUR": 0.9092,
      "USD_KRW": 1277.9,
      "USD_SGD": 1.3294,
      "USD_AUD": 1.48881,
      "USD_CNY": 7.1435
    }
  }
}
```

## 获取系统错误代码

API描述：获取系统定义的全部错误代码。

API路径：GET [/v2/market/errorCodes](https://defiapi.876ex.com/v2/market/errorCodes)

API请求参数：

- 无

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "ACCOUNT_INVALID": {
      "en": "Account error: Account {{account}} invalid",
      "cn": "子账户 {{account}} 不合法"
    },
    "ACCOUNT_NO_ENOUGH_AVAILABLE": {
      "en": "Account error: {{currency}} has no enough available",
      "cn": "{{currency}} 账户缺少足够的余额"
    },
    "ACCOUNT_NO_ENOUGH_BALANCE": {
      "en": "Account error: no enough balance",
      "cn": "余额不足"
    },
    .......
  }
}
```

## 返回错误响应

API描述：返回一个错误响应，以便调试客户端错误处理代码。

API路径：GET [/v2/market/error](https://defiapi.876ex.com/v2/market/error)

API请求参数：

- 无

```
API响应样例：
```

```json
{
  "error": "TEST_API_ERROR",
  "data": {
    "0": "Hello"
  },
  "message": "Hello, this is a test api message"
}
```

## 获取24小时统计价格

API描述：获取某个交易对的最近24小时统计价格.

API路径：GET /v2/market/ticker/:symbol_name

API示例：GET [/v2/market/ticker/BTC_USDT](https://defiapi.876ex.com/v2/market/ticker/BTC_USDT)

API请求参数(Path Param)：

| 参数              | 类型        | 说明                           |
|:----------------|-----------|:-----------------------------|
| **symbol_name** | **path**  | **必填**<br>交易对名称,例如`BTC_USDT` |
| **limit      ** | **param** | **非必填**<br> 默认200            |
 

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      {
        "sequenceId": 390,
        "data": [
          [
            1690878910379,
            0,
            29424.72,
            2.0,
            58849.44,
            0
          ]
        ]
      }
    ]
  }
}
```
说明：

数据格式：

`[timestamp, open, high, low, close, amount]`

`[时间戳，开盘价，最高价，最低价，收盘价，成交量]`


## 获取所有统计价格

API描述：获取所有交易对的最近24小时统计价格.

API路径：GET /v2/market/all-ticker

API示例：GET [/v2/market/all-ticker](https://defiapi.876ex.com/v2/market/all-ticker)

API请求参数：

- 无

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      {
        "type": "TICKER",
        "symbol": "BTC_USDT",
        "data": [
          1690878900000,
          29424.71,
          29434.1,
          29421.71,
          29421.71,
          36.1,
          1062231.209
        ],
        "sequenceId": 390,
        "ts": 1690878910379
      },
      {
        "type": "TICKER",
        "symbol": "ETH_USDT",
        "data": [
          1690794720000,
          1800,
          1800,
          1800,
          1800,
          1,
          1800
        ],
        "sequenceId": 329,
        "ts": 1690794732387
      }
    ]
  }
}
```
```
- data数据格式：[timestamp, open, high, low, close, volume, amount]
```


## 获取所有币币交易对统计价格

API描述：获取所有币币交易对的最近24小时统计价格。

API路径：GET /v2/market/spots/all-ticker

API示例：GET [/v2/market/spots/all-ticker](https://defiapi.876ex.com/v2/market/spots/all-ticker)

API请求参数：

- 无

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      {
        "type": "TICKER",
        "symbol": "BTC_USDT",
        "data": [
          1690878900000,
          29424.71,
          29434.1,
          29421.71,
          29421.71,
          36.1,
          1062231.209
        ],
        "sequenceId": 390,
        "ts": 1690878910379
      },
      {
        "type": "TICKER",
        "symbol": "ETH_USDT",
        "data": [
          1690794720000,
          1800,
          1800,
          1800,
          1800,
          1,
          1800
        ],
        "sequenceId": 329,
        "ts": 1690794732387
      }
    ]
  }
}
```



## 获取OrderBook（现货）

API描述：获取某个交易对的最近OrderBook。

API路径：GET /v2/market/spots/orderbook/:symbol_name

API示例：GET [/v2/market/spots/orderbook/BTC_USDT](https://defiapi.876ex.com/v2/market/spots/orderbook/BTC_USDT)

API请求参数(Path Param)：

| 参数              | 类型        | 说明                           |
|:----------------|-----------|:-----------------------------|
| **symbol_name** | **path**  | **必填**<br>交易对名称,例如`BTC_USDT` |
| **depth**       | **param** | **非必填**<br> 默认25档 最大支持150档   |
```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "symbolId": 100105,
    "symbol": "BTC_USDT",
    "price": 29421.71,
    "sellOrders": [],
    "buyOrders": [],
    "sequenceId": 390,
    "direction": "SHORT",
    "timestamp": 1691216889145
  }
}
```


## 获取OrderBook（合约）

API描述：获取某个交易对的最近OrderBook。

API路径：GET /v2/market/contracts/orderbook/:symbol_name

API示例：GET [/v2/market/contracts/orderbook/XBTCUSD_PERP](https://defiapi.876ex.com/v2/market/contracts/orderbook/XBTCUSD_PERP)

API请求参数(Path Param)：

| 参数       | 类型     | 说明                              |
| :--------- | -------- | :-------------------------------- |
| **symbol_name** | **path** | **必填**<br>交易对名称,例如`XBTCUSD_PERP` |

```
API响应样例:
```

```json
{
    // 最近成交价
    "price": 9179.33,
    // 序列ID
    "sequenceId": 40519,
    // 买盘
    "buyOrders": [
        [9175.08,0.35],
        [9168.73,0.54]
    ],
    // 卖盘
    "sellOrders": [
        [9179.33,0.76],
        [9181.37,0.97]
    ]
}
```

## 获取REST Tick数据

API描述：获取某个交易对的最近tick信息。接口不区分现货、合约。

API路径：GET /v2/market/ticks/:symbol_name

API示例：GET [/v2/market/ticks/BTC_USDT](https://defiapi.876ex.com/v2/market/ticks/BTC_USDT?limit=10)

API请求参数(Path Param)：

| 参数       | 类型     | 说明                                     |
| :--------- | -------- | :--------------------------------------- |
| **symbol_name** | **path** | **必填**<br>交易对名称,例如`BTC_USDT`    |
| **limit**  | **int**  | **选填**<br>获取tick数量, 1-500，默认200 |

```
API响应样例：
```

```json
{
    "results":[
        {
            "sequenceId":1666486,
            "data":[
                [
                    1596194089219,
                    0,
                    11171.6,
                    0.0119,
                    0
                ],
              	[
                    1596194089219,
                    0,
                    11171.6,
                    0.028,
                    0
                ]
            ]
        },
        {
            "sequenceId":1666477,
            "data":[
                [
                    1596194086281,
                    0,
                    11171.6,
                    0.0239,
                    0
                ]
            ]
        },
        {
            "sequenceId":1666474,
            "data":[
                [
                    1596194085781,
                    0,
                    11171.6,
                    0.0266,
                    0
                ]
            ]
        },
        {
            "sequenceId":1666462,
            "data":[
                [
                    1596194082834,
                    0,
                    11171.6,
                    0.0162,
                    0
                ]
            ]
        },
        {
            "sequenceId":1666460,
            "data":[
                [
                    1596194081031,
                    1,
                    11183,
                    0.0266,
                    0
                ]
            ]
        },
        {
            "sequenceId":1666459,
            "data":[
                [
                    1596194081006,
                    0,
                    11171.6,
                    0.0266,
                    0
                ]
            ]
        },
        {
            "sequenceId":1666432,
            "data":[
                [
                    1596194067487,
                    0,
                    11171.6,
                    0.0137,
                    0
                ]
            ]
        },
        {
            "sequenceId":1666423,
            "data":[
                [
                    1596194065608,
                    0,
                    11172.3,
                    0.0177,
                    0
                ]
            ]
        },
        {
            "sequenceId":1666417,
            "data":[
                [
                    1596194062609,
                    0,
                    11175.2,
                    0.0146,
                    0
                ]
            ]
        },
        {
            "sequenceId":1666411,
            "data":[
                [
                    1596194060078,
                    0,
                    11175.2,
                    0.0124,
                    0
                ]
            ]
        }
    ]
}
```

说明：

tick数据格式：`[timestamp, dir, price, amount, flag]`

| 参数          | 说明                           |
| :------------ | :----------------------------- |
| **timestamp** | 时间戳，单位毫秒               |
| **dir**       | 1=主动买入, 0=主动卖出         |
| **price**     | 成交价格                       |
| **amount**    | 成交量                         |
| **flag**      | 0=普通成交（后续增加爆仓标志） |



## 获取Bar数据

API描述：获取某个交易对的最近Bar数据。接口不区分现货、合约。

API路径：GET /v2/market/bars/:symbol_name/:type

API示例：GET [/v2/market/bars/BTC_USDT/min](https://defiapi.876ex.com/v2/market/bars/BTC_USDT/min)

API请求参数(Path Param)：

|参数| 类型 | 说明 |
|:---|:---|----|
|**symbol**|**string**|**必填**<br>交易对名称,例如`BTC_USDT`|
|**type**|**enum**|**必填**<br>Bar类型，目前支持`MIN`、`MIN5`、`MIN15`、`MIN30`、`HOUR`、`HOUR4`、`DAY`、`WEEK`、`MONTH`几种类型|

API请求参数(Request Param)：

|参数| 类型 | 说明 |
|:---|:---|----|
|**start**|**long**|**选填**<br/>起始时间戳，单位ms|
|**end**|**long**|**选填**<br/>结束时间戳，单位ms|
|**limit**|**long**|**选填**<br/>获取BAR的数量，默认200，最大200|

```
API响应样例：
```

```json
{
"results": [
    [
      1595584440000,
      9526.76,
      9568.38,
      9526.76,
      9528.71,
      1.11
    ],
    [
      1595584380000,
      9526.76,
      9529.63,
      9526.76,
      9529.63,
      0.3
    ],
    [
      1595584320000,
      9526.76,
      9562.95,
      9526.76,
      9528.05,
      1.64
    ],
    [
      1595584260000,
      9530.33,
      9562.95,
      9526.76,
      9532.56,
      1.23
    ],
    [
      1595584200000,
      9526.76,
      9562.95,
      9526.76,
      9530.33,
      0.93
    ]
	]
}
```

说明：

bar数据格式：

`[timestamp, open, high, low, close, amount]`

`[时间戳，开盘价，最高价，最低价，收盘价，成交量]`

结果总是按时间戳升序排序

