# ucl360-open-api-docs

### 该文档仅适用调用盎维提供的云端，如果是私有化部署请参考双方约定的规则。

## I. API Hosts
#### 盎维SAAS服务：
- 测试环境: http://scsl.onwingtech.com，开发联调基于测试环境进行
- 生产环境: https://api.unretec.com


## II. 访问授权
- 调用API需要预先获得appId和appSecret
- 请确保appId和appSecret在授权期内，无效授权将直接导致无法访问API
- appSecret请勿在网络中传输

## III. APIs

## IV. Request Params

- 所有的api都使用统一的参数构成，区别在于params参数的内容。

|参数名|	类型|	必填|	描述|
| :-----    | :----   | :-----    | :-----   |
|appid|string|Y|调用API的appId|
|params|string|Y|业务参数(JSON)|
|timestamp|integer|Y|客户端时间戳(13位)，如果客户端和服务端时间(NTP)差异过去会导致调用失败|
|token|string|Y|查看token生成规则|

- 通过Request Body传参

## V. Response Params

API调用以json返回返回，格式如下：

{"result":"success","data":"error or business data"}

|参数名|	类型|	必填|	描述|
| :-----    | :----   | :-----    | :-----   |
|result|string|Y|调用结果：success,fail|
|data|string|Y|调用返回数据|

- 当result==success时，data返回业务数据，不同API返回结果不一样，请查API说明
- 当result==fail时，data返回错误原因,格式：{"code":"error code","msg":"error message"}. 请查看常见错误


## VI. Token生成规则
请按一下步骤生成Token:
- 将参数值按照 appId, params, timestamp 顺序拼接成字符串(Str1)
- 将 Str1 拼接上 appSecret 得到(Str2)
- 将 Str2 MD5 加密(32 位小写)得到 token

## VII. Params加密方法
业务参数使用 DES 加密，加密方法如下
- 将业务数据转成 JSON 串
- DES 加密(ECB+PKCS7) ,密钥为 ChannelSecret 的前 8 位
- Base64 编码

## VIII. 常见错误

|错误代码|	错误描述|	原因|
| :-----    | :----   | :-----|
|1000|服务端异常||
|1001|参数不合法|没有按照API调用约定参数参数|
|1002|Token不合法|appSecret不正确或者未按约规则定生成Token|
|1003|Api调用超期|调用端和服务端时间差异过去，请修正调用端时间|
|1004|API已失效|所调用的API已被废弃|
