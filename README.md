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
3. Создаю inventory
```
[tesla@fedora homework12]$ cat staging/hosts
[web]
nginx ansible_host=127.0.0.1 ansible_port=2222 ansible_private_key_file=.vagrant/machines/nginx/virtualbox/private_key
```
4. Проверяю работоспособность ansible

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
```
5. Создаю файл конфигурации

```
[tesla@fedora homework12]$ cat ansible.cfg
[defaults]
inventory = staging/hosts
remote_user = vagrant
host_key_checking = False
retry_files_enabled = False
[tesla@fedora homework12]$ cat staging/hosts
[web]
nginx ansible_host=127.0.0.1 ansible_port=2222 ansible_private_key_file=.vagrant/machines/nginx/virtualbox/private_key
```
6. Проверяю, что он работает корректно

```
[tesla@fedora homework12]$ ansible nginx -m ping
[WARNING]: Platform linux on host nginx is using the discovered Python interpreter at /usr/libexec/platform-python, but future installation of another Python interpreter could change this.
See https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.
nginx | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": false,
    "ping": "pong"
}
```
7. Пробую поставить epel в host nginx через ad hoc команду, с помощью модуля dnf
```
[tesla@fedora homework12]$ ansible nginx -m dnf -a "name=epel-release state=present" -b
nginx | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": true,
    "msg": "",
    "rc": 0,
    "results": [
        "Installed: oracle-epel-release-el8-1.0-5.el8.x86_64",
        "Installed: yum-utils-4.0.21-4.0.1.el8_5.noarch"
    ]
}
```
8. Обновляю все пакеты на хосте nginx через ad hoc команду, с помощью модуля dnf

```
[tesla@fedora homework12]$ ansible nginx -m dnf -a "name=* state=latest" -b
nginx | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": true,
    "msg": "",
    "rc": 0,
    "results": [
        "Installed: redhat-release-2:8.5-0.8.0.2.el8.x86_64",
        "Installed: libxml2-2.9.7-12.el8_5.x86_64",
        "Installed: systemd-239-51.0.1.el8_5.5.x86_64",
        "Installed: systemd-udev-239-51.0.1.el8_5.5.x86_64",
        "Installed: python3-firewall-0.9.3-7.0.2.el8_5.1.noarch",
        "Installed: glibc-devel-2.28-164.0.5.el8_5.3.x86_64",
        "Installed: dracut-049-191.git20210920.0.2.el8.x86_64",
        "Installed: kernel-tools-4.18.0-348.20.1.el8_5.x86_64",
        "Installed: kernel-headers-4.18.0-348.20.1.el8_5.x86_64",
        "Installed: python3-hawkey-0.63.0-3.0.2.el8.x86_64",
        "Installed: cyrus-sasl-lib-2.1.27-6.el8_5.x86_64",
        "Installed: kernel-uek-devel-5.4.17-2136.306.1.3.el8uek.x86_64",
        "Installed: device-mapper-8:1.02.177-11.el8_5.x86_64",
        "Installed: glibc-headers-2.28-164.0.5.el8_5.3.x86_64",
        "Installed: device-mapper-event-8:1.02.177-11.el8_5.x86_64",
        "Installed: lvm2-8:2.03.12-11.el8_5.x86_64",
        "Installed: kernel-uek-5.4.17-2136.306.1.3.el8uek.x86_64",
        "Installed: systemd-libs-239-51.0.1.el8_5.5.x86_64",
        "Installed: kernel-tools-libs-4.18.0-348.20.1.el8_5.x86_64",
        "Installed: device-mapper-event-libs-8:1.02.177-11.el8_5.x86_64",
        "Installed: linux-firmware-999:20211203-999.9.1.gitb0e898fb.el8.noarch",
        "Installed: openssl-1:1.1.1k-6.el8_5.x86_64",
        "Installed: dracut-network-049-191.git20210920.0.2.el8.x86_64",
        "Installed: firewalld-0.9.3-7.0.2.el8_5.1.noarch",
        "Installed: lvm2-libs-8:2.03.12-11.el8_5.x86_64",
        "Installed: python3-libdnf-0.63.0-3.0.2.el8.x86_64",
        "Installed: dracut-squash-049-191.git20210920.0.2.el8.x86_64",
        "Installed: device-mapper-libs-8:1.02.177-11.el8_5.x86_64",
        "Installed: firewalld-filesystem-0.9.3-7.0.2.el8_5.1.noarch",
        "Installed: glibc-2.28-164.0.5.el8_5.3.x86_64",
        "Installed: oraclelinux-release-8:8.5-1.0.8.el8.x86_64",
        "Installed: systemd-pam-239-51.0.1.el8_5.5.x86_64",
        "Installed: vim-minimal-2:8.0.1763-16.0.1.el8_5.12.x86_64",
        "Installed: oraclelinux-release-el8-1.0-22.el8.x86_64",
        "Installed: libdnf-0.63.0-3.0.2.el8.x86_64",
        "Installed: krb5-libs-1.18.2-14.0.1.el8.x86_64",
        "Installed: libarchive-3.3.3-3.el8_5.x86_64",
        "Installed: openssl-libs-1:1.1.1k-6.el8_5.x86_64",
        "Installed: glibc-common-2.28-164.0.5.el8_5.3.x86_64",
        "Installed: tzdata-2022a-1.el8.noarch",
        "Installed: glibc-langpack-en-2.28-164.0.5.el8_5.3.x86_64",
        "Installed: expat-2.2.5-4.0.1.el8_5.3.x86_64",
        "Removed: libarchive-3.3.3-1.el8.x86_64",
        "Removed: python3-firewall-0.9.3-7.0.2.el8.noarch",
        "Removed: python3-hawkey-0.63.0-3.0.1.el8.x86_64",
        "Removed: python3-libdnf-0.63.0-3.0.1.el8.x86_64",
        "Removed: libdnf-0.63.0-3.0.1.el8.x86_64",
        "Removed: cyrus-sasl-lib-2.1.27-5.el8.x86_64",
        "Removed: redhat-release-2:8.5-0.8.0.1.el8.x86_64",
        "Removed: device-mapper-8:1.02.177-10.el8.x86_64",
        "Removed: device-mapper-event-8:1.02.177-10.el8.x86_64",
        "Removed: device-mapper-event-libs-8:1.02.177-10.el8.x86_64",
        "Removed: device-mapper-libs-8:1.02.177-10.el8.x86_64",
        "Removed: dracut-049-191.git20210920.0.1.el8.x86_64",
        "Removed: dracut-network-049-191.git20210920.0.1.el8.x86_64",
        "Removed: dracut-squash-049-191.git20210920.0.1.el8.x86_64",
        "Removed: systemd-239-51.0.1.el8_5.3.x86_64",
        "Removed: systemd-libs-239-51.0.1.el8_5.3.x86_64",
        "Removed: systemd-pam-239-51.0.1.el8_5.3.x86_64",
        "Removed: systemd-udev-239-51.0.1.el8_5.3.x86_64",
        "Removed: expat-2.2.5-4.el8.x86_64",
        "Removed: tzdata-2021e-1.el8.noarch",
        "Removed: vim-minimal-2:8.0.1763-16.0.1.el8_5.4.x86_64",
        "Removed: firewalld-0.9.3-7.0.2.el8.noarch",
        "Removed: firewalld-filesystem-0.9.3-7.0.2.el8.noarch",
        "Removed: glibc-2.28-164.0.3.el8.x86_64",
        "Removed: glibc-common-2.28-164.0.3.el8.x86_64",
        "Removed: glibc-devel-2.28-164.0.3.el8.x86_64",
        "Removed: glibc-headers-2.28-164.0.3.el8.x86_64",
        "Removed: glibc-langpack-en-2.28-164.0.3.el8.x86_64",
        "Removed: libxml2-2.9.7-9.0.2.el8_4.2.x86_64",
        "Removed: linux-firmware-999:20210617-999.8.git0f66b74b.el8.noarch",
        "Removed: lvm2-8:2.03.12-10.el8.x86_64",
        "Removed: lvm2-libs-8:2.03.12-10.el8.x86_64",
        "Removed: openssl-1:1.1.1k-5.el8_5.x86_64",
        "Removed: openssl-libs-1:1.1.1k-5.el8_5.x86_64",
        "Removed: oraclelinux-release-8:8.5-1.0.7.el8.x86_64",
        "Removed: oraclelinux-release-el8-1.0-21.el8.x86_64",
        "Removed: kernel-headers-4.18.0-348.12.2.el8_5.x86_64",
        "Removed: kernel-tools-4.18.0-348.12.2.el8_5.x86_64",
        "Removed: kernel-tools-libs-4.18.0-348.12.2.el8_5.x86_64",
        "Removed: krb5-libs-1.18.2-14.el8.x86_64"
    ]
}
```
9. Создаю тестовый playbook epel.yml

```
[tesla@fedora homework12]$ cat epel.yml
---
- name: Install EPEL Repo
  hosts: nginx
  become: true
  tasks:
    - name: Install EPEL Repo package from standard repo
      dnf:
        name: epel-release
        state: present
