---
title: Недокументированная возможность генератора случайных чисел.
date: 2022-02-02 01:22:00 +/-TTTT
media_subpath: /assets/posts/notes/2022-02-02-nedok-ficha-generatora-chisel/
categories: [Заметки]
tags: [1С, Заметки, Желтый Чайник 1С]
image:
  path: 01.jpg
  in_post: false
links:
  top: true
  bottom: false
  values:
  - name: Пост в Telegram
    url: https://t.me/JuniorOneS/339
---

При помощи дробного числа в границе можно указать вероятность выпадения числа.

Например, здесь мы регулируем вероятность выпадения числа 1:

```bsl
ГСЧ.СлучайноеЧисло(0, 0.1); 
ГСЧ.СлучайноеЧисло(0, 0.01);
ГСЧ.СлучайноеЧисло(0, 0.001);
```

Чем меньше дробная часть, тем меньше шансов выпадения.

P.S.: думаю, вы понимаете, что не стоит пользоваться этой багофичей на реальных проектах 😁

`ГСЧ.СлучайноеЧисло(0, 0.1);`

![Скрин](01.jpg)

`ГСЧ.СлучайноеЧисло(0, 0.01);`

![Скрин](02.jpg)

`ГСЧ.СлучайноеЧисло(0, 0.001);`

![Скрин](03.jpg)