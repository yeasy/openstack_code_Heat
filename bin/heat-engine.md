## heat-engine
启动 heat-engine 服务，其过程如下
```
cfg.CONF(project='heat', prog='heat-engine')
logging.setup('heat')
messaging.setup()

from heat.engine import service as engine

srv = engine.EngineService(cfg.CONF.host, rpc_api.ENGINE_TOPIC)
launcher = service.launch(srv, workers=cfg.CONF.num_engine_workers)
# We create the periodic tasks here, which mean they are created
# only in the parent process when num_engine_workers>1 is specified
srv.create_periodic_tasks()
notify.startup_notify(cfg.CONF.onready)
launcher.wait()
```

其主要通过调用 engine.EngineService 类来实现应用，用 service 包来管理服务。
heat-engine 作为 heat-api 的后端，是 heat 中最核心的组件。
