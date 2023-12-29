# `Домашнее задание к занятию "ELK"` - `Мешочков Александр SYS-22`



1. [Описание домашнего задания к занятию «ELKх»](https://github.com/netology-code/sdb-homeworks/blob/main/11-03.md)

---
## Дополнительные ресурсы

При выполнении задания используйте дополнительные ресурсы:
- [docker-compose elasticsearch + kibana](11-03/docker-compose.yaml);
- [поднимаем elk в docker](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/docker.html);
- [поднимаем elk в docker с filebeat и docker-логами](https://www.sarulabs.com/post/5/2019-08-12/sending-docker-logs-to-elasticsearch-and-kibana-with-filebeat.html);
- [конфигурируем logstash](https://www.elastic.co/guide/en/logstash/7.17/configuration.html);
- [плагины filter для logstash](https://www.elastic.co/guide/en/logstash/current/filter-plugins.html);
- [конфигурируем filebeat](https://www.elastic.co/guide/en/beats/libbeat/5.3/config-file-format.html);
- [привязываем индексы из elastic в kibana](https://www.elastic.co/guide/en/kibana/7.17/index-patterns.html);
- [как просматривать логи в kibana](https://www.elastic.co/guide/en/kibana/current/discover.html);
- [решение ошибки increase vm.max_map_count elasticsearch](https://stackoverflow.com/questions/42889241/how-to-increase-vm-max-map-count).
---

 ### Задание 1: 
<details>
   <summary> Задание 1: Elasticsearch  </summary>
  
Установите и запустите Elasticsearch, после чего поменяйте параметр cluster_name на случайный. 

*Приведите скриншот команды 'curl -X GET 'localhost:9200/_cluster/health?pretty', сделанной на сервере с установленным Elasticsearch. Где будет виден нестандартный cluster_name*.
</details>

### Ответ 1
<details>
  <summary>Ответ 1: </summary>

![Скриншот-1](<img src = "image/git/11.03/1.jpg" width = 100%>)
</details>


--------

 ### Задание 2: 
<details>
   <summary> Задание 2: Kibana. </summary>

Установите и запустите Kibana.

*Приведите скриншот интерфейса Kibana на странице http://<ip вашего сервера>:5601/app/dev_tools#/console, где будет выполнен запрос GET /_cluster/health?pretty*.

</details>

 ### Ответ 2: 
<details>
   <summary> Ответ 2: </summary>

![Скриншот-2](<img src = "image/git/11.03/2.jpg" width = 100%>)

</details>


--------


 ### Задание 3: 
<details>
   <summary> Задание 3: Logstash </summary>
  
Установите и запустите Logstash и Nginx. С помощью Logstash отправьте access-лог Nginx в Elasticsearch. 

*Приведите скриншот интерфейса Kibana, на котором видны логи Nginx.*



</details>

 ### Ответ 3: 
<details>
   <summary> Ответ 3: </summary>

![Скриншот-3](<img src = "image/git/11.03/3.jpg" width = 100%>)

</details>


--------


--------


 ### Задание 4: 
<details>
   <summary> Задание 4:Filebeat </summary>
  
Установите и запустите Filebeat. Переключите поставку логов Nginx с Logstash на Filebeat. 

*Приведите скриншот интерфейса Kibana, на котором видны логи Nginx, которые были отправлены через Filebeat.*



</details>

 ### Ответ 4: 
<details>
   <summary> Ответ 4: </summary>

![Скриншот-4](<img src = "image/git/11.03/4.jpg" width = 100%>)

</details>

----