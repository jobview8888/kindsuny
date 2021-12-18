# HTTP 端口
port: 7890

# SOCKS5 端口
socks-port: 7891

# Linux 及 macOS 的 redir 端口
# redir-port: 7892

allow-lan: false

# 仅适用于设置 allow-lan 为 true 时
# "*": 绑定所有 IP 地址
# 192.168.122.11: 绑定单个 IPv4 地址
# "[aaaa::a8aa:ff:fe09:57d8]": 绑定单个 IPv6 地址
# bind-address: "*"

# Rule / Global / Direct (默认为 Rule 模式)
mode: Rule

# 设置输出日志的等级 (默认为 info)
# info / warning / error / debug / silent
log-level: info

# RESTful API for clash
external-controller: 127.0.0.1:9090

# you can put the static web resource (such as clash-dashboard) to a directory, and clash would serve in `${API}/ui`
# input is a relative path to the configuration directory or an absolute path
# external-ui: folder

# Secret for RESTful API (Optional)
# secret: ""

# 实验性功能
experimental:
  ignore-resolve-fail: true # 忽略 DNS 解析失败，默认值为true
  # interface-name: en0 # 出站接口名称

# 本地 SOCKS5/HTTP(S) 服务器认证
# authentication:
#  - "user1:pass1"
#  - "user2:pass2"

# # 实验性 hosts, 支持通配符（如 *.clash.dev 甚至 *.foo.*.examplex.com ）
# # 静态域的优先级高于通配符域（foo.example.com > *.example.com）
hosts:
  'mtalk.google.com': 108.177.125.188
#   '*.clash.dev': 127.0.0.1
#   'alpha.clash.dev': '::1'

# dns:
  # enable: true # set true to enable dns (default is false)
  # ipv6: false # default is false
  # listen: 0.0.0.0:53
  # # default-nameserver: # resolve dns nameserver host, should fill pure IP
  # #   - 114.114.114.114
  # #   - 8.8.8.8
  # enhanced-mode: redir-host # or fake-ip
  # # fake-ip-range: 198.18.0.1/16 # if you don't know what it is, don't change it
  # fake-ip-filter: # fake ip white domain list
  #   - '*.lan'
  #   - localhost.ptlogin2.qq.com
  # nameserver:
  #   - 114.114.114.114
  #   - tls://dns.rubyfish.cn:853 # dns over tls
  #   - https://1.1.1.1/dns-query # dns over https
  # fallback: # concurrent request with nameserver, fallback used when GEOIP country isn't CN
  #   - tcp://1.1.1.1
  # fallback-filter:
  #   geoip: true # default
  #   ipcidr: # ips in these subnets will be considered polluted
  #     - 240.0.0.0/4

proxies:
# 支持的协议及加密算法示例请查阅 Clash 项目 README 以使用最新格式：https://github.com/Dreamacro/clash/blob/master/README.md

# Shadowsocks 支持的加密方式:
#   aes-128-gcm aes-192-gcm aes-256-gcm
#   aes-128-cfb aes-192-cfb aes-256-cfb
#   aes-128-ctr aes-192-ctr aes-256-ctr
#   rc4-md5 chacha20-ietf xchacha20
#   chacha20-ietf-poly1305 xchacha20-ietf-poly1305

# Shadowsocks
- name: "ss"
  type: ss
  server: server
  port: 443
  cipher: chacha20-ietf-poly1305
  password: "password"
  # udp: true

# Shadowsocks(simple-obfs)
- name: "ss-obfs"
  type: ss
  server: server
  port: 443
  cipher: chacha20-ietf-poly1305
  password: "password"
  plugin: obfs
  plugin-opts:
      mode: tls
      host: example.com

# Shadowsocks(v2ray-plugin)
- name: "ss-v2ray"
  type: ss
  server: server
  port: 443
  cipher: chacha20-ietf-poly1305
  password: "password"
  plugin: v2ray-plugin
  plugin-opts:
    mode: websocket # no QUIC now
    # tls: true # wss
    # skip-cert-verify: true
    # host: bing.com
    # path: "/"
    # mux: true
    # headers:
    #   custom: value

# VMess
- name: "v2ray"
  type: vmess
  server: server
  port: 443
  uuid: a3482e88-686a-4a58-8126-99c9df64b7bf
  alterId: 64
  cipher: auto
  # udp: true
  # tls: true
  # tls-hostname: 填写伪装域名
  # skip-cert-verify: true
  # network: ws
  # ws-path: /path
  # ws-headers: #这一行后面不要写东西
  #   Host: v2ray.com # 填写伪装域名

# Trojan
- name: "trojan"
  type: trojan
  server: sales.kindsuny.com
  port: 443
  password: Yq0010010010..
  # udp: true
  # sni: kindsuny.com # 填写伪装域名
  alpn:
     - h2
     - http/1.1
  # skip-cert-verify: true

# 代理组策略
# 策略组示例请查阅 Clash 项目 README 以使用最新格式：https://github.com/Dreamacro/clash/blob/master/README.md
proxy-groups:

# url-test 通过指定的 URL 测试并选择延迟最低的节点
- name: "自动选择快速节点"
  type: url-test
  proxies:
    - "ss"
    - "ss-obfs"
    - "ss-v2ray"
    - "v2ray"
    - "trojan"
  url: 'http://www.gstatic.com/generate_204'
  interval: 300

# fallback 通过指定的 URL 测试并选择可用的节点，当 1 故障不可用时自动切换到 2 以此类推
# - name: "Fallback"
#   type: fallback
#   proxies:
#     - "1"
#     - "2"
#     - "3"
#     - "4"
#   url: 'http://www.gstatic.com/generate_204'
#   interval: 300

# load-balance: 负载均衡
# - name: "LoadBalance"
#   type: load-balance
#   proxies:
#     - "1"
#     - "2"
#     - "3"
#     - "4"
#   url: 'http://www.gstatic.com/generate_204'
#   interval: 300

# 代理节点选择
- name: "PROXY"
  type: select
  proxies:
    - "自动选择快速节点"
    - "ss"
    - "ss-obfs"
    - "ss-v2ray"
    - "v2ray"
    - "trojan"

# 白名单模式 PROXY，黑名单模式 DIRECT
- name: "Final"
  type: select
  proxies:
    - "DIRECT"
    - "PROXY"

# Apple 服务代理
- name: "Apple"
  type: select
  proxies:
    - "DIRECT"
    - "PROXY"

# 国际流媒体服务
- name: "GlobalMedia"
  type: select
  proxies:
    - "PROXY"
    - "ss"
    - "ss-obfs"
    - "ss-v2ray"
    - "v2ray"
    - "trojan"
# 大陆流媒体面向港澳台限定服务
- name: "HKMTMedia"
  type: select
  proxies:
    - "DIRECT"
    - "ss"
    - "ss-obfs"
    - "ss-v2ray"
    - "v2ray"
    - "trojan"

# 规则
rules:
# Unbreak
# > Google
- DOMAIN-SUFFIX,googletraveladservices.com,DIRECT
- DOMAIN,dl.google.com,DIRECT
- DOMAIN,mtalk.google.com,DIRECT

# Internet Service Providers Hijacking 运营商劫持
- DOMAIN-SUFFIX,17gouwuba.com,REJECT
- DOMAIN-SUFFIX,186078.com,REJECT
- DOMAIN-SUFFIX,189zj.cn,REJECT
- DOMAIN-SUFFIX,285680.com,REJECT
- DOMAIN-SUFFIX,3721zh.com,REJECT
- DOMAIN-SUFFIX,4336wang.cn,REJECT
- DOMAIN-SUFFIX,51chumoping.com,REJECT
- DOMAIN-SUFFIX,51mld.cn,REJECT
- DOMAIN-SUFFIX,51mypc.cn,REJECT
- DOMAIN-SUFFIX,58mingri.cn,REJECT
- DOMAIN-SUFFIX,58mingtian.cn,REJECT
- DOMAIN-SUFFIX,5vl58stm.com,REJECT
- DOMAIN-SUFFIX,6d63d3.com,REJECT
- DOMAIN-SUFFIX,7gg.cc,REJECT
- DOMAIN-SUFFIX,91veg.com,REJECT
- DOMAIN-SUFFIX,9s6q.cn,REJECT
- DOMAIN-SUFFIX,adsame.com,REJECT
- DOMAIN-SUFFIX,aiclk.com,REJECT
- DOMAIN-SUFFIX,akuai.top,REJECT
- DOMAIN-SUFFIX,atplay.cn,REJECT
- DOMAIN-SUFFIX,baiwanchuangyi.com,REJECT
- DOMAIN-SUFFIX,beerto.cn,REJECT
- DOMAIN-SUFFIX,beilamusi.com,REJECT
- DOMAIN-SUFFIX,benshiw.net,REJECT
- DOMAIN-SUFFIX,bianxianmao.com,REJECT
- DOMAIN-SUFFIX,bryonypie.com,REJECT
- DOMAIN-SUFFIX,cishantao.com,REJECT
- DOMAIN-SUFFIX,cszlks.com,REJECT
- DOMAIN-SUFFIX,cudaojia.com,REJECT
- DOMAIN-SUFFIX,dafapromo.com,REJECT
- DOMAIN-SUFFIX,daitdai.com,REJECT
- DOMAIN-SUFFIX,dsaeerf.com,REJECT
- DOMAIN-SUFFIX,dugesheying.com,REJECT
- DOMAIN-SUFFIX,dv8c1t.cn,REJECT
- DOMAIN-SUFFIX,echatu.com,REJECT
- DOMAIN-SUFFIX,erdoscs.com,REJECT
- DOMAIN-SUFFIX,fan-yong.com,REJECT
- DOMAIN-SUFFIX,feih.com.cn,REJECT
- DOMAIN-SUFFIX,fjlqqc.com,REJECT
- DOMAIN-SUFFIX,fkku194.com,REJECT
- DOMAIN-SUFFIX,freedrive.cn,REJECT
- DOMAIN-SUFFIX,gclick.cn,REJECT
- DOMAIN-SUFFIX,goufanli100.com,REJECT
- DOMAIN-SUFFIX,goupaoerdai.com,REJECT
- DOMAIN-SUFFIX,gouwubang.com,REJECT
- DOMAIN-SUFFIX,gzxnlk.com,REJECT
- DOMAIN-SUFFIX,haoshengtoys.com,REJECT
- DOMAIN-SUFFIX,hyunke.com,REJECT
- DOMAIN-SUFFIX,ichaosheng.com,REJECT
- DOMAIN-SUFFIX,ishop789.com,REJECT
- DOMAIN-SUFFIX,jdkic.com,REJECT
- DOMAIN-SUFFIX,jiubuhua.com,REJECT
- DOMAIN-SUFFIX,jsncke.com,REJECT
- DOMAIN-SUFFIX,junkucm.com,REJECT
- DOMAIN-SUFFIX,jwg365.cn,REJECT
- DOMAIN-SUFFIX,kawo77.com,REJECT
- DOMAIN-SUFFIX,kualianyingxiao.cn,REJECT
- DOMAIN-SUFFIX,kumihua.com,REJECT
- DOMAIN-SUFFIX,ltheanine.cn,REJECT
- DOMAIN-SUFFIX,maipinshangmao.com,REJECT
- DOMAIN-SUFFIX,minisplat.cn,REJECT
- DOMAIN-SUFFIX,mkitgfs.com,REJECT
- DOMAIN-SUFFIX,mlnbike.com,REJECT
- DOMAIN-SUFFIX,mobjump.com,REJECT
- DOMAIN-SUFFIX,nbkbgd.cn,REJECT
- DOMAIN-SUFFIX,newapi.com,REJECT
- DOMAIN-SUFFIX,pinzhitmall.com,REJECT
- DOMAIN-SUFFIX,poppyta.com,REJECT
- DOMAIN-SUFFIX,qianchuanghr.com,REJECT
- DOMAIN-SUFFIX,qichexin.com,REJECT
- DOMAIN-SUFFIX,qinchugudao.com,REJECT
- DOMAIN-SUFFIX,quanliyouxi.cn,REJECT
- DOMAIN-SUFFIX,qutaobi.com,REJECT
- DOMAIN-SUFFIX,ry51w.cn,REJECT
- DOMAIN-SUFFIX,sg536.cn,REJECT
- DOMAIN-SUFFIX,sifubo.cn,REJECT
- DOMAIN-SUFFIX,sifuce.cn,REJECT
- DOMAIN-SUFFIX,sifuda.cn,REJECT
- DOMAIN-SUFFIX,sifufu.cn,REJECT
- DOMAIN-SUFFIX,sifuge.cn,REJECT
- DOMAIN-SUFFIX,sifugu.cn,REJECT
- DOMAIN-SUFFIX,sifuhe.cn,REJECT
- DOMAIN-SUFFIX,sifuhu.cn,REJECT
- DOMAIN-SUFFIX,sifuji.cn,REJECT
- DOMAIN-SUFFIX,sifuka.cn,REJECT
- DOMAIN-SUFFIX,smgru.net,REJECT
- DOMAIN-SUFFIX,taoggou.com,REJECT
- DOMAIN-SUFFIX,tcxshop.com,REJECT
- DOMAIN-SUFFIX,tjqonline.cn,REJECT
- DOMAIN-SUFFIX,topitme.com,REJECT
- DOMAIN-SUFFIX,tt3sm4.cn,REJECT
- DOMAIN-SUFFIX,tuia.cn,REJECT
- DOMAIN-SUFFIX,tuipenguin.com,REJECT
- DOMAIN-SUFFIX,tuitiger.com,REJECT
- DOMAIN-SUFFIX,websd8.com,REJECT
- DOMAIN-SUFFIX,wsgblw.com,REJECT
- DOMAIN-SUFFIX,wx16999.com,REJECT
- DOMAIN-SUFFIX,xchmai.com,REJECT
- DOMAIN-SUFFIX,xiaohuau.xyz,REJECT
- DOMAIN-SUFFIX,ygyzx.cn,REJECT
- DOMAIN-SUFFIX,yinmong.com,REJECT
- DOMAIN-SUFFIX,yitaopt.com,REJECT
- DOMAIN-SUFFIX,yjqiqi.com,REJECT
- DOMAIN-SUFFIX,yukhj.com,REJECT
- DOMAIN-SUFFIX,zhaozecheng.cn,REJECT
- DOMAIN-SUFFIX,zhenxinet.com,REJECT
- DOMAIN-SUFFIX,zlne800.com,REJECT
- DOMAIN-SUFFIX,zunmi.cn,REJECT
- DOMAIN-SUFFIX,zzd6.com,REJECT
- IP-CIDR,39.107.15.115/32,REJECT,no-resolve
- IP-CIDR,47.89.59.182/32,REJECT,no-resolve
- IP-CIDR,103.49.209.27/32,REJECT,no-resolve
- IP-CIDR,123.56.152.96/32,REJECT,no-resolve
# > ChinaTelecom
- IP-CIDR,61.160.200.223/32,REJECT,no-resolve
- IP-CIDR,61.160.200.242/32,REJECT,no-resolve
- IP-CIDR,61.160.200.252/32,REJECT,no-resolve
- IP-CIDR,61.174.50.214/32,REJECT,no-resolve
- IP-CIDR,111.175.220.163/32,REJECT,no-resolve
- IP-CIDR,111.175.220.164/32,REJECT,no-resolve
- IP-CIDR,122.229.8.47/32,REJECT,no-resolve
- IP-CIDR,122.229.29.89/32,REJECT,no-resolve
- IP-CIDR,124.232.160.178/32,REJECT,no-resolve
- IP-CIDR,175.6.223.15/32,REJECT,no-resolve
- IP-CIDR,183.59.53.237/32,REJECT,no-resolve
- IP-CIDR,218.93.127.37/32,REJECT,no-resolve
- IP-CIDR,221.228.17.152/32,REJECT,no-resolve
- IP-CIDR,221.231.6.79/32,REJECT,no-resolve
- IP-CIDR,222.186.61.91/32,REJECT,no-resolve
- IP-CIDR,222.186.61.95/32,REJECT,no-resolve
- IP-CIDR,222.186.61.96/32,REJECT,no-resolve
- IP-CIDR,222.186.61.97/32,REJECT,no-resolve
# > ChinaUnicom
- IP-CIDR,106.75.231.48/32,REJECT,no-resolve
- IP-CIDR,119.4.249.166/32,REJECT,no-resolve
- IP-CIDR,220.196.52.141/32,REJECT,no-resolve
- IP-CIDR,221.6.4.148/32,REJECT,no-resolve
# > ChinaMobile
- IP-CIDR,114.247.28.96/32,REJECT,no-resolve
- IP-CIDR,221.179.131.72/32,REJECT,no-resolve
- IP-CIDR,221.179.140.145/32,REJECT,no-resolve
# > Dr.Peng
# - IP-CIDR,10.72.25.0/24,REJECT,no-resolve
- IP-CIDR,115.182.16.79/32,REJECT,no-resolve
- IP-CIDR,118.144.88.126/32,REJECT,no-resolve
- IP-CIDR,118.144.88.215/32,REJECT,no-resolve
- IP-CIDR,118.144.88.216/32,REJECT,no-resolve
- IP-CIDR,120.76.189.132/32,REJECT,no-resolve
- IP-CIDR,124.14.21.147/32,REJECT,no-resolve
- IP-CIDR,124.14.21.151/32,REJECT,no-resolve
- IP-CIDR,180.166.52.24/32,REJECT,no-resolve
- IP-CIDR,211.161.101.106/32,REJECT,no-resolve
- IP-CIDR,220.115.251.25/32,REJECT,no-resolve
- IP-CIDR,222.73.156.235/32,REJECT,no-resolve

