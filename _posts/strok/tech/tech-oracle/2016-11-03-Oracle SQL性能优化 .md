---
layout: single
title: Oracle SQL性能优化 
description: 梳理自己在工作中的总结 
tags: 性能优化
---

# 影响SQL性能的因素
1. 数据量大，而且是全表查询
2. 没有做索引
3. 

# 明确影响性能的因素
> plsql developer 已经提供了如何查看执行计划的工具。
但是autotrace查看执行计划，还需要DBA的功底。

## 如何看执行计划的报告
1. 执行顺序明确：
> 缩进做靠右的先执行，同样缩进的有多行时，最上面的先执行。
2. 清晰执行耗费
> 
3. 执行谓词
> Access谓词：
> Filter谓词：

4. 表访问方式
>全表访问：
>索引扫描：即便是索引，也有好多种索引。
	1. B叉树索引
	>这里面又分了好多种
	2. 位图索引
	> 这个也是分了好多种。



5. 表连接方式
>嵌套循环连接：NESTED LOOPS
哈希连接：hash join
排序合并连接：sort join和merge join
反连接：nested loops anti、hash join anti、merge join anti
半连接：nested loop semi、hash join semi、merge join semi

6. 运算符



# 如何优化
>其实，最有价值的部分是找到影响性能的因素，在技术界，只要找到原因，那么就有解决办法。