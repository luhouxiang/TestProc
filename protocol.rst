=========
智能逆回购
=========

********
1、智能逆回购定制
********
* 请求方式: post
* URL：http://172.16.239.239:21800/api/autotrade/order
* 参数说明

===============  ================  ==========  =====  ===========================================
     标识              名称          类型       必要         备注
===============  ================  ==========  =====  ===========================================
identifier       用户标识符         string      Y          客户号
appType          手机类型           string      Y    
orderChannel     渠道号             string      Y
orderType        订阅类型           string      Y          1:订阅，2：暂停，3：修改，4：恢复，0: 终止
orderId          订阅Id             string      Y          订阅Id,在查询时返回，用于暂停，恢复，终止
serviceId        服务类型           string      Y          8002：表示国债逆回购
yybdm            营业部代码         string      Y          
lhxx             留痕信息           string      Y          auto.***
zjzh             资金账号           stirng      Y
order            订阅数据           数组          
---------------  ----------------  ----------  -----  -------------------------------------------
gddm             股东代码           string
marketId         市场代码           string
orderStartTime   开始时段           string                 14:30:00
orderEndTime     结束时段           string                 22:00:00
stockCode        代码               string                
stockName        名称               string
discription      中文说明           string
price            利率               string                 0.03
orderAmount      金额               string
endDate          截止日期           string                 2017-11-14
===============  ================  ==========  =====  ===========================================

* 定制,修改（orderType:1）
::
 
 http://172.16.239.239:21800/api/autotrade/reverse_order
 {
   "identifier" : "101500000010",
   "appType" : "1",
   "orderChannel" : "1",
   "orderType" : "1",
   "serviceId" : "8002",
   "yybdm" : "1031",
   "lhxx" : "auto.lhxx",
   "zjzh" : "101500000010",
   "order" : [
      {
         "gddm" : "A231941760",
         "marketId" : "0",
         "orderStartTime" : "14:30:00",
         "orderEndTime" : "22:00:00",
         "stockCode" : "131810",
         "stockName" : "R－001",
         "discription":"一天期",
         "price" : "0.01",
         "orderAmount" : "10000.00",
         "endDate" : "2017-11-30"
      }
   ]
}

* 暂停, 恢复, 终止（orderType:2, 4, 0）
::

 {
  "identifier" : "101500000010",
  "orderType" : "2",
  "serviceId" : "8002",
  "lhxx" : "auto.lhxx",
  "orderId":"26",
  "order" : [
    {
       "stockCode" : "131810",
       "marketId" : "0"
    }
  ]
 }

* 数据返回
::

 {
     "code": "-1",
     "message": "入参错误,请重新操作"
 }
 {
     "code": "0",
     "orderId": "1",
     "message": "终止成功"
 }
 {
    "code": "0",
    "orderId": "1",
    "message": "暂停成功"
 }
{
  "message": "预约成功",
  "orderId": "1",
  "code": "0"
}
 {
    "code": "0",
    "orderId": "1",
    "message": "预约成功"
 }

********
2、智能逆回购查询
********
* 请求方式: post
* URL：http://172.16.239.239:21800/api/autotrade/order
* 参数说明

===============  ================  ==========  =====  ===========================================
     标识              名称          类型       必要         备注
===============  ================  ==========  =====  ===========================================
identifier       用户标识符         string      Y      客户号
serviceId        服务类型           string      Y      8002：表示国债逆回购
type             查询类型           string      Y      0：运行结果查询，1: 订阅列表查询，2: 订阅详情查询
order            订阅数据           数组          
---------------  ----------------  ----------  -----  -------------------------------------------
marketId         市场代码           string
stockCode        代码              string 
stockExplain     股票简短解释       string
buyCount         买入次数           string
buyCount         买入成功次数       string
buyOkRate        买入成功率         string
orderAmount      委托钱数           string
price            价格，利率         string
status           状态              string              1：正常状态，0：停止，2：暂停
runDay           运行天数           string
createTime       创建时间           string
endDate          截止日期           string             "2017-03-05"
orderStartTime   开始时段           string             "14:30:00"
orderEndTime     结束时段           string             "15:00:00"
===============  ================  ==========  =====  ===========================================


* 运行结果查询（type: 0）
::
 
 http://172.16.239.239:21800/api/autotrade/reverse_query
 {
     "identifier" : "101500000010",
     "serviceId" : "8002",
     "type" : "0",
     "marketId": "0",
     "stockCode": "131810",
     "page": "1",
     "count":"20"
 }


返回::

 {
     "code": "0",
     "message": "ok",
     "order": [
         {
             "marketId": "0",
             "message": "数据过期,未进行申购",
             "orderAmount": "10000.00",
             "sndTime": "2017-11-15",
             "state": "-3",
             "stockCode": "131810"
         },
         {
             "marketId": "0",
             "message": "委托成功,合同号:[1]",
             "orderAmount": "10000.00",
             "sndTime": "2017-11-15",
             "state": "2",
             "stockCode": "131810"
         }
     ],
     "orderId": "26"
 }


* 订阅列表查询（type: 1）
::
 
 http://172.16.239.239:21800/api/autotrade/reverse_query
 {
  "identifier" : "101500000010",
  "serviceId" : "8002",
  "type" : "1"
 }


返回::

 {
    "code": "0",
    "message": "ok",
    "order": [
        {
            "buyCount": "7",
            "buyOkCount": "2",
            "buyOkRate": "0.286",
            "createTime": "2017-11-16",
            "endDate": "2017-11-30",
            "orderAmount": "10000.00",
            "orderEndTime": "15:00:00",
            "orderStartTime": "14:30:00",
            "price": "0.01",
            "runDay": "1",
            "status": "1",
            "stockCode": "131811",
            "marketId":"0",
            "stockExplain": "一天期",
            "stockName": "R－002"
        },
        {
            "buyCount": "5",
            "buyOkCount": "3",
            "buyOkRate": "0.600",
            "createTime": "2017-11-16",
            "endDate": "2017-11-30",
            "orderAmount": "10000.00",
            "orderEndTime": "22:00:00",
            "orderStartTime": "14:30:00",
            "price": "0.01",
            "runDay": "1",
            "status": "1",
            "stockCode": "131810",
            "stockExplain": "一天期",
            "stockName": "R－001"
        }
    ],
    "orderId": "26"
 }
 
 * 订阅详请查询（type: 2）
::
 
 http://172.16.239.239:21800/api/autotrade/reverse_query
 {
  "identifier" : "101500000010",
  "serviceId" : "8002",
  "type" : "1",
  "marketId": "0",
  "stockCode": "131810"
 }


返回::

 {
    "code": "0",
    "message": "ok",
    "order": [
        {
            "buyCount": "7",
            "buyOkCount": "2",
            "buyOkRate": "0.286",
            "createTime": "2017-11-16",
            "endDate": "2017-11-30",
            "orderAmount": "10000.00",
            "orderEndTime": "15:00:00",
            "orderStartTime": "14:30:00",
            "price": "0.01",
            "runDay": "1",
            "status": "1",
            "stockCode": "131811",
            "marketId":"0",
            "stockExplain": "一天期",
            "stockName": "R－002"
        }
    ],
    "orderId": "26"
 }







