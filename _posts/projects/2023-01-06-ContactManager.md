---
title: Менеджер контактов на C#
date: 2023-01-06 01:22:00 +/-TTTT
description: Проект для обучения разработки на C#
media_subpath: /assets/posts/projects/ContactManager/
categories: [Проекты]
tags: [Проекты, C#]
image:
  path: cover.png
links:
  bottom: true
  top: true
  values:
  - name: Проект на GitHub
    url: https://github.com/SeiOkami/ContactManager
---

## Что это?

Просто проект, который я разрабатывал в рамках обучения на C#

## Что умеет?

* Менеджер контактов. Позволяет зарегистрироваться и хранить свои контакты
* Функция импорта и экспорта в файл
* Возможность генерации тестовых контактов
* Веб и десктоп приложение
* Админ имеет возможность просмотра данных пользователей

## Из чего сделано?

* CQRS
* ASP NET Core 6
* EF Core 6 (SQLite)
* MediatR
* Automapper
* Web API
* Swagger
* Отдельный IdentityServer4
* MVC
* WPF

## Как запустить?

* Открыть Contacts.sln
* Выполнить мультизапуск проектов

Все базы самостоятельно создаются. Сервер идентификации при старте имеет пользователей:

* admin: admin
* alice : Pass123$
* bob : Pass123$

Можно регистрироваться и создавать своих.
