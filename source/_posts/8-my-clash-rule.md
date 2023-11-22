---
title: '我的clash rule 配置 '
date: '2023-8-28'
tags:  
    - VPN
    - clash
    - v2ay

---

# 正常的开头

作为一个高强度上网冲浪的人，当然少不了一些加速上网的工具，类似clash之类的工具

# 我的 clash 配置。

clash是我最近发现的加速工具，能够满足我高度冲浪的需求。

我的配置如下：

<!-- more -->

[clash rule ](https://github.com/gsdukbh/clash-rule)



```yaml

rule-providers:
  AbemaTV:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/AbemaTV.yaml"
    path: ./ACL4SSR/AbemaTV.yaml
    interval: 86400

  Adobe:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/Adobe.yaml"
    path: ./ACL4SSR/Adobe.yaml
    interval: 86400

  Amazon:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/Amazon.yaml"
    path: ./ACL4SSR/Amazon.yaml
    interval: 86400

  AmazonIp:
    type: http
    behavior: ipcidr
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/AmazonIp.yaml"
    path: ./ACL4SSR/AmazonIp.yaml
    interval: 86400
  
  Apple:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/Apple.yaml"
    path: ./ACL4SSR/Apple.yaml
    interval: 86400

  Bahamut:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/Bahamut.yaml"
    path: ./ACL4SSR/Bahamut.yaml
    interval: 86400

  Bilibili:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/Bilibili.yaml"
    path: ./ACL4SSR/Bilibili.yaml
    interval: 86400

  BilibiliHMT:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/BilibiliHMT.yaml"
    path: ./ACL4SSR/BilibiliHMT.yaml
    interval: 86400

  Blizzard:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/Blizzard.yaml"
    path: ./ACL4SSR/Blizzard.yaml
    interval: 86400

  Developer:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/Developer.yaml"
    path: ./ACL4SSR/Developer.yaml
    interval: 86400

  DisneyPlus:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/DisneyPlus.yaml"
    path: ./ACL4SSR/DisneyPlus.yaml
    interval: 86400

  EHGallery:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/EHGallery.yaml"
    path: ./ACL4SSR/EHGallery.yaml
    interval: 86400

  Epic:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/Epic.yaml"
    path: ./ACL4SSR/Epic.yaml
    interval: 86400
   
  Google:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/Google.yaml"
    path: ./ACL4SSR/Google.yaml
    interval: 86400
      
  GoogleCN:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/GoogleCN.yaml"
    path: ./ACL4SSR/GoogleCN.yaml
    interval: 86400
    
  GoogleFCM:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/GoogleFCM.yaml"
    path: ./ACL4SSR/GoogleFCM.yaml
    interval: 86400
    
  HBO:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/HBO.yaml"
    path: ./ACL4SSR/HBO.yaml
    interval: 86400
    
  Microsoft:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/Microsoft.yaml"
    path: ./ACL4SSR/Microsoft.yaml
    interval: 86400
    
  NetEaseMusic:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/NetEaseMusic.yaml"
    path: ./ACL4SSR/NetEaseMusic.yaml
    interval: 86400
    
  Netflix:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/Netflix.yaml"
    path: ./ACL4SSR/Netflix.yaml
    interval: 86400
    
  OneDrive:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/OneDrive.yaml"
    path: ./ACL4SSR/OneDrive.yaml
    interval: 86400
    
  Porn:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/Porn.yaml"
    path: ./ACL4SSR/Porn.yaml
    interval: 86400
    
  Scholar:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/Scholar.yaml"
    path: ./ACL4SSR/Scholar.yaml
    interval: 86400
    
  Sony:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/Sony.yaml"
    path: ./ACL4SSR/Sony.yaml
    interval: 86400
    
  Spotify:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/Spotify.yaml"
    path: ./ACL4SSR/Spotify.yaml
    interval: 86400
    
  Steam:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/Steam.yaml"
    path: ./ACL4SSR/Steam.yaml
    interval: 86400
    
  SteamCN:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/SteamCN.yaml"
    path: ./ACL4SSR/SteamCN.yaml
    interval: 86400
    
  Telegram:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/Telegram.yaml"
    path: ./ACL4SSR/Telegram.yaml
    interval: 86400
    
  Xbox:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/Xbox.yaml"
    path: ./ACL4SSR/Xbox.yaml
    interval: 86400

  YouTube:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Ruleset/YouTube.yaml"
    path: ./ACL4SSR/YouTube.yaml
    interval: 86400

  BanAD:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/BanAD.yaml"
    path: ./ACL4SSR/BanAD.yaml
    interval: 86400
    
  BanEasyList:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/BanEasyList.yaml"
    path: ./ACL4SSR/BanEasyList.yaml
    interval: 86400
    
  BanEasyListChina:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/BanEasyListChina.yaml"
    path: ./ACL4SSR/BanEasyListChina.yaml
    interval: 86400
    
  BanEasyPrivacy:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/BanEasyPrivacy.yaml"
    path: ./ACL4SSR/BanEasyPrivacy.yaml
    interval: 86400
    
  BanProgramAD:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/BanProgramAD.yaml"
    path: ./ACL4SSR/BanProgramAD.yaml
    interval: 86400
    
  ChinaCompanyIp:
    type: http
    behavior: ipcidr
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/ChinaCompanyIp.yaml"
    path: ./ACL4SSR/ChinaCompanyIp.yaml
    interval: 86400
    
  ChinaDomain:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/ChinaDomain.yaml"
    path: ./ACL4SSR/ChinaDomain.yaml
    interval: 86400
    
  ChinaIp:
    type: http
    behavior: ipcidr
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/ChinaIp.yaml"
    path: ./ACL4SSR/ChinaIp.yaml
    interval: 86400
    
  ChinaMedia:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/ChinaMedia.yaml"
    path: ./ACL4SSR/ChinaMedia.yaml
    interval: 86400
 
  Download:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/Download.yaml"
    path: ./ACL4SSR/Download.yaml
    interval: 86400
           
  LocalAreaNetwork:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/LocalAreaNetwork.yaml"
    path: ./ACL4SSR/LocalAreaNetwork.yaml
    interval: 86400
    
  ProxyGFWlist:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/ProxyGFWlist.yaml"
    path: ./ACL4SSR/ProxyGFWlist.yaml
    interval: 86400
    
  ProxyMedia:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/ProxyMedia.yaml"
    path: ./ACL4SSR/ProxyMedia.yaml
    interval: 86400
    
  UnBan:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/ACL4SSR/ACL4SSR@master/Clash/Providers/UnBan.yaml"
    path: ./ACL4SSR/UnBan.yaml
    interval: 86400 

rules:
  # 广告
  - RULE-SET,BanAD,REJECT
  - RULE-SET,BanEasyList,REJECT
  - RULE-SET,BanEasyListChina,REJECT
  - RULE-SET,BanEasyPrivacy,REJECT
  - RULE-SET,BanProgramAD,REJECT
  # 国内
  - RULE-SET,ChinaCompanyIp,DIRECT
  - RULE-SET,ChinaDomain,DIRECT
  - RULE-SET,ChinaIp,DIRECT
  # 本地
  - RULE-SET,Download,DIRECT
  - RULE-SET,LocalAreaNetwork,DIRECT
  # GFW
  - RULE-SET,ProxyGFWlist,[PROXY]
  # 白名单
  - RULE-SET,UnBan,DIRECT
  # 媒体
  - RULE-SET,ChinaMedia,DIRECT
  - RULE-SET,ProxyMedia,[PROXY]
  - RULE-SET,AbemaTV,[PROXY]
  - RULE-SET,Bilibili,DIRECT
  - RULE-SET,BilibiliHMT,[PROXY]
  - RULE-SET,DisneyPlus,[PROXY]
  - RULE-SET,HBO,[PROXY]
  - RULE-SET,NetEaseMusic,DIRECT
  - RULE-SET,Netflix,[PROXY]
  - RULE-SET,Sony,[PROXY]
  - RULE-SET,Spotify,[PROXY]
  - RULE-SET,YouTube,[PROXY]
  # 购物
  - RULE-SET,Amazon,[PROXY]
  - RULE-SET,AmazonIp,[PROXY]
  - RULE-SET,Apple,[PROXY]
  # 游戏
  - RULE-SET,Bahamut,[PROXY]
  - RULE-SET,Blizzard,DIRECT
  - RULE-SET,Epic,[PROXY]
  - RULE-SET,Steam,[PROXY]
  - RULE-SET,SteamCN,DIRECT
  - RULE-SET,Xbox,[PROXY]
  # 开发
  - RULE-SET,Developer,[PROXY]
  # 18
  - RULE-SET,EHGallery,[PROXY]
  - RULE-SET,Porn,[PROXY]
  # 学术
  - RULE-SET,Scholar,[PROXY]
  # 其他
  - RULE-SET,Adobe,[PROXY]
  - RULE-SET,Google,[PROXY]
  - RULE-SET,GoogleCN,DIRECT
  - RULE-SET,GoogleFCM,[PROXY]
  - RULE-SET,Microsoft,[PROXY]
  - RULE-SET,OneDrive,[PROXY]
  - RULE-SET,Telegram,[PROXY]
  # 必须
  - MATCH,[PROXY]
```