# Malware 恶意网站
# > 快压
# https://zhuanlan.zhihu.com/p/39534279
- DOMAIN-SUFFIX,kuaizip.com,REJECT
# > MacKeeper
# https://www.lizhi.io/blog/40002904
- DOMAIN-SUFFIX,mackeeper.com,REJECT
- DOMAIN-SUFFIX,zryydi.com,REJECT
# > Adobe Flash China Special Edition
# https://www.zhihu.com/question/281163698/answer/441388130
- DOMAIN-SUFFIX,flash.cn,REJECT
- DOMAIN,geo2.adobe.com,REJECT
# > C&J Marketing 思杰马克丁软件
# https://www.zhihu.com/question/46746200
- DOMAIN-SUFFIX,4009997658.com,REJECT
- DOMAIN-SUFFIX,abbyychina.com,REJECT
- DOMAIN-SUFFIX,bartender.cc,REJECT
- DOMAIN-SUFFIX,betterzip.net,REJECT
- DOMAIN-SUFFIX,betterzipcn.com,REJECT
- DOMAIN-SUFFIX,beyondcompare.cc,REJECT
- DOMAIN-SUFFIX,bingdianhuanyuan.cn,REJECT
- DOMAIN-SUFFIX,chemdraw.com.cn,REJECT
- DOMAIN-SUFFIX,cjmakeding.com,REJECT
- DOMAIN-SUFFIX,cjmkt.com,REJECT
- DOMAIN-SUFFIX,codesoftchina.com,REJECT
- DOMAIN-SUFFIX,coreldrawchina.com,REJECT
- DOMAIN-SUFFIX,crossoverchina.com,REJECT
- DOMAIN-SUFFIX,dongmansoft.com,REJECT
- DOMAIN-SUFFIX,earmasterchina.cn,REJECT
- DOMAIN-SUFFIX,easyrecoverychina.com,REJECT
- DOMAIN-SUFFIX,ediuschina.com,REJECT
- DOMAIN-SUFFIX,flstudiochina.com,REJECT
- DOMAIN-SUFFIX,formysql.com,REJECT
- DOMAIN-SUFFIX,guitarpro.cc,REJECT
- DOMAIN-SUFFIX,huishenghuiying.com.cn,REJECT
- DOMAIN-SUFFIX,hypersnap.net,REJECT
- DOMAIN-SUFFIX,iconworkshop.cn,REJECT
- DOMAIN-SUFFIX,imindmap.cc,REJECT
- DOMAIN-SUFFIX,jihehuaban.com.cn,REJECT
- DOMAIN-SUFFIX,keyshot.cc,REJECT
- DOMAIN-SUFFIX,kingdeecn.cn,REJECT
- DOMAIN-SUFFIX,logoshejishi.com,REJECT
- DOMAIN-SUFFIX,luping.net.cn,REJECT
- DOMAIN-SUFFIX,mairuan.cn,REJECT
- DOMAIN-SUFFIX,mairuan.com,REJECT
- DOMAIN-SUFFIX,mairuan.com.cn,REJECT
- DOMAIN-SUFFIX,mairuan.net,REJECT
- DOMAIN-SUFFIX,mairuanwang.com,REJECT
- DOMAIN-SUFFIX,makeding.com,REJECT
- DOMAIN-SUFFIX,mathtype.cn,REJECT
- DOMAIN-SUFFIX,mindmanager.cc,REJECT
- DOMAIN-SUFFIX,mindmanager.cn,REJECT
- DOMAIN-SUFFIX,mindmapper.cc,REJECT
- DOMAIN-SUFFIX,mycleanmymac.com,REJECT
- DOMAIN-SUFFIX,nicelabel.cc,REJECT
- DOMAIN-SUFFIX,ntfsformac.cc,REJECT
- DOMAIN-SUFFIX,ntfsformac.cn,REJECT
- DOMAIN-SUFFIX,overturechina.com,REJECT
- DOMAIN-SUFFIX,passwordrecovery.cn,REJECT
- DOMAIN-SUFFIX,pdfexpert.cc,REJECT
- DOMAIN-SUFFIX,photozoomchina.com,REJECT
- DOMAIN-SUFFIX,shankejingling.com,REJECT
- DOMAIN-SUFFIX,ultraiso.net,REJECT
- DOMAIN-SUFFIX,vegaschina.cn,REJECT
- DOMAIN-SUFFIX,xmindchina.net,REJECT
- DOMAIN-SUFFIX,xshellcn.com,REJECT
- DOMAIN-SUFFIX,yihuifu.cn,REJECT
- DOMAIN-SUFFIX,yuanchengxiezuo.com,REJECT
- DOMAIN-SUFFIX,zbrushcn.com,REJECT
- DOMAIN-SUFFIX,zhzzx.com,REJECT

