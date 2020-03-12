# REST API for Hopex    

    
## Request Process   

Root URL for REST access：`https://api2.hopex.com/api/v1/` 

All requests are based on Https protocol
	
Request Process Description
1. Request parameters: parameter encapsulation according to interface request parameter specifications.
2. Submit request parameters: submit the constructed request parameters to the server through POST or GET method.
3. Server response: the server first performs a parameter security check on the user's request data, and after authentication check, the response data is returned to the user in JSON format according to business logic.
4. Data processing: process the server response. 

Special note:
Some apis are response in Chinese default, if you prefer English version, please add the parameter: `culture=en` at every request.

## API Reference   

### Contract Price API 

Get Hopex Contract Price Data 

1. Get /api/v1/ticker    Get Hopex Contract Data, rate limit 10 times/s

Example	

```
# Request
GET https://api2.hopex.com/api/v1/ticker?contractCode=BTCUSDT
# Response
{
  "data": {
    "contractCode": "BTCUSDT",
    "spotIndexCode": "spot_index_BTCUSDT",
    "contractName": "BTC/USDT Swap",
    "closeCurrency": "USDT",
    "allowTrade": true,
    "pause": false,
    "lastPrice": "-3520.0",
    "lastPriceToUSD": "$3522.11",
    "lastPriceLegal": "$3522.11",
    "direction": -1,
    "changePercent24": "-0.21%",
    "marketPrice": "3518.86",
    "marketPriceInfo": "Average price of top 10 exchanges",
    "fairPrice": "3519.05",
    "fairePriceInfo": "Fair price=spot price+premium in last 15s. Unrealized PnL and liquidation price is calculated by fair price.",
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

Return Value 	

```
contractCode: Contract Code
spotIndexCode: Spot Index Code
contractName: Contract Name
closeCurrency: Settlement Currency
allowTrade: Whether the trade is applicable
pause: whether to pause trade or not, Pause：true  Continue: false
lastPrice: The latest price
lastPriceToUSD: Latest USD Price
lastPriceLegal: Latest Fiat Price
direction: Rise or fall of the latest price ( 1rise, 0 flat,-1fall)
changePercent24: 24h Rise or Fall 
marketPrice: Spot Index Price
marketPriceInfo: Spot Index Price - Explanation
fairPrice: Fair Price
fairePriceInfo: Fair Price- Explanation
price24Max: 24h Highest Price
price24Min: Highest price in the last 24 hours
amount24h: Trading volume in the last 24 hours
lastPriceToCNY: the lastest CNY price
quantity24h: 24h Trading Quantity
```

Request Parameter

|Parameter|    Type|	Required|	Description|
| :-----    | :-----   | :-----    | :-----   |
|contractCode|String| yes |BTCUSDT ETHUSDT BTCUSD ETHUSD etc.|

2.Post /api/v1/depth   Get Hopex Market Depth Information, rate limit 10 times/s

Example	

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

Return Value    

```
decimalplace: Precision
intervals: Interval
asksFilter: Highest Ask Price
asks: Asks
bidsFilter: Lowest Bid Price
bids: Bids
orderPrice: Order Price
orderQuantity: Quantity
exist: If there are traders’ orders placed at this price
```
Request Parameter	

|Parameter|	Type|	Required|	Description|
| :-----    | :-----   | :-----    | :-----   |
|contractCode|String| yes |BTCUSDT ETHUSDT BTCUSD ETHUSD etc|

3. Get /api/v1/trades   Get Hopex latest completed orders, rate limit 10 times/s


Example	

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

Return Value
```
time: Order Complete Time
timestamp: Time Stamp
fillPrice: Deal Price
fillQuantity: Order Amount
side: Type 1 Sell 2 Buy
```
Request Parameter    

|Parameter|	Type|	Required|	Description|
| :-----    | :-----   | :-----    | :-----   |
|contractCode|String| yes |BTCUSDT ETHUSDT BTCUSD ETHUSD etc|
|pageSize|int|yes|the Number of Requests|


 4. Get /api/v1/kline   Get Hopex Kline data, rate limit 10 times/s

Example	

```
# Request
GET https://api2.hopex.com/api/v1/kline?contractCode=BTCUSDT&endTime=1548160640&startTime=1548160040&interval=1m  (Get Maximun 1000 Kline Data)
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

