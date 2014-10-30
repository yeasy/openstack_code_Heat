## engine
### cfn
#### functions.py
一些继承自 function.Function 的类，代表模板中的各种方法。

#### template.py
定义 CfnTemplate 类，代表一个 cfn 的 stack 模板。

### hot

#### functions.py
定义模板中函数相关的解析方法。

#### parameters.py
定义 HOTParameters 类和 HOTParamSchema 类，解析模板中的参数。

#### template.py
定义 HOTemplate，代表一个模板。

### clients

#### os
包括对 OpenStack 中各种服务 client 的插件，例如 NeutronClientPlugin、NovaClientPlugin 等。

#### __init__.py
定义了 OpenStackClients，来创建和使用各种 client。

#### client_plugin.py
定义 ClientPlugin 类，抽象基础元类，包含各种异常类。

### notification

#### __init__.py

#### autoscaling.py
定义 send 方法。

#### stack.py
定义 send 方法。

### resources
这里面定义了对应 template 中各种资源的具体类，一般都是继承自 resource.Resource 类。这些类在最后会定义一个 resource_mapping 方法，返回 template 的某个关键词和对应类的映射。
例如 instance 模块中定义：
```
def resource_mapping():
    return {
        'AWS::EC2::Instance': Instance,
        'OS::Heat::HARestarter': Restarter,
    }
```
同时，每个资源都定义了一套标准的 Action（例如创建、更新、删除等），均以 handle_ 为前缀。

#### __init__.py
在导入包的时候，会创建 plugin_manager 并从所有资源中导入可用的映射信息。
其中，定义的 initialise 方法在 heat-engine 服务启动的时候会被调用。
initialise 方法同时调用了 _load_global_environment 方法，该方法一个是调用 _load_global_resources() 来加载和注册资源，一个是调用 read_global_environment() 来从指定的环境配置目录下查找环境配置文件来载入环境配置信息。
```
def _load_global_environment(env):
    _load_global_resources(env)
    environment.read_global_environment(env)
```
_load_global_resources() 方法则负责加载和注册资源以及限制信息，这些信息都被添加到全局的 _environment 环境字典中。
```
def _load_global_resources(env):
    _register_constraints(env, _get_mapping('heat.constraints'))

    manager = plugin_manager.PluginManager(__name__)
    # Sometimes resources should not be available for registration in Heat due
    # to unsatisfied dependencies. We look first for the function
    # 'available_resource_mapping', which should return the filtered resources.
    # If it is not found, we look for the legacy 'resource_mapping'.
    resource_mapping = plugin_manager.PluginMapping(['available_resource',
                                                     'resource'])
    constraint_mapping = plugin_manager.PluginMapping('constraint')

    _register_resources(env, resource_mapping.load_all(manager))

    _register_constraints(env, constraint_mapping.load_all(manager))
```
首先创建一个 pluginmanager，负责管理所有的资源插件，注意参数为当前包名（即heat.engine.resources），会在配置文件中指定的目录（默认为 /usr/lib/heat 和 /usr/lib64/heat）和当前包去查找所有模块。
之后定义了两个 mapping，这两个 mapping 分别在给定的模块中查找 available_resource_mapping() 方法、resource_mapping() 方法和 constraint_mapping() 方法，之后通过 load_all() 方法来在所有的 pluginmanager 管理的模块中调用查找到的方法，并将结果添加到环境字典中。这些方法会返回一个字典，包括资源名到对应类的映射信息，例如在 route_table.py 资源文件中代码为
```
def resource_mapping():
    return {
        'AWS::EC2::RouteTable': RouteTable,
        'AWS::EC2::SubnetRouteTableAssociation': SubnetRouteTableAssociation,
    }
```

#### ceilometer
定义对 Alarm 和 CombinationAlarm 两种资源的处理。

#### neutron
定义对防火墙、floatingip、负载均衡、metering、net、gateway、port、router、subnet、vpn等网络相关的资源的处理。
以 Port 为例，定义了 Port 类，通过resource_mapping指定 OS::Neutron::Port 资源映射到 Port 类上。
Port 类中定义 properties_schema 和 attributes_schema。并定义对 CUD 操作的对应方法 handle_create()、handle_delete()、handle_update()。

#### software_config

#### autoscaling.py
处理 autoscaling 相关关键字。

#### cloud_watch.py
处理 OS::Heat::CWLiteAlarm 关键字。

#### eip.py
处理 AWS 中 EIP 相关关键字。

#### glance_image.py
处理 glance image 关键字。

#### instance.py
处理 AWS::EC2::Instance 和 OS::Heat::HARestarter 关键字。

#### internet_gateway.py
处理 AWS::EC2::InternetGateway和AWS::EC2::VPCGatewayAttachment 关键字。

#### loadbalancer.py
处理 AWS::ElasticLoadBalancing::LoadBalancer 关键字。

#### network_interface.py
处理 AWS::EC2::NetworkInterface 关键字。

#### nova_floatingip.py
处理 floatingIP 相关关键字。

#### nova_keypair.py
处理 OS::Nova::KeyPair 关键字。

#### nova_servergroup.py
处理 OS::Nova::ServerGroup 关键字。

#### os_database.py
处理 OS::Trove::Instance 关键字。

#### random_string.py
处理 OS::Heat::RandomString 关键字。

#### resource_group.py
处理 OS::Heat::ResourceGroup 关键字。

#### route_table.py
处理路由表相关关键字。

#### s3.py
处理 AWS::S3::Bucket 关键字。

#### security_group.py
处理 AWS::EC2::SecurityGroup 关键字。

#### server.py
处理 OS::Nova::Server 关键字。

