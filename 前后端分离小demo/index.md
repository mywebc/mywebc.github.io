# 前后端分离小demo


> 目的：前端请求后端，插入数据到数据库。 新建两个文件夹，vueServer和vueClient

vueServer
<!--more-->

服务端用express搭建服务器，mongoose数据库存储 APP.js文件很简单
 ```javascript
  let express = require('express') 
  let app = express() 
  let router = require('./routes/router') 
  let db = require('./models/db.js') 
  let cors = require('cors') 
  //静态资源管理
   app.use(express.static("./public")) 
   // 设置跨域(*) 
   app.use(cors()) 
   // 设置路由 
   app.get('/users', router.doRegist) 
   // 监听3000端口 
   app.listen(3000); 
 ```
 router.js文件 
 ```javascript
 let Users = require('./../models/users') 
 // 路由逻辑 //注册业务 
 exports.doRegist = function (req, res, next) { 
 	//接受前台数据 
 	var queryData = req.query
 	 var username = queryData.username 
 	 var password = queryData.password 
 	 // 查询是否有重复 
 	 Users "username": username 
 	 }, 
 	 function (err, doc) { 
 	 	if (err) { 
 	 		console.log(err); return; 
 	 		} if (doc.length != 0) { 
 	 			res.send("用户已经存在！"
 	 			); } 
 	 			//写入数据库 else { 
 	 				Users.create({
 	 				 "username": username, "password": password 
 	 				 }, function (err, doc) { 
 	 				 	if (err) { console.log(err); return; 
 	 				 		} res.send("插入数据库成功！"); 
 	 				 		}) 
 	 				 		} 
 	 				 		}) 
 	 				 	}
  ```
  数据库文件 db.js 
 ```javascript
 let mongoose = require('mongoose') 
 // 连接数据库 mongoose.connect('mongodb://localhost/users') 
 let db = mongoose.connection 
 // 监听连接状态 db.once('open', function () { 
 	console.log('数据库连接成功！') 
 	}) 
 	module.exports = db 
 ```
  users.js
   ```javascript
   let mongoose = require('mongoose') 
   // 定义schema结构
    let userSchema = new mongoose.Schema({
     "username": String, "password": String 
     }) 
     // 利用schema生成模型
      let Users = mongoose.model("Users", userSchema) 
      // 暴露出去 
      module.exports = Users 
   ```

vueClient

直接用脚手架生成，用axios发送请求 
``` javascript
methods:{ doRegist() { 
	// 获取用户名 
	let username = this.$refs.username.value 
	let password = this.$refs.password.value 
	if(username === "" || password === "") {
	 alert("用户名或密码不能为空！") 
	 } else { 
	 	// 向后端发送请求，存入数据库 
	 	axios.get("http://192.168.43.54:3000/users", { 
	 		params: { "username": username, "password": password } 
	 		}).then((response) => { 
	 			if(response.status === "200") {
	 			 console.log("注册成功！") } 
	 			 }).catch((error) => { 
	 			 	console.log(error)
	 			 	 })
 			 	  } 
			 	  } 
		 	  }
	 	   }
 ```
  最后localhost:8080能成功请求localhost:3000,并将数据插入到数据库。
