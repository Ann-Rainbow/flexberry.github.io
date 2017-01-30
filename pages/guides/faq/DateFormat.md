---
title: Языконезависимый формат даты
sidebar: product--sidebar
keywords: DateTime (работа с датами), БД
toc: true
permalink: ru/Языконезависимыи-формат-даты.html
folder: product--folder
lang: ru
---

'''MS SQL Server'''

Сортировка типа:
`order by isnull("дата", '1900-01-01')`
работает при любых языковых настройках.

Языконезависимый формат даты:

Задавайте дату в виде строки 'YYYYMMDD' (без разделителей) или дату-время 'YYYYMMDD hh:mm:ss'. Фича в том, что в указанном формате SQL-сервер всегда однозначно интерпретирует дату, независимо от региональных и прочих настроек. Например, "` ... WHERE SomeDateField<'20010302' ... `". Эта дата при любом раскладе будет интерпретирована сервером как 2 марта 2001 года.

Если языконезависимый формат даты использовать не представляется возможным то можно использовать команду:
SET LANGUAGE us_english  - которая задает языковой формат в рамках сессии

'''Oracle'''

В оракле проблема решается использованием функции `ToDate()` с указанием маски.
Например: `TO_DATE('1900-01-01', 'YYYY-MM-DD')`
Так же у этой функции есть 3ий параметр, так называемые NLS Params. Он необходим если дату нужно получить, например в формате день недели, число Месяц год (Dy, dd Mon yyyy). Дело в том, что название дня недели (напр. Sunday) Dy зависит от региональных настроек оракла. В этом случае приведение будет выглядеть так:
`to_date(date,'Dy, dd Mon yyyy hh24:mi:ss', 'NLS_DATE_LANGUAGE = ''american'' ')`

Примечательно, что третий параметр TO_DATE() заключается в кавычки, а если нужно указать кавычки в кавычках, то употребляют две одинарные кавычки.


<sub>Написано: Ивуков Александр

Проверено: Круглов Артём</sub>