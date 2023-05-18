# Контейнерная архитектура

```plantuml
@startuml C4_Containers
!define C4_CONTAINER_NO_COMPONENT true
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/v2.3.0/C4_Container.puml
LAYOUT_WITH_LEGEND()
LAYOUT_LANDSCAPE()

title Контейнерная диаграмма программной системы
Person(customer, "Слушатель конференции")
Person(user, "Докладчик")
Person(sponsor, "Спонсор", "Организация, финансирующая конференцию")
System_Boundary(conference, "helloconf.mts.ru") {

    Person(organizer, "Организатор", "Организатор конференции")
        package "Работа с докладчиками" {
        Container(workflow, "Работа с докладчиками", "java", "Включает функциональность для подачи докладов, обратной связи с докладчиками и управления докладами")
        Container(workflowAPI, "REST API", "java", "Функции API для работы с рецензиями и докладами")
        Container(reviewer, "Рецензирование докладов", "java", "Включает функциональность для оценки докладов, принятия решений и выбора докладов для конференции")
        Person(recenzent, "Рецензенты", "Рецензирование докладов, общение с Докладчиками")
        sponsor --> conference: Финансирование
        Container(service_workflow, "Сервис работы с докладами", "java", "")
        Container(service_review, "Сервис работы с рецензиями", "java", "")
        ContainerDb(db, "База данных рецензий, докладов", "Postgresql",$sprite="postgresql")
        Rel(workflowAPI, service_workflow, "GET/POST", "JSON/HTTP")
        BiRel_U(service_workflow, db, "Чтение/Запись", "JDBC")
        Rel_L(user, workflow, "Создание доклада", "HTTPS")
        Rel(workflow, workflowAPI, "GET/POST", "JSON/HTTP")

        Rel(recenzent, reviewer, "Использование", "HTTP")
        Rel(reviewer, workflowAPI, "GET/POST", "JSON/HTTP")
        Rel(workflowAPI, service_review, "GET/POST", "JSON/HTTP")
        BiRel(service_review, db, "Чтение/Запись", "JDBC")

    }

    package "Работа с расписанием" {
        Container(schedule, "Планирование докладов", "Включает в себя функциональность планирования расписания докладов и составления программы конференции","java")
        Container(scheduleAPI, "REST API", "java",  "Функции API для работы с расписанием конференции")
        Container(service_programm, "Сервис cоставления программы конференции", "java", "") 
        Rel(schedule, scheduleAPI, "GET/POST", "JSON/HTTP")
        Rel(scheduleAPI, service_programm, "GET/POST", "JSON/HTTP")
        ContainerDb(db2, "Хранение расписания докладов", "Postgresql",$sprite="postgresql")
        Rel(organizer, schedule, "Использование", "HTTP")
        Rel(service_programm, db2, "Чтение/Запись", "JDBC")
    }

    package "Проведение конференции" {
        Container(feedbackAPI, "REST API", "java", "Функции API для сбора обратной связи")
        Container(broadcast, "Трансляция конференции","java","Включает функциональность для записи и трансляции докладов в режиме реального времени") 
        Container(feedback, "Обратная связь", "java", "Включает функциональность для оценки и комментирования докладов") 
        Person(technical_staff, "Технический персонал", "Технический персонал для проведения конференции")
        Rel(technical_staff, broadcast, "Подготовка")
        Container(service_feedback, "Сервис обработки обратной связи", "java", "") 
        Rel(feedback,feedbackAPI,"GET/POST", "JSON/HTTP")
        Rel(feedbackAPI, service_feedback, "GET/POST", "JSON/HTTP")
        ContainerDb(db3, "Хранение обратной связи", "Postgresql",$sprite="postgresql")
        BiRel(service_feedback, db3, "Чтение/Запись", "JDBC")

        Rel_R(user, feedback, "Использование", "HTTPS")
        Rel_L(customer, feedback, "Использование", "HTTPS")
        Rel(organizer, feedback, "Просмотр", "HTTPS")
        Rel_R(user, broadcast, "Выступление и просмотр", "HTTPS")
        Rel_L(customer, broadcast, "Просмотр", "HTTPS")


    }



}

@enduml
```

    Rel_U(feedbackAPI, db, "Чтение/Запись", "JDBC")


    broadcast <--> feedback: Синхронизация
    schedule <--> broadcast: Синхронизация
