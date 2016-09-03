# Мониторинг сервера приложений 1с в Linux

Мониторинг выполняется с помощью консольных утилит ras и rac через UserParameters

    UserParameter=onec-session,/opt/1C/v8.3/x86_64/rac session --cluster=<uuid> list --infobase=<uuid> | grep 1CV8C | wc -l
    UserParameter=onec-bgj,/opt/1C/v8.3/x86_64/rac session --cluster=<uuid> list --infobase=<uuid> | grep BackgroundJob | wc -l

ras - должен быть запущен всегда
rac - обращается к ras за запрошенными данными

Запуск ras (на том серере который нужно мониторить)

    /opt/1C/v8.3/x86_64/ras --daemon cluster

Непосредствено сам шаблон для импорта: srv-1c-linux-zabbix-temlate.xml

Реализовано два параметра и один график


Примеры:

Запрос показывающий количество сеансов

    /opt/1C/v8.3/x86_64/rac session --cluster=<uuid> list --infobase=<uuid> | grep app-id | wc -l


Получить <uuid> для параметра --cluster

    /opt/1C/v8.3/x86_64/rac cluster list

Получить <uuid> для --infobase

    /opt/1C/v8.3/x86_64/rac infobase --cluster=<uuid> summary list

Запрос показывающий количество тонких клиентов

    /opt/1C/v8.3/x86_64/rac session --cluster=<uuid> list --infobase=<uuid> | grep 1CV8C | wc -l

Графики 
![screen01](https://github.com/bessonovevgen/srv-1c-linux-zabbix-template/blob/master/images/screen-01.png)

TODO 
1. Нужно правила обнаружения.
2. Подобрать подходящие (время выполнения/нагрузка на сервер) интервалы опроса параметров.