```
10. Проверяю его работоспособность

```
[tesla@fedora homework12]$ ansible-playbook epel.yml

PLAY [Install EPEL Repo] *********************************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************************************
ok: [nginx]

TASK [Install EPEL Repo package from standard repo] ******************************************************************************************************************************************
ok: [nginx]

PLAY RECAP ***********************************************************************************************************************************************************************************
nginx                      : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0


```
11. Удаляю с хоста nginx пакет oracle-epel-release-el8 с помощь модулья dnf

```
[tesla@fedora homework12]$ ansible nginx -m dnf -a "name=oracle-epel-release-el8 state=absent" -b
nginx | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": true,
    "msg": "",
    "rc": 0,
    "results": [
        "Removed: oracle-epel-release-el8-1.0-5.el8.x86_64"
    ]
}
```
12. Еще раз запускаю playbook

```
[tesla@fedora homework12]$ ansible-playbook epel.yml

PLAY [Install EPEL Repo] ************************************************************************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************************************************************************
ok: [nginx]

TASK [Install EPEL Repo package from standard repo] *********************************************************************************************************************************************
changed: [nginx]

PLAY RECAP **************************************************************************************************************************************************************************************
nginx                      : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

```
playbook работает!!!

13. Теперь создаю playbook hw.yml для выполнения домашней работы. Добавляю в него task установки nginx

```
[tesla@fedora homework12]$ cat hw.yml
---
- name: Install EPEL Repo
  hosts: nginx
  become: true
  tasks:
    - name: Install EPEL Repo package from standard repo
      dnf:
        name: epel-release
        state: present
      tags:
       - epel
    - name: Install nginx package from epel repo
      dnf:
        name: nginx
        state: latest
      tags:
       - nginx-package
       - packages
