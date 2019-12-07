
# 部署思维导图智能导学系统

## 项目架构

![](./architecture.png)

## 本地部署

### 知识点
- nodejs
- npm
- spring boot 
- docker
- neo4j
- git
- maven
### 项目概要

本项目在github的位置：https://github.com/mind-edu

这是一个前后端分离的项目，前端使用Angular+Node.Js,后端是spring boot开发，使用的数据库为neo4j。
因为本项目的neo4j是运行在docker中的，所以要用到linux系统，于是使用了云主机，neo4j运行在云主机，nodejs和spring boot项目部署在本地。通信可以示例如下

[nodejs] ---->|依赖服务器|[spring boot] --->|依赖数据库|[Neo4j]
#### 端口
   * neo4j的端口为7474，服务于spring boot
   * spring boot的端口为8899，服务于nedejs
   * nodejs 的端口为4200，用户可以通过这个端口使用整个服务
   例如：http://localhost:4200
### 先决条件
* 一台linux 服务器
* 安装了IDEA
* 安装了GIT
* jdk 8+
* maven
   
### 部署nodejs前端
在本地电脑windows 10 部署
1. 安装nodejs 
参考：
https://www.runoob.com/nodejs/nodejs-install-setup.html
注意：
nodejs 要10.6 版本以后
2. 进入打算将前端文件放置的文件夹,鼠标右键选择Git Bash Here
3. 在出现的命令行程序中依次执行

```
git clone https://github.com/mind-edu/MindmapFrontendAnt.git
cd MindmapFrontendAnt
git reset --hard 186ca6bdfa97184276aefb1f9754adc5b96c3803
npm install
npm start
```
以上命令的含义为从github仓库中下载下来代码，回滚版本，npm安装依赖，快速启动服务器。
此时，可以在 http://localhost:4200 访问登录页面，但是，登录不进去，因为没有后台支持。
### 部署neo4j数据库
在云主机部署数据库，服务器为centOs 阿里云
1. 安装docker 
参考：https://docs.docker.com/install/linux/docker-ce/centos/
可以根据该网页的提示，敲命令实现安装docker
2. 安装neo4j
参考：https://github.com/mind-edu/neo4j-docker
3. 配置云主机的安全组，开放端口7474 和7687
此时可以访问neo4j数据库了，我配置的为：
[http://47.100.41.221:7474](http://47.100.41.221:7474)

用户名为 neo4j，密码为 test
### 部署spring boot 后端
1. 参考部署nodejs 的方式，将GraduationProject这个项目拖到本地
2. 在intellij IDEA中导入这个项目。
3. GraduationProject\src\main\resources\application.properties 是配置文件，在配置文件中更改:
```
spring.data.neo4j.uri
spring.data.neo4j.username
spring.data.neo4j.password
```
为相应的值,本例为
```
spring.data.neo4j.uri=http://47.100.41.221:7474/
spring.data.neo4j.username=neo4j
spring.data.neo4j.password=test
```
4. 在IDEA 下面的Terminal中输入 mvn spring-boot:run 
```
mvn spring-boot:run 
```
可见已经跑起来了。
### 部署成功
可以在登录界面进入应用，刚开始时数据库时空的，所以要先注册，再登录。

## 线上部署

修改为服务器外网IP地址：

- 前端项目里面的 apiURL 
- 后端项目里面允许跨域地址
- 后端项目里面Neo4j图数据库地址

然后按照三个项目去启动即可的

### 线上部署问题解决

- 前端项目 Docker 容器 镜像build对服务器内存有要求的，内存在 2G 以上，否则失败。推荐 4G 内存

- 前后端项目通信，关键在于 跨域 问题解决，见后端项目 commit log




