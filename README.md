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

```
[tesla@fedora homework12]$ ansible nginx -i staging/hosts -m ping
The authenticity of host '[127.0.0.1]:2222 ([127.0.0.1]:2222)' can't be established.
ED25519 key fingerprint is SHA256:S8hPFaWjZTzaSrmi8XTma+SkO0WLd/dTno1TWa21Ggc.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
[WARNING]: Platform linux on host nginx is using the discovered Python interpreter at /usr/libexec/platform-python, but future installation of another Python interpreter could change this.
See https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.
nginx | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": false,
    "ping": "pong"
}
[tesla@fedora homework12]$
```
