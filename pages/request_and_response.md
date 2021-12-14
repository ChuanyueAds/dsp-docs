# DSP 接入文档 

- [DSP 接入文档协议]()
    - [协议与规则说明 ](#协议与规则说明)
    - [接口注意事项](#接口注意事项)
    - [广告请求](#广告请求)
        - [接口信息(BidRequest)](#接口信息(BidRequest))
        - [Imp信息（BidRequest.Imp）](#Imp信息（BidRequest.Imp）)
            - [Ext信息（BidRequest.Imp.Ext）](#Ext信息（BidRequest.Imp.Ext）)
            - [Banner信息（BidRequest.Imp.Banner）](#Banner信息（BidRequest.Imp.Banner）)
            - [Nativead信息（BidRequest.Imp.Nativead）](#Nativead信息（BidRequest.Imp.Nativead）)
                - [Assets信息（BidRequest.Imp.Nativead.Assets）](#Assets信息（BidRequest.Imp.Nativead.Assets）)
                   - [Image信息 （BidRequest.Imp.Nativead.Assets.Ext.Image）](#geo%E5%AF%B9%E8%B1%A1%E6%89%A9%E5%B1%95bidrequestdevicegeoext)
                   - [Video信息 （BidRequest.Imp.Nativead.Assets.Ext.Video）](#geo%E5%AF%B9%E8%B1%A1%E6%89%A9%E5%B1%95bidrequestdevicegeoext)
                   - [Icon信息 （BidRequest.Imp.Nativead.Assets.Ext.Icon）](#geo%E5%AF%B9%E8%B1%A1%E6%89%A9%E5%B1%95bidrequestdevicegeoext) 
        - [App信息（BidRequest.App）](#App信息（BidRequest.App）)
            - [Pmp信息（BidRequest.Imp.Pmp）](#Pmp信息（BidRequest.Imp.Pmp）)
                - [Deals信息（BidRequest.Imp.Pmp.Deals）](#Deals信息（BidRequest.Imp.Pmp.Deals）)
        - [Device信息（BidRequest.Device）](#Device信息（BidRequest.Device）)
            - [Geo信息（BidRequest.Device.Geo）](#Geo信息（BidRequest.Device.Geo）)
        - [User信息 (BidRequest.User)](#User信息(BidRequest.User))
    - [广告应答](#广告应答)
         - [接口信息（BidResponse）](#接口信息（BidResponse）)
             - [SeatBid信息（BidResponse.SeatBid）](#SeatBid信息（BidResponse.SeatBid）)
                 - [Bid信息（BidResponse.SeatBid.Bid）](#Bid信息（BidResponse.SeatBid.Bid）)
     - [向DSP发送的竞价结果接口(Win Notice)](#向DSP发送的竞价结果接口(Win Notice))
           
##  协议与规则说明 

1. 此文档协助 DSP 接入 SSP or ADX 广告平台，其中定义了接口和交互的标准规范

2. 通信协议采用 HTTP 或者 HTTPS

3. 使用 POST 方法发送 BidRequest, 开启keep-alive，消息格式为 Json。 

4. HTTP 状态码: 正常广告应答包的返回状态码为 HTTP 200 OK，没有广告返回的则为 204 no content。其他情况均认为异常（等同与没有广告返回）。
    
## 接口注意事项

1. HTTP 请求 Content-Type 头部值为 application/json,通过 http 或 https 协议 POST 方式交换信息，目前timeout 设为 300ms;

2. 不出价时请返回HTTP 状态码204 (No Content);

3. 通过用户 DSPID（dspid）以及授权令牌（token）来做 权限验证（包含在头部中发送）;

4. 字段中所有中文必须使用UTF-8编码。

## 广告请求

### 接口信息(BidRequest)

| 字段名称  | 类型   |         | 必须 | 描述                                                                                                                                                     |
| -------- | ------ | ------ | ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id       | string |        | 是   | Bid Request 唯一 ID，由 SSP 生成                                                               |
| imp      | object array |         | 是   | 标识广告位曝光的 IMP 对象列表                                                                                                                                      |
| site     | object |        | 否   | Site  流量方对象，只针对 website 流量时存在                                                                                                                                   |
| app      | object |        | 否   | APP 流量方对象，只针对 app 流量时存在                                                                                                                                  |
| device   | object |        | 否   | 描述用户的设备信息                 |
| user     | object |        | 否   | 投放受众的用户信息描述                                                                                                                                     |
| test     | Int    |        | 是   | 0：生产模式，1：测试模式，默认 0  |
| tmax     | Int |           | 否   | 超时时间，单位毫秒，目前此参数不参与请求                                                                                                                                         |
| secure   | Int |           | 否   | 标记返回链接是否必须 https(0 =否,1 =是)，默认0，目前此参数不参与请求   
| bcat     | string array  |        | 否   | 广告行业黑名单 ，目前此参数不参与请求 
| ext      | string |        | 否   | 扩展对象 

### Imp信息（BidRequest.Imp）

| 字段名称  | 类型   | 默认值 | 必须 | 描述                                                                                                                             |
| --------- | ------ | ------ | ---- | -------------------------------------------------------------------------------------------------------------------------------- |
| id        | string |        | 是   | 曝光标识 ID，通常只在多次曝光有意义                                                                     |
| type      |  Int   |        | 是   | 广告类型（0:横幅,1:开屏,3:插屏,4:视频,5:原生） （0,1,3 对应banner 4. 对应video 5. nativead）                                                                                                                 |
| pid       | string |        | 是   | 广告位 ID                                                                                                                          |
| banner    | object |        | 否   | Banner 广告该字段必须存在                                                                                               |
| nativead  | object |        | 否   | 原生广告该字段必须存在 
| video     | object |        | 否   | 视频广告该字段必须存在                                                                                  |
| pos       | Int |           | 否   | 广告位位置 ID（参照附录 5.1）   
| bidtype   | Int |           | 否   | 售卖方式，0 = cpm, 1 = cpc,默认值0(暂时只支持cpm) 
| bidfloor  | Int |           | 是   | 底价，当 bidtype＝0 时，单位:分/CPM；当 bidtype = 1时，单位：分/点击，默认0 
| pmp       | object |        | 否   | 当该字段存在且不为空时，标识是PD或者PDB 流量
| ext       | object |        | 否   | 扩展对象 

#### Ext信息（BidRequest.Imp.Ext）

| 字段名称       | 类型   |  | 必须 | 描述                                                                                                                                                            |
| -------------- | ------ | ------ | ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| atype          | Int |        | 是   | 广告位支持的交互类型：1=网页打开；（默认） 2=网页打开+点击下载； 3=网页打开+deeplink； 4=(网页打开+点击下载+deeplink)。                                                                                               |


#### Banner信息（BidRequest.Imp.Banner）

| 字段名称    | 类型   |  | 必须 | 描述                                                |
| -----------| ----- | ------ | ---- | --------------------------------------------------- |
| id         | Int    |        | 否   | 广告位顺序 ID，一般从 1 开始                         |
| mimes      | string Array    |        | 是   | 支持的素材文件格式,通常包括（jpg,jpeg,png,gif）                     |
| w          | Int |           | 是   | 广告位宽                                           |
| h          | Int |           | 是   | 广告位高                                            |
| ext        | string |        | 否   | 扩展对象 


#### Nativead信息（BidRequest.Imp.Nativead）

| 字段名称     | 类型   |  | 必须 | 描述                                                                                                    |
| ------------ | ------ | ------ | ---- | ------------------------------------------------------------------------------------------------------- |
| template_ids | string Array  | | 是   |原生广告模板 ID 数组,返回任一符合模板条件的创意即可（参照[模板定义](template.md)）                                                                            |
| assets          | Object array  |        | 是   | 原生广告规则要求                                                                          |

##### Assets信息（BidRequest.Imp.Nativead.Assets）

| 字段名称 | 类型   | 默认值  | 必须  | 描述                                                                         |
| -------- | ------| ------ | ---- | ---------------------------------------------------------------------------- |
| id     | string  |        | 是   | 原生广告模板 ID（参照[模板定义](template.md)） 
| w      | int     |        | 否   | 原生广告位宽限制                                                      |
| h      | int     |        | 否   | 原生广告位高限制 
| ext    | object  |        | 否   | 扩展对象 

#####  Assets.ext信息（BidRequest.Imp.Nativead.Assets.Ext）

| 字段名称    | 类型     |        | 必须 | 描述                                                        |
| -----------| ------  | ------ | ---- | ----------------------------------------------------------- |
| image      | Object  |        | 否   | 图片素材属性要求                                                       |
| video      | Object  |        | 否   | 音视频素材属性要求                                               |
| icon       | Object  |        | 否   | icon 属性要求          |

##### Image信息 （BidRequest.Imp.Nativead.Assets.Ext.Image）

| 字段名称         | 类型   | 默认值 | 必须 | 描述                                                                                                                                                     |
| ---------------- | ------ | ------ | ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| mimes            | String array |  | 否   | 支持的素材文件格式,通常包括 （jpg,jpeg,png,gif）                                                                                                             |
| w                | int          |  | 否   | 素材图片宽限制 
| h                | int          |  | 否   | 素材图片高限制                                                                                |
| ext              | Object       |  | 否   | 扩展对象                                                                                                                                                |

##### Video信息 （BidRequest.Imp.Nativead.Assets.Ext.Video）

| 字段名称 | 类型  | 默认值 | 必须 | 描述                                                                          |
| -------- | ----- | ------ | ---- | ----------------------------------------------------------------------------- |
| mimes    | string array | | 否  | 支持的素材文件格式,通常包括（mp4、mp3、flv…）            
| w           | int    |        | 否   | 音视频素材宽限制(音频一般为 0)                                                                  |
| h           | int    |        | 否   | 音视频素材高限制(音频一般为 0)                                                                   |
| minduration | Int    |        | 否   | 音视频广告最小时长
| maxduration | Int    |        | 否   | 音视频广告最大时长 
| ext         | Object |        | 否   | 扩展对象

##### Icon信息 （BidRequest.Imp.Nativead.Assets.Ext.Icon）

| 字段名称 | 类型  | 默认值 | 必须 | 描述                                                                          |
| -------- | ----- | ------ | ---- | ----------------------------------------------------------------------------- |
| mimes    | string array |     | 否  | icon 支持的文件格式,通常包括（jpg,jpeg,png,gif）            
| w           | int    |        | 否   | icon 图片宽限制                                                                   |
| h           | int    |        | 否   | icon 图片高限制                                                                   |
| ext         | Object |        | 否   | 扩展对象

#### Video信息（BidRequest.Imp.Video）

| 字段名称    | 类型  | 默认值 | 必须 | 描述                                                                         |
| ----------- | ----- | ------ | ---- | ---------------------------------------------------------------------------- |
| mimes       | string array |        | 否   | 支持的广告播放格式,通常包括 mp4,flv                                                               |                                                         |
| minduration | int          |        | 否   | 视频广告最小时长                                                           |
| maxduration | int          |        | 否   | 视频广告最大时长                                                         |
| w           | int          |        | 否   | 广告位宽度                                                                   |
| h           | int          |        | 否   | 广告位高度                                                                   |
| size        | int          |        | 否   | 视频大小 

#### Pmp信息（BidRequest.Imp.Pmp）

| 字段名称     | 类型 | 默认值 | 必须 | 描述                                |
| ------------ | ---- | ------ | ---- | ----------------------------------- |
| deals        | Object array  |      | 是   | deal 对象  |

##### Deals信息（BidRequest.Imp.Pmp.Deals）

| 字段名称      | 类型 | 默认值 | 必须 | 描述                                |
| ------------- | ---- | ------ | ---- | ----------------------------------- |
| id      | String    |        | 是   | Dealid |
| at      | Int       |        | 是   | 竞价方式，目前只能是 1，竞价最高的 deal 获得成功，取最高出价作为胜出价  |
| price   | Int       |        | 是   | Deal 出价 单位 分/CPM   |

### App信息（BidRequest.App）

| 字段名称 | 类型  | 默认值 | 必须 | 描述                                                                         |
| -------- | ----- | ------ | ---- | ---------------------------------------------------------------------------- |
| id      | String    |        | 是   | 在 ssp 的唯一标识 ID |
| name    | String    |        | 否   | 应用名称 |
| bundle  | String    |        | 是   | APP 包名，具有唯一性  |
| cat     | Int array |        | 是   | app 的内容分类列表（暂保留)  |
| ver      | String   |        | 是   | APP 版本号   |                                                           |

### Device信息（BidRequest.Device）

| 字段名称 | 类型  | 默认值 | 必须 | 描述                                                                         |
| -------- | ----- | ------ | ---- | ---------------------------------------------------------------------------- |
| ip            | String    |        | 否   | 设备 IP 地址  |
| ua            | String    |        | 否   | 浏览器 User-agent  |
| carrier       | String    |        | 否   | 运营商 ID （参照[运营商ID](appendix/carrier.md)）  |
| connectiontype| Int       |        | 否   | 网络类型 （参照[网络类型](appendix/network.md)）  |
| devicetype    | Int       |        | 否   | 设备类型 （参照[设备类型 ](appendix/device.md)    |                                                           |
| imei          | String    |        | 否   | imei  for andriod                                             |   
| oaid          | String    |        | 否   | oaid  for andriod 10+                                            |   
| imsi          | String    |        | 否   | imsi     |   
| mac           | String    |        | 否   | mac 地址     |   
| idfa          | String    |        | 否   | 明文 idfa for ios     |   
| androidid     | String    |        | 否   | 安卓 ID (dpid)   for andriod    |   
| openudid      | String    |        | 否   | 明文的 openudid  for ios    |   
| make          | String    |        | 否   | 设备生产商 例：HuaWei     |   
| model         | String    |        | 否   | 设备型号例：Meta 8      |  
| lan           | String    |        | 否   | 当前设备语言例:zh-CH         |  
| os            | String    |        | 否   | 设备操作系统 (Andriod,iOS,WP,Others) 四种       |  
| osv           | String    |        | 否   | 操作系统版本      |  
| h             | String    |        | 否   | 设备屏幕高度       |  
| w             | String    |        | 否   | 设备屏幕宽度      |  
| device_boot_mark             | String    |        | 否   | 阿里系客户需要的反作弊字段      |  
| device_update_mark             | String    |        | 否   | 阿里系客户需要的反作弊字段      |  
| geo           | Object    |        | 否   | 设备位置属性       |  
| ext           | String    |        | 否   | 拓展字段        | 

#### Geo信息（BidRequest.Device.Geo）

| 字段名称 | 类型  | 默认值 | 必须 | 描述                                                                         |
| -------- | ----- | ------ | ---- | ---------------------------------------------------------------------------- |
| city     | String    |        | 否   | 市 |
| country  | String    |        | 否   | 国家或地区名称  |
| lat      | Double    |        | 否   | 维度  |
| lon      | Double    |        | 否   | 经度  |
| metro    | String    |        | 否   | 省    |                                                           |
| zip      | String    |        | 否   | zip    | 

#### User信息 (BidRequest.User)
| 字段名称 | 类型  | 默认值 | 必须 | 描述                                                                         |
| -------- | ----- | ------ | ---- | ---------------------------------------------------------------------------- |
| ups  | String array |  | 否 |  | |

## 返回信息 （BidResponse）

### 接口信息（BidResponse）

| 字段名称  | 类型     | 必须 | 默认值 | 描述                                                                                                                                                              |
| --------- | -------- | ---- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id        | string      | 是   |        | 对应 Bid Request 的请求 ID                                                                                                                                             |
| seatbid[] | 对象数组     | 否   |        | 广告请求的应答信息  SeatBid对象，若提出竞价则需提供一个，并且只接受一个                                                                                                               |
| ext       | String      | 否   |        | 拓展字段  |

#### SeatBid信息（BidResponse.SeatBid）

| 字段名称 | 类型     | 默认值 | 必须 | 描述                |
| -------- | -------- | ------ | ---- | ------------------- |
| bid    | 对象数组 |        | 是   | 广告请求的应答结果 ，目前只取一个 |
| ext    | String  |        | 否   | 保留字段  |

##### Bid信息（BidResponse.SeatBid.Bid）

| 字段名称 | 类型     | 默认值 | 必须 | 描述                                                                                                                                                                                                      |
| -------- | -------- | ------ | ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ader_id   | string   |        | 是   | 广告主 ID：(流量平台广告主送审返回的 ID),白名单广告主创意无须推送审核即可投放，要享有白单权限时此字段必须填写                                                                                                                                                                                          |
| cid       | string   |        | 是   | 创意 ID：(流量平台创意送审返回的 ID)非白名单广告主必填，白名单广告主非必填，只要填写了平台就会进行审核验证                                                                                                                                                                                                    |
| price     | Int      |        | 是   | 分/CPM 计，低于底价的将竞价失败                                                                                                                               |
| adm       | string   |        | 是   | 非信息流广告填写物料访问地址；信息流广告填写信息流模板相关JSON数据并且 templateid 必填， JSON String（详细请参考[模板定义](template.md)）。   
| durl      | string   |        | 是   | 到达页地址                       |
| adck      | Int      |        | 是   | 到达页 durl 打开类型：  1：网页类型（默认）  2：下载类型                                                                                                                                                                            |
| deep_link | string   |        | 否   | deeplink 地址：(当请求中atype等于3或4时此字段才有效，媒体会优先处理 deep_link,当无法处理deep_link时转而处理durl,如果deep_link可以落地则不会处理durl，无论是否传deep_link 字段，都必须填写 durl 字段; 如果媒体不支持deeplink, 将会直接执行durl)                                                                                                                                                                            |
| w         | int      |        | 否   | 物料宽(建议填写，宽高符合请求中创意要求)                                                                                                                            |
| h         | int      |        | 否   | 物料高(建议填写，宽高符合请求中创意要求)                                                                                                                                                                                                 |
| nurl      | string[] |        | 是   | 曝光监测地址，支持价格宏替换                                                                                           |
| curl      | string[] |        | 否   | 点击监测上报地址                                                                                                                                                                                              |
| dealid    | string   |        | 否   | PMP 售卖方式必须带上,并且必须跟广告请求    所带相等                                                           |
| templateid| string   |        | 否   | 原生广告必填（Request 对应的模板 ID）                                                                                                                                                                                              |
| package_name       | string     |        | 否   | （安卓应用包名、ios 为 APP Store 中的    ID）：下载和 deeplink 类广告建议填写                                                                                                                                                                           |
| app_name           | string     |        | 否   | 应用名如：“锤子便签”；下载和 deeplink    类广告建议填写                                                                                                                                                                            |
| app_version        | string     |        | 否   | 应用版本号：下载和 deeplink 类广告填写                                                                                                                                                                          |
| deeplink_trackers        | string[]   |        | 否   | 吊起APP成功上报监测链接| 
| video_start        | string[]   |        | 否   | 音/视频广告（播放开始上报） 
| video_one_quarter  | string[]   |        | 否   | 音/视频广告（播放了 25%上报）  
| video_one_half     | string[]   |        | 否   | 音/视频广告（播放了 50%上报） 
| video_three_quarter| string[]   |        | 否   | 音/视频广告（播放了 75%上报）    
| video_complete     | string[]   |        | 否   | 音/视频广告（播放完成）    
| video_pause        | string[]   |        | 否   | 音/视频广告（用户点击展厅按钮时上报）  
| video_resume       | string[]   |        | 否   | 音/视频广告（被暂停或被停止之后， 主动    继续播放时上报）    
| video_skip         | string[]   |        | 否   | 音/视频广告（用户点击跳过按钮时上报）  
| video_mute         | string[]   |        | 否   | 音/视频广告（主动关闭声音时上报）   
| video_unmute       | string[]   |        | 否   | 音/视频广告（用户主动开启声音时上报）  
| video_reply        | string[]   |        | 否   | 音/视频广告（重播时上报）   
| video_close        | string[]   |        | 否   | 音/视频广告（关闭时上报）   
| video_full         | string[]   |        | 否   | 音/视频广告（全屏时上报）  
| video_exit_full    | string[]   |        | 否   | 音/视频广告（退出全屏时上报）   
| Ext                | string     |        | 否   | 拓展字段 

## 向DSP发送的竞价结果接口(Win Notice)

通过对展示监测链接中特定参数的宏替换，将广告的计费价格发送给赢得竞价的 DSP 平台
