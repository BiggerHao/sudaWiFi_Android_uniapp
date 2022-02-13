# sudaWiFi_Android_uniapp
一个安卓端的SUDA_WIFI快速登录器

https://sudawifi.sudaxmt.cn

一、能做啥
1. 连接SUDA_WIFI后，点一下应用图标，一键登录wifi。
2. 如果APP在后台运行，那么在手机网络状态变更为wifi后会自动连接。

二、怎么用
1. 安装后打开APP，配置帐号密码，点确定。
2. 如果需要使用自动登录，需要保证APP常驻后台，所以要在手机设置中允许APP自启动，在省电/电池设置中将APP后台运行设置为无限制，并添加到内存清理白名单中。

三、说一声
1. 这个APP不是苏州大学开发，亦不被苏州大学认可或经过苏州大学批示同意，是个人私下开发的东西，若母校认为不妥，可通过邮件等形式联系我删除安装包。support@sudaxmt.cn。
2. APP原理很简单，就是根据用户填的账号信息向a.suda.edu.cn发送登录请求，不算APP内的web-view，一次登录会发送2次请求（一次获取内网IP，一次登录），和用户手动登录的服务器开销差不多。
3. APP现在不会，将来也不会植入广告、收费使用，请警惕从其它渠道下载来二次封装的安装包。
4. 账号密码配置保存在用户本地设备上，登录时发送到登录业务服务器；开发者现在不会，将来也不会通过这个APP获取用户的任何隐私信息或账号密码等数据。

四、下载安装包：
https://sudawifi.sudaxmt.cn

五、其他
1. 开发者只会写网站或小程序，本APP是使用HBuilder构建的uni-app，写得不好的地方……理解万岁！
2. 任何人都能通过本源码改造APP，但不得用于盈利；改编而成的APP、网站等的使用场景、目标用户等涉及到苏州大学时不得包含任何广告、后门、间谍或木马程序，不得以任何形式收费或盈利。
3. 如果有H5、微信小程序、网站等开发需求，欢迎联系support@sudaxmt.cn。
