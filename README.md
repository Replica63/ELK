# Домашнее задание к занятию "`ELK`" - `Копаческу Владимир`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1 Elasticsearch
Установите и запустите Elasticsearch, после чего поменяйте параметр cluster_name на случайный.

Приведите скриншот команды 'curl -X GET 'localhost:9200/_cluster/health?pretty', сделанной на сервере с установленным Elasticsearch. Где будет виден нестандартный cluster_name.

---
### Решение 1
Команды:
1.	sudo apt-get update && sudo apt-get install gnupg apt-transport-https
2.	wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add –
3.	echo "deb [trusted=yes] https://mirror.yandex.ru/mirrors/elastic/7/ stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
4.	sudo apt update
5.	sudo apt install elasticsearch
6.	sudo systemctl daemon-reload
7.	sudo systemctl enable elasticsearch.service
8.	sudo nano /etc/elasticsearch/elasticsearch.yml
9.	curl 'localhost:9200/_cluster/health?pretty'


![alt text](https://github.com/Replica63/ELK/blob/main/img/1.png)
![alt text](https://github.com/Replica63/ELK/blob/main/img/2.png)
---
### Задание 2 Kibana
Установите и запустите Kibana.

Приведите скриншот интерфейса Kibana на странице http://<ip вашего сервера>:5601/app/dev_tools#/console, где будет выполнен запрос GET /_cluster/health?pretty.
---

### Решение 2
Команды:
1.	sudo apt install kibana
2.	sudo systemctl daemon-reload
3.	sudo systemctl enable kibana.service
4.	sudo systemctl start kibana.service
5.	sudo nano /etc/kibana/kibana.yml
6.	server.host: "0.0.0.0"
7.	sudo systemctl restart kibana

![alt text](https://github.com/Replica63/ELK/blob/main/img/3.png)


---

### Задание 3
Установите и запустите Logstash и Nginx. С помощью Logstash отправьте access-лог Nginx в Elasticsearch.

Приведите скриншот интерфейса Kibana, на котором видны логи Nginx.
### Решение 3
![alt text](https://github.com/Replica63/ELK/blob/main/img/4.png)
![alt text](https://github.com/Replica63/ELK/blob/main/img/5.png)

Нужно создать конфигурационный файл logstash.conf
```
/etc/logstash/conf.d/logstash.conf
```

```
input {
  syslog {
    port => 6000
    tags => "nginx"
  }
}

output {
  elasticsearch {
    hosts => ["http://127.0.0.1:9200"]
    index => "nginx-index"
  }
}
```
![alt text](https://github.com/Replica63/ELK/blob/main/img/6.png)

Логи:

![alt text](https://github.com/Replica63/ELK/blob/main/img/7.png)
![alt text](https://github.com/Replica63/ELK/blob/main/img/8.png)

---
### Задание 4 Filebeat.
Установите и запустите Filebeat. Переключите поставку логов Nginx с Logstash на Filebeat.

Приведите скриншот интерфейса Kibana, на котором видны логи Nginx, которые были отправлены через Filebeat.
### Решение 4
![alt text](https://github.com/Replica63/ELK/blob/main/img/8.png)
