---
title: Коллекция кода на 1С
description: Личная коллекция универсальных (не зависящих от конфигурации) методов.
date: 2024-08-01 15:00:00 +/-TTTT
media_subpath: /assets/posts/projects/CollectionMethodsOneS/
categories: [Проекты]
tags: [1С, Проект, Желтый Чайник 1С]
image:
  path: cover.jpg
links:
  bottom: true
  top: true
  values:
  - name: Проект на GitHub
    url: https://github.com/SeiOkami/CollectionMethodsOneS
  - name: Поиск методов
    url: https://github.com/SeiOkami/CollectionMethodsOneS/issues?q=label%3A%D0%9E%D0%BF%D1%83%D0%B1%D0%BB%D0%B8%D0%BA%D0%BE%D0%B2%D0%B0%D0%BD
  - name: Пост в Telegram
    url: https://t.me/JuniorOneS/639
---


## Коллекция кода на 1С (Желтый Чайник 1С)

Данный репозиторий - просто личная коллекция универсальных (не зависящих от конфигурации) методов.
Библиотека построена по принципу минимальных зависимостей. Каждый метод старается не вызывать другие методы библиотеки, чтобы его легче было скопировать в свои инструменты.

### Структура проекта

Код в формате EDT в виде двух расширений. Основное с методами и техническое с тестами на движке YaxUnit.

Некоторые разработки могут точечно отходить от ниже указанных правил, но в целом каждый метод:

- Отдельный и независимый кусочек кода. Чтобы удобнее было копировать в свои инструменты и не приходилось тянуть кучу зависимостей.
- Имеет страницу в issues с актуальной версией. Чтобы удобнее было искать методы без скачивания проекта. На странице вся история, комментарии, лайки, прочие ссылки и так далее.
- Содержит unit-тест(ы) на YaxUnit.

### Поиск методов

Все методы, которые имеют завершенную версию, содержат тег "Опубликован". По нему стоит искать, чтобы отсечь заготовки и технические issues.

Поиск по коллекции методов можно делать по данной ссылке:  
**[Реестр методов](https://github.com/SeiOkami/CollectionMethodsOneS/issues?q=label%3A%D0%9E%D0%BF%D1%83%D0%B1%D0%BB%D0%B8%D0%BA%D0%BE%D0%B2%D0%B0%D0%BD)**

### Дисклеймер

Ахтунг, некоторые стандарты разработки 1С были специально нарушены в связи с особенностями проекта, который не предполагает использование "как есть" (полное внедрение).

Проект - место хранения небольших методов. Поэтому и не имеет смысла, например, плодить отдельные модули на Сервер, Клиент, КлиентСервер и так далее.

Все модули по умолчанию имеют флаги Клиент и Сервер, а внутри при необходимости используются инструкции предпроцессора.

В общем, это просто личная коллекция "универсального" кода, которую я решил вести в специальном проекте с возможностью покрытия unit-тестами.
