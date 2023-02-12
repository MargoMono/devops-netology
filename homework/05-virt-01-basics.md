----------------------------------------------
Задача 1
----------------------------------------------

Опишите кратко, как вы поняли: 
в чем основное отличие полной (аппаратной) виртуализации, паравиртуализации и виртуализации на основе ОС.

**ОТВЕТ**

Полная (аппаратная) виртуализация - полная эмуляция процессора, памяти и необходимых перифирийных устройств, использует технологию виртуализации ЦП для реализации общего доступа гипервизора к базовому оборудованию.
Преимущества: поддерживает почти все гостевые операционные системы без изменений, что означает хорошую совместимость.
Недостатки: поскольку некоторые элементы имеют аналоговые функции, ввод-вывод и производительность хуже, чем у паравиртуализации.

Паравиртуализациия - взаимодействует с программой гипервизора, который предоставляет ей гостевой API
Преимущества: по сравнению с полной виртуализацией паравиртуализация более рационализирована в использовании ресурсов и имеет преимущества в общей скорости.
Недостатки: необходимо изменить гостевую операционную систему, поэтому поддерживается меньшее количество операционных систем, т.к. требуется открытый код.

Виртуализации на основе ОС - реализуется без отдельного слоя гипервизора, виртуализируется пользовательское окружение ОС.

----------------------------------------------
Задача 2
----------------------------------------------

Выберите один из вариантов использования организации физических серверов, в зависимости от условий использования.

Организация серверов:

    физические сервера,
    паравиртуализация,
    виртуализация уровня ОС.

Условия использования:

    Высоконагруженная база данных, чувствительная к отказу.
    Различные web-приложения.
    Windows системы для использования бухгалтерским отделом.
    Системы, выполняющие высокопроизводительные расчеты на GPU.

Опишите, почему вы выбрали к каждому целевому использованию такую организацию

**ОТВЕТ**

Высоконагруженная база данных, чувствительная к отказу - физические сервера (проблема производительности) + master/slave (проблема отказа) + в идеале шардин (проблема нагрузки).
Различные web-приложения - виртуализация уровня ОС, удобно и для деплоя и для разработгки, поскольку довольно проста в использовании.
Windows системы для использования бухгалтерским отделом - паравиртуализация (не предполагается высокая нагрузка, воспрозведение на разных серверах(как то же веб))
Системы, выполняющие высокопроизводительные расчеты на GPU - физические сервера, поскольку только так мы получим наибольшую производительность железа

-----------------------------
Задача 3
-----------------------------

Выберите подходящую систему управления виртуализацией для предложенного сценария. Детально опишите ваш выбор.

Сценарии:


    1. 100 виртуальных машин на базе Linux и Windows, общие задачи, нет особых требований. Преимущественно Windows based инфраструктура, требуется реализация программных балансировщиков нагрузки, репликации данных и автоматизированного механизма создания резервных копий.
    2. Требуется наиболее производительное бесплатное open source решение для виртуализации небольшой (20-30 серверов) инфраструктуры на базе Linux и Windows виртуальных машин.
    3. Необходимо бесплатное, максимально совместимое и производительное решение для виртуализации Windows инфраструктуры.
    4. Необходимо рабочее окружение для тестирования программного продукта на нескольких дистрибутивах Linux.

**ОТВЕТ**

1. Hyper-V - отвечает требования + хорошо совместим с Windows
2. KVM - хорошая производительность и совместимость
3. XEN PV- хорошо совместим с Windows + open source
4. Docker - хорошая совместимость с Linux, быстрота развертывания и легкость настройки

-----------------------------
Задача 4
-----------------------------

Опишите возможные проблемы и недостатки гетерогенной среды виртуализации 
(использования нескольких систем управления виртуализацией одновременно) и 
что необходимо сделать для минимизации этих рисков и проблем.
Если бы у вас был выбор, то создавали бы вы гетерогенную среду или нет? Мотивируйте ваш ответ примерами.

**ОТВЕТ**

Нет, потому что это гетерогенная среда увеличивает в разы специфику работы, требует большего количества специалистов, 
особенно если решения взимодествуют между собой. 

Такое решение стоит использовать только в случае, если оно, и правда отправдано, к примеру, в случаях, 
как мы рассматривали выше мы выбрали разные варианты работы с бд, веб и программой для бухгалтерии. 