# Global Area Network
# (GlobalMedia)
# (Music)
# > Deezer
# USER-AGENT,Deezer*,GlobalMedia
- DOMAIN-SUFFIX,deezer.com,GlobalMedia
- DOMAIN-SUFFIX,dzcdn.net,GlobalMedia
# > KKBOX
- DOMAIN-SUFFIX,kkbox.com,GlobalMedia
- DOMAIN-SUFFIX,kkbox.com.tw,GlobalMedia
- DOMAIN-SUFFIX,kfs.io,GlobalMedia
# > JOOX
# USER-AGENT,WeMusic*,GlobalMedia
# USER-AGENT,JOOX*,GlobalMedia
- DOMAIN-SUFFIX,joox.com,GlobalMedia
# > Pandora
# USER-AGENT,Pandora*,GlobalMedia
- DOMAIN-SUFFIX,pandora.com,GlobalMedia
# > SoundCloud
# USER-AGENT,SoundCloud*,GlobalMedia
- DOMAIN-SUFFIX,p-cdn.us,GlobalMedia
- DOMAIN-SUFFIX,sndcdn.com,GlobalMedia
- DOMAIN-SUFFIX,soundcloud.com,GlobalMedia
# > Spotify
# USER-AGENT,Spotify*,GlobalMedia
- DOMAIN-SUFFIX,pscdn.co,GlobalMedia
- DOMAIN-SUFFIX,scdn.co,GlobalMedia
- DOMAIN-SUFFIX,spotify.com,GlobalMedia
- DOMAIN-SUFFIX,spoti.fi,GlobalMedia
- DOMAIN-KEYWORD,spotify.com,GlobalMedia
- DOMAIN-KEYWORD,-spotify-com,GlobalMedia
# > TIDAL
# USER-AGENT,TIDAL*,GlobalMedia
- DOMAIN-SUFFIX,tidal.com,GlobalMedia
# > YouTubeMusic
# USER-AGENT,com.google.ios.youtubemusic*,GlobalMedia
# USER-AGENT,YouTubeMusic*,GlobalMedia
# (Video)
# > All4
# USER-AGENT,All4*,GlobalMedia
- DOMAIN-SUFFIX,c4assets.com,GlobalMedia
- DOMAIN-SUFFIX,channel4.com,GlobalMedia
# > AbemaTV
# USER-AGENT,AbemaTV*,GlobalMedia
- DOMAIN-SUFFIX,abema.io,GlobalMedia
- DOMAIN-SUFFIX,ameba.jp,GlobalMedia
- DOMAIN-SUFFIX,abema.tv,GlobalMedia
- DOMAIN-SUFFIX,hayabusa.io,GlobalMedia
- DOMAIN,abematv.akamaized.net,GlobalMedia
- DOMAIN,ds-linear-abematv.akamaized.net,GlobalMedia
- DOMAIN,ds-vod-abematv.akamaized.net,GlobalMedia
- DOMAIN,linear-abematv.akamaized.net,GlobalMedia
# > Amazon Prime Video
# USER-AGENT,InstantVideo.US*,GlobalMedia
# USER-AGENT,Prime%20Video*,GlobalMedia
- DOMAIN-SUFFIX,aiv-cdn.net,GlobalMedia
- DOMAIN-SUFFIX,aiv-delivery.net,GlobalMedia
- DOMAIN-SUFFIX,amazonvideo.com,GlobalMedia
- DOMAIN-SUFFIX,primevideo.com,GlobalMedia
- DOMAIN,avodmp4s3ww-a.akamaihd.net,GlobalMedia
- DOMAIN,d25xi40x97liuc.cloudfront.net,GlobalMedia
- DOMAIN,dmqdd6hw24ucf.cloudfront.net,GlobalMedia
- DOMAIN,d22qjgkvxw22r6.cloudfront.net,GlobalMedia
- DOMAIN,d1v5ir2lpwr8os.cloudfront.net,GlobalMedia
- DOMAIN-KEYWORD,avoddashs,GlobalMedia
# > Bahamut
# USER-AGENT,Anime*,GlobalMedia
- DOMAIN-SUFFIX,bahamut.com.tw,GlobalMedia
- DOMAIN-SUFFIX,gamer.com.tw,GlobalMedia
- DOMAIN,gamer-cds.cdn.hinet.net,GlobalMedia
- DOMAIN,gamer2-cds.cdn.hinet.net,GlobalMedia
# > BBC iPlayer
# USER-AGENT,BBCiPlayer*,GlobalMedia
- DOMAIN-SUFFIX,bbc.co.uk,GlobalMedia
- DOMAIN-SUFFIX,bbci.co.uk,GlobalMedia
- DOMAIN-KEYWORD,bbcfmt,GlobalMedia
- DOMAIN-KEYWORD,uk-live,GlobalMedia
# > DAZN
# USER-AGENT,DAZN*,GlobalMedia
- DOMAIN-SUFFIX,dazn.com,GlobalMedia
- DOMAIN-SUFFIX,dazn-api.com,GlobalMedia
- DOMAIN,d151l6v8er5bdm.cloudfront.net,GlobalMedia
- DOMAIN-KEYWORD,voddazn,GlobalMedia
# > Disney+
# USER-AGENT,Disney+*,GlobalMedia
- DOMAIN-SUFFIX,bamgrid.com,GlobalMedia
- DOMAIN-SUFFIX,disney-plus.net,GlobalMedia
- DOMAIN-SUFFIX,disneyplus.com,GlobalMedia
- DOMAIN-SUFFIX,dssott.com,GlobalMedia
- DOMAIN,cdn.registerdisney.go.com,GlobalMedia
# > encoreTVB
# USER-AGENT,encoreTVB*,GlobalMedia
- DOMAIN-SUFFIX,encoretvb.com,GlobalMedia
- DOMAIN,edge.api.brightcove.com,GlobalMedia
- DOMAIN,bcbolt446c5271-a.akamaihd.net,GlobalMedia
# > FOX NOW
# USER-AGENT,FOX%20NOW*,GlobalMedia
- DOMAIN-SUFFIX,fox.com,GlobalMedia
- DOMAIN-SUFFIX,foxdcg.com,GlobalMedia
- DOMAIN-SUFFIX,theplatform.com,GlobalMedia
- DOMAIN-SUFFIX,uplynk.com,GlobalMedia
# > HBO NOW
# USER-AGENT,HBO%20NOW*,GlobalMedia
- DOMAIN-SUFFIX,hbo.com,GlobalMedia
- DOMAIN-SUFFIX,hbogo.com,GlobalMedia
- DOMAIN-SUFFIX,hbonow.com,GlobalMedia
# > HBO GO HKG
# USER-AGENT,HBO%20GO%20PROD%20HKG*,GlobalMedia
- DOMAIN-SUFFIX,hbogoasia.com,GlobalMedia
- DOMAIN-SUFFIX,hbogoasia.hk,GlobalMedia
- DOMAIN,bcbolthboa-a.akamaihd.net,GlobalMedia
- DOMAIN,players.brightcove.net,GlobalMedia
- DOMAIN,s3-ap-southeast-1.amazonaws.com,GlobalMedia
- DOMAIN,dai3fd1oh325y.cloudfront.net,GlobalMedia
- DOMAIN,44wilhpljf.execute-api.ap-southeast-1.amazonaws.com,GlobalMedia
- DOMAIN,hboasia1-i.akamaihd.net,GlobalMedia
- DOMAIN,hboasia2-i.akamaihd.net,GlobalMedia
- DOMAIN,hboasia3-i.akamaihd.net,GlobalMedia
- DOMAIN,hboasia4-i.akamaihd.net,GlobalMedia
- DOMAIN,hboasia5-i.akamaihd.net,GlobalMedia
- DOMAIN,cf-images.ap-southeast-1.prod.boltdns.net,GlobalMedia
# > 华文电视
# USER-AGENT,HWTVMobile*,GlobalMedia
- DOMAIN-SUFFIX,5itv.tv,GlobalMedia
- DOMAIN-SUFFIX,ocnttv.com,GlobalMedia
# > Hulu
- DOMAIN-SUFFIX,hulu.com,GlobalMedia
- DOMAIN-SUFFIX,huluim.com,GlobalMedia
- DOMAIN-SUFFIX,hulustream.com,GlobalMedia
# > Hulu(フールー)
- DOMAIN-SUFFIX,happyon.jp,GlobalMedia
- DOMAIN-SUFFIX,hulu.jp,GlobalMedia
# > ITV
# USER-AGENT,ITV_Player*,GlobalMedia
- DOMAIN-SUFFIX,itv.com,GlobalMedia
- DOMAIN-SUFFIX,itvstatic.com,GlobalMedia
- DOMAIN,itvpnpmobile-a.akamaihd.net,GlobalMedia
# > KKTV
# USER-AGENT,KKTV*,GlobalMedia
# USER-AGENT,com.kktv.ios.kktv*,GlobalMedia
- DOMAIN-SUFFIX,kktv.com.tw,GlobalMedia
- DOMAIN-SUFFIX,kktv.me,GlobalMedia
- DOMAIN,kktv-theater.kk.stream,GlobalMedia
# > Line TV
# USER-AGENT,LINE%20TV*,GlobalMedia
- DOMAIN-SUFFIX,linetv.tw,GlobalMedia
- DOMAIN,d3c7rimkq79yfu.cloudfront.net,GlobalMedia
# > LiTV
- DOMAIN-SUFFIX,litv.tv,GlobalMedia
- DOMAIN,litvfreemobile-hichannel.cdn.hinet.net,GlobalMedia
# > My5
# USER-AGENT,My5*,GlobalMedia
- DOMAIN-SUFFIX,channel5.com,GlobalMedia
- DOMAIN-SUFFIX,my5.tv,GlobalMedia
- DOMAIN,d349g9zuie06uo.cloudfront.net,GlobalMedia
# > myTV SUPER
# USER-AGENT,mytv*,GlobalMedia
- DOMAIN-SUFFIX,mytvsuper.com,GlobalMedia
- DOMAIN-SUFFIX,tvb.com,GlobalMedia
# > Netflix
# USER-AGENT,Argo*,GlobalMedia
- DOMAIN-SUFFIX,fast.com,GlobalMedia
- DOMAIN-SUFFIX,netflix.com,GlobalMedia
- DOMAIN-SUFFIX,netflix.net,GlobalMedia
- DOMAIN-SUFFIX,nflxext.com,GlobalMedia
- DOMAIN-SUFFIX,nflximg.com,GlobalMedia
- DOMAIN-SUFFIX,nflximg.net,GlobalMedia
- DOMAIN-SUFFIX,nflxso.net,GlobalMedia
- DOMAIN-SUFFIX,nflxvideo.net,GlobalMedia
- DOMAIN-SUFFIX,netflixdnstest0.com,GlobalMedia
- DOMAIN-SUFFIX,netflixdnstest1.com,GlobalMedia
- DOMAIN-SUFFIX,netflixdnstest2.com,GlobalMedia
- DOMAIN-SUFFIX,netflixdnstest3.com,GlobalMedia
- DOMAIN-SUFFIX,netflixdnstest4.com,GlobalMedia
- DOMAIN-SUFFIX,netflixdnstest5.com,GlobalMedia
- DOMAIN-SUFFIX,netflixdnstest6.com,GlobalMedia
- DOMAIN-SUFFIX,netflixdnstest7.com,GlobalMedia
- DOMAIN-SUFFIX,netflixdnstest8.com,GlobalMedia
- DOMAIN-SUFFIX,netflixdnstest9.com,GlobalMedia
- IP-CIDR,23.246.0.0/18,GlobalMedia,no-resolve
- IP-CIDR,37.77.184.0/21,GlobalMedia,no-resolve
- IP-CIDR,45.57.0.0/17,GlobalMedia,no-resolve
- IP-CIDR,64.120.128.0/17,GlobalMedia,no-resolve
- IP-CIDR,66.197.128.0/17,GlobalMedia,no-resolve
- IP-CIDR,108.175.32.0/20,GlobalMedia,no-resolve
- IP-CIDR,192.173.64.0/18,GlobalMedia,no-resolve
- IP-CIDR,198.38.96.0/19,GlobalMedia,no-resolve
- IP-CIDR,198.45.48.0/20,GlobalMedia,no-resolve
# > niconico
# USER-AGENT,Niconico*,GlobalMedia
- DOMAIN-SUFFIX,dmc.nico,GlobalMedia
- DOMAIN-SUFFIX,nicovideo.jp,GlobalMedia
- DOMAIN-SUFFIX,nimg.jp,GlobalMedia
- DOMAIN-SUFFIX,socdm.com,GlobalMedia
# > PBS
# USER-AGENT,PBS*,GlobalMedia
- DOMAIN-SUFFIX,pbs.org,GlobalMedia
# > Pornhub
- DOMAIN-SUFFIX,phncdn.com,GlobalMedia
- DOMAIN-SUFFIX,phprcdn.com,GlobalMedia
- DOMAIN-SUFFIX,pornhub.com,GlobalMedia
- DOMAIN-SUFFIX,pornhubpremium.com,GlobalMedia
# > 台湾好
# USER-AGENT,TaiwanGood*,GlobalMedia
- DOMAIN-SUFFIX,skyking.com.tw,GlobalMedia
- DOMAIN,hamifans.emome.net,GlobalMedia
# > Twitch
- DOMAIN-SUFFIX,twitch.tv,GlobalMedia
- DOMAIN-SUFFIX,twitchcdn.net,GlobalMedia
- DOMAIN-SUFFIX,ttvnw.net,GlobalMedia
- DOMAIN-SUFFIX,jtvnw.net,GlobalMedia
# > ViuTV
# USER-AGENT,Viu*,GlobalMedia
# USER-AGENT,ViuTV*,GlobalMedia
- DOMAIN-SUFFIX,viu.com,GlobalMedia
- DOMAIN-SUFFIX,viu.tv,GlobalMedia
- DOMAIN,api.viu.now.com,GlobalMedia
- DOMAIN,d1k2us671qcoau.cloudfront.net,GlobalMedia
- DOMAIN,d2anahhhmp1ffz.cloudfront.net,GlobalMedia
- DOMAIN,dfp6rglgjqszk.cloudfront.net,GlobalMedia
# > YouTube
# USER-AGENT,com.google.ios.youtube*,GlobalMedia
# USER-AGENT,YouTube*,GlobalMedia
- DOMAIN-SUFFIX,googlevideo.com,GlobalMedia
- DOMAIN-SUFFIX,youtube.com,GlobalMedia
- DOMAIN,youtubei.googleapis.com,GlobalMedia

# (HKMTMedia)
# > 愛奇藝台灣站
- DOMAIN,cache.video.iqiyi.com,HKMTMedia
# > bilibili
- DOMAIN-SUFFIX,bilibili.com,HKMTMedia
- DOMAIN,upos-hz-mirrorakam.akamaized.net,HKMTMedia

