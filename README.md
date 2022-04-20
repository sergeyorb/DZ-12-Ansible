# Домашнее задание-Ansible
<ol>
  <li>Создать стенд для выполнения домашнего задания</li>
  <li>Установка и первоначальная настройка Ansible</li>
  <li>Создание Playbook</li>
</ol>

# Создать стенд для выполнения домашнего задания
<ul>
<p>Для выполнения ДЗ были развёрнуты три VM 
<p>ans-server - VM для развёртывания Ansible
<p>ans-client-01 - VM для клиентов Ansible
<p>ans-client-02 - VM для клиентов Ansible
<p>Использовал образ CentOS 7 
<p>Разворачивал вручную, но можно развернуть и используя приложенный Vagrant файл  
</ul>

# Установка и первоначальная настройка Ansible
<ul>
<li>Установил Ansible на сервер ans-server</li>  
<p>yum install epel-release
<p>yum install ansible
<li>Отредактировал config file = /etc/ansible/ansible.cfg</li>  
<p>inventory      = /root/ansible/hosts
<p>host_key_checking = false
<li>Создать ssh key</li>
<p>ssh-keygen
<li>Скопировать ssh key на клиентскую VM</li>
<p>Скопировал ssh key в директорию клиенской VM /root/.ssh/authorized_keys  
<li>Создал inventory file в директории /root/ansible/hosts</li>
<li>Отредактировал файл hosts</li>
<p>[web1]
<p>ans-client-01

<p>[web2]
<p>ans-client-02

<p>[all_groups:children]
<p>web1
<p>web2  
<li>Создал Групповые пременные</li>
<p>Создал каталог group_vars
<p>Создал файлы с названием как у групп
<p>- web1 
<p>  ansible_host: 192.168.56.104
<p>  ansible_user: root
<p>  ansible_ssh_private_key_file: /root/.ssh/client01
<li>Проверил доступность управляемых хостов</li>
<p>  [root@ans-server ansible]# ansible all -i hosts -m ping
<p>ans-client-01 | SUCCESS => {
<p>    "ansible_facts": {
<p>        "discovered_interpreter_python": "/usr/bin/python"
<p>    },
<p>    "changed": false,
<p>   "ping": "pong"
<p>}
<p>ans-client-02 | SUCCESS => {
<p>    "ansible_facts": {
<p>        "discovered_interpreter_python": "/usr/bin/python"
<p>    },
<p>    "changed": false,
<p>    "ping": "pong"
<p>}
</ul>  
#Создание Playbook
<ul>
<li>Создал Playbook с именем epel.yml</li>
mkdir epel.yml в директории /root/ansible
<li>Отредактровал файл epel.yml</li>
- name: Install EPEL Repo
  hosts: web1
  become: true
  tasks:
    - name: Install EPEL Repo package from standart repo
      yum:
        name: epel-release
        state: present 
<li>Проверил работу Playbook</li>
ansible-playbook epel.yml
<li>Отредактировал epel.yml добавив строки</li>
- name: NGINX | Install and configure NGINX
  hosts: web1
  become: true
  vars:
    nginx_listen_port: 8080

  tasks:
    - name: NGINX | Install EPEL Repo package from standart repo
      yum:
        name: epel-release
        state: present
      tags:
        - epel-package
        - packages

    - name: NGINX | Install NGINX package from EPEL Repo
      yum:
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
<li>Создал файл шаблона</li>
<p>В директории /root/ansible/templates создал файл nginx.conf.j2     
<li>Отредактировал файл nginx.conf.j2 добавив следующие строки</li>
# {{ ansible_managed }}
events {
    worker_connections 1024;
}

http {
    server {
        listen       {{ nginx_listen_port }} default_server;
        server_name  default_server;
        root         /usr/share/nginx/html;

        location / {
        }
    }
}  
<li>Переопределил переменную nginx_listen_port</li>
Добавил в файл web1 с глобальными переменными из директории /root/ansible/group_vars строку
nginx_listen_port: 8080  
<li>Запустил Playbook на выполнение командой</li>
ansible-playbook -i hosts epel.yml  
</ul>
