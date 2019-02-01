
利用mutt发送接收邮件
====================

安装sendmail
------------

使用yum安装::

    yum install -y sendmail

配置sendmail
------------

在/etc/mail.rc文件最后添加以下文本::

    vi /etc/mail.rc
    set from=mzh_love_linux@163.com #外部发送邮箱地址
    set smtp=smtp.163.com #外部smtp服务器的地址
    set smtp-auth-user=mzh_love_linux #注：外部smtp服务器认证的用户名
    #（启用授权码，避免密码泄漏造成邮箱安全隐患，使用邮件客户端更安心）
    set smtp-auth-password=password #注：密码，此处使用网易邮箱授权密码
    set smtp-auth=login #注：邮件认证的方式

保存后，重启sendmail::

    service sendmail restart

测试sendmail发送邮件::

    echo "sendmail测试" \|mail -s "Linux外部邮箱发信测试" 798423939@qq.com

如果配置成功，则798423939@qq.com这个邮箱会收到测试邮件。

安装mutt
--------

使用yum安装mutt::

    yum -y install mutt

配置mutt
--------

将/etc/Muttrc文件复制到root用户家目录中，并命名为.muttrc

复制配置文件::

    cp /etc/Muttrc ~/.muttrc

vi ~/.muttrc在结尾处添加以下代码：

修改配置文件::

    set envelope_from=yes
    set from=“发送邮件地址 如：mzh_love_linux@163.com”
    set realname=“发件人 如：梅朝辉”
    set charset = “utf-8”
    set locale = “utf-8”
    set pop_user=“mzh_love_linux” #设置POP收信用户名
    set pop_pass=“password” #设置POP收信密码（网易邮箱的授权密码）
    set pop_host=“pop.163.com” #设置POP收信服务器

退出后，测试Mutt发送邮件。
发送邮件::
    
   mutt 798423939@qq.com -s “mutt测试” -a ~/.muttrc

向QQ邮箱中发送一封主题是“mutt测试”附件是.muttrc的邮件。
