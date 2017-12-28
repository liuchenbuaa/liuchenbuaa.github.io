---
layout: default
---

# _**【笔记】spring事务管理**_

## 前言
    钻研一下spring事务
    
### 1、了解PlatformTransactionManager interface

    public interface PlatformTransactionManager {
    
        TransactionStatus getTransaction(
                TransactionDefinition definition) throws TransactionException;
    
        void commit(TransactionStatus status) throws TransactionException;
    
        void rollback(TransactionStatus status) throws TransactionException;
    }
    
    
    
### 2、innodb的x锁和s锁

    innodb的行级别锁分为两种：S锁(shared locks) 和 X锁(exclusive locks).
    在读取一条记录的时候会上S锁、更新或者删除一条记录时会加X锁。
    其中X锁与S锁的竞争关系是和java的读写锁类似，就是只有SS不互相影响，其他都不能同时进行。 
    
    意向锁(Intention Locks)
    意向锁是表锁，代表了对该表读或写的意向。
    
    相关文档：https://dev.mysql.com/doc/refman/5.7/en/innodb-locking.html
    
### 3、数据库事务嵌套

    mysql事务不能嵌套，在执行start transaction之前会有隐式的commit操作。
    
    相关文档：https://dev.mysql.com/doc/refman/5.7/en/implicit-commit.html
    
### 4、innodb支持事务，myisam不支持。

    innodb支持start transaction，myisam不支持，但是两种引擎都支持setautocommit = 0;

### 5、数据库什么时候会死锁

    相关文档：https://dev.mysql.com/doc/refman/5.7/en/innodb-deadlock-example.html
    
### 6、readonly的事务声明干什么用

    保证当前会话获取的数据具有一致性
