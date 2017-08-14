#### 如何理解 Objective-C 的消息传递和转发机制？

当我们调用对象的方法时，就是向这个对象传递消息，因为要调用的函数直到运行期才能确定，所以会使用动态绑定机制来决定需要调用的方法。编译器会将消息（receiver + selector + 参数）转换为标准的 C 语言调用（objc_msgSend）, objc_msgSend 会在 receiver 所属的类中搜索方法列表，如果能找到与 selector 名称相符的方法，就跳转到其实现代码。如果找不到，就沿着继承体系继续向上寻找，找到合适的方法再跳转。如果最终找不到相符的方法。则会执行 *消息转发* 操作。

*当对象接收到无法解读的消息后，就会启动消息转发机制（message forwarding ）。*

消息转发第一阶段会先征询 receiver 所属的类，看其能否能动态添加方法来处理这个 unknow selector，也就是 动态方法解析（dynamic method resolution）。

第二阶段会让 receiver 查阅是否有其他对象能处理这条消息，如果有，则把消息转给那个对象，消息转发过程到此结束。如果没有，则把与消息的全部细节封装到 NSInvocation 对象中，再给 receiver 一次机会，让其解决当前还未处理的消息。



详情参考：Effective Objective-C 2.0

第 11 条：理解 objc_msgSend 的作用

第 12 条：理解消息转发机制




