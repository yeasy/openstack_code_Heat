OpenStack Heat 源码分析
============
[Heat](https://wiki.openstack.org/wiki/Heat) 是 OpenStack 项目中负责部署、协调资源的平台工具，它允许用户提前写好一个模板文件，然后通过 Heat 引擎来启动、停止所定义的资源。

本书将剖析 Heat 组件（服务端）的代码。

最新版本在线阅读：[GitBook](https://www.gitbook.io/book/yeasy/openstack_code_Heat)。

本书源码在 Github 上维护，欢迎参与： [https://github.com/yeasy/openstack_code_Heat](https://github.com/yeasy/openstack_code_Heat)。

感谢所有的 [贡献者](https://github.com/yeasy/openstack_code_Heat/graphs/contributors)。

## 更新历史:
* 0.2: 2014-08-18
    * 完成对整体代码的分析。
* 0.1: 2014-08-09
	* 完成代码基本结构分析。

## 参加步骤
* 在 GitHub 上 `fork` 到自己的仓库，如 `user/openstack_code_Heat`，然后 `clone` 到本地，并设置用户信息。
```
$ git clone git@github.com:user/openstack_code_Heat.git
$ cd openstack_code_Heat
$ git config user.name "User"
$ git config user.email user@email.com
```

* 修改代码后提交，并推送到自己的仓库。
```
$ #do some change on the content
$ git commit -am "Fix issue #1: change helo to hello"
$ git push
```

* 在 GitHub 网站上提交 pull request。
* 定期使用项目仓库内容更新自己仓库内容。
```
$ git remote add upstream https://github.com/yeasy/openstack_code_Heat
$ git fetch upstream
$ git checkout master
$ git rebase upstream/master
$ git push -f origin master
```
