*Alamofire (~> 4.5)*

**Alamofire.swift** 主要做的事情：

- 定义了 `URLConvertible` protocol, 使 String、URL、URLComponents 遵循了 URLConvertible protocol.
- 定义了 `URLRequestConvertible` protocol, 使 `URLRequest` 遵循了 URLRequestConvertible protocol.
- 给 URLRequest 新增了 `init` `adapt` 方法。
- 实现了全局的 `request` `download` `upload` `stream` 方法，这一系列方法都是调用 SessionManager 中的方法来完成的。

**SessionManager.swift** 主要做的事情：

- 定义了 `SessionManager` class, 该类是个单例，它的主要功能是实现了 `request` `download` `upload` `stream` 请求方法。
- Request 的保存和数据回调是通过 `SessionDelegate` 来完成的。

**SessionDelegate.swift** 主要做的事情：

定义了 `SessionDelegate` class,该类的主要功能有

- 保存所有正在进行中的 Request
- 实现 `URLSessionDelegate` `URLSessionTaskDelegate` `URLSessionDataDelegate` `URLSessionDownloadDelegate` `URLSessionStreamDelegate` 代理方法，并将相应的事件传递给对应 Request 的 delegate，也就是 TaskDelegate。

**Request.swift** 主要的功能

- 定义了 `RequestAdapter ` `RequestRetrier` `TaskConvertible` protocol.
- 定义了 `Request` `DataRequest` `DownloadRequest` `UploadRequest` `StreamRequest`，用来保存不同模式的请求数据。

**TaskDelegate.swift** 主要做的事情：

- 定义了 `TaskDelegate` `DataTaskDelegate` `DownloadTaskDelegate` `UploadTaskDelegate` class.
- 保存和处理 server 返回的 data、error 
- 保存请求完成后要执行的操作。

Validation.swift

`ValidationResult enum`
`MIMEType struct`

ParameterEncoding.swift

`HTTPMethod enum`
`ParameterEncoding protocol`
`URLEncoding struct`
`JSONEncoding struct`
`PropertyListEncoding struct`

Response.swift

`DefaultDataResponse struct`
`DataResponse struct`
`DefaultDownloadResponse struct`
`DownloadResponse struct`
`Response protocol`

ResponseSerialization.swift

`DataResponseSerializerProtocol protocol`
`DataResponseSerializer struct`

`DownloadResponseSerializerProtocol protocol`
`DownloadResponseSerializer struct`



Result.swift

`Result enum`

Timeline.swift

`Timeline struct`

Notifications.swift

`Task extension Notification.Name`
`Key extension Notification`

AFError.swift

`AFError enum`
`AdaptError struct`

DispatchQueue+Alamofire.swift

`extension DispatchQueue`

MultipartFormData.swift

`MultipartFormData class`

NetworkReachabilityManager.swift

`NetworkReachabilityManager class`

ServerTrustPolicy.swift

`ServerTrustPolicyManager class`

