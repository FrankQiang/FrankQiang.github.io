---
layout: post
title: Linux定时任务
category: 技术
tags: Linux cron job
keywords: Linux  cron job
---

#### 定时任务

    sudo vi /etc/crontab

    # m h dom mon dow user  command

    59 10 * * * root /home/deal_photo/compress.py

    分钟 小时 天 月 天每星期 命令

    每个字段代表的含义及取值范围如下：
    Minute ：分钟（0-59），表示每个小时的第几分钟执行该任务
    Hour ： 小时（1-23），表示每天的第几个小时执行该任务
    Day ： 日期（1-31），表示每月的第几天执行该任务
    Month ： 月份（1-12），表示每年的第几个月执行该任务
    DayOfWeek ： 星期（0-6，0代表星期天），表示每周的第几天执行该任务
    Command ： 指定要执行的命令

> 注意 脚本里的内容要绝对路径
