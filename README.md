# Elasticsearch, logstash, kibana + filebeat

Заготовка для elk-стека в докере.

---

# Инструкция

Проект содержит 2 docker-compose файла.

1) [docker-compose.certs.yml](./docker-compose.certs.yml) - это специальный docker-compose
для создания сертификатов. Он создаёт их на основе конфига [instance.yml](./tmp/cert_config/instance.yml).
Подробнее о заполнении этого конфига можно посмотреть в [официальной документации](https://www.elastic.co/guide/en/elasticsearch/reference/current/certutil.html).
2) [docker-compose.yml](./docker-compose.yml) - это основной docker-compose с ELK.
Также он содержит в себе `filebeat` в качестве образца.


## Запуск

### 1. Создайте сертификаты и ключи: `docker-compose -f docker-compose.certs.yml up`
Данная команда создаёт почти все необходимые сертификаты и ключи для 
настройки безопасного шифрованного взаимодействия микросервисов друг с другом.

### 1.1 Создайте валидный ключ для интеграции logstash + filebeats
Формат ssl ключа для logstash нужно переделать в формат `PKCS#8`.

Сделайте это командой:
`openssl pkcs8 -in ./tmp/certs/logstash/logstash.key -topk8 -nocrypt -out ./tmp/certs/logstash/logstash.pkcs8.key`

Выдайте права на запись, если это необходимо `sudo chmod -R 777 tmp`

### 2. Создайте файл `.env` на примере [.env.sample](./.env.sample)
Заполните все переменные окружения по своему усмотрению

### 3. Запустите ELK `docker-compose up` 
После запуска зайдите в                                                                                                                