## 模板定义 

1. 广告模板 ID 即 TemplateID, 命名方式：A-B （Ａ为广告类型，Ｂ为顺序号） 

| 广告类型  | 类型编号    |                                                                                                                                                     |
| -------- | ------ | ---------------- |
| 原生图文           | 1 |         |                                                                |
| 原生视频、音频      | 2 |         |                                                                                                                                   |

##  模板json信息

A. 1-1 (一图一文)

B. 1-8 (两图一文) 
 
C. 1-4 (三图一文) (暂时不支持)

    注意：json中的imgurl类型是字符串数组
```
{
    "title": "标题 ",
    "imgurl": [
        "https://auto1.sinaimg.cn/autoimg/car/23/15/131541523_950.jpg"
    ],                //素材 URL ,string数组
    "icon": "",       //图标url(非必填字段，以媒体列表要求为准)
    "icon_title": ""  //图标 slogan(非必填字段，以媒体列表要求为准) 
}
```

D. 1-2 (图文摘要) 

    注意：json中的imgurl类型是字符串数组
```
{
    "title": "标题 ",
    "imgurl": [
        "https://auto1.sinaimg.cn/autoimg/car/23/15/131541523_950.jpg"
    ],                //素材 URL ,string数组
    "content":"",     //摘要
    "icon": "",       //图标url(非必填字段，以媒体列表要求为准)
    "icon_title": ""  //图标 slogan(非必填字段，以媒体列表要求为准) 
}
```

E. 2-2 (原生信息流视频) 

    注意：json中的imgurl类型是字符串
```
{
    "title": "标题 ",
    "imgurl": "",     //素材URL,string类型(请特别注意类型)
    "videourl":"",    //视频URL,string类型 
    "videoduration":1,//音视频播放时长（秒） Int型
    "content":"",     //摘要
    "icon": "",       //图标url(非必填字段，以媒体列表要求为准)
    "icon_title": ""  //图标 slogan(非必填字段，以媒体列表要求为准) 
}
```

F. 2-3 (音频贴片)  暂不支持

    注意：json中的imgurl类型是字符串
```
{
    "title": "标题 ",
    "imgurl": "",     //素材URL,string类型(请特别注意类型)
    "videourl":"",    //视频URL,string类型 
    "videoduration":1,//音视频播放时长（秒） Int型
    "content":"",     //摘要
    "icon": "",       //图标url(非必填字段，以媒体列表要求为准)
    "icon_title": ""  //图标 slogan(非必填字段，以媒体列表要求为准) 
}
```
