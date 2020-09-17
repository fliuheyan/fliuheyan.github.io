## Oauth2

### OpenId

### Access Token

### Refresh Token
Access Token一般都是有timeout的。
对于应用程序而言，如果在使用access token的时候过期，而后台程序又不能请求用户来刷新token.
所以就需要refresh token来更新access token.

许多授权服务器实现了OpenID Connect规范中定义的刷新令牌请求机制。
应用程序可以使用refresh_token的授权类型(用户，密码)和openid向令牌端点发出POST请求

### 4种授权模式

#### authorization-code 最常用，最安全
前端永远不会拿到access token
Browser
Server
Google

1. 网站提供到google验证的链接
```
https://google.com/oauth/authorize?
  response_type=code&
  client_id=CLIENT_ID&
  redirect_uri=CALLBACK_URL&
  scope=read
```

`code`为要求google返回的授权码，后台会通过code来换取access token
`client_id`向google表情请求的client身份
`redirect_uri`为成功的callback link
`scope`表示授权范围

2. 用户在google完成验证，这时会返回到redirect_uri，一般为你的webserver地址

```
https://redirect_uri.com/callback?code=AUTHORIZATION_CODE
```
3. server拿到code之后，会向google去换取access token

```
https://google.com/oauth/token?
 client_id=CLIENT_ID&
 client_secret=CLIENT_SECRET&
 grant_type=authorization_code&
 code=AUTHORIZATION_CODE&
 redirect_uri=CALLBACK_URL
```

`client_id`和`client_secret`让google来确认网站的身份的
`grant_type`参数的值是AUTHORIZATION_CODE 
`redirect_uri`这个redirect_uri是server端取回access token之后的回调网站

4. Google收到server端的请求之后，会向redirect_uri发送包含access token的JSON

```
{    
  "access_token":"ACCESS_TOKEN",
  "token_type":"bearer",
  "expires_in":2592000,
  "refresh_token":"REFRESH_TOKEN",
  "scope":"read",
  "uid":100101,
  "info":{...}
}
```