#### stack.py
处理 AWS::CloudFormation::Stack 关键字。

#### subnet.py
处理 AWS::EC2::Subnet 关键字。

#### swift.py
处理 OS::Swift::Container 关键字。

#### user.py
处理 AWS::IAM::User、AWS::IAM::AccessKey 和 OS::Heat::AccessPolicy 关键字。

#### volume.py
处理 AWS::EC2::Volume、AWS::EC2::VolumeAttachment、OS::Cinder::Volume和OS::Cinder::VolumeAttachment 关键字。

#### vpc.py
处理 AWS::EC2::VPC 关键字。

#### wait_condition.py

#### iso_8601.py
定义 ISO8601Constraint 类，解析 iso8601 标准的时间为普通时间对象。

#### nova_utils.py
一些辅助处理函数，包括更新 meta、刷新等。

#### template_resource.py
定义 TemplateResource 类，代表嵌套 stack 资源。

### api.py
定义一些格式化的方法，包括对 stack、资源属性等进行格式化。让它们符合 API 输出格式。

### attributes.py
代表资源的 attribute。定义类 Attribute、Attributes和Schema。

### constraints.py
一些限制类和 schema，例如处理允许的 pattern 等。

### dependencies.py
用图模型来处理资源的依赖关系。包括 Node、Graph、Dependencies 等类，描述不同资源之间的关系。

### environment.py
定义资源信息类 ResourceInfo 和环境类 Environment 等。
所有的资源注册信息和映射关系等都被 Environment 类来统一管理。

### event.py
定义 Event 类，代表资源的状态发生改变。

### function.py
定义 Function 类，是模板中函数的抽象基础类。

### parameter_groups.py
定义 ParameterGroups 类，处理模板中的参数组。

### parameters.py
定义 Parameter、NumberParam、BooleanParam、StringParam 等类，处理模板中的参数。

### parser.py
指定 Template为template.Template，Stack为stack.Stack 类。

### plugin_manager.py
对 plugin 进行管理。
定义 PluginManager 类和 PluginMapping 类。
前者会导入所有的插件。在初始化的时候根据传入参数以及配置文件 heat.conf 中 plugin_dirs 指定的目录（默认是/usr/lib64/heat,/usr/lib/heat）去进行扫描所有模块并导入。
后者定义方法去扫描 PluginManager 中加载的模块，并根据指定感兴趣的mapping方法名称，例如 resource_mapping、available_resource_mapping、constraint_resource_mapping 等来导入相关的方法。
```
def load_from_module(self, module):
        '''Return the mapping specified in the given module.

        If no such mapping is specified, an empty dictionary is returned.
        '''
        for mapping_name in self.names:
            mapping_func = getattr(module, mapping_name, None)
            if callable(mapping_func):
                fmt_data = {'mapping_name': mapping_name, 'module': module}
                try:
                    mapping_dict = mapping_func(*self.args, **self.kwargs)
                except Exception:
                    LOG.error(_('Failed to load %(mapping_name)s '
                                'from %(module)s') % fmt_data)
                    raise
                else:
                    if isinstance(mapping_dict, collections.Mapping):
                        return mapping_dict
                    elif mapping_dict is not None:
                        LOG.error(_('Invalid type for %(mapping_name)s '
                                    'from %(module)s') % fmt_data)
        return {}
```

### properties.py
定义 Property 类，处理模板中的 property。

### resource.py
定义 Resource 基础类，是其他资源类的基类。
资源的属性包括行动、状态等，另外定义 default_client_name，用来向 openstack 中的服务发起操作指令。

### rsrc_defn.py
定义 ResourceDefinition 类和 ResourceDefinitionCore 类。

### scheduler.py
实现资源调度。对资源的操作（例如创建）是作为一个调度任务来管理的。
定义了 TaskRunner 等来实现调度任务管理。

### service.py
定义了若干很重要的服务类，首先是 EngineListener，继承自 service.Service，监听 AMQP 队列。
EngineService 服务类，继承自 service.Service，bin/heat-engine 脚本启动的服务就是这个类。heatclient 发出的命令经过 heat-api，最终会以 rpc 请求消息的方式调用本类中的方法。包括对 stack、software_config 等资源的 create、delete 等。该服务启动后会创建一个 EngineListener 来监听消息。
EngineService 服务类在初始化的时候通过调用 resources.initialise() 来完成资源的注册，包括各种关键字对应的映射放到全局环境字典中。
以 stack create 操作为例，对应方法为 EngineService 服务类中的 create_stack() 方法。该方法首先通过 _parse_template_and_validate_stack() 方法简单解析 stack 对应的模板类是哪种。

### signal_responder.py
定义 SignalResponder 类，继承自 stack_user.StackUser，响应 create、delete 方法，清除数据等。

### stack.py
定义了 Stack 类，继承自 collections.Mapping，代表 Heat template 中的一个 stack 对象，支持创建、删除、更新、回滚、暂停、继续、快照等操作。

### stack_lock.py
定义StackLock类，用于维护一个stack资源上的锁。

### stack_resource.py
定义了 StackResource 类，继承自 resource.Resource。抽象类，允许父级 stack 像管理一个资源一样来管理一个子 Stack。

### stack_user.py
定义了 StackUser 类，继承自 resource.Resource。
创建一个关联 stack 的 user。

### support.py
定义 SupportStatus，表示支持状态。

### template.py
定义 Template 类和 TemplatePluginManager类，管理模板。

### timestamp.py
定义 Timestamp 类。管理时间戳。

### update.py
定义 StackUpdate 类，升级一个存在的 stack 到一个新的模板。

### watchrule.py
定义 WatchRule 类。
