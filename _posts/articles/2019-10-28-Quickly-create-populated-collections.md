---
title: Быстрое создание наполненных коллекций
date: 2019-10-28 10:00:00 +/-TTTT
description: Разберем самые частые способы создания коллекции, значения которой известны заранее. И сравним скорость их выполнения.
media_subpath: /assets/posts/articles/2019-10-28-Quickly-create-populated-collections/
categories: [Статьи]
tags: [1С, Статьи, Желтый Чайник 1С]
image:
  path: cover.jpg
links:
  top: true
  bottom: true
  values:
  - name: Статья на Инфостарт
    url: https://infostart.ru/1c/articles/1145469/?ref=1159
  - name: Пост в Telegram
    url: https://t.me/JuniorOneS/39
---

## Введение

> — А что это такое?

Программист Артём заметил у программиста Васи такой код:

```bsl
Для Каждого КоллекцияМетаданных Из СтрРазделить("Справочники,Документы", ",") Цикл

	//...

КонецЦикла;
```

Василий рассказал Артёму, что таким образом он быстро создал наполненный массив. Сделав код более коротким. Раньше было так:

```bsl
КоллекцииМетаданных = Новый Массив;
КоллекцииМетаданных.Добавить("Справочники");
КоллекцииМетаданных.Добавить("Документы");

Для Каждого КоллекцияМетаданных Из КоллекцииМетаданных Цикл

	//...

КонецЦикла;
```

Артём понял задумку Василия. Ведь никто так хорошо не поймёт ленивого программиста, как другой ленивый программист. Но заметил, что он бы делал немного по-другому. 

```bsl
Для Каждого КоллекцияМетаданных Из Новый Структура("Справочники,Документы") Цикл

	//...

КонецЦикла;
```

> — Так оптимальнее - сказал программист Артём, рассказывая что-то про внутренние механизмы платформы...

> — А вот и нет! - не согласился программист Вася, рассказывая что-то про оптимизацию метода СтрРазделить...

Так всё и началось.

## Быстрое создание наполненных коллекций

Помните, как бывает в других языках программирования?

- C#  
`string[] seasons = {"Winter", "Spring", "Summer", "Autumn"};`

- Java  
`String[] seasons = {"Winter", "Spring", "Summer", "Autumn"};`

- JavaScript  
`var seasons = ["Winter", "Spring", "Summer", "Autumn"];`

- Python  
`seasons = ['Winter', 'Spring', 'Summer', 'Autumn']`

Как же это бывает удобно - создать сразу наполненный значениями массив. Жаль, что в 1С такое не предусмотрено.

Но хорошо, что в нашем примере потребовались строковые массивы. Для этого в 1С есть способы:

### СтрРазделить()

```bsl
РезультатСоздания = СтрРазделить("Понедельник,Вторник,Среда,Четверг,Пятница,Суббота,Воскресенье", ",");
```

Кто-то скажет, что это не совсем то, что нам нужно — парсить строку для того, чтобы получить массив, значения которого заранее известны. Однако, на практике такой подход встречается довольно часто. 

_Управление торговлей 11.4.9.91_:

```bsl
ОбязательныеСтандартныеРеквизиты = СтрРазделить("Период,Регистратор,ВидДвижения,Активность", ",");
Для каждого РеквизитРегистра Из Коллекция.СтандартныеРеквизиты Цикл
```

_Бухгалтерия предприятия КОРП 3.0.72.7:_
```bsl
СвойстваНастроек = СтрРазделить("Использование, ВидСравнения, Значение", ", ", Ложь);
	Для Каждого СвойствоНастройки Из СвойстваНастроек Цикл 
```
```bsl
Для каждого ВидПериода Из СтрРазделить("ДатаПроизводства,СрокГодности",",") Цикл
```

И тут можно вспомнить, что метод `СтрРазделить()` появился только в версии `8.3.6`. Как же раньше программисты могли одной строчкой кода создать наполненный массив?

### РазложитьСтрокуВМассивПодстрок()

