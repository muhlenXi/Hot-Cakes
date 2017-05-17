### Concept

`Observable` ：可以理解为 “以时间为索引的常量队列”，或者理解为 “事件流”。Observable 中的每一个元素都可以理解为 “一个异步发生的事件”。

`Operator` : 用于生成 Observable 或者对 Observable 加工处理。

`Observer` : 可以理解为 “订阅者”，是 subscribe Observable 的对象。

`DisposeBag` : 用于存放一些 Observer ，当 DisposeBag 销毁时，DisposeBag 中的 Observer 会自动取消订阅，订阅事件对应的 Observable 资源就被回收了。

`Subject` : 可以理解为具有双重身份的 Observable，一方面，它是一个 Observer（订阅者）来获取系统事件的通知，另一方面，它作为 Observable  供其他对象来订阅。

* `PulishSubject` 只会把消息发送给事件发生之前的订阅者。
* `BehaviorSubject` 自带默认消息，只要有订阅者产生，就会给订阅者发一个最近的消息。
* `ReplaySubject` 没有默认消息，自带缓存区，当有订阅产生时，就会给订阅者发送缓存区中的消息。
* `Variable` 响应式的，订阅是需要先调用 `asObservable()` 方法。当给 Variable 设置新值时，需要访问其 value 属性。 


