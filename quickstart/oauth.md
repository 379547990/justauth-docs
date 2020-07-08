## 关于OAuth

请先查阅以下资料：

- [The OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749)
- [OAuth 2.0](https://oauth.net/2/)

## OAuth 2 的授权流程

### 参与的角色

- `Resource Owner` 资源所有者，即代表授权客户端访问本身资源信息的用户（User），也就是应用场景中的“**开发者A**”
- `Resource Server` 资源服务器，托管受保护的**用户账号信息**，比如Github
- `Authorization Server` 授权服务器，**验证用户身份**然后为客户端派发资源访问令牌，比如Github
  - `Resource Server`和`Authorization Server` 可以是同一台服务器，也可以是不同的服务器，视具体的授权平台而有所差异
- `Client` 客户端，即代表意图访问受限资源的**第三方应用**

### 授权流程
```html
     +--------+                               +---------------+
     |        |--(A)- Authorization Request ->|   Resource    |
     |        |                               |     Owner     |
     |        |<-(B)-- Authorization Grant ---|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(C)-- Authorization Grant -->| Authorization |
     | Client |                               |     Server    |
     |        |<-(D)----- Access Token -------|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(E)----- Access Token ------>|    Resource   |
     |        |                               |     Server    |
     |        |<-(F)--- Protected Resource ---|               |
     +--------+                               +---------------+
```

上面的流程图取自[The OAuth 2.0 Authorization Framework#1.2](https://tools.ietf.org/html/rfc6749#section-1.2)

**流程解析**

- (A)  用户打开**客户端**以后，**客户端**要求**用户**给予授权。
- (B)  **用户**同意给予**客户端**授权。
- (C)  **客户端**使用上一步获得的授权，向**认证服务器**申请令牌。
- (D)  **认证服务器**对**客户端**进行认证以后，确认无误，同意发放令牌
- (E)  **客户端**使用令牌，向**资源服务器**申请获取资源。
- (F)  **资源服务器**确认令牌无误，同意向**客户端**开放资源。

### 授权许可 `Authorization Grant`

- Authorization Code
  - 结合普通服务器端应用使用(**web**端常用的授权方式)
- Implicit
  - 结合移动应用或 Web App 使用
- Resource Owner Password Credentials
  - 适用于受信任客户端应用，例如同个组织的内部或外部应用
- Client Credentials
  - 适用于客户端调用主服务API型应用（比如百度API Store）
  
## 直白话 OAuth 2 流程

以上流程理解起来可能有些难度，这儿我们给出一个白话版的流程图

首先引入三个角色：
- 用户A：可以理解成你自己
- 网站B：可以理解成 OSChina
- 第三方C：可以理解成 Github

需求：你（用户A）想通过 Github（第三方C） 登录网站B（OSChina）。

:bulb:注：**下面的内容为流程图，如果您在阅读的时候显示为纯文字，请尝试刷新页面，直到显示正常**

<div class="mermaid">
    sequenceDiagram
      用户A->>网站B: 1.我准备登录 OSChina
      网站B->>用户A: 2.我不认识你
      用户A->>第三方C: 3.大哥（Github）帮我介绍一下，把我的“授权码（code）”给 OSChina
      第三方C->>网站B: 4.刚刚有人想让我给你介绍一下，先给你个“授权码（code）”
      网站B->>第三方C: 5.收到，这个授权码是不是真的？是真的话就把他（用户A）的令牌（accessToken）给我
      第三方C->>网站B: 6.是滴是滴！给你他的令牌，后面你要是想知道他是谁，你就用这个令牌来找我
      网站B->>第三方C: 7.好的大哥（Github），那给你他的令牌，先把他（用户A）的基本信息给我吧
      第三方C->>网站B: 8.嗯，令牌正常，给你他的基本信息
      网站B->>用户A: 9.呀，原来你是用户A啊，我允许你登录了
      用户A->>网站B: 10.终于登录成功。（内心旁白：感谢我哥！）
</div>
