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





P13 YApi导入接口文档，接口管理

![image-20240303155233794](/Users/biubiubiu/Library/Application Support/typora-user-images/image-20240303155233794.png)



P14，15 Swagger规范去定义接口，就可以生成接口文档，并且在线接口调试

替代postman（参数多的时候效率低）

knige4j是swagger的框架，更容易操作



1.导入maven坐标(pm.xml, dependency)

2.加入配置(webmvcConfigutation)

3.设置静态资源映射，否则接口文档页面无法访问



![image-20240303160415264](/Users/biubiubiu/Library/Application Support/typora-user-images/image-20240303160415264.png)

![image-20240303160433139](/Users/biubiubiu/Library/Application Support/typora-user-images/image-20240303160433139.png)



**常用注解**

1.Api，用在类上，Controller表示对类的说明

2.ApiModel,用在类上，entity,DTO,VO

3.ApiModelProperty, 用在属性上

4.ApiOperation，用在方法上，说明方法的用途





# Day2: 员工管理开发

## 新增员工

![image-20240303162017676](/Users/biubiubiu/Library/Application Support/typora-user-images/image-20240303162017676.png)

账号唯一，手机号11，身份证号18，密码默认123456



几个逻辑层的开发：

controller, service, mapping

注意，controller和service一致都是处理DTO，但是到了maing层就是处理真正的entity，所以涉及到beanutils.copyProperties(original，aim).



代码开发以后，为了避免以后调试的麻烦，添加jwt的参数设置

![image-20240303171119364](/Users/biubiubiu/Library/Application Support/typora-user-images/image-20240303171119364.png)

![image-20240303171213751](/Users/biubiubiu/Library/Application Support/typora-user-images/image-20240303171213751.png)

这个参数名称是在application.yml里写好的

![image-20240303173854458](/Users/biubiubiu/Library/Application Support/typora-user-images/image-20240303173854458.png)



### 代码完善

异常处理

1.用户名是unique的，我们先测试swagger会报错，然后在handler里全局处理这个异常

2. createdId 和updatedId是写死的

![image-20240304111702676](/Users/biubiubiu/Library/Application Support/typora-user-images/image-20240304111702676.png)

![image-20240304111924677](/Users/biubiubiu/Library/Application Support/typora-user-images/image-20240304111924677.png)

**ThreadLocal**

客户端发出的每一次请求，tomcat都会分配一个单独的线程

![image-20240304112137909](/Users/biubiubiu/Library/Application Support/typora-user-images/image-20240304112137909.png)

![image-20240304112221421](/Users/biubiubiu/Library/Application Support/typora-user-images/image-20240304112221421.png)

我们写了一个工具类basecontext

然后在拦截器存，在service取



ThreadLocal是我们用的插件里的底层逻辑，代表他怎么缓存id

但实际上我们使用Page这个写好的来完成limit操作

## 员工分页查询

GET。 /admin/employee/page

![image-20240304121108704](/Users/biubiubiu/Library/Application Support/typora-user-images/image-20240304121108704.png)



后端返回给前端的都是Result范型，这里针对分页查询，统一都会封装成Pageresult对象，和这里也是对应的。然后再变成Result<PageResult>

![image-20240304121448287](/Users/biubiubiu/Library/Application Support/typora-user-images/image-20240304121448287.png)



### 代码完善

建议第二种

![image-20240304152341688](/Users/biubiubiu/Library/Application Support/typora-user-images/image-20240304152341688.png)







## 启用，禁用员工账号

![image-20240304161935589](/Users/biubiubiu/Library/Application Support/typora-user-images/image-20240304161935589.png)



这里我们在持久层写的是update方法，用mybatis来管理，mapper.xml里写update方法，能够动态的更新所有属性。现在的需求是根据id改status，之后可以根据id改其他的时候就不用再重复写。





## 编辑员工

修改按钮，跳转到修改页面

涉及两个接口：1.根据id查询员工信息（GET 2.编辑员工信息（PUT





# Day3

## 公共字段自动填充

insert: Create-time, create-user, 

insert, update: update-time, update-user 

自定义注解：@Autofill,在mapper的方法上加上这个注解

自定义切面类autofillaspect, 统一拦截这些被注解的方法，通过反射为公共字段赋值





## 新增菜品

mock



dynamic typing... 



routing.... ========== pipe....ngGenerator





angular........

=====================

half......

Suggestions........ 