Выглядит это так:

```bsl
РезультатСоздания = РазложитьСтрокуВМассивПодстрок("Понедельник,Вторник,Среда,Четверг,Пятница,Суббота,Воскресенье", ",");
```

Да, ещё один парсинг. Только на основе БСПшной функции, которая существует в типовых испокон веков.

Удивительно, но до сих пор можно встретить примеры использования даже в свежих типовых конфигурациях:

_Бухгалтерия предприятия КОРП 3.0.72.70:_
```bsl
Для Каждого ЭлтКлюч Из СтроковыеФункцииКлиентСервер.РазложитьСтрокуВМассивПодстрок("_ДатаПрмЭ,_ДатаПрмБ,_ДатаДокОтв", ",") Цикл 
	СЗ.Добавить(ЭлтКлюч, Формат(Ответ[ЭлтКлюч], "ДФ=yyyyMMdd"));
КонецЦикла;
```

```bsl
Для Каждого Элт Из СтроковыеФункцииКлиентСервер.РазложитьСтрокуВМассивПодстрок("А%Б%В", "%") Цикл
```

_Зарплата и управление персоналом 3.1.11.106:_

```bsl
Для Каждого ЭлтКлюч Из СтроковыеФункцииКлиентСервер.РазложитьСтрокуВМассивПодстрок("_ДатаПрмЭ,_ДатаПрмБ,_ДатаДокОтв", ",") Цикл
```

Что лучше использовать в таком случае? Думаю, вопрос очевиден, учитывая, что платформенный метод, как заявляют разработчики, намного быстрее БСПшного. Так ли это? Проверим далее. Но не стоит забывать, что они не совсем идентичны. И об этом писалось в статье [Чем расщепить или СтрРазделить() VS РазложитьСтрокуВМассивПодстрок()]({% link _posts/articles/2019-06-06-StrSplit-vs-SplitStringIntoSubstringsArray.md %}) ?

### Новый Структура()

Да, умельцы-программисты использовали стандартный конструктор структуры как способ создавать наполненную коллекцию.

И да, этот способ имеет ряд ограничений:

- Результат будет структурой, а не массивом.
- Все значения должны соответствовать правилам именования 1С.
- Разделитель только запятая.

Все эти минусы, зачастую, не создают проблем и разработчики с удовольствием применяют данный подход.

Ну и, как обычно, примеры из типовых:

_Управление торговлей 11.4.9.91:_

```bsl
ВозвращаемыеСвойства = "НетЗаданий, ПерезапускОбновления, ПерезапускОбновленияСНачала, НаборПорций,
       |НовыйПоследнийОбновленныйЭлемент, НачальноеОбновлениеЗавершено, ТекстОшибкиЗавершения";

Для Каждого КлючИЗначение Из Новый Структура(ВозвращаемыеСвойства) Цикл
```

_Бухгалтерия предприятия КОРП 3.0.72.70:_

```bsl
КолонкиДоходов = "ЦенныеБумагиИтогоСуммаДоходов,ЦенныеБумагиСуммаДохода";
Для Каждого КолонкаДохода Из Новый Структура(КолонкиДоходов) Цикл
```

И тут становится уже интереснее. А что быстрее: создать массив при помощи парсинга строки `СтрРазделить()` или же создать структуру стандартным конструктором `Новый Структура()` ? Чего гадать, когда можно проверить!

## Разработка инструмента

Для этого создаём простенький отчет.

В СКД указываем набор данных объект с колонками, которые нужны будут для анализа:

![Скрин](01.png)

В параметрах указываем количество "подходов", количество созданий за один подход, ну и сами методы:

![Скрин](02.png)

Накидываем ресурсы:

![Скрин](03.png)

И делаем простой вывод диаграммы с детальными записями:

![Скрин](04.png)

В `ПриКомпоновкеРезультата()` будем делать все манипуляции.

Для начала отключим стандартную обработку и очистим результат:

