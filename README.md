# GFWList2Dnsmasq

Convert [GFWList](https://github.com/gfwlist/gfwlist) to [Dnsmasq](http://www.thekelleys.org.uk/dnsmasq/doc.html) configuration file.

Since only mainland Chinese users have this kind of network issue, so I wrote the README file in Chinese. If you have any question, Please [let me know](https://github.com/HouCoder/gfwlist2dnsmasq/issues/new).

### 运行环境

建议在路由器上运行的用户使用 `$ opkg install python` 确保满足依赖。

在下面的 Python 环境中测试使用正常：

1. Python 2.6.6 on CentOS 6.5
1. Python 2.7.3 on OpenWrt 14.07
1. Python 2.7.9 on OpenWrt 15.05
1. Python 2.7.10 on OS X 10.10.5

如果程序运行异常请在 issues 里面反馈，反馈时需要的基本信息包括：Python 版本、系统环境和错误消息。

### 安装

1. `$ wget https://github.com/HouCoder/gfwlist2dnsmasq/archive/master.zip; unzip master.zip`.

如果运行环境为不支持 HTTPS 协议的路由器请下载到本地后使用 scp 命令上传至路由器。

### 使用

使用非常简单，运行下面的命令即可启动脚本：

`$ python main.py`

执行完成后在程序目录里面会发现两个文件：`gfwlist2dnsmasq.log` 和 `dnsmasq.conf`，前者是程序 log 文件，便于 Debug 时使用；后者即是我们想要的 Dnsmasq 配置文件。

出于对于低配置的路由器容量考虑，`gfwlist2dnsmasq.log` 文件大小最大值为 `128KB` ，超过该值 log 文件将会被清空。

当然你也可以指定自己的 **JSON** 格式的配置文件，程序会根据用户不同的配置生成不同的 Dnsmasq 配置：

`$ python main.py -c ~/user-config.json`


### 可配置项

JSON 文件示例可以参考 `user-config.json.example`.

可配置项说明：

**sourceUrl**

Type: `String`

指定 GFWList 的地址。

**dnsServer**

Type: `String`

Default: `208.67.222.222`

DNS 服务器地址，推荐使用 [OpenDNS](https://www.opendns.com/home-internet-security/) 的 DNS 服务。

**dnsPort**

Type: `Int`

Default: 5353

默认使用 5353 特殊端口避开 DNS 污染。

**userList**

Type: `String`

No default

除了使用 GFWList，用户还可以指定自定义列表，自定义列表规则请遵循 [Adblock Plus 过滤规则](https://adblockplus.org/zh_CN/filters)，其实非常简单，`!` 代表注释，每行写一个域名即可，例如：

```
! My Comment
google.com
youtube.com
```

用户自定义列表支持本地文件，如： `/home/bart/my-list.txt`；在线文件，如：`http://bart.com/list.txt`，还可以同时设置多个不同类型用户列表，列表之间请使用 `|` 分割，如：`/home/bart/my-list.txt|http://bart.com/list.txt`。**用户自定义列表不支持经过 base64 编码后的格式**。

**ipsetName**

Type: `String`

Default: `freedom`

本地 ipset 规则名称。

**targetFile**

Type: `String`

Default: `dnsmasq.conf`

默认会将列表生成到程序目录的 `dnsmasq.conf` 文件里面。

### 自动更新配置

在路由器上安装配置完成后测试无异常可以使用 cron 定时更新配置文件，添加下面的规则至 cron 即可实现每天凌晨五点更新一次列表：

```
00 05 * * * python PATH_TO_FILE.py -c YOUR_CONFIG_FILE
```

指定 `targetFile` 到 Dnsmasq 配置目录，记得重启 Dnsmasq 以保证配置生效。

### TODO

- [x] 最大 log 限制。

### 反馈

如果您在使用中有任何问题欢迎积极反馈或者创建一个 PR。

### LICENSE

![](http://www.wtfpl.net/wp-content/uploads/2012/12/wtfpl-badge-1.png)

[Do What the Fuck You Want to Public License](http://www.wtfpl.net/)