# (DNS Cache Pollution Protection)
# > Google
- DOMAIN-SUFFIX,ampproject.org,PROXY
- DOMAIN-SUFFIX,appspot.com,PROXY
- DOMAIN-SUFFIX,blogger.com,PROXY
- DOMAIN-SUFFIX,getoutline.org,PROXY
- DOMAIN-SUFFIX,gvt0.com,PROXY
- DOMAIN-SUFFIX,gvt1.com,PROXY
- DOMAIN-SUFFIX,gvt3.com,PROXY
- DOMAIN-SUFFIX,xn--ngstr-lra8j.com,PROXY
- DOMAIN-KEYWORD,google,PROXY
- DOMAIN-KEYWORD,blogspot,PROXY
# > Microsoft
- DOMAIN-SUFFIX,onedrive.live.com,PROXY
- DOMAIN-SUFFIX,xboxlive.com,PROXY
# > Facebook
- DOMAIN-SUFFIX,v2raytech.com,PROXY
- DOMAIN-SUFFIX,hijk.art,PROXY
- DOMAIN-SUFFIX,hijk.pw,PROXY
- DOMAIN-SUFFIX,hijk.pp.ua,PROXY
- DOMAIN-SUFFIX,cdninstagram.com,PROXY
- DOMAIN-SUFFIX,fb.com,PROXY
- DOMAIN-SUFFIX,fb.me,PROXY
- DOMAIN-SUFFIX,fbaddins.com,PROXY
- DOMAIN-SUFFIX,fbcdn.net,PROXY
- DOMAIN-SUFFIX,fbsbx.com,PROXY
- DOMAIN-SUFFIX,fbworkmail.com,PROXY
- DOMAIN-SUFFIX,instagram.com,PROXY
- DOMAIN-SUFFIX,m.me,PROXY
- DOMAIN-SUFFIX,messenger.com,PROXY
- DOMAIN-SUFFIX,oculus.com,PROXY
- DOMAIN-SUFFIX,oculuscdn.com,PROXY
- DOMAIN-SUFFIX,rocksdb.org,PROXY
- DOMAIN-SUFFIX,whatsapp.com,PROXY
- DOMAIN-SUFFIX,whatsapp.net,PROXY
- DOMAIN-KEYWORD,facebook,PROXY
- IP-CIDR,3.123.36.126/32,PROXY,no-resolve
- IP-CIDR,35.157.215.84/32,PROXY,no-resolve
- IP-CIDR,35.157.217.255/32,PROXY,no-resolve
- IP-CIDR,52.58.209.134/32,PROXY,no-resolve
- IP-CIDR,54.93.124.31/32,PROXY,no-resolve
- IP-CIDR,54.162.243.80/32,PROXY,no-resolve
- IP-CIDR,54.173.34.141/32,PROXY,no-resolve
- IP-CIDR,54.235.23.242/32,PROXY,no-resolve
- IP-CIDR,169.45.248.118/32,PROXY,no-resolve
# > Twitter
- DOMAIN-SUFFIX,pscp.tv,PROXY
- DOMAIN-SUFFIX,periscope.tv,PROXY
- DOMAIN-SUFFIX,t.co,PROXY
- DOMAIN-SUFFIX,twimg.co,PROXY
- DOMAIN-SUFFIX,twimg.com,PROXY
- DOMAIN-SUFFIX,twitpic.com,PROXY
- DOMAIN-SUFFIX,vine.co,PROXY
- DOMAIN-KEYWORD,twitter,PROXY
# > Telegram
- DOMAIN-SUFFIX,t.me,PROXY
- DOMAIN-SUFFIX,tdesktop.com,PROXY
- DOMAIN-SUFFIX,telegra.ph,PROXY
- DOMAIN-SUFFIX,telegram.me,PROXY
- DOMAIN-SUFFIX,telegram.org,PROXY
- DOMAIN-SUFFIX,telesco.pe,PROXY
- IP-CIDR,91.108.4.0/22,PROXY,no-resolve
- IP-CIDR,91.108.8.0/22,PROXY,no-resolve
- IP-CIDR,91.108.12.0/22,PROXY,no-resolve
- IP-CIDR,91.108.16.0/22,PROXY,no-resolve
- IP-CIDR,91.108.56.0/22,PROXY,no-resolve
- IP-CIDR,149.154.160.0/20,PROXY,no-resolve
- IP-CIDR,2001:b28:f23d::/48,PROXY,no-resolve
- IP-CIDR,2001:b28:f23f::/48,PROXY,no-resolve
- IP-CIDR,2001:67c:4e8::/48,PROXY,no-resolve
# > Line
- DOMAIN-SUFFIX,line.me,PROXY
- DOMAIN-SUFFIX,line-apps.com,PROXY
- DOMAIN-SUFFIX,line-scdn.net,PROXY
- DOMAIN-SUFFIX,tlanyan.me,PROXY
- DOMAIN-SUFFIX,naver.jp,PROXY
- IP-CIDR,103.2.30.0/23,PROXY,no-resolve
- IP-CIDR,125.209.208.0/20,PROXY,no-resolve
- IP-CIDR,147.92.128.0/17,PROXY,no-resolve
- IP-CIDR,203.104.144.0/21,PROXY,no-resolve
# > Other
- DOMAIN-SUFFIX,4shared.com,PROXY
- DOMAIN-SUFFIX,520cc.cc,PROXY
- DOMAIN-SUFFIX,881903.com,PROXY
- DOMAIN-SUFFIX,9cache.com,PROXY
- DOMAIN-SUFFIX,9gag.com,PROXY
- DOMAIN-SUFFIX,abc.com,PROXY
- DOMAIN-SUFFIX,abc.net.au,PROXY
- DOMAIN-SUFFIX,abebooks.com,PROXY
- DOMAIN-SUFFIX,amazon.co.jp,PROXY
- DOMAIN-SUFFIX,apigee.com,PROXY
- DOMAIN-SUFFIX,apk-dl.com,PROXY
- DOMAIN-SUFFIX,apkfind.com,PROXY
- DOMAIN-SUFFIX,apkmirror.com,PROXY
- DOMAIN-SUFFIX,apkmonk.com,PROXY
- DOMAIN-SUFFIX,apkpure.com,PROXY
- DOMAIN-SUFFIX,aptoide.com,PROXY
- DOMAIN-SUFFIX,archive.is,PROXY
- DOMAIN-SUFFIX,archive.org,PROXY
- DOMAIN-SUFFIX,arte.tv,PROXY
- DOMAIN-SUFFIX,artstation.com,PROXY
- DOMAIN-SUFFIX,arukas.io,PROXY
- DOMAIN-SUFFIX,ask.com,PROXY
- DOMAIN-SUFFIX,avg.com,PROXY
- DOMAIN-SUFFIX,avgle.com,PROXY
- DOMAIN-SUFFIX,badoo.com,PROXY
- DOMAIN-SUFFIX,bandwagonhost.com,PROXY
- DOMAIN-SUFFIX,bbc.com,PROXY
- DOMAIN-SUFFIX,behance.net,PROXY
- DOMAIN-SUFFIX,bibox.com,PROXY
- DOMAIN-SUFFIX,biggo.com.tw,PROXY
- DOMAIN-SUFFIX,binance.com,PROXY
- DOMAIN-SUFFIX,bitcointalk.org,PROXY
- DOMAIN-SUFFIX,bitfinex.com,PROXY
- DOMAIN-SUFFIX,bitmex.com,PROXY
- DOMAIN-SUFFIX,bit-z.com,PROXY
- DOMAIN-SUFFIX,bloglovin.com,PROXY
- DOMAIN-SUFFIX,bloomberg.cn,PROXY
- DOMAIN-SUFFIX,bloomberg.com,PROXY
- DOMAIN-SUFFIX,blubrry.com,PROXY
- DOMAIN-SUFFIX,book.com.tw,PROXY
- DOMAIN-SUFFIX,booklive.jp,PROXY
- DOMAIN-SUFFIX,books.com.tw,PROXY
- DOMAIN-SUFFIX,boslife.net,PROXY
- DOMAIN-SUFFIX,box.com,PROXY
- DOMAIN-SUFFIX,businessinsider.com,PROXY
- DOMAIN-SUFFIX,bwh1.net,PROXY
- DOMAIN-SUFFIX,castbox.fm,PROXY
- DOMAIN-SUFFIX,cbc.ca,PROXY
- DOMAIN-SUFFIX,cdw.com,PROXY
- DOMAIN-SUFFIX,change.org,PROXY
- DOMAIN-SUFFIX,channelnewsasia.com,PROXY
- DOMAIN-SUFFIX,ck101.com,PROXY
- DOMAIN-SUFFIX,clarionproject.org,PROXY
- DOMAIN-SUFFIX,clyp.it,PROXY
- DOMAIN-SUFFIX,cna.com.tw,PROXY
- DOMAIN-SUFFIX,comparitech.com,PROXY
- DOMAIN-SUFFIX,conoha.jp,PROXY
- DOMAIN-SUFFIX,crucial.com,PROXY
- DOMAIN-SUFFIX,cts.com.tw,PROXY
- DOMAIN-SUFFIX,cw.com.tw,PROXY
- DOMAIN-SUFFIX,cyberctm.com,PROXY
- DOMAIN-SUFFIX,dailymotion.com,PROXY
- DOMAIN-SUFFIX,dailyview.tw,PROXY
- DOMAIN-SUFFIX,daum.net,PROXY
- DOMAIN-SUFFIX,daumcdn.net,PROXY
- DOMAIN-SUFFIX,dcard.tw,PROXY
- DOMAIN-SUFFIX,deepdiscount.com,PROXY
- DOMAIN-SUFFIX,depositphotos.com,PROXY
- DOMAIN-SUFFIX,deviantart.com,PROXY
- DOMAIN-SUFFIX,disconnect.me,PROXY
- DOMAIN-SUFFIX,discordapp.com,PROXY
- DOMAIN-SUFFIX,discordapp.net,PROXY
- DOMAIN-SUFFIX,disqus.com,PROXY
- DOMAIN-SUFFIX,dlercloud.com,PROXY
- DOMAIN-SUFFIX,dns2go.com,PROXY
- DOMAIN-SUFFIX,dowjones.com,PROXY
- DOMAIN-SUFFIX,dropbox.com,PROXY
- DOMAIN-SUFFIX,dropboxusercontent.com,PROXY
- DOMAIN-SUFFIX,duckduckgo.com,PROXY
- DOMAIN-SUFFIX,dw.com,PROXY
- DOMAIN-SUFFIX,dynu.com,PROXY
- DOMAIN-SUFFIX,earthcam.com,PROXY
- DOMAIN-SUFFIX,ebookservice.tw,PROXY
- DOMAIN-SUFFIX,economist.com,PROXY
- DOMAIN-SUFFIX,edgecastcdn.net,PROXY
- DOMAIN-SUFFIX,edu,PROXY
- DOMAIN-SUFFIX,elpais.com,PROXY
- DOMAIN-SUFFIX,enanyang.my,PROXY
- DOMAIN-SUFFIX,encyclopedia.com,PROXY
- DOMAIN-SUFFIX,esoir.be,PROXY
- DOMAIN-SUFFIX,etherscan.io,PROXY
- DOMAIN-SUFFIX,euronews.com,PROXY
- DOMAIN-SUFFIX,evozi.com,PROXY
- DOMAIN-SUFFIX,feedly.com,PROXY
- DOMAIN-SUFFIX,firech.at,PROXY
- DOMAIN-SUFFIX,flickr.com,PROXY
- DOMAIN-SUFFIX,flitto.com,PROXY
- DOMAIN-SUFFIX,foreignpolicy.com,PROXY
- DOMAIN-SUFFIX,freebrowser.org,PROXY
- DOMAIN-SUFFIX,freewechat.com,PROXY
- DOMAIN-SUFFIX,freeweibo.com,PROXY
- DOMAIN-SUFFIX,friday.tw,PROXY
- DOMAIN-SUFFIX,ftchinese.com,PROXY
- DOMAIN-SUFFIX,ftimg.net,PROXY
- DOMAIN-SUFFIX,gate.io,PROXY
- DOMAIN-SUFFIX,getlantern.org,PROXY
- DOMAIN-SUFFIX,getsync.com,PROXY
- DOMAIN-SUFFIX,globalvoices.org,PROXY
- DOMAIN-SUFFIX,goo.ne.jp,PROXY
- DOMAIN-SUFFIX,goodreads.com,PROXY
- DOMAIN-SUFFIX,gov,PROXY
- DOMAIN-SUFFIX,gov.tw,PROXY
- DOMAIN-SUFFIX,greatfire.org,PROXY
- DOMAIN-SUFFIX,gumroad.com,PROXY
- DOMAIN-SUFFIX,hbg.com,PROXY
- DOMAIN-SUFFIX,heroku.com,PROXY
- DOMAIN-SUFFIX,hightail.com,PROXY
- DOMAIN-SUFFIX,hk01.com,PROXY
- DOMAIN-SUFFIX,hkbf.org,PROXY
- DOMAIN-SUFFIX,hkbookcity.com,PROXY
- DOMAIN-SUFFIX,hkej.com,PROXY
- DOMAIN-SUFFIX,hket.com,PROXY
- DOMAIN-SUFFIX,hkgolden.com,PROXY
- DOMAIN-SUFFIX,hootsuite.com,PROXY
- DOMAIN-SUFFIX,hudson.org,PROXY
- DOMAIN-SUFFIX,hyread.com.tw,PROXY
- DOMAIN-SUFFIX,ibtimes.com,PROXY
- DOMAIN-SUFFIX,i-cable.com,PROXY
- DOMAIN-SUFFIX,icij.org,PROXY
- DOMAIN-SUFFIX,icoco.com,PROXY
- DOMAIN-SUFFIX,imgur.com,PROXY
- DOMAIN-SUFFIX,initiummall.com,PROXY
- DOMAIN-SUFFIX,insecam.org,PROXY
- DOMAIN-SUFFIX,ipfs.io,PROXY
- DOMAIN-SUFFIX,issuu.com,PROXY
- DOMAIN-SUFFIX,istockphoto.com,PROXY
- DOMAIN-SUFFIX,japantimes.co.jp,PROXY
- DOMAIN-SUFFIX,jiji.com,PROXY
- DOMAIN-SUFFIX,jinx.com,PROXY
- DOMAIN-SUFFIX,jkforum.net,PROXY
- DOMAIN-SUFFIX,joinmastodon.org,PROXY
- DOMAIN-SUFFIX,justmysocks.net,PROXY
- DOMAIN-SUFFIX,justpaste.it,PROXY
- DOMAIN-SUFFIX,kakao.com,PROXY
- DOMAIN-SUFFIX,kakaocorp.com,PROXY
- DOMAIN-SUFFIX,kik.com,PROXY
- DOMAIN-SUFFIX,kobo.com,PROXY
- DOMAIN-SUFFIX,kobobooks.com,PROXY
- DOMAIN-SUFFIX,kodingen.com,PROXY
- DOMAIN-SUFFIX,lemonde.fr,PROXY
- DOMAIN-SUFFIX,lepoint.fr,PROXY
- DOMAIN-SUFFIX,lihkg.com,PROXY
- DOMAIN-SUFFIX,listennotes.com,PROXY
- DOMAIN-SUFFIX,livestream.com,PROXY
- DOMAIN-SUFFIX,logmein.com,PROXY
- DOMAIN-SUFFIX,mail.ru,PROXY
- DOMAIN-SUFFIX,mailchimp.com,PROXY
- DOMAIN-SUFFIX,marc.info,PROXY
- DOMAIN-SUFFIX,matters.news,PROXY
- DOMAIN-SUFFIX,maying.co,PROXY
- DOMAIN-SUFFIX,medium.com,PROXY
- DOMAIN-SUFFIX,mega.nz,PROXY
- DOMAIN-SUFFIX,mil,PROXY
- DOMAIN-SUFFIX,mingpao.com,PROXY
- DOMAIN-SUFFIX,mobile01.com,PROXY
- DOMAIN-SUFFIX,myspace.com,PROXY
- DOMAIN-SUFFIX,myspacecdn.com,PROXY
- DOMAIN-SUFFIX,nanyang.com,PROXY
- DOMAIN-SUFFIX,naver.com,PROXY
- DOMAIN-SUFFIX,neowin.net,PROXY
- DOMAIN-SUFFIX,newstapa.org,PROXY
- DOMAIN-SUFFIX,nexitally.com,PROXY
- DOMAIN-SUFFIX,nhk.or.jp,PROXY
- DOMAIN-SUFFIX,nicovideo.jp,PROXY
- DOMAIN-SUFFIX,nii.ac.jp,PROXY
- DOMAIN-SUFFIX,nikkei.com,PROXY
- DOMAIN-SUFFIX,nofile.io,PROXY
- DOMAIN-SUFFIX,now.com,PROXY
- DOMAIN-SUFFIX,nrk.no,PROXY
- DOMAIN-SUFFIX,nyt.com,PROXY
- DOMAIN-SUFFIX,nytchina.com,PROXY
- DOMAIN-SUFFIX,nytcn.me,PROXY
- DOMAIN-SUFFIX,nytco.com,PROXY
- DOMAIN-SUFFIX,nytimes.com,PROXY
- DOMAIN-SUFFIX,nytimg.com,PROXY
- DOMAIN-SUFFIX,nytlog.com,PROXY
- DOMAIN-SUFFIX,nytstyle.com,PROXY
- DOMAIN-SUFFIX,ok.ru,PROXY
- DOMAIN-SUFFIX,okex.com,PROXY
- DOMAIN-SUFFIX,on.cc,PROXY
- DOMAIN-SUFFIX,orientaldaily.com.my,PROXY
- DOMAIN-SUFFIX,overcast.fm,PROXY
- DOMAIN-SUFFIX,paltalk.com,PROXY
- DOMAIN-SUFFIX,pao-pao.net,PROXY
- DOMAIN-SUFFIX,parsevideo.com,PROXY
- DOMAIN-SUFFIX,pbxes.com,PROXY
- DOMAIN-SUFFIX,pcdvd.com.tw,PROXY
- DOMAIN-SUFFIX,pchome.com.tw,PROXY
- DOMAIN-SUFFIX,pcloud.com,PROXY
- DOMAIN-SUFFIX,picacomic.com,PROXY
- DOMAIN-SUFFIX,pinimg.com,PROXY
- DOMAIN-SUFFIX,pixiv.net,PROXY
- DOMAIN-SUFFIX,player.fm,PROXY
- DOMAIN-SUFFIX,plurk.com,PROXY
- DOMAIN-SUFFIX,po18.tw,PROXY
- DOMAIN-SUFFIX,potato.im,PROXY
- DOMAIN-SUFFIX,potatso.com,PROXY
- DOMAIN-SUFFIX,prism-break.org,PROXY
- DOMAIN-SUFFIX,proxifier.com,PROXY
- DOMAIN-SUFFIX,pt.im,PROXY
- DOMAIN-SUFFIX,pts.org.tw,PROXY
- DOMAIN-SUFFIX,pubu.com.tw,PROXY
- DOMAIN-SUFFIX,pubu.tw,PROXY
- DOMAIN-SUFFIX,pureapk.com,PROXY
- DOMAIN-SUFFIX,quora.com,PROXY
- DOMAIN-SUFFIX,quoracdn.net,PROXY
- DOMAIN-SUFFIX,rakuten.co.jp,PROXY
- DOMAIN-SUFFIX,readingtimes.com.tw,PROXY
- DOMAIN-SUFFIX,readmoo.com,PROXY
- DOMAIN-SUFFIX,redbubble.com,PROXY
- DOMAIN-SUFFIX,reddit.com,PROXY
- DOMAIN-SUFFIX,redditmedia.com,PROXY
- DOMAIN-SUFFIX,redditstatic.com,PROXY
- DOMAIN-SUFFIX,resilio.com,PROXY
- DOMAIN-SUFFIX,reuters.com,PROXY
- DOMAIN-SUFFIX,reutersmedia.net,PROXY
- DOMAIN-SUFFIX,rfi.fr,PROXY
- DOMAIN-SUFFIX,rixcloud.com,PROXY
- DOMAIN-SUFFIX,roadshow.hk,PROXY
- DOMAIN-SUFFIX,scmp.com,PROXY
- DOMAIN-SUFFIX,scribd.com,PROXY
- DOMAIN-SUFFIX,seatguru.com,PROXY
- DOMAIN-SUFFIX,shadowsocks.org,PROXY
- DOMAIN-SUFFIX,shopee.tw,PROXY
- DOMAIN-SUFFIX,slideshare.net,PROXY
- DOMAIN-SUFFIX,softfamous.com,PROXY
- DOMAIN-SUFFIX,soundcloud.com,PROXY
- DOMAIN-SUFFIX,ssrcloud.org,PROXY
- DOMAIN-SUFFIX,startpage.com,PROXY
- DOMAIN-SUFFIX,steamcommunity.com,PROXY
- DOMAIN-SUFFIX,steemit.com,PROXY
- DOMAIN-SUFFIX,steemitwallet.com,PROXY
- DOMAIN-SUFFIX,t66y.com,PROXY
- DOMAIN-SUFFIX,tapatalk.com,PROXY
- DOMAIN-SUFFIX,teco-hk.org,PROXY
- DOMAIN-SUFFIX,teco-mo.org,PROXY
- DOMAIN-SUFFIX,teddysun.com,PROXY
- DOMAIN-SUFFIX,textnow.me,PROXY
- DOMAIN-SUFFIX,theguardian.com,PROXY
- DOMAIN-SUFFIX,theinitium.com,PROXY
- DOMAIN-SUFFIX,thetvdb.com,PROXY
- DOMAIN-SUFFIX,tineye.com,PROXY
- DOMAIN-SUFFIX,torproject.org,PROXY
- DOMAIN-SUFFIX,tumblr.com,PROXY
- DOMAIN-SUFFIX,turbobit.net,PROXY
- DOMAIN-SUFFIX,tutanota.com,PROXY
- DOMAIN-SUFFIX,tvboxnow.com,PROXY
- DOMAIN-SUFFIX,udn.com,PROXY
- DOMAIN-SUFFIX,unseen.is,PROXY
- DOMAIN-SUFFIX,upmedia.mg,PROXY
- DOMAIN-SUFFIX,uptodown.com,PROXY
- DOMAIN-SUFFIX,urbandictionary.com,PROXY
- DOMAIN-SUFFIX,ustream.tv,PROXY
- DOMAIN-SUFFIX,uwants.com,PROXY
- DOMAIN-SUFFIX,v2ray.com,PROXY
- DOMAIN-SUFFIX,viber.com,PROXY
- DOMAIN-SUFFIX,videopress.com,PROXY
- DOMAIN-SUFFIX,vimeo.com,PROXY
- DOMAIN-SUFFIX,voachinese.com,PROXY
- DOMAIN-SUFFIX,voanews.com,PROXY
- DOMAIN-SUFFIX,voxer.com,PROXY
- DOMAIN-SUFFIX,vzw.com,PROXY
- DOMAIN-SUFFIX,w3schools.com,PROXY
- DOMAIN-SUFFIX,washingtonpost.com,PROXY
- DOMAIN-SUFFIX,wattpad.com,PROXY
- DOMAIN-SUFFIX,whoer.net,PROXY
- DOMAIN-SUFFIX,wikimapia.org,PROXY
- DOMAIN-SUFFIX,wikimedia.org,PROXY
- DOMAIN-SUFFIX,wikipedia.org,PROXY
- DOMAIN-SUFFIX,wikiquote.org,PROXY
- DOMAIN-SUFFIX,wikiwand.com,PROXY
- DOMAIN-SUFFIX,winudf.com,PROXY
- DOMAIN-SUFFIX,wire.com,PROXY
- DOMAIN-SUFFIX,wordpress.com,PROXY
- DOMAIN-SUFFIX,workflow.is,PROXY
- DOMAIN-SUFFIX,worldcat.org,PROXY
- DOMAIN-SUFFIX,wsj.com,PROXY
- DOMAIN-SUFFIX,wsj.net,PROXY
- DOMAIN-SUFFIX,xhamster.com,PROXY
- DOMAIN-SUFFIX,xn--90wwvt03e.com,PROXY
- DOMAIN-SUFFIX,xn--i2ru8q2qg.com,PROXY
- DOMAIN-SUFFIX,xnxx.com,PROXY
- DOMAIN-SUFFIX,xvideos.com,PROXY
- DOMAIN-SUFFIX,yahoo.com,PROXY
- DOMAIN-SUFFIX,yandex.ru,PROXY
- DOMAIN-SUFFIX,ycombinator.com,PROXY
- DOMAIN-SUFFIX,yesasia.com,PROXY
- DOMAIN-SUFFIX,yes-news.com,PROXY
- DOMAIN-SUFFIX,yomiuri.co.jp,PROXY
- DOMAIN-SUFFIX,you-get.org,PROXY
- DOMAIN-SUFFIX,zaobao.com,PROXY
- DOMAIN-SUFFIX,zb.com,PROXY
- DOMAIN-SUFFIX,zello.com,PROXY
- DOMAIN-SUFFIX,zeronet.io,PROXY
- DOMAIN-SUFFIX,zoom.us,PROXY
- DOMAIN-KEYWORD,github,PROXY
- DOMAIN-KEYWORD,jav,PROXY
- DOMAIN-KEYWORD,pinterest,PROXY
- DOMAIN-KEYWORD,porn,PROXY
- DOMAIN-KEYWORD,wikileaks,PROXY

