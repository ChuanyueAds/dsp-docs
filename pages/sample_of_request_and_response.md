# JSON 请求和返回示例

- [JSON 请求和返回示例]()
	- [banner](#banner)
		- [banner 请求示例](#banner请求示例)
		- [banner 返回示例](#banner返回示例)
	- [原生](#原生)
		- [原生请求示例](#原生请求示例)
		- [原生返回示例](#原生返回示例)
	- [激励视频](#激励视频)	

## banner

### banner请求示例

1. 注意：BidRequest.Imp.type，0、1、3对应banner

```json
{
    "app": {
        "bundle": "com.chaozh.iReaderFree",
        "id": "10000",
        "name": "掌阅",
        "ver": "7.23.0"
    },
    "at": 0,
    "device": {
        "androidid": "35c4604658f50abe",
        "carrier": "46000",
        "connectiontype": 2,
        "devicetype": 2,
        "geo": {
            "city": "北京",
            "country": "中国",
            "lat": 39.938884,
            "lon": 116.397459,
            "metro": "北京",
            "zip": "110000"
        },
        "h": "360",
        "imei": "865166027424665",
        "imei_md5": "e479835e213604320b5bde2991acf2ec",
        "imsi": "46000",
        "ip": "182.48.105.19",
        "mac": "00:DB:96:D2:80:79",
        "mac_md5": "ab05502f3ab0d9b07ff72cfd9ee383da",
        "make": "oppo",
        "model": "r7plus",
        "operator_type": 1,
        "os": "Android",
        "osv": "5.1.1",
        "ua": "Mozilla/5.0 (Linux; Android 5.1.1; r7plus Build/LMY48Z) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/39.0.0.0 Mobile Safari/537.36",
        "w": "640"
    },
    "id": "1dhbBO1Ko0f2q9Y2nzqiloItYIR",
    "imp": [
        {
            "banner": {
                "h": 1920,
                "mimes": [
                    "jpg",
                    "png"
                ],
                "w": 1080
            },
            "bidfloor": 2000,
            "ext": {
                "atype": 4
            },
            "id": "1",
            "pid": "10000",
            "pmp": {
                "deals": [
                    {
                        "at": 1,
                        "bidfloor": 2000,
                        "id": "yum2020"
                    }
                ]
            },
            "type": 1
        }
    ],
    "test": 0,
    "user": {
        "usr": "i906000846"
    }
}
```

### banner返回示例

```json
{
    "id": "1di5aDxGD8DaqP3JGT38nlJowrR",
    "seatbid": [
        {
            "bid": [
                {
                    "adck": 1,
                    "ader_id": "329931f31b926255",
                    "adm": "http://sv.cdn.n1q.cn/sandbox/fabce040-b4f9-11ea-9c7e-850e6d4acb4e.jpg",
                    "cid": "4270",
                    "curl": [
                        "https://sandbox.squiz.sv.nequal.cn/v1.1/c.gif?cp=1019&sp=16090&ad=3495&adgp=0&cr=4270&rq=1di5aDxGD8DaqP3JGT38nlJowrR&mck=__MCOOKIE__&did=e479835e213604320b5bde2991acf2ec&dmp=9&rs=1275320747682623489&srcsp=16090&maca=189e4919798c613d284ef10226e37a57&maca1=__MAC1__&ta=0&excl=2&ig=,u",
                        "https://g.cn.miaozhen.com/x/k=2120244&p=7OfFl&dx=__IPDX__&rt=2&ns=__IP__&ni=__IESID__&v=__LOC__&xa=__ADPLATFORM__&tr=__REQUESTID__&mo=__OS__&m0=__OPENUDID__&m0a=__DUID__&m1=__ANDROIDID1__&m1a=__ANDROIDID__&m2=__IMEI__&m4=__AAID__&m5=__IDFA__&m6=__MAC1__&m6a=__MAC__&tr3=__YUMDEALID__&o=",
                        "https://g.cn.miaozhen.com/x/k=2120244&p=7OfFl&dx=__IPDX__&rt=2&ns=__IP__&ni=__IESID__&v=__LOC__&xa=__ADPLATFORM__&tr=__REQUESTID__&mo=__OS__&m0=__OPENUDID__&m0a=__DUID__&m1=__ANDROIDID1__&m1a=__ANDROIDID__&m2=__IMEI__&m4=__AAID__&m5=__IDFA__&m6=__MAC1__&m6a=__MAC__&tr3=__YUMDEALID__&o="
                    ],
                    "dealid": "yum2020",
                    "durl": "https://tlogin.kfc.com.cn/CRM/others/jumpback.html",
                    "h": 1920,
                    "nurl": [
                        "https://sandbox.squiz.sv.nequal.cn/v1.1/i.gif?cp=1019&sp=16090&ad=3495&adgp=0&cr=4270&rq=1di5aDxGD8DaqP3JGT38nlJowrR&mck=__MCOOKIE__&did=e479835e213604320b5bde2991acf2ec&dmp=9&rs=1275320747682623489&srcsp=16090&maca=189e4919798c613d284ef10226e37a57&maca1=__MAC1__&ta=0&excl=2&ci=__CIDX__&ig=,u",
                        "https://g.cn.miaozhen.com/x/k=2120244&p=7OfFl&dx=__IPDX__&rt=2&ns=__IP__&ni=__IESID__&v=__LOC__&xa=__ADPLATFORM__&tr=__REQUESTID__&mo=__OS__&m0=__OPENUDID__&m0a=__DUID__&m1=__ANDROIDID1__&m1a=__ANDROIDID__&m2=__IMEI__&m4=__AAID__&m5=__IDFA__&m6=__MAC1__&m6a=__MAC__&tr3=__YUMDEALID__&o=",
                        "https://g.cn.miaozhen.com/x/k=2120244&p=7OfFl&dx=__IPDX__&rt=2&ns=__IP__&ni=__IESID__&v=__LOC__&xa=__ADPLATFORM__&tr=__REQUESTID__&mo=__OS__&m0=__OPENUDID__&m0a=__DUID__&m1=__ANDROIDID1__&m1a=__ANDROIDID__&m2=__IMEI__&m4=__AAID__&m5=__IDFA__&m6=__MAC1__&m6a=__MAC__&tr3=__YUMDEALID__&o="
                    ],
                    "price": 25,
                    "w": 1080
                }
            ]
        }
    ]
}
```

## 原生

### 原生请求示例

1. 注意：BidRequest.Imp.type，5 对应 nativead

```json
{
    "app": {
        "bundle": "com.chaozh.iReaderFree",
        "id": "10000",
        "name": "掌阅",
        "ver": "3.0.0"
    },
    "at": 0,
    "device": {
        "androidid": "35c4604658f50abe",
        "carrier": "46000",
        "connectiontype": 2,
        "devicetype": 2,
        "geo": {
            "city": "重庆",
            "country": "中国",
            "lat": 29.431586,
            "lon": 106.912251,
            "metro": "重庆",
            "zip": "500000"
        },
        "h": "360",
        "imei": "865166027424665",
        "imei_md5": "e479835e213604320b5bde2991acf2ec",
        "imsi": "46000",
        "ip": "14.108.175.73",
        "mac": "00:DB:96:D2:80:79",
        "mac_md5": "ab05502f3ab0d9b07ff72cfd9ee383da",
        "make": "oppo",
        "model": "r7plus",
        "operator_type": 1,
        "os": "Android",
        "osv": "5.1.1",
        "ua": "Mozilla/5.0 (Linux; Android 5.1.1; r7plus Build/LMY48Z) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/39.0.0.0 Mobile Safari/537.36",
        "w": "640"
    },
    "id": "1dRTtwkXJ0WoALPmKMXZscy2iQg",
    "imp": [
        {
            "bidfloor": 200,
            "ext": {
                "atype": 4
            },
            "id": "1",
            "nativead": {
                "assets": [
                    {
                        "ext": {
                            "image": {
                                "h": 720,
                                "mimes": [
                                    "jpg",
                                    "png"
                                ],
                                "w": 1280
                            }
                        },
                        "h": 720,
                        "id": "1-1",
                        "w": 1280
                    },
                    {
                        "ext": {
                            "image": {
                                "h": 720,
                                "mimes": [
                                    "jpg",
                                    "png"
                                ],
                                "w": 1280
                            }
                        },
                        "h": 720,
                        "id": "1-2",
                        "w": 1280
                    }
                ],
                "template_ids": [
                    "1-1",
                    "1-2"
                ]
            },
            "pid": "10017",
            "type": 5
        }
    ],
    "test": 0,
    "user": {
        "usr": "i3117071792"
    }
}
```

### 原生返回示例

1. 注意 1-1 和 1-2 模板json中素材imgurl是字符串数组。2-2模板json中imgurl是字符串

2. 请仔细阅读模板定义说明

```json
{
    "id": "1dRTtwkXJ0WoALPmKMXZscy2iQg",
    "seatbid": [
        {
            "bid": [
                {
                    "adck": 1,
                    "ader_id": "99994",
                    "adm": "{\"title\":\"二手汽车哪里有\",\"imgurl\":[\"https://auto1.sinaimg.cn/autoimg/car/23/15/131541523_950.jpg?aid=19624\\u0026cid=11100\\u0026channel=ireader\"],\"content\":\"二手汽车哪里有\"}",
                    "app_name": "微信",
                    "app_version": "10.5.1",
                    "cid": "123321",
                    "curl": [
                        "http://www.testthird.com/c"
                    ],
                    "durl": "http://gdown.baidu.com/data/wisegame/89eb17d6287ae627/weixin_1300.apk",
                    "h": 360,
                    "nurl": [
                        "http://www.testdsp.com/n/price=%%PRICE%%"
                    ],
                    "package_name": "com.tencent.mm",
                    "price": 0,
                    "templateid": "1-1",
                    "w": 640
                }
            ]
        }
    ]
}
```

## 激励视频

  ### 激励视频请求示例
  ```json
  {
    "app": {
        "bundle": "com.chaozh.iReaderFree",
        "id": "10000",
        "name": "掌阅",
        "ver": "7.23.0"
    },
    "at": 0,
    "device": {
        "androidid": "35c4604658f50abe",
        "carrier": "46000",
        "connectiontype": 2,
        "devicetype": 2,
        "geo": {
            "city": "北京",
            "country": "中国",
            "lat": 39.938884,
            "lon": 116.397459,
            "metro": "北京",
            "zip": "110000"
        },
        "h": "360",
        "imei": "865166027424665",
        "imei_md5": "e479835e213604320b5bde2991acf2ec",
        "imsi": "46000",
        "ip": "182.48.105.19",
        "mac": "00:DB:96:D2:80:79",
        "mac_md5": "ab05502f3ab0d9b07ff72cfd9ee383da",
        "make": "oppo",
        "model": "r7plus",
        "operator_type": 1,
        "os": "Android",
        "osv": "5.1.1",
        "ua": "Mozilla/5.0 (Linux; Android 5.1.1; r7plus Build/LMY48Z) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/39.0.0.0 Mobile Safari/537.36",
        "w": "640"
    },
    "id": "1dhbBO1Ko0f2q9Y2nzqiloItYIR",
    "imp": [
        {
            "video": {
                "h": 1920,
                "mimes": [
                    "mp4",
                    "flv"
                ],
                "w": 1080,
		"minduration": 1,
		"maxduration": 15
            },
            "bidfloor": 2000,
            "ext": {
                "atype": 4
            },
            "id": "1",
            "pid": "10283",
            "type": 6
        }
    ],
    "test": 0,
    "user": {
        "usr": "i906000846"
    }
}
  
```
  
 ### 激励视频返回示例
  
