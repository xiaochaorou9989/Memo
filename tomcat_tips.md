* Neither the JAVA_HOME nor the JRE_HOME environment variable is defined 

> 添加JAVA_HOME系统变量即可

> 很多时候装了JDK和IDEA，运行springboot，都是正常的，其实并没有配JAVA_HOME系统变量

---

### IDEA配置tomcat

* 添加变量

> CATALINA_BASE #tomcat安装目录

> CATALINA_HOME #tomcat安装目录

> ClassPath 加入 %CATALINA_HOME%\lib\servlet-api.jar;

> Path 加入 %CATALINA_HOME%\bin;%CATALINA_HOME%\lib

* 验证

> cmd 运行 startup

* IDEA配置

> Run -> Edit Configurations -> add -> Tomcat Server-> Local

> tomcat Server -> Unnamed -> Server -> Application server -> Configuration -> 找到本地 Tomcat 服务器 -> 点击 OK按钮

---

### 本地编译打包部署

1. 直接在idea左下的Terminal终端输入mvn install，编译打包成功，显示BUILD SUCCESS字样

2. 重新查看项目的目录，我们发现项目根目录下面多了target目录，该目录下也打出了war包

3. 再次进入部署界面在 Deployment 中 点击 + ，选择要部署的工程选择TestDemo.war作为部署包

	- war模式这种可以称之为是发布模式，看名字也知道，这是先打成war包，再发布

	- war exploded模式是直接把文件夹、jsp页面 、classes等等移到Tomcat 部署文件夹里面，进行加载部署
	
	- war exploded这种方式支持热部署，一般在开发的时候也是用这种方式

	- 在平时开发的时候，使用热部署的话，应该对Tomcat进行相应的设置，这样的话修改的jsp界面什么的东西才可以及时的显示出来
	
	- 我设置server标签On 'Update' action等为Update classes and resources

4. 填写访问路径（一般为项目名，输入后会同步到server标签页的localhost处），点击 OK

5. 在主界面中 Application Servers 中就可以看到部署的工程，点击左侧绿色三角形就可以运行 Tomcat 服务器