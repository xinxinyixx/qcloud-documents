## 功能描述

OPTIONS Object 接口实现 Object 跨域访问配置的预请求。即在发送跨域请求之前会发送一个 OPTIONS 请求并带上特定的来源域，HTTP 方法和 Header 信息等给 COS，以决定是否可以发送真正的跨域请求。当 CORS 配置不存在时，请求返回 403 Forbidden。可以通过 PUT Bucket cors 接口来开启 Bucket 的 CORS 支持。

## 请求

请求示例：

```
OPTIONS /<ObjectName> HTTP/1.1
Host: <BucketName-APPID>.cos.<region>.tce.cloud.yonghui.cn
Date: GMT Date
Authorization: Auth String
```

> Authorization: Auth String (详细参见 [请求签名](https://cloud.tencent.com/document/product/436/7778) 章节)

### 请求行

```
OPTIONS /{ObjectName} HTTP/1.1
```

该 API 接口接受 `OPTIONS` 请求。

### 请求头

#### 公共头部

该请求操作的实现使用公共请求头，了解公共请求头详细请参见 [公共请求头部](https://cloud.tencent.com/document/product/436/7728) 章节。

#### 非公共头部

| 名称                           | 类型   | 必选 | 描述                         |
| ------------------------------ | ------ | ---- | ---------------------------- |
| Origin                         | string | 是   | 模拟跨域访问的请求来源域名   |
| Access-Control-Request-Method  | string | 是   | 模拟跨域访问的请求 HTTP 方法 |
| Access-Control-Request-Headers | string | 否   | 模拟跨域访问的请求头部       |

### 请求体

该请求请求体为空。

## 响应

### 响应头

#### 公共响应头

该响应使用公共响应头，了解公共响应头详细请参见 [公共响应头部](https://cloud.tencent.com/document/product/436/7729) 章节。

#### 特有响应头

该请求操作的响应头具体数据为：

| 名称                          | 类型   | 描述                                                         |
| ----------------------------- | ------ | ------------------------------------------------------------ |
| Access-Control-Allow-Origin   | string | 模拟跨域访问的请求来源域名，当来源不允许的时候，此 Header 不返回 |
| Access-Control-Allow-Methods  | string | 模拟跨域访问的请求 HTTP 方法，当请求方法不允许的时候，此 Header 不返回 |
| Access-Control-Allow-Headers  | string | 模拟跨域访问的请求头部，当模拟任何请求头部不允许的时候，此 Header 不返回该请求头部 |
| Access-Control-Expose-Headers | string | 模拟跨域访问的请求 HTTP 方法，当请求方法不允许的时候，此 Header 不返回 |
| Access-Control-Max-Age        | string | 设置 OPTIONS 请求得到结果的有效期                            |

### 响应体

该请求响应体为空。

## 实际案例

### 请求



### 响应

