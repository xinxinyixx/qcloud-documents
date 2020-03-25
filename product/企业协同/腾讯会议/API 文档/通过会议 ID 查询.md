## 接口描述
描述：通过会议 ID 查询会议详情。
接口请求域名：
```
https://api.meeting.qq.com/v1/meetings/{meetingId}?userid={userid}&instanceid={instanceid}
```
## 输入参数

| 参数名称 | 必选 | 参数类型 | 参数描述 |
|---------|---------|---------|---------|
| meetingId | 是 | String| 有效的会议 ID。  |
| userid | 是 | String| 调用方用于标示用户的唯一 ID（例如企业用户可以为企业账户英文名、个人用户可以为手机号等）。  |
| instanceid | 是 | Integer| 用户的终端设备类型。  |

## 输出参数
| 参数名称 |参数类型 | 参数描述 |
|---------|---------|---------|
| meeting_number | integer | 会议数量。  |
|meeting_info_list  | Array | 会议列表。  |

会议对象

| 参数名称 |参数类型 | 参数描述 |
|---------|---------|---------|
|subject  |String | 会议主题。  |
|meeting_id   |String| 会议的唯一标示 。  |
|meeting_code    |String| 会议 App 的呼入号码。  |
|password   |String | 会议密码。  |
|hosts   |String 数组 | 会议主持人列表 。  |
|participants  |String数组|邀请的参会者 。|
|start_time  |String | 会议开始时间戳（单位秒）。 |
|end_time  |String | 会议结束时间戳（单位秒）。  |
|settings   |会议媒体参数对象 |会议的配置，可为缺省配置 。|

会议媒体参数对象

| 参数名称 |参数类型 | 参数描述 |
|---------|---------|---------|
|mute_enable_join  |Bool | 加入静音状态。  |
|meeting_info_list  |Bool| 静音自解除允许 。  |

## 示例
#### 输入示例

```
GET https://api.meeting.qq.com/v1/meetings/7567173273889276131?userid=tester1&instanceid=1
```

#### 输出示例

```
{  
  "meeting_number": 1,  
  "meeting_info_list": [    
    {      
      "subject": "tester's meeting",      
      "meeting_id": "7567173273889276131",      
      "meeting_code": "806146667",      
      "password": "1111",      
      "status": "MEETING_STATE_ENDED",      
      "start_time": "1572085800",      
      "end_time": "1572089400",      
      "hosts": [        
        "tester"      
      ],      
      "participants": [        
        "test1"      
      ],      
      "join_url": "https://wemeet.qq.com/w/5NmV29k",      
      "settings": {        
        "mute_enable_join": true,        
        "allow_unmute_self": false      
      }    
    }  
  ]
}

```
