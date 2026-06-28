# Домашнее задание к занятию "`Ansible. Часть 2`" - `Селезнев Николай`

### Задание 1 Базовые плейбуки

### Плейбук 1: Скачивание и распаковка архива
Файл: `p1_download_unarchive.yml`

Выполнение:
ansible-playbook -i localhost, -c local p1_download_unarchive.yml

PLAY RECAP *********************************************************************
localhost                  : ok=3    changed=3    unreachable=0    failed=0

Плейбук 2: Установка и запуск tuned
Файл: p2_tuned.yml
ansible-playbook -i localhost, -c local --become --ask-become-pass p2_tuned.yml

Результат:
TASK [Install tuned package] ***************************************************
changed: [localhost]
TASK [Ensure tuned service is started and enabled] *****************************
ok: [localhost]

Плейбук 3: Изменение motd с переменной
Файл: p3_motd.yml (первая версия)

Переменная welcome_message задана в плейбуке. Результат:
$ cat /etc/motd
Welcome to Netology DevOps lab!

---

### Задание 2 Модификация motd с фактами

Файл: p3_motd.yml (обновлённая версия)

Шаблон в плейбуке использует ansible_hostname и ansible_default_ipv4.address. Результат:
Hostname: ubuntu
IP Address: 10.0.2.15
Have a nice day, sysadmin!

---

### Задание 3  Роль Apache с кастомной страницей

Роль размещена в roles/apache/. Плейбук p3_apache.yml подключает роль.

Задачи роли:
-Установка Apache
-Конфигурация ServerName через шаблон servername.conf.j2
-Создание index.html из шаблона index.html.j2 с системными фактами (CPU, RAM, HDD, IP)
-Открытие порта 80 в UFW
-Запуск и включение Apache
-Проверка доступности модулем uri (код 200)

Результат проверки:
$ curl http://localhost
<!DOCTYPE html>
<html>
...
  <li><strong>Hostname:</strong> ubuntu</li>
  <li><strong>CPU:</strong> 11th Gen Intel(R) Core(TM) i7-11700KF @ 3.60GHz</li>
  <li><strong>RAM:</strong> 3914 MB</li>
  <li><strong>First HDD size:</strong> 50.00 GB</li>
  <li><strong>IP Address:</strong> 10.0.2.15</li>
...
</html>

Все плейбуки и роль работают локально (ansible_connection=local).
