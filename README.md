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

# Для windows 
Установка службы

    sc create "1C:Enterprise RAS" binpath= "C:\Program Files\1cv8\Х.Х.Х.ХХХХ\bin\ras.exe cluster --service" displayname= "1C:Enterprise RAS" start= auto 
    net start "1C:Enterprise RAS"

Удаление службы 

    sc delete "1C:Enterprise RAS"

настройка zabbix agent

    UserParameter=onec-session,"C:\Program Files\1cv8\8.3.9.1850\bin\rac.exe" session --cluster=03692e0b-0c48-4c6c-bf7d-a6dd89ebbd71 list --infobase=1fcffbdb-0fd4-4678-bd8b-95c96fdc419f |  find /c "1CV8C"
    UserParameter=onec-bgj,"C:\Program Files\1cv8\8.3.9.1850\bin\rac.exe" session --cluster=<uuid> list --infobase=<uuid> | find /c "BackgroundJob "


Графики 
![screen01](https://github.com/bessonovevgen/srv-1c-linux-zabbix-template/blob/master/images/screen-01.png)

TODO 
1. Нужно правила обнаружения.
2. Подобрать подходящие (время выполнения/нагрузка на сервер) интервалы опроса параметров.