# (Region-Restricted Access Denied)
- DOMAIN-SUFFIX,apartmentratings.com,PROXY
- DOMAIN-SUFFIX,apartments.com,PROXY
- DOMAIN-SUFFIX,bankmobilevibe.com,PROXY
- DOMAIN-SUFFIX,bing.com,PROXY
- DOMAIN-SUFFIX,booktopia.com.au,PROXY
- DOMAIN-SUFFIX,cccat.io,PROXY
- DOMAIN-SUFFIX,centauro.com.br,PROXY
- DOMAIN-SUFFIX,clearsurance.com,PROXY
- DOMAIN-SUFFIX,costco.com,PROXY
- DOMAIN-SUFFIX,crackle.com,PROXY
- DOMAIN-SUFFIX,depositphotos.cn,PROXY
- DOMAIN-SUFFIX,dish.com,PROXY
- DOMAIN-SUFFIX,dmm.co.jp,PROXY
- DOMAIN-SUFFIX,dmm.com,PROXY
- DOMAIN-SUFFIX,dnvod.tv,PROXY
- DOMAIN-SUFFIX,esurance.com,PROXY
- DOMAIN-SUFFIX,extmatrix.com,PROXY
- DOMAIN-SUFFIX,fastpic.ru,PROXY
- DOMAIN-SUFFIX,flipboard.com,PROXY
- DOMAIN-SUFFIX,fnac.be,PROXY
- DOMAIN-SUFFIX,fnac.com,PROXY
- DOMAIN-SUFFIX,funkyimg.com,PROXY
- DOMAIN-SUFFIX,fxnetworks.com,PROXY
- DOMAIN-SUFFIX,gettyimages.com,PROXY
- DOMAIN-SUFFIX,go.com,PROXY
- DOMAIN-SUFFIX,here.com,PROXY
- DOMAIN-SUFFIX,jcpenney.com,PROXY
- DOMAIN-SUFFIX,jiehua.tv,PROXY
- DOMAIN-SUFFIX,mailfence.com,PROXY
- DOMAIN-SUFFIX,nationwide.com,PROXY
- DOMAIN-SUFFIX,nbc.com,PROXY
- DOMAIN-SUFFIX,nexon.com,PROXY
- DOMAIN-SUFFIX,nordstrom.com,PROXY
- DOMAIN-SUFFIX,nordstromimage.com,PROXY
- DOMAIN-SUFFIX,nordstromrack.com,PROXY
- DOMAIN-SUFFIX,superpages.com,PROXY
- DOMAIN-SUFFIX,target.com,PROXY
- DOMAIN-SUFFIX,thinkgeek.com,PROXY
- DOMAIN-SUFFIX,tracfone.com,PROXY
- DOMAIN-SUFFIX,unity3d.com,PROXY
- DOMAIN-SUFFIX,uploader.jp,PROXY
- DOMAIN-SUFFIX,vevo.com,PROXY
- DOMAIN-SUFFIX,viu.tv,PROXY
- DOMAIN-SUFFIX,vk.com,PROXY
- DOMAIN-SUFFIX,vsco.co,PROXY
- DOMAIN-SUFFIX,xfinity.com,PROXY
- DOMAIN-SUFFIX,zattoo.com,PROXY
# USER-AGENT,Roam*,PROXY

