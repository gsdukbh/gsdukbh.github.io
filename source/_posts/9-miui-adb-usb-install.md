---
title: '使用root权限跳过小米SIM卡确认'
date: '2023-11-22'
tags:  
    - Android
    - MIUI
    - 小米
    - USB安装应用
---



# 使用root权限跳过小米SIM卡确认



因为小米开启USB安装应用程序需要插入SIM 并且登录小米账户验证身份证。
这对于正常使用小米手机进行Android 开发来说一件非常痛苦的事情

<!-- more -->

## 具体方法.



修改文件 ` /data/data/com.miui.securitycenter/shared_prefs/remote_provider_preferences.xml` 

在 `map`节点中添加:



```xml
  <boolean name="permcenter_install_intercept_enabled" value="false" />
  <boolean name="security_adb_install_enable" value="true" />
```



修改 `adb`其他权限.

```shell
su setprop persist.security.adbinstall 1
su setprop persist.security.adbinput 1
su setprop persist.fastboot.enable 1
```

在更改 `xml`配置文件前先执行下面命令用于解除小米安全软件的影响.



```shell
su am force-stop com.miui.securitycenter
```

然后重启手机, USB安装应用已经打开.


