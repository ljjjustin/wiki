<link rel="stylesheet" type="text/css" href="/auto-number.css">

# Keepalived

## VRRP

### vrrp_script

vrrp_script的目的是对业务状态进行监控，根据业务状态觉得是否进行主备切换。

Keepalived会定时执行业务监控脚本，根据业务监控的结果，动态调整vrrp_instance的优先级。优先级的调整原则如下：

* 如果脚本执行结果为0，并且weight配置的值大于0，则优先级相应的增加
* 如果脚本执行结果非0，并且weight配置的值小于0，则优先级相应的减少
* 其他情况，维持原本配置的优先级，即配置文件中priority对应的值

注意：各个节点的权重不会一直升高或下降，权重的范围始终是：1-254。

### 最佳实践

为避免不必要的主备切换，建议将所有的节点都设置为BACKUP角色，权重相同，并设置nopreempt，这样多个节点最终都是通过权重决定谁是master。相同权重的情况下，Keepalived会根据mac
地址来决定谁变为master节点。通过业务的状态检测，动态调整权重，权重最大的变为MASTER。这样，一点master宕机，再次加入时就不会进行切换了。


## keepalived+haproxy

## keepalived+nginx
