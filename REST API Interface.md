# REST API for Hopex    

    
## 请求交互    

REST访问的根URL：`https://api2.hopex.com/api/v1/` 

所有请求基于Https协议
	
请求交互说明    
1. 请求参数：根据接口请求参数规定进行参数封装。    
2. 提交请求参数：将封装好的请求参数通过POST 或GET 方式提交至服务器。    
3. 服务器响应：服务器首先对用户请求数据进行参数安全校验,通过校验后根据业务逻辑将响应数据以JSON格式返回给用户。    
4. 数据处理：对服务器响应数据进行处理。 

## API参考  

### 合约行情 API 

获取Hopex合约行情数据  

1. Get /api/v1/ticker    获取Hopex合约行情,访问频率 10次/秒

示例	

```
# Request
GET https://api2.hopex.com/api/v1/ticker?contractCode=BTCUSDT
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
    "amount24h": "3,735,255 USDT",
    "lastPriceToCNY": "￥26242.47",
    "quantity24h": "13,736,960"
  },
  "ret": 0,
  "env": 0,
  "errCode": null,
  "errStr": null,  
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
lastPriceToCNY: 最新价To CNY
quantity24h: 24小时交易量
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|contractCode|String|是|BTCUSDT ETHUSDT BTCUSD ETHUSD等|

2. Post /api/v1/depth   获取Hopex合约深度信息,访问频率 10次/秒

示例	

```
# Request
POST https://api2.hopex.com/api/v1/depth
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
  "errCode": null,
  "errStr": null,  
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

3. Get /api/v1/trades   获取Hopex新成交信息,访问频率 10次/秒

示例	

```
# Request
GET https://api2.hopex.com/api/v1/trades?contractCode=BTCUSDT&pageSize=1
# Response
{
  "data": [
    {
      "id": 3102886,
      "time": "19:45:20",
      "timestamp": 1573540239.530246,      
      "fillPrice": "3504.0",
      "fillQuantity": "1,603",
      "side": "1"
    }
  ],
  "ret": 0,
  "env": 0,
  "errCode": null,
  "errStr": null,  
  "timestamp": 1548157523524
}
```

返回值说明	

