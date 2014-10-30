## policy.json
配置策略。
每次进行 API 调用时，会采取对应的检查，policy.json 文件发生更新后会立即生效。
该文件在开头定义
```
"context_is_admin":  "role:admin",
"deny_stack_user": "not role:heat_stack_user",
"deny_everybody": "!",
```
目前支持的策略有三种：rule、role 或者 generic。
其中 rule 后面会跟一个文件名，例如
```
"cloudformation:ListStacks": "rule:deny_stack_user",
```
其策略为 rule:deny_stack_user，表明仅限 heat_stack_user 角色使用。
role 策略后面会跟一个 role 名称，表明只有指定 role 才可以执行。
generic 策略则根据参数来进行比较。
