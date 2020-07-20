# API接口

- [API接口](#api%E6%8E%A5%E5%8F%A3)
    - [文档更新记录](#%E6%96%87%E6%A1%A3%E6%9B%B4%E6%96%B0%E8%AE%B0%E5%BD%95)
    - [前提准备](#dsp%E8%BE%85%E5%8A%A9api%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%A0%B9%E5%9C%B0%E5%9D%80)
    - [创意上传API](#%E7%89%A9%E6%96%99%E5%AE%A1%E6%A0%B8api)
        - [提交广告素材地址](#%E6%8F%90%E4%BA%A4%E5%B9%BF%E5%91%8A%E7%B4%A0%E6%9D%90api)
        - [请求字段](#%E5%8E%9F%E7%94%9F%E5%B9%BF%E5%91%8Aasset)
            - [creative对像](#%E5%8E%9F%E7%94%9F%E5%B9%BF%E5%91%8Atitle)
        - [返回字段](#%E5%8E%9F%E7%94%9F%E5%B9%BF%E5%91%8Aimage)
        - [请求示例](#%E5%8E%9F%E7%94%9F%E5%B9%BF%E5%91%8Adata)
        - [返回示例](#%E8%BF%94%E5%9B%9E%E4%BF%A1%E6%81%AF%E8%AF%B4%E6%98%8E)
    - [获取创意的审核结果API](#%E6%9F%A5%E8%AF%A2%E5%AE%A1%E6%A0%B8%E6%9C%AA%E9%80%9A%E8%BF%87%E7%9A%84%E5%B9%BF%E5%91%8A%E4%BF%A1%E6%81%AF)
        - [请求字段](#%E8%BF%94%E5%9B%9E%E4%BF%A1%E6%81%AF%E8%AF%B4%E6%98%8E)
            - [creativeid对像](#%E6%9F%A5%E8%AF%A2%E5%B9%BF%E5%91%8A%E7%9A%84%E5%AE%A1%E6%A0%B8%E7%8A%B6%E6%80%81)
        - [请求示例](#%E8%BF%94%E5%9B%9E%E4%BF%A1%E6%81%AF%E8%AF%B4%E6%98%8E)
        - [返回示例](#%E6%9F%A5%E8%AF%A2dsp%E7%BB%9F%E8%AE%A1%E4%BF%A1%E6%81%AF)

## 文档更新记录

| 版本 | 作者 |    时间    |                  备注                  |
| ---- | ---- | ---------- | -------------------------------------- |
|  0.1 | xz  | 2020.7.17 | 创建                                   |

## 前提准备

1.注：dspid 和 token 通过 head 头发送。重复上传会重新发起审核（素材最长有效期从上传之日起 180 天，超过 
180 天将视为无效素材）

### 提交广告素材地址

    测试：
        | http://123.57.68.113:8070/openapi/creative/upload #上传创意
    
    正式：
        | https://sspapiad.ms.zhangyue.net/openapi/creative/upload   #上传创意 

### 请求字段

|   字段名称  |  类型  | 必须 |                                                                               描述                                                                               |
| ----------- | ------ | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| creative    | Array object | 是   | 每次最多 10 条数据                                                                                                             |

#### creative对像

|   字段名称  |  类型  | 必须 |                                                                               描述                                                                               |
| ----------- | ------ | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| advertiserid    | string | 是   | 广告主id，SSP 端分配的广告主 id                                                                                                            |
| creativeid      | string | 是   | 创意id，DSP端                                                                                                                                               |
| name            | string | 是   | 广告类型，现在支持的类型有0(banner)，1(插屏)，2(开屏)，3(原生)，4(视频)                                                                                          |
| width           | Int    | 是   | 宽                                        |
| height          | Int    | 是   | 高                                                                                                                                            |
| landingpage     | string | 是   | 到达地址                                                                                                                                                        |
| monitor         | Array string     | 否   | 曝光检测 |
| type            | Int    | 是   | 创意类型                                                                                                  |
| duration        | Int    | 否   | 创意时长，当 type=2 时，必填    
| materials       | Array string     | 否   | 素材的 url 地址列表，格式：[“http://xxx.xxx.xx/xxx.jpg”]当 type=3 时，不填   
| native          | Array object     | 否   | 模版内容，当type=3时，必填。（参照[模板定义](template.md) ）    
 
 ### 返回字段
 
 |   字段名称  |  类型  | 必须 |                                                                               描述                                                                               |
 | ----------- | ------ | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
 | code      | Int           | 否   | 状态码 0:成功，1:不成功 
 | data      | Array object  | 是   | 返回的结果集 ，具体内容请参考下面示例：1、creativeid 为 DSP 端提交的 ID，2、order_id为SSP端生成的唯一ID 
 | message   | String        | 否   | 错误提示
 
 ### 请求示例
 
```
信息流创意：
    {
        "creative": [
            {
                "advertiserid": "100",
                "creativeid": "121154793",
                "height": 260,
                "landingpage": "http://www.xxx.com",
                "materials": [[//当 type=3 时，不填 
                    "http://www.jxx.com/jjxxxxx.jpg"
                ],
                "monitor": [
                    "http://www.bd.xsd"
                ],
                "name": "sssss",
                "native": [//当 type=3 时,必填,其他不填 
                    {
                        "imgurl": [
                            "http://www.jxx.com/jjxxxxx.jpg"
                        ],
                        "title": "ertyui"
                    }
                ],
                "type": 3,
                "width": 580
            }
        ]
    }

Banner创意：
        {
            "creative": [
                {
                    "advertiserid": "100",
                    "creativeid": "801258",
                    "height": 1920,
                    "landingpage": "http://dlied5.myapp.com/h115_1.0.8.1Build20_e118ce.html",
                    "materials": [
                        "http://jlpool.oss-cn-shanghai.aliyuncs.com/IMG/xiaochuzhelianmengadx_1080-1920.jpg"
                    ],
                    "monitor": "",
                    "name": "test",
                    "type": 1,
                    "width": 1080
                }
            ]
        }
       
```

### 返回示例

```
成功：
    {
        "code": 0,
        "data": [
            {
                "creativeid": 121154793,
                "order_id": 10000
            }
        ],
        "message": ""
    }

失败：
    {
        "code": 1,
        "data": [],
        "message": "创意地址无效"
    }
```
 
###  获取创意的审核结果地址 

    测试：
        | http://123.57.68.113:8070/openapi/creative/get 或   #获取创意上传状态 
        
    正式：
        | https://sspapiad.ms.zhangyue.net/openapi/creative/get #获取创意上传状态

### 请求字段

|   字段名称  |  类型  | 必须 |                                                                               描述                                                                               |
| ----------- | ------ | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| creativeId  | Array | 是   | ssp返回的创意 id，若创意 id 不存在则忽略此 id。每次最多 10 条数据                                                                                                          |

#### creativeid对像

|   字段名称  |  类型  | 必须 |                                                                               描述                                                                               |
| ----------- | ------ | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| code     | Int           | 是   | 状态码 0 成功，1 不成功                                                                                                       |
| data     | Array object  | 是   | 返回的结果集 ,具体内容请参考下面示例；1、"creativeid":查询的广告主 ID， 2、"audit":审核状态(1:待审核,2:通过,3:拒 绝) 3、"refuseReason":拒绝原因                                                                                                                                                |
| message  | string        | 否   | 错误提示                                                                                       |

### 请求示例
 
```
    {
        "creativeid": [
            22,
            10000
        ]
    }
```

### 返回示例

```
    {
        "code": 0,
        "data": [
            {
                "audit": 1,//审核状态(1:待审核,2:通过,3:拒绝) 
                "creativeid": 10000,
                "refuseReason": ""//拒绝原因或者其他提示 
            }
        ],
        "message": ""
    }
```