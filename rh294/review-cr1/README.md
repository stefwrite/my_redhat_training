#NOTES

Running the users.yml playbook, then checking that users are present:
```
% ansible-navigator run -m stdout users.yml
% ansible dev -m ansible.builtin.command -a "tail -5 /etc/passwd"
```

Running the packages.yml playbook, then checking that packages are installed:
```
% ansible-navigator run -m stdout packages.yml
% ansible dev -m ansible.builtin.command -a "dnf info httpd mariadb-server"
% ansible dev -m ansible.builtin.command -a "dnf info redis"
```

Getting ansible\_facts value for total swap:
```
% ansible localhost -m ansible.builtin.setup | grep swap
```

