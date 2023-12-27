# springboot如何快速入门


> 前一段时间一直在看Java，一直没有总结，今天总结下spring boot如何快速入门。

### 前置条件
学习Java，环境当然要装，以下几个直接无脑装。
1. jdk - java开发环境能够编译运行java
2. maven - java中的包管理类似于npm
3. mysql - 数据库本地安装或者使用Docker安装
4. idea - 开发工具没得说

### 项目新建
项目新建有两种方式, [官网的生成器](https://start.spring.io/) 和 idea自带的Spring Initializar

#### 官网的Spring Initializar
进去后，java版本改为8（稳定），web开发需要加一个依赖spring web,其他一切默认就好，然后直接点击GERERATE下载就好，idea直接打开;
![avatar](/images/posts/springInit.png)

#### idea的Spring Initializar
使用idea自带的Spring Initializar也是很方便的
1. 先创建一个project，选择Spring Initializar;
![avatar](/images/posts/springInitIdea.png)
2. 同样版本选为8，next;
![avatar](/images/posts/springInitIdea2.png)
3. 加个spring web的依赖，一路next到底结束;
![avatar](/images/posts/springInitIdea3.png)
4. 很顺利的最后的目录结构应该是这样
![avatar](/images/posts/springInitProject.png)

### 项目运行
* springboot是一个开箱即用的框架，目前为止我们新建的项目已经可以运行了，我们可以写个接口测试以下;
* 首先在com.example.demo下新建controller包，不要问为什么，问回答这就是约定;
* 在controller下新建一个DemoController的类，类的内容如下：
```java
@Controller
public class DemoController {
    @GetMapping("/test")
    @ResponseBody
    public String testDemo() {
        return "这是第一个接口";
    }
}
```
首先看到@第一反应是不是很像装饰器，其实是两码事，你可以认为这个注解只是springboot跟你的一个约定，一个标记而已，而装饰器则会直接改变行为。
1. @Controller: 定义类为控制类，一般接口就由控制类来转发;
2. @GetMapping： 顾名思义该接口由get请求，举一反三还有个@PostMapping;
3. @ResponseBody: 定义方法返回json的数据格式;

springboot的注解还有很多，在此就不一一例举了，等到用到什么再去查就好了;

* 完成以上工作直接运行DemoApplication类就好了，然后浏览器直接访问localhost:8080/test，就能看到我们这个接口返回的字符串了;
![avatar](/images/posts/springbootInitTest.png)

测试下github actions



