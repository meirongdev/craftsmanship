# 锁机制

> 目标：讲清 JVM 内锁的演进与 AQS 结构，以及分布式锁（Redis / ZooKeeper）的失效边界。

## JVM 锁

- [ ] synchronized：偏向 → 轻量级 → 重量级（JDK 15+ 偏向锁默认关闭）
- [ ] AQS：状态位、ConditionObject、公平/非公平
- [ ] StampedLock 与读倾斜

## 并发容器与无锁

- [ ] ConcurrentHashMap（JDK 8：CAS + synchronized）
- [ ] 无锁队列（Mpsc、JCTools）

## 分布式锁

- [ ] Redis SETNX + 看门狗（Redisson）：主从切换丢锁
- [ ] ZooKeeper 临时顺序节点：性能与脑裂
- [ ] 数据库锁 / etcd lease 对比

## 常见追问

- （TODO）
