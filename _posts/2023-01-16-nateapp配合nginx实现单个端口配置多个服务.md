---
layout:     post   				    # 使用的布局（不需要改）
title:       nateapp配合nginx实现单个端口配置多个服务			# 标题 
subtitle:  #副标题
date:       2023-01-16				# 时间
author:     t298						# 作者
header-img: img/wallhaven-4grwgd.png 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - issue
---



## nateapp配合nginx实现单个端口配置多个服务

1. 现在的情况是服务器没有外网地址，只能用netapp来转发3001端口到外网访问，而我们还有其他的两个项目也想通过nginx来实现外网访问。

2. 我们的项目都是前后端分离式，所以如果有2个项目的话，我们应该在nginx上的server有4个location，第一个项目(地下水)路径是'/'，直接用natapp的域名访问没有问题。第二个项目(模型平台)的访问路径是'/model'，在访问的时候会出现两个问题：
   - 访问的路径是'http://xxx.natapp.cc/model',但是路径会变成'http://xxx.natapp.cc:3001/model/',手动的将端口号删除之后是可以正常访问的。
   
     是要以http://xxx.natapp1.cc/model/来访问的嘛？可以我nginx的路径写的也是/model。
   
   - 正常访问，输入用户名密码之后会进入到系统的404页面，需要点击404页面的返回首页才可以正常访问系统。

```xml
server {
        listen       3001;
        server_name  XXX.natapp.cc;

# 地下水前端
        		location / {
                root  E:/water/dist;
   				try_files $uri $uri/ /index.html;
                index  index.html index.htm;
        }
# 地下水后端接口
          		location /api/{
   				proxy_set_header Host $http_host;
   				proxy_set_header X-Real-IP $remote_addr;
  				proxy_set_header REMOTE-HOST $remote_addr;
  				proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
   				proxy_pass http://localhost:8080/;
  	}

# 模型平台前端
   				location /model {
                alias  C:/Users/DELL/.jenkins/workspace/xxx/xxxx/dist;
   				try_files $uri $uri/ /index.html;
                index  index.html index.htm;
        }
# 模型平台后端
     			location /prod-api/{
   				proxy_set_header Host $http_host;
   				proxy_set_header X-Real-IP $remote_addr;
  				proxy_set_header REMOTE-HOST $remote_addr;
  				proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
   				proxy_pass http://localhost:8082/;
  	}
  	error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            	root   html;
}
```

第一个问题是因为try_files重定向的时候会使用nginx的内部地址和端口，我们加个条件判断一下就可以了

```xml
 if (-d $request_filename) {
 rewrite [^/]$ $scheme://$http_host$uri/ permanent;
}
```



第二个问题是因为outer 把二级目录也当成路由了，我们加个配置就好

nginx配置：

```xml
# 模型平台前端入口
   	location /model{
 		if (-d $request_filename) {
 		rewrite [^/]$ $scheme://$http_host$uri/ permanent;
	}
        alias  C:/Users/DELL/.jenkins/workspace/model-platform-ui/manageBackground/dist;
		try_files $uri $uri/ /model/index.html;
        index  index.html index.htm;
}
```

vue.config的配置：

```vue
publicPath: process.env.NODE_ENV === "production" ? "/model/" : "/model/",
```

router/index.js配置：

```js
export default new Router({
  mode: 'history', // 去掉url中的#
  base:'/model/',
  scrollBehavior: () => ({ y: 0 }),
  routes: constantRoutes
})
```

