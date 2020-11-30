# Ack abonent accept service 
Микросервис формирования квитанций о получении пакета абонентом. 
Тело пакета формируется с помощью шаблона в формате https://freemarker.apache.org/

### Описание настроек для application.properties
##### 1 Настройки логики приложения
Позволяет настроить логику в случае отсуствия шаблона для формирования квитанции в схеме

	skip-empty-router-template - boolean (true/false). true - пропускать пакеты, false - генерировать ошибку и ждать появления шаблона в схеме
##### 2 Настройки подключения к сервису DFS
	dfs-service.server - string Адрес сервера (http://localhost:8080)
	cache.dfs-service.timeout  - int Время жизни кеша в секундах (120)
	cache.dfs-service.size - int максимальный размер кеша (работает как LRU)

##### 3 Настройки подключения для консьюмера кафки

    spring.kafka.bootstrap-servers - Сервера (PLAINTEXT://localhost:8081,PLAINTEXT://localhost:8082)
	kafka.output.topic-name - string исходящий топик куда будет отправлен пакет сформированный по шаблону
	kafka.input.topic-name - string входящий топик (aismv-my-topic)
	kafka.input.concurrency - int количество поток для слушателя топика (1), приложение используется стандартную схему получения разделов, поэтому убедитесь что количество разделов топика кратно количеству консьюмеров или количеству поток
	kafka.input.groupId - string id группы сшателей (aismv-my-group-id)
	kafka.input.clientId - string id клиента которые будет использоваться этой группой потребителей (aismv-my-client-id)
	kafka.input.pollTimeout - int сколько времи будет опрашиваться
	kafka.input.max.poll.interval.ms - int максимальный интервал опроса 

##### 4 Настройка полити обработки ошибок
Вслучае ошибки во время выполнения сервис будет пробывать выполнить свою бизнеслогику пока недостигрент количества повторов больше чем **reply.max-attempts** или ошибка не будет исправленна.

	reply.max-attempts - int количество попыток для повторной попытки (рекомендуется 2147483000)
	reply.backoff - int время задержки между повторами
	
#### 5 Настроки для http клиента
Клиент используется для взаимодействия с сервисами платформы по http

Описание полей настройки совпадает с документацией apache https://hc.apache.org/httpcomponents-client-4.5.x/httpclient/apidocs/ 

    restTemplate.maxTotalPooling
    restTemplate.defaultMaxPerRoute
    restTemplate.requestTimeout 
    
Описание полей настройки совпадает с документацией apache https://hc.apache.org/httpcomponents-client-4.5.x/httpclient/apidocs/

    restTemplate.poolTimeout
    restTemplate.connectionTimeout
 
