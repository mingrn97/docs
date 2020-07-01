# 前言

作为开发人员通常因为各种原因会将个人电脑做成 `Linux` 服务器，大多数童鞋应该是抱着学习观摩的想法吧。

台式主机还好，不过如果是笔记本一类的便携设备的话我们通常会遇到一个问题：合上笔记本就直接进入入休眠模式无法继续对外提供服务了。

这种感jio超级蛋疼，为此呢我们就需要研究一下 `Linux` 的电源管理设置了🤐~

| Note                                                         |
| :----------------------------------------------------------- |
| 本文以 `CentOS7`  做说明：`CentOS Linux release 7.8.2003 (Core)`（`cat /etc/redhat-release`） |

# 电源管理

`Linux` 的电源管理通常是一个被称为 **登录管理器配置** 的文件，该文件在系统中的位置通常如下：

```bash
/etc/systemd/logind.conf
/etc/systemd/logind.conf.d/*.conf
/run/systemd/logind.conf.d/*.conf
/usr/lib/systemd/logind.conf.d/*.conf
```

默认的设置通常是在编译期间就确定了，默认的文件（初始配置文件）配置文件在 `/etc/systemd` 目录下。有一点需要说明，`/etc` 目录是超级管理员用户( `sudo` ) 才有的权限，所以如果是其他用户想要修改电源配置就需要将该文件拷贝到 `/usr/lib/systemd/*.conf.d/` 的目录下然后再做修改。`/etc` 作为主配置，优先级也是最低的，所以 `*.conf.d/` 目录中的配置会覆盖主配置。

同样的，如果管理员想要屏蔽 `/usr/lib/` 目录中的某个配置文件最佳的做法就是在 `/etc` 目录中创建一个指向 `/dev/null` 的同名符号链接即可彻底屏蔽 `/usr/lib/` 目录中的同名文件了~

| Note                                                         |
| :----------------------------------------------------------- |
| 所有的 `*.conf.d` 目录中的配置统一是按照字典的顺序进行加载。所以，如果多个文件存在相同的配置，文件越靠后级别越高。所以，为了便于排序，建议将 `*.conf.d/`目录中的配置文件都加上两位阿拉伯数字作为文件名前缀。 |

现在就来看下主配置文件 `/etc/systemd/logind.conf` 默认的登录管理器配置内容：

```bash
$ cat /etc/systemd/logind.conf

[Login]
#NAutoVTs=6
#ReserveVT=6
#KillUserProcesses=no
#KillOnlyUsers=
#KillExcludeUsers=root
#InhibitDelayMaxSec=5
#HandlePowerKey=poweroff
#HandleSuspendKey=suspend
#HandleHibernateKey=hibernate
#HandleLidSwitch=suspend
#HandleLidSwitchDocked=ignore
#PowerKeyIgnoreInhibited=no
#SuspendKeyIgnoreInhibited=no
#HibernateKeyIgnoreInhibited=no
#LidSwitchIgnoreInhibited=yes
#IdleAction=ignore
#IdleActionSec=30min
#RuntimeDirectorySize=10%
#RemoveIPC=no
#UserTasksMax=
```

