Ошибка заключается в том, что бизнес хочет получать обязательно **фамилию** автора, а интеграционный сервис выдаёт только 
* Название презентации
* email автора
* Дату рождения автора
* Дату выступления на конференции

Swagger интеграционного сервиса: \
https://app.swaggerhub.com/apis/GAGRSKAYAAANASTASIA/conference-server_api/1.0.0

Чтобы фамилия автора появилась, в файле service_main.py 
* добавила переменную last_name в класс Presentation_With_Author
* поправила вызов класса