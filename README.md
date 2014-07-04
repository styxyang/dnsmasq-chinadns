dnsmasq-chinadns
================

A patched version of dnsmasq which filters out some suspicious IP

Install
-------

* Linux / OS X

        git clone https://github.com/styx-hy/dnsmasq-chinadns.git
        cd dnsmasq-chinadns.git
        make

  I suggest you use the system default settings for dnsmasq.

  For example, on Debian GNU/Linux,
  simply change `DAEMON` variable in `/etc/init.d/dnsmasq` to the `/path/of/dnsmasq-chinadns/src/dnsmasq`.
  Then use command `sudo service dnsmasq restart` to restart the service. Other distributions may
  have similar configurations

Patch
-------

Patch file will be available later on.

Usage
-------

After you have correctly configure your dnsmasq as your DNS cache server,
you can test it with command

    › nslookup www.youtube.com
    Server:         127.0.0.1
    Address:        127.0.0.1#53
    
    Non-authoritative answer:
    www.youtube.com canonical name = youtube-ui.l.google.com.
    youtube-ui.l.google.com canonical name = youtube-ui-china.l.google.com.
    Name:   youtube-ui-china.l.google.com
    Address: 74.125.31.139
    Name:   youtube-ui-china.l.google.com
    Address: 74.125.31.138
    Name:   youtube-ui-china.l.google.com
    Address: 74.125.31.101
    Name:   youtube-ui-china.l.google.com
    Address: 74.125.31.100
    Name:   youtube-ui-china.l.google.com
    Address: 74.125.31.102
    Name:   youtube-ui-china.l.google.com
    Address: 74.125.31.113

To examine the query process (use Debian as the example),
uncommnent `log-queries` in `/etc/dnsmasq.conf`, use the command (Debian):

    sudo tail -f /var/log/syslog

and you may observe logs like

    using nameserver 8.8.8.8#53
    using nameserver 114.114.114.114#53
    read /etc/hosts - 7 addresses
    query[A] www.youtube.com from 127.0.0.1
    forwarded www.youtube.com to 8.8.8.8
    forwarded www.youtube.com to 114.114.114.114
    reply www.youtube.com is <CNAME>
    reply youtube-ui.l.google.com is <CNAME>
    possible DNS-rebind attack detected: youtube-ui-china.l.google.com
    possible DNS-rebind attack detected: www.youtube.com
    possible DNS-rebind attack detected: www.youtube.com
    reply www.youtube.com is <CNAME>
    reply youtube-ui.l.google.com is <CNAME>
    reply youtube-ui-china.l.google.com is 173.194.72.100
    reply youtube-ui-china.l.google.com is 173.194.72.101
    reply youtube-ui-china.l.google.com is 173.194.72.139
    reply youtube-ui-china.l.google.com is 173.194.72.102
    reply youtube-ui-china.l.google.com is 173.194.72.138
    reply youtube-ui-china.l.google.com is 173.194.72.113

