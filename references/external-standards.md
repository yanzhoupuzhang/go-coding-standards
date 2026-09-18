# 外部规范对照

外部规范仅作补充，冲突时以本规范为准。这里只保留本规范未重复的条目，重复约束请直接查看主规范。

## Go 官方 CodeReviewComments

来源：`https://go.dev/wiki/CodeReviewComments`

- 提交前运行 `gofmt`；使用 `goimports` 管理导入。
- 声明空切片优先使用 `var t []string`，不用 `t := []string{}`。
- 注释紧邻对应声明或 `package` 子句。
- 不丢弃错误。
- 导入标准库一组、外部依赖一组，组间空行。
- 不要随意命名结果参数或使用裸返回。
- 同步函数优先；异步接口需显式说明。
- 测试失败信息包含函数、输入、`got` 和 `want`。

## Google Go 风格指南

来源：`https://google.github.io/styleguide/go/decisions`

- 泛型、类型别名、`switch/break` 等按官方决策使用。
- 格式化字符串中使用 `%q` 显示字符串值。
- 使用标准库 `testing`，避免断言库。
- 测试采用表驱动、子测试；失败信息明确、可比较。

## Uber Go 风格指南

来源：`https://github.com/uber-go/guide/blob/master/style.md`

- 显式验证接口实现，例如 `var _ http.Handler = (*Handler)(nil)`。
- 枚举从 1 开始，保留零值含义。
- 主程序退出只发生一次，`main` 通常调用 `os.Exit`。
- 序列化结构体使用字段标签。
- 性能：`strconv` 优于 `fmt`，避免重复字符串与字节互转，可预估容量时指定容量。
- 风格：避免过长行、保持一致性、同类声明分组、减少嵌套、去掉无意义 `else`。
- 测试使用表驱动和并行测试；避免表测试中不必要的复杂度。

## Effective Go

来源：`https://go.dev/doc/effective_go`

- 遵循 `gofmt` 格式约定。
- 嵌入用于组合，不用于继承语义。
- 并发使用 channel 和 goroutine，但优先可同步验证的设计。