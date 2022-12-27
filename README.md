<h1 align='center'>Автоматизация развёртывания Asterisk + FreePBX на CentOS 8</h1>

<p>
    <strong>Шаг 1. </strong> Создание playbook для запуска роли
</p>
<p><i>Пример:</i></p>
<pre>
  ---
  - name: Installing Asterisk + FreePBX
    hosts: asterisk 
    become: true <br>
    roles: 
      - ansible-role-asterisk 

</pre>
<p>
    <strong>Шаг 2. </strong> Склонировать роль в дирректорию с playbook:
</p>

  <pre>git clone https://github.com/NewErr0r/ansible-role-asterisk.git</pre>

<p>

<p>
    <strong>Список переопределяемых переменных для playbook. </strong>
</p>
<pre>
#System preparation
time_zone: 'Europe/Moscow'
ansible_ssh_host: '192.168.0.105'<br>
#MariaDB
mariadb_root_password: 'P@ssw0rd'<br>
#Asterisk
url_asterisk: 'http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-16-current.tar.gz'
path_download: '/root'<br>
#FreePBX
url_freepbx: 'http://mirror.freepbx.org/modules/packages/freepbx/freepbx-15.0-latest.tgz'
download_path: '/root'
</pre>

<p>
    <strong>Шаг 3. </strong> Запуск playbook:
</p>

  <pre>ansible-playbook -i inventory/hosts playbook.yml</pre>

<p>

<p>
    <strong>Шаг 4. </strong> Запускаем браузер и вводим адрес http://'IP-адрес сервера' — должна открываться страница конфигурирования FreePBX. Задаем настройки:
</p>
<p>
<i>достаточно указать логин и пароль для пользователя, под которым мы будем заходить в панель управления FreePBX и email адрес</i>
</p>

<p>
После входим в панель администратора под созданной учетной записью. Система нас запросит региональные настройки:
</p>

<p>
    <strong>Шаг 5. </strong> Запускаем браузер и вводим адрес http://'IP-адрес сервера' — должна открываться страница конфигурирования FreePBX. Задаем настройки:
</p>
<p>
Подключаемся к серверу по SSH открываем конфигурационный файл:
</p>
<pre>
vi /etc/asterisk/manager.conf
</pre>
<p>
Находим строки:
</p>
<pre>
#include manager_additional.conf
#include manager_custom.conf
</pre>
<p>
... и меняем их на:
</p>
<pre>
;include manager_additional.conf
;include manager_custom.conf
</pre>
<p>
Перезапускаем сервис Asterisk:
</p>
<pre>
systemctl restart asterisk
</pre>
