---
title: Каскадное удаление объектов
sidebar: flexberry-orm_sidebar
keywords: Объекты данных, Flexberry ORM, базы данных, бизнес-серверы, ограничения
summary: Способы каскадного удаления объектов, их плюсы и минусы
toc: true
permalink: ru/fo_cascade-delete.html
lang: ru
---

## Задача каскадного удаления

Пусть дана следующая [диаграмма](fd_class-diagram.html):

![](/images/pages/products/flexberry-orm/business-servers/kredit-diagramm.png)

Если в базе данных есть объекты типа `Клиент`, ссылающиеся на `Адрес`, то без дополнительных настроек при попытке удаления объекта типа `Адрес` произойдёт ошибка. База данных не даст удалить такой объект.

## Варианты решения проблемы

Вариантов может быть очень много, в данной статье будут приведено только несколько. Технология предоставляет механизмы для решения проблемы (в основном они опираются на использование [бизнес-серверов](fo_bs-wrapper.html)), варианты ограничиваются лишь фантазией разработчика. 

### Специальные интерфейсы

Для реализации каскадного удаления можно воспользоваться специально разработанными интерфейсами [IReferencesCascadeDelete](fo_i-references-cascade-delete.html) и [IReferencesNullDelete](fo_i-references-null-delete.html).

### Рекурсивное удаление

Это самый простой вариант, но и самый недружелюбный к пользователю: удаление 1 объекта может привести к удалению важной информации информации, связанной с данным объектом.

Алгоритм:

* В бизнес-сервере мастера (в примере - `Адрес`) вычитать все объекты, ссылающиеся на удаляемый.
* Проставить всем объектам статус [ObjectStatus.Deleted](fo_object-status.html).
* Отправить на удаление все объекты.
* Повторить рекурсивно для всех объектов.

### Фиктивный объект

Такой вариант позволяет сохранить все данные, кроме того объекта, который необходимо удалить. Однако в базе останется множество объектов, ссылающихся на несуществующий.

Стоит также отметить, что данный способ требует дополнительной обработки данных при выводе пользователю. Объекты, ссылающиеся на фиктивные, необходимо фильтровать или обрабатывать особым образом.

Вариантов решения проблемы несколько:

* создавать фиктивный объект при каждом удалении
* создать по 1 фиктивному объекту для каждого класса и "вешать" все ссылки на него.

Алгоритм для второго варианта:

* (один раз) Создать объект и записать его в базу. Запоминить его [PrimaryKey](fo_primary-keys-objects.html), например, в файле конфигурации или в файле с константами.
* В бизнес-сервере мастера (в примере - `Адрес`) вычитать все объекты, ссылающиеся на удаляемый.
* Проставить всем объектам ссылку на фиктивный объект.
* Отправить на обновление все объекты.

### Фиктивное удаление

При фиктивном удалении данные на самом деле не удаляются из базы, а всего лишь помечаются как удаленные. Во все объекты добавляется какое-нибудь поле типа `bool`. При удалении объекта в бизнес-сервере перехватывается объект, у него меняется статус с `Deleted` на `Altered` и изменяется поле `Актуально = false;`.

После этого объект уходит на обновление в базу и остается в ней, но считается удаленным. Разумеется, необходимо реализовывать логику, которая будет "считать" такие объекты удаленными: при выводе информации пользователю необходимо накладывать ограничения на выводимые данные.

{% include note.html content="Такой способ позволяет восстанавливать удаленные объекты." %}

#### Пример

Необходимо доработать диаграмму классов таким образом, чтобы она поддерживала фиктивное удаление: добавить поле `Актуально:bool`.

![](/images/pages/products/flexberry-orm/business-servers/kredit-diagramm-aktualno.png)

Добавить логику в бизнес-сервера объектов (на примере `Адреса`):

```csharp
if (UpdatedObject.GetStatus() == ObjectStatus.Deleted)
{
	// Не дадим объекту удалиться, но выставим флаг Актуальности.
	UpdatedObject.SetStatus(ObjectStatus.Altered);
	UpdatedObject.Актуально = false;

	// Найдем все объекты, ссылающиеся на "удаляемый" и удалим их.
	var ds = (SQLDataService)DataServiceProvider.DataService;
	var klients =
		ds.Query<Клиент>(Клиент.Views.КлиентE)
		  .Where(k => k.Прописка.__PrimaryKey == UpdatedObject.__PrimaryKey);
	foreach (var k in klients)
	{
		k.SetStatus(ObjectStatus.Deleted);
	}

	return klients.ToArray();
}
```

{% include note.html content="Внимание! Cсылающиеся объекты отправленные на удаление, но они точно также перехватятся в своем бизнес-сервере и не удалятся." %}

Далее, чтобы пользователю не выводились "удаленные" данные при просмотре списка объектов, требуется на соответствующий контрол наложить ограничение вида:

``` csharp
var ds = (MSSQLDataService)DataServiceProvider.DataService;
IQueryable<Клиент> limit1 = ds.Query<Адрес>(Адрес.Views.АдресL).Where(Address => Address.Актуально);
Function onlyActual = LinqToLcs.GetLcs(limit1.Expression, Адрес.Views.АдресL).LimitFunction;
```