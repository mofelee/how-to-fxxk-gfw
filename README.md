# 如何愉快的翻墙


**mac升级新版EI Capitan注意下**，proxychain4 在新版系统可能会有些小问题，详见https://github.com/rofl0r/proxychains-ng/issues/78

在[恢复模式](https://support.apple.com/en-us/HT201314)下运行 `csrutil enable --without debug`

-----------------

## 目录
1. [vps 服务商](#vps-服务商)
2. [部署shadowsocks](#部署)
3. [如何使用](#使用)
  1. [Macbook上使用客户端连接远程服务器](#osx下的shadowsocks客户端)
  2. [浏览器翻墙](#浏览器使用)
  3. [linux或mac命令行下使用](#linux或mac终端下使用--proxychains4强制让进程使用代理服务器访问网络)
  4. [windows下命令行下使用](#windows下命令行下使用--proxifier)
  5. [手机端翻墙](#手机端翻墙)
4. [代理服务器的加速优化](#加速优化)
5. [其他翻墙方法总结](#其他翻墙方法总结)

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
- [vultr](https://www.vultr.com/)
  - 优点：
    - 日本服务器翻墙速度也不错
    - 价钱便宜（日本服务器每个月1000G流量5美元，换算成人民币30元一个月）
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
  - （2017年1月21日更新：Tokyo 2区的机器网络不错，然后推荐链接是 https://www.linode.com/?r=a7f3625c3d98069bd1d7f86a843db68b7751e998 😅）
  
#### 写在vps服务商后面的话

之前有考虑过使用香港的vps，速度快，开通方便，但是宽带给的低，高宽带的服务器价格贵的不适合翻墙服务器

个人认为好用又便宜的vps逃不出香港，台湾，新加坡或者韩国日本（泰国、越南的vps目前没有试到比较快的）

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

python版安装脚本
```bash
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh
chmod +x shadowsocks.sh
./shadowsocks.sh 2>&1 | tee shadowsocks.log
```
Centos下libev版安装脚本
```bash
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-libev.sh
chmod +x shadowsocks-libev.sh
./shadowsocks-libev.sh 2>&1 | tee shadowsocks-libev.log
```

Debian或Ubuntu下libev版安装脚本
```bash
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-libev-debian.sh
chmod +x shadowsocks-libev-debian.sh
./shadowsocks-libev-debian.sh 2>&1 | tee shadowsocks-libev-debian.log
```

#### 手动安装（喜欢折腾可以参考下面的链接）

shadowsocks 源代码可以通过切换分支的方式访问:

https://github.com/shadowsocks/shadowsocks/tree/master

wiki:

https://github.com/shadowsocks/shadowsocks/wiki

## 使用

#### OSX下的shadowsocks客户端

OSX客户端地址：
[shadowsocks-iOS](https://github.com/shadowsocks/shadowsocks-iOS/releases)
或
[ShadowsocksX-NG](https://github.com/shadowsocks/ShadowsocksX-NG)

#### 浏览器使用

Chrome下使用[SwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif),推荐先找个临时翻墙服务器，从Chrome官方扩展中心安装，

**代理配置**：

1. 打开SwitchOmega选项
2. 打开导入导出界面，在线恢复地址设置为：
 
`https://raw.githubusercontent.com/MofeLee/how-to-fxxk-gfw/master/OmegaBackup-mofe.bak`

其他浏览器也有相应工具，不过我目前只用Chrome。

#### linux或mac终端下使用 >> [proxychains4](https://github.com/rofl0r/proxychains-ng)，强制让进程使用代理服务器访问网络。

**homebrew安装**（推荐mac下使用）：

1. 安装homebrew

    ```bash
    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ```
2. 安装proxychains-ng

    ```bash
    brew install proxychains-ng
    ```

**编译安装**（推荐linux系统下使用）：

1. 安装xcode（请直接从App Store安装）,安装完成后打开命令行输入`sudo xcodebuild -license`（用来同意xcode的license）。
2. 下载安装包https://github.com/rofl0r/proxychains-ng/archive/master.zip
3. 编译安装(需要安装xcode)
  ```bash
   # needs a working C compiler, preferably gcc
    ./configure
    make
    [optional] sudo make install
    [optional] sudo make install-config (installs proxychains.conf)
  
   # if you dont install, you can use proxychains from the build directory. like this:
   ./proxychains4 -f src/proxychains.conf telnet google.com 80
  ```


**配置代理**： 

详细配置文件在这里>>[proxychains.conf](https://github.com/MofeLee/how-to-fxxk-gfw/blob/master/proxychains.conf)

linux下编译安装需要拷贝到`/etc`目录下；

mac下brew安装需要拷贝到`/usr/local/etc`目录下。

最关键的配置是把`[ProxyList]`下代理列表改为

`socks5 127.0.0.1 1080`

测试是否代理成功

`proxychains4 curl google.com`

**映射为http代理**

注：ShadowsocksX-NG 已经映射过http代理，无需polipo

1. 安装polipo

  ```bash 
  brew install polipo
  ```
2. 修改`/usr/local/opt/polipo/homebrew.mxcl.polipo.plist`文件，添加一些启动参数，具体参照>> [homebrew.mxcl.polipo.plist](https://github.com/MofeLee/how-to-fxxk-gfw/blob/master/homebrew.mxcl.polipo.plist)
3. 配置开机自动启动

  ```bash
  ln -sfv /usr/local/opt/polipo/*.plist ~/Library/LaunchAgents
  ```
4. 立即启动polipo

  ```bash
  launchctl load ~/Library/LaunchAgents/homebrew.mxcl.polipo.plist
  ```
5. 测试代理是否成功

  ```bash
  export http_proxy=http://127.0.0.1:8123 https_proxy=http://127.0.0.1:8123
  curl google.com
  ```
  


**alias 一些常用命令**
```bash
# 命令行下使用http代理
alias hp='export http_proxy=http://127.0.0.1:8123 https_proxy=http://127.0.0.1:8123'
alias nhp='unset http_proxy https_proxy'

# 命令行下
alias pxy=proxychains4
# 完全代理zsh
alias pz='proxychains4 -q zsh'
# 代理单个进程(推荐)
alias x='proxychains4 -q '
alias xni='proxychains4 -q npm install '
```

**查看使用的是哪台服务器的网络**

```cirl iiip.co``` （我自己搭建的服务，ifconfig.co已经无法在国内直接访问了）

#### windows下命令行下使用 >> [proxifier](https://www.proxifier.com/)
具体使用可以搜索google或者baidu。


#### 手机端翻墙

推荐使用surge配合shadowsocks加国内中转代理（用于把shadowsocks协议转换成socks5代理）翻墙

或者使用[Hosts-for-Surge](https://github.com/ifyour/Hosts-for-Surge)，通过一些未被gfw封锁的ip进行翻墙

## 加速优化

锐速提供免费加速20M宽带，实测访问youtube速度的确会快很多

http://www.serverspeeder.com/

## 其他翻墙方法总结

- HOSTS翻墙：比较简单，但是只能访问限定的某些网址；
- DNS翻墙：最简单，推荐电脑没有重要资料，或者iPad或者iPhone临时翻墙用，有比较严重的安全隐患；
- 翻墙浏览器：也简单，不过安全隐患也很严重，比如百度浏览器（没用过，用windows的可以尝试下）；
- 自由门(极其不推荐)：上古时期的翻墙方法，有些安装包被人绑了木马，并且只能在windows下使用；
- vpn翻墙：购买或搭建vpn服务器，有可能被gfw屏蔽流量；
- Lantern：据说挺不错的翻墙工具，只能用来浏览网页。

## 如有疑问可以发issue，有必要讲的更清楚的信息我会及时更新到repo上~ 点右上角的watch可以收到更新~
