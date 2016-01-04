### 用户身份认证

#### 流程
1. 客户端注册输入 
	+ email/phone
	+ password
	+ username

2. 服务器接收
	+ email/phone
	+ password

3. 服务器hash(email/phone + password)生成token，保存token并返回给客户端。

		CREATE TABLE `user_token` (
		  `token` varchar(32) NOT NULL,
		  `user_id` int(11) NOT NULL,
		  `created_on` datetime NOT NULL,
		  PRIMARY KEY (`token`),
		  UNIQUE (`user_id`)
		) ENGINE=InnoDB DEFAULT CHARSET=utf8;

4. 客户端保存Token，登录成功。

5. 用户登出，Token删除。

6. 用户重新登陆，由于Token无效，用户重新输入email/phone + password， 将其发送到服务器，验证通过则重新生成Token，失败重新登陆。


#### 注意
1. Token是用于用户免输入用户和密码，而不是用来直接认证的，认证还是需要email+password的形式。
2. Token过期，以及过期Token删除问题。
	1. 给Token加一个标志：是否过期。在用户重新登陆成功时，将之前的Token标为无效。
3. Token使用缓存存储，提高认证速度。


#### 网络资源
- [用户身份认证与在线标记](http://my.oschina.net/chihz/blog/153270)
- [使用Flask设计带认证token的RESTful API接口](http://www.cnblogs.com/vovlie/p/4182814.html)
- [Python functional programming](http://coolshell.cn/articles/11265.html)