```bsl
Процедура ПриКомпоновкеРезультата(ДокументРезультат, ДанныеРасшифровки, СтандартнаяОбработка)
	
	СтандартнаяОбработка = Ложь;
	ДокументРезультат.Очистить();
	
КонецПроцедуры
```

Получим параметры из настроек (добавим функцию):

```bsl
Процедура ПриКомпоновкеРезультата(ДокументРезультат, ДанныеРасшифровки, СтандартнаяОбработка)
	
	СтандартнаяОбработка = Ложь;
	ДокументРезультат.Очистить();
	
	ПараметрыВыполнения = ПараметрыВыполнения();
	
КонецПроцедуры

Функция ПараметрыВыполнения()
	
	ПараметрыВыполнения = Новый Структура;
	
	НастройкиКомпоновки	= КомпоновщикНастроек.ПолучитьНастройки();
	ПараметрыВыполнения.Вставить("НастройкиКомпоновки", НастройкиКомпоновки);
	
	Для Каждого ПараметрКомпоновщика Из НастройкиКомпоновки.ПараметрыДанных.Элементы Цикл
		Если ПараметрКомпоновщика.Использование Тогда
			ПараметрыВыполнения.Вставить(ПараметрКомпоновщика.Параметр, ПараметрКомпоновщика.Значение);
		КонецЕсли;
	КонецЦикла;
	
	Возврат ПараметрыВыполнения;

КонецФункции
```

Подготовим функцию для замеров:

```bsl
Процедура ПриКомпоновкеРезультата(ДокументРезультат, ДанныеРасшифровки, СтандартнаяОбработка)
	
	СтандартнаяОбработка = Ложь;
	ДокументРезультат.Очистить();
	
	ПараметрыВыполнения = ПараметрыВыполнения();

	ДанныеЗамеров = ДанныеЗамеров(ПараметрыВыполнения);
	
КонецПроцедуры

Функция ДанныеЗамеров(ПараметрыВыполнения)
	
	ДанныеЗамеров = Новый ТаблицаЗначений; //Просто заполним структуру таблицы из полей описанного нами набора схемы компоновки
	Для Каждого ТекущееПоле Из СхемаКомпоновкиДанных.НаборыДанных.Получить(0).Поля Цикл
		ДанныеЗамеров.Колонки.Добавить(ТекущееПоле.Поле, ТекущееПоле.ТипЗначения, ТекущееПоле.Заголовок);
	КонецЦикла;
	
	Если НЕ ЗначениеЗаполнено(ПараметрыВыполнения.Методы) Тогда
		Возврат ДанныеЗамеров;
	КонецЕсли;
	
	//Здесь будем замерять
	
	Возврат ДанныеЗамеров;
	
КонецФункции
```

И сразу же не забудем вывести результат в табличный документ.

Для удобства будем использовать процедуру, описанную в статье [Меньше копипаста!, или как Вася универсальную процедуру писал]({% link _posts/articles/2021-06-29-Less-copypasta-or-how-Vasya-wrote-universal-procedure.md %})

``bsl
Процедура ПриКомпоновкеРезультата(ДокументРезультат, ДанныеРасшифровки, СтандартнаяОбработка)
	
	СтандартнаяОбработка = Ложь;
	ДокументРезультат.Очистить();
	
	ПараметрыВыполнения = ПараметрыВыполнения();
	
	ДанныеЗамеров = ДанныеЗамеров(ПараметрыВыполнения);
    
    ВнешниеНаборыДанных = Новый Структура("Результат", ДанныеЗамеров);
    
	СкомпоноватьРезультатОтчета(ДокументРезультат, СхемаКомпоновкиДанных, 
	ПараметрыВыполнения.НастройкиКомпоновки, ДанныеРасшифровки, ВнешниеНаборыДанных);
	
