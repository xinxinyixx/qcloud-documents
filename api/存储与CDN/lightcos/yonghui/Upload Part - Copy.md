## 功能描述

Upload Part - Copy 请求实现将一个文件的分块内容从源路径复制到目标路径。通过指定 x-cos-copy-source 来指定源文件，x-cos-copy-source-range 指定字节范围（允许分块的大小为 5 MB - 5 GB）。

## 请求

请求示例：

```
PUT /{ObjectName}?partNumber=PartNumber&uploadId=UploadId HTTP/1.1
Host: <Bucketname-APPID>.cos.<region>.tce.cloud.yonghui.cn

x-cos-copy-source: string
x-cos-copy-source-range: string
x-cos-copy-source-If-Modified-Since: string
x-cos-copy-source-If-Unmodified-Since: string
x-cos-copy-source-If-Match: string
x-cos-copy-source-If-None-Match: string
x-cos-acl: public-read
x-cos-grant-read: id="qcs::cam::uin/\<OwnerUin>:uin/<SubUin>"
x-cos-grant-write: id="qcs::cam::uin/\<OwnerUin>:uin/<SubUin>"。
x-cos-grant-full-control: id="qcs::cam::uin/\<OwnerUin>:uin/<SubUin>"

Date: GMT Date
Authorization: Auth String
```

> Authorization: Auth String (详细参见 [请求签名](https://cloud.tencent.com/document/product/436/7778) 章节)

### 请求行

```
PUT /{ObjectName} HTTP/1.1
```

该 API 接口接受 `PUT` 请求。

#### 请求参数

| 名称       | 类型   | 必选 | 描述                                                         |
| ---------- | ------ | ---- | ------------------------------------------------------------ |
| partNumber | string | 是   | 分块拷贝的块号                                               |
| uploadId   | string | 是   | 使用上传分块文件，必须先初始化分块上传。在初始化分块上传的响应中，会返回一个唯一的描述符（upload ID），您需要在分块上传请求中携带此 ID |

### 请求头

#### 公共头部

该请求操作的实现使用公共请求头，了解公共请求头详细请参见 [公共请求头部](https://cloud.tencent.com/document/product/436/7728) 章节。

#### 非公共头部

| 名称                                  | 描述                                                         | 类型    | 必选 |
| ------------------------------------- | ------------------------------------------------------------ | ------- | ---- |
| x-cos-copy-source                     | 源文件 URL 路径，可以通过 versionid 子资源指定历史版本       | string  | 是   |
| x-cos-copy-source-range               | 源文件的字节范围，范围值必须使用 bytes=first-last 格式，first 和 last 都是基于 0 开始的偏移量 | string  | 否   |
| x-cos-copy-source-If-Modified-Since   | 当 Object 在指定时间后被修改，则执行操作，否则返回 412。可与 x-cos-copy-source-If-None-Match 一起使用，与其他条件联合使用返回冲突 | string  | 否   |
| x-cos-copy-source-If-Unmodified-Since | 当 Object 在指定时间后未被修改，则执行操作，否则返回 412。可与 x-cos-copy-source-If-Match 一起使用，与其他条件联合使用返回冲突 | string  | 否   |
| x-cos-copy-source-If-Match            | 当 Object 的 Etag 和给定一致时，则执行操作，否则返回 412。可与 x-cos-copy-source-If-Unmodified-Since 一起使用，与其他条件联合使用返回冲突 | string  | 否   |
| x-cos-copy-source-If-None-Match       | 当 Object 的 Etag 和给定不一致时，则执行操作，否则返回 412。可与 x-cos-copy-source-If-Modified-Since 一起使用，与其他条件联合使用返回冲突 | string  | 否   |
| x-cos-acl                             | 定义 Object 的 ACL 属性。有效值：private，public-read-write，public-read；默认值：private | string  | 否   |
| x-cos-grant-read                      | 赋予被授权者读的权限。格式：x-cos-grant-read: id=" ",id=" "； 当需要给子账户授权时，id="qcs::cam::uin/<OwnerUin>:uin/"， 当需要给根账户授权时，id="qcs::cam::uin/<OwnerUin>:uin/<OwnerUin>" | string  | 否   |
| x-cos-grant-write                     | 赋予被授权者读的权限。格式：x-cos-grant-write: id=" ",id=" "； 当需要给子账户授权时，id="qcs::cam::uin/<OwnerUin>:uin/"， 当需要给根账户授权时，id="qcs::cam::uin/<OwnerUin>:uin/<OwnerUin>" | string  | 否   |
| x-cos-grant-full-control              | 赋予被授权者读的权限。格式：x-cos-grant-full-control: id=" ",id=" "； 当需要给子账户授权时，id="qcs::cam::uin/<OwnerUin>:uin/"， 当需要给根账户授权时，id="qcs::cam::uin/<OwnerUin>:uin/<OwnerUin>" | string  | 否   |
| x-cos-copy-source-range               | 源文件的字节范围，范围值必须使用 bytes=first-last 格式，first 和 last 都是基于 0 开始的偏移量。例如 bytes=0-9 表示你希望拷贝源文件的开头 10 个字节的数据 ， 如果不指定，则表示拷贝整个文件。 | Integer | 否   |

### 请求体

该请求请求体为空。

## 响应

### 响应头

#### 公共响应头

该响应使用公共响应头，了解共响应头详细请参见 [公共响应头部](https://cloud.tencent.com/document/product/436/7729) 章节。

### 响应体

拷贝成功，返回响应体。

```
<?xml version="1.0" encoding="UTF-8" ?>
<ETag>string</ETag>
<LastModified>string</LastModified>
```

具体的数据描述如下：

| 节点名称（关键字） | 父节点 | 描述                                                         | 类型   | 必选 |
| ------------------ | ------ | ------------------------------------------------------------ | ------ | ---- |
| ETag               | 无     | 返回文件的 MD5 算法校验值。ETag 的值可以用于检查 Object 的内容是否发生变化 | string | 是   |

## 实际案例

### 请求



### 响应

