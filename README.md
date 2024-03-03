## 配置



## 前后端联调

员工登陆，异常处理

登陆后为前端生成令牌

P9



P10 

nginx**反向代理**，把前端的动态请求转发到后端服务器

Http://localhost**/api/**employee/login -> NGINX -> http://localhost:8080**/admin**/employee/login -> Tomcat

* 提高访问速度（nginx缓存访问数据）
* 复杂均衡器。后端服务ABC
* 保证后端安全。后端服务不对外开放

```
//nginx.conf
server{
	...
	location /api/ {
		proxy_pass http://localhost:8080/admin/;//反向代理配置项
	}
}


upstream webservers{
	server 192.168.100.128:8080
	server 192.168.100.129:8080
}
server{
	...
	location /api/ {
		proxy_pass http://webservers/admin/;//负载均衡配置项，平均的分配到后端服务器上
	}
}
```





P12 完善登录功能

员工表的密码是明文存储。

1.MD5加密密码，不可逆。 12345 -> eshf387r4w8

2.修改java代码，前端提交的密码进行md5加密后在比对。