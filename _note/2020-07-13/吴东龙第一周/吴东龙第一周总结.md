# CAT暑假训练第一周周记

- ## 本周总结

  - 初步掌握了Git的各种命令以及Github多人协作的方式
  - 熟悉了Idea的操作，并把考核的项目在部署在Idea上运行
  - 熟悉了Postman的功能，并尝试测试了一两个公开接口以及个人项目的接口
  - 下载安装了ubuntu，本打算熟悉Linux命令，先搁置了
  - 熟悉了maven项目结构，懂得如何使用maven导包
  - 学习了Spring框架，初步了解了Spring框架的操作

- ## 遇到的问题，如何解决？

  - #### java不支持发行版本5

    ```
    java的安装版本与项目版本不一致。在setting中可以设置项目的java版本，使其跟安装的java版本一致
    ```

  - #### 不再支持源选项 1.5。请使用 1.6 或更高版本

    ```xml
    <!--要在maven项目的pom.xml加入jdk的设置-->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>10</maven.compiler.source>
        <maven.compiler.target>10</maven.compiler.target>
    </properties>
    ```

  - #### maven项目导包慢

    ```xml
    <!--切换了国内的阿里镜像站
    在本地的setting.xml文件中的所有东西换掉-->
    <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                          http://maven.apache.org/xsd/settings-1.0.0.xsd">
      <localRepository/>
      <interactiveMode/>
      <usePluginRegistry/>
      <offline/>
      <pluginGroups/>
      <servers/>
      <mirrors>
        <mirror>
        <id>aliyunmaven</id>
        <mirrorOf>central</mirrorOf>
        <name>aliyun maven</name>
        <url>https://maven.aliyun.com/repository/public </url>
        </mirror>
      </mirrors>
      <proxies/>
      <profiles/>
      <activeProfiles/>
    </settings>
    ```

  - #### maven3.3.2版本有bug

  - ####  Spring项目中缺少javax.annotation包的依赖。在maven配置文件pom.xml中加入依赖

    ```xml
    <!-- Javax Annotation -->
            <dependency>
                <groupId>javax.annotation</groupId>
                <artifactId>javax.annotation-api</artifactId>
                <version>1.3.1</version>
            </dependency>
    ```

- ## 下周规划

  - 学完ssm，并且整合ssm
  - 尝试与队友解决git的代码冲突
  - 试着学习ssm的练手项目