# 创建 Ambari & HDP 的本地仓库
1. 下载仓库文件tar包，地址可以从 [这里](http://docs.hortonworks.com/HDPDocuments/Ambari-2.4.2.0/bk_ambari-installation/content/ambari_repositories.html)获得。
2. 安装 http 服务 (httpd, apache2, etc.)
3. 生成 `ambari.repo` 文件
```
#VERSION_NUMBER=2.4.1.0-22
[Updates-ambari-2.4.0.1]
name=ambari-2.4.0.1 - Updates
baseurl=http://192.168.1.9/AMBARI-2.4.0.1/centos7/2.4.0.1-1
gpgcheck=1
gpgkey=http://public-repo-1.hortonworks.com/ambari/centos7/RPM-GPG-KEY/RPM-GPG-KEY-Jenkins
enabled=1
priority=1
```

# 环境准备

## 建立免密 SSH 登录

1. 在 Ambari-Server 主机上生成 SSH 公钥和私钥
  `ssh-keygen`
2. 将公钥拷贝到目标主机上
3. 将公钥内容追加到 `authorized_keys` 文件中
  `cat id_rsa.pub >> authorized_keys`
4. 你可能需要将 `.ssh` 目录的权限设置为 `700`，将 `authorized_keys` 文件权限设置为 `600`
  ```
  chmod 700 ~/.ssh
  chmod 600 ~/.ssh/authorized_keys
  ```
5. 确保能够在 Ambari-Server 机器上免密 SSH 到 ambari-agent 机器上

## 建立 HDP 服务的用户账户

每个 HDP service 都会要求建立一个用户账户（Linux user）

## 在集群中和通过浏览器访问集群的主机上开启 NTP 服务

集群和通过浏览器访问集群的主机之间的时钟必须保持同步

安装 NTP 服务
```
RHEL/CentOS/Oracle 6
  yum install -y ntp
RHEL/CentOS/Oracle 7
  yum install -y ntp
```
检查每个主机上的 NTP 服务是否开机启动
```
RHEL/CentOS/Oracle 6
  chkconfig --list ntpd
RHEL/CentOS/Oracle 7
  systemctl is-enabled ntpd
```
设置 NTP 服务开机自启
```
RHEL/CentOS/Oracle 6
  chkconfig ntpd on
RHEL/CentOS/Oracle 7
  systemctl enable ntpd
```
开启 NTP 服务
```
RHEL/CentOS/Oracle 6
  service ntpd start
RHEL/CentOS/Oracle 7
  systemctl start ntpd
```

## 检查 DNS 和 NSCD （Name Service Caching Daemon）
必须为所有的主机配置正向和反向的 DNS ？

在 `/etc/hosts` 文件中添加集群中所有的机器的 ip 地址与机器名的映射

Hadoop 强烈依赖 DNS。Hadoop relies heavily on DNS, and as such performs many DNS lookups during normal operation. To reduce the load on your DNS infrastructure, it's highly recommended to use the Name Service Caching Daemon (NSCD) on cluster nodes running Linux. This daemon will cache host, user, and group lookups and provide better resolution performance, and reduced load on DNS infrastructure.

## 配置 iptables

Ambari 在运行 setup 过程期间会检查 iptables 是否启用，如果出现了warning且你没有关闭iptables，那么你就要检查是否所有所需的端口都已对外开放且可用。

暂时关闭 iptables
  ```
  chkconfig iptables off
  /etc/init.d/iptables stop
  ```

  ```
  systemctl disable firewalld
  service firewalld stop
  ```

或者是保持 iptables 开启，配置所需的端口开放可用即可。所需开放[端口](http://docs.hortonworks.com/HDPDocuments/Ambari-2.4.2.0/bk_ambari-reference/content/ch_configuring_network_port_numbers.html)

## Disable SELinux and PackageKit and check the umask Value

1. 禁用集群中所有主机的 SELinux
  `setenforce 0`
  要确保SELinux不会在机器重启后自行打开，可在`/etc/selinux/config`中设置`SELINUX=disabled`
2. 在安装了 PackageKit 的 RHEL/CENTOS 主机上，更改 `/etc/yum/pluginconf.d/refresh-packagekit.conf` 中 `enable=0`
3. UMASK (User Mask or User file creation MASK) sets the default permissions or base permissions granted when a new file or folder is created on a Linux machine. Most Linux distros set 022 as the default umask value. A umask value of 022 grants read, write, execute permissions of 755 for new files or folders. A umask value of 027 grants read, write, execute permissions of 750 for new files or folders. Ambari 和 HDP 支持的umask值为022（即0022）和027（即0027）
  查看umask： `umask`
  设置umask： `umask 022`
  为所有交互用户更改umask： `echo umask 0022 >> /etc/profile`

http://docs.hortonworks.com/HDPDocuments/Ambari-2.4.2.0/bk_ambari-installation/content/disable_selinux_and_packagekit_and_check_the_umask_value.html
