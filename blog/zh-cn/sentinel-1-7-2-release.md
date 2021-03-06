# Sentinel 1.7.2 版本发布

[Sentinel 1.7.2](https://github.com/alibaba/Sentinel/releases/tag/1.7.2) 正式发布，带来了 Logger SPI 扩展机制、Zuul 2.x 网关流控、SOFARPC 适配等多项特性和改进。下面我们来一起探索一下 Sentinel 1.7.2 的重要特性。

## 多样化的适配模块

到目前为止，Sentinel 已覆盖微服务、API Gateway 和 Service Mesh 三大板块的核心生态，同时多语言已推出 Java、C++、Go 三种语言的原生实现。

![sentine-oss-ecosystem](https://user-images.githubusercontent.com/9434884/78636450-ef3a4b00-78da-11ea-89ce-c7a2b58c2deb.png)

得益于社区的贡献，Sentinel 1.7.2 带来了更多的适配模块：

- Zuul 2.x 适配模块：可以针对 Zuul 2.x 网关配置定制化的流控策略，流控粒度可以是路由维度以及自定义 API 分组维度。
- SOFARPC 适配模块：可以针对 SOFARPC provider/consumer 接口和方法配置规则，支持来源限流，支持配置 fallback 处理逻辑。

## 日志扩展机制

1.7.2 版本引入了全新的日志扩展机制，新增 `Logger` SPI 扩展点（目前仅针对 RecordLog 和 CommandCenterLog 生效）。用户可以自定义 Logger 实现来适配项目中的日志模块（如 slf4j、logback、log4j2 等）。Sentinel Core 默认的日志实现仍然基于 JDK logging，同时社区提供了 slf4j 适配模块，用户只需引入 `sentinel-logging-slf4j` 模块并在相应的日志配置文件中针对 `sentinelRecordLogger` 和 `sentinelCommandCenterLogger` 进行配置即可，方便使用。

## Slot SPI 扩展机制重构

Sentinel 各个特性都是由不同的 slot 组成的。在之前的版本中，slot 扩展是通过 `SlotChainBuilder` SPI 机制来实现的，这样设计的初衷是让用户关注各 slot 的顺序，显式地编排 slot chain。但这种方式对于不同模块分别扩展 slot 来说是不灵活的，同时对于大部分用户来说其实不关心各个模块的各个 slot 的顺序。因此 1.7.2 版本我们对 slot 扩展机制进行了重构，将 `ProcessorSlot` 本身作为 SPI 进行扩展，每个 slot 通过 `@SpiOrder` 注解指定顺序，从而可以方便地将不同模块的 slot 组合起来。未来版本社区还会进一步强化 slot SPI 的扩展方式，使之具备任意插拔的能力。

## 其它特性与改进

- Spring Web 适配模块支持链路维度流控
- 完善 `sentinel-transport-simple-http` 模块，支持较大的 POST 请求
- 完善规则 HTTP 方式推送的错误提示，检测客户端低版本 fastjson


详情请参考 [Release Notes](https://github.com/alibaba/Sentinel/wiki/Release-Notes#172)，欢迎大家使用并提出建议，同时欢迎大家一起参与后续版本的演进。