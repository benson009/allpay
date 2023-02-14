
### 支付

**接口说明**

 该接口为接入商家提供消费购买的功能。

**请求路径**

 `/api/v5/createorder`

**请求参数**

 凡是上送参数都需参与签名。


 ***通用参数：***

| 参数           | 类型         | 必填 | 描述                                                    |
| -------------  | ----- | --- | ------------------------------------------------------------ |
| paymentBrand   | String|是 | 支付品牌，参考[交易场景和支持品牌](#交易场景和支持品牌)|
| tradeFrom      | String|是 | 交易场景，参考[交易场景和支持品牌](#交易场景和支持品牌)|
| orderNum       | String|是 | 订单号：商户自行定义，需保证同一商户号下订单号不能重复|
| orderAmount    | String|是 | 订单金额：如 100 元，表示为 100 或 100.00 |
| orderCurrency  | String|是 | 订单币种：ISO标准。如：人民币填写“CNY”，美元填写"USD" |
| frontURL       | String|是 | 支付完成后前端跳转到该URL。GET请求，参数详见[支付结果通知(回调)](#支付结果通知) |
| backURL        | String|是 | 支付结果异步通知到该URL。支付成功后，GoAllPay 会以 POST JSON 方式调用 backURL 通知支付结果（详见[支付结果通知(回调)](#支付结果通知)）。商户在接收到通知后，需响应字符串“OK”。<br>如果没有收到商户响应“OK”，GoAllPay将会过一段时间后重新推送，时间间隔为[15, 15, 15, 30, 180, 1800, 3600, 7200, 14400]，单位为秒。 |
| merID          | String|是 | 商户 ID，由 GoAllPay 分配 |
| goodsInfo      | String|是 | 商品信息。注意不要包含特殊符号，如 "#"，"&"，"+" 等 |
| detailInfo     | String|是 | 商品明细。<br>格式：[{"goods_name":"iPhone X","quantity":"2"},{"goods_name":"iPhone 8","quantity":"4"}]，需Base64编码后上送。注意goods_name不要包含特殊符号，如 "#"，"&"，"+" 等 |
| transTime      | String|是 | 交易时间，格式："yyyyMMddHHmmss" |
| merReserve     | String|否 | 预留内容，商户自定义。注意不要包含特殊符号，如 "#"，"&"，"+" 等 |
| osType         | String|是 | 用户操作系统类型。<br>"IOS"，"ANDROID"，"HARMONYOS"，"WINDOWS"，"MAC"，"OTHER" 选其中一个上送 |
| osVersion      | String|否 | 用户操作系统版本。示例："10.0.19043" |
| userIP         | String|是 | 用户IP |
| userID         | String|是 | 用户在商家的唯一ID |
| logisticsStreet| String|是 | 收货地址。若为虚拟物品，填写用户邮箱地址 |
| openid         | String|否 | 仅适用于微信小程序支付场景 |
| signType       | String|是 | SHA256 |
| signature      | String|是 | 签名 |

**响应参数**

| 参数       | 类型   | 必填 | 描述                                                         |
| --------- | -------| ---- | ------------------------------------------------------------ |
| respCode  | String | 是 | 应答码，00表示请求成功 |
| respMsg   | String | 是 | 应答信息 |
| merID     | String | 否 | 商户ID |
| orderNum  | String | 否 | 订单号 |
| transID   | String | 否 | GoAllPay流水号 |
| parameter | Object | 否 | 支付相关参数。RespCode为00且非后台支付模式时返｜




**响应参数`parameter`说明:**

(1) H5/WEB/JSAPI交易场景(tradeFrom=`H5`,`WEB`,`JSAPI`)

| 参数     | 类型   | 必填 | 描述                            |
| -------- | ------ | ---- | ------------------------------- |
| payUrl | String | 否    | 支付URL |

(2) APP交易场景(tradeFrom=`APP`)


| 参数     | 类型   | 必填 | 描述                                                      |
| -------- | ------ | ---- | --------------------------------------------------------- |
| tn       | String | 否    | 交易流水号，作为调起 sdk 支付的参数 |

获取到tn后，根据[APP对接文档](AllPay_Integration_Specification_CH.md#6-app模式对接文档)调用SDK进行支付。


(3) APPLET交易场景(tradeFrom=`APPLET`)


| 参数       | 类型   | 必填 | 描述                                                   |
| ---------- | ------ | ---- | ------------------------------------------------------ |
| sdk_params | Object | 否    | 小程序支付所需参数 |



(4) QRCODE交易场景(tradeFrom=`QRCODE`)

| 参数     | 类型   | 必填 | 描述                            |
| -------- | ------ | ---- | ------------------------------- |
| code_url | String | 否    | 二维码字符串 |


(5) QUICK交易场景(tradeFrom=`QUICK`)

| 参数     | 类型   | 必填 | 描述                            |
| -------- | ------ | ---- | ------------------------------- |
| payUrl | String | 否    | 收银台URL |


### 交易场景和支持品牌 <!-- {docsify-ignore} -->

tradeFrom|支持paymentBrand列表|交易示例
---|---|---
H5|unionpay,alipay_cn,alipay_hk,...|[报文示例](https://local.allpayx.com/#/api_examples?id=h5-payment)
WEB|alipay_cn,alipay_hk,...|[报文示例](https://local.allpayx.com/#/api_examples?id=web-payment)
APP|unionpay,wechat_pay,alipay_cn,alipay_hk,...|[报文示例](https://local.allpayx.com/#/api_examples?id=app-payment)
JSAPI|wechat_pay,alipay_cn|[报文示例](https://local.allpayx.com/#/api_examples?id=jsapi-payment)
APPLET|wechat_pay,alipay_cn|[报文示例](https://local.allpayx.com/#/api_examples?id=applet-payment)
QRCODE|unionpay,wechat_pay,alipay_cn,....|[报文示例](https://local.allpayx.com/#/api_examples?id=qrcode-payment)
QUICK|wechat_pay|[报文示例](https://local.allpayx.com/#/api_examples?id=h5-payment)



### 支付(PPRO)

**请求参数**

| 参数           | 类型         | 必填 | 描述                                                    |
| -------------  | ----- | --- | ------------------------------------------------------------ |
| paymentBrand   | String|是 | 支付品牌，参考[交易场景和支持品牌](#PPRO交易场景和支持品牌)|
| tradeFrom      | String|是 | 交易场景，参考[交易场景和支持品牌](#PPRO交易场景和支持品牌)|
| orderNum       | String|是 | 订单号：商户自行定义，需保证同一商户号下订单号不能重复|
| orderAmount    | String|是 | 订单金额：如 100 元，表示为 100 或 100.00 |
| orderCurrency  | String|是 | 订单币种：ISO标准。如：人民币填写“CNY”，美元填写"USD" |
| frontURL       | String|是 | 支付完成后前端跳转到该URL。GET请求，参数详见[支付结果通知(回调)](#支付结果通知) |
| backURL        | String|是 | 支付结果异步通知到该URL。支付成功后，GoAllPay 会以 POST JSON 方式调用 backURL 通知支付结果（详见[支付结果通知(回调)](#支付结果通知)）。商户在接收到通知后，需响应字符串“OK”。<br>如果没有收到商户响应“OK”，GoAllPay将会过一段时间后重新推送，时间间隔为[15, 15, 15, 30, 180, 1800, 3600, 7200, 14400]，单位为秒。 |
| merID          | String|是 | 商户 ID，由 GoAllPay 分配 |
| goodsInfo      | String|是 | 商品信息。注意不要包含特殊符号，如 "#"，"&"，"+" 等 |
| detailInfo     | String|是 | 商品明细。<br>格式：[{"goods_name":"iPhone X","quantity":"2"},{"goods_name":"iPhone 8","quantity":"4"}]，需Base64编码后上送。注意goods_name不要包含特殊符号，如 "#"，"&"，"+" 等 |
| transTime      | String|是 | 交易时间，格式："yyyyMMddHHmmss" |
| merReserve     | String|否 | 预留内容，商户自定义。注意不要包含特殊符号，如 "#"，"&"，"+" 等 |
| osType         | String|是 | 用户操作系统类型。<br>"IOS"，"ANDROID"，"HARMONYOS"，"WINDOWS"，"MAC"，"OTHER" 选其中一个上送 |
| osVersion      | String|否 | 用户操作系统版本。示例："10.0.19043" |
| userIP         | String|是 | 用户IP |
| userID         | String|是 | 用户在商家的唯一ID |
| logisticsStreet| String|是 | 收货地址。若为虚拟物品，填写用户邮箱地址 |
| signType       | String|是 | SHA256 |
| signature      | String|是 | 签名 |
| countrycode      | String|是 | The 2-letter ISO country code of the country in which the payment instrument is issued/operated (e.g. DE). |
| accountholdername | String|是 | The account holder - minimum of 3 characters, up to 100 characters. |
| consumerref | String|否 | Unique reference identifying the consumer, has to satisfy the regular expression [A-Za-z0-9.%,&/+*$-]{1,20}  <br/>paymentBrand为 aura, baloto, bancodobrasil, boleto, hipercard, itau, webpay,时为必填 |
| nationalid | String|否 | 签名Consumer’s national id (up to 30 characters)  <br/>paymentBrand为 aura, baloto, bancodobrasil, boleto, hipercard, itau, webpay,santander, pagofacil, rapipago时为必填 |
| email | String|否 | RFC compliant email address of the account holder  <br/>paymentBrand为 aura, baloto, bancodobrasil, boleto, hipercard, itau, ,webpay,dragonpay, enets,payu, p24, safetypay,sepaddmodela,santander, pagofacil, rapipago,konbini, payeasy,doku, ov时为必填 |
| address | String|否 | Consumer's address  <br/>paymentBrand为 aura, baloto, bancodobrasil, boleto, hipercard, itau, webpay,时为必填 |
| zipcode | String|否 | Consumer's zip/postal code  <br/>paymentBrand为 aura, baloto, bancodobrasil, boleto, hipercard, itau, webpay,时为必填 |
| phone | String|否 | Valid international phone number of the account holder  <br/>paymentBrand为 dragonpay, enets,konbini, payeasy时为必填 |
| mobilephone | String|否 | Valid international Russian mobile phone number identifying the QIWI destination account to pay out to (excluding plus sign or any other international prefixes, including a leading 7 for Russia, 11 digits in total, e.g. 71234567890).  <br/>paymentBrand为 qiwi时为必填 |
| siteid | String|否 | Unique site identifier, forwarded to qiwi. Required for clients serving multiple points of sale.  <br/>  <br/>paymentBrand为 qiwi时为必填 |
| iban | String |否 | Valid IBAN  <br/>  <br/>paymentBrand为 sepaddmodela时为必填 |
| iteminfo | String |否 | Information about the item(s) being purchased, displayed on the payment page (up to 22 characters)  <br/>  <br/>paymentBrand为 konbini, payeasy时为必填 |
| orderitems | String |否 | The list of items being purchased formatted as a JSON array. Allowed characters: alphanumeric, blank space, a hyphen, underscore, and dot.  <br/>paymentBrand为 doku, ovo时为必填:  <br/>  <br/>Each item contains the following fields:  <br/>  • name: The name of the item. No commas  <br/>  • price: The price of the item in major units. The cents must be in 00.  <br/>  • quantity: The number of items bought. Integer.  <br/>  • totalcost: The price multiplied by the quantity, in major units. The cents must be in 00.  <br/>  <br/>Example:  <br/>[{\"name\":\"table\",\"price\":\"1000.00\",\"quantity\":2,\"totalcost\":\"2000.00\"},{\"name\":\"Laptop\",\"price\":\"15000.00\",\"quantity\":2,\"totalcost\":\"30000.00\"}]  <br/>The total transaction amount is 32000.00<br /><br /><br />paymentBrand为 klarna时为必填:  <br/>Example:  <br/>[{\"name\":\"apple\",\"quantity\":1,\"tax_rate\":1000,\"total_amount\":100,\"total_discount_amount\":20,\"total_tax_amount\":9,\"unit_price\":120}]  <br/>  <br/>Field description:  <br/>  • name: String. Descriptive name of the order line item.  <br/>  • quantity: Integer. Quantity of the order line item. Must be a non-negative number.  <br/>  • tax_rate: Integer. Tax rate of the order line. Non-negative value. The percentage value is represented with two implicit decimals. I.e 1000 = 10%.  <br/>  • total_amount: Integer. Total amount of the order line. Must be defined as non-negative minor units. Includes tax and discount. Eg: 2500=25 euros. Value = (quantity _ unit_price) - total_discount_amount. (max value: 100000000)  <br/>  • total_discount_amount: Integer. Non-negative minor units. Includes tax. Eg: 500=5 euros.  <br/>  • total_tax_amount: Integer. Total tax amount of the order line. Must be within ±1 of total_amount - total_amount _ 10000 / (10000 + tax_rate). Negative when type is discount.  <br/>  • unit_price: Integer. Price for a single unit of the order line. Non-negative minor units. Includes tax, excludes discount. (max value: 100000000) |
| taxamount | String |否 | The total tax amount of the order in the currency's minor units. This information is essential to provide detailed purchase information to the consumer (e.g., on the invoice).  <br/>  <br/>paymentBrand为 klarna时为必填 |
| paymentmethodcategory | String |否 | The payment method category.  <br/>Options:  <br/>  • DIRECT_DEBIT  <br/>  • DIRECT_BANK_TRANSFER  <br/>  • PAY_NOW  <br/>  • PAY_LATER  <br/>  • PAY_OVER_TIME  <br/>  <br/>paymentBrand为 klarna时为选填 |
| billingaddress | String |否 | The billing address. It is passed as a JSON string.  <br/>  <br/>Example:  <br/>{\"city\":\"Munich\",\"country\":\"DE\",\"email\":\"test.customer@mpay.int\",\"phone\":\"07792555555\",\"family_name\":\"Doe\",\"given_name\":\"John Doe\",\"postal_code\":\"EH12 3AB\",\"street_address\":\"Ocean Point\"}  <br/>  <br/>paymentBrand为 klarna时为必填 |



**响应参数**

| 参数        | 类型          | 必填 | 描述                            |
| --------- | ----------- | -- | ----------------------------- |
| RespCode  | String(2)   | 是  | 应答码，00表示请求成功                  |
| RespMsg   | String(256) | 是  | 应答信息                          |
| merID     | String(15)  | 否  | 商户ID                          |
| orderNum  | String(60)  | 否  | 订单号                           |
| transID   | String(32)  | 否  | GoAllPay流水号                   |
| parameter | Object      | 否  | 支付相关参数。RespCode为00且非后台支付模式时返回 |

响应参数parameter详情
**响应参数`parameter`说明:**

(1) H5交易场景(tradeFrom=`H5`)

| 参数     | 类型   | 必填 | 描述                            |
| -------- | ------ | ---- | ------------------------------- |
| payUrl | String | 否    | 支付URL |


### PPRO交易场景和支持品牌 <!-- {docsify-ignore} -->

tradeFrom|支持paymentBrand列表|交易示例
---|---|---
H5|directpay, boleto, webpay, trustpay, skrill, sepaddmodela, safetypay, qiwi, p24, poli, paysafecard, mybank, itau, ideal, hipercard, giropay, eps, enets, dragonpay, bancodobrasil, baloto, aura, payu, bcmc, santander, pagofacil, rapipago, konbini, payeasy, doku, ovo, grabpayotp, klarna|[报文示例](https://local.allpayx.com/#/api_examples?id=create-orderppro)


### 退款

**1. 接口功能说明**

  该接口为接入商家提供交易退款的功能。

 （1）接口补充说明
 * 该接口在消费交易成功后发起，要求指定原订单号
 * 外卡交易当日不能做退款，可以做撤销，只能隔日退款
 * 正常情况下，商户当日退款金额，不得大于当日成功交易额

 （2）接口交易流程

  ![](https://allpayfile-hd2.oss-cn-shanghai.aliyuncs.com/git/b2c/15665469966231.jpg)

**2. 请求路径**

 `/api/v5/refund`

**3. 请求参数** 

| 参数          | 类型       | 必填 | 描述                                                         |
| ------------- | ---------- | ---- | ------------------------------------------------------------ |
| orderNum      | String| 是| 退款订单号:商户自行定义，需保证同一商户号下退款订单号不能重复 |
| origOrderNum  | String| 是| 原支付订单号                                                 |
| returnAmount  | String| 是| 退款金额：如 100 元，表示为 100 或 100.00                    |
| orderCurrency | String| 是| 订单币种：ISO标准 如：人民币填写“CNY”,美元填写"USD"          |
| merID         | String| 是| 商户 ID，由 GoAllPay 分配                                       |
| paymentBrand  | String| 是| 支付品牌|
| transTime     | String| 是| 交易时间，格式："yyyyMMddHHmmss"                             |
| backURL       | String| 是| 退款结果异步通知到该URL。退款成功后，GoAllPay 会以 POST JSON 方式调用 backURL 通知退款结果（详见退款结果通知-回调）。商户在接收到通知后，需响应字符串“OK”。 <br>如果没有收到商户响应“OK”，GoAllPay将会过一段时间后重新推送，时间间隔为[15, 15, 15, 30, 180, 1800, 3600, 7200, 14400]，单位为秒。|
| signType      | String| 是| SHA256                                                  |
| signature     | String| 是| 签名                                        |


**4. 响应参数** 

| 参数      | 类型       | 必填 | 描述                                           |
| --------- | ---------- | ---- | ---------------------------------------------- |
| transType    | String| 是| “REFD”                                 |
| orderNum     | String| 是| 退款订单号                                     |
| transID      | String| 是| GoAllPay流水号         |
| merID        | String| 是| 商户 ID |
| paymentBrand | String| 是| 支付品牌|
| respCode     | String| 是| 应答码 00-成功，01-失败。详情见本文档第5章应答码 |
| respMsg      | String| 是| 应答消息                       |
| transTime    | String| 是| 交易时间，格式："yyyyMMddHHmmss"                 |
| gwTime       | String| 是| yyyyMMddHHmmss，为 GW 时间，目前为本地交易时间 |
| returnAmount | String| 是| 退款金额 |
| orderCurrency| String| 是| 订单币种 |
| signType     | String| 是| SHA256                                   |
| signature    | String| 是| 签名                          |



### 查询

**1. 接口功能说明**

  该接口为接入商家提供交易退款的功能。

 （1）接口补充说明
 * 该接口在消费交易成功后发起，要求指定原订单号
 * 外卡交易当日不能做退款，可以做撤销，只能隔日退款
 * 正常情况下，商户当日退款金额，不得大于当日成功交易额

 （2）接口交易流程

  ![](https://allpayfile-hd2.oss-cn-shanghai.aliyuncs.com/git/b2c/15665469966231.jpg)

**2. 请求路径**

 `/api/v5/refund`

**3. 请求参数** 

| 参数          | 类型       | 必填 | 描述                                                         |
| ------------- | ---------- | ---- | ------------------------------------------------------------ |
| orderNum      | String| 是| 退款订单号:商户自行定义，需保证同一商户号下退款订单号不能重复 |
| origOrderNum  | String| 是| 原支付订单号                                                 |
| returnAmount  | String| 是| 退款金额：如 100 元，表示为 100 或 100.00                    |
| orderCurrency | String| 是| 订单币种：ISO标准 如：人民币填写“CNY”,美元填写"USD"          |
| merID         | String| 是| 商户 ID，由 GoAllPay 分配                                       |
| paymentBrand  | String| 是| 支付品牌|
| transTime     | String| 是| 交易时间，格式："yyyyMMddHHmmss"                             |
| backURL       | String| 是| 退款结果异步通知到该URL。退款成功后，GoAllPay 会以 POST JSON 方式调用 backURL 通知退款结果（详见退款结果通知-回调）。商户在接收到通知后，需响应字符串“OK”。 <br>如果没有收到商户响应“OK”，GoAllPay将会过一段时间后重新推送，时间间隔为[15, 15, 15, 30, 180, 1800, 3600, 7200, 14400]，单位为秒。|
| signType      | String| 是| SHA256                                                  |
| signature     | String| 是| 签名                                        |




**4. 响应参数** 

| 参数      | 类型       | 必填 | 描述                                           |
| --------- | ---------- | ---- | ---------------------------------------------- |
| transType    | String| 是| “REFD”                                 |
| orderNum     | String| 是| 退款订单号                                     |
| transID      | String| 是| GoAllPay流水号         |
| merID        | String| 是| 商户 ID |
| paymentBrand | String| 是| 支付品牌|
| respCode     | String| 是| 应答码 00-成功，01-失败。详情见本文档第5章应答码 |
| respMsg      | String| 是| 应答消息                       |
| transTime    | String| 是| 交易时间，格式："yyyyMMddHHmmss"                 |
| gwTime       | String| 是| yyyyMMddHHmmss，为 GW 时间，目前为本地交易时间 |
| returnAmount | String| 是| 退款金额 |
| orderCurrency| String| 是| 订单币种 |
| signType     | String| 是| SHA256                                   |
| signature    | String| 是| 签名                          |





### 支付结果通知

**1. 接口说明**

 商家调用支付接口、收银台支付接口、后台支付接口、预授权接口，结果将通过该接口返回到交易的 backURL 位置。

**2. 请求路径**

 商户支付接口中的 backURL 参数指定的路径。

**3. 请求参数**

| 参数          | 类型         | 必填 | 描述                                                         |
| ------------- | ------------ | ---- | -----|
| transType     | String|是| `PURC`                                 |
| orderNum      | String|是| 订单号 |
| orderAmount   | String|是| 订单金额 |
| orderCurrency | String|是| 订单币种 |
| merID         | String|是| 商户 ID |
| respCode      | String|是| 应答码 00-成功，01-失败。详情见本文档第5章应答码 |
| respMsg       | String|是| 应答消息                                                     |
| transID       | String|是| GoAllPay流水号                       |
| gWTime        | String|是| yyyyMMddHHmmss，为 GW 时间                                   |
| transTime     | String|是| 交易时间，格式："yyyyMMddHHmmss"                             |
| merReserve    | String|否| 商户预留内容 |
| paymentBrand  | String|否| 支付品牌，交易成功时（RespCode=00），为必要的信息|
| signType      | String|是| SHA256 |
| signature     | String|是| 签名|

**4. 响应参数**

字符串OK



### 退款结果通知

**1. 接口说明**

 退款交易成功后，结果将通过该接口返回到退款交易的 backURL 位置。

**2. 请求路径**

 商户支付接口中的 backURL 参数指定的路径。

**3. 请求参数**

| 参数      | 类型       | 必填 | 描述                                           |
| --------- | ---------- | ---- | ---------------------------------------------- |
| transType    | String| 是| “REFD”                                 |
| orderNum     | String| 是| 退款订单号                                     |
| transID      | String| 是| GoAllPay流水号         |
| merID        | String| 是| 商户 ID |
| paymentBrand | String| 是| 支付品牌|
| respCode     | String| 是| 应答码 00-成功，01-失败。详情见本文档第5章应答码 |
| respMsg      | String| 是| 应答消息                       |
| transTime    | String| 是| 交易时间，格式："yyyyMMddHHmmss"                 |
| gwTime       | String| 是| yyyyMMddHHmmss，为 GW 时间，目前为本地交易时间 |
| returnAmount | String| 是| 退款金额 |
| orderCurrency| String| 是| 订单币种 |
| signType     | String| 是| SHA256                                   |
| signature    | String| 是| 签名         

**4. 响应参数**

字符串OK







​      

