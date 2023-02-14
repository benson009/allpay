
>V5.3

[TOC]

# PAYMENTS
## CREATE ORDER
### H5 PAYMENT 

>Support H5 payment of UnionPay and Alipay.

**REQUEST**

``` json
curl --request POST \
     --url https://testapi.allpayx.com/api/v5/createorder \
     --header 'accept: application/json' \
     --header 'content-type: application/json' \
     --data '
{
    "orderNum":"20230210095557",
    "orderCurrency":"CNY",
    "frontUrl":"https://test.allpayx.com/demo/result",
    "backUrl":"https://test.allpayx.com/demo/result",
    "merId":"600039259442068",
    "transTime":"20230210095557",
    "signType":"SHA256",
    "osType":"WINDOWS",
    "osVersion":"",
    "orderAmount":"1",
    "goodsInfo":"apple",
    "detailInfo":"W3siZ29vZHNfbmFtZSI6ICJhcHBsZSIsICJxdWFudGl0eSI6ICIyIn1d",
    "merReserve":"",
    "userIp":"116.230.182.159",
    "userId":"user01",
    "logisticsStreet":"zhangjiang",
    "paymentBrand":"unionpay",       
    "tradeFrom":"H5",   
    "signature":"7878cd46d5cf52f2900bc7f209b167cd45a03460ff8ad0fcedbf5feb562d644d"
}
'
```
	paymentBrand:unionpay,alipay_cn,wechat_pay,...

**RESPONSE**
```json
{
    "respCode":"00",
    "respMsg":"success",
    "merId":"600039259442068",
    "orderNum":"20230210095557",
    "transId":"3bIMFhBBNgil5vpF",
    "parameter":{
        "payUrl":"https://testapi.allpayx.com/api/receiveCode?allPayTn=2fb4d428f96d332df2220e3f7856e292"
    }
}

```


### APP PAYMENT 

>Support xx payment of UnionPay and Alipay.

**REQUEST**
``` json

```


**RESPONSE**
```json

```


### WEB PAYMENT 

>Support xx payment of UnionPay and Alipay.

**REQUEST**
``` json

```


**RESPONSE**
```json

```


### JSAPI PAYMENT 

>Support xx payment of UnionPay and Alipay.

**REQUEST**
``` json

```


**RESPONSE**
```json

```



### APPLET PAYMENT 

>Support xx payment of UnionPay and Alipay.

**REQUEST**
``` json

```


**RESPONSE**
```json

```



### QRCODE PAYMENT 

>Support xx payment of UnionPay and Alipay.

**REQUEST**
``` json

```


**RESPONSE**
```json

```


## CREATE ORDER(PPRO)
## CREATE ORDER(EVO)
## CREATE ORDER(FC)
## CHECKOUT


# ORDER QUERY
>Query the status of `payment` transaction.

**REQUEST**
``` json

```


**RESPONSE**
```json

```




# REFUND
> Refund only support successful payment transaction.

**REQUEST**
``` json

```


**RESPONSE**
```json

```

# ASYNC NOTICE 
## PAYMENT RESULT 
>  ....

**REQUEST**
``` json

```


**RESPONSE**
```json

```

## REFUND RESULT 

>  ....

**REQUEST**
``` json

```


**RESPONSE**
```json

```