Return Value
```
data: Kline Data
decimalplace: Decimal Places
time: Time Stamp
open: The Opening Price
close: The Closing Price
high: The Highest Price
low: The Lowest Price
vol: Trading Volume
val: Turnover
prevClose: Latest Close Price
upDown: Rise and Fall
upDownRate: Change Rate
direct: Long or Short
contractValue: Contract Value
```
Request Parameter
|Parameter|    Type|	Required|	Description|
| :-----    | :-----   | :-----    | :-----   |
|contractCode|String|no|BTCUSDT ETHUSDT BTCUSD ETHUSD etc|
|endTime|int64|yes|End Time-stamp(s)|
|startTime|int64|yes|Start Time-stamp(s)|
|interval|String|yes|interval mark, 1m->1min Kline 5m->5mins Kline 1h->1hr Kline 1d->1d Kline 1w->1week Kline 1M->1month Kline etc|




5. Get /api/v1/markets   Get Hopex markets data, rate limit 10 times/s

Example	

```
# Request
GET https://api2.hopex.com/api/v1/markets
# Response
{
    "data": [
        {
            "contractCode": "BTCUSDT",
            "contractName": "BTC/USDT Perpetual Swap",
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
            "sumAmount24hUSDT": 75235914.6934367300447790,
            "posVauleUSD": "234,406,032.14"
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

Return Value

```
contractCode: Contract Code
contractName: Contract Name
allowTrade: Allow trade or not
closeCurrency: Settlement Currency
minPriceMovement: Tick Size
lastestPrice: the Latest Price
pricePrecision: Price Precision
changePercent24h: 24h Change
sumAmount24h：24h Trading Turnover in Settlement Currency
sumAmount24hUSDT: 24h Trading Turnover in USDT
posVauleUSD: Open Interest
```

6. Get /api/v1/indexStat    Get Hopex Volume,rate limit 10 times/s

Example  

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

Return Value  

```
posVauleUSD: Open Interest(USD)
posVauleCNY: Open Interest(CNY)
amount24hUSD: 24H Volume(USD)
amount24hCNY: 24H Volume(CNY)
amount7dayUSD: Trading volume in the last 7 days(USD)
amount7dayCNY: Trading volume in the last 7 days(CNY)
userCount: User Count
dealCountUSD: Total Trading volume(USD)
dealCountCNY: Total Trading volume(CNY)
```

7. Get /api/v1/index_notify   Get Hopex Notifies,rate limit 10 times/s

示例  

```
# Request
GET https://api2.hopex.com/api/v1/index_notify?page=1&limit=5&culture=zh-CN 
# Response
{
    "data": {
        "totalCount": 42,
        "page": 1,
        "pageSize": 5,
        "result": [{
            "id": 118,
            "title": "Hopex Annual Review 2019",
            "link": "https://h5.hopex.com/notify/index.html?id=118",
            "lastModifiedTime": "2020-03-04 13:29:07",
            "time": "2020-03-04",
            "timestamp": 1583299747
        }]
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1583997209048
}
```

Return Value   

```
totalCount: Hopex Notify Count
title: Notify Title
link: Notify Link
lastModifiedTime: Notify Last Update Time
timestamp: timestamp
```

Request Parameter
|Parameter|    Type|    Required|   Description|
| :-----    | :-----   | :-----    | :-----   |
|page|int|no|Page No.,Default 1, Request Query|
|limit|int|no|Every Page Count,Default 20, Request Query|
|culture|String|否|Language(zh-CN,en,zh-HK),Default zh-CN,The number of announcements may be different in different languages,zh-CN prevail,Request Query|


### Trading API
Used for Hopex Contracts Trading

1. Get /api/v1/userinfo    Get Hopex user information, rate limit 1 time/s

Example	

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

Return Value    

```
conversionCurrency: Quote Currency
profitRate: Unrealized P/L Rate(Unrealized P/L/Position Margin)
totalWealth: Total Equity
floatProfit: Floating P/L
position: Positions
activeOrder: Active Orders
``` 

|Parameter|	Type|	Required|	Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|conversionCurrency|String|no|Quote Currency,Default in USD, Request Header|


2. Post /api/v1/order    Place Orders, rate limit 1 time/s

Example

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

Return Value    

```
ret: 0 represents successfully placed orders
data: Order ID
``` 

|Parameter|	Type|	Required|	Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|contractCode|String|no|BTCUSDT ETHUSDT BTCUSD ETHUSD etc|
|side|int|no|1:Buy Long, 2:Sell Short, 3:Buy to Close Short, 4:Sell to Close Long|
|orderQuantity|int|yes|Order Quantity|
|orderPrice|number|no|integral multiple of tick size. If not filled, it means place a market order|

3. Get /api/v1/cancel_order    Cancel Orders, rate limit 1 time/s

Example	

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

Return Value    

```
ret: return 0 and data is true means canceling is successful
data: true means canceling is successful
``` 

|Parameter|	Type|	Required|	Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|orderId|int|yes|Order id|
|contractCode|String|no|BTCUSDT ETHUSDT BTCUSD ETHUSD etc|


4.Get /api/v1/order_info    Get active orders information, rate limit 1 time/s

Example    

```
# Request
GET https://api2.hopex.com/api/v1/order_info?contractCode=BTCUSDT

# Response
{
    "data": [
        {
            "orderId": 120396444,
            "orderType": "Buy long",
            "direct": 1,
            "contractCode": "BTCUSDT",
            "contractName": "BTC/USDT Perpetual Swap",
            "type": "1",
            "side": "2",
            "sideDisplay": "BUY",
            "ctime": "2019-06-26 15:03:09",
            "mtime": "2019-06-26 15:03:09",
            "orderQuantity": "+1,000",
            "leftQuantity": "1,000",
            "fillQuantity": "0",
            "orderStatus": "2",
            "orderStatusDisplay": "Pending",
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

Return Value
```
orderId: Order ID
orderType: Order Type
direct 1 LONG,2SHORT
contractCode: Contract Code
contractName: Contract Name
type: 1.Limit Open2.Market Open 3.Limit Close 4.Market Close 5.Limit Partially Close 6.Market Partially Close
side: Buy or Sell, 1:Sell 2Buy
sideDisplay: Direction
ctime: Creation Time
mtime: Update Time
orderQuantity: Quantity（contracts）
leftQuantity: Pending Quantity
fillQuantity: Complete Quantity
orderStatus: Order Status:1.Partially Complete 2: Pending
orderStatusDisplay: Order Status
orderPrice: Order Price
leverage: Leverages（2 decimals）
fee: Trading Fee(4 decimals)
avgFillMoney: Average Deal Price(Index_Fair Price Decimals)
orderMargin: Order Margin(4 decimals)
expireTime: Expire Time
``` 

|Parameter|    Type|	Required|	Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|contractCode|String|no|if it is blank, it’s default to search for all active orders of all contracts|


5. Post /api/v1/order_history    Get History Orders, rate limit 1 time/s

Example
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
                "contractName": "BTC/USDT Perpetual Swap",
                "type": "4",
                "side": "1",
                "direct": 2,
                "sideDisplay": "Sell",
                "ctime": "2019-04-17 10:55:51",
                "ftime": "2019-04-17 10:55:51",
                "orderQuantity": "-100",
                "fillQuantity": "-100",
                "orderStatus": "2",
                "orderStatusDisplay": "Order Complete",
                "orderPrice": "Market Price",
                "leverage": "20.00",
                "fee": "0.0522 USDT",
                "avgFillMoney": "5219.50",
                "closePosPNL": "-0.0050 USDT",
                "timestamp": 1555469751974534,
                "orderType": "Sell to Open Short"
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

Return Value	

```
orderId: Order ID
contractCode: Contract Code
contractName: Contract Name
type: 1.Limit Order 2.Market Order 3.Limit Price to Close 4.Market Price to Close
side: Buy or Sell, 1:Sell 2 Buy
direct: Positions: 1.Long 2.Short
sideDisplay: Direction
ctime: Creation Time
ftime: Complete Time
orderQuantity: Quantity（contracts）
fillQuantity: Complete Quantity
orderStatus: Order Status:1.Partially Complete 2: Pending
orderStatusDisplay: Order Status
orderPrice: Order Price
leverage: Leverages（2 decimals）
fee: Trading Fee(4 decimals)
avgFillMoney: Average Deal Price(Index_Fair Price Decimals)
closePosPNL: Realized P/L 4 decimals
timestamp: Time Stamp(Microsecond)
orderType: Buy to Open Long, Sell to Open Short, Buy to Close Short, Sell to Close Short
``` 

|Parameter|	Type|	Required|	Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|contractCodeList|[]|yes|Contract List, Being blank to search all contracts, Request Body|
|typeList|[]|yes|1.Limit Price to Open 2.Market Price to Open 3.Limit Price to Close 4.Market Price to Close 5.Limit Price Close Partially Complete 6.Market Price Close Partially Complete, Request Body
|side|int|yes|0:no limit 1 for sell, 2 for buy, Request Body|
|startTime|int|yes|Start Time Stamp, Request Body|
|endTime|int|yes|End Time Stamp, Request Body|
|page|int|yes|Page No.,Default 1, Request Query|
|limit|int|no|Every Page Count,Default 10, Request Query|



6. Get /api/v1/position    Get positions information, rate limit 1 time/s

Example
```
# Request
GET https://api2.hopex.com/api/v1/position

# Response
{
    "data": [
        {
            "allowFullClose": true,
            "contractCode": "BTCUSDT",
            "contractName": "BTC/USDT Perpetual Swap",
            "leverage": "20.00",
            "contractValue": "0.0001",
            "maintMarginRate": "0.005",
            "takerFee": "0.001",
            "positionQuantity": "+100",
            "direct": 2,
            "posiDirect": 1,
            "posiDirectD": "Long Positions",
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
Return Value    

```
allowFullClose: Allow Full Close
contractCode: Contract Code
contractName: Contract Name
leverage: Leverages
contractValue: Contract Value
maintMarginRate: Maintenance Margin Rate 
takerFee: Taker Fee
positionQuantity: Position Quantity
direct: Long or Short 1: Long,2：Short
posiDirect: Positions（Long 1 /Short -1)
posiDirectD: Positions（Long/Short)
entryPrice: Average Open Price
entryPriceD: Average Open Price
positionMargin: Position Margin
positionMarginD: Position Margin
liquidationPrice: Liquidation Price
maintMargin: Maintenance Margin
unrealisedPnl: Unrealized P/L
unrealisedPnlPcnt: Unrealized P/L Rate
fairPrice: Fair Price
fairPriceD: Fair Price
lastPrice: the Latest Price
sequence: Sequence Length
rank: Rank
minPriceMovement: Tick Size
minPriceMovementPrecision: Tick Size Precision
positionQuantityFreeze: Frozen Positions,Total number of close orders that are in pending
closeablePositionQuantity: Quantity of  existing positions that can be closed
``` 

|Parameter|	Type|	Required|	Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|contractCode|String|no|Contract List|


7. Get /api/v1/wallet    Get asset information, rate limit 1 time/s

Example

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

Return Value
```
assetName: Asset Name
totalWealth: Asset Value
``` 

|Parameter|    Type|	Required|	Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|


8. Get /api/v1/get_leverage    Get positions’ leverages information, rate limit 1 time/s

Example

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

Return Value
```
longLeverage: Leverage of Long Position（If not set as maximum leverage or default leverage）
shortLeverage: Leverage of Short Position
longLeverageEditable: If the long position’s leverage is editable（If there are pending orders or pending limit orders exist, the leverage is not editable）
shortLeverageEditable: If the short position’s leverage is editable（If there are pending orders or pending limit orders exist, the leverage is not editable）
varyRange: Contract Vary Range
maintenanceMarginRate: Maintenance Margin Rate
minLeverage: Minimum Leverage
maxLeverage: Maximum Leverage
defaultLeverage: Default Leverage
``` 

|Parameter|    Type|	Required|	Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verfication, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|contractCode|String|yes|Contract Code|


9. Get /api/v1/set_leverage    Get Contract Leverage, rate limit 1 time/s

Example	

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

Return Value
```
data: Leverage
``` 

|Parameter|    Type|	Required|	Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|contractCode|String|yes|Contract Name|
|direct|String|yes|Long or Short:1 Long,2 Short|
|leverage|String|yes|Leverage|



10. Post /api/v1/liquidation_history    Get liquidation history, rate limit 1 time/s

Example

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
                "contractName": "ETH/USDT Perpetual Swap",
                "side": "2",
                "sideDisplay": "Buy",
                "orderType": "Buy to Close Short",
                "direct": 2,
                "leverage": 20,
                "orderQuantity": "+70,731",
                "orderPrice": "194.43",
                "closePosPNL": "-6545.5086 USDT",
                "fee": "68.7606 USDT",
                "ctime": "2019-10-30 14:19:52",
                "timestamp": 1572416393251656,
                "direction": 1,
                "directionDisplay": "Short",
                "positionMargin": "+6614.2692 USDT",
                "openPrice": "185.17",
                "liquidationPriceReal": "193.50",
                "showDetail": false
            },
        ...
            {
                "orderId": 2,
                "contractCode": "EOSUSDT",
                "contractName": "EOS/USDT Perpetual Swap",
                "side": "1",
                "sideDisplay": "Sell",
                "orderType": "Sell to Close Long",
                "direct": 1,
                "leverage": 20,
                "orderQuantity": "-1,123",
                "orderPrice": "6.4883",
                "closePosPNL": "-383.6963 USDT",
                "fee": "3.6432 USDT",
                "ctime": "2019-06-27 04:44:47",
                "timestamp": 1561581887104635,
                "direction": 2,
                "directionDisplay": "Long",
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

Return Value	

```
orderId: Order ID
contractCode: Contract Code
contractName: Contract Name
side: Buy or Sell, 1:Sell 2 Buy
sideDisplay: Buy or Sell
orderType: Buy to Open Long, Sell to Open Short, Buy to Close Short, Sell to Close Long
direct: Long or Short: 1.Long 2.Short
leverage: Leverage（2 Decimals）
ftime: Finish Time
orderQuantity: Order Quantity（contracts）
orderPrice: Order Price
closePosPNL: Realized P/L, 4 Decimals
fee: Trading Fee(4 Decimals)
ctime: Creation Time
timestamp: Time Stamp(microsecond)
direction: 1.Short 2.Long
positionMargin: Position Margin (4Decimals)
openPrice: Average Open Price
liquidationPriceReal: Liquidation Price
``` 

|Parameter|Type|Required|Description|
|:-----|:-----|:-----|:-----|
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|contractCodeList|[]|yes|Contract List, leave it black to search all contracts|
|side|int|yes|0:no limit 1 for sell, 2 for buy.|
|page|int|yes|Page No., Default 1|



11. Get /api/v1/account_records    Get deposit and withdraw history, rate limit 1 time/s

Example
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
                "orderTypeD": "Onchian Withdraw",
                "amount": "-1900.00000000 USDT",
                "rmbAmount": "0.00 CNY",
                "bankName": "",
                "addr": "mvYW73ZTo8F7aksBrWad3XvqViqDE3saaP",
                "orderStatus": 1,
                "orderStatusD": "Complete",
                "createdTime": "2019-10-31 15:52:11"
            },
            ...
            {
	    	    "id": 58421,
                "asset": "USDT",
                "orderType": 2,
                "orderTypeD": "OTC Withdraw",
                "amount": "-100.00000000 USDT",
                "rmbAmount": "704.00 CNY",
                "bankName": "Postal Savings Bank of China",
                "addr": "",
                "orderStatus": 1,
                "orderStatusD": "Complete",
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
Return Value	

```
asset: Asset Name
orderType: 1 OTC Deposit,2 OTC Withdraw,3 Onchain Deposit,4 Onchain Withdraw,5 Internal Transfer-Deposit,6 Internal Transfer-Withdraw, 7 Manual Deposit, 8 Manual Withdraw,9 Quick Deposit,10 Quick Withdraw
orderTypeD: Order Type Description
amount: Asset Amount
rmbAmount: RMB Amount
bankName: Bank Name
addr: Wallet Address
orderStatus: 0 Pending, 1 Complete, 2 Failed
orderStatusD: Order Status Description
createdTime: Creation Time
``` 

|Parameter|	Type|	Required|	Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|page|int|no|Page No., Default 1|
|limit|int|no|the number of returned information on each page, Default 20|





The authentication details include the following four aspects:
- Generate API Key
    - Your API Key and API Secret must be generated through Hopex. API Key and API Secret will be randomly generated and provided by Hopex.
- Initiate a request
    - All private REST request headers must include Authorization, Date and Digest.
- Signature
    - Authorization header has following format: -H 'Authorization: hmac apikey = "...", algorithm = "hmac-sha256", headers = "date request-line digest", signature = "..."'. apikey is Your API Key, signature is method + request-line + "HTTP / 1.1" + digest, encrypt with HMAC SHA256 method by secret, and obtain through BASE64 encoded output. request-line is the request path. like /api /v1/userinfo, digest is the digest of the request body.
- GMT time
    - The Date request header must be the standard Greenwich Mean Time format corresponding to the time of the request, such as Fri, 19 Apr 2019 03:15:20 GMT, if the request is sent more than 1 minute away from the server time, the system will consider the request to be expired and refused.

Here is a sample program of generating Authorization, Date and Digest Request Header:
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