# (The Most Popular Sites)
# > Apple
# >> TestFlight
- DOMAIN,testflight.apple.com,PROXY
# >> Apple URL Shortener
- DOMAIN-SUFFIX,appsto.re,PROXY
# >> iBooks Store download
- DOMAIN,books.itunes.apple.com,PROXY
# >> iTunes Store Moveis Trailers
- DOMAIN,hls.itunes.apple.com,PROXY
# >> App Store Preview
- DOMAIN,apps.apple.com,PROXY
- DOMAIN,itunes.apple.com,PROXY
# >> Spotlight
- DOMAIN,api-glb-sea.smoot.apple.com,PROXY
# >> Dictionary
- DOMAIN,lookup-api.apple.com,PROXY
# > Google
- DOMAIN-SUFFIX,abc.xyz,PROXY
- DOMAIN-SUFFIX,android.com,PROXY
- DOMAIN-SUFFIX,androidify.com,PROXY
- DOMAIN-SUFFIX,dialogflow.com,PROXY
- DOMAIN-SUFFIX,autodraw.com,PROXY
- DOMAIN-SUFFIX,capitalg.com,PROXY
- DOMAIN-SUFFIX,certificate-transparency.org,PROXY
- DOMAIN-SUFFIX,chrome.com,PROXY
- DOMAIN-SUFFIX,chromeexperiments.com,PROXY
- DOMAIN-SUFFIX,chromestatus.com,PROXY
- DOMAIN-SUFFIX,chromium.org,PROXY
- DOMAIN-SUFFIX,creativelab5.com,PROXY
- DOMAIN-SUFFIX,debug.com,PROXY
- DOMAIN-SUFFIX,deepmind.com,PROXY
- DOMAIN-SUFFIX,firebaseio.com,PROXY
- DOMAIN-SUFFIX,getmdl.io,PROXY
- DOMAIN-SUFFIX,ggpht.com,PROXY
- DOMAIN-SUFFIX,gmail.com,PROXY
- DOMAIN-SUFFIX,gmodules.com,PROXY
- DOMAIN-SUFFIX,godoc.org,PROXY
- DOMAIN-SUFFIX,golang.org,PROXY
- DOMAIN-SUFFIX,gstatic.com,PROXY
- DOMAIN-SUFFIX,gv.com,PROXY
- DOMAIN-SUFFIX,gwtproject.org,PROXY
- DOMAIN-SUFFIX,itasoftware.com,PROXY
- DOMAIN-SUFFIX,madewithcode.com,PROXY
- DOMAIN-SUFFIX,material.io,PROXY
- DOMAIN-SUFFIX,polymer-project.org,PROXY
- DOMAIN-SUFFIX,admin.recaptcha.net,PROXY
- DOMAIN-SUFFIX,recaptcha.net,PROXY
- DOMAIN-SUFFIX,shattered.io,PROXY
- DOMAIN-SUFFIX,synergyse.com,PROXY
- DOMAIN-SUFFIX,telephony.goog,PROXY
- DOMAIN-SUFFIX,tensorflow.org,PROXY
- DOMAIN-SUFFIX,tfhub.dev,PROXY
- DOMAIN-SUFFIX,tiltbrush.com,PROXY
- DOMAIN-SUFFIX,waveprotocol.org,PROXY
- DOMAIN-SUFFIX,waymo.com,PROXY
- DOMAIN-SUFFIX,webmproject.org,PROXY
- DOMAIN-SUFFIX,webrtc.org,PROXY
- DOMAIN-SUFFIX,whatbrowser.org,PROXY
- DOMAIN-SUFFIX,widevine.com,PROXY
- DOMAIN-SUFFIX,x.company,PROXY
- DOMAIN-SUFFIX,youtu.be,PROXY
- DOMAIN-SUFFIX,yt.be,PROXY
- DOMAIN-SUFFIX,ytimg.com,PROXY
# > Microsoft
# >> Microsoft OneDrive
- DOMAIN-SUFFIX,1drv.com,PROXY
- DOMAIN-SUFFIX,1drv.ms,PROXY
- DOMAIN-SUFFIX,blob.core.windows.net,PROXY
- DOMAIN-SUFFIX,livefilestore.com,PROXY
- DOMAIN-SUFFIX,onedrive.com,PROXY
- DOMAIN-SUFFIX,storage.live.com,PROXY
- DOMAIN-SUFFIX,storage.msn.com,PROXY
- DOMAIN,oneclient.sfx.ms,PROXY
# > Other
- DOMAIN-SUFFIX,0rz.tw,PROXY
- DOMAIN-SUFFIX,4bluestones.biz,PROXY
- DOMAIN-SUFFIX,9bis.net,PROXY
- DOMAIN-SUFFIX,allconnected.co,PROXY
- DOMAIN-SUFFIX,aol.com,PROXY
- DOMAIN-SUFFIX,bcc.com.tw,PROXY
- DOMAIN-SUFFIX,bit.ly,PROXY
- DOMAIN-SUFFIX,bitshare.com,PROXY
- DOMAIN-SUFFIX,blog.jp,PROXY
- DOMAIN-SUFFIX,blogimg.jp,PROXY
- DOMAIN-SUFFIX,blogtd.org,PROXY
- DOMAIN-SUFFIX,broadcast.co.nz,PROXY
- DOMAIN-SUFFIX,camfrog.com,PROXY
- DOMAIN-SUFFIX,cfos.de,PROXY
- DOMAIN-SUFFIX,citypopulation.de,PROXY
- DOMAIN-SUFFIX,cloudfront.net,PROXY
- DOMAIN-SUFFIX,ctitv.com.tw,PROXY
- DOMAIN-SUFFIX,cuhk.edu.hk,PROXY
- DOMAIN-SUFFIX,cusu.hk,PROXY
- DOMAIN-SUFFIX,discord.gg,PROXY
- DOMAIN-SUFFIX,discuss.com.hk,PROXY
- DOMAIN-SUFFIX,dropboxapi.com,PROXY
- DOMAIN-SUFFIX,duolingo.cn,PROXY
- DOMAIN-SUFFIX,edditstatic.com,PROXY
- DOMAIN-SUFFIX,flickriver.com,PROXY
- DOMAIN-SUFFIX,focustaiwan.tw,PROXY
- DOMAIN-SUFFIX,free.fr,PROXY
- DOMAIN-SUFFIX,gigacircle.com,PROXY
- DOMAIN-SUFFIX,hk-pub.com,PROXY
- DOMAIN-SUFFIX,hosting.co.uk,PROXY
- DOMAIN-SUFFIX,hwcdn.net,PROXY
- DOMAIN-SUFFIX,ifixit.com,PROXY
- DOMAIN-SUFFIX,iphone4hongkong.com,PROXY
- DOMAIN-SUFFIX,iphonetaiwan.org,PROXY
- DOMAIN-SUFFIX,iptvbin.com,PROXY
- DOMAIN-SUFFIX,linksalpha.com,PROXY
- DOMAIN-SUFFIX,manyvids.com,PROXY
- DOMAIN-SUFFIX,myactimes.com,PROXY
- DOMAIN-SUFFIX,newsblur.com,PROXY
- DOMAIN-SUFFIX,now.im,PROXY
- DOMAIN-SUFFIX,nowe.com,PROXY
- DOMAIN-SUFFIX,redditlist.com,PROXY
- DOMAIN-SUFFIX,s3.amazonaws.com,PROXY
- DOMAIN-SUFFIX,signal.org,PROXY
- DOMAIN-SUFFIX,smartmailcloud.com,PROXY
- DOMAIN-SUFFIX,sparknotes.com,PROXY
- DOMAIN-SUFFIX,streetvoice.com,PROXY
- DOMAIN-SUFFIX,supertop.co,PROXY
- DOMAIN-SUFFIX,tv.com,PROXY
- DOMAIN-SUFFIX,typepad.com,PROXY
- DOMAIN-SUFFIX,udnbkk.com,PROXY
- DOMAIN-SUFFIX,urbanairship.com,PROXY
- DOMAIN-SUFFIX,whispersystems.org,PROXY
- DOMAIN-SUFFIX,wikia.com,PROXY
- DOMAIN-SUFFIX,wn.com,PROXY
- DOMAIN-SUFFIX,wolframalpha.com,PROXY
- DOMAIN-SUFFIX,x-art.com,PROXY
- DOMAIN-SUFFIX,yimg.com,PROXY
- DOMAIN,api.steampowered.com,PROXY
- DOMAIN,store.steampowered.com,PROXY

