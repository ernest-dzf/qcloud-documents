## 1. 说明

OCR - 车牌号识别用于识别图像上的车牌号码。

## 2. 接口概述

OCR接口采用http协议，支持指定图片URL和上传本地图片文件两种方式。

限制说明

(1) 每个请求的包体大小限制为??MB。

(2) 所有接口都为POST方法。

(3) 不支持 .gif这类的多帧动图。

协议头部

所有请求都要求含有下表列出的头部信息

| 参数名            | 值                              | 描述                                       |
| -------------- | ------------------------------ | ---------------------------------------- |
| Host           | recognition.image.myqcloud.com | 万象优图服务器域名                                |
| Content-Length | 包体总长度                          | 整个请求包体内容的总长度，单位：字节（Byte）                 |
| Content-Type   | text/json                      | 标准json格式                                 |
| Authorization  | 鉴权签名                           | 用于鉴权的签名，使用多次有效签名。[详情](https://cloud.tencent.com/doc/product/275/3805) |

## 3.接口描述

1) 接口
http://recognition.image.myqcloud.com/ocr/plate

2) 描述
根据用户上传的图像，返回识别出的图片中的车牌号

3) 参数

| 参数名   | 是否必须 | 类型     | 参数说明                                  |
| ----- | ---- | ------ | ------------------------------------- |
| appid | 必须   | String | 项目ID                                  |
| image | 可选   | Binary | 使用base64编码的二进制图片数据                    |
| url   | 可选   | String | 图片的url, image和url只提供一个即可,如果都提供,只使用url |

4) 返回值说明

| 字段         | 类型          | 说明                     |
| ---------- | ----------- | ---------------------- |
| code       | Int         | 返回码                    |
| message    | String      | 返回错误消息                 |
| data.items | Array(Item) | 识别出的所有字段信息，详见下文istem说明 |

items说明

| 字段         | 子字段    | 类型     | 说明        |
| ---------- | ------ | ------ | --------- |
| item       |        | String | 字段名称      |
| itemstring |        | String | 字段内容      |
| itemcoord  | x      | Int    | item框左上角x |
|            | y      | Int    | item框左上角y |
|            | width  | Int    | item框宽度   |
|            | height | Int    | item框高度   |
| itemconf   |        | Float  | 字段识别结果置信度 |

5) 样例

使用url的请求包:

```
POST /ocr/drivinglicence HTTP/1.1
Authorization: FCHXdPTEwMDAwMzc5Jms9QUtJRGVRZDBrRU1yM2J4ZjhRckJi==
Host: recognition.image.myqcloud.com
Content-Length: 80
Content-Type: text/json

{
  "appid":"123456",
  "url":"http://test-123456.image.myqcloud.com/test.jpg"
}
```

使用image的请求包:

```
POST /ocr/drivinglicence HTTP/1.1
Authorization: FCHXdPTEwMDAwMzc5Jms9QUtJRGVRZDBrRU1yM2J4ZjhRckJi==
Host: recognition.image.myqcloud.com
Content-Length: 49
Content-Type: text/json

{
  "appid":"123456",
  "image":"SALDKHKAFLASD"
}
```

回包:

```
HTTP/1.1 200 OK
Connection: keep-alive
Content-Length: 242
Content-Type: text/json

{
"code":0,
"message":"OK",
"items":
    [
        {"item": "车牌", 
          "itemstring": "津NHB321",
         "itemcoord":{"x" : 0, "y" : 1, "width" : 2, "height" : 3}, 
         "itemconf": 0.99
        },
    ],
 "session_id":"xxxxxx"
}
```

 

## 4.  返回码

| 错误码   | 含义                       |
| ----- | ------------------------ |
| 3     | 错误的请求                    |
| 4     | 签名为空                     |
| 5     | 签名串错误                    |
| 6     | 签名中的appid/bucket与操作目标不匹配 |
| 9     | 签名过期                     |
| 10    | appid不存在                 |
| 11    | secretid不存在              |
| 12    | appid和secretid不匹配        |
| 13    | 重放攻击                     |
| 14    | 签名校验失败                   |
| 15    | 操作太频繁，触发频控               |
| 21    | 无效参数                     |
| 23    | 请求包体过大                   |
| 24    | 没有权限                     |
| 25    | 您购买的资源已用完                |
| 107   | 鉴权服务不可用                  |
| 108   | 鉴权服务不可用                  |
| 213   | 内部错误                     |
| -1102 | 图片解码失败                   |
| -1300 | 图片为空                     |
| -1301 | 参数为空                     |
| -1304 | 参数过长                     |

