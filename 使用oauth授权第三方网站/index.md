# 使用OAuth授权第三方网站

> OAuth是一种行业标准的授权方式，我们在登录一些网站时，其也可以使用第三方账号比如QQ或者微信登录， 都是使用OAuth授权的， 版本有1.x和2.x两个，目前主要使用2.x版本，2.X版本大概有六种模式，本文介绍最常用的也是最安全的一种模式Authorization Code；

<!-- more -->
[OAuth2支持授权的几种方式（官网）](https://oauth.net/2/grant-types/)
## 六种模式
* Authorization Code (先申请Code,再申请Token，较安全)
* Refresh Token （token过期后，避免重复登录，可以刷新Token）
* Device Code (一般用于TV等设备端，不常用)
* Password (需要再第三方网站暴露授权网站的密码，不安全)
* Implicit (不需要获取code,直接获取token,不推荐)
* Client （可以使用Client id和Client sercert 去授权网站获取客户端相关的信息，与第三方用户无关，不常用）

## 获取Client Id和Client Secret
我们以Github为例，我们首先需要获取Client Id和Client Secret这两样东西，直接在Github个人设置里面develop settings选项，创建一个应用；
![githubSettings](/images/githubSettings.jpg)

## 如何请求Code
[OAuth请求官网示例](https://developer.github.com/apps/building-oauth-apps/authorizing-oauth-apps/)
* 请求链接
```javascript
GET https://github.com/login/oauth/authorize
```
* 请求参数
client_id（必填）：注册Github App时的client id；
redirect_uri：请求成功后重定向的网址带有code；
login: 登录特定账户；
scope： 授权范围，比如scope=user;
state: 随机字符串,防止跨站点请求伪造攻击;
allow_signup: 默认为true,是否提供注册github选项；

## 请求access_token
* 请求链接
```javascript
POST https://github.com/login/oauth/access_token
```
* 请求参数
client_id（必填）：注册Github App时的client_id；
client_secret（必填）：注册Github App时的client_secret；
code（必填）：上一个请求返回的code;
redirect_uri：同上；
state：同上；
**code只能使用一次**

## 访问用户信息
* 我们拿到了token就可以访问我们github上的信息了；
* 请求链接；
```javascript
GET https://api.github.com/user
```
* 在headers里面增加一个字段
```javascript
Authorization: token 你的token
```

## postman请求返回示例
![返回数据示例](/images/githubUser.png)
