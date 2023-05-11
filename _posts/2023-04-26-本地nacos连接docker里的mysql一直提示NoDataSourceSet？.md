---

layout:     post   				    		# 使用的布局（不需要改）
title:      本地nacos连接docker里的mysql，一直提示No DataSource Set？		# 标题 
subtitle:  									# 副标题
date:       2023-05-11						# 时间
author:     t298							# 作者
header-img: img/fj.jpg					#这篇文章标题背景图片
catalog: 	true 								# 是否归档
tags:										#标签

    - bug

---

### 环境描述：

在局域网服务器centos7.9上使用dokcer部署了ruoyi的微服务项目，其中的mysql端口号为3307，然后我又部署了一个同版本的mysql，端口号为3306，mysql版本均为8.0.23.使用navicat可以正常连接这两个数据库。

### 问题描述：

一直使用的是本机（win）上的nacos2.0.0(standalone)连接mysql:3306来使用的一直没有问题，最近觉得同步数据库太麻烦了，想使用本地的nacos来连接项目中的mysql:3307来使用，但是更换了端口号之后，nacos就启动不了了。一直提示“放弃连接”，“No DataSource Set”。但是此时，navicat和服务器上的nacos都是连接正常的。我确定我的nacos配置是没有问题的，因为以前连接mysql:3306的时候都是正常使用的。

### 尝试方法：

1. 我认为有可能是mysql:3307反应太慢了，就是navicat连接，查询都要等一会，我也不知道为什么会这么慢。所以我尝试添加如下配置，但是还是失败。

   ```java
   spring.datasource.hikari.connection-timeout=50000
   ```

2. 我新建了lib目录和plugins放入了mysql8.0.23的驱动，启动还是错误。

