---
title: Неопределено - это тип или его отсутствие?
description: И то и другое. Одновременно...
date: 2021-05-19 01:22:00 +/-TTTT
categories: [Заметки]
tags: [1С, Заметки, Желтый Чайник 1С]
links:
  top: true
  bottom: false
  values:
  - name: Пост в Telegram
    url: https://t.me/JuniorOneS/138
---

Действительно, есть такой тип "`Неопределено`". 

```bsl
//Этот код сообщит "Да"
Сообщить( ТипЗнч(Неопределено) = Тип("Неопределено") );
```

И вроде бы вопрос решен, НО. Попробуйте создать таблицу значений с колонкой, у которой тип - `Неопределено`:

```bsl
ТЗ = Новый ТаблицаЗначений;
ТЗ.Колонки.Добавить("Колонка", Новый ОписаниеТипов("Неопределено"));

Строка = ТЗ.Добавить();

Строка.Колонка = 1;
Сообщить(Строка.Колонка); //Сообщит: 1

Строка.Колонка = "Строка";
Сообщить(Строка.Колонка); //Сообщит: Строка
```

Именно, мы получили колонку с произвольным типом, ибо **невозможно создать колонку с типом Неопределено**.

Кто-то может сказать: _"А как же создать колонку с типом, у которого только одно значение?"_
А вы попробуйте выполнить тот же самый код, но укажите в качестве типа колонки `Null` ;)

И так. `Неопределено` - это тип. Но этот тип при этом означает отсутствие типа. И невозможно создать `ОписаниеТипов` с типом `Неопределено`:

```bsl
ОписаниеТипов = Новый ОписаниеТипов("Null,Неопределено");
Сообщить(ОписаниеТипов.Типы().Количество()); //1
```

1С проигнорировала `Неопределено` при создании `ОписанияТипов`. 
Вместо этого он **появится автоматически, если описание будет составным**.

И, в контексте [заметки про выгрузки запроса]({% link _posts/notes/2021-05-07-columns-of-query-result-ones.md %}), прекрасно видно, как сама 1С не знает, что же ей делать. 

В запросе ниже просто колонка с `Неопределено`. И эту выгрузку запроса мы **не сможем поместить в другой запрос**! Попробуйте сами и убедитесь, что получите ошибку: *Тип не может быть выбран в запросе*

```bsl
//Получили ТЗ с колонкой Неопределено
Запрос = Новый Запрос("ВЫБРАТЬ Неопределено КАК Поле");
ТЗ = Запрос.Выполнить().Выгрузить();

//Попытались выбрать её в другом запросе
Запрос.Текст = "ВЫБРАТЬ 
|    ТЗ.Поле 
|ПОМЕСТИТЬ ВТ
|ИЗ
|     &ТЗ КАК ТЗ;
|ВЫБРАТЬ 
|    *
|ИЗ 
|    ВТ КАК ВТ";

Запрос.УстановитьПараметр("ТЗ", ТЗ);
Результат = Запрос.Выполнить();
```

**Почему так?**

Мы получаем выгрузку запроса. И 1С должна была автоматически добавить тип `Null` ([как она делает всегда ]({% link _posts/notes/2021-05-07-columns-of-query-result-ones.md %})). Но если платформа так сделает, то получит колонку с типом `Null`, а `Неопределено` потеряет. Ведь кодом раньше мы доказали себе, что невозможно создать описание типов с этими двумя типами вместе. И тогда 1С идет по другому пути - она создает колонку просто с типом `Неопределено`. Но по законам платформы такая колонка превращается в тип `Произвольный`. И становится недоступной для передачи в запрос. Вот незадача 🤷‍♂️

Это не совсем очевидный нюанс платформы. И вы можете с ним столкнуться, если вдруг ваш запрос строится динамически, и в какой-то момент одна из колонок текста запроса содержит только Неопределено. Выгрузку такого запроса невозможно будет поместить в другой запрос. И придется либо намеренно расширять тип до составного ([выложил шаблон кода](https://fastcode.im/Templates/7482)), либо переписывать текущую реализацию.