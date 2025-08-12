# **ДЗ_2_sflt_arb**


## Задание 1

Что нужно сделать:

1. Запустите два simple python сервера на своей виртуальной машине на разных портах.
2. Установите и настройте HAProxy, воспользуйтесь материалами к лекции по [ссылке](https://github.com/netology-code/sflt-homeworks/blob/main/2)
3. Настройте балансировку Round-robin на 4 уровне.

На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy.

## Решение 1

Запускаем и проверяем два simple python сервера на портах 8888 и 9999:

![alt text](image.png)
![alt text](image-1.png)

Устанавливаем, настраиваем и перезапускаепм HAProxy:
```
sudo apt install haproxy
sudo nano /etc/haproxy/haproxy.cfg 
```
Конфигурационный файл /etc/haproxy/haproxy.cfg[here](./arch/haproxy.cfg)

Проверяем работу:

![alt text](image-3.png)

Смотрим статистику:

![alt text](image-2.png)