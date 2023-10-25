# 公开市场

公开市场API是指无需登录，即可直接访问的API。

## 获取交易信息

API描述：获取交易所当前所有交易。

API路径：GET [/v2/market/meta](http://13.112.107.117/v2/market/meta)

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

API示例：GET [/v2/market/ticker/BTC_USDT](http://13.112.107.117/v2/market/ticker/BTC_USDT)

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
    "symbol": "BTC_USDT",
    "data": [
      1698237360000,
      34664.61,
      34910.03,
      33253.31,
      34102.91,
      24368.79047,
      8.365057205026E8,
      -0.016203845939706
    ],
    "type": "TICKER",
    "sequenceId": 94102531,
    "ts": 1698237392509
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
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      {
        "type": "TICKER",
        "symbol": "BTC_USDT",
        "data": [
          1698237180000,
          34616.9,
          34910.03,
          33253.31,
          34204.88,
          24699.97564,
          847974381.44003,
          -0.011902278944677
        ],
        "sequenceId": 94100950,
        "ts": 1698237188172
      },
      {
        "type": "TICKER",
        "symbol": "ETH_USDT",
        "data": [
          1696737180000,
          1637.2,
          1639.22,
          1637.1,
          1639.22,
          426.18,
          697973.039,
          0.0012338138284877
        ],
        "sequenceId": 961,
        "ts": 1696737211715
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


## 获取所有币币交易对统计价格

API描述：获取所有币币交易对的最近24小时统计价格。

API路径：GET /v2/market/spots/all-ticker

API示例：GET [/v2/market/spots/all-ticker](http://13.112.107.117/v2/market/spots/all-ticker)

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
          1698237180000,
          34616.9,
          34910.03,
          33253.31,
          34204.88,
          24699.97564,
          847974381.44003,
          -0.011902278944677
        ],
        "sequenceId": 94100950,
        "ts": 1698237188172
      },
      {
        "type": "TICKER",
        "symbol": "ETH_USDT",
        "data": [
          1696737180000,
          1637.2,
          1639.22,
          1637.1,
          1639.22,
          426.18,
          697973.039,
          0.0012338138284877
        ],
        "sequenceId": 961,
        "ts": 1696737211715
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

API路径：GET /v2/market/spots/orderbook/:symbol_name

API示例：GET [/v2/market/spots/orderbook/BTC_USDT](http://13.112.107.117/v2/market/spots/orderbook/BTC_USDT)

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

API示例：GET [/v2/market/ticks/BTC_USDT](http://13.112.107.117/v2/market/ticks/BTC_USDT?limit=10)

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
          ],
          [
            1690878910379,
            0,
            29424.71,
            3.0,
            88274.13,
            0
          ],
          [
            1690878910379,
            0,
            29424.71,
            3.0,
            88274.13,
            0
          ],
          [
            1690878910379,
            0,
            29421.71,
            1.0,
            29421.71,
            0
          ]
        ]
      },
      {
        "sequenceId": 389,
        "data": [
          [
            1690878901245,
            1,
            29434.1,
            0.9,
            26490.69,
            0
          ]
        ]
      },
      {
        "sequenceId": 388,
        "data": [
          [
            1690878891166,
            0,
            29424.74,
            1.0,
            29424.74,
            0
          ]
        ]
      },
      {
        "sequenceId": 387,
        "data": [
          [
            1690878839689,
            0,
            29424.75,
            0.9,
            26482.275,
            0
          ],
          [
            1690878839689,
            0,
            29424.75,
            0.9,
            26482.275,
            0
          ],
          [
            1690878839689,
            0,
            29424.75,
            1.8,
            52964.55,
            0
          ]
        ]
      },
      {
        "sequenceId": 384,
        "data": [
          [
            1690878454992,
            1,
            29434.1,
            0.1,
            2943.41,
            0
          ]
        ]
      },
      {
        "sequenceId": 383,
        "data": [
          [
            1690878434470,
            0,
            29424.75,
            0.1,
            2942.475,
            0
          ]
        ]
      },
      {
        "sequenceId": 382,
        "data": [
          [
            1690878379864,
            0,
            29424.76,
            1.0,
            29424.76,
            0
          ]
        ]
      },
      {
        "sequenceId": 381,
        "data": [
          [
            1690877504746,
            0,
            29424.77,
            1.0,
            29424.77,
            0
          ]
        ]
      },
      {
        "sequenceId": 380,
        "data": [
          [
            1690877213439,
            1,
            29424.88,
            0.7,
            20597.416,
            0
          ]
        ]
      },
      {
        "sequenceId": 379,
        "data": [
          [
            1690877180900,
            1,
            29424.87,
            0.1,
            2942.487,
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

API示例：GET [/v2/market/bars/BTC_USDT/MIN](http://13.112.107.117/v2/market/bars/BTC_USDT/MIN)

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

