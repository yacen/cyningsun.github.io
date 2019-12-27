---
layout: post
title: 一文读懂 Prometheus
category: Prometheus
tags: 概念
keywords:
  - Prometheus
  - 基本概念
---

* TOC
{:toc}

### 背景
对很多人来说，未知、不确定、不在掌控的东西，会有潜意识的逃避。当我第一次接触 Prometheus 的时候也有类似的感觉。对初学者来说， Prometheus 包含的概念太多了，门槛也太高了。
> 概念： Instance、Job、Metric、Metric Name、Metric Label、Metric Value、Metric Type（Counter、Gauge、Histogram、Summary）
    、DataType（Instance Vector、Range Vector、Scalar、String）、Operator、Function

马云说：“虽然阿里巴巴是全球最大的零售平台，但阿里不是零售公司，是一家数据公司”。Prometheus 也是一样，本质来说是一个基于数据的监控系统。

### 日常监控
假设需要监控 WebServerA 每个API的请求量为例




### 存储引擎
TSDB 作为 Prometheus 的存储引擎完美契合了监控数据的应用场景
- 存储的数据量级十分庞大
- 大部分时间都是写入操作
- 写入操作几乎是顺序添加，大多数时候数据到达后都以时间排序
- 写操作很少写入很久之前的数据，也很少更新数据。大多数情况在数据被采集到数秒或者数分钟后就会被写入数据库
- 删除操作一般为区块删除，选定开始的历史时间并指定后续的区块。很少单独删除某个时间或者分开的随机时间的数据
- 基本数据大，一般超过内存大小。一般选取的只是其一小部分且没有规律，缓存几乎不起任何作用
- 读操作是十分典型的升序或者降序的顺序读
- 高并发的读操作十分常见