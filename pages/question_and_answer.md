# 常见问题以及注意事项

## Q：广告应答adm字段如何展示？

位置：BidResponse.Seatbid.Bid.adm

A：请 check 以下项：

1. 非信息流广告填写物料访问地址；

2. 信息流广告填写信息流模板相关JSON数据并且**templateid**必填，JSONString（详细请参考附录6[模板定义](template.md)）。 

## Q：广告应答时需要注意哪些字段类型？

A：请 check 以下项：

1. price出价  Int型(必填)，分/CPM 计，低于底价的将竞价失败；

2. adm  String类型，信息流模板时填写相关JSON,参考[模板定义](template.md)；

3. templateid 原生广告必填（Request 对应的模板 ID）； 

4. durl       String类型 到达页地址

5. nurl和curl  为字符串数组

## Q: 如何判断请求的广告类型是什么？

1. 根据 BidRequest.Imp.type 字段进行判断

   根据该字段的值确定广告位类型，0:横幅,1:开屏,3:插屏,4:视频,5:原生
   
    . 0、1、3 对应:banner
    
    . 4 对应video
    
    . 5 对应nativead
