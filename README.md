# 如何愉快的翻墙

####  ps:希望不要外传，树大招风，好不容易找到好用的服务器（已经记不清用过多少家vps服务商的服务器了。。）

####  根据我的经验，最好不要给翻墙服务器绑定域名，或者让它做些其他事（例如运行一个web服务器），我没法确定是不是因为绑定了域名导致服务器丢包严重，但是我绑定过域名的翻墙服务器都出现过这种情况。

## vps 服务商

- [Hinet中华电信](https://userportal.hicloud.hinet.net/cloud/)
  - 优点：翻墙速度快，宽带足（因为按流量收费，不限制宽带），并且不容易因为海外发生什么事件受影响（前一段时间阅兵某些外国shadowsocks服务流量被污染）
  - 缺点：
    - 偶尔宕（duang）机
    - 收费相对较高（按流量收费，我一个人使用差不多60~100+）
    - 开通流程比较复杂（需要使用信用卡，台币结算），并且需要提供护照号码。
    - 给shadowsocks加外挂翻墙比较麻烦（需要手动升级系统内核，我已经不想再搞第二次了，后面会介绍）
    - 管理网站用extjs做的，不知道为啥我这打开卡的要死。
  - 推荐像我这种离开了墙没法活的人使用。。。
- [vultr](https://userportal.hicloud.hinet.net/cloud/)
  - 优点：
    - 日本服务器翻墙速度也不错
    - 价钱便宜（日本服务器每个月400G流量5美元，换算成人民币30元一个月）
    - 开通特别方便
    - 可以设置启动脚本，一键部署shadowsocks
  - 缺点：
    - 大概是速度比不上hinet
    - 有时会丢包
  - 推荐不喜欢折腾，又希望速度能相对快些的
  - 推荐链接：http://www.vultr.com/?ref=6813206 ，通过这里注册能给我返现 XD 
- [oneasiahost](https://www.oneasiahost.com/billing/cart.php)
  - 优点：
    - 新加坡服务器，速度比vultr更快（以前买测试是比vultr日本快）
    - 价钱便宜（最便宜的套餐12美元3个月，每月200G，对于翻墙够用了。）
  - 缺点：
    - 开通需要隔天，不能按小时或者按天计费
    - 最便宜的服务器常卖断货
    - 配置比较低
    - 公司给人一种随时要跑路的感觉
  - 有用过一段时间，推荐喜欢折腾又想价格便宜些的使用
- [digitalocean](https://www.digitalocean.com/)
  - 优点：开通最快最方便，公司比较有实力
  - 缺点：
    - 美国的服务器延时比较高
    - 新加坡的服务器完全没用（丢包太厉害了）
  - 可以配合国内cdn搭建网站，但是做长期使用的翻墙服务器还是算了
  - 推荐链接：https://www.digitalocean.com/?refcode=c0c3f693bd98 通过这里注册也能给我返现。
- [linode](https://www.linode.com/)
  - 优点：
    - 老牌vps服务商，某些人买到的vps访问速度很快（之前看别人买到的ping值60-100左右）
    - 宽带给的特别足
    - 可以设置启动脚本，一键搭建翻墙服务器
  - 缺点：不管你信不信，反正我是没有买到好用的vps，试过开几十台服务器，一台一台测试速度，最后还是放弃了。
  - 推荐链接还是不放了，从没打算常用linode
  
## 写在vps服务商后面的话

之前有考虑过使用香港的vps，速度快，开通方便，但是宽带给的低，高宽带的服务器价格贵的不适合翻墙服务器

个人认为好用又便宜的vps逃不出香港，台湾，新加坡或者韩国日本（我会告诉大家我连泰国、越南的vps都用过吗？好吧，当然会。。）

大家如果找到了好用又便宜的vps可以直接发个pr，看到了我会及时更新的，

## 部署

#### shadowsocks一键安装脚本（推荐）

python版和libev版区别
-  python版在不同版本的linux使用只需使用同样一个脚本
-  libev版节约内存

安装脚本地址（需要翻墙）

[Shadowsocks Python版一键安装脚本（CentOS，Debian，Ubuntu）](https://teddysun.com/342.html)
[CentOS 下 shadowsocks-libev 一键安装脚本](https://teddysun.com/357.html)
[Debian 下 shadowsocks-libev 一键安装脚本](https://teddysun.com/358.html)

#### 手动安装

shadowsocks最后一个release还可以访问
https://github.com/shadowsocks/shadowsocks/tree/7c08101ce8a673fafb22477e8ad720aa57114a1f

官方repo的wiki已经不能访问，这是fork别人保存下来的wiki，手动安装或者优化可以参考
https://github.com/MofeLee/shadowsocks-wiki

## 使用流程

### 浏览器使用

SwitchyOmega

### 命令行使用

[proxychains4](https://github.com/rofl0r/proxychains-ng)，强制让进程使用代理服务器访问网络。

安装方法：

1. 下载安装包https://github.com/rofl0r/proxychains-ng/archive/master.zip
2. 编译安装
  ```bash
   # needs a working C compiler, preferably gcc
    ./configure --prefix=/usr --sysconfdir=/etc
    make
    [optional] sudo make install
    [optional] sudo make install-config (installs proxychains.conf)
  
   # if you dont install, you can use proxychains from the build directory. like this:
   ./proxychains4 -f src/proxychains.conf telnet google.com 80
  ```
3. 配置代理 详细配置文件在[这个文件里](https://github.com/MofeLee/how-to-fxxk-gfw/blob/master/proxychains.conf)


#### alias 一些常用命令
```bash
# 命令行下使用http代理
hp='export http_proxy=http://127.0.0.1:8123 https_proxy=http://127.0.0.1:8123'
nhp='unset http_proxy https_proxy'

# 命令行下强制使用代理服务器
pxy=proxychains4
#   完全代理zsh
pz='echo '\''in shadow'\'' && in=in_shadow proxychains4 -q zsh && echo '\''out shadow'\'
#   代理单个进程
x='proxychains4 -q '
xni='proxychains4 -q npm install '
```

## 加速优化
http://www.serverspeeder.com/

## 其他翻墙方法总结

- DNS翻墙：最简单，推荐电脑没有重要资料，或者iPad或者iPhone临时翻墙用，有比较严重的安全隐患。
- 翻墙浏览器：也简单，不过安全隐患也很严重。
- 自由门(极其不推荐)：上古时期的翻墙方法，有些安装包被人绑了木马，并且只能在windows下使用。
- vpn翻墙：购买或搭建vpn服务器，有可能被gfw屏蔽流量

## 如有疑问可以发issue，有必要讲的更清楚的信息我会及时更新到repo上~点右上角的watch可以收到更新~
