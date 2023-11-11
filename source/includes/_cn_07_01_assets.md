# 资产中心

资产中心API可用于获取资产中心、合约账户信息



## 获取资产中心账户余额

API描述：查询用户资产中心账户余额。

API路径：GET /v2/assets/user/accounts

API请求参数：无

返回：

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "BTC": [
      99800004  // 可用
      0         // 冻结
    ],
    "ETH": [
      99800000,
      0
    ],
    "USDT": [
      99800000,
      0
    ]
  }
}
```

## 资产中心划转到合约账户

API描述：从资产中心划转资产到合约账户

API路径：POST /v2/assets/contracts/transfer/out

API请求参数：JSON Object

| 参数         | 类型        | 说明                               |
| :----------- | ----------- | :--------------------------------- |
| **sourceId** | **string**  | **必填**<br>请求ID，唯一标识       |
| **currency** | **string**  | **必填**<br/>currency名称，如“BTC” |
| **amount**   | **decimal** | **必填**<br/>划转数量              |

返回：

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "id": 10873,
    "sourceId": "0492c6d34d4c499cb8a08c0baf8e0cfa",
    "targetAddress": null,
    "transferType": "INTERNAL",
    "transferFrom": "ASSETS",
    "transferTo": "CONTRACTS",
    "userId": 10010001017,
    "userTargetId": null,
    "currencyId": 100,
    "coinId": null,
    "coin": null,
    "amount": 0.5,
    "fee": null,
    "status": "PENDING",
    "confirm": 0,
    "minConfirmThreshold": null,
    "txid": null,
    "msg": null,
    "sign": "08ecf8cb8f663e403f60847fe8c171f5bd22137c",
    "createdAt": 1699674040027,
    "updatedAt": 1699674040027,
    "currency": "BTC"
  }
}
```






