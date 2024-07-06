---
title: Java后端面经
tags:
  - Java
  - 面经
categories:
  - 面经
keywords:
  - 面经
description: Java后端面经
abbrlink: 17766
date: 2024-05-27 19:58:53
updated: 2024-06-28 00:12:23
top_img: https://s2.loli.net/2024/05/27/ytcdAHzliRquNM2.png
comments:
cover: https://s2.loli.net/2024/05/27/6wWObXhdZL13pqo.png
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
abcjs:
---

> 准备在这篇文章中，先详细分析简历中可能会被问到的问题，然后总结一下 Java基础的精简答案，最后再回顾一下每次面试的面经。
> 本文参考 [JavaGuide](https://javaguide.cn/)

## 简历相关

### 自我介绍

面试官您好，我叫陈温鹏，就读于南京理工大学软件工程专业，学位是学硕，这次应聘的是 **Java后端开发岗位**。

我呢，研一的时候积极参加开源社区建设，先参加了Casbin社区一个线上实习，担任社区维护者，日常工作会解决社区 issue，跟踪解决用户需求，修bug以及完善相关文档；然后在研一暑假三个月时间中了一个中科院和 Casbin 社区联合举办的一个开源项目，主要的工作是完善社区整个大前端系统，包括 web，移动端功能完善，以及为社区开发了一款支持通用2FA的移动端app。除此之外，我还参与过国家电网经济研究院的一个项目。 这几段项目实习经历锻炼了我文档阅读、编写的能力，并在代码规范、开发流程等技能上获得提升。

然后我呢，我评价觉得自己是一个学习能力很强的人，可以比较快速的学习并适应新的环境和技术栈。 最后感谢 xxx 给我这次面试机会，我也十分希望能进入 xxx ，与公司共同成长进步！

### Casbin明日之星实习

1. 在实习期间负责处理社区中导师分配的issue。由于Casbin的核心访问控制功能已经比较完善，且有很多仓库及sdk，所以issue的提交并不单单围绕主仓库，我的职责更多的是放在完善sdk上面。具体就是使用Casdoor的RESTful API接口，然后使用其他语言如JS、Java、Python来调用接口实现登录登出、对相关用户信息、资源等进行增删改查，这就是其他语言的sdk。当然，随着主仓库的相关接口或者功能更新，sdk也要做出相应的调整。

2. 有时候我也负责Casdoor中的issue或者bug修复，Casdoor 是基于 Casbin 的单点登录(SSO)和权限管理系统，用于管理用户身份验证和访问权限。它提供了用户注册、登录、角色管理、权限控制等功能，可以与 Casbin 搭配使用，实现全面的身份验证和权限管理解决方案。这个系统使用react作为前端，beego作为后端，实现相关功能。

3. 另外就是负责编写演示文档了，社区中某些开发文档不够完善，需要自己测试使用某些功能后，写出教程文档。

### 中科院开源之夏项目

**简介**
Casdoor单点登录系统⼤前端项⽬包括ios、flutter、uinapp、⼩程序等sdk的开发、功能的增强， 对微信等第三⽅登录的移动端⽅式的⽀持。开发⼤前端应⽤的主要⽬的是为了提⾼⽤户体验和增加应⽤的可访问 性，可以让⽤户在不同的设备和应⽤中都能够⽅便地使⽤应⽤，提⾼⽤户的满意度和使⽤率。同时，⼤前端应⽤也可以为开发者提供更多的开发选项和⼯具，提⾼开发效率和应⽤的可维护性。

**项目诉求**
1. 对微信第三方登录的移动端支持
    - 在casdoor移动网页端，点击微信第三方登录按钮，弹出的却是扫二维码登录，这在移动端显然是不合理的，需要的是点击微信第三方登录后，跳转到微信app进行认证授权登录，用户确认后返回移动网页端。

2. 对flutter、react-native、unity等sdk的开发及功能的增强
    - 对flutter-sdk的增强如第一点

3. 临时加上的开发casdoor-app诉求，对标Google Authenticator

**技术细节**
1. 在flutter-sdk中通过截取`session`中的`url`和`callbackURLscheme`判断其中是否有"weixin"字段，如果有，则进行微信第三方原生授权登录；
    - 使用`WKWebView`替换原本的`ASWebAuthenticationSession`，对ios认证部分进行重写，以支持对每个跳转的链接进行监听；
    - 使用rb脚本文件自动化配置微信SDK导入后xcode中`xcodeproj`文件的相关配置；
    - 接入`xcframework`的微信SDK，使用obj-c重写原本的swift代码，以使用微信SDK；

    接下来根据微信SDK开发文档进行配置即可。由于配置微信SDK需要苹果开发者账号以配置`universal Link`，需付费几百美元，所以最终代码并未实际测试。
2. 分为两部分
    1. casdoor-unity-example
        - 在github上开源的一个小游戏ValleyOfCubes_Unity3D (方块之谷)的基础上，演示如何使用casdoor RESTful API进行web SSO(Single Sign On) 登录；
        - 以C#为开发语言，调用casdoor-dotnet-sdk进行web SSO 登录；
        - 在casdoor服务端配置好应用的`clientCode`和`clientSecret`后，在Unity3D中向服务端发送认证请求(`cilentCode`等)，解析返回的`token`，就可以得到用户信息，也可以使用`session`保存`token`等信息。
        - 认证过程中还使用到了unity-webview这个由格力公司一个团队开发的小组件，因为它是开源免费的。
    2. casdoor-react-native-sdk、example
        - 以react-native作为框架，使用casdoor RESTful API进行web SSO 登录；
        - react-native与JS不同的点在于在JS中，使用sessionStorage保存临时的session，主要是针对web端的，是同步的；react-native中与之类似的一个组件叫AsyncStorage，虽然功能大致相同，但却是异步的，导致不能共用一个sdk，所以就着手写了一个react-native sdk。
        - sdk采用PKCE(Proof Key for Code Exchange, 代码交换的证明密钥)这种更安全的方式获取token。PKCE是⼀种⽤于增强OAuth 2.0授权码流程安全性的协议扩展。这种协议使得在客户端不需要 clientSecret 就能获取到 token ，它依赖的是 `code_challenge` 和 `code_verifier` 。
3. casdoor-app 采用react-native框架，在授权登录功能上使用了react-native-sdk。
    - casdoor-app使用开源的totp-generator作为TOTP(Time-based One Time Passwords)码生成器，默认使用SHA-256(Secure Hash Algorithm-256)哈希算法，也可以支持SHA384、SHA512等其他多种哈希算法。
    - casdoor-app支持输入密钥和扫码生成OTP，目前已完成核心功能开发。

> SHA(Secure Hash Algorithm，安全哈希算法)是一系列的哈希算法，它们用于将数据转换成固定长度的唯一标识符，通常用于数据完整性验证、数字签名、密码学安全等领域。SHA 算法属于密码学中的哈希函数，而不是加密算法。

**总结**
参加开源活动对我的锻炼是非常大的，我觉得虽然每个小例子和sdk涉及相关知识都没那么难，但是他难就难在你从没接触过，要在短时间内学习，并将之应用。在短时间内学习掌握一种领域的知识，我觉得这才是我参与开源项目收获最大的经验。

### 单点登录SSO原理
单点登录(Single Sign-On, SSO)是一种身份验证的解决方案，它允许用户在多个应用间共享身份信息，也就是说，用户只需要登录一次，就可以在多个系统或者应用之间无缝访问。这大大提高了用户体验和安全性。

在企业发展初期，涉及的系统不多，可能只需要一个系统就可以满足业务需求，用户只需要用账号密码登录这个系统就可以完成验证。但是随着企业发展，业务也变得越来越多，随之演变越来越多的子系统，用户每进一个系统都可能需要登录一次，才能进行相关操作。为了解决这类问题，就产生了单点登录，也就是在一个多系统共存的环境下，用户只要在任何一个系统登录以后，就不用在其他系统再次登录，就可以得到其他所有系统的信任。

流程：
1. 用户访问app系统，而app需要登录，假设这个时候用户没用登录，则系统会触发一个302的重定向请求，转到CAS server，也就是SSO登录系统。 SSO系统发现当前用户没用登录app，就弹出用户登录页面，用户在这个页面填写用户名和密码，那么SSO系统就会进行一个验证，验证成功以后，把登录状态写入到SSO的`session`里面，浏览器也会写入SSO域下的`cookie`。 SSO系统登录完成以后，系统会生成一个ST(Service Ticket)，然后跳转到app系统，同时把ST作为参数传递给app系统。app系统拿到ST以后，从后台向SSO发送请求，验证ST是否有效。 验证通过以后，app系统把登录状态写入到`session`中，并设置app域下的`cookie`，这样，跨域的单点登录就完成了。后面在访问app系统的时候，app就是一个登录状态。
2. 此时当用户访问app2系统，app2系统没有登录会跳转到SSO，由于SSO在访问app系统的时候已经是登录态了，所以不需要重新登录验证，也就是不用输入账号密码。这个时候SSO也会生成一个ST，浏览器会跳转到app2系统，把ST作为参数传递给app2。 app2拿到ST以后，后台访问SSO验证ST是否有效，如果验证成功，那么app2会把登录状态写入到`session`，并在app2的域下写入一个`cookie`。这样，app2系统不需要再走登录流程，就已经是一个登录状态了。SSO、app、app2不同域，它们之间的`session`不共享，也没有任何问题。

SSO的具体实现因为技术栈不同会有所不同，比如OAuth2.0和OpenID Connect经常被用作于实现SSO的标准协议，

### OAuth 2.0原理
https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2

OAuth2.0是一个授权框架，使应用程序(如Github)能够获得对 HTTP 服务上的用户帐户的有限访问权限。它的工作原理是将用户身份验证委托给托管用户帐户的服务，并授权第三方应用程序访问该用户帐户。 OAuth 2 为 Web 和桌面应用程序以及移动设备提供授权流程。

定义了4种角色：

- 用户(资源所有者)：资源所有者是授权应用程序访问其帐户的用户。应用程序对用户帐户的访问仅限于授予的授权范围。
- 应用程序(客户端)：客户端是想要访问用户帐户信息的应用程序。在执行此操作之前，必须获得用户的授权，并且该授权必须由 API 进行验证。
- 资源服务器：资源服务器托管受保护的用户帐户的资源。
- 授权服务器：授权服务器验证用户的身份，然后向应用程序颁发访问令牌。

步骤：
1. 应用程序(可以是Casdoor)向用户请求访问服务器资源的授权。
2. 如果用户授权该请求，应用程序会收到授权准许(`Authorization Grant`)。
3. 应用程序通过提供其自身的身份验证和授权准许(`Authorization Grant`)来向授权服务器 (API) 请求访问令牌(`access_token`)。
4. 如果应用程序身份通过验证且授权准许(`Authorization Grant`)有效，则授权服务器(API)向应用程序颁发访问令牌(`access_token`)。授权完成。
5. 应用程序从资源服务器(API)请求资源并提供访问令牌(`access_token`)进行身份验证。
6. 如果访问令牌(`access_token`)有效，则资源服务器 (API) 向应用程序提供资源。

授权准许(`Authorization Grant`)的类型取决于应用程序请求授权所使用的方法以及 API 支持的授权类型。 OAuth 2 定义了三种主要准许(grant)类型，每种类型在不同情况下都有用：

1. 授权码(`Authorization Code`)：与服务器端应用程序一起使用。
2. 客户端凭证(`Client Credentials`)：与具有 API 访问权限的应用程序一起使用。
3. 设备代码(`Device Code`)：用于缺少浏览器或有输入限制的设备。

### 2FA
两因素认证(`Two-Factor Authentication, 2FA)`是一种安全系统，用于确保用户登录到账户或访问资源时，需要通过两种不同类型的验证方式。这种方法比仅依靠密码更安全，因为它增加了额外的验证层，使得即使密码被泄露，攻击者仍然难以访问用户账户。

身份验证因素：2FA通常结合两种不同的验证因素，分三类：
- 知识因素(Something you know)：用户知道的东西，例如密码、PIN码、回答安全问题等。
- 持有因素(Something you have)：用户拥有的物品，例如手机、硬件令牌、智能卡等。
- 固有因素(Something you are)：用户自身的特征，例如指纹、面部识别、声音识别等生物特征。

验证过程：
- 第一步：输入用户名和密码：用户首先输入他们的用户名和密码(知识因素)。
- 第二步：额外验证：系统会要求用户进行第二步验证。这可以是：  
  - 一次性密码(OTP)：通过短信、电子邮件或专用应用程序(如Google Authenticator、Authy)生成并发送给用户的随机密码。
  - 硬件令牌：用户插入或展示硬件令牌生成的代码。
  - 生物特征识别：用户通过指纹、面部识别等方式进行验证。

验证成功：如果两步验证都通过，用户将获得访问权限。
验证失败：如果任何一步失败，访问将被拒绝。

### PKCE
PKCE(`Proof Key for Code Exchange, PKCE`)是一种用于保护OAuth 2.0授权码授权流程的机制，主要目的是防止授权码拦截攻击(Authorization Code Interception Attack)。

原理：
PKCE通过在OAuth 2.0授权码请求和令牌交换过程中引入一个随机生成的`code_challenge`和`code_verifier`来增强安全性。具体来说，PKCE引入了两个新参数：
- `code_verifier`：一个高熵的随机字符串，客户端在请求`code`(授权码)时生成并保存。
- `code_challenge`：由`code_verifier`生成的一个变体，发送给授权服务器。可以是`code_verifier`本身，或者是`code_verifier`的SHA256哈希值。

流程：
1. 客户端生成一个随机的`code_challenge`和`code_verifier`，`code_verifier`可以是明文(`plain`)SHA256哈希值(`s256`)。
2. 客户端将`code_challenge`和`code_challenge_method`(`plain`或`s256`)发送给授权服务器。
   - 如：`GET /authorize?response_type=code&client_id=CLIENT_ID&redirect_uri=REDIRECT_URI&scope=SCOPE&state=STATE&code_challenge=CODE_CHALLENGE&code_challenge_method=S256`
3. 用户在授权服务器进行身份验证，同意授权，授权服务器通过重定向URI将`code`(授权码)返回给客户端。
4. 客户端将`code`(授权码)和`code_verifier`发送给授权服务器，以获取`access_token`(访问令牌)。
   - 如：`POST /token?grant_type=authorization_code&code=AUTHORIZATION_CODE&redirect_uri=REDIRECT_URI&client_id=CLIENT_ID&code_verifier=CODE_VERIFIER`
5. 授权服务器收到客户端发送的`code`(授权码)和`code_verifier`后，使用之前保存的`code_challenge`进行验证。
   - 如果`code_challenge_method`是`plain`，则直接比较`code_verifier`和`code_challenge`。
   - 如果`code_challenge_method`是`s256`，则比较`code_verifier`的SHA256哈希值和`code_challenge`。
6. 如果验证通过，授权服务器返回`access_token`(访问令牌)给客户端。

这样，即使`code`(授权码)在传输过程中被拦截，攻击者也无法使用该`code`(授权码)，因为缺少正确的`code_verifier`。

### OTP/TOTP/HOTP
OTP (One-Time Password) 是一种一次性有效的密码，通常用于两因素身份验证(2FA)。每次使用 OTP 进行身份验证后，这个密码便失效，无法再使用。OTP 提高了安全性，因为即使密码被截获或暴力破解，也不能再次使用。OTP 生成和使用的方式有很多种，包括基于时间的(TOTP)和基于事件的(HOTP)。

**TOTP(基于时间的一次性密码)**
TOTP (Time-Based One-Time Password) 是 OTP 的一种，它基于当前时间生成密码。TOTP 是 HOTP 的扩展，结合了时间元素来提高安全性。TOTP 的工作原理如下：
1. 共享密钥：用户和服务器共享一个密钥，这个密钥用于生成和验证 TOTP。
2. 时间段：TOTP 使用当前时间的一个窗口(通常是 30 秒)来生成密码。每个时间段内生成的密码是唯一的。
3. 算法：TOTP 使用 HMAC-SHA1 算法和共享密钥以及当前时间戳来生成密码。

**生成和验证**
- 生成 TOTP：用户的设备(如手机上的 Google Authenticator 应用)使用共享密钥和当前时间生成 TOTP。
- 验证 TOTP：服务器使用同样的共享密钥和当前时间计算 TOTP，并与用户提供的 TOTP 进行对比。如果匹配，则验证成功。

**优点**
- 安全性高：每个密码只能使用一次，有效时间短，降低了被攻击的风险。
- 易于使用：用户只需要输入一次性密码，而不需要记住复杂的密码。
- 灵活性：适用于各种应用场景，如登录、交易验证等。

**缺点**
- 时间同步：TOTP 依赖于设备和服务器时间的同步，如果时间不同步，可能会导致验证失败。
- 设备依赖：需要用户拥有生成 OTP 或 TOTP 的设备，如智能手机。

**HOTP(基于事件的一次性密码)**
HOTP (HMAC-Based One-Time Password) 是一种基于事件的 OTP(一次性密码)生成方法。HOTP 的生成与验证基于 HMAC-SHA1 算法及一个计数器，确保每次生成的密码都是唯一的。HOTP 的工作原理如下：
1. 共享密钥：用户和服务器共享一个密钥，这个密钥用于生成和验证 HOTP。
2. 计数器：用户和服务器都维护一个计数器，计数器初始值相同，每次生成或验证 OTP 时都会增加计数器的值。
3. HMAC-SHA1 算法：HOTP 使用 HMAC-SHA1 算法，将共享密钥和计数器作为输入，生成一个哈希值。
4. 生成 OTP：从哈希值中提取部分数据(通常是取哈希值的后几位)，将其转换为一个短数字串，即 OTP。

**生成和验证**
- 生成 HOTP：用户的设备使用共享密钥和计数器生成 HOTP。
- 验证 HOTP：服务器使用同样的共享密钥和计数器计算 HOTP，并与用户提供的 OTP 进行对比。如果匹配，则验证成功，并增加计数器的值。

**优点**
- 无时间依赖：HOTP 不依赖于设备和服务器时间的同步，避免了时间不同步带来的问题。
- 安全性高：每个密码只能使用一次，降低了密码被截获或重复使用的风险。
- 灵活性：适用于各种应用场景，如登录、交易验证等。

**缺点**
- 计数器同步：用户和服务器必须保持计数器的同步，否则会导致验证失败。如果计数器不同步，需要有机制进行重新同步。
- 复杂性：相较于静态密码，HOTP 的实现和维护相对复杂。

**使用场景**
- 银行和金融服务：TOTP/HOTP 经常用于网上银行和金融交易中，以确保只有授权用户才能进行操作。
- 企业安全：企业系统常使用 TOTP/HOTP 进行多因素身份验证，以保护敏感数据和资源。
- 社交媒体和电子邮件：为了防止账户被黑客入侵，许多社交媒体和电子邮件服务提供商都提供 TOTP/HOTP 作为额外的安全措施。

### Casbin Casdoor 介绍
> Casbin是一个强大的访问控制库，用于实现权限管理和访问控制的功能。它使用基于策略(Policy-Based)的访问控制模型，支持各种访问控制模型(如 ACL、RBAC、ABAC 等)，并提供了丰富的功能和灵活的配置选项，使开发者可以轻松地实现精细化的权限管理。 
> Casbin 的主要特点包括：
> 1. 多种访问控制模型支持：包括基于角色的访问控制(RBAC)、基于属性的访问控制(ABAC)、访问控制列表(ACL)等，满足不同场景的权限管理需求。
> 2. 灵活的策略管理：通过策略文件进行权限管理，支持多种格式(如 CSV、JSON、数据库等)，方便管理和配置访问策略。
> 3. 细粒度的权限控制：支持精细到 API 级别的权限控制，可以根据需求对用户和资源进行精确控制。
> 4. 跨语言支持：Casbin 提供了多种语言的实现，如 Go、Java、Node.js、Python 等，方便开发者在不同的技术栈中使用 Casbin 进行权限管理。

> Casdoor 是基于 Casbin 的单点登录(SSO)和权限管理系统，用于管理用户身份验证和访问权限。它提供了用户注册、登录、角色管理、权限控制等功能，可以与 Casbin 搭配使用，实现全面的身份验证和权限管理解决方案。
> 1. 单点登录(SSO)：支持统一身份验证，用户只需登录一次即可访问多个应用系统。
> 2. 用户管理：提供用户注册、登录、密码找回等功能，支持用户角色和权限的管理。
> 3. 权限控制：与 Casbin 集成，实现灵活的权限控制和策略管理，可以根据用户角色和权限设置不同的访问策略。
> 4. 开源社区：Casdoor 是开源项目，提供了完整的文档和示例代码，同时也有活跃的社区支持，方便开发者使用和定制。

### RESTful API
> RESTful API(`Representational State Transfer API`)是一种基于REST架构风格的Web服务接口。REST是一种架构风格，用于设计网络应用，使其更轻量、性能更高且更易于扩展。RESTful API利用HTTP协议作为通信标准，并遵循REST架构原则。
> REST架构原则：
> - 无状态性：每个请求从客户端到服务器必须包含足够的信息以使服务器理解请求。服务器不存储客户端的上下文，所有状态信息由客户端维护。
> - 统一接口： 资源：通过URL定位资源(通常是名词，如 `/users` 表示用户资源)。 操作：使用标准HTTP方法对资源进行操作： `GET`：获取资源 `POST`：创建资源 `PUT`：更新资源 `DELETE`：删除资源
> - 可缓存性：服务器的响应可以被标记为可缓存或不可缓存，以提高性能。
> - 分层系统：客户端不需要直接与服务器交互，可能会通过中间服务器(如代理、负载均衡器等)来提升系统的可扩展性和安全性。
> - 按需代码：在某些情况下，服务器可以返回可执行代码(如JavaScript)，使客户端能够执行。
>
> 特点：
> - 资源导向：一切皆资源，每个资源使用唯一的URL表示。 
> - 标准化操作：利用HTTP动词来描述对资源的操作。 
> - 状态表示：服务器在每次请求中发送所需的状态信息，以便客户端理解和使用。 
> - 客户端-服务器架构：客户端和服务器职责分离，提升了系统的可维护性和可扩展性。 
> - 无状态交互：每个请求独立，不依赖于之前的请求。
> 
> 优点 
> - 简单易懂：遵循标准的HTTP协议和方法，便于理解和使用。 
> - 性能高：无状态和可缓存性提高了性能。 
> - 可扩展性：分层系统和明确的资源导向使得系统易于扩展。 
> - 灵活性：支持多种数据格式(如JSON、XML)，适应不同客户端需求。
> 
> 缺点 
> - 无状态：每个请求都需要携带所有信息，可能会导致开销增加。
> - 缺乏标准化：虽然REST原则明确，但实际实现可能会有差异，导致互操作性问题。 
> - 适用于简单操作：对于复杂事务处理，REST可能不如其他协议(如GraphQL)高效。
> 
> RESTful API因其简单、灵活和高效，已经成为现代Web服务开发的主流选择。

### 国家电网项目

**简介**

这是我导师在国家电网南京经济研究院接的一个横向项目，江苏省国家电网智能诊断数据分析平台。具体来说，电网那边会给一些 Excel表格数据，数据是一些电网的指标，比如电压、电流、功率等等，还有一些指标计算公式。

我的主要工作如下：
1. 首先将这些 Excel表格数据导入到 mongodb 中，我考虑用 mongodb 存储的原因是，这些数据是一些指标，不需要关系型数据库的关联查询，而且 mongodb 的文档存储模型比较适合这种数据。且得知该系统部署在电网内网中，其实接口 QPS 不高，不需要关系型数据库的高并发处理能力，mongodb足够。而且单个 excel 使用一个文档存储，方便后续的查询。
2. 将这些表格数据实时展示到前端页面上，并在前端实现数据的增删改查功能，这里用到了 react、Antd等技术。
3. 实现将表格导出为 excel 表格。
4. 对指标的智能诊断分析，是根据电网那边给出的公式，进行计算，然后展示到前端页面上。(这块不是我负责)

**技术细节**
react、Antd、Flask、mongodb

**我的职责**
在项目中，负责实现各项重要指标的计算、以及对各指标进行灵活的增删改查，实现数据的存取以及前端展示等功能。

### 为什么使用mongodb
mongodb是一个非关系型数据库，采用文档存储模型，数据以BSON格式存储。项目中，需要导入或者导出 Excel 表格，一个 Excel文件正好可以对应mongodb中的一个文档，这样可以方便地将 Excel 中的数据导入到 mongodb 中。

### 江南布衣实习
在公司实习期间，我担任网络工程师实习生，主要负责以下工作：

1. 新入职员工设备管理：负责<u>新入职员工的电脑设备接入公司内网，包括网络配置、安装必要软件以及配备相应办公区域的打印机驱动程序。</u>通过有效的网络设备管理，确保新员工能够快速融入公司工作环境，并顺利开始工作。

2. 离职员工设备回收和资料销毁：<u>负责处理离职员工设备的回收工作，包括安全地清除设备上的敏感数据并销毁资料，保障公司数据安全和隐私保护。</u>

3. 订货会期间设备组装和网络连接：<u>在公司订货会期间，负责组装现场所需的大量电脑及相关打印设备，并确保它们在网络上正常连接，打印设备能够顺利工作。</u>通过良好的设备管理和网络连接调试，保证了订货会的顺利进行和信息传输的稳定性。

4. 员工设备报修响应与问题解决：<u>及时响应员工设备报修请求，并通过重装系统、更换硬件设备等手段迅速解决问题</u>，确保员工工作不受影响，提高公司办公效率和网络设备的稳定性。

通过这些工作，我积累了丰富的网络设备管理、故障排除和应急响应的经验，对计算机硬件、网络有了更深入的理解和实践。

> **公司内网**：指的是一个局域网(LAN)，用于连接公司内部各种设备和资源，如电脑、打印机、服务器等，使它们可以相互通信和共享资源。公司内网通常由路由器、交换机等设备组成，通过内部网络连接。
> 要使一台电脑接入公司内网，经过以下步骤：
> 
> 1. 网络配置：打开电脑的网络设置，配置IP地址、子网掩码、网关和DNS服务器等网络参数。这些参数通常由网络管理员提供或在公司内部网络设备中配置。
> 2. 身份验证：根据公司的网络策略，需要先在管理员处创建账号密码，然后在需要接入的电脑上输入用户名和密码进行身份验证，以获得对公司内部资源的访问权限。
> 3. 访问内部资源：一旦电脑成功接入公司内网，就可以通过内部网络访问共享文件夹、打印机、应用程序和其他资源。

### 印象最深的一个项目
之前在社区做开源的时候，给社区写过一个异步的权限管理持久化适配器 `async-sqlalchemy-adapter`，这个项目是我印象最深的一个项目。这个项目是基于 Casbin 的一个持久化适配器，用于将 Casbin 的策略存储到数据库中。在这个项目中，我主要负责了整个项目的设计和开发，包括数据库表结构设计、持久化逻辑实现、单元测试等。这个项目的难点在于异步编程，因为 Casbin 是一个同步的库，而我需要将其适配成异步的，这就需要对异步编程有一定的了解。通过这个项目，我学到了很多关于异步编程的知识，也提升了自己的编码能力和解决问题的能力。

**亮点**
1. **异步支持**：
    - 亮点：代码中使用了 `AsyncSession` 和 `async with` 语法，使得数据库操作支持异步执行。这在处理大量并发请求时，能够显著提升性能和响应速度。
    - 回答：这个适配器支持异步操作，可以在高并发场景下提高数据库访问的效率和响应速度，减少阻塞，提高系统的吞吐量。
2. **灵活的过滤机制**：
    - 亮点：通过 `Filter` 类和 `filter_query` 方法，可以根据多种条件灵活地筛选数据。这种设计使得策略规则的加载和过滤变得非常方便。
    - 回答：适配器提供了灵活的过滤机制，允许根据多种条件动态筛选和加载策略规则，适应各种复杂的访问控制需求。
3. **可扩展性**：
    - 亮点：支持自定义 `db_class`，可以根据不同的需求定制存储策略。同时，默认的 `CasbinRule` 类提供了一个通用的实现。
    - 回答：适配器设计时考虑了可扩展性，允许用户自定义数据库模型类，以适应不同的存储需求。这使得该适配器具有很强的适应性和可扩展性。
4. **事务处理**：
    - 亮点：使用了 `_session_scope` 作为上下文管理器，确保每次数据库操作都在事务中执行，保证数据的一致性和完整性。
    - 回答：通过使用上下文管理器管理数据库会话，确保每次操作都在事务中执行，保证了数据的一致性和完整性。

**难点**
1. **异步编程的复杂性**：
    - 难点：异步编程相比同步编程更加复杂，需要处理更多的并发问题，如资源竞争、死锁等。此外，还需要确保每个异步操作都正确处理异常，以防止未捕获的异常导致程序崩溃。
    - 回答：实现异步编程是一大难点，需要处理并发问题，并确保每个异步操作的异常处理得当，以保证系统的稳定性和可靠性。
2. **数据库模型的灵活性与一致性**：
    - 难点：适配器需要支持用户自定义的数据库模型类，这要求对模型的属性进行严格检查，确保其符合 Casbin 的策略存储要求。
    - 回答：支持自定义数据库模型类是一个挑战，因为需要确保这些自定义模型类具有所有必要的属性，符合 Casbin 的策略存储要求。
3. **复杂的策略更新逻辑**：
    - 难点：如 `update_policy` 和 `update_policies` 方法，涉及到策略规则的更新，这需要精确定位旧规则并正确替换为新规则，逻辑复杂且容易出错。
    - 回答：策略更新逻辑较为复杂，需要精确定位并替换旧的规则，确保更新操作的正确性和高效性。
4. **高效的批量操作**：
    - 难点：处理批量添加、删除和更新策略规则时，需要确保操作的高效性，同时避免数据库锁定和性能瓶颈。
    - 回答：批量操作的实现需要确保高效性，并尽量避免数据库锁定和性能瓶颈，这在高并发环境下尤为重要。

测试方面
- 适配器实现：编写了SQLAlchemy适配器，使得Casbin可以使用SQLAlchemy进行权限管理存储。
- 测试覆盖：编写了全面的测试用例，覆盖了适配器的所有主要功能，包括政策的添加、删除、更新和过滤。

技术细节
- 测试框架：说明使用了`unittest`库，并扩展了`IsolatedAsyncioTestCase`来测试异步功能。
- 测试用例设计：这个适配器需要在异步环境下持久管理 Casbin 策略，所以需要对策略的增删改查、保存、以及策略过滤查找等进行测试。测试用例保证了覆盖基本功能测试。
    - 测试添加一个策略，添加多个策略
    - 测试删除一个策略，删除多个策略，删除经过过滤的策略
    - 测试更新一个策略，测试更新多个策略。

通过强调这些亮点和难点，可以展示你在该项目中的成就和技术深度，突出你在异步编程、高性能数据库访问和复杂逻辑处理方面的经验和能力。

### 学生生涯遇到的挫折和应对策略
高一升高二的时候，当时是从一个普通班进入全校最好的英才班嘛。然后第一次考试，我考了全班倒数第三，当时从普通班的前几名到倒数第三，可谓是很大的打击，我从小到大就没考过这么差的名词，当时很伤心沮丧吧，回家在房间偷偷哭。

但我也知道，哭是没有用的，我跟别人的差距是确实存在的，从那次后，我比别人更加努力，我花别人更多的时间去学习，去做题，去复习。不会的知识点也请教老师和同学，他们也都很乐意帮助我，我很感谢他们。争取把遇到的每个不会的知识点攻克，我也慢慢的感觉到自己的进步，从才开始的在英才班倒数，到中游 10-20名这样子，到最后高考的时候，考了英才班第四名。

我觉得吧，从这个经历中，我学到了很多，比如说遇到困难要找对方法才行，其次就是要有毅力，不要轻易放弃，最后就是要有信心，相信自己可以做到。

最让我有感触的是，让我学会了 任何时候，都要对自己有信心，不要怀疑自己的能力，相信自己可以做到。这点在我后面的大学学习、跨专业保研中起到了很重要的作用。

## Java基础

### Java和C++的区别
Java 和 C++ 都是面向对象的语言，都支持封装、继承和多态，但还是有很多不同的地方：
- Java 不提供指针来直接访问内存，程序内存更加安全，C++ 支持指针；
- Java 的类是单继承的，C++ 支持多继承； Java 的接口可以多继承；
- Java 有自动内存管理垃圾回收机制(GC)，而C++ 没有垃圾回收机制，程序员需要手动释放无用内存；
- C++ 支持方法重载和操作符重载，Java 只支持方法重载(操作符重载增加了复杂性，与Java最初的设计思想不符)。

> C 是面向过程的语言，C++ 既支持面向对象也支持面向过程，算是半面向对象语言，Java 是面向对象的语言。

### 移位运算符
`<<`：左移运算符，向左移若干位，高位丢弃，低位补零。`x << 1`,相当于 x 乘以 2(不溢出的情况下)。
`>>`：带符号右移，向右移若干位，高位补符号位，低位丢弃。正数高位补 0,负数高位补 1。`x >> 1`,相当于 x 除以 2。
`>>>`：无符号右移，忽略符号位，空位都以 0 补齐。

使用 `<<`、 `>>` 和`>>>`转换成的指令码运行起来会更高效些。由于 `double`，`float` 在二进制中的表现比较特殊，因此不能来进行移位操作。移位操作符实际上支持的类型只有 `int` 和 `long`，编译器在对 `short`、`byte`、`char` 类型进行移位前，都会将其转换为`int`类型再操作。

如果移位的位数超过数值所占有的位数会怎样？
当 `int` 类型左移/右移位数大于等于 32 位操作时，会先 **求余(%)** 后再进行左移/右移操作。也就是说左移/右移 32 位相当于不进行移位操作(32%32=0)，左移/右移 42 位相当于左移/右移 10 位(42%32=10)。当 `long` 类型进行左移/右移操作时，由于 `long` 对应的二进制是 64 位，因此求余操作的基数也变成了 64。也就是说：`x<<42`等同于`x<<10`，`x>>42`等同于`x>>10`，`x>>>42`等同于`x>>>10`。

### 基本数据类型/包装类型 及区别
**8种基本数据类型**
- 6 种数字类型： 
  - 4 种整数型：`byte`、`short`、`int`、`long`
  - 2 种浮点型：`float`、`double`
- 1 种字符类型：`char`
- 1 种布尔型：`boolean`

> 注意：Java 里使用 `long` 类型的数据一定要在数值后面加上 `L` ，否则将作为整型解析。 
> `char a = 'h'`，`char`： 单引号，`String a = "hello"`，`String`： 双引号。

**8种包装类型** 
`Byte`、`Short`、`Integer`、`Long`、`Float`、`Double`、`Character`、`Boolean` 。

**区别**
- 用途：基本类型用来定义常量和局部变量，包装类型可用于泛型，而基本类型不可以。
- 存储方式：基本数据类型的局部变量存放在 Java 虚拟机栈中的局部变量表中，基本数据类型的成员变量(未被 `static` 修饰 )存放在 Java 虚拟机的堆中。包装类型属于对象类型，几乎所有对象实例都存在于堆中。
- 占用空间：相比于包装类型(对象类型)， 基本数据类型占用的空间往往非常小。
- 默认值：成员变量包装类型不赋值就是 `null` ，而基本类型有默认值且不是 `null`。
- 比较方式：对于基本数据类型来说，`==` 比较的是值。对于包装数据类型来说，`==` 比较的是对象的内存地址。所有整型包装类对象之间值的比较，全部使用 `equals()` 方法。

### 包装类的缓存机制
Java 基本数据类型的包装类型的大部分都用到了缓存机制来提升性能。`Byte`,`Short`,`Integer`,`Long` 这 4 种包装类默认创建了数值 [-128，127] 的相应类型的缓存数据，`Character` 创建了数值在 [0,127] 范围的缓存数据，`Boolean` 直接返回 `True` or `False`。

### 什么是自动拆装箱
```java
Integer i = 10;  //装箱
int n = i;   //拆箱
```

- 装箱：将基本类型用它们对应的引用类型包装起来；
- 拆箱：将包装类型转换为基本数据类型；

从字节码来看，装箱调用包装类的 `valueOf()` 方法，拆箱调用 `xxxValue()` 方法。

### 如何解决浮点数运算的精度丢失问题

无限循环的小数存储在计算机时，只能被截断，所以就会导致小数精度发生损失的情况。`BigDecimal` 可以实现对浮点数的运算，不会造成精度丢失。大部分需要浮点数精确运算结果的业务场景(比如涉及到钱的场景)都是通过 `BigDecimal` 来做的。

### 局部变量/成员变量/静态变量

- 语法形式：成员变量可以被 `public`,`private`,`static` 等修饰符所修饰，而局部变量不能被访问控制修饰符及 `static` 所修饰；但是，成员变量和局部变量都能被 final 所修饰。
- 存储方式：如果成员变量是使用 `static` 修饰的，那么这个成员变量是属于类的，如果没有使用 `static` 修饰，这个成员变量是属于实例的。而对象存在于堆内存，局部变量则存在于栈内存。
- 生存时间：成员变量是对象的一部分，它随着对象的创建而存在，而局部变量随着方法的调用而自动生成，随着方法的调用结束而消亡。
- 默认值：成员变量如果没有被赋初始值，则会自动以类型的默认值而赋值(一种情况例外：被 `final` 修饰的成员变量也必须显式地赋值)，而局部变量则不会自动赋值。

静态变量是被 `static` 关键字修饰的变量。它可以被类的所有实例共享，无论一个类创建了多少个对象，它们都共享同一份静态变量。也就是说，即使创建多个对象，静态变量只会被分配一次内存，这样可以节省内存。

### 重载和重写
- 重载就是同名的方法能够根据输入数据的不同，做出不同的处理。重载发生在同一个类中(或者父类和子类之间)，方法名必须相同，参数类型不同、个数不同、顺序不同，方法返回值和访问修饰符可以不同。
- 重写发生在运行期，是子类对父类的允许访问的方法的实现过程进行重新编写。
  - 方法名、参数列表必须相同，子类方法返回值类型应比父类方法返回值类型更小或相等，抛出的异常范围小于等于父类，访问修饰符范围大于等于父类。
  - 如果父类方法访问修饰符为 private/final/static 则子类就不能重写该方法，但是被 static 修饰的方法能够被再次声明。
  - 构造方法无法被重写

### 面向对象和面向过程区别
两者的主要区别在于解决问题的方式不同：
- 面向过程把解决问题的过程拆成一个个方法，通过一个个方法的执行解决问题。
- 面向对象会先抽象出对象，然后用对象执行方法的方式解决问题。

另外，面向对象开发的程序一般更易维护、易复用、易扩展。

### 接口和抽象类区别
共同点： 都不能被实例化
https://learn.skyofit.com/archives/351
**区别**
- 子类使用`extends`关键字来继承抽象类，只能继承 1 个抽象类；子类使用关键字`implements`来实现接口，可以实现多个接口。
- 抽象类可以有构造方法，接口不能有构造方法。
- 抽象类允许有普通方法，接口中的方法默认是`public abstract`类型(JDK8后允许使用`default`、`static`定义非抽象方法)。
- 抽象类允许有成员变量，接口中只允许有常量(默认是`public static final`类型)。
- 抽象类中的抽象方法可以有访问修饰符(可以是`public`、`protected`、`private`)，接口中的抽象方法默认是`public`类型。
- 抽象类可以有 `main` 方法，接口中不能有 `main` 方法。

### 堆和栈的区别
https://blog.csdn.net/qq_44944221/article/details/126692973
- 栈：是运行时单位，代表逻辑，内含基本数据类型和堆中的对象引用，所在区域连续，没有碎片；
- 堆：是存储单元，代表着数据，可以被多个栈共享，所在区域不连续，有碎片；

区别：
- 功能不同：栈内存用来存储局部变量和方法调用，而堆内存用存储Java中的对象；无论是成员变量、局部变量、还是类变量他们指向的对象都存储在堆内存中；
- 共享性不同：栈是线程私有，而堆是线程共享；
- 异常错误不同：当内存不足时；栈抛出的是`StackOverFlowError`异常，而堆抛出的是`OutOfMemoryError`；
- 空间大小不同：堆空间大小远远大于栈的内存空间。

### 深拷贝/浅拷贝/引用拷贝
- 深拷贝： 完全复制整个对象，包括这个对象所包含的内部对象。
- 浅拷贝： 浅拷贝会在堆上创建一个新的对象(区别于引用拷贝的一点)，不过，如果原对象内部的属性是引用类型的话，浅拷贝会直接复制内部对象的引用地址，也就是说拷贝对象和原对象共用同一个内部对象。
- 引用拷贝： 引用拷贝是两个不同的引用指向同一个对象。这不就是拷贝了一份引用吗哈哈哈？

### Object 类
Object 类是一个特殊的类，是所有类的父类。主要提供了以下 11 个方法：
```java
//native 方法，用于返回当前运行时对象的 Class 对象，使用了 final 关键字修饰，故不允许子类重写。
public final native Class<?> getClass()

//native 方法，用于返回对象的哈希码，主要使用在哈希表中，比如 JDK 中的HashMap。
public native int hashCode()

//用于比较 2 个对象的内存地址是否相等，String 类对该方法进行了重写以用于比较字符串的值是否相等。
public boolean equals(Object obj)

//native 方法，用于创建并返回当前对象的一份拷贝。
protected native Object clone() throws CloneNotSupportedException

//返回类的名字实例的哈希码的 16 进制的字符串。建议 Object 所有的子类都重写这个方法。
public String toString()

//native 方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。
public final native void notify()

//native 方法，并且不能重写。跟 notify 一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。
public final native void notifyAll()

//native方法，并且不能重写。暂停线程的执行。注意：sleep 方法没有释放锁，而 wait 方法释放了锁 ，timeout 是等待时间。
public final native void wait(long timeout) throws InterruptedException

//多了 nanos 参数，这个参数表示额外时间(以纳秒为单位，范围是 0-999999)。 所以超时的时间还需要加上 nanos 纳秒。。
public final void wait(long timeout, int nanos) throws InterruptedException

//跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念
public final void wait() throws InterruptedException

// 实例被垃圾回收器回收的时候触发的操作
protected void finalize() throws Throwable { }
```

### 单例模式
- 饿汉式：饿汉式单例模式在类加载时就完成实例化，线程安全，简单但可能会造成资源浪费。
- 懒汉式：懒汉式单例模式在第一次调用 `getInstance` 方法时创建实例，线程不安全，需要额外处理同步。
- 线程安全的懒汉式
  - 同步方法：在 `getInstance` 方法上加 `synchronized` 关键字，保证线程安全，但是效率低。
  - 双重检查锁定：在 `getInstance` 方法内部进行双重检查，保证只有第一次调用时才会加锁，提高效率。
- 静态内部类：利用静态内部类来实现懒加载和线程安全。
- 枚举：枚举实现单例模式是最简洁、安全的实现方式，可以防止反射和序列化攻击。


```java
// 饿汉式
public class Singleton {
    private static final Singleton instance = new Singleton();
    private Singleton() {}
    public static Singleton getInstance() {
        return instance;
    }
}

// 懒汉式
public class Singleton {
    private static Singleton instance;
    private Singleton() {}
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}

// 线程安全的懒汉式-同步方法
public class Singleton {
    private static Singleton instance;
    private Singleton() {}
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}

// 线程安全的懒汉式-双重检查锁定
public class Singleton {
    // 单例模式中用于保存实例的字段，被声明为volatile，确保对该变量的写入操作会立即反映到所有线程中，这样可以防止可能发生的指令重排序问题。
    private volatile static Singleton uniqueInstance;
    // 私有的构造方法确保该类不能在外部被初始化，只能通过getUniqueInstance()方法获取实例
    private Singleton() {
    }
    // 双重检查锁定的机制，实现对外提供的获取单例实例的方法。
    public static Singleton getInstance() {
        // 第一层检查：首先检查 uniqueInstance 是否为 null。如果不是 null，意味着实例已经被创建，则直接返回这个实例。
        if (uniqueInstance == null) {
            // 类对象加锁，表示进入同步代码前要获得 Singleton类 的锁
            synchronized (Singleton.class) {
                // 第二层检查：在同步代码块内再次检查 uniqueInstance 是否为 null。
                // 这种双重检查是为了在等待锁的线程获取到锁后再次确认实例是否已经被创建，因为在等待锁的过程中可能有其他线程已经创建了实例。
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }
    public static void main(String[] args) {
        System.out.println(getInstance());
    }

}

// 静态内部类
public class Singleton {
    private Singleton() {}
    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }
    public static Singleton getInstance() {
        return SingletonHolder.INSTANCE;
    }
}

// 枚举
public enum Singleton {
    // 注意 上面不是 class 是 enum
    INSTANCE;
    public void someMethod() {
        // do something
    }
    public static void main(String[] args) {
        Singelton singleton = Singleton.INSTANCE;
        singleton.someMethod();
    }
}
```

### 形参&实参
- 形参(形式参数，Parameters)：用于定义函数/方法，接收实参，不需要有确定的值。
- 实参(实际参数，Arguments)：用于传递给函数/方法的参数，必须有确定的值。

### 值传递&引用传递
- 值传递：方法接收的是实参值的拷贝，会创建副本。
- 引用传递：方法接收的直接是实参所引用的对象在堆中的地址，不会创建副本，对形参的修改将影响到实参。

Java 中只有值传递，C++ 中有值传递和引用传递。

### ==和equals()区别
- `==` 可以用来比较基本数据类型和引用数据类型
  - 基本数据类型：比较的是值是否相等
  - 引用数据类型：比较的是引用地址是否相等
  - Java只有值传递，不管是基本数据类型还是引用数据类型，比较的都是值，只是引用类型变量存的值是对象的地址。
- `equals()` 不能用于判断基本数据类型的变量，只能用来判断两个对象是否相等。
  - 如果没有重写`equals()`方法， 子类调用`Object` 类中的`equals()`方法，等价于通过“==”比较这两个对象，即比较的是两个对象的引用地址。
  - 一般重写`equals()`方法来比较两个对象中的属性是否相等；若属性相等，则返回 `true`(即认为这两个对象相等)。

### hashCode()和equal()
`hashCode()` 的作用是获取哈希码(`int` 整数)，也称为散列码。哈希码的作用是确定该对象在哈希表中的索引位置。`hashCode()` 定义在 `Object` 类中，意味着 Java 中的任何类都有 `hashCode()`。注意：`Object` 的 `hashCode()` 方法是本地方法，也就是用 C 语言或 C++ 实现的。

`hashCode()` 和 `equals()`都是用于比较两个对象是否相等。JDK 同时提供这两个方法，`hashCode()`方法可以大大减少`equals()`方法的调用次数，从而提高程序的性能。
- 如果两个对象的`hashCode` 值相等，那这两个对象不一定相等(**哈希碰撞**)。
- 如果两个对象的`hashCode` 值相等并且`equals()`方法也返回 `true`，认为这两个对象相等。
- 如果两个对象的`hashCode` 值不相等，可以直接认为这两个对象不相等。

**为什么重写 `equals()` 时必须重写 `hashCode()` 方法？**
因为两个相等对象的 `hashCode` 值必须相等。也就是说如果 `equals` 方法判断两个对象是相等的，那这两个对象的 `hashCode` 值也要相等。如果重写 `equals()` 时没有重写 `hashCode()` 方法的话就可能会导致 `equals` 方法判断是相等的两个对象，`hashCode` 值却不相等。

### String/StringBuffer/StringBuilder
- `String` ：字符串常量，不可变，线程安全，适用于少量的字符串操作的情况。
- `StringBuffer` ：字符串变量(线程安全)，适用于多线程下大量字符串操作的情况。
- `StringBuilder` ：字符串变量(非线程安全)，适用于单线程下大量字符串操作的情况。

- 每次对 `String` 类型进行改变的时候，都会生成一个新的 `String` 对象，然后将指针指向新的 `String` 对象。
- `StringBuffer` 改变时会对本身进行操作，而不是生成新的对象并改变对象引用。
- `StringBuilder` 相比使用 `StringBuffer` 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险。

**`String` 为什么不可变？**
- `String`类内保存字符串的`char`数组被 `final` 修饰且为私有的，并且 `String` 类没有提供/暴露修改这个字符串的方法。
- `String` 类被 `final` 修饰导致其不能被继承，进而避免了子类破坏 `String` 不可变。

### 字符串拼接使用+还是StringBuilder？
Java中“+”和“+=” 实际上是通过 `StringBuilder` 调用 `append()` 方法实现的，拼接完成之后调用 `toString()` 得到一个 String 对象。在循环内使用“+”进行字符串的拼接的话，存在比较明显的缺陷：编译器不会创建单个 `StringBuilder` 以复用，会导致创建过多的 `StringBuilder` 对象。

### String.equals()/Object.equals()
`String` 中的 `equals` 方法是被重写过的，比较的是 `String` 字符串的值是否相等。 `Object` 的 `equals` 方法是比较的对象的内存地址。

### 字符串常量池
**字符串常量池** 是 JVM 为了提升性能和减少内存消耗针对字符串(String 类)专门开辟的一块区域，主要目的是为了避免字符串的重复创建。

### 异常
异常的共同祖先是 `Throwable` 类，其有两个重要的子类：
`Error`： 程序无法处理的错误，不建议通过 `catch` 捕获。一般由 JVM 抛出，线程终止执行。
`Exception`： 程序本身可以处理的异常，可以通过 `catch` 来进行捕获。

### Checked/Unchecked Exception
- `Checked Exception`即受检查异常 ，在编译过程中，若受检查异常没有被 `catch` 或者 `throws` 关键字处理的话，就没办法通过编译。
  - 除了 `RuntimeException` 及其子类以外，其他的 `Exception` 类及其子类都属于受检查异常 。常见的受检查异常有：`ClassNotFoundException`、`FileNotFoundException`、`SQLException` 等。
- `Unchecked Exception` 即不受检查异常 ，在编译过程中 ，即使不处理也可以正常通过编译。
  - `RuntimeException` 及其子类属于不受检查异常。常见的不受检查异常有：`NullPointerException`、`ArrayIndexOutOfBoundsException`、`IllegalArgumentException` 等。

### try-catch-finally
`try`：用于捕获异常。其后可接零个或多个 `catch` 块，如果没有 `catch` 块，则必须跟一个 `finally` 块。
`catch`：用于处理 `try` 捕获到的异常。
`finally`：无论是否捕获或处理异常，`finally` 块里的语句都会被执行。当在 `try` 块或 `catch` 块中遇到 `return` 语句时，`finally` 语句块将在方法返回之前被执行。

> 注意：不要在 `finally` 语句块中使用 `return` ! 当 `try` 语句和 `finally` 语句中都有 `return` 语句时，`try` 语句块中的 `return` 语句会被忽略。

### 泛型
- 使用泛型参数，可以增强代码的可读性以及稳定性。
- 泛型一般有三种使用方式：泛型类、泛型接口、泛型方法。

### 反射
反射是框架的灵魂，主要是因为它赋予了在运行时分析类以及执行类中方法的能力。通过反射可以获取任意一个类的所有属性和方法，还可以调用这些方法和属性。

- 优点：可以让代码更加灵活、为各种框架提供开箱即用的功能提供了便利。
- 缺点：在运行时有了分析操作类的能力，这同样也增加了安全问题。比如可以无视泛型参数的安全检查(泛型参数的安全检查发生在编译时)。另外，反射的性能也要稍差点，不过，对于框架来说实际是影响不大的。

### 注解
注解使用了反射，可以看作是一种特殊的注释，主要用于修饰类、方法或者变量，提供某些信息供程序在编译或者运行时使用。

注解只有被解析之后才会生效，常见的解析方法有两种：
- 编译期直接扫描：编译器在编译代码的时候扫描对应的注解并处理，如某个方法使用 `@Override` 注解，编译器在编译的时候就会检测当前的方法是否重写了父类对应的方法。
- 运行期通过反射处理：像框架中自带的注解(比如 `Spring` 框架的 `@Value`、`@Component`)都是通过反射来进行处理的。

### 序列化/反序列化
如果需要持久化 Java 对象比如将 Java 对象保存在文件中，或者在网络传输 Java 对象，这些场景都需要用到序列化。
- 序列化：将数据结构或对象转换成二进制字节流的过程
- 反序列化：将在序列化过程中所生成的二进制字节流转换成数据结构或者对象的过程

序列化和反序列化常见应用场景：
- 对象在进行网络传输(比如远程方法调用 RPC 的时候)之前需要先被序列化，接收到序列化的对象之后需要再进行反序列化；
- 将对象存储到文件之前需要进行序列化，将对象从文件中读取出来需要进行反序列化；
- 将对象存储到数据库(如 Redis)之前需要用到序列化，将对象从缓存数据库中读取出来需要反序列化；
- 将对象存储到内存之前需要进行序列化，从内存中读取出来之后需要进行反序列化。

### 语法糖
语法糖(Syntactic sugar) 代指的是编程语言为了方便程序员开发程序而设计的一种特殊语法，这种语法对编程语言的功能并没有影响。实现相同的功能，基于语法糖写出来的代码往往更简单简洁且更易阅读。

Java 中最常用的语法糖主要有**switch语句**、**自动拆装箱**、**for-each循环**、**try-with-resources** 语法、**lambda 表达式**等。

### Java设计模式
Java 设计模式是一种在软件开发中常用的解决特定问题的通用方案。设计模式可以提高代码的可维护性、可重用性和可扩展性。根据目的和使用场景，设计模式通常分为三大类：
- 创建型模式
  - 工厂模式：工厂模式定义了一个用于创建对象的接口，但由子类决定实例化哪个类。它使得类的实例化延迟到子类。
  - 单例模式：单例模式确保一个类只有一个实例，并提供一个全局访问点。
- 结构型模式
  - 适配器模式：适配器模式将一个类的接口转换成客户希望的另一个接口，使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。
  - 装饰器模式：装饰者模式允许向一个现有的对象添加新的功能，同时又不改变其结构。
- 行为型模式
  - 策略模式：策略模式定义了一系列算法，并将每一个算法封装起来，使它们可以相互替换。策略模式使得算法可独立于使用它的客户而变化。
  - 观察者模式：观察者模式定义对象间的一种一对多的依赖关系，使得每当一个对象改变状态，则所有依赖于它的对象都会得到通知并被自动更新。

### JDK中用到哪些设计模式
1. **单例模式(Singleton Pattern)**
    - **`java.lang.Runtime#getRuntime()`**：`Runtime`类确保只有一个实例存在，并提供了一个全局访问点。
    - **`java.awt.Desktop#getDesktop()`**：`Desktop`类也是一个单例模式的实现。
2. **工厂模式(Factory Pattern)**
    - **`java.util.Calendar#getInstance()`**：根据不同的参数，返回不同的`Calendar`子类实例。
    - **`java.sql.DriverManager#getConnection()`**：根据提供的URL、用户名和密码返回不同类型的数据库连接。
3. **抽象工厂模式(Abstract Factory Pattern)**
    - **`javax.xml.parsers.DocumentBuilderFactory`**：用于创建不同类型的XML解析器。
    - **`javax.xml.transform.TransformerFactory`**：用于创建不同类型的XML转换器。
4. **建造者模式(Builder Pattern)**
    - **`java.lang.StringBuilder`** 和 **`java.lang.StringBuffer`**：用于构建字符串。
    - **`java.nio.ByteBuffer`**：用于构建字节缓冲区。
5. **原型模式(Prototype Pattern)**
    - **`java.lang.Object#clone()`**：所有实现了`Cloneable`接口的类都可以使用`clone`方法来复制对象。
6. **适配器模式(Adapter Pattern)**
    - **`java.util.Arrays#asList()`**：将数组转换为`List`。
    - **`java.io.InputStreamReader`** 和 **`java.io.OutputStreamWriter`**：将字节流转换为字符流。
7. **装饰器模式(Decorator Pattern)**
    - **`java.io.BufferedInputStream`** 和 **`java.io.BufferedOutputStream`**：为现有的输入/输出流添加额外的功能。
    - **`java.util.Collections#synchronizedList()`**、`#unmodifiableList()`**：为现有的集合添加额外的行为。
8. **观察者模式(Observer Pattern)**
    - **`java.util.Observer`** 和 **`java.util.Observable`**：用于实现观察者模式。
    - **`javax.swing.event.EventListenerList`**：用于管理事件监听器。
9. **策略模式(Strategy Pattern)**
    - **`java.util.Comparator`**：用于定义比较两个对象的策略。
    - **`javax.swing.text.Document#insertString()`**：不同的插入策略。
10. **责任链模式(Chain of Responsibility Pattern)**
    - **`java.util.logging.Logger#log()`**：通过不同的`Handler`处理日志记录请求。
11. **状态模式(State Pattern)**
    - **`javax.swing.JComponent#paint(Graphics)`**：通过状态模式来管理组件的绘制状态。
12. **模板方法模式(Template Method Pattern)**
    - **`java.util.AbstractList`**、`java.util.AbstractSet`**：这些抽象类定义了集合的一些模板方法，具体的子类来实现具体的方法。

这些设计模式的使用，使得JDK在设计和实现上更加灵活、可扩展且易于维护。这些模式不仅仅是代码结构的规范，它们还体现了面向对象设计的基本原则，如单一职责原则、开闭原则和依赖倒置原则。

### 内存溢出/内存泄露区别
- 内存溢出(Memory Overflow)：内存溢出是指程序尝试使用超过其可用内存量的情况。这通常会导致程序崩溃或产生意外行为。内存溢出可以发生在堆栈或堆内存中。
  - 常见原因：
    - 递归太深：没有终止条件的递归函数调用会导致堆栈溢出。
    - 内存分配过多：程序请求的内存超过了系统可用的物理内存或虚拟内存。
- 内存泄露(Memory Leak)：内存泄漏是指程序在运行过程中，动态分配了内存但未能正确释放，导致这些内存不可达并且不能再被使用。内存泄漏会导致程序占用的内存越来越多，最终可能导致系统内存耗尽。
  - 常见原因：
    - 未释放内存：程序分配了内存但没有调用适当的释放函数(如 `free`)。
    - 指针丢失：丢失对已分配内存的引用，使得内存无法被释放。

**区别**
- 内存溢出是指程序尝试使用超过其可用内存量的情况，而内存泄漏是指程序动态分配的内存未被正确释放，导致内存逐渐耗尽。
- 二者成因不同。内存溢出：递归太深、内存分配过多等。内存泄漏：未释放内存、指针丢失等。
- 二者影响不同。内存溢出：立即导致程序崩溃或异常。内存泄漏：程序占用的内存越来越多，最终可能导致系统性能下降或内存耗尽而崩溃。

## Java集合

### List/Set/Queue/Map区别
- List： 存储的元素是有序的、可重复的。
- Set： 存储的元素不可重复的。
- Queue： 按特定的排队规则来确定先后顺序，存储的元素是有序的、可重复的。
- Map： 使用键值对(key-value)存储，key 是无序的、不可重复的，value 是无序的、可重复的，每个键最多映射到一个值。

### 如何选用集合?
主要根据集合的特点来选择合适的集合。如：
- 需要根据键值获取到元素值时就选用 `Map` 接口下的集合，需要排序时选择 `TreeMap` ,不需要排序时就选择 `HashMap` ,需要保证线程安全就选用 `ConcurrentHashMap` 。
- 只需要存放元素值时，就选择实现 `Collection` 接口的集合，需要保证元素唯一时选择实现 `Set` 接口的集合比如 `TreeSet` 或 `HashSet` ，不需要就选择实现 `List` 接口的比如 `ArrayList` 或 `LinkedList` ，然后再根据实现这些接口的集合的特点来选用。

### 为什么要使用集合？
在实际开发中，存储的数据类型多种多样且数量不确定。相较于数组，Java 集合提供了更灵活、更有效的方法来存储多个数据对象，其优势在于集合的大小可变、支持泛型、具有内建算法等。总的来说，Java 集合提高了数据的存储和处理灵活性，可以更好地适应现代软件开发中多样化的数据需求，并支持高质量的代码编写。

### ArrayList和Array区别
`ArrayList` 内部基于动态数组实现，比 `Array`(静态数组) 更加灵活：
- `ArrayList`可动态扩容/缩容，`Array` 创建后不能改变长度。
- `ArrayList` 可使用泛型确保类型安全，`Array` 则不可以。
- `ArrayList` 中只能存对象，存基本类型数据时要用对应包装类(如`Integer`)。`Array` 可直接存储基本类型数据，也可存储对象。
- `ArrayList` 提供增删改查等 API 操作方法，如 `add()`、`remove()`等。`Array` 只是一个固定长度数组，只能按照下标访问元素，无动态添加、删除元素能力。
- `ArrayList`创建时不需指定大小，而`Array`创建时必须指定大小。

> 以无参数构造方法创建 `ArrayList` 时，实际上初始化赋值的是一个空数组。当真正对数组进行添加元素操作时，才真正分配容量。即向数组中添加第一个元素时，数组容量扩为 10。之后每次扩容容量变为原来的 1.5 倍。

### ArrayList和LinkedList区别
- `ArrayList` 底层基于`Object`数组实现的，`LinkedList` 是基于双向链表实现的(JDK1.6之前是循环链表，1.7改为双向链表)。
  - `ArrayList`插入和删除元素的时间复杂度受元素位置的影响。`add()`方法默认在尾部添加元素为O(1)，在指定位置插入元素或者删除元素时间复杂度为O(n)，因为需要移动元素。
  - `LinkedList`插入和删除元素的时间复杂度为O(1)，因为只需要改变指针指向。如果要在指定位置插入或者删除元素，时间复杂度为O(n)，需要先移动到指定位置再插入和删除。
- 二者都是线程不安全的。
- `ArrayList` 随机访问效率高(实现了`RandomAccess()`接口)，`LinkedList` 插入删除效率高。
- 占用空间：`ArrayList` 主要体现在在列表的结尾会留一定的容量空间，`LinkedList` 每个元素占用空间比前者大(要存放直接后继和直接前驱以及数据)。

> 项目中一般不会使用 `LinkedList`。

### ArrayList和LinkedList插入/删除效率
`ArrayList`
- 头部插入/删除：O(n), 需要移动元素。
- 指定位置插入/删除：O(n), 需要移动元素。
- 尾部插入/删除：O(1), 直接在尾部添加或删除元素。尾部插入时，当容量已到极限并需扩容时，需执行一次 O(n) 的操作将原数组复制到新的更大的数组中，然后再执行 O(1) 的操作添加元素。
`LinkedList`
- 头部插入/删除：O(1), 只需改变指针指向。
- 尾部插入/删除：O(1), 只需改变指针指向。
- 指定位置插入/删除：O(n), 需要先移动到指定位置再插入和删除。

### CopyOnWriteArrayList
`CopyOnWriteArrayList` 是一种线程安全的集合类，适用于读操作远多于写操作的场景。它使用了一种叫做 `Copy-On-Write(COW)`的策略，即每次写操作(如添加、删除、更新元素)都会创建一个新的数组副本。这种策略在以下场景中非常有用：
- 读多写少：大部分操作都是读取数据，写操作相对较少。
- 不要求实时性：因为写操作需要创建数组副本，可能会有短暂的延迟。

`ReentrantReadWriteLock` 读写锁的设计思想非常类似，即读读不互斥、读写互斥、写写互斥(只有读读不互斥)。`CopyOnWriteArrayList` 更进一步地实现了这一思想。为了将读操作性能发挥到极致，`CopyOnWriteArrayList` 中的读取操作是完全无需加锁的，写入操作也不会阻塞读取操作，只有写写才会互斥。

**优点**
- 线程安全：所有读操作不需要加锁，因为读操作只是读取一个不可变的数组副本。
- 简化并发控制：不需要显式的同步块或锁定机制。
- 迭代器安全：迭代器不会抛出 `ConcurrentModificationException`，因为迭代时是基于快照的。

**缺点**
- 内存开销大：每次写操作都会创建数组副本，可能会占用较多的内存。
- 写操作开销大：由于需要复制整个数组，写操作的时间复杂度为 O(n)。
- 读取旧数据：在高并发场景下，可能会读取到旧的数据。

**适用场景**
- 配置类数据：如系统配置参数、常量集合等。
- 事件监听器列表：如监听器的添加和删除较少，事件触发较频繁。

**不适用场景**
- 高并发写操作：如果有大量的写操作，那么 `CopyOnWriteArrayList` 的性能会变差。
- 实时性要求高的场景：在需要读取最新数据的情况下，可能不适合使用。

**结论**
`CopyOnWriteArrayList` 适合用于读操作远多于写操作且不要求实时性数据更新的场景。但对于高并发写操作或需要读取最新数据的场景，建议使用其他更合适的数据结构或并发容器，如 `ConcurrentHashMap` 或 `ConcurrentLinkedQueue`。

### HashSet/LinkedHashSet/TreeSet
- 都是 `Set` 接口的实现类，都保证元素唯一，且都不是线程安全的。
- 主要区别在于底层数据结构不同。
  - `HashSet` 底层是哈希表(`HashMap`)。
  - `LinkedHashSet` 底层是链表和哈希表，元素的插入和取出顺序满足 FIFO。
  - `TreeSet` 底层数据结构是红黑树，元素是有序的，排序的方式有自然排序和定制排序。
- 底层数据结构不同导致三者应用场景不同。`HashSet` 用于不需要保证元素插入和取出顺序的场景，`LinkedHashSet` 用于保证元素的插入和取出顺序满足 FIFO 的场景，`TreeSet` 用于支持对元素自定义排序规则的场景。

### Map(重要)
### HashMap/HashTable区别
- `HashMap` 是非线程安全的，`HashTable` 是线程安全的(内部方法经过`synchronized`修饰)。
- JDK1.8以后 `HashMap` 在解决哈希冲突时有了较大的变化，当链表长度大于阈值(默认为8)时，将链表转化为红黑树(将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树)，以减少搜索时间。`Hashtable` 没有这样的机制。
- `HashTable`中有锁，所以效率比 `HashMap` 低。
- `HashMap` 可以有一个 `null` key和多个`null` value，而 `HashTable` 不允许有`null` key和value，否则会报错`NullPointerException`。
- 初始容量大小和每次扩充容量大小的不同： 
  - `Hashtable` 默认的初始大小为 11，之后每次扩充，容量变为原来的 2n+1。
  - `HashMap` 默认的初始化大小为 16。之后每次扩充，容量变为原来的 2 倍。
  - 创建时如果给定了容量初始值，`Hashtable` 会直接使用给定的大小，而 `HashMap` 总是使用 2 的初始容量的幂作为哈希表的大小。

> `HashTable` 已经被淘汰，不建议使用。
> `HashMap` `loadFactor` 负载因子默认为 0.75，即当 HashMap 中的元素个数超过容量的 75% 时，就会进行扩容操作。`threshold = capacity * loadFactor`

### HashMap长度为什么是2的幂次方
key经过扰动函数`hash()`后得到 `hash` 值(取值范围是 [0, 2^32-1])，太大，用之前还要先做对数组的长度取余(%)运算，得到的余数对应的数组下标才是存放元素的位置。

“取余(%)操作中如果除数是 2 的幂次 则等价于 与其除数减一的与(&)操作(也就是说 `hash % n == hash & (n - 1)` 的前提是 n 是 2 的幂次)。” 并且 采用二进制位操作 &，相对于%能够提高运算效率，这就解释了 `HashMap` 的长度为什么是 2 的幂次方。

### HashMap/TreeMap区别
- `HashMap` 是基于哈希表实现的，`TreeMap` 是基于红黑树实现的。
- `HashMap` 是无序的，`TreeMap` 是有序的。
- `HashMap` 的键值对允许有一个 `null` 键和多个 `null` 值，`TreeMap` 不允许有 `null` 键，但允许有 `null` 值。
- `HashMap` 的查询、插入、删除操作的时间复杂度是 O(1)，而 `TreeMap` 的时间复杂度是 O(logn)。

相比于`HashMap`，`TreeMap` 主要多了对集合中的元素根据键排序的能力以及对集合内元素的搜索的能力。

### HashSet如何检查重复
把对象加入`HashSet`时，先计算对象的`hashcode`值来判断对象加入的位置，同时会与其他加入的对象的 `hashcode` 值作比较，如果没有相符的 `hashcode`，认为对象没有重复出现。但如果发现有相同 `hashcode` 值的对象，这时会调用`equals()`方法来检查 `hashcode` 相等的对象是否真的相同。如果两者相同，`HashSet` 就不会让加入操作成功。

在 JDK1.8 中，实际上无论`HashSet`中是否已经存在某元素，都会直接插入，只是会在`add()`方法的返回值处返回插入前是否存在相同元素。

### HashMap底层实现
**JDK1.8 之前**
底层是数组和链表。哈希冲突使用“拉链法”解决。

`HashMap` 通过 key 的 `hashcode` 经过扰动函数`hash()`处理过后得到 `hash` 值，然后通过 `(n - 1) & hash` 判断当前元素存放的位置(这里的 n 指的是数组的长度)，如果当前位置存在元素的话，就判断该元素与要存入的元素的 `hash` 值以及 `key` 是否相同，如果相同的话，直接覆盖，不相同就通过“拉链法”解决冲突。

- 扰动函数 `hash()` 可以减少碰撞。
- “拉链法”：将链表和数组相结合。也就是说创建一个链表数组，数组中每一格就是一个链表。若遇到哈希冲突，则将冲突的值加到链表中即可。
- “拉链法”中，数组的 index 就是通过扰动函数`hash()`计算出来的 `hash` 值，数组中存放的是链表的头结点。而链表中的每个节点中存放的是键值对。

**JDK1.8 之后**
底层还是数组和链表，当链表长度大于阈值(默认为8)时，调用 `treeifyBin()`方法，判断是否决定要将链表转化为红黑树(将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会执行 `resize()` 方法数组扩容，而不是转换为红黑树)，以减少搜索时间。

- 数组扩容后，原本在一个链表上的元素可能会分散到多个链表上，这样就会降低链表的长度，提高查询效率。
- 扩容步骤：创建一个长度是原来两倍的新数组，由于扩容前后key经过`hash()`计算出来的`hash`值不变，但数组长度 n 变化，将原数组中的元素通过 `(n - 1) & hash`重新计算 index 放入新数组中。

### 红黑树简介
https://blog.csdn.net/cy973071263/article/details/122543826
红黑树是一种自平衡的二叉查找树，是一种高效的查找树。它可在 O(logN) 时间内完成查找、增加、删除等操作。红黑树的出现是为了应对二叉搜索树的极端情况(插入数据有序，则会退化成链表)。
特性：
- 节点是红色或黑色
- 根是黑色
- 叶子节点(外部节点，空节点)都是黑色，这里的叶子节点指的是最底层的空节点(外部节点)，null节点才是叶子节点，null节点的父节点在红黑树里不将其看作叶子节点
- 红色节点的子节点都是黑色
- 红色节点的父节点都是黑色
- 从根节点到叶子节点的所有路径上不能有 2 个连续的红色节点
- 从任一节点到叶子节点的所有路径都包含相同数目的黑色节点


### HashMap多线程扩容死循环问题
JDK1.7及之前版本的`HashMap`在多线程环境下扩容操作可能存在死循环问题。扩容时，多个线程同时对链表进行操作，头插法可能会导致链表中的节点指向错误的位置，从而形成一个环形链表，进而使得查询元素的操作陷入死循环无法结束。

为了解决这个问题，JDK1.8 版本的` HashMap` 采用了尾插法而不是头插法来避免链表倒置，使得插入的节点永远都是放在链表的末尾，避免了链表中的环形结构。

> 注意：不建议在多线程下使用 `HashMap`，会存在数据覆盖的问题，并发环境下，推荐使用`ConcurrentHashMap`。

### HashMap为什么不支持高并发
### HashMap为什么线程不安全
- JDK1.7及之前版本，存在多线程扩容死循环问题，如上。
- JDK1.7 和 JDK 1.8 都存在数据丢失问题。如下：

**数据丢失**
在 `HashMap` 中，多个键值对可能会被分配到同一个桶(bucket)，并以链表或红黑树的形式存储。多个线程对 `HashMap` 的 `put` 操作会导致线程不安全，具体来说会有数据覆盖的风险。
> 例子：
> 线程 1,2 同时进行 `put` 操作，并且发生了哈希冲突(hash 函数计算出的插入下标是相同的)。
> 不同的线程可能在不同的时间片获得 CPU 执行的机会，当前线程 1 执行完哈希冲突判断后，由于时间片耗尽挂起。线程 2 先完成了插入操作。
> 随后，线程 1 获得时间片，由于之前已经进行过 `hash` 碰撞的判断，所有此时会直接进行插入，这就导致线程 2 插入的数据被线程 1 覆盖了。

### 遍历HashMap
七种
```java
HashMap<Integer, String> map = new HashMap<>();
// ForEach EntrySet
for (Map.Entry<Integer, String> entry : map.entrySet()) {
    System.out.println(entry.getKey() + ":" + entry.getValue());
}
// ForEach KeySet
for (Integer key : map.keySet()) {
    System.out.println(key + ":" + map.get(key));
}
// 迭代器EntrySet
Iterator<Map.Entry<Integer, String>> iterator = map.entrySet().iterator();
while (iterator.hasNext()) {
    Map.Entry<Integer, String> entry = iterator.next();
    System.out.println(entry.getKey() + ":" + entry.getValue());
}
// 迭代器KeySet
Iterator<Integer> iterator = map.keySet().iterator();
while (iterator.hasNext()) {
    Integer key = iterator.next();
    System.out.println(key + ":" + map.get(key));
}
// Lambda
map.forEach((key, value) -> {
    System.out.println(key);
    System.out.println(value);
});
// Streams API 单线程
map.entrySet().stream().forEach(entry -> {
    System.out.println(entry.getKey());
    System.out.println(entry.getValue());
});
// Streams API 多线程
map.entrySet().parallelStream().forEach(entry -> {
    System.out.println(entry.getKey());
    System.out.println(entry.getValue());
});
```

### ConcurrentHashMap/Hashtable区别
- 底层：JDK1.7的`ConcurrentHashMap`采用**分段的数组+链表** 实现，JDK1.8跟 `HashMap` 1.8结构一样，**数组+链表/红黑二叉树** 。`Hashtable` 是**数组+链表** 实现。
- 实现线程安全的方式：
  - JDK1.7`ConcurrentHashMap` 对整个桶数组进行分割分段(`Segment`，分段锁)，`Segment`数组中每个元素都有一个锁，且每个元素包含一个`HashEntry`数组，其中每个`HashEntry`元素是一个链表，即一个`Segment`中守护一个`HashEntry`，多个 `Segment` 组成一个 `ConcurrentHashMap`。当一个线程占用`Segment`锁访问其中的元素时，其他线程可以访问其他 `Segment`。
  - JDK1.8`ConcurrentHashMap` 摒弃 `Segment` 的概念，而是直接用 **`Node`数组+链表+红黑树** 实现，并发控制使用 `synchronized` 和 CAS 来操作。(JDK1.6 以后 `synchronized` 锁做了很多优化) 整个看起来就像是优化过且线程安全的 `HashMap`，虽然在 JDK1.8 中还能看到 `Segment` 的数据结构，但是已经简化了属性，只是为了兼容旧版本；
  - `Hashtable`(同一把锁) ：使用 `synchronized` 保证线程安全，效率非常低下。当一个线程访问同步方法时，其他线程也访问同步方法，会进入阻塞或轮询状态，如使用 `put` 添加元素，另一个线程不能使用 `put` 添加元素，也不能使用 `get`，竞争会越来越激烈效率越低。

### ConcurrentHashMap底层

JDK1.7`ConcurrentHashMap` 数据结构为`Segment`数组+`HashEntry`数组+链表。`ConcurrentHashMap`对整个桶数组进行分割分段(`Segment`，分段锁)，`Segment`数组每个元素存放一个`HashEntry`数组，其中每个`HashEntry`元素是一个链表，即一个`Segment` 守护一个 `HashEntry` 数组里的元素(`HashEntry`结构类似于`HashMap`)，当对 `HashEntry` 数组的数据进行修改时，必须首先获得对应的 `Segment` 的锁。也就是说，对同一 `Segment` 的并发写入会被阻塞，不同 `Segment` 的写入是可以并发执行的。

`Segment` 继承了 `ReentrantLock` ，是可重入锁，`Segment` 的个数一旦初始化就不能改变，默认是 16，默认同时支持 16 个线程并发写。

JDK1.8`ConcurrentHashMap`取消了 `Segment` 分段锁，采用 `Node + CAS + synchronized` 来保证并发安全。数据结构跟 `HashMap`1.8 的结构类似，`Node`数组+链表/红黑二叉树。Java 8 在链表长度超过一定阈值(8)时将链表(寻址时间复杂度为 O(N))转换为红黑树(寻址时间复杂度为 O(log(N)))。Java 8 中，锁粒度更细，`synchronized` 只锁定当前链表或红黑二叉树的首节点，这样只要 `hash` 不冲突，就不会产生并发，就不会影响其他 `Node` 的读写，效率大幅提升。


### JDK 1.7和1.8的ConcurrentHashMap实现有什么不同？
- 线程安全实现方式：JDK 1.7采用 `Segment` 分段锁来保证安全，`Segment`继承自 `ReentrantLock`。JDK1.8 放弃了 `Segment` 分段锁的设计，采用 `Node + CAS + synchronized` 保证线程安全，锁粒度更细，`synchronized` 只锁定当前链表或红黑二叉树的首节点。
- Hash 碰撞解决方法 ： JDK 1.7采用拉链法，JDK1.8采用拉链法结合红黑树(链表长度超过一定阈值时，将链表转换为红黑树)。
- 并发度：JDK 1.7最大并发度是 `Segment` 的个数，默认是 16。JDK 1.8 最大并发度是 `Node` 数组的大小，并发度更大。

### ConcurrentHashMap能保证复合操作的原子性吗？
- `ConcurrentHashMap`是线程安全的，可以保证多个线程同时对它进行读写操作，不会出现数据不一致的情况，也不会导致 JDK1.7 及之前版本的 `HashMap` 多线程操作导致死循环问题。
- `ConcurrentHashMap` 提供了一些原子性的复合操作，如 `putIfAbsent`、`compute`、`computeIfAbsent` 、`computeIfPresent`、`merge`等。这些方法都可以接受一个函数作为参数，根据给定的 `key` 和 `value` 来计算一个新的 `value`，并且将其更新到 `map` 中。

### 什么是LinkedHashMap
继承了 `HashMap` 的所有属性和方法，在 `HashMap` 基础上在各个节点之间维护一条双向链表，使得原本散列在不同 `bucket` 上的节点、链表、红黑树有序关联起来。具备如下特性：
- 支持遍历时会按照插入顺序有序进行迭代
- 支持按照元素访问顺序排序,_**适用于封装 LRU 缓存工具**_ 。
- 因为内部使用双向链表维护各个节点，所以遍历时的效率和元素个数成正比，相较于和容量成正比的 `HashMap` 来说，迭代效率会高很多。

> 在 `HashMap` 的基础重写了 `afterNodeRemoval`、`afterNodeInsertion`、`afterNodeAccess` 方法。使之拥有顺序插入和访问有序的特性。

### LinkedHashMap如何按照访问顺序迭代元素？
`LinkedHashMap`通过构造函数中的 `accessOrder` 参数指定按照访问顺序迭代元素。当 `accessOrder` 为 `true` 时，每访问一个元素，该元素会被移动到链表的末尾，因此下次访问该元素时，它就会成为链表中的最后一个元素，从而实现按照访问顺序迭代元素。

### 如何实现LRU缓存
- `accessOrder` = `true` 
- 继承`LinkedHashMap`
- 重写 `removeEldestEntry` 方法。当链表大小超过容量时返回 `true`，使得每次访问一个元素时，该元素会被移动到链表的末尾。一旦插入操作让 `removeEldestEntry` 返回 `true` 时，视为缓存已满，`LinkedHashMap` 就会将链表首元素移除，由此实现 LRU 缓存。
```java
public class LRUCache<K, V> extends LinkedHashMap<K, V> {
    private final int capacity;
    public LRUCache(int capacity) {
        super(capacity, 1f, true);
        this.capacity = capacity;
    }
    // 判断size超过容量时返回true，告知LinkedHashMap移除最老的缓存项(即链表的第一个元素)
    @Override
    protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
        return size() > capacity;
    }
}
```
### LinkedHashMap和HashMap的区别
- 最大区别在于迭代元素的顺序。`HashMap` 迭代元素的顺序是不确定的，而 `LinkedHashMap` 提供了按照插入顺序或访问顺序迭代元素的功能。
- `LinkedHashMap` 内部维护了一个双向链表，用于记录元素的插入顺序或访问顺序，而 `HashMap` 则没有这个链表。因此，`LinkedHashMap` 插入性能比 `HashMap` 略低，但提供了更多功能且迭代起来比`HashMap`更高效。

### Queue/Deque区别
- `Queue` 是单端队列，只能从一端插入元素，另一端删除元素，实现上遵循 先进先出(FIFO)规则。
- `Deque` 是双端队列，在队列的两端均可以插入或删除元素，其还提供有 `push()`等方法，可用于模拟栈。

### ArrayDeque/LinkedList区别
- `ArrayDeque` 基于**可变长数组** 和**双指针** 来实现，而 `LinkedList` 通过链表来实现。
- `ArrayDeque` 不能存储 `NULL` 数据，`LinkedList` 能。
- `ArrayDeque` 插入时可能存在扩容过程, 不过均摊后的插入操作依然为 O(1)。虽然 `LinkedList` 不需要扩容，但每次插入数据时都要申请新的堆空间，均摊性能相比更慢。

从性能的角度上，选用 `ArrayDeque` 来实现队列要比 `LinkedList` 更好。此外，`ArrayDeque` 也可以用于实现栈。

### PriorityQueue
PriorityQueue 中元素出队顺序是与优先级相关的，即总是优先级最高的元素先出队。
- 利用了二叉堆的数据结构来实现的，底层使用可变长的数组来存储数据
- 通过堆元素的上浮和下沉，实现了在 O(logn) 的时间复杂度内插入元素和删除堆顶元素。
- 是非线程安全的，且不支持存储 `NULL` 和 `non-comparable` 的对象。
- 默认是小顶堆，但可以接收一个 `Comparator` 作为构造参数，从而来自定义元素优先级的先后。

> `PriorityQueue` 在面试中可能更多的会出现在手撕算法的时候，典型例题包括堆排序、求第 K 大的数、带权图的遍历等，所以需要会熟练使用才行。

### BlockingQueue及其实现类
`BlockingQueue`(阻塞队列)是一个接口，其支持当队列没有元素时一直阻塞，直到有元素；还支持如果队列已满，一直等到队列可以放入新元素时再放入。常用于**生产者-消费者模型** 中，生产者线程向队列中添加数据，消费者线程从队列中取出数据进行处理。

**实现类**
- `ArrayBlockingQueue`：使用数组实现的有界阻塞队列。在创建时需要指定容量大小，并支持公平和非公平两种方式的锁访问机制。
- `LinkedBlockingQueue`：使用单向链表实现的可选有界阻塞队列。在创建时可以指定容量大小，如果不指定则默认为Integer.MAX_VALUE。和ArrayBlockingQueue不同的是， 它仅支持非公平的锁访问机制。
- `PriorityBlockingQueue`：支持优先级排序的无界阻塞队列。元素必须实现`Comparable`接口且不能插入 `null` 元素。
- `SynchronousQueue`：同步队列，是一种不存储元素的阻塞队列。每个插入操作都必须等待对应的删除操作，反之删除操作也必须等待插入操作。通常用于线程之间的直接传递数据。
- `DelayQueue`：延迟队列，其中的元素只有到了其指定的延迟时间，才能够从队列中出队。

### ArrayBlockingQueue
- `put()`、`take()`：插入和删除元素时，如果队列已满或者为空，会阻塞等待。
- `offer()`、`poll()`：插入和删除元素时，如果队列已满或者为空，不会阻塞，而是直接返回 `false` 或者 `null`。
- 并发控制采用可重入锁 `ReentrantLock` 保证线程安全，插入/读取操作都需获取到锁才能进行，且支持公平/非公平两种方式的锁访问机制，默认是非公平锁。

### ArrayBlockingQueue实现原理
- `ArrayBlockingQueue` 内部维护一个定长的数组用于存储元素。
- 通过使用 `ReentrantLock` 锁对象对读写操作进行同步，即通过锁机制来实现线程安全。
- 通过 `Condition` 接口实现线程间的等待和唤醒操作。

线程间的等待和唤醒具体的实现：
- 当队列已满时，生产者线程会调用 `notFull.await()` 方法让生产者进行等待，等待队列非满时插入(非满条件)。
- 当队列为空时，消费者线程会调用 `notEmpty.await()`方法让消费者进行等待，等待队列非空时消费(非空条件)。
- 当有新的元素被添加时，生产者线程会调用 `notEmpty.signal()`方法唤醒正在等待消费的消费者线程。
- 当队列中有元素被取出时，消费者线程会调用 `notFull.signal()`方法唤醒正在等待插入元素的生产者线程。

### 生产者消费者模型
阻塞队列就是典型的生产者-消费者模型：
- 当阻塞队列数据为空时，所有的消费者线程都会被阻塞，等待队列非空。
- 当生产者往队列里填充数据后，队列就会通知消费者队列非空，消费者此时就可以进来消费。
- 当阻塞队列因为消费者消费过慢或者生产者存放元素过快导致队列填满时无法容纳新元素时，生产者就会被阻塞，等待队列非满时继续存放元素。
- 当消费者从队列中消费一个元素之后，队列就会通知生产者队列非满，生产者可以继续填充数据了。


### ArrayBlockingQueue/LinkedBlockingQueue区别
二者是Java并发中常用的两种阻塞队列实现，都是线程安全的。区别：
- 底层实现：`ArrayBlockingQueue` 基于数组实现，而 `LinkedBlockingQueue` 基于链表实现。
- 是否有界：`ArrayBlockingQueue` 是有界队列，必须在创建时指定容量大小。`LinkedBlockingQueue` 创建时可以不指定容量大小，默认是`Integer.MAX_VALUE`，也就是无界的。但也可以指定队列大小，从而成为有界的。
- 锁是否分离：`ArrayBlockingQueue`中的锁是没有分离的，即生产和消费用的是同一个锁；`LinkedBlockingQueue`中的锁是分离的，即生产用的是`putLock`，消费是`takeLock`，这样可以防止生产者和消费者线程之间的锁争夺。
- 内存占用：`ArrayBlockingQueue` 需要提前分配数组内存，而 `LinkedBlockingQueue` 则是动态分配链表节点内存。因此，`ArrayBlockingQueue` 在创建时就会占用一定的内存空间，且往往申请的内存比实际所用的内存更大，而`LinkedBlockingQueue` 则是根据元素的增加而逐渐占用内存空间。

### DelayQueue底层
是线程安全的延迟队列，其中的元素只有到了其指定的延迟时间，才能够从队列中出队。

**底层**
`DelayQueue` 底层用 `PriorityQueue` 存储元素，`PriorityQueue` 采用**二叉小顶堆**的思想确保值小的元素排在最前面，使得 `DelayQueue` 对于延迟任务优先级的管理非常方便。同时 `DelayQueue` 为了保证线程安全还用到了可重入锁 `ReentrantLock`,确保单位时间内只有一个线程可以操作延迟队列。最后，为了实现多线程之间等待和唤醒的交互效率还用到了 `Condition`接口，通过 `Condition` 的 `await` 和 `signal` 方法完成多线程之间的等待唤醒。

> ```java
> //可重入锁，实现线程安全的关键
> private final transient ReentrantLock lock = new ReentrantLock();
> //延迟队列底层存储数据的集合,确保元素按照到期时间升序排列
> private final PriorityQueue<E> q = new PriorityQueue<E>();
> //指向准备执行优先级最高的线程
> private Thread leader = null;
> //实现多线程之间等待唤醒的交互
> private final Condition available = lock.newCondition();
> ```
> `leader`： 延迟队列的任务只有到期之后才会执行,对于没有到期的任务只有等待,为了确保优先级最高的任务到期后可以即刻被执行,用 `leader` 来管理延迟任务，只有 `leader` 所指向的线程才具备定时等待任务到期执行的权限，而其他那些优先级低的任务只能无限期等待，直到 `leader` 线程执行完手头的延迟任务后唤醒它。
> `available`： 等待唤醒操作的交互是通过 `available` 实现的，假如一个线程尝试在空的 `DelayQueue` 获取任务时，`available` 就将其放入等待队列中。直到有一个线程添加一个延迟任务后通过 `available` 的 `signal` 方法将其唤醒。

### DelayQueue是否线程安全？
`DelayQueue`是线程安全的，它通过 `ReentrantLock` 实现了互斥访问和 `Condition` 实现了线程间的等待和唤醒操作，可以保证多线程环境下的安全性和可靠性。

### DelayQueue使用场景
通常用于实现定时任务调度和缓存过期删除等场景。
- 在定时任务调度中，需要将需要执行的任务封装成延迟任务对象，并将其添加到 `DelayQueue` 中，`DelayQueue` 会自动按照剩余延迟时间进行升序排序(默认情况)，以保证任务能够按照时间先后顺序执行。
- 缓存过期删除中，在数据被缓存到内存之后，可以将缓存的 `key` 封装成一个延迟的删除任务，并将其添加到 `DelayQueue` 中，当数据过期时，拿到这个任务的 `key`，将这个 `key` 从内存中移除。

### DelayQueue中Delayed接口的作用是什么？
`DelayQueue` 中存放的元素必须实现 `Delayed` 接口，并且需要重写 `getDelay()`方法。`Delayed`接口定义了元素的剩余延迟时间(`getDelay()`)和元素之间的比较规则(继承 `Comparable` 接口),否则 `DelayQueue` 无法得知当前任务剩余时长和任务优先级的比较。

### DelayQueue/Timer/TimerTask区别
都可以用于实现定时任务调度，但实现方式不同。
- `DelayQueue` 基于优先级队列和堆排序算法，可以实现多个任务按照时间先后顺序执行；
- `Timer/TimerTask` 基于单线程，只能按照任务的执行顺序依次执行，如果某个任务执行时间过长，会影响其他任务的执行。
- `DelayQueue` 支持动态添加/移除任务，`Timer/TimerTask` 只能在创建时指定任务。


## Java并发

### 进程/线程/协程
https://blog.csdn.net/m0_60505735/article/details/131047046
https://blog.csdn.net/weixin_49199646/article/details/109210547
- 进程： 进程是程序的一次执行过程，是系统资源分配和运行程序的基本单位；一个进程在其执行的过程中可以产生多个线程。
- 线程： 线程是进程的一个执行单元，是任务调度和系统执行的最小单位；与进程不同的是同类的多个线程共享进程的堆和方法区资源，但每个线程有自己的程序计数器、虚拟机栈和本地方法栈。所以系统在产生一个线程，或是在各个线程之间做切换工作时，负担要比进程小得多，也正因为如此，线程也被称为轻量级进程。
- 协程： 协程是一种用户态的轻量级线程，协程的调度完全由用户控制。

**进程与线程的区别**
- 根本区别： 进程是操作系统资源分配和独立运行的最小单位；线程是任务调度和系统执行的最小单位。
- 地址空间区别： 每个进程都有独立的地址空间，一个进程崩溃不影响其它进程；一个进程中的多个线程共享该 进程的地址空间，一个线程的非法操作会使整个进程崩溃。
- 上下文切换开销区别： 每个进程有独立的代码和数据空间，进程之间上下文切换开销较大；线程组共享代码和数据空间，线程之间切换的开销较小。

**进程与线程的联系**
一个进程由共享空间(包括堆、代码区、数据区、进程空间和打开的文件描述符)和一个或多个线程组成，各个线程之间共享进程的内存空间。而一个标准的线程由线程ID、程序计数器PC、寄存器和栈组成。

*进程与线程的选择**
- 线程的创建或销毁的代价比进程小，需要频繁创建和销毁时应优先选用线程；
- 线程上下文切换的速度比进程快，需要大量计算时优先选用线程；
- 线程在CPU上的使用效率更高，需要多核分布时优先选用线程，需要多机分布时优先选用进程
- 线程的安全性、稳定性没有进程好，需要更稳定安全时优先使用进程。

综上，线程创建和销毁的代价低、上下文切换速度快、对系统资源占用小、对CPU的使用效率高，因此一般情况下优先选择线程进行高并发编程；但线程组的所有线程共用一个进程的内存空间，安全稳定性相对较差，若其中一个线程发生崩溃，可能会使整个进程，因此对安全稳定性要求较高时，需要优先选择进程进行高并发编程。

### 进程间的通信方式
进程间通信(Inter-Process Communication, IPC)是指在操作系统中不同进程之间传递数据或信息的机制。常见的进程间通信方式有多种，每种方式都有其独特的应用场景和优缺点。以下是一些主要的进程间通信方式：
1. **管道(Pipes)**：
    - **匿名管道(Anonymous Pipes)**：主要用于有亲缘关系的进程之间的通信，如父子进程。数据以字节流的形式在进程间传递。
    - **命名管道(Named Pipes)**：支持在无亲缘关系的进程之间进行通信。命名管道存在于文件系统中，可以被不同的进程打开和使用。
2. **消息队列(Message Queues)**：
    - 允许进程通过发送和接收消息进行通信。消息队列提供了一种在进程间传递数据的有序方式，可以实现异步通信。
3. **共享内存(Shared Memory)**：
    - 进程共享一段内存空间，进程可以直接读写这段共享内存中的数据。共享内存是最快的一种通信方式，因为数据不需要在进程间复制，但需要额外的同步机制来避免并发访问问题。
4. **信号量(Semaphores)**：
    - 用于控制多个进程对共享资源的访问，通过信号量可以实现进程间的同步和互斥。
5. **信号(Signals)**：
    - 信号是一种有限的异步通知机制，用于通知进程某个事件的发生。进程可以捕捉和处理信号，从而实现简单的通信和控制。
6. **套接字(Sockets)**：
    - 套接字不仅支持同一台计算机上进程间的通信，也支持分布式网络中不同计算机上的进程间通信。常用于网络编程中。
7. **文件系统(File System)**：
    - 进程可以通过读写共享的文件进行通信。这种方式简单但效率较低，适用于需要持久化存储的场景。
8. **内存映射文件(Memory-Mapped Files)**：
    - 通过将文件映射到进程的地址空间，实现文件内容的共享和通信。与共享内存类似，但数据的持久化由文件系统提供。

### 线程间的通信方式
线程间的通信是指在同一个进程内，不同线程之间交换数据或信号的机制。常见的线程间通信方式包括以下几种：
1. **共享内存**：
    - **全局变量**：所有线程都可以访问和修改同一个全局变量。
    - **静态变量**：静态变量在进程的生命周期内只初始化一次，所有线程共享。
2. **互斥锁(Mutex)**：
    - 用于防止多个线程同时访问共享资源，从而避免数据竞争。
3. **读写锁(RWLock)**：
    - 允许多个线程同时读数据，但在写数据时需要独占锁，确保写操作的安全性。
4. **信号量(Semaphore)**：
    - 主要用于限制对共享资源的访问数量，可以控制同时访问资源的线程数。
5. **条件变量(Condition Variable)**：
    - 用于线程之间的等待通知机制，一个线程可以等待一个条件变量，而另一个线程可以通知该条件变量改变状态，从而唤醒等待的线程。
6. **事件(Event)**：
    - 线程可以等待一个事件，直到另一个线程设置该事件，从而实现线程之间的同步。
7. **队列(Queue)**：
    - 线程安全的队列，常用于生产者-消费者模型，一个线程放入数据，另一个线程取出数据。
8. **管道(Pipe)**：
    - 用于线程之间的数据传输，常见于一些操作系统提供的进程间通信机制中。
9. **消息队列(Message Queue)**：
    - 一种线程安全的队列，专门用于在多个线程之间传递消息。
10. **信号(Signal)**：
    - 一种用于通知线程某个事件发生的机制，通常用在异步事件处理。

### Semaphore
`Semaphore` 是一个计数信号量，用于控制同时访问特定资源的线程数量，通过协调各个线程，以保证合理的使用资源。其中的`state`表示许可数(>=1)，当一个线程调用 `acquire()` 方法时，会首先尝试获取一个许可，如果成功，该线程就可以继续执行，否则就会被阻塞。当一个线程调用 `release()` 方法时，会释放一个许可，这样就会唤醒一个被阻塞的线程。

### Semaphores信号量原理
信号量(Semaphore)是一种用于进程间同步和互斥的机制。它通过维护一个计数器来管理多个进程对共享资源的访问。

Semaphore 是共享锁的一种实现，它默认构造 AQS 的 state 值为 permits，表示许可的数量。只有拿到许可证的线程才能执行。
- 调用`semaphore.acquire()`，线程尝试获取许可证，如果 `state >= 0` 的话，则表示可以获取成功。如果获取成功的话，使用 CAS 操作去修改 `state` 的值 `state=state-1`。如果 `state<0` 的话，则表示许可证数量不足。此时会创建一个 Node 节点加入阻塞队列，挂起当前线程。
- 调用`semaphore.release()`，线程尝试释放许可证，并使用 CAS 操作去修改 `state` 的值 `state=state+1`。释放许可证成功之后，同时会唤醒阻塞队列中的一个线程。被唤醒的线程会重新尝试去修改 `state` 的值 `state=state-1`，如果 `state>=0` 则获取令牌成功，否则重新进入阻塞队列，挂起线程。

### Java线程和操作系统线程有何区别
现在的 Java 线程的本质其实就是操作系统的线程。

- JDK1.2之前Java线程基于绿色线程(`Green Threads`)实现，即用户线程，由JVM自己实现线程调度，不依赖于操作系统的线程调度。
- JDK 1.2 及以后，Java 线程改为基于原生线程(Native Threads)实现，也就是说 JVM 直接使用操作系统原生的内核级线程(内核线程)来实现 Java 线程，由操作系统内核进行线程的调度和管理。

线程模型是用户线程和内核线程之间的关联方式，常见的线程模型有三种： 一对一(一个用户线程对应一个内核线程)、 多对一、 多对多。

在 Windows 和 Linux 等主流操作系统中，Java 线程采用的是一对一的线程模型，也就是一个 Java 线程对应一个系统内核线程。

### 程序计数器为啥是私有的
程序计数器主要有下面两个作用：
- 字节码解释器通过改变程序计数器来依次读取指令，从而实现代码的流程控制，如：顺序执行、选择、循环、异常处理。在多线程的情况下，程序计数器用于记录当前线程执行的位置，从而当线程被切换回来的时候能够知道该线程上次运行到哪儿了。
- 如果执行的是 `native` 方法，那么程序计数器记录的是 `undefined` 地址，只有执行的是 Java 代码时程序计数器记录的才是下一条指令的地址。

所以，程序计数器私有主要是为了线程切换后能恢复到正确的执行位置。

### 虚拟机栈和本地方法栈为啥是私有的
- 虚拟机栈： 每个 Java 方法在执行之前会创建一个栈帧用于存储局部变量表、操作数栈、常量池引用等信息。从方法调用直至执行完成的过程，就对应着一个栈帧在 Java 虚拟机栈中入栈和出栈的过程。
- 本地方法栈： 和虚拟机栈所发挥的作用非常相似，区别是：虚拟机栈为虚拟机执行 Java 方法 (也就是字节码)服务，而本地方法栈则为虚拟机使用到的 Native 方法服务。 在 HotSpot 虚拟机中和 Java 虚拟机栈合二为一。

所以，为了保证线程中的局部变量不被别的线程访问到，虚拟机栈和本地方法栈是线程私有的。

### 一句话简单了解堆和方法区
堆和方法区是所有线程共享的资源，其中堆是进程中最大的一块内存，主要用于存放新创建的对象 (几乎所有对象都在这里分配内存)，方法区主要用于存放已被加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。

### 如何创建线程
- 继承`Thread`类并重写`run()`方法
- 实现`Runnable`接口并重写`run()`方法
- 实现`Callable`接口并重写`call()`方法
- 使用线程池`ThreadPoolExecutor`创建线程
- 等等......

其实只有一种方式可以创建线程，那就是通过 `new Thread().start()` 创建。不管是哪种方式，最终还是依赖于`new Thread().start()`。

### 线程的生命周期和状态
Java 线程 6 种状态：
`NEW`： 初始状态，线程被创建出来但没有被调用 `start()` 。
`RUNNABLE`： 运行状态，线程被调用了 `start()`等待运行的状态。
`BLOCKED`：阻塞状态，需要等待锁释放。
`WAITING`：线程执行`wait()`方法等待状态，表示该线程需要等待其他线程做出一些特定动作(通知或中断)。
`TIME_WAITING`：超时等待状态，调用`sleep(long millis)`可以在指定的时间后自行返回而不是像 `WAITING` 那样一直等待。
`TERMINATED`：执行完`run()`方法进入终止状态，表示该线程已经运行完毕。线程在生命周期中并不是固定处于某一个状态而是随着代码的执行在不同状态之间切换。

### 线程上下文切换开销
线程在执行过程中会有自己的运行条件和状态(也称**上下文**)，比如上文所说到过的程序计数器，栈信息等。 当出现如下情况的时候，线程会从占用 CPU 状态中退出。
- 主动让出 CPU，比如调用了 `sleep()`, `wait()` 等。
- 时间片用完(因为操作系统要防止一个线程或者进程长时间占用 CPU 导致其他线程或者进程饿死)。
- 调用了阻塞类型的系统中断，比如请求 IO，线程被阻塞。

线程切换意味着需要保存当前线程的上下文，留待线程下次占用 CPU 的时候恢复现场。并加载下一个将要占用 CPU 的线程上下文。这就是所谓的 上下文切换。

上下文切换是现代操作系统的基本功能，因其每次需要保存信息恢复信息，这将会占用 CPU，内存等系统资源进行处理，也就意味着效率会有一定损耗，如果频繁切换就会造成整体效率低下。

### Thread.sleep()方法和Object.wait()方法对比
共同点：两者都可以暂停线程的执行。
区别：
- `sleep()` 方法没有释放锁，`wait()` 方法释放锁 。
- `sleep()`通常被用于暂停执行，`wait()` 通常被用于线程间交互/通信。
- `wait()`方法被调用后，线程不会自动苏醒，需要别的线程调用同一个对象上的 `notify()`或者 `notifyAll()`方法。`sleep()`方法执行完成后，线程会自动苏醒(或者也可以使用`wait(long timeout)`超时后线程会自动苏醒)。
- `sleep()` 是 `Thread` 类的静态本地方法，`wait()` 则是 `Object` 类的本地方法。

### 锁的基本概念
Java 中，每个对象都有一个与之关联的监视器(`monitor`)，也叫做“对象锁”或“监视器锁”。当线程想要执行对象的同步方法或同步代码块时，必须首先获得该对象的监视器锁。如果其他线程已经持有该锁，则新的线程必须等待，直到该锁被释放。

### Java中有哪些锁
- `synchronized`关键字
  - 用于同步方法或同步代码块。
  - 基于对象锁，每个对象都有一个监视器锁。
- `ReentrantLock`
  - 在`java.util.concurrent.locks`包中提供。
  - 提供更灵活的锁机制，相比`synchronized`，支持公平锁、非公平锁等特性。
  - 支持可重入锁，即同一个线程可以多次获取同一个锁。
- `ReentrantReadWriteLock`
  - 在`java.util.concurrent.locks`包中提供。
  - 提供读写锁的实现，读锁共享，写锁独占。
  - 适用于读多写少的场景，提升并发性能。 
- `StampedLock`
  - 在`java.util.concurrent.locks`包中提供。
  - 提供三种锁模式：写锁、悲观读锁和乐观读锁。
  - 乐观读锁在某些情况下可以避免加锁操作，提升性能。
- `LockSupport`
  - 提供基本的线程阻塞和唤醒机制。
  - 通常用于构建更高级的同步器，如`Semaphore`、`CountDownLatch`等。
- `Semaphore`
  - 在`java.util.concurrent`包中提供。
  - 用于控制同时访问特定资源的线程数量。
- `CountDownLatch`
  - 在`java.util.concurrent`包中提供。
  - 允许一个或多个线程等待一组操作完成。 
- `CyclicBarrier`
  - 在`java.util.concurrent`包中提供。
  - 使一组线程在到达一个共同的屏障点时被阻塞，直到所有线程都到达该点。
- Phaser
  - 在`java.util.concurrent`包中提供。
  - 允许多阶段的线程协作，每个阶段完成后继续下一阶段。 
- `Atomic`变量
  - 在`java.util.concurrent.atomic`包中提供。
  - 提供了一些基本类型(如`AtomicInteger`、`AtomicLong`)的原子操作，实现了无锁的线程安全。

这些锁机制可以根据具体的应用场景选择，以实现高效、安全的并发控制。

### 为什么wait()方法不定义在Thread中
- 锁是对象级别的，每个对象都有一把锁。
- `wait()` 方法是对对象的监视器(锁)进行操作，释放的是对象的锁，而不是线程的锁。

### 为什么sleep()方法定义在Thread中
- `sleep()` 是线程控制方法，作用对象是线程本身。
- 线程暂停执行，不涉及到对象类，也不需要获得对象锁。

### sleep()如何控制当前线程沉睡
调用 `Thread.sleep()` 会作用于调用它的线程(当前线程)。它并不需要知道是哪一个线程调用的，因为它暂停的是当前线程。

### 可以直接调用Thread类的run方法吗
可以但不建议。`new` 一个 `Thread`，线程进入了新建状态。调用 `start()`方法，会启动一个线程并使线程进入就绪状态，当分配到时间片后就可以开始运行。 `start()` 会执行线程的相应准备工作，然后自动执行 `run()` 方法的内容，这是真正的多线程工作。 但是，直接执行 `run()` 方法，会把 `run()` 方法当成一个 `main` 线程下的普通方法去执行，并不会在某个线程中执行它，所以这并不是多线程工作。

总结：调用 `start()` 方法方可启动线程并使线程进入就绪状态，直接执行 `run()` 方法的话不会以多线程的方式执行。

### 并发/并行区别
- 并发：两个及两个以上的作业在同一 **时间段** 内执行。
- 并行：两个及两个以上的作业在同一 **时刻** 执行。

### 同步/异步区别
- 同步：发出一个调用之后，在没有得到结果之前， 该调用就不可以返回，一直等待。
- 异步：调用在发出之后，不用等待返回结果，该调用直接返回。

### 为什么要使用多线程
- 线程切换和调度的成本远远小于进程。
- 单核时代：在单核时代多线程主要是提高单进程利用 CPU 和 IO 系统的效率。  当使用多线程的时候，一个线程被 IO 阻塞，其他线程还可以继续使用 CPU，从而提高 Java 进程利用系统资源的整体效率。
- 多核时代：多核时代多线程主要是提高进程利用多核 CPU 的能力。  创建多个线程可以被映射到底层多个 CPU 核心上执行，在任务中的多个线程没有资源竞争的情况下，任务执行的效率会有显著性的提高。
- 并发编程是当代互联网发展趋势(现在的系统动不动就要求百万级甚至千万级的并发量)。

### 使用多线程风险
- 线程安全问题：多个线程访问共享资源时，可能会出现数据不一致的情况。
- 死锁：多个线程相互等待对方释放资源，导致所有线程都无法继续执行。
- 内存泄漏：线程未正确释放资源，导致内存泄漏。
- 上下文切换：线程频繁切换，会增加系统开销。
- ...

### 如何理解线程安全和不安全
线程安全和不安全是在多线程环境下对于同一份数据的访问是否能够保证其**正确性** 和**一致性** 的描述。
- 线程安全指的是在多线程环境下，对于同一份数据，不管有多少个线程同时访问，都能保证这份数据的正确性和一致性。
- 线程不安全则表示在多线程环境下，对于同一份数据，多个线程同时访问时可能会导致数据混乱、错误或者丢失。

### Java如何保证线程安全
1. 同步代码块和方法：通过`synchronized`关键字来实现同步代码块和方法，只有一个线程可以在任意时刻进入同步代码块或方法，这样可以避免多个线程同时访问共享资源而导致数据不一致的问题。
2. 显式锁：`java.util.concurrent.locks` 包中的显式锁(如 1ReentrantLock1)，这些锁提供了比 `synchronized` 更加灵活的锁定机制。
3. 原子变量类：`java.util.concurrent.atomic` 包中的原子变量类(如 `AtomicInteger`、`AtomicLong`)，这些类提供了一些原子操作，支持无锁的线程安全操作。可以避免多线程访问共享资源时的数据不一致问题。
4. 并发集合： 如`ConcurrentHashMap`, `CopyOnWriteArrayList` 等，这些集合类内部实现了线程安全的操作。
5. `volatile` 关键字用于声明一个变量在多个线程中是可见的，即当一个线程修改了该变量的值，其他线程立即可以看到修改后的值。
6. 并发工具类：`java.util.concurrent` 包提供了许多用于线程安全的工具类，如 `CountDownLatch`, `CyclicBarrier`, `Semaphore`, `Exchanger` 等。这些工具类可以帮助管理和协调多个线程之间的交互。

### 多线程实现方式
1. 继承 `Thread` 类
通过继承 `Thread` 类，并重写 `run` 方法来实现多线程。
```java
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread is running...");
    }

    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.start();
    }
}
```

2. 实现 `Runnable` 接口
通过实现 `Runnable` 接口，并将其实例传递给 `Thread` 对象来实现多线程。
```java
class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Runnable is running...");
    }

    public static void main(String[] args) {
        Thread t1 = new Thread(new MyRunnable());
        t1.start();
    }
}
```

3. 使用 `Callable` 和 `Future`
`Callable` 接口和 `Future` 接口可以在任务完成时返回结果或抛出异常。
```java
class MyCallable implements Callable<Integer> {
    @Override
    public Integer call() throws Exception {
        return 123;
    }

    public static void main(String[] args) {
        ExecutorService executor = Executors.newSingleThreadExecutor();
        Future<Integer> future = executor.submit(new MyCallable());
        try {
            Integer result = future.get();
            System.out.println("Result: " + result);
        } catch (InterruptedException | ExecutionException e) {
            e.printStackTrace();
        }
        executor.shutdown();
    }
}
```

4. 使用 `ExecutorService`
通过 `ExecutorService` 管理线程池，执行多线程任务。
```java
public class Main {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(5);
        for (int i = 0; i < 5; i++) {
            executor.execute(new Runnable() {
                @Override
                public void run() {
                    System.out.println("Thread is running...");
                }
            });
        }
        executor.shutdown();
    }
}
```
5. 通过 `ThreadPoolExecutor` 创建线程池
```java
class Task implements Runnable {
    private final String name;
    public Task(String name) {
        this.name = name;
    }
    @Override
    public void run() {
        System.out.println("Executing: " + name + " by " + Thread.currentThread().getName());
        try {
            // 模拟任务执行时间
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        System.out.println("Completed: " + name + " by " + Thread.currentThread().getName());
    }
}
public class ThreadPoolExecutorExample {
    public static void main(String[] args) {
        // 创建ThreadPoolExecutor
        ThreadPoolExecutor executor = (ThreadPoolExecutor) Executors.newFixedThreadPool(5);
        // 提交任务给线程池
        for (int i = 1; i <= 10; i++) {
            Task task = new Task("Task-" + i);
            System.out.println("Submitting: " + task.name);
            executor.execute(task);
        }
        // 关闭线程池
        executor.shutdown();
        try {
            // 等待所有任务完成
            if (!executor.awaitTermination(60, TimeUnit.SECONDS)) {
                executor.shutdownNow();
            }
        } catch (InterruptedException e) {
            executor.shutdownNow();
        }
    }
}
```



### 高并发存在什么问题,Java如何解决的
**高并发问题**
- 资源竞争：多个线程竞争同一资源(如内存、文件、数据库连接等)，可能导致数据不一致、死锁等问题。
- 死锁：多个线程在等待对方释放资源，从而相互阻塞，导致程序无法继续执行。
- 上下文切换开销：线程之间的切换会消耗系统资源，频繁的上下文切换会降低系统性能。
- 线程安全：确保多个线程对共享资源的操作不会导致数据不一致的问题。
- 内存泄漏：大量创建和销毁线程，如果不合理管理，会导致内存泄漏。
- 饥饿和公平性：某些线程可能长期得不到资源，导致饥饿问题。如何确保线程公平地获取资源也是一个挑战。
- 负载均衡：在分布式系统中，如何将请求均衡地分配到不同的服务器，以避免某些服务器过载。

**Java解决高并发问题**
- 线程池(Thread Pool)：通过线程池管理线程的创建和销毁，减少资源消耗，提高系统性能。
- 同步机制(Synchronization)：使用 `synchronized`关键字或显示锁(如`ReentrantLock`)来确保线程安全，避免资源竞争。
- 并发工具类：如`ConcurrentHashMap`、`CopyOnWriteArrayList`等，这些工具类可以帮助开发者更方便地实现线程间的协调和资源共享。
- 无锁算法(Lock-Free Algorithms)：使用原子变量类(如`AtomicInteger`、`AtomicLong`)来实现无锁并发，减少上下文切换开销，提高性能。
- CAS操作(Compare-And-Swap)：CAS是一种无锁算法，通过比较和交换操作来实现原子性。Java中的`Unsafe`类和`Atomic`包使用了CAS操作来实现无锁算法。
- Fork/Join框架：用于任务拆分和并行处理，适用于大规模数据并行处理。Java 7引入了`ForkJoinPool`来支持这一框架。
- 异步编程(Asynchronous Programming)：通过`CompletableFuture`和异步方法调用，实现非阻塞的并发编程模型。

### Java如何实现高并发
**使用线程池**
线程池可以有效管理线程的创建和销毁，减少资源消耗和上下文切换开销。Java中的`ExecutorService`和`Executors`类提供了便捷的线程池实现。
```java
ExecutorService executor = Executors.newFixedThreadPool(10);
for (int i = 0; i < 100; i++) {
    executor.submit(new Task());
}
executor.shutdown();
```
**同步机制**
使用`synchronized`关键字或者显式锁(如`ReentrantLock`)来确保线程安全，避免资源竞争和数据不一致问题。
```java
public synchronized void safeMethod() {
    // Critical section
}

Lock lock = new ReentrantLock();
lock.lock();
try {
    // Critical section
} finally {
    lock.unlock();
}
```
**并发集合类**
Java提供了线程安全的并发集合类，如`ConcurrentHashMap`、`CopyOnWriteArrayList`、`BlockingQueue`等，这些集合类可以简化多线程编程。

**原子变量类**
使用原子变量类(如`AtomicInteger`、`AtomicLong`)来实现无锁并发，减少上下文切换，提高性能。

**CAS操作**
Java的`Atomic`包和`Unsafe`类使用了CAS(Compare-And-Swap)操作来实现无锁算法，从而提高并发性能。
```java
AtomicInteger count = new AtomicInteger(0);
boolean updated = count.compareAndSet(0, 1);
```
**Fork/Join框架**
`ForkJoinPool`用于任务拆分和并行处理，适用于大规模数据的并行计算。
```java
ForkJoinPool pool = new ForkJoinPool();
pool.invoke(new RecursiveTask<Void>() {
    @Override
    protected Void compute() {
        // Task implementation
        return null;
    }
});
```
**异步编程**
使用`CompletableFuture`和异步方法调用，实现非阻塞的并发编程模型。
```java
CompletableFuture.supplyAsync(() -> {
    // Async task
    return "result";
}).thenAccept(result -> {
    // Process result
});
```
**使用非阻塞I/O**
在网络编程中，使用NIO(Non-blocking I/O)实现高并发。NIO提供了选择器(`Selector`)、通道(`Channel`)和缓冲区(`Buffer`)等工具，可以实现高效的非阻塞I/O操作。

**使用第三方并发框架**
如Akka、RxJava、Vert.x等，它们提供了更加高级的并发和反应式编程模型。

### 处理上限为100的接口，突然10000个请求过来了，怎么办？
当接口突然接收到超出其处理能力的大量请求时，需要采取一些策略来防止系统过载，并确保服务的可用性。以下是一些可以在 Java 中使用的方法和策略：
- 限流：限流可以通过限制每个时间单位内允许处理的请求数量来防止过载。常见的限流算法包括漏桶算法和令牌桶算法。
- 负载均衡：使用负载均衡器将请求分配到多个服务器上，以均衡负载。可以使用硬件负载均衡器(如 F5)，也可以使用软件解决方案(如 Nginx 或 Apache)。
- 缓存：缓存可以减轻数据库和后端服务的压力。对于一些可以缓存的请求结果，可以使用 Redis 或 Memcached 进行缓存。
- 降级：在高负载时，可以对某些非关键功能进行降级，例如延迟处理或返回默认值。
- 消息队列：将请求放入消息队列中进行异步处理，如使用 Kafka、RabbitMQ 或 ActiveMQ。
- 扩展资源：根据实际情况扩展服务器资源，包括水平扩展(增加服务器数量)和垂直扩展(增加单个服务器的处理能力)。

通过这些策略和代码示例，可以在处理突然增加的请求量时有效地保护系统。

### 针对转账高并发，在系统层面如何优化？
在处理转账的高并发情况时，系统层面的优化涉及多个方面，包括架构设计、数据库优化、缓存使用、分布式事务处理等。以下是具体的优化策略：
1. 架构设计
   - 水平扩展(Scaling Out)：
       - 集群化：将服务部署到多个服务器上，使用负载均衡器(如Nginx、HAProxy)分发请求，以应对大量并发请求。
       - 微服务架构：将系统拆分为多个独立的微服务，每个微服务负责特定的功能模块，这样可以独立扩展和优化各个服务。
2. 异步处理：
    - 消息队列：使用消息队列(如RabbitMQ、Kafka)处理转账请求，异步处理可以减轻主系统的负载，提升系统的响应速度和稳定性。
    - 事件驱动架构：通过事件驱动机制来处理转账请求，将转账操作分解成多个事件，以分布式的方式处理。
3. 数据库优化
   - 数据库分片：
       - 分库分表：将数据库按照某种规则进行分片(如按用户ID、地域等)，减小单个数据库的压力，提升整体处理能力。
       - 读写分离：使用主从复制架构，将读操作分配到从库，写操作集中在主库，减少读操作对写操作的影响。
   - 索引优化：
       - 合理使用索引：为常用查询添加适当的索引，提升查询速度。
       - 避免全表扫描：确保查询条件能够利用索引，避免全表扫描。
   - 事务优化：
       - 减小事务范围：尽量缩小事务的范围，减少锁的持有时间，提升并发处理能力。
       - 乐观锁与悲观锁：根据业务场景选择合适的锁机制，避免不必要的锁争用。
4. 缓存使用
   - 分布式缓存：
       - 使用缓存系统：如Redis、Memcached，将频繁访问的数据缓存起来，减轻数据库压力。
       - 缓存一致性：设计缓存更新策略，确保数据的一致性，如使用缓存失效、定时刷新等机制。
   - 热点数据缓存：
       - 热点账户缓存：对频繁进行转账操作的账户进行缓存，减少对数据库的直接访问。
       - 事务结果缓存：将转账结果缓存一段时间，避免重复查询数据库。
-  分布式事务处理
  - 两阶段提交(2PC)：
      - 分布式事务协调器：使用两阶段提交协议确保分布式系统中的数据一致性，尽管性能可能受影响，但在需要强一致性的场景下是必要的。
    - TCC(Try-Confirm-Cancel)模式：
        - 三阶段事务控制：分别进行预留资源(Try)、确认操作(Confirm)和取消操作(Cancel)，通过分阶段的事务控制确保一致性和可靠性。
- 其他优化策略
  - 限流与降级：
      - 限流机制：使用令牌桶、漏桶算法等限流机制控制并发请求数量，防止系统过载。
      - 降级策略：在高并发情况下，优先处理核心业务，非核心业务可以降级或延迟处理。
  - 监控与报警：
      - 实时监控：使用监控工具(如Prometheus、Grafana)监控系统性能指标(如QPS、响应时间、错误率等)，及时发现并解决问题。
      - 报警机制：设置关键指标的报警规则，出现异常情况时及时通知相关人员。
  - 优化代码：
      - 提高代码效率：优化业务逻辑，减少不必要的操作和冗余代码。
      - 并发处理：使用合适的并发处理技术(如线程池、协程等)提升系统的并发处理能力。

通过上述多种优化措施，可以有效提升系统在高并发情况下的处理能力和稳定性，确保转账操作的快速和可靠执行。

### 单核CPU上运行多个线程效率一定会高吗
如果线程是 CPU 密集型的，那么多个线程同时运行会导致频繁的线程切换，增加了系统的开销，降低了效率。
如果线程是 IO 密集型的，那么多个线程同时运行可以利用 CPU 在等待 IO 时的空闲时间，提高了效率。

### 死锁
死锁是多个线程同时被阻塞，它们中的一个或者全部都在等待某个资源被释放。由于线程被无限期地阻塞，因此程序不可能正常终止。

> 线程 A 持有资源 2，线程 B 持有资源 1，他们同时都想申请对方的资源，所以这两个线程就会互相等待而进入死锁状态。

产生死锁的四个必要条件：
- 互斥条件：该资源任意一个时刻只由一个线程占用。
- 请求与保持条件：一个线程因请求资源而阻塞时，对已获得的资源保持不放。
- 不剥夺条件：线程已获得的资源在未使用完之前不能被其他线程强行剥夺，只有自己使用完毕后才释放资源。
- 循环等待条件：若干线程之间形成一种头尾相接的循环等待资源关系。

### 如何检测死锁
- 一般死锁可能会导致 CPU 使用率飙升，线程处于 `BLOCKED` 状态，可以通过监控工具查看。
- jConsole： 可以检测死锁，查看线程的状态。
- jstack： 如有死锁，会输出`Found one Java-level deadlock: `线程的状态信息

### 如何预防和避免死锁
破坏死锁的产生的必要条件：
- 破坏请求与保持条件：一次性申请所有的资源。
- 破坏不剥夺条件：占用部分资源的线程进一步申请其他资源时，如果申请不到，可以主动释放它占有的资源。
- 破坏循环等待条件：靠按序申请资源来预防。按某一顺序申请资源，释放资源则反序释放。破坏循环等待条件。

避免死锁就是在资源分配时，借助于算法(比如**银行家算法** )对资源分配进行计算评估，使其进入安全状态。

> 安全状态 指的是系统能够按照某种线程推进顺序(P1、P2、P3……Pn)来为每个线程分配所需资源，直到满足每个线程对资源的最大需求，使每个线程都可顺利完成。称 <P1、P2、P3.....Pn> 序列为安全序列。

### Java内存模型JMM
JMM(Java 内存模型)主要定义了对于一个共享变量，当另一个线程对这个共享变量执行写操作后，这个线程对这个共享变量的可见性。

### 为什么需要Java内存模型
- Java是跨平台的，它需要自己提供一套内存模型以屏蔽系统差异。
- JMM是 Java 定义的并发编程相关的一组规范，主要目的是为了简化多线程编程，增强程序可移植性的。开发者可以利用这些规范更方便地开发多线程程序。
- 对于Java开发者说，不需要了解底层原理，直接使用并发相关的一些关键字和类(比如 `volatile`、`synchronized`、各种 `Lock`)即可开发出并发安全的程序。

### Java内存区域和Java内存模型有什么区别
- Java 内存区域：是 Java 虚拟机管理的内存中的逻辑划分，包括堆、栈、方法区、本地方法栈、程序计数器等。
- Java 内存模型：是 Java 虚拟机规范中定义的一套规范，用于规范 Java 程序中多线程并发访问共享变量的行为。

### JMM是如何抽象线程和主内存之间的关系
Java内存模型通过定义主内存和工作内存之间的关系，以及变量在两者之间的交互规则，确保了多线程环境下的内存可见性和一致性。JMM通过可见性、原子性和有序性等属性，以及happens-before规则，来保证多线程程序的正确性和可预测性。

### 主内存与工作内存
- 主内存：所有的变量(实例字段、静态字段和数组元素)都存储在主内存中，主内存是所有线程共享的内存区域。
- 工作内存：每个线程都有自己的工作内存，工作内存中存储了该线程从主内存中拷贝的变量副本。线程对变量的所有操作(读写等)都必须在工作内存中进行，不能直接操作主内存中的变量。

### 并发编程三大特性
可见性、原子性和有序性
- 可见性(Visibility)：保证一个线程对变量的修改能够被其他线程及时看到。JMM通过内存屏障、`volatile`关键字、`synchronized`块等手段来实现可见性。
- 原子性(Atomicity)：确保操作的不可分割性，即某个操作一旦开始就不会被其他线程看到中间状态。基本的读写操作是原子性的，但更复杂的操作需要借助同步机制(如锁)来保证原子性。
- 有序性(Ordering)：保证程序执行的顺序符合一定规则，避免指令重排序带来的问题。JMM定义了一系列的`happens-before`规则，确保代码的执行顺序对多线程环境是可预测的。

JMM通过上面三种属性来确保线程之间的内存一致性。

### volatile关键字✅

### 如何保证变量的可见性(volatile)
`volatile` 关键字可以保证变量的可见性，如果将变量声明为 `volatile` ，这就指示 JVM，这个变量是共享且不稳定的，每次使用它都到主存中进行读取。

### 如何禁止指令重排序
`volatile` 关键字除了可以保证变量的可见性，还有一个重要的作用就是防止 JVM 的指令重排序。 如果将变量声明为 `volatile` ，在对这个变量进行读写操作的时候，会通过插入特定的 内存屏障 的方式来禁止指令重排序。注意，`volatile` 关键字能保证数据的可见性，但不能保证数据的原子性。`synchronized` 关键字两者都能保证。

### 乐观锁/悲观锁
- 悲观锁：总是假设最坏的情况，认为共享资源每次被访问的时候就会出现问题(比如共享数据被修改)，所以每次在获取资源操作的时候都会上锁，这样其他线程想拿到这个资源就会阻塞直到锁被上一个持有者释放。也就是说，共享资源每次只给一个线程使用，其它线程阻塞，用完后再把资源转让给其它线程。
  - `synchronized`和`ReentrantLock`等独占锁就是悲观锁思想的实现。
  - 高并发的场景下，激烈的锁竞争会造成线程阻塞，大量阻塞线程会导致系统频繁的上下文切换，增加系统的性能开销。
  - 悲观锁可能会存在死锁问题。
  - 通常多用于写比较多的情况(多写场景，竞争激烈)，这样可以避免频繁失败和重试影响性能，悲观锁的开销是固定的。
- 乐观锁：总是假设最好的情况，认为共享资源每次被访问的时候不会出现问题，线程可以不停地执行，无需加锁也无需等待，只是在提交修改的时候去验证对应的资源(也就是数据)是否被其它线程修改了。
  - 具体方法可以使用版本号机制或 CAS 算法，`AtomicInteger`、`LongAdder`等都是乐观锁的实现。
  - 高并发场景，不存在锁竞争造成线程阻塞，也不会有死锁的问题，在性能上往往会更胜一筹。
  - 如果冲突频繁发生(写占比非常多的情况)，会频繁失败和重试，这样同样会非常影响性能，导致 CPU 飙升。
    - `LongAdder`以空间换时间的方式就解决了大量重试问题。
  - 通常多用于写比较少的情况(多读场景，竞争较少)，这样可以避免频繁加锁影响性能。不过，乐观锁主要针对的对象是单个共享变量。

### 如何实现乐观锁
- 版本号机制：在数据表中增加一个版本号字段，每次更新数据的时候，将版本号加一，更新的时候判断版本号是否一致，一致则更新成功，否则失败。
- CAS：如下。

### CAS自旋锁

- CAS：`compare and swap`，用一个预期值和要更新的变量值进行比较，两值相等才会进行更新。是原子操作。
  - V：要更新的变量值(Var)，E：预期值(Expected)，N：拟写入的新值(New)。当且仅当 V 的值等于 E 时，CAS 通过原子方式用新值 N 来更新 V 的值。如果不等，说明已经有其它线程更新了 V，则当前线程放弃更新。

> 举一个简单的例子：线程 A 要修改变量 i 的值为 6，i 原值为 1(V = 1，E=1，N=6，假设不存在 ABA 问题)。i 与 1 进行比较，如果相等， 则说明没被其他线程修改，可以被设置为 6 。i 与 1 进行比较，如果不相等，则说明被其他线程修改，当前线程放弃更新，CAS 操作失败。当多个线程同时使用 CAS 操作一个变量时，只有一个会胜出，并成功更新，其余均会失败，但失败的线程并不会被挂起，仅是被告知失败，并且允许再次尝试，当然也允许失败的线程放弃操作。

### CAS算法存在问题

### ABA问题
变量 V 初次读取的时候是 A 值，并且在使用 CAS算法 准备赋值的时候检查到它仍然是 A 值，不能说明它的值没有被其他线程修改过，因为在这段时间它的值可能被改为其他值(B)，然后又改回 A，那 CAS 操作就会误认为它从来没有被修改过。这个问题被称为 CAS 操作的 "ABA"问题。

ABA问题的解决思路是在变量前面追加上版本号或者时间戳。

**循环时间长开销大**
CAS 经常会用到自旋操作来进行重试，自旋时间长，如果 CAS 一直不成功，会导致 CPU 一直自旋，这样会消耗 CPU 资源。

**只能保证一个共享变量的原子操作**
CAS 只对单个共享变量有效，当操作涉及跨多个共享变量时 CAS 无效，这时可以使用`AtomicReference`。

### synchronized关键字
`synchronized` 是一种同步锁。主要解决的是多个线程之间访问资源的同步性，可以保证被它修饰的方法或者代码块在任意时刻只能有一个线程执行。

### JDK1.6之后synchronized的优化
在 Java 早期版本中，synchronized属于重量级锁，效率低下，但是在 Java 6 之后，`synchronized`引入了大量的优化如自旋锁、适应性自旋锁、锁消除、锁粗化、偏向锁、轻量级锁等技术来减少锁操作的开销，使得 synchronized 的效率大大提高。

### 锁升级原理了解吗
锁主要存在四种状态，依次是：无锁状态、偏向锁状态、轻量级锁状态、重量级锁状态，他们会随着竞争的激烈而逐渐升级。注意锁可以升级不可降级，这种策略是为了提高获得锁和释放锁的效率。

### 如何使用synchronized
- 修饰实例方法：锁住当前实例对象。
```java
synchronized void method() {
    //业务代码
}
```
- 修饰静态方法：锁住当前类的 Class 对象。
```java
synchronized static void method() {
    //业务代码
}
```
- 修饰代码块：锁住括号里面的对象。
  - `synchronized(object)` 表示进入同步代码库前要获得 给定对象的锁。
  - `synchronized(类.class)` 表示进入同步代码前要获得 给定 `Class` 的锁

```
synchronized(对象) {
    //业务代码
}
synchronized(类.class) {
    //业务代码
}
```

### synchronized底层原理
`synchronized` 关键字是通过对象内部的一个叫做监视器锁(`monitor`)来实现的。

- `synchronized` 同步语句块的实现使用的是 `monitorenter` 和 `monitorexit` 指令，其中 `monitorenter` 指令指向同步代码块的开始位置，`monitorexit` 指令则指明同步代码块的结束位置。
- `synchronized` 修饰的方法使用 `ACC_SYNCHRONIZED` 标识，JVM 通过该 `ACC_SYNCHRONIZED` 访问标志来辨别一个方法是否声明为同步方法，从而执行相应的同步调用。

两者的本质都是对对象监视器 `monitor` 的获取。

### ReentrantLock
`ReentrantLock` 是一个可重入且独占式的锁，和 `synchronized` 关键字类似。不过，`ReentrantLock` 更灵活强大，增加了轮询、超时、中断、公平锁和非公平锁等高级功能。

`ReentrantLock` 里面有一个内部类 `Sync`，`Sync` 继承抽象类 `AQS(AbstractQueuedSynchronizer，抽象队列同步器)`，添加锁和释放锁的大部分操作实际上都是在 `Sync` 中实现的。`Sync` 有公平锁 `FairSync` 和非公平锁 `NonfairSync` 两个子类。

### AQS核心思想
AQS 核心思想是，如果被请求的共享资源空闲，则将当前请求资源的线程设置为有效的工作线程，并且将共享资源设置为锁定状态。如果被请求的共享资源被占用，使用基于`CLH`锁实现的一套线程阻塞等待以及被唤醒时锁分配机制。

以可重入互斥锁 `ReentrantLock` 为例，其内部维护了一个使用`volatile`修饰(保证线程可见性)的`state`变量，用来表示锁的占用状态。`state` 的初始值为 0，表示锁处于未锁定状态。当线程 A 调用 `lock()` 方法时，会尝试通过 `tryAcquire()` 方法独占该锁，并让 `state` 的值加 1。如果成功了，那么线程 A 就获取到了锁。如果失败了，那么线程 A 就会被加入到一个等待队列(`CLH` 锁队列)中，直到其他线程释放该锁。假设线程 A 获取锁成功了，释放锁之前，A 线程自己是可以重复获取此锁的(`state` 会累加)。这就是可重入性的体现：一个线程可以多次获取同一个锁而不会被阻塞。但是，这也意味着，一个线程必须释放与获取的次数相同的锁，才能让 `state` 的值回到 0，也就是让锁恢复到未锁定状态。只有这样，其他等待的线程才能有机会获取该锁。

> `CLH`锁是对自旋锁的一种改进，是一个虚拟的双向队列(虚拟的双向队列即不存在队列实例，仅存在结点之间的关联关系)，暂时获取不到锁的线程将被加入到该队列中。AQS 将每条请求共享资源的线程封装成一个 `CLH` 队列锁的一个结点(`Node`)来实现锁的分配。在 `CLH` 队列锁中，一个节点表示一个线程，它保存着线程的引用(`thread`)、 当前节点在队列中的状态(`waitStatus`)、前驱节点(`prev`)、后继节点(`next`)。

### AQS资源共享模式
AQS 支持两种资源共享方式：独占和共享。
- `Exclusive`(独占，只有一个线程能执行，如`ReentrantLock`)
- `Share`(共享，多个线程可同时执行，如`Semaphore/CountDownLatch`)

> 也可以自定义同步器同时实现独占和共享，如`ReentrantReadWriteLock`，读操作时多个线程可以同时进行，写操作时只能一个线程进行。

### 公平锁/非公平锁
- 公平锁：锁被释放之后，先申请的线程先得到锁。性能较差一些，因为公平锁为了保证时间上的绝对顺序，上下文切换更频繁。
- 非公平锁：锁被释放之后，后申请的线程可能会先获取到锁，是随机或者按照其他优先级排序的。性能更好，但可能会导致某些线程永远无法获取到锁。

### synchronized和ReentrantLock有什么区别？
两者都是可重入锁
- `synchronized` 是关键字，是内置的语言实现，`ReentrantLock` 是一个类。
- `synchronized` 是 JVM 实现的，`ReentrantLock` 是 JDK 实现的(也就是 API 层面，需要 `lock()` 和 `unlock()` 方法配合 `try/finally` 语句块来完成)
- `ReentrantLock` 比 `synchronized` 增加了一些高级功能：
  - 等待可中断 ： `ReentrantLock`提供一种能够中断等待锁的线程的机制，通过 `lock.lockInterruptibly()`实现。
  - 可实现公平锁： `ReentrantLock`可以指定是公平锁还是非公平锁。而`synchronized`只能是非公平锁。通过`ReentrantLock(boolean fair)`构造方法来指定是否是公平的。
  - 可实现选择性通知(锁可以绑定多个条件)： `synchronized`关键字与`wait()`和`notify()/notifyAll()`方法相结合可以实现等待/通知机制。`ReentrantLock`类当然也可以，但需借助`Condition`接口与`newCondition()`方法。

> 可重入锁 也叫递归锁，指的是线程可以再次获取自己的内部锁。比如一个线程获得了某个对象的锁，此时这个对象锁还没有释放，当其再次想要获取这个对象的锁(一个类中有多个`synchronized`修饰的方法)的时候还是可以获取的，如果是不可重入锁的话，就会造成死锁。

### 可中断锁/不可中断锁
- 可中断锁：正在等待获取锁的线程可以选择放弃等待，改为处理其他事情。`ReentrantLock` 是可中断锁。
- 不可中断锁：一旦线程申请了锁，就只能等到拿到锁以后才能进行其他的逻辑处理。 `synchronized` 是不可中断锁。

### ReentrantReadWriteLock
是一个可重入的读写锁，既可以保证多个线程同时读的效率，同时又可以保证有写入操作时的线程安全。

`ReentrantReadWriteLock` 其实是两把锁，一把是 `WriteLock` (写锁)，一把是 `ReadLock`(读锁) 。读锁是**共享锁**，写锁是**独占锁**。读锁可以被同时读，可以同时被多个线程持有，而写锁最多只能同时被一个线程持有。

> 一般锁进行并发控制的规则：读读互斥、读写互斥、写写互斥。
> 读写锁进行并发控制的规则：读读不互斥、读写互斥、写写互斥(只有读读不互斥)。

### 共享锁/独占锁
- 共享锁：一把锁可以被多个线程同时获得。
- 独占锁：一把锁只能被一个线程获得。

### 线程持有读锁还能获取写锁吗
- 在线程持有读锁的情况下，该线程不能取得写锁(因为获取写锁的时候，如果发现当前的读锁被占用，就马上获取失败，不管读锁是不是被当前线程持有)。
- 在线程持有写锁的情况下，该线程可以继续获取读锁(获取读锁时如果发现写锁被占用，只有写锁没有被当前线程占用的情况才会获取失败)。

### 读锁为什么不能升级为写锁
- 写锁可以降级为读锁，但是读锁却不能升级为写锁。这是因为读锁升级为写锁会引起线程的争夺，毕竟写锁属于是独占锁，这样的话，会影响性能。
- 可能会有死锁问题发生。

### StampedLock
`StampedLock` 是 JDK 1.8 引入的性能更好的读写锁，没有实现 `Lock`或 `ReadWriteLock`接口，而是基于 `CLH` 锁独立实现的。

提供三种访问模式：
- 写锁：独占锁，一把锁只能被一个线程获得。当一个线程获取写锁后，其他请求读锁和写锁的线程必须等待。类似于 ReentrantReadWriteLock 的写锁，不过这里的写锁是不可重入的。
- 读锁 (悲观读)：共享锁，没有线程获取写锁的情况下，多个线程可以同时持有读锁。如果己经有线程持有写锁，则其他线程请求获取该读锁会被阻塞。类似于 ReentrantReadWriteLock 的读锁，不过这里的读锁是不可重入的。
- 乐观读：允许多个线程获取乐观读以及读锁。同时允许一个写线程获取写锁。(性能比`ReadWriteLock`更好的原因)

### ThreadLocal
`ThreadLocal` 是一个线程内部的数据存储类，可以在每个线程中创建一个变量副本，各个线程之间的数据互不干扰。可以使用 `get()` 和 `set()` 方法来获取默认值或将其值更改为当前线程所存的副本的值，从而避免了线程安全问题。

### ThreadLocal原理
`ThreadLocal` 通过 `ThreadLocalMap` 来实现线程内部的数据存储。`ThreadLocalMap` 是 `ThreadLocal` 的一个静态内部类，每个线程中都有一个 `ThreadLocalMap`，`ThreadLocal` 通过 `get()`、`set()` 方法访问 `ThreadLocalMap`。在一个线程中创造多个`ThreadLocal`对象，这个许多个`ThreadLocal`对象会被放到一个`ThreadLocalMap`中。

> `ThreadLocalMap`可以理解为一个定制化的 `HashMap`，`key` 是 `ThreadLocal` 对象，`value` 是存储的值。
> 可以存在这种情况： 在线程 1 中创建了两个 `ThreadLocal` 对象，在线程 1 中只有一个 `ThreadLocal` 对象。

### ThreadLocal内存泄漏
`ThreadLocalMap` 中使用的 `key` 为 `ThreadLocal` 的弱引用，而 `value` 是强引用。所以，如果 `ThreadLocal` 没有被外部强引用的情况下，在垃圾回收的时候，`key` 会被清理掉，而 `value` 不会被清理掉。这样一来就会出现 `key` 为 `null` 的 键值对。如果不做任何措施的话，`value` 永远无法被 GC 回收，这个时候就可能会产生内存泄露。

其实`ThreadLocalMap`实现中已经考虑了内存泄漏问题，在调用 `set()`、`get()`、`remove()` 方法的时候，会清理掉 `key` 为 `null` 的记录。不过使用完 `ThreadLocal`方法后最好手动调用`remove()`方法。

> `HashMap` 的 `key` 和 `value` 都是强引用，因此不会存在弱引用导致的内存泄漏问题。

### 线程池
线程池就是管理一系列线程的资源池。当有任务要处理时，直接从线程池中获取线程来处理，处理完之后线程并不会立即被销毁，而是等待下一个任务。

### 为什么用线程池
- 降低资源消耗。通过重复利用已创建的线程降低线程创建和销毁造成的消耗。
- 提高响应速度。当任务到达时，任务可以不需要等到线程创建就能立即执行。
- 提高线程的可管理性。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一的分配，调优和监控。

> 在计算机领域中有很多池化技术，线程池、数据库连接池、内存池、对象池等等，都是为了减少每次获取资源的消耗，提高对资源的利用率。

### 创建线程池的方式
- 通过`ThreadPoolExecutor`构造函数来创建 _**(推荐)**_ 。
- 通过`Executors`工具类来创建 _**(不推荐)**_ 。通过这种方式可以创建出多种类型的线程池
  - `FixedThreadPool`：固定大小的线程池。
  - `SingleThreadPool`：只有一个线程的线程池。
  - `CachedThreadPool`：可根据实际情况调整线程数量的线程池。线程池的线程数量不确定，但若有空闲线程可以复用，则会优先使用可复用的线程。
  - `ScheduledThreadPool`：定时任务的线程池。

### 为什么不推荐使用内置线程池
- `FixedThreadPool`，`SingleThreadPool`，`ScheduledThreadPool`：使用的是无界的 `LinkedBlockingQueue`，允许的请求队列长度为 `Integer.MAX_VALUE`，可能会堆积大量的请求，从而导致 OOM。
- `CachedThreadPool`：使用同步队列`SynchronousQueue`，允许的创建线程数量为 `Integer.MAX_VALUE`，可能会创建大量线程，从而导致 OOM。

### ThreadPoolExecutor参数
3 个最重要的参数：
- `corePoolSize` ： 任务队列未达到队列容量时，最大可以同时运行的线程数量。
- `maximumPoolSize` ： 任务队列中存放的任务达到队列容量的时候，当前可以同时运行的线程数量变为最大线程数。
- `workQueue`： 新任务来的时候会先判断当前运行的线程数量是否达到核心线程数，如果达到的话，新任务就会被存放在队列中。

其他常见参数 ：
- `keepAliveTime`：线程池中的线程数量大于 `corePoolSize` 的时候，如果这时没有新的任务提交，核心线程外的线程不会立即销毁，而是会等待，直到等待的时间超过了 `keepAliveTime`才会被回收销毁。
- `unit` ： `keepAliveTime` 参数的时间单位。
- `threadFactory` ：`executor` 创建新线程的时候会用到。
- `handler` ：拒绝策略,当提交的任务过多而不能及时处理时，可以定制策略来处理任务。

### 线程池的拒绝策略
- `AbortPolicy`：直接抛出异常，阻止系统正常工作。
- `CallerRunsPolicy`：既不会抛弃任务，也不会抛出异常，而是将任务回退给调用者，使用调用者的线程来执行任务。
- `DiscardOldestPolicy`：丢弃最早的未处理的任务请求。
- `DiscardPolicy`：不处理新任务，直接丢弃掉。

### CallerRunsPolicy策略风险及解决方案
**风险**
`CallerRunsPolicy` 保证任何一个任务请求都要被执行，但这非常耗时，且如果提交任务的线程是主线程，可能会导致主线程阻塞，影响程序的正常运行。

**解决方案**
1. 使用`CallerRunsPolicy`是希望所有任务都被执行，而暂时无法处理的任务又被保存在阻塞队列`BlockingQueue`中，这样在内存允许的情况下，可以增加阻塞队列`BlockingQueue`的大小以容纳更多的任务，同时调整线程池的`maximumPoolSize` (最大线程数)参数，这样可以提高任务处理速度，避免累计在 `BlockingQueue`的任务过多导致内存用完。
2. 使用`ThreadPoolExecutor`的`setRejectedExecutionHandler`方法，自定义拒绝策略，比如将任务保存到数据库中，或者将任务保存到消息队列中，等待下次执行，或者使用 `Redis`缓存任务。

### 线程池常用阻塞队列
- `LinkedBlockingQueue`：基于链表的阻塞队列，大小默认为 `Integer.MAX_VALUE`，即任务队列永远不会放满。`FixedThreadPool` 和 `SingleThreadExector`使用，前者只能创建核心线程数的线程，后者只能创建一个线程。
- `SyncronousQueue`：同步队列，1CachedThreadPool1使用。不存储元素，目的是保证对于提交的任务，如果有空闲线程，则使用空闲线程来处理；否则新建一个线程来处理任务。`CachedThreadPool` 的最大线程数是 `Integer.MAX_VALUE` ，可以理解为线程数是可以无限扩展的，可能会创建大量线程，从而导致 OOM。
- `DelayedWorkQueue`：延迟阻塞队列，`ScheduledThreadPool` 和 `SingleThreadScheduledExecutor`使用。队列中的元素只有当其指定的延迟时间到了才能从队列中取出。内部元素并不是按照放入的时间排序，而是会按照延迟的时间长短对任务进行排序，内部采用的是“堆”的数据结构，可以保证每次出队的任务都是当前队列中执行时间最靠前的。添加元素满了之后会自动扩容原来容量的 1/2，即永远不会阻塞，最大扩容可达 `Integer.MAX_VALUE`，所以最多只能创建核心线程数的线程。

### 线程池处理任务流程
1. 如果当前运行的线程数小于核心线程数，那么就会新建一个线程来执行任务。
2. 如果当前运行的线程数等于或大于核心线程数，但是小于最大线程数，并且任务队列没满，那么就把该任务放入到任务队列里等待执行。
3. 如果向任务队列投放任务失败(任务队列已经满了)，但是当前运行的线程数是小于最大线程数的，就新建一个线程来执行任务。
4. 如果当前运行的线程数已经等同于最大线程数了，新建线程将会使当前运行的线程超出最大线程数，那么当前任务会被拒绝，根据拒绝策略处理。

### 线程池中线程异常后销毁还是复用？
两种情况：
- 使用`execute()`时，未捕获异常导致线程终止，线程池创建新线程替代；
- 使用`ExecutorService.submit()`时，异常被封装在`Future`中，线程继续复用。

### 如何设定线程池大小
- CPU 密集型任务(N+1)： 将线程数设置为 N(CPU 核心数)+1。比 CPU 核心数多出来的一个线程是为了防止线程偶发的缺页中断，或者其它原因导致的任务暂停而带来的影响。
- I/O 密集型任务(2N)： 这种情况系统大部分的时间用来处理 I/O 交互，此时可以将 CPU 交出给其它线程使用，可以多配些线程。

### 如何设计一个根据任务优先级执行的线程池
- 使用`PriorityBlockingQueue`作为任务队列，队列中的任务按照优先级顺序执行。
- 创建 `PriorityBlockingQueue` 时传入一个 `Comparator` 对象来指定任务之间的排序规则(推荐)。

这样还存在一些问题：
- `PriorityBlockingQueue` 是无界的，可能堆积大量的请求，从而导致 OOM。
  - 解决方法：继承`PriorityBlockingQueue` 并重写一下 `offer` 方法(入队)的逻辑，当插入的元素数量超过指定值就返回 `false 。
- 可能会导致饥饿问题，即低优先级的任务长时间得不到执行。
  - 解决方法：优化设计，等待时间过长的任务会被移除并重新添加到队列中，但是优先级会被提升。

### 异步调用Future类

`Future` 类是异步思想的典型运用，主要用在一些需要执行耗时任务的场景，具体来说是这样的：当主线程执行某一耗时的任务时，可以将这个耗时任务交给一个子线程去异步执行，同时主线程做其他事情，不用傻傻等待耗时任务执行完成。等事情干完后，再通过 Future 类获取到耗时任务的执行结果。这样一来，程序的执行效率就明显提高了。

`Future`类是一个泛型接口，主要包含下面几个方法：
```
// V 代表了Future执行的任务返回值的类型
public interface Future<V> {
    // 取消任务执行，成功取消返回 true，否则返回 false
    boolean cancel(boolean mayInterruptIfRunning);
    // 判断任务是否被取消
    boolean isCancelled();
    // 判断任务是否已经执行完成
    boolean isDone();
    // 获取任务执行结果
    V get() throws InterruptedException, ExecutionException;
    // 指定时间内没有返回计算结果就抛出 TimeOutException 异常
    V get(long timeout, TimeUnit unit)
        throws InterruptedException, ExecutionException, TimeoutExceptio
}
```

### Callable和Future关系
`Callable` 用于定义可以返回结果的任务，`Future` 用于获取 `Callable` 任务的异步结果。两者常常结合使用，以便在并发编程中有效地管理任务执行和结果处理。
- 任务提交与执行：通常通过 `ExecutorService` 提交 `Callable` 任务，`submit` 方法返回一个 `Future` 对象。
- 异步结果获取：`Future` 对象可以用来获取 `Callable` 任务的返回结果或处理异常。

### 实现异步调用的多种方法
1. `Future`类：通过 `Future` 类的 `get()` 方法获取异步任务的执行结果。
2. `CompletableFuture`类：Java 8 引入的 `CompletableFuture` 类，提供了更强大的方法链和回调机制，用于构建复杂的异步逻辑和并行操作。
3. `CompletionService`类和`Callable`类：`CompletionService` 是一个接口，它允许提交一组 Callable 任务，这些任务将异步执行，并且可以按照完成的顺序获取它们的结果。
4. `FutureTask`类：`FutureTask` 是 `Future` 的一个实现类，它实现了 `Runnable` 接口，可以作为 `Runnable` 被线程执行，也可以作为 `Future` 得到任务的执行结果。
5. `ExecutorService`类：`ExecutorService` 是一个接口，它是 `Executor` 的子接口，它提供了更丰富的线程池功能，可以提交任务、执行任务、关闭线程池等。
6. `ForkJoinPool`、`ScheduledExecutorService`等。

### CompletableFuture类有什么用？
`CompletableFuture` 类是 Java 8 中引入的一个增强版 `Future`，它不仅可以代表一个异步计算的结果，还提供了强大的方法链和回调机制，用于构建复杂的异步逻辑和并行操作。与 `Future` 相比，`CompletableFuture` 更加灵活和强大，支持函数式编程、异步任务编排组合等能力。

### CompletableFuture类异步调用实现
```java
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutionException;
public class AsyncExample {
    public static void main(String[] args) {
        // 创建一个CompletableFuture来执行异步任务
        CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
            // 模拟一个长时间运行的任务
            try {
                Thread.sleep(2000); // 休眠2秒
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            return "任务完成";
        });
        // 注册一个回调函数，当任务完成时获取结果
        future.thenAccept(result -> {
            System.out.println("异步任务结果: " + result);
        });
        // 主线程继续执行其他操作
        System.out.println("主线程继续执行...");
        // 阻塞主线程，直到异步任务完成(可选)
        try {
            // 这一步会阻塞主线程，直到异步任务完成
            String result = future.get();
            System.out.println("异步任务完成后获取的结果: " + result);
        } catch (InterruptedException | ExecutionException e) {
            e.printStackTrace();
        }
    }
}
```


### 虚拟线程
虚拟线程在 Java 21 正式发布，这是一项重量级的更新。

虚拟线程(`Virtual Thread`)是 JDK 而不是操作系统实现的轻量级线程(`Lightweight Process，LWP`)，由 JVM 调度。许多虚拟线程共享同一个操作系统线程，虚拟线程的数量可以远大于操作系统线程的数量。

在没有虚拟线程之前，JVM使用的是**平台线程**，平台线程与内核线程的对应关系在 Windows 和 Linux 等主流操作系统中是一对一的(Solaris 系统是一个特例，HotSpot VM 在 Solaris 上支持多对多和一对一)，这样会导致线程创建和销毁的开销较大，线程切换的开销也较大。

JVM 调度程序通过平台线程(载体线程)来管理虚拟线程，一个平台线程可以在不同的时间执行不同的虚拟线程(多个虚拟线程挂载在一个平台线程上)，当虚拟线程被阻塞或等待时，平台线程可以切换到执行另一个虚拟线程。

**优点**
- 非常轻量级：可以在单个线程中创建成百上千个虚拟线程而不会导致过多的线程创建和上下文切换。
- 简化异步编程： 虚拟线程可以简化异步编程，使代码更易于理解和维护。它可以将异步代码编写得更像同步代码，避免了回调地狱(Callback Hell)。
- 减少资源开销： 相比于操作系统线程，虚拟线程的资源开销更小。本质上是提高了线程的执行效率，从而减少线程资源的创建和上下文切换。

**缺点**
不适用于计算密集型任务： 虚拟线程适用于 I/O 密集型任务，但不适用于计算密集型任务，因为密集型计算始终需要 CPU 资源作为支持。
依赖于语言或库的支持： 协程需要编程语言或库提供支持。不是所有编程语言都原生支持协程。比如 Java 实现的虚拟线程。

## IO

IO(`Input/Output`)即输入/输出，数据输入到计算机内存的过程即输入，反之输出到外部存储(比如数据库，文件，远程主机)的过程即输出。

IO 流的 40 多个类都是从如下 4 个抽象类基类中派生出来的。
- `InputStream/Reader`： 所有的输入流的基类，前者是字节输入流，后者是字符输入流。
- `OutputStream/Writer`： 所有输出流的基类，前者是字节输出流，后者是字符输出流。

### Java为什么要分字节流和字符流
Java 分为字符流和字节流是为了更好地处理不同类型的数据、简化程序编写和提高代码的可读性与可维护性。
- 字符流自动处理字符编码问题，非常适合文本数据；
- 字节流则适合处理所有类型的文件，特别是非文本数据如图片、视频、音频等。

### 字节流和字符流区别
- 字节流处理的基本单位是字节(8 bit)，字符流处理的基本单位是字符(16 bit)。
- 字节流可以处理所有类型的数据，包括二进制文件(如图像、视频、音频)以及文本文件，字符流专门用于处理文本数据(字符数据)，适合处理各种语言的文本文件。
- 字节流不进行字符编码转换，直接读写原始的二进制数据，字符流自动进行字符编码和解码，读写时可以处理不同的字符集(如UTF-8、UTF-16、ISO-8859-1等)。

### IO设计模式总结
- 装饰器模式：可以在不改变原有对象的情况下拓展其功能。可以对原始类嵌套使用多个装饰器类。
- 适配器模式：主要用于接口互不兼容的类的协调工作，适配器分为对象适配器和类适配器，类适配器使用继承关系来实现，对象适配器使用组合关系来实现。
- 工厂模式：定义一个用于创建对象的接口，让子类决定实例化哪一个类。工厂方法使一个类的实例化延迟到其子类。
- 观察者模式：定义了对象之间的一对多依赖，这样一来，当一个对象改变状态，依赖它的对象都会收到通知并自动更新。

### 装饰器模式和适配器模式区别
- 装饰器模式 更侧重于动态地增强原始类的功能，装饰器类需要跟原始类继承相同的抽象类或者实现相同的接口。并且，装饰器模式支持对原始类嵌套使用多个装饰器。
- 适配器模式 更侧重于让接口不兼容而不能交互的类可以一起工作，当调用适配器对应的方法时，适配器内部会调用适配者类或者和适配类相关的类的方法，这个过程透明的。适配器和适配者两者不需要继承相同的抽象类或者实现相同的接口。

### 3种常见的IO模型
- BIO(`Blocking IO`)：同步阻塞 IO 模型中，应用程序发起 read 调用后，会一直阻塞，直到内核把数据拷贝到用户空间。
  - 优点：编程简单，代码易于理解。
  - 缺点：并发性能低，每个连接都需要独立的线程进行处理，线程资源消耗大。
- NIO(`Non-blocking IO`)：同步非阻塞 IO 模型中，应用程序发起 read 调用后，会立即返回，不会阻塞，应用程序需要不断轮询内核，直到数据准备好。
  - 优点：单线程可以处理多个连接，提高并发性能。
  - 缺点：应用程序不断进行 I/O 系统调用轮询数据是否已经准备好的过程是十分消耗 CPU 资源的。
- AIO(`Asynchronous IO`)：异步非阻塞 IO 模型中，应用程序发起 read 调用后，不会阻塞，内核会在数据准备好后通知应用程序。
  - 优点：异步 IO 模型的优势在于 IO 操作完全由操作系统来完成，应用程序只需要在数据准备好时得到通知，不需要不断轮询内核。
  - 缺点：AIO 是 Java 7 引入的，不太常用，且实现较为复杂。




## JVM
### 内存区域
Java内存区域/Java虚拟机内存 通常被划分为多个部分，每个部分在Java应用程序运行时发挥不同的作用。

线程私有的内存区域包括：
- **程序计数器**：
  - 这是当前线程执行的字节码行号指示器。每个线程都有一个独立的程序计数器，指向下一条要执行的字节码指令。
  - 如果正在执行的是本地方法，这个计数器是未指定值(undefined)。
- **Java虚拟机栈**：
  - Java虚拟机栈用于存储局部变量、操作数栈、中间结果等。
  - 栈帧(Stack Frame)是虚拟机栈中的基本元素，每个方法调用对应一个栈帧。栈帧包括局部变量表、操作数栈和帧数据(如方法返回地址、动态链接信息等)。
- **本地方法栈**：
  - 本地方法栈与Java虚拟机栈类似，只不过它为本地方法(Native Methods)服务。
  - 一般情况下，使用C语言等实现的本地方法的调用也会在本地方法栈中执行。

线程共享的内存区域包括：
- **堆**：
  - Java堆是所有线程共享的内存区域，用于存放对象实例及数组。所有对象实例及数组都在堆上分配。
  - 堆是垃圾收集器管理的主要区域，因此也被称为“GC堆”。
  - 堆内存通常又被划分为年轻代(Young Generation)和老年代(Old Generation)，其中年轻代进一步划分为Eden区、Survivor0区和Survivor1区。
    - JDK1.8将字符串常量池从方法区中移除，放到堆中，这样可以避免字符串常量池占用过多的方法区内存。
- **方法区**：
  - 方法区也是所有线程共享的内存区域，用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译后的代码等数据。
  - 方法区在JVM规范中是堆的一部分，但实际实现中常被单独划分，比如HotSpot虚拟机中的永久代(Permanent Generation，已在Java 8中被元空间Metaspace取代)。
- **运行时常量池**：
  - 运行时常量池是方法区的一部分，用于存放编译期生成的各种字面量和符号引用。这些常量在类加载后被放入方法区的运行时常量池中。
  - JDK1.7位于方法区，JDK1.8位于直接内存中的元空间。

本地内存：
- 直接内存：
  - 直接内存是一种特殊的内存缓冲区，并不在 Java 堆或方法区中分配的，而是通过 JNI 的方式在本地内存上分配的。
  - JDK1.8之后加入了元空间，元空间是直接内存的一部分，用于存放类的元数据信息。
  - JDK1.8将运行时常量池放到直接内存的元空间中，这样可以避免方法区内存溢出的问题。


**内存模型与内存管理**
- **内存模型(Java Memory Model, JMM)**：JMM定义了线程之间如何通过内存进行交互的规则，特别是如何确保操作的可见性、原子性和有序性。
- **垃圾收集(Garbage Collection, GC)**：Java通过垃圾收集机制自动管理堆内存，回收无用对象以释放空间。常见的垃圾收集器包括Serial、Parallel、CMS和G1等。

**总结**
Java内存区域划分的目的是为了提高性能和管理复杂性。程序计数器、Java虚拟机栈和本地方法栈是线程私有的，而堆和方法区是线程共享的。Java内存模型保证了多线程环境下的内存可见性和指令重排序的正确性，垃圾收集器则帮助管理堆内存，提升程序运行效率和稳定性。

### 成员变量/全局变量/局部变量存放在哪里
- 成员变量(实例变量)存储在堆内存中，每个对象都有自己的一份成员变量。
- 静态变量(类变量)存储在方法区(或元空间)中，是类级别的变量，共享给所有实例。
- 局部变量存储在栈内存中，属于方法调用帧的一部分。

### 对象的创建过程(5步)
1. 类加载检查。虚拟机遇到一条 new 指令时，首先将去检查这个指令的参数是否能在常量池中定位到这个类的符号引用，并且检查这个符号引用代表的类是否已被加载过、解析和初始化过。如果没有，那必须先执行相应的类加载过程。
2. 分配内存。在类加载检查通过后，接下来虚拟机将为新生对象分配内存。对象所需的内存大小在类加载完成后便可确定，为对象分配空间的任务等同于把一块确定大小的内存从 Java 堆中划分出来。分配方式有 “指针碰撞” 和 “空闲列表” 两种，选择哪种分配方式由 Java 堆是否规整决定，而 Java 堆是否规整又由所采用的垃圾收集器是否带有压缩整理功能决定。
3. 初始化零值。内存分配完成后，虚拟机需要将分配到的内存空间都初始化为零值(不包括对象头)，这一步操作保证了对象的实例字段在 Java 代码中可以不赋初始值就直接使用，程序能访问到这些字段的数据类型所对应的零值。
4. 设置对象头。初始化零值完成之后，虚拟机要对对象进行必要的设置，例如这个对象是哪个类的实例、如何才能找到类的元数据信息、对象的哈希码、对象的 GC 分代年龄等信息。 这些信息存放在对象头中。 另外，根据虚拟机当前运行状态的不同，如是否启用偏向锁等，对象头会有不同的设置方式。
5. 执行 init 方法。在上面工作都完成之后，从虚拟机的视角来看，一个新的对象已经产生了，但从 Java 程序的视角来看，对象创建才刚开始，<init> 方法还没有执行，所有的字段都还为零。所以一般来说，执行 new 指令之后会接着执行 <init> 方法，把对象按照程序员的意愿进行初始化，这样一个真正可用的对象才算完全产生出来。

### 对象的内存布局
对象在内存中的布局可以分为 3 块区域： 
- 对象头(`Header`)： 
  - 标记字段(`Mark Word`)：用于存储对象自身的运行时数据， 如哈希码(HashCode)、GC 分代年龄、锁状态标志、线程持有的锁、偏向线程 ID、偏向时间戳等等。
  - 类型指针(`Klass Word`)：对象指向它的类元数据的指针，虚拟机通过这个指针来确定这个对象是哪个类的实例。
- 实例数据(`Instance Data`)： 对象真正存储的有效信息，也是在程序中所定义的各种类型的字段内容。
- 对齐填充(`Padding`)： 不是必然存在的，也没有什么特别的含义，仅仅起占位作用。

### 对象的访问定位
建立对象就是为了使用对象，Java 程序通过栈上的 reference 数据来操作堆上的具体对象。对象的访问方式由虚拟机实现而定，目前主流的访问方式有：使用句柄、直接指针。
- 句柄。如果使用句柄的话，那么 Java 堆中将会划分出一块内存来作为句柄池，reference 中存储的就是对象的句柄地址，而句柄中包含了对象实例数据与对象类型数据各自的具体地址信息。
- 直接指针。如果使用直接指针访问，reference 中存储的直接就是对象的地址。

两种对象访问方式各有优势。使用句柄来访问的最大好处是 reference 中存储的是稳定的句柄地址，在对象被移动时只会改变句柄中的实例数据指针，而 reference 本身不需要修改。使用直接指针访问方式最大的好处就是速度快，它节省了一次指针定位的时间开销。

### 垃圾回收机制✅
**垃圾回收机制定义**
垃圾回收(GC)是内存管理的核心组成部分，它负责自动回收不再使用的内存空间。在Java中，程序员不需要手动释放对象占用的内存，一旦对象不再被引用，垃圾回收器就会在适当的时机回收它们所占用的内存。这样可以避免内存泄漏和野指针，从而大大减轻了程序员的负担，也使得Java成为一个相对安全、易于开发的编程语言。

**堆空间结构**
堆内存是垃圾回收的主要区域，JDK1.7之前，堆通常被分为新生代、老年代和永久代三部分。JDK1.8之后，永久代被元空间取代，堆内存结构变为新生代(Eden，S0，S1)、老年代和元空间。

**内存分配和回收原则**
- 对象优先在新生代分配内存，新生代内存分为 Eden 区、Survivor0 区和 Survivor1 区。当 Eden 区内存不足时，虚拟机会触发一次 Minor GC，将 Eden 区中存活的对象复制到 Survivor0 区，然后清空 Eden 区。Survivor0 和 Survivor1 交替使用，当其中一个区域内存不足时，虚拟机会将存活的对象复制到另一个区域，然后清空原区域。当对象在新生代经历多次复制仍然存活时，会被晋升到老年代。
- 大对象直接进入老年代。如果对象的大小超过了新生代的某个阈值，虚拟机会直接将这个对象分配到老年代。
- 长期存活的对象将进入老年代。虚拟机会给每个对象定义一个年龄计数器，对象在新生代每经历一次 Minor GC，年龄加1。当对象的年龄达到一定阈值(默认 15)时，虚拟机会将这个对象晋升到老年代。

垃圾回收的基本步骤分两步：
- 查找内存中不再使用的对象(GC判断策略)
- 释放这些对象占用的内存(GC收集算法)

**死亡对象/GC判断策略**
- 引用计数法：引用计数法是最简单的垃圾回收算法，它通过引用计数器来判断对象是否存活。当对象被引用时，计数器加1；当引用失效时，计数器减1。当计数器为0时，说明对象不再被引用，可以被回收。但引用计数法无法解决循环引用的问题，因此在实际应用中很少使用。
    - 循环引用：两对象无外界引用，但因互相引用对方，导致它们的引用计数器都不为 0，于是引用计数算法无法通知 GC 回收器回收他们。
- 可达性分析法：可达性分析法是现代垃圾回收算法的主流。它通过一系列的“GC Roots”对象作为起始点，从这些节点开始向下搜索，搜索过程所走过的路径称为引用链。当一个对象到 GC Roots 没有任何引用链相连时，则证明此对象是没有被引用，需要被回收。
    - 这些对象可以用作 GC Roots： 
      - 虚拟机栈(栈帧中的局部变量表)中引用的对象
      - 本地方法栈(Native 方法)中引用的对象
      - 方法区中类静态属性引用的对象
      - 方法区中常量引用的对象
      - 所有被同步锁持有的对象
      - JNI(Java Native Interface)引用的对象

> 对象被标记为不可达，就代表一定会被回收吗？
> 不一定。真正宣告一个对象死亡至少要经过两次标记过程。

**垃圾回收算法**
- 标记-清除算法：标记-清除算法是最基础的垃圾回收算法，分为标记和清除两个阶段。标记阶段遍历所有对象，标记出所有存活的对象；清除阶段清除所有未标记的对象。
  - 存在两个问题： 1. 效率问题：标记和清除两个过程效率都不高。 2. 空间问题：标记清除后会产生大量不连续的内存碎片，导致大对象无法分配内存。
- 复制算法：复制算法将内存分为两块，每次只使用其中一块。当这一块内存用完后，将存活的对象复制到另一块内存中，然后清除当前内存。这样可以避免内存碎片化问题。这种算法实现简单，运行高效，不会产生内存碎片。
  - 存在两个问题：1. 内存利用率低，只有一半的内存可以使用。2. 不适用于老年代，因为老年代存活对象较多，复制成本高。
- 标记-整理算法：标记-整理算法是根据老年代的特点提出的一种标记算法，它在标记阶段完成后，将存活的对象向一端移动，然后清除边界外的对象。这样可以避免内存碎片化问题。
  - 问题：整理的过程效率也不高。
- 分代收集算法：分代收集算法是目前主流的垃圾回收算法，它根据对象存活周期的不同将内存划分为不同的区域，每个区域采用适合的垃圾回收算法。一般将堆内存划分为新生代和老年代，新生代使用标记-复制算法，老年代使用标记-整理算法。

**垃圾回收器**
默认收集器：
JDK 8：Parallel Scavenge(新生代)+ Parallel Old(老年代)
JDK 9 ~ JDK20： G1(Garbage-First)收集器

- Serial 收集器：Serial 收集器是最基础的垃圾收集器，它是单线程的收集器，只会使用一个线程进行垃圾回收，在进行垃圾收集工作的时候必须暂停其他所有的工作线程( "Stop The World" )，直到它收集结束。。Serial 收集器适用于单核处理器和小内存的环境。
- ParNew 收集器：ParNew 收集器是 Serial 收集器的多线程版本，它可以使用多个线程进行垃圾回收。ParNew 收集器适用于多核处理器和多线程环境。
- Parallel Scavenge 收集器：Parallel Scavenge 收集器是一种以获取最大吞吐量为目标的收集器，它使用多线程进行垃圾回收，可以充分利用多核处理器的优势。Parallel Scavenge 收集器适用于对吞吐量要求较高的应用。
- Serial Old 收集器：Serial Old 收集器是 Serial 收集器的老年代版本，它使用单线程进行垃圾回收，适用于单核处理器和小内存的环境。
- Parallel Old 收集器：Parallel Old 收集器是 Parallel Scavenge 收集器的老年代版本，它使用多线程进行垃圾回收，适用于多核处理器和多线程环境。
- CMS 收集器：CMS 收集器是一种以获取最短回收停顿时间为目标的收集器，它使用多线程进行垃圾回收，可以显著减少垃圾回收的停顿时间。CMS 收集器适用于对停顿时间要求较高的应用。
- G1 收集器：G1 收集器是一种面向服务端应用的垃圾收集器，它将堆内存划分为多个区域，每个区域可以根据垃圾回收的需要进行独立回收。G1 收集器适用于大内存、多核处理器和对停顿时间要求较高的应用。
- ZGC 收集器：ZGC 收集器是一种低延迟的垃圾收集器，它可以在几毫秒内完成垃圾回收，适用于对停顿时间要求极高的应用。


> HotSpot VM 的实现，它里面的 GC 其实准确分类只有两大种：
> - 部分收集 (`Partial GC`)：
>     - 新生代收集(Minor GC / Young GC)：只对新生代进行垃圾收集；
>     - 老年代收集(Major GC / Old GC)：只对老年代进行垃圾收集。需要注意的是 Major GC 在有的语境中也用于指代整堆收集；
>     - 混合收集(Mixed GC)：对整个新生代和部分老年代进行垃圾收集。
> - 整堆收集 (Full GC)：收集整个 Java 堆和方法区。

### 为什么要进行垃圾回收
- **防止内存泄漏**：手动管理内存容易导致内存泄漏，而GC可以自动回收不再使用的对象，防止内存泄漏的发生。
- **提高开发效率**：程序员不再需要关心内存释放的问题，可以更加集中精力在业务逻辑的实现上。
- **系统性能和稳定性**：通过有效的垃圾回收策略，可以保证系统的性能和稳定性。

### 强引用/软引用/弱引用/虚引用✅
https://blog.csdn.net/u013718071/article/details/134789666
Java中的引用类型主要分为强引用、软引用、弱引用和虚引用，它们之间的区别主要体现在垃圾回收的行为上。
- 强引用(Strong Reference)：这是使用最普遍和默认的引用类型。如果一个对象具有强引用，那么垃圾回收器就永远不会回收它。当内存空间不足，Java虚拟机宁愿抛出 `OutOfMemoryError` 错误，也不会回收这种对象。
- 软引用(Soft Reference)：软引用是用来描述一些还有用但并非必需的对象。只有当JVM认为内存不足时，才会去剔除这些基于软引用的对象。在Java中，可以用 `SoftReference` 类来实现软引用。
- 弱引用(Weak Reference)：弱引用则是用来描述非必需对象的，但是它的强度比软引用更弱一些，被弱引用关联的对象只能生存到下一次垃圾回收发生为止。当垃圾回收器工作时，无论当前内存是否足够，都会回收掉只被弱引用关联的对象。在Java中，可以用 `WeakReference` 类来实现弱引用。
- 虚引用(Phantom Reference)：虚引用主要用来跟踪对象被垃圾回收的活动。虚引用与其他几种引用的一个区别在于：虚引用必须和引用队列(`ReferenceQueue`)联合使用。当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，将这个虚引用加入到与之关联的引用队列中。

以上四种引用类型，强度依次递减：强引用最强，虚引用最弱。在程序设计中一般很少使用弱引用与虚引用，使用软引用的情况较多，这是因为软引用可以加速 JVM 对垃圾内存的回收速度，可以维护系统的运行安全，防止内存溢出(OutOfMemory)等问题的产生。

### 如何判断一个类是无用的
类需要同时满足下面 3 个条件才能算是 “无用的类”：
- 该类所有的实例都已经被回收，也就是 Java 堆中不存在该类的任何实例。
- 加载该类的 `ClassLoader` 已经被回收。
- 该类对应的 `java.lang.Class` 对象没有在任何地方被引用，无法在任何地方通过反射访问该类的方法。

虚拟机可以(不是必然)对满足上述 3 个条件的无用类进行回收

### 类的生命周期
加载(Loading)、验证(Verification)、准备(Preparation)、解析(Resolution)、初始化(Initialization)、使用(Using)和卸载(Unloading)。

其中验证、准备、解析 3 部分统称为连接。

### 类加载过程
分三步：加载、连接(验证、准备、解析)、初始化。

**加载**
加载使用类加载器完成。类加载器有很多种，当想要加载一个类的时候，具体是哪个类加载器加载由 **双亲委派模型** 决定。加载主要分下面三步：
1. 通过全类名获取定义此类的二进制字节流。
2. 将字节流所代表的静态存储结构转换为方法区的运行时数据结构。
3. 在内存中生成一个代表该类的 `Class` 对象，作为方法区这些数据的访问入口。

> 加载和连接阶段的部分动作(如验证)是交叉进行的。加载还没结束，连接可能就已经开始了。

**验证**
连接的第一步，确保 `Class` 文件的字节流中包含的信息符合规范，并且不会危害虚拟机自身的安全。主要包括四个阶段：
1. 文件格式验证：验证字节流是否符合 Class 文件格式规范。
2. 元数据验证：对字节码描述的信息进行语义分析，以保证其符合 Java 语言规范。
   - 如验证类是否有父类
3. 字节码验证：通过数据流和控制流分析，确定程序语义是合法的、符合逻辑的。
   - 如验证函数的参数是否正确
4. 符号引用验证：确保解析动作能正确执行。
   - 如验证该类要使用的其他类、方法是否存在。

**准备**
连接的第二步，准备阶段是正式为类变量分配内存并设置类变量初始值的阶段，这些内存都将在方法区(Java 7 之前分配在永久代，Java 8 之后移动到堆中)中分配。
- 进行内存分配的仅包括类变量(被`static`修饰)而不包括实例变量。实例变量会在对象实例化时随着对象一块分配在 Java 堆中。
- 初始值是数据类型的默认值，如 0、null 等。

**解析**
解析阶段是虚拟机将常量池内的符号引用替换为直接引用的过程。主要包括类或接口、字段、类方法、接口方法、方法类型等。

**初始化**
初始化阶段是执行初始化方法 `<clinit>()`方法的过程，是类加载的最后一步，这一步 JVM 才开始真正执行类中定义的 Java 程序代码(字节码)。

### 类卸载
卸载类即该类的 `Class` 对象被 GC。

卸载类需要满足 3 个要求：
- 该类的所有的实例对象都已被 GC，也就是说堆不存在该类的实例对象。
- 该类没有在其他任何地方被引用
- 该类的类加载器的实例已被 GC

在 JVM 生命周期内，由 JVM 自带的类加载器加载的类(如`BootstrapClassLoader`, `ExtClassLoader`, `AppClassLoader` )是不会被卸载的。但是由自定义的类加载器加载的类是可能被卸载的。

### 类加载器
类加载器是一个负责加载类的对象。每个 Java 类都有一个引用指向加载它的 `ClassLoader`。数组类不是通过 `ClassLoader` 创建的(数组类没有对应的二进制字节流)，是由 JVM 直接生成的。

> 类加载器的主要作用就是加载 Java 类的字节码( .class 文件)到 JVM 中(在内存中生成一个代表该类的 Class 对象)。

JVM内置三个重要的类加载器：
`BootstrapClassLoader`(启动类加载器)：最顶层的加载类，由 C++实现，通常表示为 `null`，并且没有父级，主要用来加载 JDK 内部的核心类库。
`ExtensionClassLoader`(扩展类加载器)：主要负责加载 `%JRE_HOME%/lib/ext` 目录下的 jar 包和类以及被 `java.ext.dirs` 系统变量所指定的路径下的所有类。
`AppClassLoader`(应用程序类加载器)：面向用户的加载器，负责加载当前应用 `classpath` 下的所有 jar 包和类。

### 双亲委派模型
双亲委派模型是一种类加载器的层次结构，每个类加载器都有一个父类加载器。当一个类加载器收到加载类的请求时，它会先将请求委派给父类加载器，直到最顶层的类加载器。只有当父类加载器无法加载该类时，子类加载器才会尝试加载。

**优点**
双亲委派模型保证了 Java 程序的稳定运行，可以避免类的重复加载(JVM 区分不同类的方式不仅仅根据类名，相同的类文件被不同的类加载器加载产生的是两个不同的类)，也保证了 Java 的核心 API 不被篡改。

如果需要打破双亲委派模型，需要自定义类加载器，并重写 `loadClass` 方法。



