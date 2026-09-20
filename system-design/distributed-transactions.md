# 分布式事务

> 目标：能在白板 30 分钟内讲清 2PC/3PC、TCC、Saga、本地消息表、最大努力通知的适用边界与失效模式。

## 核心要点

- [ ] 2PC / 3PC：流程、阻塞点、协调者故障
- [ ] TCC：Try-Confirm-Cancel，空回滚、悬挂、幂等
- [ ] Saga：编排式 vs 协同式，补偿事务的粒度
- [ ] 本地消息表 / 事务消息（outbox pattern）
- [ ] 一致性级别选择：强一致 vs 最终一致的业务判断

## 常见追问

- （TODO：记录面试中被问到的问题与答案）

## 参考

- 落地案例见 [payment-system-design.md](payment-system-design.md)
