### XSS攻击

XSS(cross site scripting)攻击是指攻击者向用户的页面注入脚本，用脚本来实现攻击。主要分为三类:

1. 存储型攻击

   通过页面输入脚本，**存储到服务器**，用户在访问页面，脚本被插入到页面。比如攻击者在评论中输入`<script>alert('xss')</script>`，那么用户在查看评论时脚本就会被插入到html中，脚本就会执行。

2. ​	反射型攻击，与存储型不同的是，反射型的脚本不会存储在服务器端，不是持久的。比如我们访问链接`http://www.baidu.com?name=alert('xss')`，而页面的行为是获取那么属性并添加到html中，那么脚本就会被执行。

XSS主要会有以下一些危害

1. 获取用户的cookie信息(document.cookie)
2. 监听用户的行为(addEventListener)
3. 通过js修改dom

大体来说js能做的事情，脚本都可以

### CSRF攻击

CSRF(cross site forgery)攻击是指攻击者在自己的页面利用用户的登录状态(cookie)行服务器发送请求来实现攻击。主要分三类：

1. 自动发起get请求。比如攻击这在自己的页面利用img标签的src来发送get请求。
2. 自动发起post请求。攻击者在自己的页面创建一个自动提交的form表单，发起post请求。
3. 诱导用户点击，发起请求。比如一张图片或者文字诱导用户点击，从而发起请求。

防止攻击

1. 设置Cookie的sameSite属性，防止第三方页面发起的请求携带Cookie
2. 服务端根据请求头的origin和referer来判断请求的来源
3. 使用token方式取代cookie来验证用户信息

