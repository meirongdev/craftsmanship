# interview-prep

> 面试复习仓库：按模块组织，每个模块独立可导航。代码类条目要求可运行 + 单元测试 + 规范注释；系统设计类条目用 Mermaid 图表。

**技术栈侧重点：**

- Java / Spring Boot 后端与高并发架构
- JVM 调优与并发编程
- 分布式系统：事务、一致性、消息队列
- 云原生：Kubernetes 与容器化部署

## 目录导航

| 模块 | 内容 | 入口 |
| --- | --- | --- |
| [system-design](system-design/) | 系统设计与高并发架构 | [分布式事务](system-design/distributed-transactions.md) · [支付系统设计](system-design/payment-system-design.md) |
| [concurrency-and-performance](concurrency-and-performance/) | 多线程、高并发与性能调优 | [JVM 调优笔记](concurrency-and-performance/jvm-tuning-notes.md) · [锁机制](concurrency-and-performance/lock-mechanisms.md) |
| [algorithms-and-katas](algorithms-and-katas/) | 经典算法与手写核心组件 | [源码](algorithms-and-katas/src/) · [复杂度分析与思路](algorithms-and-katas/docs/) |
| [cloud-native-and-k8s](cloud-native-and-k8s/) | 云原生、容器化与分布式中间件 | [模块说明](cloud-native-and-k8s/README.md) |

## 复习路线图

1. **第 1 周**：concurrency-and-performance —— 锁机制、JVM 内存模型与调优
2. **第 2 周**：system-design —— 从分布式事务到完整支付系统设计
3. **第 3 周**：algorithms-and-katas —— 跑通全部手写组件 + 单元测试
4. **第 4 周**：cloud-native-and-k8s —— K8s 排障与中间件对比
5. **循环**：每周复盘错题，口头讲一遍每个模块的核心机制（费曼自测）

> 路线图是建议节奏，按实际面试安排调整。

## 仓库约定

- Commit Message 遵循 Conventional Commits（`feat:` / `docs:` / `test:` / `refactor:`）
- 代码：标准命名、必要注释、必须附单元测试；算法题在 `docs/` 给出复杂度分析与解题思路
- 图表：系统设计、架构演进、复杂机制优先用 Mermaid，避免截图
