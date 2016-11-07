#	Express

>	Fast, unopinionated, minimalist web framework for Node.js  
>	引自 [Express 官方网站](http://expressjs.com)

Java? Tomcat!  
PHP? Apache!  
.Net? IIS!  
Node.js? Yum ... Express?

每一种主流的 web 开发语言，都有度身定制的应用容器。然而，Node.js 发展至 7.0，却依旧孑然一身。幸好，它还有 Express。

##	在线资源

*	[Express 官方网站，http://expressjs.com](http://expressjs.com)

##	从 Express Generator 开始

```bash
# 安装初始化命令行工具
npm install express-generator

# 查看命令帮助
express -h
# 注意：这里生成的 express 命令，可用于初始化基于 Express 框架的应用，但它不是框架本身。

# 创建（如不存在）并初始化应用目录
express myExpressApp

# 切换到应用目录
cd myExpressApp
# 以调试模式启动应用
PORT=8080 DEBUG=1 ./bin/www
# 或，直接启动
npm start
```

如果不指定启动端口，通过 Express Generator 创建的应用的默认端口为 3000：  
![localhost3000](expressjs.img/localhost3000.png)
