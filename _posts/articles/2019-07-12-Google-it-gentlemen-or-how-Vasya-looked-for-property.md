---
title: Гуглите, господа!, или как Вася свойство искал
date: 2019-07-12 10:00:00 +/-TTTT
description: Небольшая история про то, как программист Вася искал свойство на палитре.
media_subpath: /assets/posts/articles/2019-07-12-Google-it-gentlemen-or-how-Vasya-looked-for-property/
categories: [Статьи]
tags: [1С, Статьи, Желтый Чайник 1С]
image:
  path: cover.png
links:
  top: true
  bottom: true
  values:
  - name: Статья на Инфостарт
    url: https://infostart.ru/1c/articles/1091321/?ref=1159
---

Бывает так, что разработчики тупят. Нет, правда.

_С нами_, конечно, не _случается_. _Мы_ ведь не _делаем_ никогда _ошибки_. Это история точно не _про нас_.

Она про ~~гипотетического~~ разработчика Васю. 

**Дано:** У обработки есть реквизит с типом "СписокЗначений".

**Задание:**  вынести его на форму и ограничить выбор значений типом "Число". Пользователи должны заносить в него только числа.

**Решение:**
> _- да чё тут делать? Помню, просто решается. У реквизита на форме есть свойство ТипЗначения..."_

Но стоит нашему разработчику открыть палитру свойств...

## Стадия 1. Отрицание

![Отсутствие свойства](01.png)

> _- Да было же. Я ж видел его!_

Разработчик Вася не верит своим глазам. И начинает поиск свойства по другим местам.

И у элемента формы, и у самой обработки и, чем чёрт не шутит, в свойствах всей конфигурации...

Нигде нету.

> _- Может на Мисте подскажут?_

![Миста](02.png)

Есть легенда, что на Мисте есть топик, в котором чётко и по делу задают вопрос и так же чётко отвечают. Но это совсем другая история..

> _- Ну хорошо, значит найду ту обработку, в которой это видел!_

В моменты поиска "той самой" так быстро летит время:

> _- Ух ты, вот эту обработку я искал на прошлой неделе._
> _- О, а эту я вообще забыл. Нужно отдать заказчику. Вспомнить бы какому.._
> _- Что за большая красная кнопка с надписью "Сделать всё"? Проверю ка её на продуктиве..._

И так далее, пока не кончится терпение. Или, как в нашем случае, найдётся злосчастная "та самая" обработка.

## Стадия 2. Гнев

> _- Нашёл! Ну вот же, есть свойство!_

И, действительно, свойство есть:

![Свойство есть](03.png)

Василий открывает предыдущую обработку... Свойства нет.

Разработчик не сдаётся. Резким движением руки он перекидывает реквизит с одной формы на другую. И сравнивает.

![Так в чем же разница](04.png)

Васю посещают догадки, но...

## Стадия 3. Торг

> _- Может почистить кэш? Сделать тестирование и исправление? chdbfl?_

![Шаман](05.jpg)

Василий решает отложить эту задачу, перезагрузить всё что возможно и сходить принять валерьянки.

Бывает так отложишь сломанную обработку, она полежит, а завтра, глядишь, и работает снова. Главное не забыть про неё.

Эту обработку Вася точно не забудет. Её через день сдавать уже.

И вот на следующее утро, после полной перезагрузки компьютеров, серверов и электростанций, наш разработчик возвращается к ней.

## Стадия 4. Депрессия

> _- Ничего не изменилось..._

![Что](06.jpg)

Василий чувствует, что потратил время зря. Он вспоминает всё, что сделал. Сколько времени впустую.

Опустив руки, разработчик прибегает к последнему. Гуглит...

## Стадия 5. Принятие

> _- Может просто и не было такого свойства?_

Василий подозревал, что это не глюк. И что не разыгравшееся воображение.

> _- так хотелось найти свойство!_

Естественно, это просто нюанс платформы, который нужно знать. На форме палитра свойств есть у реквизита формы, а не у реквизита обработки. Если у вас на форме список значений именно реквизит формы, то ограничить его числами можно установив свойство в редакторе

![Что](07.png)

Но если у вас реквизит обработки, то такой возможности не будет.

![Что](08.png)

Что остаётся делать? Великий Гугл подсказывает:

```bsl
&НаСервере
Процедура ПриСозданииНаСервере(Отказ, СтандартнаяОбработка)
    
    Объект.СписокЗначений.ТипЗначения = Новый ОписаниеТипов("Число");
    
КонецПроцедуры
```

На самом деле очень просто. Если знать об этом и помнить. А если не помнить - гуглить. И не тупить.

Мы ведь с вами никогда не _тупим_, верно? И ничего не _забываем_. С нашим уровнем такого не _происходит_.

Ну ничего, разработчик Вася не унывает. Он помнит, что 

> _Не ошибается тот, кто ничего не делает_

И, сдав задачку вовремя, с удовольствием отправился на выходные пожарить шашлычков. Отдых - лучшее лекарство.