## 脚本工作流程

工作流程是：对比云端解析记录-> 不存在则添加 -> 存在则更新。



## 运行依赖

脚本运行依赖：curl、openssl  $\textcolor{red}{（注：群晖NAS环境默认自带这些工具，无需安装！）}$



##        使用方法

1、首先，登录阿里控制台：

​	1-1）进入《云解析DNS》，新增待解析的域名记录，如：example.com;

​	1-2)  进入《AccessKey 管理》，获取AccessKey（没有则创建）。

2、然后，打开aliddns.sh脚本，在Setting里设置AccessKey，如下：

```
access_key_id=""
access_key_secret=""
```

3、最后，执行aliddns.sh脚本，语法如下：

```
./aliddns.sh [OPTION] <String>

Options:
-d, --domain    域名 (必需参数)
-h, --host      主机记录, 默认为: @
-t, --type      记录类型, 默认为: A，候选有：A, AAAA, CNAME, MX, REDIRECT_URL, FORWARD_URL ...
-v, --value     记录值, 默认为:自动获取的公网IPv4地址，其他类型可以自由设置
-l, --ttl       TTL, 默认为: 600s，免费用户固定为600秒，付费用户可以设置其他值。
-m, --mac		IPv6模式下，通过指定邻居机器的MAC地址，即可获取邻居机器的IPv6地址进行DDNS。(v1.4.0支持)
```

### 使用示例

```
1. 在example.com下，创建或更新两条记录：@、www [ IPv4 ]
./aliddns.sh -d example.com -h @ -h www
         
2. 在example.com下，创建或更新两条记录：@、www [ IPv6 ]
./aliddns.sh -d example.com -h @ -h www -t AAAA
         
3. 在example.com下，为邻居机器创建或更新两条记录：@、www [ IPv6 ]
./aliddns.sh -d example.com -h @ -h www -t AAAA -m 79:81:c2:62:86:45
       
4. 将所有域名*.example.com都指向二级域名abc.sample.com，同时指定TTL为60秒
./aliddns.sh -d example.com -h \* -t CNAME -v abc.sample.com -l 60
```