[tesla@fedora homework12]$ ansible-playbook hw.yml --list-tags

playbook: hw.yml

  play #1 (nginx): Install EPEL Repo	TAGS: []
      TASK TAGS: [epel, nginx-package, packages]
```
14. Проверяю работоспособность 

```
[tesla@fedora homework12]$ ansible-playbook hw.yml -t nginx-package

PLAY [Install EPEL Repo] ************************************************************************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************************************************************************
ok: [nginx]

TASK [Install nginx package from epel repo] *****************************************************************************************************************************************************
changed: [nginx]

PLAY RECAP **************************************************************************************************************************************************************************************
nginx                      : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

```
15. Создаю файл staging/host_vars/nginx.yml, в котором определю параметры для хоста nginx

```
tesla@fedora homework12]$ tree -L 3
.
├── 2022-04-23-20-36-44.006-VBoxHeadless-6251.log
├── ansible.cfg
├── hw.yml
├── README.md
├── staging
│   ├── hosts
│   └── host_vars
│       └── nginx.yml
└── Vagrantfile

2 directories, 7 files
[tesla@fedora homework12]$ cat staging/host_vars/nginx.yml
nginx_listen_port: 8080
```
16. Перенесу в него все параметры для хоста

```
[tesla@fedora homework12]$ cat staging/host_vars/nginx.yml
ansible_host: 127.0.0.1
ansible_port: 2222
ansible_private_key_file: .vagrant/machines/nginx/virtualbox/private_key
nginx_listen_port: 8080
```
17. Проверяю, что все корректно.

```
tesla@fedora homework12]$ ansible-inventory --list -i staging/hosts
{
    "_meta": {
        "hostvars": {
            "nginx": {
                "ansible_host": "127.0.0.1",
                "ansible_port": 2222,
                "ansible_private_key_file": ".vagrant/machines/nginx/virtualbox/private_key",
                "nginx_listen_port": 8080
            }
        }
    },
    "all": {
        "children": [
            "ungrouped",
            "web"
        ]
    },
    "web": {
        "hosts": [
            "nginx"
        ]
    }
}
```
```
[tesla@fedora homework12]$ cat hw.yml
---
- name: homework ansible
  hosts: nginx
  become: true

  tasks:
    - name: Install EPEL Repo package from standard repo
      dnf:
        name: epel-release
        state: present
      tags:
       - epel-package
       - packages

    - name: Install nginx package from epel repo
      dnf:
        name: nginx
        state: latest
      notify:
        - restart nginx
      tags:
       - nginx-package
       - packages

    - name: NGINX | Create NGINX config file from template
      template:
        src: templates/nginx.conf.j2
        dest: /etc/nginx/nginx.conf
      notify:
        - reload nginx
      tags:
       - nginx-configuration

  handlers:
    - name: restart nginx
      systemd:
        name: nginx
        state: restarted
        enabled: yes

    - name: reload nginx
      systemd:
        name: nginx
        state: reloaded
```

```
[tesla@fedora homework12]$ ansible-playbook hw.yml

PLAY [homework ansible] *************************************************************************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************************************************************************
ok: [nginx]

TASK [Install EPEL Repo package from standard repo] *********************************************************************************************************************************************
changed: [nginx]

TASK [Install nginx package from epel repo] *****************************************************************************************************************************************************
changed: [nginx]

TASK [NGINX | Create NGINX config file from template] *******************************************************************************************************************************************
changed: [nginx]

RUNNING HANDLER [restart nginx] *****************************************************************************************************************************************************************
changed: [nginx]

RUNNING HANDLER [reload nginx] ******************************************************************************************************************************************************************
changed: [nginx]

PLAY RECAP **************************************************************************************************************************************************************************************
nginx                      : ok=6    changed=5    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0


```
![Снимок экрана 2022-04-24 в 11 02 51](https://user-images.githubusercontent.com/98701086/164966485-1764af46-72fe-499e-a767-e9a772cb5c65.png)
