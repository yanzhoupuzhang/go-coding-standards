# Go Coding Standards

一个面向 Go 项目的通用编码规范，可作为 Codex Skill 或团队规范文档使用。

目标是让 AI 和开发者写出更少、更清晰、更易调试和审查的 Go 代码：命名一致、注释规范、错误和日志统一、架构保持简单。

## 安装为 Codex Skill

将本目录复制到 Codex 的 skills 目录：

```powershell
$CODEX_HOME = "$env:USERPROFILE\.codex"
Copy-Item -Recurse -Force ".\go-coding-standards" "$CODEX_HOME\skills\go-coding-standards"
```

也可以直接复制 `SKILL.md` 所在目录到你的 Codex skills 根目录。

## 目录结构

```text
go-coding-standards/
├── SKILL.md                          # 主规范，Codex 自动加载
├── README.md                         # 使用与共享说明
├── LICENSE                           # 开源许可协议
├── agents/
│   └── openai.yaml                   # 技能显示名称和默认提示词
└── references/
    ├── architecture.md               # 架构边界与目录约束
    ├── code-review.md                # 代码审查清单
    └── external-standards.md         # 外部规范对照
```

## 规范摘要

### 结构与简单性

- 调用层级不超过 2 层，不过度设计，业务逻辑就地写。
- 不为单个使用点抽取辅助函数或构造器。
- 禁止把 `interface{}/any` 作为通用字段或参数类型做动态分派，禁止反射解析。
- 每个包保持单一职责，避免为“可能复用”提前抽象。

### 复用规则

- 公共包和工具函数统一放在项目当前公共目录，不写死路径。
- 已有同功能函数直接调用，不重新实现。
- 同一逻辑在 2 个及以上包中重复时，提取到公共目录并删除副本。

### 命名规则

- 标识符统一采用驼峰命名，禁止下划线作标识符分隔。
- 变量名、函数名、参数名和类型名至少 4 个字符，必须有明确业务含义。
- 接收器名、`err`、`ctx` 等惯用缩写除外。
- 缩写词大小写一致，例如 `ServeHTTP`、`appID`。

### 注释规则

- 注释使用中文；标识符和函数名除外。
- 方法使用多行块注释，`/*` 与 `*/` 各占一行。
- 方法注释固定包含：用途、参数、返回值。
- 方法内部使用 `//` 注释业务关键点。

### 错误处理

- 错误字符串小写开头，不以标点结尾。
- 不丢弃错误，禁止用 `panic` 代替错误处理。
- 正常路径保持最小缩进，错误先处理。
- 错误只包装一次并保留原始错误。

### 日志规则

- 日志库固定使用 `github.com/rs/zerolog`。
- 函数入口记录关键入参，敏感信息用 `"[REDACTED]"`。
- 每个 `return err` 前记录错误详情。
- 只使用 `Error` 和 `Info` 两个级别。
- 同一位置只打一条日志，默认写入日志文件。

### 接口与并发

- 接口定义在使用方，不定义在实现方。
- 实现方返回具体类型，不先定义接口再等待使用。
- 请求级函数显式传递 `context.Context`。
- goroutine 生命周期可退出、可等待。
- 密钥、token、随机数使用 `crypto/rand`。

### 测试规则

- 修改源码必须同步新增或更新自动化测试。
- 测试使用临时目录和独立数据。
- 优先使用表驱动测试和子测试。
- 失败信息包含函数、输入、`got` 与 `want`。
- 使用标准库 `testing`，运行 `go test ./...`。

## 外部参考

本规范优先，外部指南仅作补充：

- [Go CodeReviewComments](https://go.dev/wiki/CodeReviewComments)
- [Effective Go](https://go.dev/doc/effective_go)
- [Go Project Layout](https://github.com/golang-standards/project-layout)
- [Google Go Style Guide](https://google.github.io/styleguide/go/decisions)
- [Uber Go Style Guide](https://github.com/uber-go/guide/blob/master/style.md)

## 许可

本项目采用 MIT 许可协议，可自由使用、修改和分发。