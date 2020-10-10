```shell

# 关闭Selinux
setenforce 0

# 关闭防火墙
systemctl stop firewalld
systemctl disable firewalld

# 设置IPV4转发
vim /etc/sysctl.conf
net.ipv4.ip_forward = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1

# 禁用Swap,否则kubelet组件无法运行
vim /etc/fstab # 注释掉swap项。
swapoff -a

# 启用cgroup,修改配置文件/etc/default/grub，启用cgroup内存限额功能,配置两个参数：
GRUB_CMDLINE_LINUX_DEFAULT="cgroup_enable=memory swapaccount=1"
GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"
注意：要执行sudo update-grub 更新grub，然后重启系统后生效。


wget https://github.com/rancher/rke/releases/download/v1.1.9/rke_linux-amd64
```


常见问题

```shell
# 执行sysctl -p时出现
sysctl: cannot stat /proc/sys/net/bridge/bridge-nf-call-ip6tables: No such file or directory
sysctl: cannot stat /proc/sys/net/bridge/bridge-nf-call-iptables: No such file or directory

# 解决办法
modprobe br_netfilter
ls /proc/sys/net/bridge
sysctl -p
```
