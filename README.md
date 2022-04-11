# Домашнее задание-Ansible
<ol>
  <li>Создать стенд для выполнения домашнего задания</li>
  <li>Установка и первоначальная настройка Ansible</li>
  
</ol>

# Создать стенд для выполнения домашнего задания
<ul>
<p>Для выполнения ДЗ были развёрнуты три VM 
<p>ans-server - VM для развёртывания Ansible
<p>ans-client-01 - VM для клиентов Ansible
<p>ans-client-02 - VM для клиентов Ansible
<p>Использовал образ CentOS 7    
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
<li></li>  
</ul>  
