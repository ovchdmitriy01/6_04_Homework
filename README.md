# Домашнее задание к занятию «Docker. Часть 2»

### Задание 1

**Напишите ответ в свободной форме, не больше одного абзаца текста.**

Установите Docker Compose и опишите, для чего он нужен и как может улучшить вашу жизнь.
Docker Compose — входит в состав Docker. С помощью Docker Compose можно создать один YAML-файл для определения служб и с помощью одной команды запустить или остановить все, что нужно при развертывании многоконтейнерных приложений.

<a href="https://ibb.co/myx68vp"><img src="https://i.ibb.co/wQvMsJt/6-4-1.png" alt="6-4-1" border="0"></a>

---

### Задание 2 

**Выполните действия и приложите текст конфига на этом этапе.** 

Создайте файл docker-compose.yml и внесите туда первичные настройки: 

 * version;
 * services;
 * networks.

При выполнении задания используйте подсеть 172.22.0.0.
Ваша подсеть должна называться: <ваши фамилия и инициалы>-my-netology-hw.

<a href="https://ibb.co/bWTS223"><img src="https://i.ibb.co/X21h334/6-4-2.png" alt="6-4-2" border="0"></a>

---

### Задание 3 

**Выполните действия и приложите текст конфига текущего сервиса:** 

1. Установите PostgreSQL с именем контейнера <ваши фамилия и инициалы>-netology-db. 
2. Предсоздайте БД <ваши фамилия и инициалы>-db.
3. Задайте пароль пользователя postgres, как <ваши фамилия и инициалы>12!3!!
4. Пример названия контейнера: ivanovii-netology-db.
5. Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.

<a href="https://ibb.co/gw03Dt0"><img src="https://i.ibb.co/CvjznVj/6-4-3.png" alt="6-4-3" border="0"></a>

---

### Задание 4 

**Выполните действия:**

1. Установите pgAdmin с именем контейнера <ваши фамилия и инициалы>-pgadmin. 
2. Задайте логин администратора pgAdmin <ваши фамилия и инициалы>@ilove-netology.com и пароль на выбор.
3. Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.
4. Прокиньте на 80 порт контейнера порт 61231.

В качестве решения приложите:

* текст конфига текущего сервиса;
* скриншот админки pgAdmin.

<a href="https://ibb.co/TBKbkwH"><img src="https://i.ibb.co/MD8GsgR/6-4-4.png" alt="6-4-4" border="0"></a>
<a href="https://ibb.co/61gxGxS"><img src="https://i.ibb.co/ZYWPvPr/6-4-5.png" alt="6-4-5" border="0"></a>

---

### Задание 5 

**Выполните действия и приложите текст конфига текущего сервиса:** 

1. Установите Zabbix Server с именем контейнера <ваши фамилия и инициалы>-zabbix-netology. 
2. Настройте его подключение к вашему СУБД.
3. Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.

<a href="https://ibb.co/WnrjqWK"><img src="https://i.ibb.co/fM3LBY0/6-4-6.png" alt="6-4-6" border="0"></a>

---

### Задание 6

**Выполните действия и приложите текст конфига текущего сервиса:** 

1. Установите Zabbix Frontend с именем контейнера <ваши фамилия и инициалы>-netology-zabbix-frontend. 
2. Настройте его подключение к вашему СУБД.
3. Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.

<a href="https://ibb.co/m8BVLcV"><img src="https://i.ibb.co/xHstp7t/6-4-8.png" alt="6-4-8" border="0"></a>

---

### Задание 7 

**Выполните действия.**

Настройте линки, чтобы контейнеры запускались только в момент, когда запущены контейнеры, от которых они зависят.

В качестве решения приложите:

* текст конфига **целиком**;
* скриншот команды docker ps;
* скриншот авторизации в админке Zabbix.
<details>  
version: '3.8'

services:
 ovchinnikovda-netology-db:
  image: postgres:latest #Образ, который будем использовать
  container_name: ovchinnikov-netology-db #Имя, которым будет называться контейнер
  ports: #Порты, которые пробрасываем с докер сервера внутрь контейнера
    - 5432:5432
  volumes: #Папка, которую пробросим с докер сервера внутрь контейнера
    - ./pg_data:/var/lib/postgresql/data/pgdata
  environment: #Переменные среды
    POSTGRES_PASSWORD: ovchinnikovda12!3!! #Задаем пароль от пользователя postgres
    POSTGRES_DB: ovchinnikovda_db #БД, которая будет создана
    PGDATA: /var/lib/postgresql/data/pgdata #Путь внутри контейнера, гдебудет папка pgdata
  networks:
    ovchinnikovda-my-netology-hw:
      ipv4_address: 172.22.0.2
  restart: always #Режим перезапуска контейнера. Контейнер всегда будет перезапускаться.


 pgadmin:
  image: dpage/pgadmin4
  container_name: ovchinnikovda-pgadmin
  environment:
    PGADMIN_DEFAULT_EMAIL: ovchinnikovda@ilove-netology.com
    PGADMIN_DEFAULT_PASSWORD: ovchinnikovda12!3!!
  ports:
    - 61231:80
  networks:
    ovchinnikovda-my-netology-hw:
      ipv4_address: 172.22.0.3
  restart: always


 zabbix-server:
  image: zabbix/zabbix-server-pgsql
  links:
    - ovchinnikovda-netology-db
  container_name: ovchinnikov-zabbix-db
  environment:
    DB_SERVER_HOST: '172.22.0.2'
    POSTGRES_USER: postgres
    POSTGRES_PASSWORD: ovchinnikovda12!3!!
  ports:
    - 10051:10051
  networks:
    ovchinnikovda-my-netology-hw:
      ipv4_address: 172.22.0.4
  restart: always


 zabbix_wgui:
  image: zabbix/zabbix-web-apache-pgsql
  links:
    - ovchinnikovda-netology-db
    - pgadmin
    - zabbix-server
  container_name: ovchinnikovda-netology-zabbix-frontend
  environment:
    DB_SERVER_HOST: '172.22.0.2'
    POSTGRES_USER: postgres
    POSTGRES_PASSWORD: ovchinnikovda12!3!!
    ZBX_SERVER_HOST: zabbix_wgui
    PHP_TZ: Europe/Moscow
  ports:
   - 80:8080
   - 443:8443
  networks:
    ovchinnikovda-my-netology-hw:
      ipv4_address: 172.22.0.5
  restart: always




networks:
 ovchinnikovda-my-netology-hw:
  driver: bridge
  ipam:
    config:
    - subnet: 172.22.0.0/24


</details>

<a href="https://ibb.co/d4DKTfF"><img src="https://i.ibb.co/VYTB1LR/6-4-7.png" alt="6-4-7" border="0"></a>
<a href="https://ibb.co/0X45Mqr"><img src="https://i.ibb.co/thjRsBD/6-4-7z.png" alt="6-4-7z" border="0"></a>

---

### Задание 8 

**Выполните действия:** 

1. Убейте все контейнеры и потом удалите их.
Приложите скриншот консоли с проделанными действиями.
<a href="https://ibb.co/sHkBMvq"><img src="https://i.ibb.co/k1kdWgX/6-4-9.png" alt="6-4-9" border="0"></a>
