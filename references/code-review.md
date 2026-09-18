# Go 代码审查要点

结合本规范与 Go 官方、Google、Uber 风格指南。冲突时以本规范为准。

## 可读性与命名

- 标识符清晰、简短，统一采用驼峰命名，禁止用下划线作标识符分隔；导出名称有中文块注释。
- 变量名、函数名、参数名和类型名至少 4 个字符，必须有明确业务含义；接收器名和 `err`、`ctx` 等惯用缩写除外。
- 避免缩写歧义；包名和导出名组合后自然可读。
- 避免公开结构体嵌入外部类型，暴露无关字段。
- 缩写词大小写一致，例如 `URL`、`ID`。
- 接收器名用类型缩写，不使用 `me`、`this`、`self`。
- 避免内置名称和裸参数。

## 格式与导入

- 已运行 `gofmt`，导入已用 `goimports` 整理。
- 导入分组清晰，标准库在前，外部依赖在后。
- 无冲突导入别名、无点导入；空白导入仅在确有副作用时使用。

## 正确性

- 验证接口实现是否显式断言。
- 边界处拷贝切片和 map。
- 明确类型断言失败必须显式处理。
- 错误只处理一次：要么记录并返回，要么只返回。


## 测试与验证

- 修改源码必须同步新增或更新测试，并运行 `go test ./...`。
- 测试只依赖临时目录和独立数据，不触碰生产数据或内容资产。
- 优先使用表驱动测试和子测试。
- 失败信息包含函数、输入、期望 `got` 与实际 `want`。
- 检查测试是否验证业务行为，而不是只匹配输出文本。
- 优先用标准库 `testing`，不引入断言库。

## 外部参考

- Go 官方 CodeReviewComments：`https://go.dev/wiki/CodeReviewComments`
- Go 官方 Effective Go：`https://go.dev/doc/effective_go`
- Go 项目布局约定：`https://github.com/golang-standards/project-layout`
- Google Go 风格指南：`https://google.github.io/styleguide/go/decisions`
- Uber Go 风格指南：`https://github.com/uber-go/guide/blob/master/style.md`