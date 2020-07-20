# API接口

- [API接口](#api%E6%8E%A5%E5%8F%A3)
    - [文档更新记录](#%E6%96%87%E6%A1%A3%E6%9B%B4%E6%96%B0%E8%AE%B0%E5%BD%95)
    - [前提准备](#dsp%E8%BE%85%E5%8A%A9api%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%A0%B9%E5%9C%B0%E5%9D%80)
    - [广告主审核API](#%E7%89%A9%E6%96%99%E5%AE%A1%E6%A0%B8api)
        - [广告主上传API](#%E6%8F%90%E4%BA%A4%E5%B9%BF%E5%91%8A%E7%B4%A0%E6%9D%90api)
            - [请求字段](#%E5%8E%9F%E7%94%9F%E5%B9%BF%E5%91%8Aasset)
            - [advertiser对像](#%E5%8E%9F%E7%94%9F%E5%B9%BF%E5%91%8Atitle)
                - [credentials资质集合详情（Advertiser.Credentials）](#%E5%8E%9F%E7%94%9F%E5%B9%BF%E5%91%8Aimage)
        - [返回信息说明](#%E8%BF%94%E5%9B%9E%E4%BF%A1%E6%81%AF%E8%AF%B4%E6%98%8E)
    - [查询广告主状态信息](#%E6%9F%A5%E8%AF%A2%E5%AE%A1%E6%A0%B8%E6%9C%AA%E9%80%9A%E8%BF%87%E7%9A%84%E5%B9%BF%E5%91%8A%E4%BF%A1%E6%81%AF)
        - [广告主状态信息获取](#%E8%BF%94%E5%9B%9E%E4%BF%A1%E6%81%AF%E8%AF%B4%E6%98%8E)
        - [advertiserid对像](#%E6%9F%A5%E8%AF%A2%E5%B9%BF%E5%91%8A%E7%9A%84%E5%AE%A1%E6%A0%B8%E7%8A%B6%E6%80%81)

## 文档更新记录

| 版本 | 作者 |    时间    |                  备注                  |
| ---- | ---- | ---------- | -------------------------------------- |
|  0.1 | xz   | 2020.1.27 | 创建                                   |


## 前提准备

1. dspid 和 token 通过 head 头发送，重复上传会重新发起审核。dspid和token会提前发送对方

2. 头部示例：curl -H  "content-type:application/json"  -H  "dspid:10"  -H "token:xxxxxxxx" ..... 

## 广告主审核API

|    方法   | 格式 |  编码 |
| --------- | ---- | ----- |
| HTTP POST | JSON | UTF-8 |

###  广告主上传API

    测试： 
        |  http://123.57.68.113:8070/openapi/Advertiser/upload  #上传广告主资质           
           
    正式： 
        |  https://sspapiad.ms.zhangyue.net/openapi/Advertiser/upload   #上传广告主资质        

####  请求字段

|   字段名称  |  类型  | 必须 |                                                                               描述                                                                               |
| ----------- | ------ | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| advertiser   | Array object  | 是   | 广告主信息，每次最多 10 条数据     
                                                                                                        |
####  advertiser对像

|   字段名称  |  类型  | 必须 |                                                                               描述                                                                               |
| ----------- | ------ | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| advertiserid   | string        | 是   | 广告主 id                                                                                                            |
| name           | string        | 是   | 广告主名称（公司名是称）                                                                                                                                                |
| web_site       | string        | 否   | 网址                                                                                          |
| industry_id    | int           | 是   | 行业分类(二级分类是详见[广告主行业分类编码](appendix/advertising_industry.md))                                        |
| credentials    | Array object  | 是   | 资质集合                                                                                                                                               |

#### credentials资质集合详情（Advertiser.Credentials） 

| 字段名称 | 类型 | 必须 |                             描述                            |
| -------- | ---- | ---- | ----------------------------------------------------------- |
| cid        | int    | 是   | 资质标识。 1:营业执照 具体内容详见[资质文件](appendix/qualifications.md)                                                     |
| url        | string | 是   | 资质url 。同一资质多文件，请用压缩包方式（支持： rar,7z,zip）        |
| remarks    | string | 否   | 资质补充内容。 有特殊要求时必填，如：要求填写组织机构代码     |                                        |

### 返回信息说明

| 字段名称 |  类型  | 必须 |           描述           |
| -------- | ------ | ---- | ------------------------ |
| code      | int    | 否   | 0表示成功，1 不成功  |
| data      | Array  | 是   | 具体内容请参考请求示例：1、"advertiserid":DSP端提交的ID， 2、"order_id":SSP端生成的唯一 ID        |
| message   | string | 否   | 错误提示     |
                                                                                                      |
```
请求示例：
        {
            "advertiser": [
                {
                    "advertiserid": 435791021,
                    "credentials": [
                        {
                            "cid": 1,
                            "remarks": "1101050167411910",
                            "url": "http://www.xx.xx/xxx.jpg"
                        },
                        16
                    ],
                    "industry_id": 5101,
                    "name": "yiuty",
                    "web_site": "http://www.bb.x"
                }
            ]
        }

返回示例：
    成功：
        {
            "code": 0,
            "data": [
                {
                    "advertiserid": 435791021,//ssp分配的广告主id，验证失败则返回空 
                    "order_id": 100
                }
            ],
            "message": ""
        }

    失败：
        {
            "code": 1,
            "data": [],
            "message": "请求失败"
        }
```


## 查询广告主状态信息 
```
        测试： 
            |  http://123.57.68.113:8070/openapi/Advertiser/get  #获取广告主上传状态           
               
        正式： 
            |  https://sspapiad.ms.zhangyue.net/openapi/Advertiser/get  #获取广告主上传状态        
```

### 广告主状态信息获取

|   字段名称  |  类型  | 必须 |                                                  描述                                                  |
| ----------- | ------ | ---- | ------------------------------------------------------------------------------------------------------ |
| advertiserid[]   | Array object  | 是   | 广告主id，SSP 端分配的广告主 id，每次最多 10 条数据                                                 |

#### advertiserid对像

| 字段名称 |  类型  | 必须 |           描述           |
| -------- | ------ | ---- | ------------------------ |
| code      | int    | 否   | 0表示成功，1 不成功  |
| data      | Array  | 是   | 具体内容请参考请求示例：1、"advertiserid":查询的广     告主 ID，     2、"audit":审核状态(1:待审     核,2:通过,3:拒绝)     3、"refuseReason":拒绝原因     4、"is_white":是否白名单     (1:是，0:否   |
| message   | string | 否   | 错误提示     |
      
```
请求示例：
        {
            "advertiserid": [
                100
            ]
        }

返回示例：
        {
            "code": 0,
            "data": [
                {
                    "advertiserid": 100,
                    "audit": 1, //审核状态(1:待审核,2:通过,3:拒绝) 
                    "is_white": 1,,//是否白名单(1:是，0:否)  暂不支持白名单
                    "refuseReason": ""//拒绝原因 
                }
            ],
            "message": ""
        }
```