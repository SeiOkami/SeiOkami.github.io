---
title: Острожнее с Асинх!
description: Особенности асинхронных методов
date: 2023-08-18 01:22:00 +/-TTTT
media_subpath: /assets/posts/notes/2023-08-18-ostorozhnee-s-asinh/
categories: [Заметки]
tags: [1С, Заметки, Желтый Чайник 1С]
image:
  path: cover.jpg
  in_post: false
links:
  top: false
  bottom: true
  values:
  - name: Пост в Telegram
    url: https://t.me/JuniorOneS/557
---

 Серию постов про нюансы Асинх методов начинаем с асинхронных процедур.

😁 Если коротко, то **не создавайте их**

У асинхронных **процедур** есть очень неприятный нюанс. Их невозможно *Ждать*. ⏲
Так как процедуры не возвращают результат, то и Ждать их результата невозможно.

💬Например, у нас есть процедура, которая заполняет дерево на форме. 
Заполняет она его из файла, предварительно асинхронно проверяя его существование при помощи `Ждать Файл.СуществуетАсинх()`

❗️ Такую процедуру **нельзя вызвать и подождать**.
Нельзя где-то в коде вызвать вашу процедуру "`ЗаполнитьДеревоИзФайла`", а сразу после что-то пытаться сделать с этим самым деревом. Потому что оно ещё не будет заполнено.

💡 Поэтому, если вы создаёте асинхронную процедуру (метод, который ничего не должен возвращать), то подумайте хорошенько... и сделайте её Функцией.
Пусть лучше ваш метод бессмысленно вернёт Неопределено или Истина. Зато тогда его можно будет подо**Ждать**

👍 Кстати, ещё хорошей практикой будет в конце названия метода добавить "Асинх". Тогда разработчик, который вызывает метод, точно поймёт, что он асинхронен. 
Заботьтесь о тех, кто будет работать с вашим кодом после вас ❤️

![](cover.jpg)