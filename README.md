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

На хостовой машине должен быть свободен порт 8080. Предполагается, что стандартный ip-адрес eth0 (по умолчанию public в vagrant) - 10.0.2.15.

# Проверка:

Подключиться к centralRouter

```
vagrant ssh centralRouter
```
С centralRouter

```
for x in 8881 7777 9991; do sudo nmap -Pn --host_timeout 100 --max-retries 0 -p $x 192.168.255.1; done
```
После этого можно подключиться к inetRouter (пароль: vagrant)

```
ssh vagrant@192.168.255.1
```

После запуска стенда из Vagrantfile на хоствой машине необходимо перейти на http://127.0.0.1:8080, где будет доступна стандартная страница nginx.
![Img_alt](https://github.com/Edo1993/otus_21/blob/master/201.png)
