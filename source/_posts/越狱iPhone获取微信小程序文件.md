---
title: 越狱iPhone获取微信小程序文件
date: 2019-09-26 22:55:40
tags: 微信小程序、iPhone
id: iphoneGetWxapkg
---


### 反编译工具：
1. Node环境
2. wxappUnpacker [下载链接](https://github.com/qwerty472123/wxappUnpacker)



网上找了一圈，几乎都是Android的路径

```
adb pull /data/data/com.tencent.mm/MicroMsg/{User}/appbrand/pkg
```

其中`{User}` 为当前用户的用户名，类似于 `2bc**************b65`。

无赖iOS猿没root安卓机，但是iPhone越狱机还是有的。

* iPhone

  Cydia，安装开启 OpenSSH

* Mac

  ssh链接到手机，执行 `find / -name *.wxapkg`，即可找到到小程序保存的路径！！

  ```
  ssh root@手机IP地址
  （输入密码连接）
  iPhone:~ root# find / -name *.wxapkg
  /private/var/mobile/Containers/Data/Application/28BB4A85-6B20-4E48-AE32-F60ADCB3F5BB/Library/WechatPrivate/a15b39095380775c0a44250d3a184139/WeApp/LocalCache/release/wx53ed90a7aefc9795/18.wxapkg
  iPhone:~ root# exit
  logout
  Connection to 192.168.1.132 closed.
  
  ```

保存.wxapkg文件到本机器` scp root@手机IP地址:手机上.wxapkg文件路径 保存到本机的路径 `

```
$ scp root@192.168.1.132:/private/var/mobile/Containers/Data/Application/28BB4A85-6B20-4E48-AE32-F60ADCB3F5BB/Library/WechatPrivate/a15b39095380775c0a44250d3a184139/WeApp/LocalCache/release/wx53ed90a7aefc9795/18.wxapkg /Users/eagle/Downloads
root@192.168.1.132's password: 
18.wxapkg                                     100% 1777KB   3.4MB/s   00:00    
```

end