КонецПроцедуры
```

Всё, база готова. Уже сейчас отчёт может выводиться, хоть и без замеров. Пора заняться ими! 

Подготовим циклы. Будем сначала выполнять подходы, а потом для каждого подхода будем выполнять каждый метод:

```bsl
Функция ДанныеЗамеров(ПараметрыВыполнения)
	
	ДанныеЗамеров = Новый ТаблицаЗначений;    //Просто заполним структуру таблицы из полей описанного нами набора схемы компоновки
	Для Каждого ТекущееПоле Из СхемаКомпоновкиДанных.НаборыДанных.Получить(0).Поля Цикл
		ДанныеЗамеров.Колонки.Добавить(ТекущееПоле.Поле, ТекущееПоле.ТипЗначения, ТекущееПоле.Заголовок);
	КонецЦикла;
	
	Если НЕ ЗначениеЗаполнено(ПараметрыВыполнения.Методы) Тогда
		Возврат ДанныеЗамеров;
	КонецЕсли;
	
	Для ТекущийПодход = 1 По ПараметрыВыполнения.КоличествоПодходов Цикл
		
		Для Каждого ТекущийМетод Из ПараметрыВыполнения.Методы Цикл
			
			
			
		КонецЦикла;
		
	КонецЦикла;
	
	Возврат ДанныеЗамеров;
	
КонецФункции
```

Добавим новую строчку замера и заполним текущими данными
```bsl
		Для Каждого ТекущийМетод Из ПараметрыВыполнения.Методы Цикл
			
			ТекущиеДанные = ДанныеЗамеров.Добавить();
			ТекущиеДанные.Метод         = ТекущийМетод.Значение;
			ТекущиеДанные.ТекущийПодход = ТекущийПодход;
			ТекущиеДанные.КоличествоСозданий = ПараметрыВыполнения.КоличествоСозданий;
			
		КонецЦикла;
```

Пока пропустим ненадолго момент с самими замерами. Представим, что они выполнены. Теперь нам нужно рассчитать количество созданий экземпляра коллекции за одну миллисекунду:

```bsl
			Если ТекущиеДанные.ОбщееВремяСозданий > 0 Тогда
				ТекущиеДанные.КоличествоСозданийЗаМилисекунду = Окр(ТекущиеДанные.КоличествоСозданий / ТекущиеДанные.ОбщееВремяСозданий, 2);
			Иначе
				ТекущиеДанные.КоличествоСозданийЗаМилисекунду = ТекущиеДанные.КоличествоСозданий;
			КонецЕсли;
```

База готова. Теперь просто выполним замеры каждого метода.

```bsl
			Если ТекущиеДанные.Метод = "СтрРазделить" Тогда
				
				ЗамеритьМетодСтрРазделить(ТекущиеДанные);
				
			ИначеЕсли ТекущиеДанные.Метод = "НовыйСтруктура" Тогда
				
				ЗамеритьМетодНовыйСтруктура(ТекущиеДанные);
				
			ИначеЕсли ТекущиеДанные.Метод = "РазложитьСтрокуВМассивПодстрок" Тогда
				
				ЗамеритьМетодРазложитьСтрокуВМассивПодстрок(ТекущиеДанные);
			
            ИначеЕсли ТекущиеДанные.Метод = "НовыйМассив" Тогда
				
				ЗамеритьМетодНовыйМассив(ТекущиеДанные);

			КонецЕсли;
```

Ну и, собственно, сами методы:

```bsl
Процедура ЗамеритьМетодСтрРазделить(ТекущиеДанные)
	
	НачалоЗамера = ТекущаяУниверсальнаяДатаВМиллисекундах();
	
	Для ТекущееСоздание = 1 По ТекущиеДанные.КоличествоСозданий Цикл
		РезультатСоздания = СтрРазделить("Понедельник,Вторник,Среда,Четверг,Пятница,Суббота,Воскресенье", ",");
	КонецЦикла;
	
	ТекущиеДанные.ОбщееВремяСозданий = ТекущаяУниверсальнаяДатаВМиллисекундах() - НачалоЗамера;
		
КонецПроцедуры

