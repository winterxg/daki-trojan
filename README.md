
## Daki上部署trojan
### 平台操作
打开[Daki官网](https://daki.cc/)，注册账号。daki采用的oauth2登陆，需要注册Discord账号以授权。注册好并登陆Discord后，点击`create server`，创建一个server尽量把可用的资源都分配掉，再点击左侧`Credentials`->`Generate Password`，记住页面上的username和弹出的password。然后打开[Daki Portal](https://portal.daki.cc/auth/login)，输入刚才记下的username/email和password，点击login即进入控制台页面。

<br>

控制台主页会显示你刚创建的server和分配的域名及端口，这里是http协议，不带ssl/tls的，切记。点击server的名字，进入详情页。点击File，把本项目中的几个文件上传并覆盖进去。`start up`页中`STARTUP COMMAND 1`设置为`npm install`，`STARTUP COMMAND 2`设置为`npm start`，`DOCKER IMAGE`选择`nodejs-18`，然后部署一下项目，若干分钟后就可以http协议在浏览器中访问上述提到的分配给你的域名和端口，不出意外的话会出现hello world，再手动请求`http://域名:端口/start`，即可把web(xray)调起来。v2ray客户端配置不带tls的trojan即可。

### Cloudflare CDN任意端口回源及自选IP

建议用一个新的域名来解析上述地址，因为daki没有开TLS，cloudflare的SSL/TLS页中需要设置为`灵活`，这样设置后可能影响之前配置的其他解析记录，所以，用一个新的域名来解析daki是完全有必要的。
1. DNS页中添加CNAME类型的解析记录，记录值（目标）填上述daki平台分配给你的域名。假设你持有的域名是`example.com`。添加的CNAME记录为daki，并开启小云朵，则浏览器访问`http://daki.example.com:端口`就可以出现之前的hello world页面。
2. 点击`规则`->`Origin Rules`->`创建规则`，规则名称随意，“当传入请求匹配时”下面三个输入框以此是“主机名”、“等于”、“daki.example.com（以你在1中配置的域名为准，这里仅仅做示范）”，下面“目标端口”选“重写到->daki平台给你分配的端口”，最后点击保存，就大功告成了。这时你以https的方式访问`https://daki.example.com`，不出意外，就出现熟悉的hello world，最后，愉快地优选ip和配置v2ray客户端吧！