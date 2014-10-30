# common
### auth_password.py
定义 KeystonePasswordAuthProtocol 类，提供通过用户名密码来进行认证的方法。

### auth_url.py
定义 AuthUrlFilter类。

### config.py
定义配置文件中使用的配置项关键字和默认值。以及 load_paste_app() 等方法。

### context.py
定义 RequestContext 类，请求的上下文信息。

### crypt.py
加密和解密的相关方法。

### custom_backend_auth.py
定义 AuthProtocol 类。

### environment_format.py
对环境字符串进行解析。

### exception.py
各种异常类。

### heat_keystoneclient.py
定义 KeystoneClient 类和 KeystoneClientV3 类。封装 keystoneclient，以供资源使用。

### identifier.py
定义 HeatIdentifier 类。

### messaging.py
定义一些消息的序列化处理。

### param_utils.py
解析参数辅助函数。

### plugin_loader.py
定义动态从指定包加载所有模块的方法 load_modules。

### policy.py
定义 Enforcer 类，加载和实施规则。

### serializers.py
json 和 xml 回复的序列化类。

### short_id.py
生成 id 方法。

### template_format.py
一些模板格式化的方法。

### timeutils.py
对时间进行处理的方法，包括转化为ISo 8601格式等。

### urlfetch.py
从 URL 中获取数据的方法。

### wsgi.py
wsgi 相关的封装类，包括请求、路由器、服务器等。
