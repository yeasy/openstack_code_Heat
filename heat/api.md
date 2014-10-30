## api
因为 Heat 面向的操作对象是可扩展的，包括 openstack 资源、aws 等等，所以 api 也分为不同的类型，以备后面仅限扩展。
heat-api 以服务形式存在，接受 heatclient 传递过来的 rest 请求，从中读取模板信息，重新整理请求后以 rpc 的方式发送给 heat-engine。

### aws
#### ec2token.py
定义EC2Token类，利用keystone来验证一个EC2请求，并将它转化为token。
#### exception.py
定义处理api的异常类。
#### utils.py
一些辅助方法，包括提取参数等。
### cfn
#### v1
##### signal.py
定义SignalController类。
###### stacks.py
定义StackController类，作为stack资源的WSGI控制器。
#### versions.py
返回版本信息的控制器类。
### cloudwatch
#### __init__.py
定义了继承自wsgi.Router的API类。
#### versions.py
定义返回版本信息的控制器类。
#### watch.py
定义WatchController类，包含有一个EngineClient句柄，发送请求给heat-engine。
### middleware
#### fault.py
定义了Fault类和FaultWrapper类。

#### ssl.py
定义了SSLMiddleware类。

#### version_negotiation.py
定义了VersionNegotiationFilter类。

### openstack
#### v1
##### views
定义了格式化输出的一些方法。
##### __init__.py
里面定义了一个很重要的类：API，这个类直接接收 restful 的 api 调用，它里面带有一个StackController类句柄，将不同的 url 请求映射为对应资源的方法。例如
```
stacks_resource = stacks.create_resource(conf)
with mapper.submapper(controller=stacks_resource,
                              path_prefix="/{tenant_id}") as stack_mapper:
stack_mapper.connect("template_validate",
                                 "/validate",
                                 action="validate_template",
                                 conditions={'method': 'POST'})
```
会将url到/tenant_id/validate的POST请求交给stacks_resource对象的validate_template方法处理，即调用stacks.StackController类的validate_template方法。
该类中分别定义了对Stack资源、Resource资源、Event资源、Action资源、Info资源、Software config资源、Software deployment资源的相关http 请求的处理，分别映射到该包中对应模块下的控制器上。
这些控制器根据收到的请求调用自身的对应方法，这些方法调用自身的engineclient（调用 messaging 中的 rpc client）来发出 rpc 请求调用 engine 中的方法，主题为 api.ENGINE_TOPIC。

##### actions.py
定义ActionController类，响应对Action资源的请求，调用自身的engineclient进行stack的暂停和继续操作。

##### build_info.py
定义 BuildInfoController 类，响应对 BuildInfo 资源的请求，调用自身的 engineclient 进行获取 buildinfo 操作。

##### events.py
定义 EventController类，响应对 Event 资源的请求，调用自身的 engineclient 进行列出和获取操作。

##### resources.py
##### software_configs.py
##### stacks.py
##### util.py
#### versions.py
响应对 api 版本请求，返回 http response。
