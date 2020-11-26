# springboot项目部署到阿里云轻量型服务器

### 开发环境配置

1. jdk 11

2. tomcat 9.0.40

3. mysql 8.0.11

   > 在Ubuntu种装mysql的时候一定要先把系统中原本有的mysql删除，否则会装不上



### 遇到的bug

1. javaMail发送邮件失败

   > 解决方法，这是由于javaMail默认使用25端口发送，改成`587端口`即可
   >
   > 但值得注意的是，我第一次改成465端口报bad connect的错误(防火墙已打开)，不知道为什么换成587就可以了。

2. 访问路径问题

   > 一开始我直接把生成的cloud.war放在tomcat的webapps目录下，tomcat自动解包，但是访问路径名为**xxx:8080/projectName/user/login**，中间多了*projectName*导致路径过滤死循环以及访问不到静态资源，解决方案有两种，一种是在server.xml的host中配置context使路径为`""`，第二种是删除webapps下**ROOT**文件，并把自己的项目名改成ROOT(部署的项目名就是你war的名字，所以直接把cloud.war改成ROOT.war)。
   >
   > 我使用了第二种方式。