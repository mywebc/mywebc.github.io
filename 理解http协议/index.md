# 理解http协议

>  http协议全称超文本传输协议，它是无连接无状态的，默认端口为80，学习前端呢，我们也需要对其请求到响应也要有一个熟悉过程。 无连接：就是每次连接只能处理一次请求； 无状态：就是没有记忆能力，每次连接都会重复传数据，上次已经请求了的话。 http协议与服务器的交互主要有4种方式GET,POST,PUT,DLETE,分别对应着查询、修改、增添、删除操作。

<!--more-->

 ## http请求由三部分组成，状态行、请求头、请求正文 
 我们以下图post方式为例： 
 状态行：就是第一行，以空格隔开第一个是请求方式，第二个是请求路径，第三个是http协议版本； 请求头：就是第二行一直到最后，这里是一些浏览器环境的信息，比如浏览器所支持的语言，请求正文的长度等； 请求正文：这里没有，主要是放一些向服务器传送的数据，注意它是以空格和请求头隔开的。 http响应由三部分组成，状态行、响应头、响应正文
状态行：和请求的状态行类似，第一个表示http协议版本，第二个是状态码200 OK表示客户端请求成功； 状态码是由三位组成的： 态码一般由3位构成：
  * 1xx : 表示请求已经接受了，继续处理。 
  * 2xx : 表示请求已经处理掉了。
  *  3xx : 重定向。 4xx : 一般表示客户端有错误，请求无法实现。 5xx : 一般为服务器端的错误。 比如常见的状态码：
    200 OK 客户端请求成功。 301 Moved Permanently 请求永久重定向。 
    302 Moved Temporarily 请求临时重定向。 304 Not Modified 文件未修改，可以直接使用缓存的文件。 400 Bad Request 由于客户端请求有语法错误，不能被服务器所理解。 
  * 401 Unauthorized 请求未经授权，无法访问。 403 Forbidden 服务器收到请求，但是拒绝提供服务。服务器通常会在响应正文中给出不提供服务的原因。
    404 Not Found 请求的资源不存在，比如输入了错误的URL。 
  * 500 Internal Server Error 服务器发生不可预期的错误，导致无法完成客户端的请求。 
   503 Service Unavailable 服务器当前不能够处理客户端的请求，在一段时间之后，服务器可能会恢复正常。 响应头：和请求头差不多，包含一些服务器有用的信息，
   例如服务器的类型，日期，长度等； 响应正文：这里是服务器返回的HTML文件。 现在当我们在地址栏输入badu.com后，我们就能知道发生了什么事了。 输入域名后，浏览器会找DNS解析域名（当然他先会转化成这种格式http://www.baidu.com/） 
   1. 首先他会在浏览器自身DNS缓存里找； 
   2. 找不到就回去操作系统（os）DNS缓存里找； 
   3. 找不到会到系统host文件里找，看对应的域名有没有映射IP；
   4. 最后会到域名服务器找，找到后与服务器进行TCP三次握手进行连接； 
   5. 连接后，进行http请求；
   6. 服务器收到请求后会返回状态行和响应信息（html）; 7浏览器开始解析HTML构建DOM树，如果没有后续请求，浏览器会向服务器发起TCP断开（四次挥手）。

