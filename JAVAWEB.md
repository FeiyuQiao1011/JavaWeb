# Java Web

- Java SE
- Java JDBC
- MySQL
- 前端

## Servlet —— 服务器端的java小程序

- 系统架构

  B/S 网页/服务器

  C/S 客户端/服务器

**WEB后端——WEB服务器端，最核心的开发规范，Servlet**

- JavaEE是什么？

  JavaSE——一套类库，标准类库，基础

  JavaEE——一套类库，用于企业开发，可以开发企业级项目——web系统

  **JavaEE13种规范，servlet是其中之一，学习servlet本质上还是学java**

  JavaME——java微型版，开发电子微型设备内核，吸尘器啥的

## B/S系统的通信原理（没有涉及到JAVA小程序）

- WEB访问过程
  - 打开浏览器
  - 找到地址栏
  - 输入网址
  - 呈现内容

- 关于域名：
  - https://www,baidu.com/是网址
  - www.baidu.com是域名
  - 浏览器输入域名，域名解析器会解析出具体的IP地址和端口号
  - 解析结果或许：http://106:242:68:3/index.html

- IP地址是啥——计算机在网络中的身份证
- 端口号是啥？——计算机软件的唯一标识，服务
- WEB系统的通信原理？步骤？
  - 用户输入网址
  - 域名解析起解析http://106:242:68:3/index.html
  - 浏览器在网络中搜索这台主机
  - 定位这台主机的服务器软件，端口号80
  - 80端口对应服务器软件得知想要的资源是index.html
  - 服务器找到并返回（html，css，jsp）
  - 浏览器识别代码并渲染

## 关于WEB服务器软件

- 有哪些（都是提前开发好的）
  - Tomcat
  - jetty等

- 应用服务器和WEB服务器的关系

  应用服务器实现了JavaEE的所有规范，13个

  WEB服务器只实现了Servlet + JSP规范

  **应用服务器包含WEB服务器**

- 关于Tomcat
  - bin 启动、关闭tomact
  - conf 配置文件目录
  - lib jar包
  - logs 日志
  - temp 存放临时文件
  - webapps 放webapp
  - work 将来生成的java 、java class文件

- 启动Tomcat

  bin目录下有start.bat文件，运行在windows环境，批量执行windows dos命令的文件，对应的，还有startup.sh文件，运行在Linux系统

  startup -> catalina -> main -> Tomcat完全用java编写，启动就相当于打开主方法。

  配置环境变量JAVA_HOME PATH CATALINA_HOME

  Localhost:8080

  127.0.0.1:8080

  -  使用步骤
    - 找到CATALINA_HOME/webapps
    - 新建OA文件夹
    - OA中新建index.html文件
    - 启动Tomcat服务器访问这个文件

- ### 动态Web应用，思考？

  - 角色

    - 浏览器软件开发商（谷歌浏览器）
    - Web Server开发商（Tomcat）
    - DB Server（MySQL）
    - webapp的开发团队

  - 角色之间的协议？规范？

    - webapp的开发团队和web server的开发团队——Servlet规范

      **WEB Server 和 webapp解耦合**

    - 浏览器和web Server——HTTP
    - web app和DB Server——JDBC

