# **hw_04_smon_arb**

## Задание 1

Установить Prometheus.

Процесс выполнения:

1. Выполняя задание, сверяйтесь с процессом, отражённым в записи лекции.
2. Создайте пользователя prometheus.
3. Скачайте prometheus и в соответствии с лекцией разместите файлы в целевые директории.
4. Создайте сервис как показано на уроке.
5. Проверьте что prometheus запускается, останавливается, перезапускается и отображает статус с помощью systemctl.

Требования к результату:

- Прикрепите к файлу README.md скриншот systemctl status prometheus, где будет написано: prometheus.service — Prometheus Service Netology Lesson 9.4 — [Ваши ФИО]

## Решение 1

Создаем пользователя prometheus
```
sudo useradd --no-create-home --shell /bin/false prometheus
```
Скачиваем и распаковываем
```
sudo wget https://github.com/prometheus/prometheus/releases/download/v2.53.5/prometheus-2.53.5.linux-amd64.tar.gz
sudo tar xvfz prometheus-2.53.5.linux-amd64.tar.gz 
```
Создаем каталоги
```
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
```
Копируем
```
sudo cp ./prometheus promtool /usr/local/bin/
sudo cp -R ./console_libraries/ /etc/prometheus/
sudo cp -R ./consoles/ /etc/prometheus/
sudo cp ./prometheus.yml /etc/prometheus/
```
Передаем права пользователю prometheus
```
sudo chown -R prometheus:prometheus /etc/prometheus/ /var/lib/prometheus/
sudo chown prometheus:prometheus /usr/local/bin/prometheus 
sudo chown prometheus:prometheus /usr/local/bin/promtool 
```
Создаем prometeheus.service
```
sudo nano /etc/systemd/system/prometeheus.service
```
Со следующим содержимым
```
[Unit]
Description=Prometheus Service ARB
After=network.target
[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
--config.file /etc/prometheus/prometheus.yml \
--storage.tsdb.path /var/lib/prometheus/ \
--web.console.templates=/etc/prometheus/consoles \
--web.console.libraries=/etc/prometheus/console_libraries
ExecReload=/bin/kill -HUP $MAINPID Restart=on-failure
[Install]
WantedBy=multi-user.target
```
Добавляем в автозагрузку, запускаем и смотрим статус:
```
sudo systemctl enable prometeheus.service 
sudo systemctl start prometeheus.service
sudo systemctl status prometeheus.service
```
![alt text](image.png)\

## Задание 2

Установить Node Exporter.

Процесс выполнения:

1. Выполняя задание, сверяйтесь с процессом, отражённым в записи лекции.
2. Скачайте node exporter приведённый в презентации и в соответствии с лекцией разместите файлы в целевые директории.
3. Создайте сервис как показано на уроке.
4. Проверьте что node exporter запускается, останавливается, перезапускается и отображает статус с помощью systemctl.


Требования к результату:

- Прикрепите к файлу README.md скриншот systemctl status node-exporter, где будет написано: node-exporter.service — Node Exporter Netology Lesson 9.4 — [Ваши ФИО]

## Решение 2

Скачиваем и распаковываем
```
sudo wget https://github.com/prometheus/node_exporter/releases/download/v1.9.1/node_exporter-1.9.1.linux-amd64.tar.gz
sudo tar xvfz node_exporter-1.9.1.linux-amd64.tar.gz 
```
Создаем каталог, копируем, присваеваем права
```
sudo mkdir /etc/prometheus/node-exporter
sudo cp ./node_exporter /etc/prometheus/node-exporter/
sudo chown prometheus:prometheus /etc/prometheus/node-exporter/node_exporter
```
Создаем node-exporter.service
```
sudo nano /etc/systemd/system/node-exporter.service
```
СО следующим содержимым:
```
[Unit]
Description=Node Exporter Service ARB
After=network.target
[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/etc/prometheus/node-exporter/node_exporter
[Install]
WantedBy=multi-user.target
```
Добавляем в автозагрузку, запускаем и смотрим статус:
```
sudo systemctl enable node-exporter.service
sudo systemctl start node-exporter.service
sudo systemctl status node-exporter.service
```
![alt text](image-1.png)