# China Area Network
# > 360
- DOMAIN-SUFFIX,qhres.com,DIRECT
- DOMAIN-SUFFIX,qhimg.com,DIRECT
# > Akamai
- DOMAIN-SUFFIX,akadns.net,DIRECT
# - DOMAIN-SUFFIX,akamai.net,DIRECT
# - DOMAIN-SUFFIX,akamaiedge.net,DIRECT
# - DOMAIN-SUFFIX,akamaihd.net,DIRECT
# - DOMAIN-SUFFIX,akamaistream.net,DIRECT
# - DOMAIN-SUFFIX,akamaized.net,DIRECT
# > Alibaba
# USER-AGENT,%E4%BC%98%E9%85%B7*,DIRECT
- DOMAIN-SUFFIX,alibaba.com,DIRECT
- DOMAIN-SUFFIX,alicdn.com,DIRECT
- DOMAIN-SUFFIX,alikunlun.com,DIRECT
- DOMAIN-SUFFIX,alipay.com,DIRECT
- DOMAIN-SUFFIX,amap.com,DIRECT
- DOMAIN-SUFFIX,autonavi.com,DIRECT
- DOMAIN-SUFFIX,dingtalk.com,DIRECT
- DOMAIN-SUFFIX,mxhichina.com,DIRECT
- DOMAIN-SUFFIX,soku.com,DIRECT
- DOMAIN-SUFFIX,taobao.com,DIRECT
- DOMAIN-SUFFIX,tmall.com,DIRECT
- DOMAIN-SUFFIX,tmall.hk,DIRECT
- DOMAIN-SUFFIX,ykimg.com,DIRECT
- DOMAIN-SUFFIX,youku.com,DIRECT
- DOMAIN-SUFFIX,xiami.com,DIRECT
- DOMAIN-SUFFIX,xiami.net,DIRECT
# > Baidu
- DOMAIN-SUFFIX,baidu.com,DIRECT
- DOMAIN-SUFFIX,baidubcr.com,DIRECT
- DOMAIN-SUFFIX,bdstatic.com,DIRECT
- DOMAIN-SUFFIX,yunjiasu-cdn.net,DIRECT
# > bilibili
- DOMAIN-SUFFIX,acgvideo.com,DIRECT
- DOMAIN-SUFFIX,biliapi.com,DIRECT
- DOMAIN-SUFFIX,biliapi.net,DIRECT
- DOMAIN-SUFFIX,bilibili.com,DIRECT
- DOMAIN-SUFFIX,bilibili.tv,DIRECT
- DOMAIN-SUFFIX,hdslb.com,DIRECT
# > Blizzard
- DOMAIN-SUFFIX,blizzard.com,DIRECT
- DOMAIN-SUFFIX,battle.net,DIRECT
- DOMAIN,blzddist1-a.akamaihd.net,DIRECT
# > ByteDance
- DOMAIN-SUFFIX,feiliao.com,DIRECT
- DOMAIN-SUFFIX,pstatp.com,DIRECT
- DOMAIN-SUFFIX,snssdk.com,DIRECT
- DOMAIN-SUFFIX,iesdouyin.com,DIRECT
- DOMAIN-SUFFIX,toutiao.com,DIRECT
# > CCTV
- DOMAIN-SUFFIX,cctv.com,DIRECT
- DOMAIN-SUFFIX,cctvpic.com,DIRECT
- DOMAIN-SUFFIX,livechina.com,DIRECT
# > DiDi
- DOMAIN-SUFFIX,didialift.com,DIRECT
- DOMAIN-SUFFIX,didiglobal.com,DIRECT
- DOMAIN-SUFFIX,udache.com,DIRECT
# > 蛋蛋赞
- DOMAIN-SUFFIX,343480.com,DIRECT
- DOMAIN-SUFFIX,baduziyuan.com,DIRECT
- DOMAIN-SUFFIX,com-hs-hkdy.com,DIRECT
- DOMAIN-SUFFIX,czybjz.com,DIRECT
- DOMAIN-SUFFIX,dandanzan.com,DIRECT
- DOMAIN-SUFFIX,fjhps.com,DIRECT
- DOMAIN-SUFFIX,kuyunbo.club,DIRECT
# > ChinaNet
- DOMAIN-SUFFIX,21cn.com,DIRECT
# > HunanTV
- DOMAIN-SUFFIX,hitv.com,DIRECT
- DOMAIN-SUFFIX,mgtv.com,DIRECT
# > iQiyi
- DOMAIN-SUFFIX,iqiyi.com,DIRECT
- DOMAIN-SUFFIX,iqiyipic.com,DIRECT
- DOMAIN-SUFFIX,71.am.com,DIRECT
# > JD
- DOMAIN-SUFFIX,jd.com,DIRECT
- DOMAIN-SUFFIX,jd.hk,DIRECT
- DOMAIN-SUFFIX,jdpay.com,DIRECT
- DOMAIN-SUFFIX,360buyimg.com,DIRECT
# > Kingsoft
- DOMAIN-SUFFIX,iciba.com,DIRECT
- DOMAIN-SUFFIX,ksosoft.com,DIRECT
# > Meitu
- DOMAIN-SUFFIX,meitu.com,DIRECT
- DOMAIN-SUFFIX,meitudata.com,DIRECT
- DOMAIN-SUFFIX,meitustat.com,DIRECT
- DOMAIN-SUFFIX,meipai.com,DIRECT
# > MI
- DOMAIN-SUFFIX,duokan.com,DIRECT
- DOMAIN-SUFFIX,mi-img.com,DIRECT
- DOMAIN-SUFFIX,miui.com,DIRECT
- DOMAIN-SUFFIX,miwifi.com,DIRECT
- DOMAIN-SUFFIX,xiaomi.com,DIRECT
# > Microsoft
- DOMAIN-SUFFIX,microsoft.com,DIRECT
- DOMAIN-SUFFIX,msecnd.net,DIRECT
- DOMAIN-SUFFIX,office365.com,DIRECT
- DOMAIN-SUFFIX,outlook.com,DIRECT
- DOMAIN-SUFFIX,s-microsoft.com,DIRECT
- DOMAIN-SUFFIX,visualstudio.com,DIRECT
- DOMAIN-SUFFIX,windows.com,DIRECT
- DOMAIN-SUFFIX,windowsupdate.com,DIRECT
- DOMAIN,officecdn-microsoft-com.akamaized.net,DIRECT
# > NetEase
# USER-AGENT,NeteaseMusic*,DIRECT
# USER-AGENT,%E7%BD%91%E6%98%93%E4%BA%91%E9%9F%B3%E4%B9%90*,DIRECT
- DOMAIN-SUFFIX,163.com,DIRECT
- DOMAIN-SUFFIX,126.net,DIRECT
- DOMAIN-SUFFIX,127.net,DIRECT
- DOMAIN-SUFFIX,163yun.com,DIRECT
- DOMAIN-SUFFIX,lofter.com,DIRECT
- DOMAIN-SUFFIX,netease.com,DIRECT
- DOMAIN-SUFFIX,ydstatic.com,DIRECT
# > Sina
- DOMAIN-SUFFIX,sina.com,DIRECT
- DOMAIN-SUFFIX,weibo.com,DIRECT
- DOMAIN-SUFFIX,weibocdn.com,DIRECT
# > Sohu
- DOMAIN-SUFFIX,sohu.com,DIRECT
- DOMAIN-SUFFIX,sohucs.com,DIRECT
- DOMAIN-SUFFIX,sohu-inc.com,DIRECT
- DOMAIN-SUFFIX,v-56.com,DIRECT
# > Sogo
- DOMAIN-SUFFIX,sogo.com,DIRECT
- DOMAIN-SUFFIX,sogou.com,DIRECT
- DOMAIN-SUFFIX,sogoucdn.com,DIRECT
# > Steam
- DOMAIN-SUFFIX,steampowered.com,DIRECT
- DOMAIN-SUFFIX,steam-chat.com,DIRECT
- DOMAIN-SUFFIX,steamgames.com,DIRECT
- DOMAIN-SUFFIX,steamusercontent.com,DIRECT
- DOMAIN-SUFFIX,steamcontent.com,DIRECT
- DOMAIN-SUFFIX,steamstatic.com,DIRECT
- DOMAIN-SUFFIX,steamcdn-a.akamaihd.net,DIRECT
- DOMAIN-SUFFIX,steamstat.us,DIRECT
# > Tencent
# USER-AGENT,MicroMessenger%20Client,DIRECT
# USER-AGENT,WeChat*,DIRECT
# USER-AGENT,%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1*,DIRECT
- DOMAIN-SUFFIX,gtimg.com,DIRECT
- DOMAIN-SUFFIX,idqqimg.com,DIRECT
- DOMAIN-SUFFIX,igamecj.com,DIRECT
- DOMAIN-SUFFIX,myapp.com,DIRECT
- DOMAIN-SUFFIX,myqcloud.com,DIRECT
- DOMAIN-SUFFIX,qq.com,DIRECT
- DOMAIN-SUFFIX,servicewechat.com,DIRECT
- DOMAIN-SUFFIX,tencent.com,DIRECT
- DOMAIN-SUFFIX,tencent-cloud.net,DIRECT
- DOMAIN-SUFFIX,tenpay.com,DIRECT
- DOMAIN,file-igamecj.akamaized.net,DIRECT
- IP-CIDR,182.254.116.0/24,DIRECT
- IP-CIDR,203.205.252.0/23,DIRECT
- IP-CIDR,203.205.254.0/23,DIRECT
# > YYeTs
# USER-AGENT,YYeTs*,DIRECT
- DOMAIN-SUFFIX,jstucdn.com,DIRECT
- DOMAIN-SUFFIX,zimuzu.io,DIRECT
- DOMAIN-SUFFIX,zimuzu.tv,DIRECT
- DOMAIN-SUFFIX,zmz2019.com,DIRECT
- DOMAIN-SUFFIX,zmzapi.com,DIRECT
- DOMAIN-SUFFIX,zmzapi.net,DIRECT
- DOMAIN-SUFFIX,zmzfile.com,DIRECT
# > Content Delivery Network
- DOMAIN-SUFFIX,ccgslb.com,DIRECT
- DOMAIN-SUFFIX,ccgslb.net,DIRECT
- DOMAIN-SUFFIX,chinanetcenter.com,DIRECT
- DOMAIN-SUFFIX,meixincdn.com,DIRECT
- DOMAIN-SUFFIX,ourdvs.com,DIRECT
- DOMAIN-SUFFIX,staticdn.net,DIRECT
- DOMAIN-SUFFIX,wangsu.com,DIRECT
# > IP Query
- DOMAIN-SUFFIX,ipip.net,DIRECT
- DOMAIN-SUFFIX,ip.la,DIRECT
- DOMAIN-SUFFIX,ip-cdn.com,DIRECT
- DOMAIN-SUFFIX,ipv6-test.com,DIRECT
- DOMAIN-SUFFIX,test-ipv6.com,DIRECT
- DOMAIN-SUFFIX,whatismyip.com,DIRECT
# > Speed Test
# - DOMAIN-SUFFIX,speedtest.net,DIRECT
- DOMAIN-SUFFIX,netspeedtestmaster.com,DIRECT
- DOMAIN,speedtest.macpaw.com,DIRECT
# > Private Tracker
- DOMAIN-SUFFIX,awesome-hd.me,DIRECT
- DOMAIN-SUFFIX,broadcasthe.net,DIRECT
- DOMAIN-SUFFIX,chdbits.co,DIRECT
- DOMAIN-SUFFIX,classix-unlimited.co.uk,DIRECT
- DOMAIN-SUFFIX,empornium.me,DIRECT
- DOMAIN-SUFFIX,gazellegames.net,DIRECT
- DOMAIN-SUFFIX,hdchina.org,DIRECT
- DOMAIN-SUFFIX,hdsky.me,DIRECT
- DOMAIN-SUFFIX,icetorrent.org,DIRECT
- DOMAIN-SUFFIX,jpopsuki.eu,DIRECT
- DOMAIN-SUFFIX,keepfrds.com,DIRECT
- DOMAIN-SUFFIX,madsrevolution.net,DIRECT
- DOMAIN-SUFFIX,m-team.cc,DIRECT
- DOMAIN-SUFFIX,nanyangpt.com,DIRECT
- DOMAIN-SUFFIX,ncore.cc,DIRECT
- DOMAIN-SUFFIX,open.cd,DIRECT
- DOMAIN-SUFFIX,ourbits.club,DIRECT
- DOMAIN-SUFFIX,passthepopcorn.me,DIRECT
- DOMAIN-SUFFIX,privatehd.to,DIRECT
- DOMAIN-SUFFIX,redacted.ch,DIRECT
- DOMAIN-SUFFIX,springsunday.net,DIRECT
- DOMAIN-SUFFIX,tjupt.org,DIRECT
- DOMAIN-SUFFIX,totheglory.im,DIRECT
# > Scholar
- DOMAIN-SUFFIX,acm.org,DIRECT
- DOMAIN-SUFFIX,acs.org,DIRECT
- DOMAIN-SUFFIX,aip.org,DIRECT
- DOMAIN-SUFFIX,ams.org,DIRECT
- DOMAIN-SUFFIX,annualreviews.org,DIRECT
- DOMAIN-SUFFIX,aps.org,DIRECT
- DOMAIN-SUFFIX,ascelibrary.org,DIRECT
- DOMAIN-SUFFIX,asm.org,DIRECT
- DOMAIN-SUFFIX,asme.org,DIRECT
- DOMAIN-SUFFIX,astm.org,DIRECT
- DOMAIN-SUFFIX,bmj.com,DIRECT
- DOMAIN-SUFFIX,cambridge.org,DIRECT
- DOMAIN-SUFFIX,cas.org,DIRECT
- DOMAIN-SUFFIX,clarivate.com,DIRECT
- DOMAIN-SUFFIX,ebscohost.com,DIRECT
- DOMAIN-SUFFIX,emerald.com,DIRECT
- DOMAIN-SUFFIX,engineeringvillage.com,DIRECT
- DOMAIN-SUFFIX,icevirtuallibrary.com,DIRECT
- DOMAIN-SUFFIX,ieee.org,DIRECT
- DOMAIN-SUFFIX,imf.org,DIRECT
- DOMAIN-SUFFIX,iop.org,DIRECT
- DOMAIN-SUFFIX,jamanetwork.com,DIRECT
- DOMAIN-SUFFIX,jhu.edu,DIRECT
- DOMAIN-SUFFIX,jstor.org,DIRECT
- DOMAIN-SUFFIX,karger.com,DIRECT
- DOMAIN-SUFFIX,libguides.com,DIRECT
- DOMAIN-SUFFIX,madsrevolution.net,DIRECT
- DOMAIN-SUFFIX,mpg.de,DIRECT
- DOMAIN-SUFFIX,myilibrary.com,DIRECT
- DOMAIN-SUFFIX,nature.com,DIRECT
- DOMAIN-SUFFIX,oecd-ilibrary.org,DIRECT
- DOMAIN-SUFFIX,osapublishing.org,DIRECT
- DOMAIN-SUFFIX,oup.com,DIRECT
- DOMAIN-SUFFIX,ovid.com,DIRECT
- DOMAIN-SUFFIX,oxfordartonline.com,DIRECT
- DOMAIN-SUFFIX,oxfordbibliographies.com,DIRECT
- DOMAIN-SUFFIX,oxfordmusiconline.com,DIRECT
- DOMAIN-SUFFIX,pnas.org,DIRECT
- DOMAIN-SUFFIX,proquest.com,DIRECT
- DOMAIN-SUFFIX,rsc.org,DIRECT
- DOMAIN-SUFFIX,sagepub.com,DIRECT
- DOMAIN-SUFFIX,sciencedirect.com,DIRECT
- DOMAIN-SUFFIX,sciencemag.org,DIRECT
- DOMAIN-SUFFIX,scopus.com,DIRECT
- DOMAIN-SUFFIX,siam.org,DIRECT
- DOMAIN-SUFFIX,spiedigitallibrary.org,DIRECT
- DOMAIN-SUFFIX,springer.com,DIRECT
- DOMAIN-SUFFIX,springerlink.com,DIRECT
- DOMAIN-SUFFIX,tandfonline.com,DIRECT
- DOMAIN-SUFFIX,un.org,DIRECT
- DOMAIN-SUFFIX,uni-bielefeld.de,DIRECT
- DOMAIN-SUFFIX,webofknowledge.com,DIRECT
- DOMAIN-SUFFIX,westlaw.com,DIRECT
- DOMAIN-SUFFIX,wiley.com,DIRECT
- DOMAIN-SUFFIX,worldbank.org,DIRECT
- DOMAIN-SUFFIX,worldscientific.com,DIRECT
# > Plex Media Server
- DOMAIN-SUFFIX,plex.tv,DIRECT
# > Other
- DOMAIN-SUFFIX,cn,DIRECT
- DOMAIN-SUFFIX,360in.com,DIRECT
- DOMAIN-SUFFIX,51ym.me,DIRECT
- DOMAIN-SUFFIX,8686c.com,DIRECT
- DOMAIN-SUFFIX,abchina.com,DIRECT
- DOMAIN-SUFFIX,accuweather.com,DIRECT
- DOMAIN-SUFFIX,aicoinstorge.com,DIRECT
- DOMAIN-SUFFIX,air-matters.com,DIRECT
- DOMAIN-SUFFIX,air-matters.io,DIRECT
- DOMAIN-SUFFIX,aixifan.com,DIRECT
- DOMAIN-SUFFIX,amd.com,DIRECT
- DOMAIN-SUFFIX,b612.net,DIRECT
- DOMAIN-SUFFIX,bdatu.com,DIRECT
- DOMAIN-SUFFIX,beitaichufang.com,DIRECT
- DOMAIN-SUFFIX,bjango.com,DIRECT
- DOMAIN-SUFFIX,booking.com,DIRECT
- DOMAIN-SUFFIX,bstatic.com,DIRECT
- DOMAIN-SUFFIX,cailianpress.com,DIRECT
- DOMAIN-SUFFIX,camera360.com,DIRECT
- DOMAIN-SUFFIX,chinaso.com,DIRECT
- DOMAIN-SUFFIX,chua.pro,DIRECT
- DOMAIN-SUFFIX,chuimg.com,DIRECT
- DOMAIN-SUFFIX,chunyu.mobi,DIRECT
- DOMAIN-SUFFIX,chushou.tv,DIRECT
- DOMAIN-SUFFIX,cmbchina.com,DIRECT
- DOMAIN-SUFFIX,cmbimg.com,DIRECT
- DOMAIN-SUFFIX,ctrip.com,DIRECT
- DOMAIN-SUFFIX,dfcfw.com,DIRECT
- DOMAIN-SUFFIX,docschina.org,DIRECT
- DOMAIN-SUFFIX,douban.com,DIRECT
- DOMAIN-SUFFIX,doubanio.com,DIRECT
- DOMAIN-SUFFIX,douyu.com,DIRECT
- DOMAIN-SUFFIX,dxycdn.com,DIRECT
- DOMAIN-SUFFIX,dytt8.net,DIRECT
- DOMAIN-SUFFIX,eastmoney.com,DIRECT
- DOMAIN-SUFFIX,eudic.net,DIRECT
- DOMAIN-SUFFIX,feng.com,DIRECT
- DOMAIN-SUFFIX,fengkongcloud.com,DIRECT
- DOMAIN-SUFFIX,frdic.com,DIRECT
- DOMAIN-SUFFIX,futu5.com,DIRECT
- DOMAIN-SUFFIX,futunn.com,DIRECT
- DOMAIN-SUFFIX,gandi.net,DIRECT
- DOMAIN-SUFFIX,geilicdn.com,DIRECT
- DOMAIN-SUFFIX,getpricetag.com,DIRECT
- DOMAIN-SUFFIX,gifshow.com,DIRECT
- DOMAIN-SUFFIX,godic.net,DIRECT
- DOMAIN-SUFFIX,hicloud.com,DIRECT
- DOMAIN-SUFFIX,hongxiu.com,DIRECT
- DOMAIN-SUFFIX,hostbuf.com,DIRECT
- DOMAIN-SUFFIX,huxiucdn.com,DIRECT
- DOMAIN-SUFFIX,huya.com,DIRECT
- DOMAIN-SUFFIX,infinitynewtab.com,DIRECT
- DOMAIN-SUFFIX,ithome.com,DIRECT
- DOMAIN-SUFFIX,java.com,DIRECT
- DOMAIN-SUFFIX,jidian.im,DIRECT
- DOMAIN-SUFFIX,kaiyanapp.com,DIRECT
- DOMAIN-SUFFIX,kaspersky-labs.com,DIRECT
- DOMAIN-SUFFIX,keepcdn.com,DIRECT
- DOMAIN-SUFFIX,kkmh.com,DIRECT
- DOMAIN-SUFFIX,licdn.com,DIRECT
- DOMAIN-SUFFIX,linkedin.com,DIRECT
- DOMAIN-SUFFIX,loli.net,DIRECT
- DOMAIN-SUFFIX,luojilab.com,DIRECT
- DOMAIN-SUFFIX,maoyan.com,DIRECT
- DOMAIN-SUFFIX,maoyun.tv,DIRECT
- DOMAIN-SUFFIX,meituan.com,DIRECT
- DOMAIN-SUFFIX,meituan.net,DIRECT
- DOMAIN-SUFFIX,mobike.com,DIRECT
- DOMAIN-SUFFIX,moke.com,DIRECT
- DOMAIN-SUFFIX,mubu.com,DIRECT
- DOMAIN-SUFFIX,myzaker.com,DIRECT
- DOMAIN-SUFFIX,nim-lang-cn.org,DIRECT
- DOMAIN-SUFFIX,nvidia.com,DIRECT
- DOMAIN-SUFFIX,oracle.com,DIRECT
- DOMAIN-SUFFIX,paypal.com,DIRECT
- DOMAIN-SUFFIX,paypalobjects.com,DIRECT
- DOMAIN-SUFFIX,qdaily.com,DIRECT
- DOMAIN-SUFFIX,qidian.com,DIRECT
- DOMAIN-SUFFIX,qyer.com,DIRECT
- DOMAIN-SUFFIX,qyerstatic.com,DIRECT
- DOMAIN-SUFFIX,raychase.net,DIRECT
- DOMAIN-SUFFIX,ronghub.com,DIRECT
- DOMAIN-SUFFIX,ruguoapp.com,DIRECT
- DOMAIN-SUFFIX,s-reader.com,DIRECT
- DOMAIN-SUFFIX,sankuai.com,DIRECT
- DOMAIN-SUFFIX,scomper.me,DIRECT
- DOMAIN-SUFFIX,seafile.com,DIRECT
- DOMAIN-SUFFIX,sm.ms,DIRECT
- DOMAIN-SUFFIX,smzdm.com,DIRECT
- DOMAIN-SUFFIX,snapdrop.net,DIRECT
- DOMAIN-SUFFIX,snwx.com,DIRECT
- DOMAIN-SUFFIX,sspai.com,DIRECT
- DOMAIN-SUFFIX,takungpao.com,DIRECT
- DOMAIN-SUFFIX,teamviewer.com,DIRECT
- DOMAIN-SUFFIX,tianyancha.com,DIRECT
- DOMAIN-SUFFIX,udacity.com,DIRECT
- DOMAIN-SUFFIX,uning.com,DIRECT
- DOMAIN-SUFFIX,vmware.com,DIRECT
- DOMAIN-SUFFIX,weather.com,DIRECT
- DOMAIN-SUFFIX,weico.cc,DIRECT
- DOMAIN-SUFFIX,weidian.com,DIRECT
- DOMAIN-SUFFIX,xiachufang.com,DIRECT
- DOMAIN-SUFFIX,ximalaya.com,DIRECT
- DOMAIN-SUFFIX,xinhuanet.com,DIRECT
- DOMAIN-SUFFIX,xmcdn.com,DIRECT
- DOMAIN-SUFFIX,yangkeduo.com,DIRECT
- DOMAIN-SUFFIX,zhangzishi.cc,DIRECT
- DOMAIN-SUFFIX,zhihu.com,DIRECT
- DOMAIN-SUFFIX,zhimg.com,DIRECT
- DOMAIN-SUFFIX,zhuihd.com,DIRECT
- DOMAIN,download.jetbrains.com,DIRECT
- DOMAIN,images-cn.ssl-images-amazon.com,DIRECT
- DOMAIN,cdn.angruo.com,DIRECT

