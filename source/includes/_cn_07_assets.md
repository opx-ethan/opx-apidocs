# 资产中心

资产中心API可用于获取资产中心账户余额、与币币账户。



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
      99800004,
      0
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



## 资产中心划转到币币账户

API描述：从资产中心划转资产到币币账户

API路径：POST /v2/assets/spots/transfer/out

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
    "id": 10008,
    "sourceId": "38dea7c5108f498a8c4036214d02cf71",
    "targetAddress": null,
    "transferType": "INTERNAL",
    "transferFrom": "ASSETS",
    "transferTo": "SPOTS",
    "userId": 2,
    "userTargetId": null,
    "currencyId": 101,
    "amount": 200000.000000000000000000,
    "fee": null,
    "status": "DONE",
    "confirm": 0,
    "txid": null,
    "msg": null,
    "sign": "465e5fcefa6b38d462d59390fd6d2c6c0fa89fd1",
    "createdAt": 1690379350114,
    "updatedAt": 1690379350215,
    "currency": "ETH"
  }
}
```







