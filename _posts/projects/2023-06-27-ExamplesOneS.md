---
title: Примеры работы с новыми возможностями 1С
date: 2023-06-27 01:22:00 +/-TTTT
description: Репозиторий содержит примеры работы с новыми возможностями 1С.
media_subpath: /assets/posts/projects/ExamplesOneS/
categories: [Проекты]
tags: [1С, Проекты, Желтый Чайник 1С]
image:
  path: cover.jpg
links:
  bottom: false
  top: true
  values:
  - name: Проект на GitHub
    url: https://github.com/SeiOkami/ExamplesOneS
  - name: Пост в Telegram
    url: https://t.me/JuniorOneS/549
---

##  О репозитории

Репозиторий содержит примеры работы с новыми возможностями 1С.

![](01.jpg)

На [странице релизов](https://github.com/SeiOkami/ExamplesOneS/releases) можно скачать отдельно каждый инструмент, но рекомендуется брать полную конфигурацию, потому что ряд возможностей платформы находится в глобальных событиях и свойствах.

На [странице ишуизов](https://github.com/SeiOkami/ExamplesOneS/issues) можно зарегистрировать ошибку или же добавить предложение по улучшению.

## Работа с буфером обмена (8.3.24+)

![Работа с буфером обмена](01.png){: .shadow }

Инструмент для тестирования программного взаимодействия с буфером обмена.

- Примеры работы разных форматов
- Копирование и вставка табличной части (текстом, HTML и содержимым с ссылками)
- Обработка события при вставке из буфера
- Анализ содержимого буфера (с учетом всех доступных MIME-форматов).

[Обзор работы с буфером обмена в платформе]({% link _posts/articles/2023-06-27-bufer-obmena-ones-obzor.md %})

## Проверка регулярных выражений (8.3.23+)

![Проверка регулярных выражений](02.png){: .shadow }

Инструмент для тестирования встроенного в платформу 1С функционала регулярных выражений

- Просмотр результата поиска и замены выражений
- Примеры работы с методами
- Сравнение с RegExp по скорости и результатам

[Обзор работы с регулярными выражениями в платформе]({% link _posts/notes/2022-12-23-regular-expressions-ones-note.md %})

## Установка основного оформления отчетов 1С (8.3.22+)

![Установка основного оформления отчетов 1С](03.png){: .shadow }

Возможности обработки:

- Установить общий основной макет оформления для всех пользователей
- Установить основной макет оформления для себя (текущего пользователя)
- Если вы администратор, то появляется возможность установить макеты оформления для других пользователей. Т.е. администрировать настройки в базе.

Важный нюанс! Настройка влияет и на программные формирования СКД. Т.е. если где-то вы создаете печатную форму при помощи СКД, то убедитесь, что в ней указан требуемый макет оформления. Иначе у пользователя печатная форма будет окрашена =)

[Обзор новой возможности в платформе]({% link _posts/articles/2023-05-26-osnovnoy-maket-skd.md %})
