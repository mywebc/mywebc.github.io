# java中解析XML

> xml文件一般有三种用途，1. 可以用来保存数据，2. 可以用来做配置文件，3. 数据传输载体，本篇文章主要记载java中如何解析XML文件。

<!-- more -->
## XML解析方式
XML文件的定义以及标签的用法就不多说了，XML文件的约束主要有两种（DTD和Schema），现在最常见的XML的解析方式主要有两种，一个是**DOM**， 一个是**SAX**;两者区别如下
* DOM方法是将XML文件读取到内存中去生成树状结构（对象），这样的话如果文件过大就可能会出现内存溢出；
* SAX（simple api for xml）方法则是事件驱动模式，读取一行解析一行；

> 针对这两种解析方式，市场上出现了很多的解决方案比如dom4j,jaxp,jdom等

## Dom4j使用
* 首先需要用到Dom4j的jar包；
```java
    try {
        //1. 创建sax读取对象
        SAXReader reader = new SAXReader(); //jdbc -- classloader
        //2. 指定解析的xml源
        Document  document  = reader.read(new File("src/xml/stus.xml"));
        
        //3. 得到元素、
        //得到根元素
        Element rootElement= document.getRootElement();
        
        //获取根元素下面的子元素 age
        //rootElement.element("age") 
        //System.out.println(rootElement.element("stu").element("age").getText());

        //获取根元素下面的所有子元素 。 stu元素
        List<Element> elements = rootElement.elements();
        //遍历所有的stu元素
        for (Element element : elements) {
            //获取stu元素下面的name元素
            String name = element.element("name").getText();
            String age = element.element("age").getText();
            String address = element.element("address").getText();
            System.out.println("name="+name+"==age+"+age+"==address="+address);
        }
        
    } catch (Exception e) {
        e.printStackTrace();
    }
```
* 如果XML层级过深，我们可以使用Dom4j中的Xpath,xpath其实是xml的路径语言，支持我们在解析xml的时候，能够快速的定位到具体的某一个元素,当然我们还是需要添加jar包；
```java
        Element nameElement = (Element) rootElement.selectSingleNode("//name");
        System.out.println(nameElement.getText());

        System.out.println("----------------");

        //获取文档里面的所有name元素 
        List<Element> list = rootElement.selectNodes("//name");
        for (Element element : list) {
            System.out.println(element.getText());
        }
```




