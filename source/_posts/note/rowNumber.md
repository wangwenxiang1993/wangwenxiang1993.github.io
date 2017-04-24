---
layout: post
title: 记一次hive洗数
date: 2017/04/16
---

##### 最近在公司洗数的需求比较多，其中的一个需求是这样子的，原始的表结构类似于：
|&nbsp;page|&nbsp;rule|&nbsp;&nbsp;uid|&nbsp;&nbsp;test|date|
| -- | -- | -- | -- | -- |
|页面|规则|用户唯一id|实验编号和分组|日期|
##### 解析后的表结构：
|date|test_code|test_group|type|times|
| -- | -- | -- | -- | -- |
|时间（精确到小时）|实验编号|实验分组|记录类型|记录次数|
<!--more-->

### 表说明
##### 原表记录的是一个用户在某一个时刻在某个页面的某个操作，test字段标识的是这个用户所属的实验和分组
##### 目标表指的是在某个小时内，某个实验的某个分组的某个动作，一共有多少次操作。比如说在2017年4月16日的12点到13点这一个小时，实验1的A分组下，搜索的次数有1w，下单的用户有5000。很明显，这是一张竖表，不同的动作用type去标识，而不是一个具体的字段。
##### type字段下，有表示搜索次数的，也有表示搜索人数的(也就是搜索次数按照uid去重，表示有多少人搜索了)。

## 问题解决
###
##### 所有的这些东西，都需要用hive的sql去解决和关联，首先是test字段