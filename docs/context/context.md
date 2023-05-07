# Контекст решения
<!-- Окружение системы (роли, участники, внешние системы) и связи системы с ним. Диаграмма контекста C4 и текстовое описание. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375783261
-->
```plantuml
@startuml C5_Elements
title Контекстная диаграмма программной системы
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

Person(customer, "Слушатель конференции")
Person(user, "Докладчик")
Person(sponsor, "Спонсор", "Организация, финансирующая конференцию")
System_Boundary(conference, "helloconf.mts.ru"){
    Person(organizer, "Организатор", "Организатор конференции") 
    Container(workflow, "Работа с докладчиками", "Включает функциональность для подачи докладов, обратной связи с докладчиками и управления докладами")
    Container(reviewer, "Рецензирование докладов", "Включает функциональность для оценки докладов, принятия решений и выбора докладов для конференции") 
    Container(schedule, "Работа с расписанием", " Включает функциональность для планирования докладов, управления временем и составлении программы конференции") 
    Person(technical_staff, "Технический персонал", "Технический персонал для проведения конференции")
    Container(broadcast, "Трансляция конференции", "Включает функциональность для записи и трансляции докладов в режиме реального времени") 
    Container(feedback, "Сбор обратной связи", "Включает функциональность для оценки и комментирования докладов") 
    Person(recenzent, "Рецензенты", "Рецензирование докладов, общение с Докладчиками")
    
    Rel(organizer, schedule, "Использование")
    BiRel_L(workflow, user, "Взаимодействие")
    Rel_L(workflow, reviewer, "Подача докладов")
    workflow-> schedule: Подача докладов
    schedule --> broadcast: Синхронизация
    broadcast --> feedback: Синхронизация
    Rel_U(technical_staff,broadcast, "Подготовка")
    technical_staff -> conference: Техническое обеспечение 
    customer -> broadcast: Просмотр
    user -> broadcast: Выступление
    Rel_U(sponsor,conference, "Финансирование")
    Rel_D(recenzent,reviewer, "Использование")
    Rel(customer, feedback, "Использование")
    Rel(user, feedback, "Использование")
    Rel(organizer, feedback, "Использование")

}
@enduml
