# 交易相关的数据结构

## 行情信息

### Tick
```
{
     "asks":
     [
         {"price": 9218.5, "volume": 1.7371},
         ...
     ],
     "bids":
     [
         {"price": 9218.4, "volume": 0.81871728},
         ...
     ],
     "contract": "bitfinex/btc.usd",
     "last": 9219.3,  // 最新成交价
     "time": "2018-05-03T16:16:41.630400+08:00",  // 1token的系统时间 ISO 格式 带时区
     "exchange_time": "2018-05-03T16:16:41.450400+08:00",  // 交易所给的时间 ISO 格式 带时区
     "amount": 16592.4,  //上一个snapshot到这一个snapshot中间的成交额 (CNY)
     "volume": 0.3   // 上一个snapshot到这一个snapshot中间的成交量
}
```

### Tick.v3(alpha)

TODO

### 逐笔(Zhubi)
```
[
     [
         {
             "price": 9218.5,
             "amount": 0.371, // 成交量(按币对， 比如这里是BTC的数量）， 这个地方暂时是amount 之后会改成volume 和tick统一起来
             "bs": "b",
             "contract": "bitfinex/btc.usd",
             "time": "2018-05-03T16:16:41.630400+08:00",  // 1token的系统时间 ISO 格式 带时区
             "exchange_time": "2018-05-03T16:16:41.450400+08:00",  // 交易所给的时间 ISO 格式 带时区
         },
         ...
     ],
]
```

### 逐笔 V3 (Zhubi.v3) (alpha)
```
[
     {
         "p": 9218.5,                          // price
         "a": 0.371,                           // 成交量(按币对， 比如这里是BTC的数量)
         "v": 21203.24,                        // 成交额（按CNY 计价）
         "bs": "b",                            // b for buy, s for sell
         "c": "bitfinex/btc.usd",              // contract
         "t": 1528435679.272815,               // 1token 系统时间 (单位为秒 精确到微秒)
         "et": 1528435679.297,                 // exchange timestamp （单位为秒 精确到秒/毫秒/微妙 分交易所）
     },
     ...
]
```

## 账户信息

OneToken支持**3种**不同的账户类型：现货账户，杠杆交易账户，期货账户


### 现货账户
```$xslt
{
  "balance": 4362.166242423991,                 # 总资产 = 现金 + 虚拟货币市值
  "cash": 0.0,                                  # 现金（根据人民币汇率计算）
  "market_value": 4362.166242423991,            # 货币市值
  "market_value_detail": {                      # 每个币种的市值
    "btc": 0.0,
    "usdt": 0.0,
    "eth": 4362.166242423991
  },
  "position": [{                                # 货币持仓，默认包含btc，usdt或法币
      "total_amount": 1.0,                      # 总数
      "contract": "eth",                        # 币种
      "market_value": 4362.166242423991,        # 市值
      "available": 0.97,                        # 可用数量
      "frozen": 0.03,                           # 冻结数量
      "type": "spot"                            # 类型，spot表示现货持仓
    },
    {
      "total_amount": 0.0,
      "contract": "usdt",
      "market_value": 0.0,
      "available": 0.0,
      "frozen": 0.0,
      "type": "spot",
      "value_cny": 0.0
    },
    {
      "total_amount": 0.0,
      "contract": "btc",
      "market_value": 0.0,
      "available": 0.0,
      "frozen": 0.0,
      "type": "spot"
    }
  ]
}
```

### 杠杆交易账户
```$xslt
{
    "balance": 589943.9724,                 # 总资产 = 现金 + 虚拟货币市值
    "cash": 6198.5392,                      # 现金（根据人民币汇率计算）
    "market_value: 583745.4332,             # 货币市值
    "market_value_detail: {                 # 市值详细情况
        eos: 583745.4332,
        usdt: 0
    },
    "position": [
        {
            "contract": "eos.usdt",         # "<coin>.<base>"
            "market_value": 583745.4332,    # 市值
            "amount_coin": 20071.4762,      # 总数
            "available_coin": 18971.4762,   # 可用数量
            "frozen_coin": 1100.0,          # 冻结数量
            "pl_coin": 0,                   # pl_coin = profit and lose (or interest) of coins
            "loan_coin": 0,                 # loan of coins
            market_value_detail: {
                "eos": 583745.4332,
                "usdt": 0
            },
            "value_cny": 0,                 # CNY value of the contract (if available)
            "type": "margin",               # position type
            "mv_coin": 583745.4332,         # market value of coins
            "amount_base": 979.0929,        # total amount of the base currency
            "mv_base": 0,                   # market value of the base currency, 0 for USDT
            "available_base": 7029.3753,    # amount of the available base currency
            "frozen_base": 4511.19,         # frozen amount of the base currency
            "pl_base": -73.4164,            # pl_base = profit and lose (or interest) of the base currency
            "loan_base": -10448.056,        # loan of the base currency
            "value_cny_base": 6198.5392     # CNY value of the base currency (if available)
        },
        ...
    ]
}
```

