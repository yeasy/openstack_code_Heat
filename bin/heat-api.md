## heat-api

启动 heat-api 服务的主文件。

主要代码为
```
cfg.CONF(project='heat', prog='heat-api')
cfg.CONF.default_log_levels = ['amqplib=WARN',
                              'qpid.messaging=INFO',
                              'keystone=INFO',
                              'eventlet.wsgi.server=WARN',
                              ]
logging.setup('heat')

app = config.load_paste_app()

port = cfg.CONF.heat_api.bind_port
host = cfg.CONF.heat_api.bind_host
LOG.info('Starting Heat ReST API on %s:%s' % (host, port))
server = wsgi.Server()
server.start(app, cfg.CONF.heat_api, default_port=port)
notify.startup_notify(cfg.CONF.onready)
server.wait()
```
可见，其主要过程为，从 api-paste.ini 文件中读取应用并加载，之后启动一个 wsgi 服务，上面运行配置好的应用，并根据配置文件指定，发出启动完成的通知。