Процедура ЗамеритьМетодРазложитьСтрокуВМассивПодстрок(ТекущиеДанные)
	
	НачалоЗамера = ТекущаяУниверсальнаяДатаВМиллисекундах();
	
	Для ТекущееСоздание = 1 По ТекущиеДанные.КоличествоСозданий Цикл
		РезультатСоздания = РазложитьСтрокуВМассивПодстрок("Понедельник,Вторник,Среда,Четверг,Пятница,Суббота,Воскресенье", ",");
	КонецЦикла;
	
	ТекущиеДанные.ОбщееВремяСозданий = ТекущаяУниверсальнаяДатаВМиллисекундах() - НачалоЗамера;
		
КонецПроцедуры

Процедура ЗамеритьМетодНовыйСтруктура(ТекущиеДанные)
	
	НачалоЗамера = ТекущаяУниверсальнаяДатаВМиллисекундах();
	
	Для ТекущееСоздание = 1 По ТекущиеДанные.КоличествоСозданий Цикл
		РезультатСоздания = Новый Структура("Понедельник,Вторник,Среда,Четверг,Пятница,Суббота,Воскресенье");
	КонецЦикла;
	
	ТекущиеДанные.ОбщееВремяСозданий = ТекущаяУниверсальнаяДатаВМиллисекундах() - НачалоЗамера;
		
КонецПроцедуры

Процедура ЗамеритьМетодНовыйМассив(ТекущиеДанные)
	
	НачалоЗамера = ТекущаяУниверсальнаяДатаВМиллисекундах();
	
	Для ТекущееСоздание = 1 По ТекущиеДанные.КоличествоСозданий Цикл
			
		РезультатСоздания = Новый Массив;
		РезультатСоздания.Добавить("Понедельник");
		РезультатСоздания.Добавить("Вторник");
		РезультатСоздания.Добавить("Среда");
		РезультатСоздания.Добавить("Четверг");
		РезультатСоздания.Добавить("Пятница");
		РезультатСоздания.Добавить("Суббота");
		РезультатСоздания.Добавить("Воскресенье");
	
	КонецЦикла;
	
	ТекущиеДанные.ОбщееВремяСозданий = ТекущаяУниверсальнаяДатаВМиллисекундах() - НачалоЗамера;
	
		
КонецПроцедуры

