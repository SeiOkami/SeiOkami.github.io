---
title: Рекурсия без рекурсии
date: 2020-06-22 01:22:00 +/-TTTT
description: Как обойтись без рекурсии в случаях, когда кажется, что иначе нельзя?
media_subpath: /assets/posts/notes/2020-06-22-recursion-without-recursion/
categories: [Заметки]
tags: [1С, Заметки, Желтый Чайник 1С]
image:
  path: cover.png
  in_post: false
links:
  top: false
  bottom: true
  values:
  - name: Заметка Telegra.ph 
    url: https://telegra.ph/Rekursiya-bez-rekursii-06-22
  - name: Пост в Telegram
    url: https://t.me/JuniorOneS/77
---

Часто используете рекурсию в 1С? Как же без неё. Иногда приходится делать рекурсивные методы, даже если очень этого не хочется. Например:

Нужно найти отбор поля в настройках компоновки данных. И, естественно, нужна рекурсия, ведь отбор может содержать группы с вложенными отборами. Обычно, это делается примерно так:

Находим элемент отбора с переданным полем:
![Скрин](01.png)

Но что делать, если нет возможности (или желания) делать рекурсивный код? Давайте попробуем обойтись без него (Никаких самовызовов):

![Скрин](02.png)

Назовём этот подход _"Рекурсия без рекурсии"_. Практически, мы добиваемся того же результата. Но рекурсию не используем.

Как это работает:

1. Объявляем массив, в котором будем содержать все коллекции, которые нужно перебрать
2. Добавляем в массив переданную в наш метод изначальную коллекцию
3. Пробегаем наш набор, а внутри сразу пробегаем все элементы. 
4. Если среди элементов мы так же находим коллекцию, то добавляем её в наш массив. 

Таким образом, все найденные коллекции добавляются "в очередь" и перебираются позднее. 

_Зачем это нужно?_

Бывают ситуации, когда нет возможности описать метод, но необходимо выполнить рекурсивный перебор. Например, в куске произвольного алгоритма.

`Выполнить(ТекстАлгоритма);`

Такое используется в шагах бизнес-процессов Документооборота. И в старой доброй Конвертации данных 2.0. В этом случае кусок кода будет выглядеть так:

![Скрин](03.png)

Этот кусок алгоритма делает рекурсивный перебор, но при этом не создаёт самой рекурсии, а значит и не требует создания отдельного метода. Теперь его можно свободно поместить в текст для метода Выполнить().

Однако, описанный в статье подход работает немного иначе, чем стандартный с рекурсией. Дело в том, что "рекурсивный" перебирает подчиненные элементы сразу, а "нерекурсивный" - добавляет новые коллекции в очередь. И обработает тогда, когда до них эта очередь дойдёт. Результат такой же, но скорость выполнения может отличаться. А уже в чью сторону - зависит от конкретной ситуации. 