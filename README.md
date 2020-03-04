# otus_21
Фильтрация трафика

Сценарии iptables
1) реализовать knocking port
- centralRouter может попасть на ssh inetrRouter через knock скрипт
пример в материалах
2) добавить inetRouter2, который виден(маршрутизируется (host-only тип сети для виртуалки)) с хоста или форвардится порт через локалхост
3) запустить nginx на centralServer
4) пробросить 80й порт на inetRouter2 8080
5) дефолт в инет оставить через inetRouter

* реализовать проход на 80й порт без маскарадинга

________________________________________________________________

Полезные ссылки, которые помогли
https://wiki.archlinux.org/index.php/Port_knocking
https://otus.ru/nest/post/267/

# Проверка:

# 1) 
Подключиться к centralRouter

```
vagrant ssh centralRouter
```
С centralRouter

```
cd /vagrant/
/vagrant/knock.sh 192.168.255.1 8881 7777 9991
```

После этого можно подключиться к inetRouter

```
ssh 192.168.255.1
```
Are you sure you want to continue connecting (yes/no)? ```yes```

Пароль ```vagrant```

![Img_alt](https://github.com/Edo1993/otus_21/blob/master/111.png)

# 2-4) 

```http://192.168.11.171:8080/```

![Img_alt](https://github.com/Edo1993/otus_21/blob/master/112.png)

# 5) 

![Img_alt](https://github.com/Edo1993/otus_21/blob/master/113.png)
