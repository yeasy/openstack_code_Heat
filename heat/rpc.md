# rpc
### api.py
定义了一些常量，包括 ENGINE_TOPIC、STACK_KEYTS 等。

### client.py
定义了 EngineClient 类，该类可以利用 rpc 跟 heat-engine 进行通信完成各种资源操作。对资源进行操作的各个控制器实际上就是调用本类中的方法。
支持利用 call 和 cast 方法发出各种消息来调用方法，包括对 stack 的 show、count 等。
