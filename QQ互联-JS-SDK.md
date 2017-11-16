> 对于自己最近在做QQ第三方登录的过程过个简单的总结和笔记。方便自己以后的记忆也希望可以帮到有需要的朋友。

1.  首先，需要申请接入QQ登录，并成功获取到++appid++和++appkey++。另外还要拿到之前在QQ互联申请接入时候填的++回调地址++。
2.  设置2个页面：++QQ登录页面++和++回调地址页面++。 （注：如果QQ登录页面与回调地址页面是同一个页面， 则只需要引用一次脚本文件。）

**回调地址页面（因为回调地址页面比较简单）**
只要添加一下的代码可以：
```
<script src="http://qzonestyle.gtimg.cn/qzone/openapi/qc_loader.js" type="text/javascript" charset="utf-8" data-callback="true"></script>
```
 - **QQ登录页面**
**引入JS SDK的JS文件**
```
<script src="http://qzonestyle.gtimg.cn/qzone/openapi/qc_loader.js" type="text/javascript" charset="utf-8" 
data-appid="[APPID]" data-redirecturi="[回调地址]"></script>
```
**设置QQ登录按钮**
```
<span id="qqLoginBtn"></span>
<script type="text/javascript">
// <![CDATA[
//调用QC.Login方法，指定btnId参数将按钮绑定在容器节点中 QC.Login({ 
//btnId：插入按钮的节点id，必选 btnId:"qqLoginBtn", 
//用户需要确认的scope授权项，可选，默认all scope:"all",
//按钮尺寸，可用值[A_XL| A_L| A_M| A_S| B_M| B_S| C_S]，可选，默认B_S size: "A_XL" })
// ]]>
</script>
```
如果正确的话会出现这个按钮:
![image](http://upload-images.jianshu.io/upload_images/1947234-99d9d9f3b2872111.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个时候点击按钮的时候，就会跳转到QQ用户的登录授权页面。如下图所示：
![image](http://upload-images.jianshu.io/upload_images/1947234-6dca013582577468.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>【链接是由你的appid，回调地址，授权信息和一些设置得到的。用户确认授权之后，跳转到回调页面并得到code和state参数之后，在去获取Access Token，等到这个Token之后结合appid去拿到每个QQ用户唯一的OpenID，之后就可以调用用户的数据和权限等，OAuth的具体详情有兴趣可以参考我另一篇文章。】

**授权成功返回数据**

只要授权和登录成功后，就可以获取用户数据和调用接口了，JS的SDK不会很复杂，只要掌握好对应的几个公开方法既可操作。
```
QC.Login(
/*设置按钮的样式*/
{
btnId: "qqLoginBtn",
scope: "all",
size: "B_M"
},
/*
@登录成之后的回调方法
@oInfo object 返回用基本信息
@oOpts object 按钮的基本情况，多个按钮时可用来区分
*/
function(oInfo, oOpts){
console.log(oInfo);
console.log(oOpts);
}
);
```

就可以根据登录成功的回调函数获取到基本信息去设置用户登录的状态了。

![image](http://chenjinhao.cn/note_img/note_youdao/qqlogininfo.png)

---
**JS SDK公开方法介绍：**
1. ```QC.Login.showPopup(oOpts)```【直接打开QQ登录弹窗】
*参数说明：*
oOpts：需要指定appId，回调地址redirect_URI。

*参数示例如下：*
```
QC.Login.showPopup({
appId:"[appId]",
redirectURI:"[回调地址]"
});
```
*注意：*
oOpts参数不需必传，不传此参数时，登录成功后会跳回登录按钮所在页面。
如果已使用QC.Login方法，则不需再使用此方法。
2. ```QC.Login.signOut()```【注销当前登录用户】
3. ```QC.Login.check()```【检测当前登录状态】

*返回值说明：*

true：说明登录成功。
false：说明登录失败。

4. `QC.Login.getMe(function(openId, accessToken){})`【获取当前登录用户的Access Token以及OpenID】

*参数说明：*

这里的参数为回调函数，通过回调函数获取openId和accessToken。
openId：用户身份的唯一标识。建议保存在本地，以便用户下次登录时可对应到其之前的身份信息，不需要重新授权。
accessToken：表示当前用户在此网站/应用的登录状态与授权信息，建议保存在本地。

*参数示例如下:*

```
QC.Login.getMe(function(openId, accessToken){
console.log("当前登录用户的accessToken为："+accessToken);
console.log("当前登录用户的openId为："+openId);
});
```

---
**调用QQ登录OpenAPI接口**

QQ互联在JS SDK中封装了所有的OpenAPI接口，开发者只需要传递OpenAPI名称，以及OpenAPI需要的相关参数，就可以调用OpenAPI。
`QC.api(api, paras, fmt, method)`【OpenAPI统一调用方式】

参数说明：

|参数名称 | 是否必须 | 描述
|---|---|---
|api |必须 |指定要调用的OpenAPI名称。例如：调用add_t时，OpenAPI名称为“add_t”。各OpenAPI的名称具体请参见API列表。
|paras |必须 |指定要调用的OpenAPI对应的参数。各参数使用JSON的键值对格式列出。OpenAPI对应的参数具体请参见API列表中各OpenAPI的参数说明。注意：此处参数不需要自行传递access_token与openid
|fmt |可选 |指定OpenAPI的返回格式，可用值为“json”或“xml”。默认为“json”。 注意：json、xml为小写，否则将不识别。
|method |可选 |指定OpenAPI调用请求的发起方式，可用值为“GET”或“POST”。根据配置，默认发送数据为“POST”，获取数据为“GET”。

直接上代码最靠谱了。
可以直接用个`function`包含起来，然后通过一个按钮的事件去触发而获取到用户信息

```
<script type="text/javascript">
// <![CDATA[
//从页面收集OpenAPI必要的参数。get_user_info不需要输入参数，因此paras中没有参数 var paras = {}; 
//用JS SDK调用OpenAPI QC.api("get_user_info", paras) 
//指定接口访问成功的接收函数，s为成功返回Response对象 .success(function(s){
 //成功回调，通过s.data获取OpenAPI的返回数据 alert("获取用户信息成功！当前用户昵称为："+s.data.nickname); }) 
//指定接口访问失败的接收函数，f为失败返回Response对象 .error(function(f){ 
//失败回调 alert("获取用户信息失败！"); }) 
//指定接口完成请求后的接收函数，c为完成请求返回Response对象 .complete(function(c){ 
//完成请求回调 alert("获取用户信息完成！"); });
// ]]>
</script>
```

这个可以`add_t`接口也是可以用个`function`包含起来，然后通过一个按钮的事件去触发用户发布微博。
```
<script type="text/javascript">
// <![CDATA[
var paras = {content : "#QQ互联JSSDK测试#曾经沧海难为水，除却巫山不是云。"}; 
QC.api("add_t", paras) .success(function(s){
//成功回调 alert("发送微博成功，请到腾讯微博内查看！"); 
}) .error(function(f){
//失败回调 alert("发送微博失败！"); 
}) .complete(function(c){
//完成请求回调 alert("发送微博完成！"); 
});
// ]]></script>
```
而我们需要的只是更改API调用的接口名。如果没有参数就为空，如果有，则为JSON的键值对形式填写。
具体的众多参数请访问[API列表](http://wiki.connect.qq.com/api%E5%88%97%E8%A1%A8)。

**Request与Response内置对象**

#####1. Request

JS SDK在初始化时会根据浏览器环境创建不同的请求代理，QC.api的每次调用都是一个Request对象。

`Request`对象的公开方法如下：
***success(resp)***: 请求完成并且返回码为0的回调。
***error(resp)***: 请求完成并且返回码不为0的回调。
***complete(resp)***: 请求完成后的回调。

调用时序为success/error -> complete，每个方法都可以调用多次，每次调用返回Request本身，支持链式调用。
resp参数为回调函数，回调函数参数为Response对象。

#####2.Response

Response为Request对象绑定的回调函数的返回参数，每次QC.api调用的异步响应都会返回一个Response对象，该对象在Request对象的success/error -> complete调用流程中传递。

`Response`的公开方法如下：
***stringifyData***：返回该Response对象包含的数据体的文本串。

`Response`的公开属性如下：
***status***：响应状态，-1：代表未知；404：响应错误；200：响应成功。
***fmt***：响应数据格式，json/xml。
***code/ret***：响应返回码，0为成功，其他为失败。
***data***：响应数据实体，json对象/xml对象。
***seq***：响应序号，从1000开始编号。
