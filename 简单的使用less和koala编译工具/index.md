# 简单的使用less和koala编译工具


> LESSCSS是一种动态样式语言，属于CSS预处理语言的一种，它使用类似CSS的语法，为CSS的赋予了动态语言的特性，如变量、继承、运算、函数等，更方便CSS的编写和维护。 LESSCSS可以在多种语言、环境中使用，包括浏览器端、桌面客户端、服务端。 
1. 在sublime中安装less插件（插件只是影响高亮效果） 

<!-- more -->

1. ctrl+shift+p>install Package>less>回车即可自动安装，开始我会出现下面这种提示：there is no package available,这时候只要去[https://packagecontrol.io/installation#st2](https://packagecontrol.io/installation#st2),下载 Package Control.sublime-package并且放到Sublime text安装目录下的Packages里面，重启即可。 
（2）第二种安装方法是去网上下载好插件，然后放到sublime的插件文件夹里再重启即可； 

2. 去官网下好koala后安装，设置里选择简体中文，然后测试koala工具； 
在同一个文件夹下建立两个文件：index.css和index.less文件，然后拖到koala里  然后在sublime中打开文件，在查看中打开两个窗口，左边是less，右边是css，less写好后Ctrl+s就会自动编译成css文件，如图说明编译成功； 
3. less的语法 [http://www.1024i.com/demo/less/index.html](http://www.1024i.com/demo/less/index.html) 
4. 最近又看到node js 了，一般来说是这样编译的，先安装node.js,之后再全局安装less，那么lessc就是编译less文件的意思 再sublime中我们要装LESS、lessc、Less2Css三个插件，这样sublime会调用lessc去帮我们编译，在webstorm下一般他会智能提示你的。正常来讲，这种编译方式也是很少用得到的，如通less文件很多，我们不可能一边写，一边编译，所以我们可以引入一个less.js的文件，让js帮我们去解析即可（注意在引入less时，link的rel标签要改成stylesheet/less）。
