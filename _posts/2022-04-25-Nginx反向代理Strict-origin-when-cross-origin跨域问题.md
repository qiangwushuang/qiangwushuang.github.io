---
layout: post
title: Nginx反向代理Strict-origin-when-cross-origin跨域问题
date: 2022-04-25
Author: qiangwushuang 
tags: [nginx,cross,跨域]
comments: true
toc: false
---

项目部署时采用nginx+vue+iris-go架构  

后台代码部署完成后，前台在访问时总是报cors错误（Strict-origin-when-cross-origin），但是使用postman测试确没有任何问题。  

原因是浏览器处于安全考虑本身是不允许跨域访问的，出现referrer不一致的时候会发一次options请求询问服务端是否允许请求，需要服务端授权允许这种请求，返回200之后才会继续发送实际业务请求。

最终解决方法是在nginx代理中添加下面的配置：
```yml
server {
        listen 80;
        server_name xxx.com;
 location /data/ {
         proxy_pass http://172.0.0.1:8080/;
         proxy_set_header X-Real-IP $remote_addr;
         proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
         add_header Access-Control-Allow-Origin $http_origin;
         #add_header Access-Control-Allow-Methods *;
         add_header Access-Control-Allow-Headers $http_access_control_request_headers;
         #add_header Access-Control-Max-Age 60000;
         add_header Access-Control-Allow-Credentials true;
         if ( $request_method = OPTIONS ){
             return 200;
          }
 }
}
```
## 以下内容更新于2022年4月26日  
### 解决办法二  

在iris-go中增加以下函数：
```golang
func Cors(ctx iris.Context) {
	ctx.Header("Access-Control-Allow-Origin", "*")
	if ctx.Request().Method == "OPTIONS" {
		ctx.Header("Access-Control-Allow-Methods", "GET,POST,PUT,DELETE,PATCH,OPTIONS")
		ctx.Header("Access-Control-Allow-Headers", "Content-Type, Accept, Authorization")
		ctx.StatusCode(204)
		return
	}
	ctx.Next()
}
```
iris启用中间件
```golang
iris.Use(Cors)
```