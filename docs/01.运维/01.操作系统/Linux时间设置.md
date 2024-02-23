---
title: Linux时间设置
date: '2024-02-23 11:36:18'
meta:
  - name: keywords
    content: Area/Linux
tags:
  - Area/Linux
categories:
  - 运维
  - 操作系统
permalink: /post/linux-time-settings-vu9hl.html
author:
  name: Zhou
  link: https://github.com/Abner1zhou
---


<!-- more -->




# Linux时间设置

NTP服务器搭建

## 查看Linux时区

### 查看当前时区

1. <span style="font-weight: bold;" data-type="strong">使用</span>​<span style="font-weight: bold;" data-type="strong">`timedatectl`</span>​<span style="font-weight: bold;" data-type="strong">命令</span>
    如果您的系统使用的是systemd，您可以使用`timedatectl`​命令来查看当前的时区设置：

    ```
    timedatectl
    ```

    这将显示当前的日期和时间详情，包括时区。
2. <span style="font-weight: bold;" data-type="strong">查看</span>​ <span style="font-weight: bold;" data-type="strong">`/etc/timezone`</span>​<span style="font-weight: bold;" data-type="strong">文件</span>
    在一些基于Debian的系统中，时区设置存储在`/etc/timezone`​文件中：

    ```
    cat /etc/timezone
    ```
3. <span style="font-weight: bold;" data-type="strong">查看</span>​ <span style="font-weight: bold;" data-type="strong">`/etc/localtime`</span>​<span style="font-weight: bold;" data-type="strong">链接</span>
    `/etc/localtime`​是一个到`/usr/share/zoneinfo`​目录中相应时区文件的链接。您可以通过检查这个链接来确定当前设置的时区：

    ```
    ls -l /etc/localtime
    ```

### 列出所有可用时区

1. <span style="font-weight: bold;" data-type="strong">使用</span>​<span style="font-weight: bold;" data-type="strong">`timedatectl`</span>​<span style="font-weight: bold;" data-type="strong">命令</span>
    您可以列出所有可用的时区，以便选择合适的时区进行设置：

    ```
    timedatectl list-timezones
    ```
2. <span style="font-weight: bold;" data-type="strong">浏览</span>​ <span style="font-weight: bold;" data-type="strong">`/usr/share/zoneinfo`</span>​<span style="font-weight: bold;" data-type="strong">目录</span>
    `/usr/share/zoneinfo`​目录包含了所有可用的时区文件。您可以通过浏览这个目录来查看所有可用的时区：

    ```
    ls /usr/share/zoneinfo
    ```

### 设置时区

如果您需要更改时区，可以使用`timedatectl`​命令。例如，将时区设置为“America/New_York”：

```
sudo timedatectl set-timezone America/New_York
```

请根据您的具体需求和系统配置选择合适的方法来查看和设置时区。

1. <span style="font-weight: bold;" data-type="strong">使用</span>​<span style="font-weight: bold;" data-type="strong">`timedatectl`</span>​<span style="font-weight: bold;" data-type="strong">命令</span>  
    如果您的系统使用的是systemd，您可以使用`timedatectl`​命令来查看当前的时区设置：

‍

## 开启时间同步

​`timedatectl`​查看发现没有开启ntp，所以还是出现时间不准的情况

```lua
[root@localhost conf]# timedatectl
Warning: Ignoring the TZ variable. Reading the system's time zone setting only.

      Local time: Fri 2024-02-23 19:18:53 CST
  Universal time: Fri 2024-02-23 11:18:53 UTC
        RTC time: Fri 2024-02-23 03:20:53
       Time zone: Asia/Shanghai (CST, +0800)
     NTP enabled: no
NTP synchronized: no
 RTC in local TZ: no
      DST active: n/a
```

要在Linux系统中开启NTP（Network Time Protocol）同步，可以使用`timedatectl`​命令启用NTP服务。这将确保你的系统时钟自动与互联网上的NTP服务器同步，以保持精确的时间。以下是如何操作的步骤：

1. 首先，确保你的系统上安装了`systemd-timesyncd`​服务或其他NTP客户端（如`ntpd`​或`chronyd`​）。
2. 使用`timedatectl`​命令启用NTP同步：

    ```
    sudo timedatectl set-ntp true
    ```

    这个命令会启用`systemd-timesyncd`​服务或确保其他NTP服务（如果已安装）被激活。
3. 验证NTP同步是否已启用：

    ```
    timedatectl
    ```

    在命令输出中，你应该会看到“NTP enabled: yes”的行，表示NTP同步已经开启。

如果你的系统使用的是`ntpd`​或`chronyd`​作为NTP服务，你可能需要使用特定于服务的命令来启动和启用服务。例如：

* 对于`ntpd`​：

  ```bash
  sudo systemctl enable ntpd
  sudo systemctl start ntpd
  ```
* 对于`chronyd`​：

  ```bash
  sudo systemctl enable chronyd
  sudo systemctl start chronyd
  ```

确保你的防火墙设置允许NTP流量通过，以便你的系统可以与外部NTP服务器通信。常见的NTP流量使用UDP协议的123端口。

‍

## 手动修改时间

使用`timedatectl`​设置时间，您需要使用ISO 8601格式，如`YYYY-MM-DD HH:MM:SS`​：

```bash
sudo timedatectl set-time '2024-02-23 19:30:00'

```

‍