### 期货账户
```$xslt
{
  "balance": 4361345.793589303,                 # 总资产 = 现金 + 虚拟货币市值
  "cash": 0.0,                                  # 现金（根据人民币汇率计算）
  "market_value": 8728917.770172266,            # 虚拟货币市值
  "market_value_detail": {                      # 每个币种的市值
    "btc": 8728917.770172266,
    "usd": 0.0
  },
  "position": [{                                # 持仓，默认包含btc，usdt或法币现货
    "total_amount": 74.90222428,
    "contract": "btc",
    "market_value": 4361345.793589303,
    "available": 74.90222428,
    "frozen": 0.0,
    "type": "spot"
  }, {
    "total_amount": 0.0,
    "contract": "usd",
    "market_value": 0.0,
    "available": 0.0,
    "frozen": 0.0,
    "type": "spot",
    "value_cny": 0.0
  }, {
    "total_amount": 6904.0,                     # 总数
    "contract": "btc.usd.q",                    # 合约
    "market_value": 4367571.976582963,          # 市值
    "available": 6904.0,                        # 可用数量
    "frozen": 0.0,                              # 冻结数量
    "pl": 1.64417643,                           # profit and lose
    "market_value_detail": {                    #
      "btc": 4367571.976582963
    },
    "type": "future",                           # 类型，future表示期货持仓
    "total_xtc_amount": 75.00915342001107,      #
    "available_xtc": 75.00915342001107,         #
    "frozen_xtc": 0.0,                          #
    "available_long": 6905.0,                   # 多头仓位
    "available_short": 1.0                      # 空头仓位
  }]
}
```

## 订单信息
```$xslt
{
    "account": "binance/test_account",              # 账户名
    "contract": "binance/ltc.usdt",                 # 合约标识
    "bs": "b",                                      # "b"对应买或"s"对应卖
    "client_oid": "binance/ltc.usdt-xxx123",        # 由用户给定或由OneToken系统生成的订单追踪ID
    "exchange_oid": "binance/ltc.usdt-xxx456",      # 由交易所生成的订单追踪ID
    "status": "part-deal-pending",                  # 订单状态
    "entrust_price": 113,                           # 委托价格
    "entrust_amount": 10,                           # 委托数量
    "entrust_time": "2018-04-03T12:21:13+08:00",    # 下单时间
    "average_dealt_price": 112.1,                   # 平均成交价
    "dealt_amount": 1,                              # 成交数量
    "last_dealed_amount": 0.8,                      # 最近一次成交数量
    "commission": 0.0025,                           # 成交手续费
    "last_update": "2018-04-03T12:22:56+08:00",     # 最近更新时间
    "canceled_time": None,                          # 撤单时间
    "options": {},                                  #
    "comment": "string",                            #
    "tags": {}                                      #
}
```

## 成交记录
```$xslt
{
    "account": "binance/test_account",                  # 账户名
    "contract": "binance/ltc.usdt",                     # 合约标识
    "bs": "b",                                          # "b"对应买或"s"对应卖
    "client_oid": "binance/ltc.usdt-xxx123",            # 由用户给定或由OneToken系统生成的订单追踪ID
    "exchange_oid": "binance/ltc.usdt-xxx456",          # 由交易所生成的订单追踪ID
    "exchange_tid": "binance/ltc.usdt-xxx789",          # 由交易所生成的成交ID
    "dealt_amount": 1,                                  # 成交数量
    "dealt_price": 0,                                   # 成交价格
    "dealt_time": "2018-04-03T12:22:56+08:00",          # 成交时间
    "dealt_type": "maker",                              # 主动成交"taker"、被动成交"maker"
    "commission": 0.0025,                               # 成交手续费
    "commission_currency": "ltc",                       # 手续费币种
    "tags": {}                                          #
}
```