```

Здесь стоит пояснить, что можно было реализовать без повторяющегося кода. Однако, в данной реализации всё сделано специально, чтобы было как можно меньше сомнений в одинаковых условиях выполнения методов. Пусть лучше мы сделаем не такой классный и красивый код, как хотелось бы, зато получим более точные результаты.

Ну всё. Инструмент готов! 

## Сравнение

Пробуем запустить замеры на серверной базе. Для этого поочередно каждый метод в цикле создаёт по N раз одну и ту же коллекцию из заранее известных элементов. И так указанное количество «подходов».

Почему одну и туже? Потому что в этом и суть эксперимента — показать, как будут работать такие создания, если вы где-то в коде захотите быстро создать наполненную заранее известными значениями коллекцию. И как будут работать примеры типовых, указанные выше.

Для начала установим:

- Количество созданий в одном подходе: 10 000
- Количество подходов: 10

### СтрРазделить() и РазложитьСтрокуВМассивПодстрок()

И глянем, насколько по скорости отличаются `СтрРазделить()` и `РазложитьСтрокуВМассивПодстрок()`.

Код создания:

`РезультатСоздания = СтрРазделить("Понедельник,Вторник,Среда,Четверг,Пятница,Суббота,Воскресенье", ",");`

`РезультатСоздания = РазложитьСтрокуВМассивПодстрок("Понедельник,Вторник,Среда,Четверг,Пятница,Суббота,Воскресенье", ",");`

Результат неудивителен. Количество создания коллекции за одну миллисекунду у СтрРазделить() значительно выше:

![Скрин](05.png)

Было 10 подходов. В каждый подход коллекция создавалась по 10 000 раз. В каждый подход количество создаваемых элементов (10 000) делились на общее затраченное время на их создание.

В принципе, не удивительно. Метод `СтрРазделить()` был специально оптимизирован так, чтобы заменить БСПшный `РазложитьСтрокуВМассивПодстрок()`.

Выходит, `РазложитьСтрокуВМассивПодстрок()` для подобного использовать не оптимально (интересно, почему в типовых до сих пор встречаются примеры). Но что покажет сравнение между `СтрРазделить()` и `Новый Структура()` ? 

### Новый Структура()

Добавим в сравнение и этот метод:

`РезультатСоздания = Новый Структура("Понедельник,Вторник,Среда,Четверг,Пятница,Суббота,Воскресенье");`

Результаты замеров получились такие:

![Скрин](06.png)

И так, на графике видно, помимо того, что сервер периодически «проседает», и то, что `СтрРазделить()` лидирует в этом сравнении.

Даже в «худшие времена» `СтрРазделить()` срабатывает быстрее, чем `Новый Структура()` в лучшие.

Почему так?

Мы можем лишь предположить, что структура — это более затратная коллекция, чем массив. Насколько это верный вывод лучше уточнять у знатоков всея нутра платформы 1С.

И так, что имеем на данный момент? Вася, который утверждал, что для наших целей `СтрРазделить()` аки лучше, чем `Новый Структура()`, оказался прав. С точки зрения скорости этот метод будет работать быстрее. Естественно, что выигрыш в производительности на вряд ли позволит нам сказать, что пользователи будут прыгать от радости.

Вроде Вася уже собрался открывать бутылку шампанского, но тут подходит Он. Оптимальный Программист. Священник Восьми Платформ. Хранитель Желтых Талмудов и Свидетель Великого Ассемблера.

> — Костыли всё это. Никаких созданий наполненного массива быть не должно! Зачем дополнительно напрягать вычислительную машину?

### Новый Массив()

Эта статья нужна именно для того, чтобы сравнить между собой создания коллекций с заранее известными элементами. И Гуру программирования на нулях и единицах настаивает использовать только Новый Массив с последующими добавлениями элементов.

```bsl
	РезультатСоздания = Новый Массив;
	РезультатСоздания.Добавить("Понедельник");
	РезультатСоздания.Добавить("Вторник");
	РезультатСоздания.Добавить("Среда");
	РезультатСоздания.Добавить("Четверг");
	РезультатСоздания.Добавить("Пятница");
	РезультатСоздания.Добавить("Суббота");
	РезультатСоздания.Добавить("Воскресенье");
```

И так. Принцип анализа тот же. Только теперь будем сравнивать четыре метода. Без особого желания, ведь победитель ясен. Создать и наполнить массив таки лучше, чем парсить строку и разбивать её на элементы...

![Скрин](07.png)

Результат нас удивил.

Мы перепроверили. На разных серверах.

Оказывается, что `СтрРазделить()` быстрее, чем `Новый Массив()`. Более того, `Новый Массив()` ещё и борется за третье место с `Новый Структура()`.

На создание и наполнение массива уходит больше времени, чем на парсинг строки при помощи `СтрРазделить()`.

Пока мы удивлялись происходящему, к нам подплыл на волнах торсионных полей эзотерик от мира программирования Инокентий.

> — Я знаю как вам помочь.

_Продолжение следует..._


## Заключение
В данной статье мы рассмотрели основные способы создания коллекции с заранее известными строковыми значениями. Примеры их использования есть и в типовых и, тем более, в всякого рода самописок и самодописок. По результатам замера вы сами можете сделать вывод. Разница есть, но не существенная. Какой подход использовать? Решать вам. Однако, на мой сугубо личный взляд, `СтрРазделить` выглядит более лаконично. Так что, если вы как-нибудь задумаетесь, мол, «этот массив из трех строк будет создаваться 100500 раз» , то не переживайте — написав его обычным созданием массива вы вряд ли предотвратите образование узкого места будущего кода.

В следующей статье разберем более «эзотерические» методы создания наполненного массива. И сравним их результаты.