# > Apple
- DOMAIN-SUFFIX,aaplimg.com,Apple
- DOMAIN-SUFFIX,apple.co,Apple
- DOMAIN-SUFFIX,apple.com,Apple
- DOMAIN-SUFFIX,apple.com.cn,Apple
- DOMAIN-SUFFIX,apple-cloudkit.com,Apple
- DOMAIN-SUFFIX,appstore.com,Apple
- DOMAIN-SUFFIX,cdn-apple.com,Apple
- DOMAIN-SUFFIX,crashlytics.com,Apple
- DOMAIN-SUFFIX,icloud.com,Apple
- DOMAIN-SUFFIX,icloud.com.cn,Apple
- DOMAIN-SUFFIX,icloud-content.com,Apple
- DOMAIN-SUFFIX,me.com,Apple
- DOMAIN-SUFFIX,mzstatic.com,Apple
- DOMAIN,www-cdn.icloud.com.akadns.net,Apple
- IP-CIDR,17.0.0.0/8,Apple,no-resolve

# Local Area Network
- IP-CIDR,192.168.0.0/16,DIRECT
- IP-CIDR,10.0.0.0/8,DIRECT
- IP-CIDR,172.16.0.0/12,DIRECT
- IP-CIDR,127.0.0.0/8,DIRECT
- IP-CIDR,100.64.0.0/10,DIRECT

# DNSPod Public DNS+
- IP-CIDR,119.28.28.28/32,DIRECT,no-resolve
# GeoIP China
- GEOIP,CN,DIRECT

- MATCH,Final

# Clash for Windows
cfw-bypass:
  - qq.com
  - music.163.com
  - '*.music.126.net'
  - localhost
  - 127.*
  - 10.*
  - 172.16.*
  - 172.17.*
  - 172.18.*
  - 172.19.*
  - 172.20.*
  - 172.21.*
  - 172.22.*
  - 172.23.*
  - 172.24.*
  - 172.25.*
  - 172.26.*
  - 172.27.*
  - 172.28.*
  - 172.29.*
  - 172.30.*
  - 172.31.*
  - 192.168.*
  - <local>
cfw-latency-timeout: 5000
