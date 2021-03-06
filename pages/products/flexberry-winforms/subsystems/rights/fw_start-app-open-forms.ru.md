---
title: Разрешение на запуск приложения и открытие форм
sidebar: flexberry-winforms_sidebar
keywords: Flexberry Winforms 
summary: Указано как добавлять пользователю те или иные полномочия
toc: true
permalink: ru/fw_start-app-open-forms.html
lang: ru
---

* Настройка приложения
Развернуть `AzManBridgeService` и настроить приложение.

* Добавить разрешение на старт приложения
Для добавления определенным пользователям прав на запуск приложения необходимо:
 
* Для приложения (класс application) в атрибуте [`AccessType`](fo_access-type.html) указать "`this`".

* В [консоли управления полномочиями](efs_security-console.html) в разделе "Субъекты\Операции" создать операцию, название которой соответствует названию приложения.
Затем на форме редактирования пользователя в разделе "Агенты\Пользователи" на вкладке "Операции" отметить добавленную операцию с [типом доступа `Execute`(Исполнение)](efs_right-manager.html).

* Добавить разрешение на открытие форм
Для начала нужно добавить полномочия на классы.
Как добавить полномочия на классы можно прочитать в [этой статье](fa_authority-classes.html) 

{% include note.html content="Для добавления в список классов нужных объектов использовать кнопку «Заполнить список классов из сборки с объектами» в разделе «Субъекты\Классы», где выбрать необходимую сборку с объектами. Классы форм добавляются в список классов вручную." %}
