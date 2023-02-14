

## 概述 

GoAllPay 为客户提供统一的API接入，以帮助客户实现快速、安全、集中式的接入支付渠道。



## 交易流程
![image](https://git.allpayx.com/OpenAPI/b2c/raw/master/images/pc_flowsheet.png)

## 签名规则 


1. 将所有传入参数（除 signature 参数外）按照参数名的 ASCII 码从小到大排序后（字典序），使用 URL 键值对的格式（即 key1=value1&key2=value2...）拼接成字符串 string1。

2. 在 string1 最后直接拼接（不需要用“&”连接）双方约定的签名密钥 Key（接入 GoAllPay 时分配），得到 stringSignTemp 字符串，并对 stringSignTemp 进行加密运算（加密算法以参数signType值为准），得到 signature 的值。

3. 签名示例：

>string1: OsType=IOS&OsVersion=&acqID=99020344&backURL=https://testapi.allpayx.com/test&charSet=UTF-8&detailInfo=W3siZ29vZHNfbmFtZSI6ICJhcHBsZSIsICJxdWFudGl0eSI6ICIyIn1d&frontURL=http://example.com&goodsInfo=apple&logisticsStreet=上海市浦东新区xx路xx号xxx室&merID=merchant_id&merReserve=&orderAmount=1&orderCurrency=CNY&orderNum=20220620171733&paymentSchema=UP&signType=SHA256&tradeFrom=H5&transTime=20220620171733&transType=PURC&userID=user01&userIP=114.91.1.243&version=VER000000005


>stringSignTemp: OsType=IOS&OsVersion=&acqID=99020344&backURL=https://testapi.allpayx.com/test&charSet=UTF-8&detailInfo=W3siZ29vZHNfbmFtZSI6ICJhcHBsZSIsICJxdWFudGl0eSI6ICIyIn1d&frontURL=http://example.com&goods+Info=apple&logisticsStreet=上海市浦东新区xx路xx号xxx室&merID=merchant_id&merReserve=&orderAmount=1&orderCurrency=CNY&orderNum=20220620171733&paymentSchema=UP&signType=SHA256&tradeFrom=H5&transTime=20220620171733&transType=PURC&userID=user01&userIP=114.91.1.243&version=VER000000005key

`signature: 40f1e5adebba58cc7822dac0b3a2bfebb6676e245370be027cdfb952695bee29`


## 通用说明

 请求方式：`POST`<br>
 数据格式：`JSON`<br>
 测试主机：`https://testapi.allpayx.com`<br>
 生产主机：`https://api.allpayx.com`