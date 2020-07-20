## 点击坐标宏替换

### 点击坐标说明： 

    downX：点击时的 x 坐标，需要替换宏DOWNX 
    downY：点击时的 y 坐标，需要替换宏DOWNY 
    upX：点击抬起时的 x 坐标，需要替换宏UPX 
    upY：点击抬起时的 y 坐标，需要替换宏 UPY
    
### 详细说明： 

1、坐标指相对于实际广告位的左上角的坐标，如下图所示，箭头方向为正坐标方向，单位为像素。
    ![点击坐标系](/image/click1.png)
    
2、以下![坐标均会进行宏替换](/image/click2.png)，若无法获取坐标，会替换为-999。单位均为像素。 

    名称    类型    必填     描述                                  
                      
    downX  Int      否     手指点击时的横坐 标，需要替换 宏 DOWNX     
                                                                 
    downY  Int      否     手指点击时的纵坐 标，需要替换 宏 DOWNY    

    upX    Int      否     手指抬起离开设备 时的横坐标， 需 要替换宏 UPX 

    upY    Int      否     手指抬起离开设备 时的纵坐标， 需 要替换宏 UPY 
    
    限制：针对全部。有要求上报点击坐标的，可以按 照此协议拼接点击上报url，当无法获取时，会替换为-999。
    示例：downX=DOWNX&downY=DOWNY&upX=UPX&upY=UPY 
