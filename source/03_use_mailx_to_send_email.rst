利用mailx发送邮件通知
======================

安装sendmail和mailx
-----------------------

使用yum安装::

    yum install sendmail mailx  -y 

修改mailx的配置文件
------------------------

在/etc/mail.rc文件最后添加以下文本::

    vi /etc/mail.rc
    # 外部发送邮箱地址
    set from=mzh_love_linux@163.com
    # 外部smtp服务器的地址，不使用SSL协议加密，端口号25
    set smtp=smtp.163.com
    # 外部smtp服务器认证的用户名
    set smtp-auth-user=mzh_love_linux
    # 启用授权码，避免密码泄漏造成邮箱安全隐患，使用邮件客户端更安心
    # 密码，此处使用网易邮箱授权密码
    set smtp-auth-password=password
    # 邮件认证的方式
    set smtp-auth=login
    
保存后，重启sendmail::

    service sendmail restart

测试sendmail发送邮件::

    echo "sendmail测试"|mail -s "Linux外部邮箱发信测试" xxxxxxx@qq.com

如果配置成功，则xxxxxxx@qq.com这个邮箱会收到测试邮件。

配置SSL加密协议发送邮件
----------------------------------

在/etc/mail.rc文件最后添加以下文本::

    # 外部发送邮箱地址
    set from=mzh_love_linux@163.com
    # 外部smtp服务器的地址，使用SSL协议加密，端口号465
    # 设置SMTP地址及端口，注意smtps说明启用了SSL加密
    set smtp=smtps://smtp.163.com:465
    # 外部smtp服务器认证的用户名
    set smtp-auth-user=mzh_love_linux
    # 启用授权码，避免密码泄漏造成邮箱安全隐患，使用邮件客户端更安心
    # 密码，此处使用网易邮箱授权密码
    set smtp-auth-password=password
    # 邮件认证的方式
    set smtp-auth=login
    # 指定本地证书路径
    set nss-config-dir=/etc/pki/nssdb/
    # 忽略证书错误
    set ssl-verify=ignore


发送测试邮件::

    echo -e "正文"|mail -s "Linux使用mailx发送邮件通知" xxxxxxx@qq.com
    
监控磁盘空间并自动发送邮件通知
-----------------------------------


脚本内容如下::

    #!/bin/bash
    # Filename: disk_check.sh
    # Author: Mei Zhaohui
    # Date: 2019-08-15 22:17
    # Function: Monitor hard disk space and send email to Admin
    # You can add this shell to crontab

    # For details see man 4 crontabs

    # Example of job definition:
    # .---------------- minute (0 - 59)
    # |  .------------- hour (0 - 23)
    # |  |  .---------- day of month (1 - 31)
    # |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
    # |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
    # |  |  |  |  |
    # *  *  *  *  * user-name  command to be executed

    # 30 1 * * * /usr/bin/sh /root/disk_check.sh
    export PATH=/sbin:/bin:/usr/sbin:/usr/bin:$PATH
    maxused=75
    admin_emails="xxxxxxx@qq.com"
    host_ip=$(ip addr show|grep 192|awk -F '[ /]+' '{print $3}')
    disk_size=$(df -h|grep '/$'|awk -F '[ %]+' '{print $2}')
    used=$(df -h|grep '/$'|awk -F '[ %]+' '{print $5}')
    hard_free=$(df -h|grep '/$'|awk -F '[ %]+' '{print $4}')
    if [[ "${used}" -gt "${maxused}" ]]; then
        echo "Used greater then the maxused value"
        avail_percentage=$[100-$used]
        echo -e "Linux系统磁盘快满了，请尽快处理。详情如下：\n主机IP：${host_ip}\n磁盘总量：${disk_size}\n剩余空间：${hard_free}\n当前可用比例：${avail_percentage}%\n\n*********本邮件由系统自动发送，请勿直接回复*********"|mailx -s "[警告]Linux系统(${host_ip})磁盘要满了" "${admin_emails}" > /dev/null 2>&1
    fi

将脚本加入到定时任务中，收到的邮件通知类似下图：

.. image:: ./_static/images/disk_check_email.png
