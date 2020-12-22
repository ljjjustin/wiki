<link rel="stylesheet" type="text/css" href="/auto-number.css">

# 监控方案

## 监控设计原则

* 需要处理的告警才发出来，发出来的告警必须得到处理
* 架构尽可能简单稳定，业务系统出问题了，监控也不能出问题
* 监控是基础设施，不比大而全，能解决问题就好

### 黄金指标

* Google SRE：请求数、响应时间、错误数、饱和度；
* 资源监控USE法：Utilization、Saturation、Errors
* 业务监控RED法：Rate、Errors、Duration

## Prometheus

### Prometheus的特点

* Prometheus是基于metric的监控系统，需要提炼监控的metric；不适用于日志、事件、调用链的监控
* Prometheus是基于pull的模式进行监控，部署时要充分考虑网络通信需求
* Prometheus的监控不能保证监控的准确性，与基于日志的监控有较大的区别
* Prometheus的数据都是经过聚合的数据

## Zabbix

## Telegraf

## 容器监控方案

https://yasongxu.gitbook.io/container-monitor/
