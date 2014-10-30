## openstack
6.9.1	Common
含有 openstack 提供的基础服务类。openstack-common 项目负责维护这些代码，为所有的 openstack 子项目提供方法支持。
#### Config
还有 generator.py 文件，可以根据指定的代码文件来提取配置项，生成样例配置文件。

#### Crypto
跟加密相关的辅助方法。

#### Middleware
实现 WSGI 的 middleware 层。

#### Context.py
定义处理 http 请求上下文的 ReuqestContext 类。

#### eventlet_backdoor.py
实现 eventlet 的后门，监听。

#### excutils.py
定义处理异常类 save_and_reraise_exception，可以保存当前异常，运行一些代码，然后再次抛出异常。

#### fileutils.py
定义一些对文件进行操作的辅助方法。

#### gettextutils.py
语言国际化。

#### importutils.py
根据输入参数来动态导入对象、类等等。

#### jsonutils.py
对 json 内容进行处理。

#### local.py
弱本地引用类 WeakLocal。

#### lockutils.py
处理锁相关操作。

#### log.py
log 相关操作。

#### loopingcall.py
周期性调用类，包括 DynamicLoopingCall、FixedIntervalLoopingCall、LoopingCallBase。

#### network_utils.py
一些网络配置相关的辅助方法，包括从字符串解析主机名和端口、解析url等。

#### policy.py
对策略的处理。

#### processutils.py
管理进程。

#### service.py
实现服务基础类 Service、Services 等。

#### sslutils.py
定义使用 ssl 的辅助方法。

#### strutils.py
对字符串进行解码、编码等操作。

#### systemd.py
对 systemd 进行管理的方法。

#### threadgroup.py
定义 thread 类和 threadgroup 类，对线程进行管理。

#### timeutils.py
对时间相关的字符串进行处理等。

#### uuidutils.py
生成和检查 uuid。

#### versionutils.py
检查版本兼容信息。
