# Контейнерная архитектура

```plantuml
@startuml C4_Containers
!define C4_CONTAINER_NO_COMPONENT true
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/v2.3.0/C4_Container.puml

title Контейнерная диаграмма программной системы
Person(customer, "Слушатель конференции")
Person(user, "Докладчик")
Person(sponsor, "Спонсор", "Организация, финансирующая конференцию")
System_Boundary(conference, "helloconf.mts.ru") {

    Person(organizer, "Организатор", "Организатор конференции")

    package "Работа с докладчиками" {
        Container(workflow, "Работа с докладчиками", "java", "Включает функциональность для подачи докладов, обратной связи с докладчиками и управления докладами")
        Container(workflowAPI, "REST API", "java", "Функции API для работы с докладчиками")
        Container(reviewer, "Рецензирование докладов", "java", "Включает функциональность для оценки докладов, принятия решений и выбора докладов для конференции")
        Container(reviewerAPI, "REST API", "java", "Функции API для рецензирования докладов")
        Person(recenzent, "Рецензенты", "Рецензирование докладов, общение с Докладчиками")
        reviewer <--> workflow: Синхронизация
        sponsor --> conference: Финансирование
        ContainerDb(db, "База данных рецензий, докладов, обратной связи", "Postgresql",$sprite="postgresql")
    }

    package "Работа с расписанием" {
        Container(schedule, "Планирование докладов", "java") 
        Container(schedule2, "Составление программы конференции", "java") 
        BiRel_R(schedule, schedule2, "Cинхронизация")

    }

    package "Проведение конференции" {
        Container(feedbackAPI, "REST API", "java", "Функции API для сбора обратной связи")
        Container(broadcast, "Трансляция конференции","java","Включает функциональность для записи и трансляции докладов в режиме реального времени") 
        Container(feedback, "Обратная связь", "java", "Включает функциональность для оценки и комментирования докладов") 
        Person(technical_staff, "Технический персонал", "Технический персонал для проведения конференции")
        Rel(technical_staff, broadcast, "Подготовка")
        Rel_R(feedback,feedbackAPI,"POST \npostfeedback", "JSON")
        Rel_R(feedback,feedbackAPI,"GET \ngetfeedback", "JSON")
    }



    Rel_R(user, feedback, "Использование", "HTTPS")
    Rel_L(customer, feedback, "Использование", "HTTPS")
    Rel(organizer, feedback, "Просмотр", "HTTPS")
    Rel_R(user, broadcast, "Выступление и просмотр", "HTTPS")
    schedule <--> broadcast: Синхронизация
    broadcast <--> feedback: Синхронизация
    Rel_L(customer, broadcast, "Просмотр", "HTTPS")

    Rel(recenzent, reviewer, "Запрос списка неоцененных докладов", "HTTPS")
    Rel(reviewer, reviewerAPI, "GET \nunreviewedTalks", "JSON")
    Rel(reviewerAPI, db,"Чтение", "JDBC")

    Rel(organizer, schedule, "Запрос информации о новых поданных докладах", "HTTPS")
    Rel(organizer, schedule2, "Использование", "HTTPS")
    Rel_R(schedule, workflowAPI, "GET \ninfonewtalks", "JSON")
    Rel_R(workflowAPI, db,"Чтение/Запись", "JDBC")

    Rel_L(user, workflow, "Создание доклада", "HTTPS")
    Rel(workflow, workflowAPI, "POST \ntalks", "JSON")
    Rel_U(feedbackAPI, db, "Чтение/Запись", "JDBC")


}

@enduml
```

