# NOTES

This activity required the redhat.rhel\_system\_roles tarball from Red Hat.
I used this command to install that tarball into the collections directory:
```
$ ansible-galaxy collection install redhat.rhel_system_roles-1.19.3.tar.gz -p collections/
```

Run storage.yml and test results:
```
$ ansible-navigator run -m stdout storage.yml
$ ansible webservers -m command -a "df -h"
servera.lab.example.com | CHANGED | rc=0 >>
... output truncated ...
/dev/mapper/vg_web-lv_content  123M  7.6M  116M   7% /var/www/html/content
/dev/mapper/vg_web-lv_uploads  251M   15M  236M   6% /var/www/html/uploads
tmpfs                           97M     0   97M   0% /run/user/1001
```

Run dev-users.yml and test results:
```
$ ansible-navigator run -m stdout dev-users.yml --playbook-artifact-enable false --vault-id @prompt
$ ansible webservers -m command -a "cat /etc/passwd" | grep developer
developer:x:1002:1003::/home/developer:/bin/bash
$ ansible webservers -m command -a "cat /etc/sudoers" | grep webdev
%webdev ALL=(ALL) NOPASSWD: ALL
$ ssh developer@servera.lab.example.com
[servera]$ sudo su -
[servera]# exit
[servera]$ exit 
```

Run network.yml and test results:
```
$ ansible-navigator run -m stdout network.yml
$ ansible webservers -m command -a "ip addr"
servera.lab.example.com | CHANGED | rc=0 >>
... output truncated ...
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:01:fa:1e brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    altname ens4
    inet 172.25.250.25/24 brd 172.25.250.255 scope global noprefixroute eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::c475:aec7:2c68:e9/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```

Run log-rotate.yml and test results:
```
$ ansible-navigator run -m stdout log-rotate.yml
$ ansible webservers -m command -a "cat /etc/cron.d/rotate_web"
servera.lab.example.com | CHANGED | rc=0 >>
#Ansible: rotate_web
0 0 * * * devops logrotate -f /etc/logrotate.d/httpd
```

Run site.yml, including making sure it prompts to unlock the vaulted file:
```
$ ansible-navigator run -m stdout --pae false site.yml --vault-id @prompt
```

