##《精品活动》混合APP开发总结

### 一句话概述产品

在朋友圈内玩的团购活动，基于强好友关系，消除传统云购人人可以购买造成的信任等问题，这也算是产品的特色吧，然而我们也要做上一元云购了。

### 研发历程

团队4人，包括3技术（1前1后1实习前端），1产品。外包的UI。历时三个半月迭代三个版本。第二个版本下载地址：[http://pre.im/yangnk](http://pre.im/yangnk)。第三版基本研发完成，进行测试阶段。

1. 第一版：无UI，最小功能版本。
2. 第二版：更新UI，完善功能版本，增加红包模块功能。
3. 第三版：增加一元云购模块以及wap端。

### 技术方面

* 前端技术选型

关于前端技术选型，公司决定混合开发模式开发APP，总监确定使用ionic框架，于是选择了我。ionic框架基于angularJs和cordova，框架在国内也已经有几年的发展，技术相对比较成熟，社区比较活跃，一些问题也基本都有解决方案。自己也是用ionic框架开发了几个产品，体验上要求不是那么极致的情况下，还是能接受。ionic在安卓和IOS低版本设备上，体验较差，比较卡，会掉帧。虽然也有相应的解决方案，将ionic项目封装在crosswalk里面，这样一来可以解决低版本设备上webview性能较差和支持性较差的问题，可是crosswalk封装过后的APP体积就比较大了。正常情况下7M的APP，打包出来一般是20M+。这样就看产品的定位和用户人群了。虽然对React native、koaJs等移动端技术有一定的了解
，但是React native开发起来还是有一定的技术梯度，ES6，webpack，高度组件化的开发模式。koa基于nodeJs，我们后端采用JAVA开发，个人也没有实际项目中应用nodeJs开发，最终还是选择了稳健和熟悉的ionic框架+gulp。

* 移动端布局

样式采用sass，这也贴合了ionic的css架构，对APP中需要定制的一些布局，ionic是比较难实现的，这样一来就可以直接修改ionic的sass文件了。当然由于时间的原因，APP没有考虑横屏适配，横屏适配对UI设计要求就比较高了，外包UI，，，差不多就差不多了吧。前端实现上主要就是应用query media媒体查询了。

* cordova部分
 1. [微信分享，支付，登录](https://github.com/xu-li/cordova-plugin-wechat)
 2. [QQ分享](https://github.com/iVanPan/Cordova_QQ)
 3. [极光推送，IM](https://github.com/jpush/jmessage-phonegap-plugin)
 4. [ngCordova调用摄像头](http://ngcordova.com/docs/plugins/camera/)
 5. [ngCordova文件上传](http://ngcordova.com/docs/plugins/fileTransfer/)
 6. [ngCordova本地文件读取](http://ngcordova.com/docs/plugins/fileOpener2/)
 7. [ngCordova本地相册](http://ngcordova.com/docs/plugins/imagePicker/)
 8. [SQLite本地信息存储](http://ngcordova.com/docs/plugins/sqlite/)
 9. [APP版本更新](https://github.com/zxj963577494/ionic-AutoUpdateApp)
 10. [ionic热更新](https://github.com/nordnet/cordova-hot-code-push)
 11. [ionic图片资源延时加载](https://github.com/paveisistemas/ionic-image-lazy-load)
 
列举了部分cordova轮子，其中IM功能是我一直想用socket直接进行开发的，HTML5中websocket技术也是比较成熟，socketJs，socket.io等类库也是比较成熟。然后就是还没有开发，应该也是不准备用js开发这个了。

ionic热更新，这个是我很喜欢的功能，混合APP离线文件存储，需要换某个HTML或者js文件时，可以直接更新服务器上的文件版本信息即可，用户不需要直接进行版本更新，用户体验较好，其实现在很多成熟的APP这个是基本功能。然后，应该也是不准备开发了，我心好塞。

SQLite，一个轻量化的数据库，这个东西用上来，是用来做登录保持和数据存储的，当时是与localStorage对比。localStorage在移动端有5M大小的限制，在IOS内存不足的情况下，系统会将本地localStorage清除掉。但是这个SQLite的SQL语句执行是异步的，没有提供同步的方法，也许是我知识有限。这个就比较坑啦，你去SQLite里面读取数据，它在执行SQL查询语句的时候，就执行了后面的代码，一般后面的代码，都是需要从SQlite中去获取数据，带着参数去请求接口，这个异步，那是一定要出错了，然后一步步log，发现了SQLite是异步的，还发现了SQLite返回的是一个promise，这样就把这个result当成一个promise去处理，解决了这个异步的问题。如果是localStorage就不会有这个异步的问题。

APP版本更新，这个遇到一个坑，发现在华为高端机版本更新异常（暂时称为高端机，具体型号忘记了，总经理的，不能进行版本更新，这个手机还不能设置代理，不能进行手机抓包，，，不知道是不是什么新的手机安全机制），后面由于时间问题，这个问题先延期了。

极光推送，js SDK推送有延迟，到达率也有点问题。用的免费的，不知道收费的效果怎么样，不清楚原生的SDK效果怎么样，还望大神分享经验。

微信支付，[微信分享](https://github.com/Rain1368189893/Wx)，QQ分享，这些功能目前都比较稳定了，当时，在做的时候，各种未知错误。js调起微信比较慢，微信分享的链接不能用ip地址，这样不能跳转，需用域名。QQ分享的链接下面，如果有安卓审核通过，IOS审核未通过字样，说明在腾讯应用宝上，安卓审核通过了，IOS审核未通过，，，。

* gulp 部分

	1. 编译sass
	2. 压缩合并html，css
	3. 压缩混淆js
	4. 替换文件
	5. 自动刷新浏览器
	6. 雪碧图

学习研究了一阵子的前端安全，[前端如何给 JavaScript 加密](https://www.zhihu.com/question/47047191)、[移动时代的前端加密](http://div.io/topic/1220)、《web前端黑客技术揭秘》。

* HTML5 服务器推送事件

	1. WebSocket（套接字连接，基于 TCP 协议）
	2. 基于HTTP 协议来达到实时推送
		1. 简易轮询（即浏览器端定时向服务器端发出请求，来查询是否有数据更新）
		2. COMET技术（长轮询）
		3. 服务器推送事件（除 IE 外的大部分桌面和移动浏览器上得到了支持）
	
HTML5服务器推送事件分为以上两种，然后都没有使用。很难想象APP的四个TAB按钮各点击一次供触发了33次ajax请求，平均一个tab有8个请求，还能对APP要求什么体验么？一个是消息通知，一个是购买详情数据的实时变化，都需要服务器进行数据推送。必须优化的场景。


* 再小结

现如今，这款混合APP的使命被定格在了第三版本，也就是把云购版块做上去之后，将不再在此基础上研发，由原生开发的APP代替。对于这款APP，我表示很遗憾，做到这个样子，我觉得还没有看清楚混合APP应有的样子，然后已经决定放弃了。一个不愿意花时间去优化，避重就轻，又想着APP能如何的好，这是很矛盾的。“这是一个成功的失败案例！”。
