# Java的包管理


> java中的包就是一些第三方的类库，跟前端的npm包差不多，前端通过npm这个平台管理npm包，在java中通过maven来实现包管理。

## maven的包管理
* 传递依赖的包的自动管理
* 依赖冲突的自动解决 - 就近原则
* 包的作用域（scope）

## 关于pom.xml （Project Object Model）
**pom.xml是maven用来管理包的配置文件，与前端使用npm包生成的package.json文件类似。**

* 一份简单的pom.xml配置
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>groupID</groupId>
    <artifactId>artifactId</artifactId>
    <version>1.0.0</version>
    <packaging>...</packaging>

    <name>...</name>
    <url>...</url>
    <properties>...</properties>
    <!-- 依赖关系 -->
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.0</version>
            <type>jar</type>
            <scope>test</scope>
            <optional>true</optional>
        </dependency>
    </dependencies>
    <!--构建设置 -->
    <build>...</build>
</project>
```
* 其中必须的三元素： 项目组( groupId )，项目名称( artifactId )及其版本( version )。
* packaging:打包机制，如pom,jar,maven-plugin,ejb,war,ear,rar,par
* name:自定义名称 可选
* url:自定义网站，可选
* properties:是为pom定义一些常量。

**关于dependencies**
* type：默认为jar类型，常用的类型有：jar,ejb-client,test-jar...,
* scope：是用来指定当前包的依赖范围，
* optional:设置指依赖是否可选，默认为false,

## 包冲突的解决方案
* 包冲突的错误一般会报如下错误
1. AbstractMethodError
2. NoClassDefFoundError
3. ClassNotFoundException
4. LinkageError

**maven的自动处理**

maven会采取就近原则自动帮我们处理。

**手动排除**

使用mvn dependency:tree命令分析，在pom.xml中使用 exclusion 标签去排除冲突的jar包。

**Maven Helper插件（推荐）**

idea中安装Maven Helper插件，使用插件分析冲突的包，自动添加 exclusion 。