```
time: 成交时间
timestamp: 成交时间戳
fillPrice: 成交价格
fillQuantity: 成交数量
side: 方向 1 sell 2 buy
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|contractCode|String|是|BTCUSDT ETHUSDT BTCUSD ETHUSD等|
|pageSize|int|是|请求个数|



 4. Get /api/v1/kline   获取Hopex K线,访问频率 10次/秒

示例	

```
# Request
GET https://api2.hopex.com/api/v1/kline?contractCode=BTCUSDT&endTime=1548160640&startTime=1548160040&interval=1m  (最多只能获取1000根K线数据)
# Response
{
    "data": {
        "decimalplace": "1",
        "timeData": [
            {
                "time": 1561223880,
                "open": "10901.0",
                "close": "10876.5",
                "high": "10901.0",
                "low": "10867.5",
                "vol": "194911",
                "val": "424098.559",
                "prevClose": "0.0",
                "upDown": "-24.50",
                "upDownRate": "-0.22%",
                "direct": 1,
                "contractValue": "0.0001"
            },
            ...
        ]
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1561343966013
}
```

返回值说明	

```
data: K线数据
decimalplace: 小数点位数
time: 时间戳
open: 开市值
close: 闭市值
high: 最高价
low: 最低价
vol: 成交量
val: 成交额
prevClose: 上一笔的闭市价
upDown: 涨跌额
upDownRate: 涨跌率
direct: 合约方向
contractValue: 合约价值
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|contractCode|String|是|BTCUSDT ETHUSDT BTCUSD ETHUSD等|
|endTime|int64|是|结束时间戳(单位秒)|
|startTime|int64|是|开始时间戳(单位秒)|
|interval|String|是|间隔标识, 1m->1分钟K线 5m->5分钟K线 1h->1小时K线 1d->天K线 1w->周K线 1M->月K线 依此类推|



 5. Get /api/v1/markets   获取Hopex 所有合约行情,访问频率 10次/秒

示例	

```
# Request
GET https://api2.hopex.com/api/v1/markets
# Response
{
    "data": [
        {
            "contractCode": "BTCUSDT",
            "contractName": "BTC/USDT永续",
            "allowTrade": true,
            "hasPosition": false,
            "posiDirect": 0,
            "closeCurrency": "USDT",
            "quotedCurrency": "USDT",
            "precision": 2,
            "minPriceMovement": 0.5,
            "pricePrecision": 1,
            "lastestPrice": 3904.5,
            "changePercent24h": -0.00153433064825469888761028,
            "sumAmount24h": 7354.4393639723098773,
            "sumAmount24hUSDT": 75235914.6934367300447790
        },
        ...
   ],
  "ret": 0,
  "env": 0,
  "errCode": null,
  "errStr": null,  
  "timestamp": 1548160685910
}
```

返回值说明	

```
contractCode: 合约编码
contractName: 合约名称
allowTrade: 是否允许交易
closeCurrency: 结算货币
minPriceMovement: 最小变动价位
lastestPrice: 最新价
pricePrecision: 价格精度
changePercent24h: 24小时涨跌幅
sumAmount24h：24小时交易额, 以结算货币为单位
sumAmount24hUSDT: 24小时交易额, 以USDT为单位
```

6. Get /api/v1/indexStat    获取Hopex成交量,访问频率 10次/秒

示例	

```
# Request
GET https://api2.hopex.com/api/v1/indexStat
# Response
{
  "data": {
        "posVauleUSD": "568,467,422.95",
        "posVauleCNY": "3,943,771,170.06",
        "amount24hUSD": "1,504,196,973.56",
        "amount24hCNY": "10,435,441,713.95",
        "amount7dayUSD": "12,702,202,974.38",
        "amount7dayCNY": "88,122,168,244.92",
        "userCount": "527,670",
        "dealCountUSD": "1,765,475,066,609.42",
        "dealCountCNY": "12,248,071,548,356.19"
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1583300598281
}
```

返回值说明	

```
posVauleUSD: 未平仓合约价值(USD)
posVauleCNY: 未平仓合约价值(CNY)
amount24hUSD: 24小时交易额(USD)
amount24hCNY: 24小时交易额(CNY)
amount7dayUSD: 7天交易额(USD)
amount7dayCNY: 7天交易额(CNY)
userCount: 用户数
dealCountUSD: 总交易额(USD)
dealCountCNY: 总交易额(CNY)
```

### 合约交易 API
用于Hopex合约交易

1. Get /api/v1/userinfo    获取Hopex用户信息,访问频率 1次/秒

示例	

```
# Request
GET https://api2.hopex.com/api/v1/userinfo
# Response
{
    "data": {
        "conversionCurrency": "USD",
        "profitRate": "0.00%",
        "totalWealth": "0.00",
        "floatProfit": "0.00",
        "position": 0,
        "activeOrder": 0
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1555554824314
}
```

返回值说明	

```
conversionCurrency: 计价货币
profitRate: 当前持仓收益率(浮动盈亏/持仓占用保证金)
totalWealth: 账户总权益估值
floatProfit: 总浮动盈亏估值
position: 持仓数
activeOrder: 活跃委托书数
``` 

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证, Request Header|
|Date|String|是|当前的GMT时间, Request Header|
|Digest|String|是|请求包体摘要, Request Header|
|conversionCurrency|String|否|计价货币,默认为USD, Request Header|


2. Post /api/v1/order    下单,访问频率 1次/秒

示例	

```
# Request
POST https://api2.hopex.com/api/v1/order
{
  "param": {
    "contractCode": "BTCUSDT",
    "side": 2,
    "orderQuantity": 100,
    "orderPrice": 5255.5
  }
}
# Response
{
    "data": 1950354706,
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1555559102763
}
```

返回值说明	

```
ret: 为0表示下委托成功
data: 订单id
``` 

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证|
|Date|String|是|当前的GMT时间|
|Digest|String|是|请求包体摘要|
|contractCode|String|是|BTCUSDT ETHUSDT BTCUSD ETHUSD等|
|side|int|是|1:买入开多, 2:卖出开空, 3:买入平空, 4:卖出平多|
|orderQuantity|int|是|订单数量|
|orderPrice|number|否|最小变动价位整数倍,不填时表示下市价单|


3. Get /api/v1/cancel_order    撤单,访问频率 1次/秒

示例	

```
# Request
GET https://api2.hopex.com/api/v1/cancel_order?orderId=1952293255&contractCode=BTCUSDT

# Response
{
    "data": true,
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1555569296445
}
```

返回值说明	

```
ret: 为0且data为true时表示撤单成功
data: true表示撤单成功
``` 

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证|
|Date|String|是|当前的GMT时间|
|Digest|String|是|请求包体摘要|
|orderId|int|是|委托id|
|contractCode|String|是|BTCUSDT ETHUSDT BTCUSD ETHUSD等|


4. Get /api/v1/order_info    获取用户的活跃委托,访问频率 1次/秒

示例	

```
# Request
GET https://api2.hopex.com/api/v1/order_info?contractCode=BTCUSDT

# Response
{
    "data": [
        {
            "orderId": 120396444,
            "orderType": "买入开多",
            "direct": 1,
            "contractCode": "BTCUSDT",
            "contractName": "BTC/USDT永续",
            "type": "1",
            "side": "2",
            "sideDisplay": "买入",
            "ctime": "2019-06-26 15:03:09",
            "mtime": "2019-06-26 15:03:09",
            "orderQuantity": "+1,000",
            "leftQuantity": "1,000",
            "fillQuantity": "0",
            "orderStatus": "2",
            "orderStatusDisplay": "等待成交",
            "orderPrice": "12785.5",
            "leverage": 20,
            "fee": "--",
            "avgFillMoney": "--",
            "orderMargin": "66.4846 USDT",
            "expireTime": "2019-07-03 15:03:09"
        }
    ],
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1561532593797
}   
```

返回值说明	

```
orderId: 订单ID
orderType 委托类型
direct 1 多仓，2空仓
contractCode: 合约code
contractName: 合约name
type: 1.限价开仓 2.市价开仓 3.限价全平 4.市价全平 5.限价部分平仓单 6.市价部分平仓单
side: 方向, 1:卖 2买
sideDisplay: 方向
ctime: 创建时间
mtime: 更新时间
orderQuantity: 数量（张）
leftQuantity: 还剩下多少没有成交
fillQuantity: 已经成交的数量
orderStatus: 订单状态:1.部分成交 2:等待成交
orderStatusDisplay: 订单状态
orderPrice: 委托价
leverage: 杠杆倍数（2位小数）
fee: 手续费(小数点后4位)
avgFillMoney: 成交均价(指数_合理价格小数位数)
orderMargin: 委托保证金(小数点后4位)
expireTime: 过期时间
``` 

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证|
|Date|String|是|当前的GMT时间|
|Digest|String|是|请求包体摘要|
|contractCode|String|否|不传时查所有合约的活跃委托|


5. Post /api/v1/order_history    获取历史委托,访问频率 1次/秒

示例	

```
# Request
POST https://api2.hopex.com/api/v1/order_history
{
  "param": {
    "contractCodeList": [
      
    ],
    "typeList": [
      
    ],
    "side": 0,
    "startTime": 0,
    "endTime": 0
  }
}

# Response
{
    "data": {
        "totalCount": 0,
        "page": 2,
        "pageSize": 10,
        "result": [
            {
                "orderId": 1935768402,
                "contractCode": "BTCUSDT",
                "contractName": "BTC/USDT永续",
                "type": "4",
                "side": "1",
                "direct": 2,
                "sideDisplay": "卖出",
                "ctime": "2019-04-17 10:55:51",
                "ftime": "2019-04-17 10:55:51",
                "orderQuantity": "-100",
                "fillQuantity": "-100",
                "orderStatus": "2",
                "orderStatusDisplay": "完全成交",
                "orderPrice": "市价",
                "leverage": "20.00",
                "fee": "0.0522 USDT",
                "avgFillMoney": "5219.50",
                "closePosPNL": "-0.0050 USDT",
                "timestamp": 1555469751974534,
                "orderType": "卖出开空"
            },
            ...
        ]
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1555571374944
}      
```

返回值说明	

```
orderId: 订单ID
contractCode: 合约code
contractName: 合约name
type: 1.限价 2.市价 3.限价全平 4.市价全平
side: 方向, 1:卖 2买
direct: 订单对应的持仓方向: 1.多仓 2.空仓
sideDisplay: 方向
ctime: 创建时间
ftime: 完成时间
orderQuantity: 数量（张）
fillQuantity: 已经成交的数量
orderStatus: 订单状态:1.部分成交 2:等待成交
orderStatusDisplay: 订单状态
orderPrice: 委托价
leverage: 杠杆倍数（2位小数）
fee: 手续费(小数点后4位)
avgFillMoney: 成交均价(指数_合理价格小数位数)
closePosPNL: 平仓盈亏 小数点后4位
timestamp: 时间戳(微秒)
orderType: 买入开多 卖出开空 买入平空 卖出平多
``` 

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证|
|Date|String|是|当前的GMT时间|
|Digest|String|是|请求包体摘要|
|contractCodeList|[]|是|合约列表,为空查询所有|
|typeList|[]|是|1.限价开仓 2.市价开仓 3.限价全平 4.市价全平 5.限价部分平仓单 6.市价部分平仓单|
|side|int|是|0:no limit 1 for sell, 2 for buy.|
|startTime|int|是|开始时间戳|
|endTime|int|是|结束时间戳|
|page|int|是|第几页,默认1|


6. Get /api/v1/position    获取持仓,访问频率 1次/秒

示例	

```
# Request
GET https://api2.hopex.com/api/v1/position

# Response
{
    "data": [
        {
            "allowFullClose": true,
            "contractCode": "BTCUSDT",
            "contractName": "BTC/USDT永续",
            "leverage": "20.00",
            "contractValue": "0.0001",
            "maintMarginRate": "0.005",
            "takerFee": "0.001",
            "positionQuantity": "+100",
            "direct": 2,
            "posiDirect": 1,
            "posiDirectD": "持多",
            "entryPrice": "5261.00",
            "entryPriceD": 5261,
            "positionMargin": "2.6831 USDT",
            "positionMarginD": 2.68311,
            "liquidationPrice": "5024.02",
            "maintMargin": "0.3133 USDT",
            "unrealisedPnl": "-0.0025 USDT",
            "unrealisedPnlPcnt": "-0.09%",
            "fairPrice": "5260.97",
            "fairPriceD": 5260.97,
            "lastPrice": "5260.5",
            "sequence": 0,
            "rank": 0,
            "minPriceMovement": 0.5,
            "minPriceMovementPrecision": 1,
            "positionQuantityFreeze": "0",
            "closeablePositionQuantity": "200"
        }
    ],
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1555582208144
}
```

返回值说明	

```
allowFullClose: 允许全平
contractCode: 合约编码
contractName: 合约名称
leverage: 杠杆倍数
contractValue: 合约价值
maintMarginRate: 合约的维持保证金率
takerFee: 提取方费率
positionQuantity: 持仓量
direct: 持仓方向 1: 多仓，2：空仓
posiDirect: 持仓方向（持多 1 /持空 -1)
posiDirectD: 持仓方向（持多/持空)
entryPrice: 开仓均价
entryPriceD: 开仓均价
positionMargin: 持仓占用保证金
positionMarginD: 持仓占用保证金
liquidationPrice: 强平价格
maintMargin: 维持保证金
unrealisedPnl: 浮动盈亏
unrealisedPnlPcnt: 收益率
fairPrice: 合理价格
fairPriceD: 合理价格
lastPrice: 最新成交价
sequence: 档长度
rank: 用户落在第几档
minPriceMovement: 最小变动价位
minPriceMovementPrecision: 最小变动价位精度
positionQuantityFreeze: 冻结的持仓量,等待成交的平仓订单的总量
closeablePositionQuantity: 可平数量
``` 

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证|
|Date|String|是|当前的GMT时间|
|Digest|String|是|请求包体摘要|
|contractCode|String|否|合约列表|


7. Get /api/v1/wallet    获取资产信息,访问频率 1次/秒

示例	

```
# Request
GET https://api2.hopex.com/api/v1/wallet

# Response
{
    "data": {
        "detail": [
            {
                "assetName": "USDT",
                "totalWealth": "4.60731228"
            },
            {
                "assetName": "BTC",
                "totalWealth": "0.00000000"
            },
            {
                "assetName": "ETH",
                "totalWealth": "0.77710121"
            }
        ]
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1555583462806
}
```

返回值说明	

```
assetName: 资产名字
totalWealth: 数量
``` 

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证|
|Date|String|是|当前的GMT时间|
|Digest|String|是|请求包体摘要|


8. Get /api/v1/get_leverage    获取用户合约杠杆,访问频率 1次/秒

示例	

```
# Request
GET https://api2.hopex.com/api/v1/get_leverage?contractCode=BTCUSDT

# Response
{
    "data": {
        "longLeverage": "20.00",
        "shortLeverage": "15.00",
        "longLeverageEditable": true,
        "shortLeverageEditable": true,
        "varyRange": "0.5",
        "maintenanceMarginRate": "0.5%",
        "minLeverage": 2,
        "maxLeverage": 100,
        "defaultLeverage": 59
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1563009712076
}
```

返回值说明	

```
longLeverage: 多仓杠杆（如果没有设置为最大杠杆或默认杠杆）
shortLeverage: 空仓杠杆
longLeverageEditable: 多仓杠杆是否允许编辑（如果有活跃委托或者活跃计划委托就不允许编辑）
shortLeverageEditable: 空仓杠杆是否允许编辑（如果有活跃委托或者活跃计划委托就不允许编辑）
varyRange: 合约的委托列表区间
maintenanceMarginRate: 维持保证金率
minLeverage: 杠杆范围-最小值
maxLeverage: 杠杆范围-最大值
defaultLeverage: 默认杠杆
``` 

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证|
|Date|String|是|当前的GMT时间|
|Digest|String|是|请求包体摘要|
|contractCode|String|是|合约名字|


9. Get /api/v1/set_leverage    获取用户合约杠杆,访问频率 1次/秒

示例	

```
# Request
GET https://api2.hopex.com/api/v1/set_leverage?contractCode=BTCUSDT&direct=2&leverage=70

# Response
{
    "data": 70,
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1563010363110
}
```

返回值说明	

```
data: 设置成功的杠杆倍数
``` 

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证|
|Date|String|是|当前的GMT时间|
|Digest|String|是|请求包体摘要|
|contractCode|String|是|合约名字|
|direct|String|是|方向:1 多仓，2 空仓|
|leverage|String|是|杠杆倍数|


10. Post /api/v1/liquidation_history    获取强平历史,访问频率 1次/秒

示例	

```
# Request
POST https://api2.hopex.com/api/v1/liquidation_history
{
  "param": {
    "contractCodeList": [
      
    ],
    "side": 0
  }
}

# Response
{
    "data": {
        "totalCount": 7,
        "page": 1,
        "pageSize": 10,
        "result": [
            {
                "orderId": 7,
                "contractCode": "ETHUSDT",
                "contractName": "ETH/USDT永续",
                "side": "2",
                "sideDisplay": "买入",
                "orderType": "买入平空",
                "direct": 2,
                "leverage": 20,
                "orderQuantity": "+70,731",
                "orderPrice": "194.43",
                "closePosPNL": "-6545.5086 USDT",
                "fee": "68.7606 USDT",
                "ctime": "2019-10-30 14:19:52",
                "timestamp": 1572416393251656,
                "direction": 1,
                "directionDisplay": "空",
                "positionMargin": "+6614.2692 USDT",
                "openPrice": "185.17",
                "liquidationPriceReal": "193.50",
                "showDetail": false
            },
	    ...
            {
                "orderId": 2,
                "contractCode": "EOSUSDT",
                "contractName": "EOS/USDT永续",
                "side": "1",
                "sideDisplay": "卖出",
                "orderType": "卖出平多",
                "direct": 1,
                "leverage": 20,
                "orderQuantity": "-1,123",
                "orderPrice": "6.4883",
                "closePosPNL": "-383.6963 USDT",
                "fee": "3.6432 USDT",
                "ctime": "2019-06-27 04:44:47",
                "timestamp": 1561581887104635,
                "direction": 2,
                "directionDisplay": "多",
                "positionMargin": "+387.3395 USDT",
                "openPrice": "6.8300",
                "liquidationPriceReal": "6.5567"
            }
        ]
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1572501526044
}
```

返回值说明	

```
orderId: 订单ID
contractCode: 合约code
contractName: 合约name
side: 方向, 1:卖 2买
sideDisplay: 方向
orderType: 买入开多 卖出开空 买入平空 卖出平多
direct: 订单对应的持仓方向: 1.多仓 2.空仓
leverage: 杠杆倍数（2位小数）
ftime: 完成时间
orderQuantity: 数量（张）
orderPrice: 价格
closePosPNL: 平仓盈亏 小数点后4位
fee: 手续费(小数点后4位)
ctime: 创建时间
timestamp: 时间戳(微秒)
direction: 1.空 2.多
positionMargin: 持仓占用保证金 (4位小数)
openPrice: 开仓均价
liquidationPriceReal: 强平价格
``` 

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证|
|Date|String|是|当前的GMT时间|
|Digest|String|是|请求包体摘要|
|contractCodeList|[]|是|合约列表,为空查询所有|
|side|int|是|0:no limit 1 for sell, 2 for buy.|
|page|int|是|第几页,默认1|


11. Get /api/v1/account_records    获取用户出入金记录,访问频率 1次/秒

示例	

```
# Request
GET https://api2.hopex.com/api/v1/account_records?page=1&limit=10

# Response
{
    "data": {
        "totalCount": 8,
        "page": 1,
        "pageSize": 10,
        "result": [
            {
	    	"id": 58413,
                "asset": "USDT",
                "orderType": 4,
                "orderTypeD": "普通链上出金",
                "amount": "-1900.00000000 USDT",
                "rmbAmount": "0.00 CNY",
                "bankName": "",
                "addr": "mvYW73ZTo8F7aksBrWad3XvqViqDE3saaP",
                "orderStatus": 1,
                "orderStatusD": "完成",
                "createdTime": "2019-10-31 15:52:11"
            },
            ...
            {
	    	"id": 58421,
                "asset": "USDT",
                "orderType": 2,
                "orderTypeD": "OTC出金",
                "amount": "-100.00000000 USDT",
                "rmbAmount": "704.00 CNY",
                "bankName": "邮政",
                "addr": "",
                "orderStatus": 1,
                "orderStatusD": "完成",
                "createdTime": "2019-07-27 15:02:21"
            }
        ]
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1572935091833
}
```

返回值说明	

```
asset: 币种
orderType: 1 OTC入金，2 OTC出金，3 链上入金，4 链上出金，5 内部转账-入金，6 内部转账-出金, 7 人工入金, 8 人工出金,9 快速入金,10 快速出金
orderTypeD: 交易类型描述
amount: 数字货币金额
rmbAmount: 人民币金额
bankName: 银行名
addr: 钱包地址
orderStatus: 0 进行中，1 完成，2失败
orderStatusD: 交易状态描述
createdTime: 时间
``` 

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证|
|Date|String|是|当前的GMT时间|
|Digest|String|是|请求包体摘要|
|page|int|否|第几页,默认1|
|limit|int|否|每页返回条数,默认20|





验证的细节分为以下四个方面:
- 生成API Key
    - 对任何请求进行签名之前,须通过Hopex创建属于您的API Key和API Secret, API Key和API Secret将由Hopex随机生成和提供.
- 发起请求
    - 所有私有REST请求头都必须包含Authorization, Date和Digest.
- 签名
    - Authorization头的格式如下: -H 'Authorization: hmac apikey="...", algorithm="hmac-sha256", headers="date request-line digest", signature="..."', apikey为您的API Key, signature为method+request-line+" HTTP/1.1"+digest, 用secret使用HMAC SHA256方法加密，通过BASE64编码输出而得到的, request-line是请求接口路径. 如: /api/v1/userinfo, digest是请求body的摘要.
- GMT时间
    - Date请求头必须是发送请求时刻对应的标准格林威治时间格式,如 Fri, 19 Apr 2019 03:15:20 GMT, 如果发送请求时刻和服务器时间相差1分钟以上的请求将被系统视为过期并拒绝.


以下是生成Authorization, Date和Digest Request Header的示例程序:
``` javascript
var apiKey = "..."; // your api key
var apiSecret = "..."; // your applied secret key

var algorithm = "hmac-sha256";
var head_auth_headers = "date request-line digest";

var date = new Date().toGMTString();
pm.globals.set("Date", date);

var data = request.data;
var Digest = CryptoJS.SHA256(data).toString(CryptoJS.enc.Base64);
pm.globals.set('Digest', "SHA-256=" + Digest);

var textToSign = "";
textToSign += "date: " + date + "\n";
textToSign += request.method + " " + "/api/v1/userinfo" + " HTTP/1.1" + "\n";
textToSign += "digest: " + "SHA-256=" + Digest;
console.log("textToSign:\n" + textToSign.replace(/\n/g, "#"));
var signature = CryptoJS.HmacSHA256(textToSign, apiSecret).toString(CryptoJS.enc.Base64);
var head_auth = "hmac apikey=\"" + apiKey + "\", algorithm=\"" + algorithm  + "\", headers=\"" + head_auth_headers + "\", signature=\"" + signature + "\"";
pm.globals.set("Authorization",  head_auth);
``` 