- ## 模拟Servlet本质

  - SUN公司——制定Servlet规范

    ```java
    package javax.servlet;
    
    //sun公司
    public interface Servlet{
      void service();
    }
    ```

  - webapp开发者——写小程序，必须实现Servlet接口，实现方法

    ```java
    //实现接口，所有方法
    public class Webapp implements Servlet{
      public void service(){
        System.out.println("UserLoginServlet's service...")
      }
    }
    ```

  - Tomcat服务器开发者

    ```java
    package org.apache;
    
    import java.util.Scanner;
    import java.util.Properties;
    import java.io.FileReader;
    import javax.servlet.Servlet;
    
    public class Tomcat{
      public static void main(String[] args){
        System.out.println("Tomcat服务器启动成功，开始接受用户访问");
        System.out.print("请输入访问路径");
        Scanner s = new Scanner(System.in);
        
        //用户请求路径
        String path = s.nextLine();
        
        //请求路径和xxx Servlet的关系由谁指定？
        //由我们指定，配置文件，描述路径和xxx Servlet之间的映射关系
        /**
        /aaa = com.qiaofeiyu.desktop.index.html
        **/
        //读配置文件
        FileReader reader =new FileReader("web.properties");
        Properties pro = new Properties();
        pro.load(reader);
        reader.close;
        
        //通过key获取value
        String className = pro.getProperty(key);
        //通过反射机制创建对象
        Class clazz = Class.forName(className);
        Objtct obj = clazz.newInstance();//Tomcat开发人员不知道obj的类型
        
        //但是Tomcat开发者知道，你写的xxx Servlet一定实现了Servlet接口
        Servlet servlet = (Servlet)obj;
        sevlet.service();
        //Servlet程序不同，实现的service方法也不同
      }
    }
    ```

  - javaWeb程序员
    - 编写类实现Servlet接口
    - 配置文件，路径和类，**文件名，文件位置不能随便，Tomcat写死**

### **Servlet严格意义上来说不是简简单单的接口**

- 目录结构
- 配置文件
- 配置文件放哪里
- java程序放哪里
- 等。。。

### 开发一个带有Servlet的webapp（重点）

- 开发步骤

  - 在webapp下新建目录，crm，crm就是webapp的名字

  - 在webapp的根下新建目录：WEB-INF

    **Servlet规定的名字，必须一样**

  - 在WEB-INF下新建目录classes

    **字节码文件，存放java编译后的class文件**

  - 在WEB-INF下新建目录：lib

    **不是必须，若需要jar包**

  - 在WEB-INF下新建：web.xml

    **必须的，配置文件，描述了Servlet类和路径的对应信息**

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                          http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
      version="4.0"
      metadata-complete="true">
    
    </web-app>
    ```

  - 编写java程序，必须实现Servlet接口
    - Servlet接口在哪？不在JDK中，属于JavaEE
    - Servlet接口，Servlet.class文件由Oracle提供
    - Tomcat实现了Servlet规范，Tomcat中也需要使用Servlet，Tomcat服务器中应该有
    - 从jakartaEE9开始，Servlet接口的全名：jakarta.servlet.Servlet

  - 编译HelloServlet
    - 编译通过，需要配置CLASSPATH 
    - 配置该环境变量与Tomcat运行该文件有无关系？——**无任何关系**

  - 将编译后的class文件放到WEB-INF下面的classes文件夹下

  - 编写XML文件

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                          http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
      version="4.0"
      metadata-complete="true">
      
      <!--servlet描述信息-->
      <!--任何一个servlet都对应一个servlet-mapping-->
      <servlet>
      	<servlet-name>随便</servlet-name>
        <!--必须是带有包名的全限定类名-->
        <servlet-class>com.qiaofeiyu.servlet.HelloServlet</servlet-class>
      </servlet>
      
      <!--servlet映射信息-->
      <servlet-mapping>
      	<!--这个也随便，但需要和上面一致-->
        <servlet-name>随便</servlet-name>
        <!--这里需要一个路径-->
        <!--唯一要求是必须以 / 开始-->
        <!--路径可以随便写-->
        <url-pattern>/c/bin/sdsd</url-pattern>
      </servlet-mapping>
    
    </web-app>
    ```

  - 启动Tomact服务器

  - 打开浏览器，输入url 

    - url：http://127.0.0.1/crm/c/bin/sdsd

    - **路径太复杂可以使用超链接**

      WEB-INF同级写一个index.html

      ```html
      <!doctype html>
      <html>
        <head>
          <title>index page</title>
        </head>
        <body>
          <a href="/crm/c/bin/sdsd">hello servlet</a>
        </body>
      </html>
      ```

    - Tomcat服务器启动即调用main方法，我们只需要编写servlet实现类

### 关于javaEE的版本 

