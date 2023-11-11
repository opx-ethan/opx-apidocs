# 公开市场

公开市场API是指无需登录，即可直接访问的API。

## 获取交易信息

API描述：获取交易所当前所有交易信息。

API路径：GET [/v2/market/meta](http://13.112.107.117/v2/market/meta)

API请求参数：

- 无

```
API响应样例：
```

```json
{
  "code":200,
  "msg":"success",
  "data":{
    "spotsSymbols":[

    ],
    "contractsCurrencies":[
      "BTC",
      "ETH",
      "USDT"
    ],
    "contractsSymbols":[
      {
        "id":16,
        "name":"BTCUSDT_PERP",  
        "openTime":1546300800000,
        "endTime":5000000000000,
        "type":"PERPETUAL",
        "marginCurrency":"USDT", 
        "sizeCurrency":"BTC",
        "inverse":false,
        "liquidateBy":"INDEX_PRICE",
        "multiplier":0.0001,          // 面值
        "minimumPriceIncrement":0.5,  // 价格最小步长
        "priceStep":5,
        "priceScale":1,
        "maximumQuantityPerOrder":2000,  // 每笔订单最大下单量
        "riskLimit":{
          "id":102,
          "initialMarginRate":0.01,          // 初始保证金率
          "maintenanceMarginRateStep":0.005,  // 维持保证金率
          "maxLeverage":100,                  // 最大杠杆倍数
          "riskLimitBase":200000,
          "riskLimitStep":100000,
          "maxRiskLimitSteps":9,
          "createdAt":1546956010600
        },
        "settlementFeeRate":0,
        "displayOrder":3,
        "hidden":false,
        "zone":null,
        "referencedIndexes":{
          "SPOT":30010,   // 指数价格
          "FAIR":30030,  // 标记价格
          "PREMIUM_1M":30031,
          "PREMIUM_AGGREGATE":30032,
          "MAX_PREMIUM_RATE":30021,
          "FUNDING_RATE_1M":30033,
          "FUNDING_RATE_AGGREGATE":30034, // 8小时资金费率
          "LENDING_RATE_1D":30020
        }
      }
    ],
    "marginSymbols":[
      {
        "id":100105,
        "name":"BTC_USDT",
        "openTime":0,
        "endTime":5000086400000,
        "baseName":"BTC",
        "baseMinimumIncrement":0.00001,
        "baseScale":5,
        "baseMinimumQuantity":0.00001,
        "baseMaximumQuantity":100000,
        "quoteName":"USDT",
        "quoteMinimumIncrement":0.01,
        "quoteScale":2,
        "supportMarginTrade":true,
        "alwaysChargeQuote":false,
        "displayOrder":0,
        "hidden":false,
        "derivative":false,
        "referencedIndexIdMap":{

        },
        "zone":null
      }
    ],
    "indexes":[
      {
        "id": 30010,
        "name": "BTCUSDT",
        "startTime": 0,
        "endTime": 5000000000000,
        "scale": 2
      },
      {
        "id": 30020,
        "name": "BTCUSDTDAILYRATE",
        "startTime": 0,
        "endTime": 5000000000000,
        "scale": 6
      },
      {
        "id": 30021,
        "name": "BTCUSDTMAXPIRATE",
        "startTime": 0,
        "endTime": 5000000000000,
        "scale": 6
      },
      {
        "id": 30030,
        "name": "BTCUSDTFAIR",
        "startTime": 0,
        "endTime": 5000000000000,
        "scale": 6
      },
      {
        "id": 30031,
        "name": "BTCUSDTPI",
        "startTime": 0,
        "endTime": 5000000000000,
        "scale": 6
      },
      {
        "id": 30032,
        "name": "BTCUSDTPI8H",
        "startTime": 0,
        "endTime": 5000000000000,
        "scale": 6
      },
      {
        "id": 30033,
        "name": "BTCUSDTFR",
        "startTime": 0,
        "endTime": 5000000000000,
        "scale": 6
      },
      {
        "id": 30034,
        "name": "BTCUSDTFR8H",
        "startTime": 0,
        "endTime": 5000000000000,
        "scale": 6
      },
      {
        "id": 30040,
        "name": "USDTINS1H",
        "startTime": 0,
        "endTime": 5000000000000,
        "scale": 6
      },
      {
        "id": 30041,
        "name": "USDTINS1D",
        "startTime": 0,
        "endTime": 5000000000000,
        "scale": 6
      }
    ],
    "spotsCurrencies":[
      "BTC",
      "ETH",
      "USDT"
    ],
    "riskLimits":[
      {
        "id":102,
        "initialMarginRate":0.01,
        "maintenanceMarginRateStep":0.005,
        "maxLeverage":100,
        "riskLimitBase":200000,
        "riskLimitStep":100000,
        "maxRiskLimitSteps":9,
        "createdAt":1546956010600
      }
    ],
    "currencies":[
      {
        "id":100,
        "name":"BTC",
        "derivative":false,
        "iconUrl":"https://static.opx.pro/icon/btc.svg",
        "displayScale":8,
        "depositOpenTime":0,
        "withdrawOpenTime":0,
        "hidden":false,
        "displayOrder":0,
        "displayName":"Bitcoin"
      },
      {
        "id":101,
        "name":"ETH",
        "derivative":false,
        "iconUrl":"https://static.opx.pro/icon/eth.svg",
        "displayScale":8,
        "depositOpenTime":0,
        "withdrawOpenTime":0,
        "hidden":false,
        "displayOrder":0,
        "displayName":"Ethereum"
      },
      {
        "id":105,
        "name":"USDT",
        "derivative":false,
        "iconUrl":"https://static.opx.pro/icon/usdt.svg",
        "displayScale":8,
        "depositOpenTime":0,
        "withdrawOpenTime":0,
        "hidden":false,
        "displayOrder":0,
        "displayName":"TetherUS"
      }
    ]
  }
}
```

## 获取时间

API描述：获取服务器当前时间，以毫秒为单位。

API路径：GET [/v2/market/timestamp](http://13.112.107.117/v2/market/timestamp)

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

API路径：GET [/v2/market/fex](http://13.112.107.117/v2/market/fex)

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

API路径：GET [/v2/market/errorCodes](http://13.112.107.117/v2/market/errorCodes)

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
      "zh_TW": "子賬戶 {{account}} 不合法",
      "en": "Account error: Account {{account}} invalid",
      "zh_CN": "子账户 {{account}} 不合法"
    },
    "ACCOUNT_NO_ENOUGH_AVAILABLE": {
      "zh_TW": "{{currency}} 賬戶缺少足夠的餘額",
      "en": "Account error: {{currency}} has no enough available",
      "zh_CN": "{{currency}} 账户缺少足够的余额"
    },
    "ACCOUNT_NO_ENOUGH_BALANCE": {
      "zh_TW": "餘額不足",
      "en": "Account error: no enough balance",
      "zh_CN": "余额不足"
    },
    "ACCOUNT_SIGN_INVALID": {
      "zh_TW": "賬戶 {{account}} 簽名不合法",
      "en": "Account error: Account {{account}} signature invalid",
      "zh_CN": "账户 {{account}} 签名不合法"
    }
    ......
  }
}
```

## 返回错误响应

API描述：返回一个错误响应，以便调试客户端错误处理代码。

API路径：GET [/v2/market/error](http://13.112.107.117/v2/market/error)

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

API示例：GET [/v2/market/ticker/BTCUSDT_PERP](http://13.112.107.117/v2/market/ticker/BTCUSDT_PERP)

API请求参数(Path Param)：

| 参数              | 类型        | 说明                           |
|:----------------|-----------|:-----------------------------|
| **symbol_name** | **path**  | **必填**<br>交易对名称,例如`BTCUSDT_PERP` |
| **limit      ** | **param** | **非必填**<br> 默认200            |
 

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
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
    "type": "TICKER",
    "sequenceId": 157154,
    "ts": 1699523058351
  }
}

```
说明：

数据格式：

`[timestamp, open, high, low, close, volume, amount, change]`

`[时间戳，开盘价，最高价，最低价，收盘价，成交量, 成交额，涨跌幅]`


## 获取所有统计价格

API描述：获取所有交易对的最近24小时统计价格.

API路径：GET /v2/market/all-ticker

API示例：GET [/v2/market/all-ticker](http://13.112.107.117/v2/market/all-ticker)

API请求参数：

- 无

```
API响应样例：
```

```json
{
  "code":200,
  "msg":"success",
  "data":{
    "results":[
      {
        "type":"TICKER",
        "symbol":"BTCUSDT_PERP",
        "data":[
          1699523040000,
          25000,
          25000,
          25000,
          25000,
          43,
          107.5,
          0
        ],
        "sequenceId":157154,
        "ts":1699523058351
      },
      {
        "type":"TICKER",
        "symbol":"ETHUSDT_PERP",
        "data":[
          1699608600000,
          2000,
          2000,
          2000,
          2000,
          3271,
          6542,
          0
        ],
        "sequenceId":205836,
        "ts":1699608607796
      }
    ]
  }
}
```
```
数据格式：

`[timestamp, open, high, low, close, volume, amount, change]`

`[时间戳，开盘价，最高价，最低价，收盘价，成交量, 成交额，涨跌幅]`
```


## 获取所有合约交易对统计价格

API描述：获取所有币币交易对的最近24小时统计价格。

API路径：GET /v2/market/contracts/all-ticker

API示例：GET [/v2/market/contracts/all-ticker](http://13.112.107.117/v2/market/contracts/all-ticker)

API请求参数：

- 无

```
API响应样例：
```

```json
{
  "code":200,
  "msg":"success",
  "data":{
    "results":[
      {
        "type":"TICKER",
        "symbol":"BTCUSDT_PERP",
        "data":[
          1699523040000,
          25000,
          25000,
          25000,
          25000,
          43,
          107.5,
          0
        ],
        "sequenceId":157154,
        "ts":1699523058351
      },
      {
        "type":"TICKER",
        "symbol":"ETHUSDT_PERP",
        "data":[
          1699608600000,
          2000,
          2000,
          2000,
          2000,
          3271,
          6542,
          0
        ],
        "sequenceId":205836,
        "ts":1699608607796
      }
    ]
  }
}
```
```
数据格式：

`[timestamp, open, high, low, close, volume, amount, change]`

`[时间戳，开盘价，最高价，最低价，收盘价，成交量, 成交额，涨跌幅]`
```


## 获取OrderBook

API描述：获取某个交易对的最近OrderBook。

API路径：GET /v2/market/contracts/orderbook/:symbol_name

API示例：GET [/v2/market/spots/orderbook/BTCUSDT_PERP](http://13.112.107.117/v2/market/contracts/orderbook/BTCUSDT_PERP)

API请求参数(Path Param)：

| 参数              | 类型        | 说明                           |
|:----------------|-----------|:-----------------------------|
| **symbol_name** | **path**  | **必填**<br>交易对名称,例如`BTCUSDT_PERP` |
| **depth**       | **param** | **非必填**<br> 默认25档 最大支持150档   |
```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "symbolId": 16,
    "symbol": "BTCUSDT_PERP",
    "price": 25777.52,
    "sellOrders": [
      [
        25777.60, // price
        1.86     // amount
      ],
      [
        25780.94,
        1.61
      ]
    ],
    "buyOrders": [
      [
        25777.46,
        0.47
      ],
      [
        25777.38,
        0.97
      ]
    ],
    "sequenceId": 945599,
    "direction": "LONG",
    "timestamp": 1693624727179
  }
}
```


## 获取REST Tick数据

API描述：获取某个交易对的最近tick信息。

API路径：GET /v2/market/ticks/:symbol_name

API示例：GET [/v2/market/ticks/BTCUSDT_PERP](http://13.112.107.117/v2/market/ticks/BTCUSDT_PERP?limit=10)

API请求参数(Path Param)：

| 参数       | 类型     | 说明                                     |
| :--------- | -------- | :--------------------------------------- |
| **symbol_name** | **path** | **必填**<br>交易对名称,例如`BTCUSDT_PERP`    |
| **limit**  | **int**  | **选填**<br>获取tick数量, 1-500，默认200 |

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
        "sequenceId": 157154,
        "data": [
          [
            1699523058351,
            0,
            25000,
            3,
            7.5,
            0
          ]
        ]
      },
      {
        "sequenceId": 155680,
        "data": [
          [
            1699519428699,
            0,
            25000,
            20,
            50,
            0
          ]
        ]
      }
    ]
  }
}
```

说明：

tick数据格式：`[timestamp, dir, price, volume, amount, flag]`

| 参数            | 说明             |
|:--------------|:---------------|
| **timestamp** | 时间戳，单位毫秒       |
| **dir**       | 1=主动买入, 0=主动卖出 |
| **price**     | 成交价格           |
| **volume**    | 成交量            |
| **amount**    | 成交额            |
| **flag**      | 0=普通成交         |



## 获取Bar数据

API描述：获取某个交易对的最近Bar数据。

API路径：GET /v2/market/bars/:symbol_name/:type

API示例：GET [/v2/market/bars/BTCUSDT_PERP/MIN](http://13.112.107.117/v2/market/bars/BTCUSDT_PERP/MIN)

API请求参数(Path Param)：

|参数| 类型 | 说明 |
|:---|:---|----|
|**symbol**|**string**|**必填**<br>交易对名称,例如`BTCUSDT_PERP`|
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
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      [
        1692777480000,
        29421.71,
        29421.71,
        29421.71,
        29421.71,
        0,
        0
      ],
      [
        1692777540000,
        29421.71,
        29421.71,
        29421.71,
        29421.71,
        0,
        0
      ]
    ]
  }
}
```

说明：

bar数据格式：

`[timestamp, open, high, low, close, volume, amount]`

`[时间戳，开盘价，最高价，最低价，收盘价，成交量, 成交额]`

结果总是按时间戳升序排序

