## 使用限制
PostgreSQL for Serverless 不支持如下操作：
- 创建数据库
- 访问系统库 postgres
- 查看数据库参数
- SET/RESET 语句
- LOAD 语句
- PRESERVE/DELETE ROWS temp tables+
- LISTEN/NOTIFY
- WITH HOLD CURSOR
- PREPARE / DEALLOCATE
- 内测版本限制单用户支持最大 QPS 40000/s
- 当前仅支持通过云 API 进行创建实例 ServerlessDB
- 当前仅开放北京一区、上海二区、广州二区


## 创建实例
目前可通过云 API 创建 PostgreSQL for Serverless（ServerlessDB）实例，接口名：[CreateServerlessDBInstance](https://cloud.tencent.com/document/product/409/42762)。

下表为输入参数：

| 参数名         | 必填项 | 类型   | 介绍                                                        |
| -------------- | ------ | ------ | ----------------------------------------------------------- |
| Zone          | 是     | string | 可用区 ID，当前支持 ap-shanghai-2、ap-beijing-3、ap-guangzhou-2。 |
| DBInstanceName | 是     | string | 数据库实例名，同一个帐号下该值必须唯一。                    |
| DBVersion      | 是     | string | 数据库版本，目前仅支持10.4。                                |
| DBCharset      | 是     | string | PostgreSQL 数据库字符集，目前支持 UTF8、LATIN1 两种。        
| VpcId          | 否     | string | 私有网络 ID，若不指定此项，则为实例分配基础网络 IP 地址。     |
| SubnetId       | 否     | string | 私有网络子网 ID，与私有网络 ID 同时搭配使用。                  |

执行成功后，输出示例如下：
```
{
  "Response": {
    "RequestId": "20304c-6fd7-4427-8e09-2b081e1",
    "DBInstanceId": "postgres-xxxxxxx"  
  }
}
```

其中返回的 DBInstanceId 指实例 ID。

## 开启关闭实例外网
通过 [OpenServerlessDBExtranetAccess](https://cloud.tencent.com/document/product/409/42759) 和 [CloseServerlessDBExtranetAccess](https://cloud.tencent.com/document/product/409/42763) 接口可开启或关闭 PostgreSQL for Serverless 的外网连接功能，用户可通过外网连接访问数据库。
>?开启外网功能后，外部可以连接访问数据库，导致数据库存在安全隐患，请尽量使用私有网络进行实例访问。

## 访问实例
1. 通过 [DescribeServerlessDBInstances](https://cloud.tencent.com/document/product/409/42760) 接口查看所有创建的 PostgreSQL for Serverless 实例信息。获取实例的 IP 地址、端口、数据库用户和初始密码。
```
{
  "Response": {
    "TotalCount": 1,
    "DBInstanceSet": [
      {
        "DBInstanceId": "postgres-xxxxxxx",
        "DBInstanceName": "test",
        "DBInstanceStatus": "running",#数据库实例状态
        "Region": "ap-shanghai",
        "Zone": "ap-shanghai-2",
        "ProjectId": 0,
        "VpcId": "vpc-test",
        "SubnetId": "subnet-test",
        "DBCharset": "UTF8",
        "DBVersion": "10.4",
        "CreateTime": "2020-03-23 11:43:56",
        "DBInstanceNetInfo": [
          {
            "Address": "",
            "Ip": "10.1.1.2", #IP 为示例，同一私有网络，子网内可访问
            "Port": 5432, #连接端口
            "Status": "opened",
            "NetType": "private"
          },
          {
            "Address": "",
            "Ip": "",
            "Port": 0,
            "Status": "0",
            "NetType": "public"
          }
        ],
        "DBAccountSet": [
          {
            "DBUser": "tencentdb_xxxxxxx",
            "DBPassword": "**************",#数据库密码，请根据实际情况获取，当修改过密码后，此接口返回的密码将不可使用
            "DBConnLimit": 100
          }
        ],
        "DBDatabaseList": [
          "tencentdb_xxxxxxx"
        ]
      }
    ],
    "RequestId": "89583d-cfdd-4db1-bd32-64eb1dbfa"
  }
}
```

2. 通过客户端执行如下命令访问数据库。
```
psql -U 数据库用户 -h IP地址 -p 端口
```
![](https://main.qcloudimg.com/raw/bb9562c380e7c42d3e26e7c9d61a172a.png)


## 销毁实例
通过 [DeleteServerlessDBInstance](https://cloud.tencent.com/document/product/409/42761) 接口可销毁 PostgreSQL for Serverless 实例。
