---
title: WebLockHelper
sidebar: flexberry-aspnet_sidebar
keywords: Flexberry ASP-NET
toc: true
permalink: ru/fa_web-lock-helper.html
folder: products/flexberry-aspnet/
lang: ru
---

<div style="margin:5px; padding-left:28px; float:right; width:60%; outline:1px solid white;">
<br>
<table border="0" width="100%" bgcolor="#6495ED">
<tbody><tr><td bgcolor="#FFFFFF">
&nbsp;&nbsp;&nbsp;'''WebLockHelper'''

* '''Платформа''': [FlexberryASP.NET](flexberry-a-s-p-n-e-t.html).
* '''Компонент''': [Web-контролы и web-компоненты](web-controls.html).
* '''Предназначение''': класс для работы с блокировками объектов данных в web-приложениях.

</td>
</tr></tbody></table></a>
</div>

# WebLockHelper
`ICSSoft.STORMNET.Web.Tools.WebLockHelper` - класс для работы с web-блокировками объектов данных в веб-приложениях. Использует дату и время жизни блокировки для определения ее актуальности, блокировки хранит в БД.

## Интерфейс
Класс реализует интерфейс `ILockHelper`:
```cs
public interface ILockHelper
{
    bool LockDataObject(string dataObjectKey, string userName, out string lockUserName);

    bool ClearWebLock(string dataObjectKey, string userName);

    string GetLock(string dataObjectKey);

    TimeSpan LockTimeout { get; set; }
}
```
`LockDataObject` - попытаться установить блокировку на объект по переданному ключу и имени пользователя. В случае удачной блокировки возвращает `true`, если объект уже заблокирован - `false`. При этом в параметр `lockUserName` записывается имя пользователя, установившего блокировку.<br>
`ClearWebLock` - удалить блокировку, если она была установлена текущим пользователем. В случае удачной очистки вернет `true`, иначе false.<br>
`GetLock` - возвращает имя пользователя, заблокировавшего объект, по ключу. Если объект не заблокирован, возвращается пустая строка.<br>
`LockTimeout` - время жизни блокировки. Необходимо для того, чтобы удалять старые блокировки. Если страница редактирования открыта, то она периодически посылает ajax-запросы на продление блокировки. При закрытии страницы запросы прекращаются, и блокировка стареет. Периодичность запросов соответствует времени жизни блокировки.<br>

## Использвание
Если нужен свой сервис блокировок, можно просто реализовать интерфейс `ILockHelper` и использовать свой класс, где нужно, например, задать его для блокировок в [WebObjectListView](web-object-list-view.html). 

Пример использования WebLockHelper (код [WOLV](web-object-list-view.html)):
```cs
        // Свойство, хранящее LockHelper
        public static ILockHelper LockHelper
        {
            get
            {
                if (_lockHelper == null)
                {
                    _lockHelper = new WebLockHelper();
                }
                return _lockHelper;
            }
            set { _lockHelper = value; }
        }
```
Установка блокировки:
```cs
if (lockHelper.LockDataObject(PK, Context.User.Identity.Name, out lockUserName))
{
    MessageBox.Show("Объект успешно заблокирован");
} 
else
{
    MessageBox.Show("Объект уже заблокирован пользователем " + lockUserName);
}
```
 
