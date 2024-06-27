---
title: Колонки результата запроса
date: 2021-05-07 01:22:00 +/-TTTT
categories: [Заметки]
tags: [1С, Заметки, Желтый Чайник 1С]
links:
  top: true
  bottom: false
  values:
  - name: Пост в Telegram
    url: https://t.me/JuniorOneS/135
---

У результата запроса есть одна особенность. 
Какой бы вы ни выбирали тип значений, платформа добавит к колонке результата тип `Null`.
`Число`, `Строка`, `Булево`, `Ссылка`. Неважно. Даже если по тексту запроса очевидно, что в результате невозможен `Null` - он всё равно будет.

Например:
```bsl
Запрос  = Новый Запрос("ВЫБРАТЬ 1");
Колонки = Запрос.Выполнить().Колонки;

Сообщить(Колонки.Получить(0).ТипЗначения);
//Сообщение: Null, Число
```

Так как в колонку добавляется тип `Null`, то она становится составного типа. И получает в довесок тип `Неопределено`.
С таким поведением можно столкнуться воочию, если решить дополнять данными Выгрузку результата запроса. 
Все колонки добавленной строки будут значением `Неопределено`. 

Например:

```bsl
Запрос      = Новый Запрос("ВЫБРАТЬ 1 КАК Поле");
Выгрузка    = Запрос.Выполнить().Выгрузить();
НоваяСтрока = Выгрузка.Добавить();

Сообщить(ТипЗнч(НоваяСтрока.Поле));
//Сообщение: Неопределено
```

В принципе, поведение платформы нельзя назвать ошибочным. 1С просто не считает целесообразным проверять, есть ли действительно `Null` в результате. И просто добавляет этот тип, исключая возможные ошибки. 

Поэтому, разработчику просто нужно это помнить. 
\+ Есть [простой метод](https://fastcode.im/Templates/7418), который делает копию таблицы, удаляя из неё тип `Null`. Его можно использовать на простых табличках. Например, в [этом методе](https://fastcode.im/Templates/7420).

**Как думаете, а возможны ли случаи иного поведения формирования типа колонки результата запроса ?**
