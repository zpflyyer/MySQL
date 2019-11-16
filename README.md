# MySQL InnoDB存储引擎

------

## 关键特性
- 事务
- 锁/并发
- 聚合/二级索引

## 事务四大原则
- Atomicity
    - 自动提交
    - 提交
    - 回滚
- Consistency
    - 双写buffer
    - crash recovery
- Isolation
    - 事务隔离级别
    - InnoDb 锁机制
    - 自动提交
- Durability

## Multi-Versioning
- InnoDB是multi-versioned的storage engine：保留old versions of changed rows,以支持consistent read和transaction rollback。这些信息存储在**rollback segment**里
- InnoDB为数据库里每个row增加三条附加字段：
   1. DB_TRX_ID：最近一次更新（insert/update/delete）该记录的transaction id；
   2. DB_ROLL_PTR：指向rollback segment里对应的一条undo log 记录的指针；
   3. DB_ROW_ID：单调自增的行号值.
- undo log
   1. Insert undo log：仅在事务回滚时会用到，且一旦事务提交后即可丢弃
   2. Update undo log：不仅用于事务回滚，一致性读里也需要，即使当前事务提交了，如果innodDB为其他事务创建了该row的snapshot，那么依然需要undo log来实现一致性读
