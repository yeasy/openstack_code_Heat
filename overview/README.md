# 整体结构
源代码主要分为 6 个目录和若干文件：
bin，contrib，doc，etc，neutron和tools。

除了这 6 个目录外，还包括一些说明文档、安装需求说明文件等。

## bin
主要包括 heat-api、heat-api-cfn、heat-api-cloudwatch、heat-engine、heat-manage。
这些是相应服务的主文件。

##	contrib
包括若干第三方的插件，包括 docker、extraroute、keystoneclient_v2、marconi和rackspace 等。

##	doc
包括文档生成的相关源码。

##	etc
跟服务和配置相关的文件，基本上该目录中内容在安装时会被复制到系统的 /etc/ 目录下。
* environment.d/default.yaml 文件中可以预定义一些环境参数。
* templates 目录下有两个 yaml 格式的模板文件。
* api-paste.ini，启动 heat-api 服务时候的 paste-deploy 配置文件。

##	heat
核心的代码实现都在这个目录下。
可以通过下面的命令来统计主要实现的核心代码量。
```
find heat -name "*.py" | xargs cat | wc -l
```
目前版本，约为 107k 行。

##	tools
一些相关的代码格式化检测、环境安装的脚本。
