# Домашнее задание к занятию "10.03. Grafana"

## Обязательные задания

### Задание 1
Используя директорию [help](./help) внутри данного домашнего задания - запустите связку prometheus-grafana.

Зайдите в веб-интерфейс графана, используя авторизационные данные, указанные в манифесте docker-compose.

Подключите поднятый вами prometheus как источник данных.

Решение домашнего задания - скриншот веб-интерфейса grafana со списком подключенных Datasource.

<img width="1063" alt="1" src="https://user-images.githubusercontent.com/99620296/209447933-64b5f892-c158-494d-ab4a-1a9330c2cb6f.png">

## Задание 2
Изучите самостоятельно ресурсы:
- [PromQL query to find CPU and memory](https://stackoverflow.com/questions/62770744/promql-query-to-find-cpu-and-memory-used-for-the-last-week)
- [PromQL tutorial](https://valyala.medium.com/promql-tutorial-for-beginners-9ab455142085)
- [Understanding Prometheus CPU metrics](https://www.robustperception.io/understanding-machine-cpu-usage)

Создайте Dashboard и в ней создайте следующие Panels:
- Утилизация CPU для nodeexporter (в процентах, 100-idle)
- CPULA 1/5/15
- Количество свободной оперативной памяти
- Количество места на файловой системе

Для решения данного ДЗ приведите promql запросы для выдачи этих метрик, а также скриншот получившейся Dashboard.

1) 100 - (avg by (instance, mode)(irate(node_cpu_seconds_total{instance="nodeexporter:9100", mode="idle"}[5m])) * 100)
2) avg by (instance)(irate(node_cpu_seconds_total{instance="nodeexporter:9100"}[1m])) * 100
3) 100 * (1-((avg_over_time(node_memory_MemFree_bytes[5m]) + avg_over_time(node_memory_Cached_bytes[5m]) + avg_over_time(node_memory_Buffers_bytes[5m])) / avg_over_time(node_memory_MemTotal_bytes[5m])))
4) sum(node_filesystem_free_bytes{fstype=~"ext4", mountpoint=~"/mnt|/run/desktop/mnt|/usr/lib/wsl|/var/lib"})

<img width="937" alt="2" src="https://user-images.githubusercontent.com/99620296/209447967-466503d1-4ca7-42eb-921a-727dfcf53e39.png">

## Задание 3
Создайте для каждой Dashboard подходящее правило alert (можно обратиться к первой лекции в блоке "Мониторинг").

Для решения ДЗ - приведите скриншот вашей итоговой Dashboard.

<img width="939" alt="3" src="https://user-images.githubusercontent.com/99620296/209447971-8e7b17a3-364c-453e-a750-3b68d6fe0725.png">


## Задание 4
Сохраните ваш Dashboard.

Для этого перейдите в настройки Dashboard, выберите в боковом меню "JSON MODEL".

Далее скопируйте отображаемое json-содержимое в отдельный файл и сохраните его.

В решении задания - приведите листинг этого файла.


---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
