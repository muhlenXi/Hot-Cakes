### 图解 HTTP

#### 相关概念

TCP/IP 协议族按层次分别分为以下4层：应用层、传输层、网络层和数据链路层。

##### 状态码的类别

|               | 类别           | 原因短语  |
| ------------- |:-------------:| -----:|
| 1XX | Informational（信息性状态码） | 接收的请求正在处理 |
| 2XX | Success（成功状态码）| 请求正常处理完毕 |
| 3XX | Redirection（重定向状态码） | 需要进行附加操作以完成请求 |
| 4XX | Client Error （客户端错误状态码） |  服务器无法处理请求 |
| 5XX | Server Error（服务器错误状态码） | 服务器处理请求出错 |

常用状态码：

200 OK ，请求已正常处理。

204 No Content，请求处理成功，但没有资源返回。

206 Partial Content，资源部分请求。

301 Moved Permanently，永久性重定向。

302 Found，临时性重定向。

303 See Other， 请求资源存在另一个 URI，应使用 GET 定向获取请求的资源。

304 Not Modified，资源已找到，但未符合条件请求。

307 Temporary Redirect，临时重定向。

400 Bad Request，请求报文中存在语法错误。

401 Unauthorized，用户认证失败。

403 Forbidden，请求资源的访问被服务器拒绝了。

404 Not Found，服务器上无法找到请求的资源。

500 Internal Server Error，服务器发生错误。

503 Service Unavailable，服务器暂时处于超负载或正在停机维护。

#### 名词释义

`WWW`：*World Wide Web, 万维网。*

`SGML`：*Standard Generalized Markup Language, 标准通用标记语言。*

`HTML`：*HyperText Markup Language, 超文本标记语言。*

`URL`：*Uniform Resource Locator, 统一资源定位符。*

`NSCA`：*National Center for Supercomputer Applications, 美国国家超级计算机应用中心。*

`FTP`：*File Transfer Protocol, 文件传输协议。*

`DNS`：*Domain Name System, 域名系统。*

`TCP`：*Transmission Control Protocol, 传输控制协议。*

`UDP`：*User Data Protocol, 用户数据报协议。*

`NIC`：*Network Interface Card, 网络适配器，即网卡。*

`IP`：*Internet Protocol, 网际协议。*

`MAC`：*Media Access Control Address, MAC 地址。*

`ARP`：*Address Resolution Protocol, 地址解析协议。*根据通信方的 IP 地址就可以反查出对应的 MAC 地址。

`BSS`：*Byte Stream Service, 字节流服务。*

`SYN`：*synchronize, 。*

`ACK`：*acknowledge, 。*

`URI`：*Uniform Resource Identifier, 统一资源标识符。*

`ICANN`：*Internet Corporation for Assigned Names and Numbers, 互联网名称与数字地址分配机构。*

`IANA`：*Internet Assigned Numbers Authority, 互联网号码分配局。*

`CGI`：*Common Gateway Interface, 通用网关接口。*

`REST`：*REpresentional State Transfer, 表征状态转移。*

`CST`：*Cross-Site Tracing, 跨站追踪。*

`SSL`：*Secure Socket Layer, 安全套接层。*

`Transport Layer Security`：*, 传输层安全。*

`RFC`：*Request for Comments, 征求修正意见书。*

`CR`：*Carriage Return, 回车符。*

`LF`：*Line Feed, 换行符。*

`MIME`：*Multipurpose Internet Mail Extensions, 多用途因特网邮件扩展。*

`Web-DAV`：*Web-based Distributed Authoring and Versioning, 基于万维网的分布式创作和版本控制。*

`CRC`：*Cyclic Redundancy Check, 循环冗余验证。*

`DNT`：*Do Not Track, 拒绝个人信息被收集。*

`P3P`：*The Platform for Privacy Preferences, 在线隐私偏好平台。*

`IETF`：*Internet Engineering Task Force, Internet 工程任务组。*

`SNS`：*Social Networking Service, 社交网络服务。*

`Ajax`：*Asynchronous JavaScript and XML, 异步 JavaScript 与 XML 技术。*

`DOM`：*Document Object Model, 文档对象模型。*

`W3C`：*World Wide Web Consortium, 。*

`CSS`：*Cascading Style Sheets, 层叠样式表。*

`XML`：*eXtensible Markup Language, 可扩展标记语言。*

`JSON`：*JavaScript Object Notation, 一种以 JavaScript 的对象表示法为基础的轻量级数据标记语言。*

`RDBMS`：*Relational DataBase Management System, 关系型数据库管理系统。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*
``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

``：*, 。*

up to 32 pages

