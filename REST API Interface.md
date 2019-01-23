# REST API for Hopex    

    
## 请求交互    

REST访问的根URL：`https://api.hopex.com/api/v1/gateway/` 

所有请求基于Https协议
	
请求交互说明    
1. 请求参数：根据接口请求参数规定进行参数封装。    
2. 提交请求参数：将封装好的请求参数通过POST 或GET 方式提交至服务器。    
3. 服务器响应：服务器首先对用户请求数据进行参数安全校验,通过校验后根据业务逻辑将响应数据以JSON格式返回给用户。    
4. 数据处理：对服务器响应数据进行处理。 

## API参考  

### 合约行情 API 

获取Hopex合约行情数据  

1. Get /api/v1/gateway/Home/ContractSummary    获取Hopex合约行情

示例	

```
# Request
GET https://api.hopex.com/api/v1/gateway/Home/ContractSummary?contractCode=BTCUSDT
# Response
{
  "data": {
    "contractCode": "BTCUSDT",
    "spotIndexCode": "spot_index_BTCUSDT",
    "contractName": "BTC/USDT永续",
    "closeCurrency": "USDT",
    "allowTrade": true,
    "pause": false,
    "lastPrice": "-3520.0",
    "lastPriceToUSD": "$3522.11",
    "lastPriceLegal": "$3522.11",
    "direction": -1,
    "changePercent24": "-0.21%",
    "marketPrice": "3518.86",
    "marketPriceInfo": "全球top10成交量交易所均价",
    "fairPrice": "3519.05",
    "fairePriceInfo": "合理价格=现货价格+最近15秒成交溢价,此价格可以更加合理地代表Hopex合约价格。浮动盈亏和强平价格均是根据合理价格来计算。",
    "price24Max": "3551.5",
    "price24Min": "3494.5",
    "amount24h": "3,735,255 USDT"
  },
  "ret": 0,
  "env": 0,
  "timestamp": 1548150572035
}
```

返回值说明	

```
contractCode: 合约编码
spotIndexCode: 现货指数编码
contractName: 合约名称
closeCurrency: 结算货币
allowTrade: 是否允许交易
pause: 是否暂停交易,暂停：true 启用: false
lastPrice: 最新价
lastPriceToUSD: 最新价To USD
lastPriceLegal: 最新价To 法币
direction: 最新价涨跌方向( 1上涨,0持平,-1下跌)
changePercent24: 24小时涨跌幅
marketPrice: 现货指数价格
marketPriceInfo: 现货指数价格-解释
fairPrice: 合理价格
fairePriceInfo: 合理价格-解释
price24Max: 24小时最高价
price24Min: 24小时最低价
amount24h: 24小时交易额
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|contractCode|String|是|BTCUSDT ETHUSDT BTCUSD ETHUSD等|

2. Get /api/v1/gateway/OrderBook/Index   获取Hopex合约深度信息

示例	

```
# Request
Post https://api.hopex.com/api/v1/gateway/OrderBook/Index
{
  "param": {
    "contractCode": "BTCUSDT",
  }
}
# Response
{
  "data": {
    "decimalplace": "1",
    "intervals": [
      "0.5"
    ],
    "asksFilter": "3545.9",
    "asks": [
      {
        "priceD": 3511,
        "orderPrice": "3511.0",
        "orderQuantity": 28701,
        "orderQuantityShow": "28,701",
        "exist": 0
      },
      ...
    ],
    "bidsFilter": "3475.6",
    "bids": [
      {
        "priceD": 3510.5,
        "orderPrice": "3510.5",
        "orderQuantity": 14619,
        "orderQuantityShow": "14,619",
        "exist": 0
      },
      ...
    ]
  },
  "ret": 0,
  "env": 0,
  "timestamp": 1548156640871
 }    
```

返回值说明	

```
decimalplace: 精度
intervals: 区间
asksFilter: 最大卖价
asks: 卖盘
bidsFilter: 最小买价
bids: 买盘
orderPrice: 价格
orderQuantity: 数量
exist: 此价格是否有用户自己的委托
```


请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|contractCode|String|是|BTCUSDT ETHUSDT BTCUSD ETHUSD等|

3. Get /api/v1/gateway/Home/GetDeals   获取Hopex新成交信息

示例	

```
# Request
GET https://api.hopex.com/api/v1/gateway/Home/GetDeals?contractCode=BTCUSDT&pageSize=1&lastId=3
# Response
{
  "data": [
    {
      "id": 3102886,
      "time": "19:45:20",
      "fillPrice": "3504.0",
      "fillQuantity": "1,603",
      "side": "1"
    }
  ],
  "ret": 0,
  "env": 0,
  "timestamp": 1548157523524
}
```

返回值说明	

```
time: 成交时间
fillPrice: 成交价格
fillQuantity: 成交数量
side: 方向 1 sell 2 buy
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|contractCode|String|是|BTCUSDT ETHUSDT BTCUSD ETHUSD等|
|pageSize|int|是|请求个数|
|lastId|int|是|默认填0|



 4. Get /api/v1/gateway/Home/KLines   获取Hopex K线

示例	

```
# Request
GET https://api.hopex.com/api/v1/gateway/Home/KLines?contractCode=BTCUSDT&endTime=1548160640&startTime=1548160040&interval=60
# Response
{
  "data": [
    [
      "1548160060",
      "3536.0",
      "3536.5",
      "3536.5",
      "3535.0",
      "6136",
      "2169.642",
      "BTCUSDT"
    ]
  ],
  "ret": 0,
  "env": 0,
  "timestamp": 1548160685910
}
```

返回值说明	

```
data: K线数据：成交时间戳,开/收/高/低价,成交量,成交额,合约名称
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|contractCode|String|是|BTCUSDT ETHUSDT BTCUSD ETHUSD等|
|endTime|int64|是|结束时间戳(单位秒)|
|startTime|int64|是|开始时间戳(单位秒)|
|interval|int|是|间隔(单位秒)|


