---
title: Solitaire
date: 2018-06-11 13:48:29
tags:
    - Meiri
categories: Meiri Project
---

# 写在前面
冥利计划好久没有更新了，此次更新的是冥利的记录(Record)功能，冥利会把内容记录到数据库中，供以后查询分析使用。

<!--more-->

## 数据库设计
<center>Author table</center>

|Field Name|Data Type|Note|
|----|----|----|
|id|integer|primary key|
|name|text|null|
|note|text|null|
<center>data table</center>

|Field Name|Data Type|Note|
|----|----|----|
|id|integer|primary key|
|record id|integer|foreign key|
|author id|integer|foreign key|
|time|timemap|not null|
|content|text|not null|
<center>record table</center>

|Field Name|Data Type|Note|
|----|----|----|
|id|integer|primary key|
|name|text|not null|
|createtime|timemap|not null|
|group|text|null|
|status|text|in (active, dead, pause, continue)|
<center>author-record table</center>

|Field Name|Data Type|Note|
|----|----|----|
|id|integer|primary key|
|author id|integer|foreign key|
|record id|integer|foreign key|
