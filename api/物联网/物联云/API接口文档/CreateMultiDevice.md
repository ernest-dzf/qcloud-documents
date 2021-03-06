### 1. 接口描述
本接口 (CreateMultiDevice) 用于新建多个物联云设备。

接口请求域名：`iothub.api.qcloud.com`

* 同一开发商账户下设备名称需保持唯一
* 创建设备时，需要与一个产品绑定，这样便于管理
* 设备创建后，会自动创建关联的证书，同时和产品默认的权限进行绑定

### 2. 输入参数

以下请求参数列表仅列出了接口请求参数，其它参数见 [公共请求参数](https://cloud.tencent.com/document/api/213/6976) 页面。

| 参数名称 | 必选 | 类型 | 描述 |
|---------|---------|---------|---------|
| productName | 是 | String | 产品名称。命名规则：[a-zA-Z0-9:_-]{1,128}。|
| listDeviceName | 是 | Array of String | 批量创建的设备名数组，单次最多创建100个设备。命名规则：[a-zA-Z0-9:_-]{1,128}。|


### 3. 输出参数

| 参数名称 | 类型 | 描述 |
|---------|---------|---------|
| code | Int | 公共错误码。0 表示成功，其他值表示失败。详见错误码页面的【公共错误码】。|
| message | String | 模块错误信息描述，格式为 "(模块错误码)模块错误信息" 详见本页面的[模块错误信息](#module_error_info)。|
| codeDesc | String | 模块错误码的英文描述 |
| listCreateDeviceInfo | Array of Devices | 批量创建设备的结果。 |

Devices的结构如下
| 参数名称 | 类型 | 描述 |
|---------|---------|---------|
| deviceName | String | 设备的名称。|
| deviceId | String | 设备的腾讯云资源唯一标识。用于 MTQQ 登录和 TLSPSK Hint|
| devicePassword| String | 设备的密码用于 MQTT 登录。后台不保存明文，请妥善保管。|
| deviceKey| String | 设备的对称密钥。用于 TLSPSK 的通讯 |
| deviceCert | String | 设备的证书。用于 TLS 协商。|
| devicePrivateKey | String | 设备证书的私钥。用于 TLS 协商。后台不保存，请妥善保管。|


### 4. 示例
 
输入
<pre>
    POST https://iothub.api.qcloud.com/v2/index.php 
</pre>
```
Body:
{
    "Action": "CreateMultiDevice",
    "productName": "fruit",
    "listDeviceName": [ "apple", "banana" ],
    #公共请求参数
}
```

输出
```
{       
    listCreateDeviceInfo: [
        {
            "deviceId": "144115205259838136",
            "deviceName": "apple",
            "devicePassword":"12345678",
            "deviceKey":"abcdef",
            "deviceCert":"{publickey}",
            "devicePrivateKey":"{privatekey}",
        },
        {
            "deviceId": "144115205259838152",
            "deviceName": "banana",
            "devicePassword":"87654321",
            "deviceKey":"uvwxyz",
            "deviceCert":"{publickey}",
            "devicePrivateKey":"{privatekey}",
        }
    ],
    "message": "",
    "codeDesc": "Success",
    "code": 0
}

```
<span id = "module_error_info"></span>
### 5. 模块错误信息

|模块错误码|描述|
|---------|----|
|6|后台内部错误，请重试。|
|2000|创建的设备超过数量限制。|
|2001|创建的设备名已存在。|
|2002|请求中存在重复的设备名。|



