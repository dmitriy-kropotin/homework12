# homework12
1. Установка ansible на хостовую машину
```
[tesla@fedora ~]$ sudo dnf install ansible
```
2. Создаю виртуальную машину для домашней работы по приложенному Vagrantfile
```
[tesla@fedora homework12]$ vagrant up
....
[tesla@fedora homework12]$ vagrant ssh-config
Host nginx
  HostName 127.0.0.1
  User vagrant
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile /home/tesla/homework12/.vagrant/machines/nginx/virtualbox/private_key
  IdentitiesOnly yes
  LogLevel FATAL

[tesla@fedora homework12]$
```
