---
title: Ограничения с параметрами для пользователя
sidebar: flexberry-aspnet_sidebar
keywords: Flexberry ASP-NET, Ограничения
toc: true
permalink: ru/fa_limit-parameters-user.html
lang: ru
---

## Добавление параметра

Для использования параметров в ограничении необходимо добавить описание параметра. Сделать это необходимо на вкладке `Параметры`, выбрав команду `Добавить параметр`. После выполнения данной команды в разделе `Список параметров` появится новая строка, в которой необходимо указать имя параметра и его [тип](fa_advanced-limit-editor-parameters.html). Обязательно следует сохранить введенную информацию с помощью кнопки `Сохранить`.

![](/images/pages/products/flexberry-aspnet/controls/limit-editor/add-parameter.png)

## Использование параметра в ограничении

Имя параметра может быть использовано в тех местах ограничения, в которых допустимо использование константы. Для выбора параметра при редактировании поля условия необходимо нажать кнопку, расположенную справа. Список доступных для выбора параметров строится исходя из их типа.

![](/images/pages/products/flexberry-aspnet/controls/limit-editor/choose-parameter.png)

## Ввод значений параметров

Конкретные значения параметров при применении запрашиваются у пользователя перед построением списка на специальной форме. Для ввода новых значений параметров необходимо сбросить текущее ограничение и применить ограничение повторно.

![](/images/pages/products/flexberry-aspnet/controls/limit-editor/input-parameter.png)

# См. также
[Ограничения с параметрами в WOLV (для программиста)](fa_limit-parameters-developer.html)
