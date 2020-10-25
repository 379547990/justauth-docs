## 如何获取QQ登录的`unionId`？

在AuthConfig中设备`unionId`为`true`

```java
AuthRequest authRequest = new AuthQqRequest(AuthConfig.builder()
        .clientId("clientId")
        .clientSecret("clientSecret")
        .redirectUri("redirectUri")
        .unionId(true)
        .build());
```
> 注：使用unionId要求开发者必须已在qq开放平台申请了获取unionId的权限，否则可能会发生错误！切记！参考链接：[unionid介绍](http://wiki.connect.qq.com/unionid%E4%BB%8B%E7%BB%8D)

## 微信登录时能在微信端提示登录成功吗？

不可以，这是**微信公众平台**的功能，截至到目前（JustAuth v1.12.0）为止，暂不支持**微信公众平台**的授权登录

## 微信登录时能不需要手机确认吗？扫码后就自动登录

不可以，微信开放平台不支持这种操作。可以把微信扫码登录理解成qq用账号密码登录，扫完码后不手动点确认，微信怎么知道用户是否同意了授权？    
当然，**微信公众平台**的授权流程可以越过这个限制，只要关注了公众号，后续扫码成功后就会自动登录，但是这是**微信公众平台**的功能，截至到目前（JustAuth v1.12.0）为止，暂不支持**微信公众平台**的授权登录

## 本地如何测那些*不支持本地地址回调*的授权登录？

推荐几种方案：
1. 改`hosts`，然后将测试程序的端口改为`80`
2. 使用`Nginx`/`Apache`做代理
3. FRP内网穿透，参考地址：[使用内网穿透的方式集成第三方登录](https://xkcoding.com/2019/05/22/spring-boot-login-with-oauth.html)
