[https://github.com/childe/gohangout](https://github.com/childe/gohangout) 插件示例.

Golang 的 Plugin 文档参考 [https://tip.golang.org/pkg/plugin/](https://tip.golang.org/pkg/plugin/)

## 编译

```shell
go build -buildmode=plugin -o title.so title.go
```

## gohangout 配置示例 

```yaml
inputs:
    - Stdin: {}

filters:
    - title.so:
        fields: [message]

outputs:
    - Stdout: {}
```

## 代码说明

### New

New 函数的定义一定是这样的 `New(config map[interface{}]interface{}) filter.Filter` . 在 Gohangout 里面会调用 plugin 的 New 方法来生成一个 Filter 实例.

### Filter

Filter 定义如下 `Filter(event map[string]interface{}) (map[string]interface{}, bool)` , 实现 Gohangout 里面的 Filter 接口

### Gohangout 里面的调用

```go
		p, err := plugin.Open(filterType)  # filterType is "title.so" here
		
		new, err := p.Lookup("New")

		return new.(func(map[interface{}]interface{}) Filter)(config)
```

