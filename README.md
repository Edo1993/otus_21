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



После запуска стенда из Vagrantfile на хоствой машине необходимо перейти на http://127.0.0.1:8080, где будет доступна стандартная страница nginx.
![Img_alt](https://github.com/Edo1993/otus_21/blob/master/201.png)
