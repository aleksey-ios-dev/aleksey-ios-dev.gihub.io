---
layout: post
title:  "Effective localization key naming"
date:   2014-11-22 11:57:07 +0200
categories: code style
---
The naming style of localization string keys is often given little attention, as the primary goal of a programmer’s efforts is reliable code, and matters of taste are not measurable and do not make it onto the checklist for completion. For small applications, this might be true. However, larger projects follow different rules, and agreements on naming strings can help not only make the code cleaner but also avoid errors, thus saving time on translation and, ultimately, bringing direct benefits. Below, I will share my thoughts on effective string naming.

The simplest form a localization key can take is the string itself. In this case, if there is no localized version, the user will see the key itself, in other words, the text in English.

```c
"Hello!" = "Bonjour!";
"Login" = "Login";
```

When the localization file reaches the translator, they will encounter a host of questions. For example, the word play could refer to a genre of musical works in a list of categories or be the label on a play button. Moreover, even if the translator guesses the correct meaning, gender, tense, and number remain unclear in languages where these categories apply. Clearly, the key needs context. Context can be provided, for instance, through a comment:

`NSLocalizedString("play", comment: "play button title")`

However, comments are only visible where the string is declared, which means the programmer will also have to work on the translation alongside the translator. One of my favorite TRIZ (Theory of Inventive Problem Solving) rules states:

>The ideal final result (IFR) is achieved when the useful function is performed without any mechanism, meaning the system performs its function on its own, without or with minimal costs.

Let the key itself serve as a hint for the translator!

```c
"welcome_screen.hello" = "Bonjour!";
"welcome_screen.login" = "Login";
```
Much better! Now, by looking at the key, the translator will know what the string refers to and how to translate it. Let’s continue improving. The label on the `welcome_screen.login button` can be translated either as a verb or as a noun. Let’s also add information about the type of element for which the text is being translated.

```c
"welcome_screen.hello" = "Bonjour!";
"welcome_screen.action.login" = "Login"
```
Although the element on the screen is a button, I recommend using labels based on meaning. The control element may change, but the essence of the action does not.

Let’s return to the greeting. Will the application greet every user in the same way? For example, a dating app inevitably has a detailed profile of the user. Why not use this knowledge to personalize the dialogue? Different communication styles can be handled by using separate localization files, isolating all the “cultural language” of the application in a style file. Therefore, it’s better to replace hello with more general `greeting`:

```c
"welcome_screen.greeting" = "Bonjour";
"welcome_screen.action.login" = "Login"
```

So much better! Here are a few more general categories for describing user-system interaction elements:
`action`, `message`, `prompt`, `placeholder`, `warning`, `disclaimer`, `footnote`, `call_to_action`. It’s much more informative than the “opaque” terms like `button` and `label`!

I intentionally avoid using capital letters, and separate parts of the key with periods. Keys can be quite long, so it’s important to maintain readability and eliminate visual clutter. Compare:

```c
"PaymentConfirmationDialog.Action.ConfirmPayment" = "Confirm";
"payment_confirmation_dialog.action.confirm_payment" = "Confirm";
```

Here are a few examples of what I believe are good keys for inspiration:

```c
"lockout.action.buy_for_price" = "Buy for %@"; // note that the key hints at the presence of a parameter.
"car_details.action.view_bookings" = "Bookings"; // here, the key is even more descriptive than the text itself
"location_status.recent" = "Recent location";
"address_editor.field.area" = "Locality/Area";
```

In the absence of clear rules, common sense becomes the compass. Generally, the localization file in a project is a dark corner, a repository of random, ad-hoc decisions. However, since localization is usually handled by third-party companies, this document will, at some point, be under the spotlight. The translation document must be clear, comprehensive, and unambiguous. I hope the rules I’ve suggested will also assist you in translating applications into other languages


<br><br><br><br><br><br>



Стилю именования ключей локализованных строк обычно придают мало значения, ведь цель усилий программиста — надежно работающий код, а вопросы вкуса не подлежат измерению и не попадают в список критериев сдачи.

Для небольших приложений это, пожалуй, так. Однако большие проекты подчиняются иным правилам, и договоренности об именовании строк помогут не только сделать код опрятнее, но и избежать ошибок, тем самым сэкономить время на перевод, а значит, принести прямую выгоду.

Ниже я поделюсь собственными соображениями об эффективном именовании строк.

Самое простое, чем может быть ключ локализации, это сама строка.
В таком случае если локализованной версии нет, пользователь увидит сам ключ, иными словами текст на английском. 

```c
"Hello!" = "Bonjour!";
"Login" = "Login";
```

Когда файл локализаций попадет к переводчику, у последнего возникнет масса вопросов. Например,  слово `play` может оказаться как жанром музыкальных произведений в списке категорий, так и подписью на кнопке воспроизведения. Более того, даже если переводчик угадает значение, неясным остается гендер, время и число в тех языках, к которым эти категории применимы. Очевидно, ключу необходим контекст. Контекст может быть передан, к примеру, через комментарий.
`NSLocalizedString("play", comment: "play button title")`
Однако, комментарии видны лишь в месте объявления строки, и значит, вместе с переводчиком над переводом придется трудиться и программисту.

Одно из моих любимых правил ТРИЗ гласит:
>Идеальный конечный результат (ИКР) достигается тогда, когда полезная функция выполняется без какого-либо механизма, т.е. система выполняет свою функцию сама, без затрат или с минимальными затратами.

Пусть ключ сам и будет подсказкой для переводчика!

```c
"welcome_screen.hello" = "Bonjour!";
"welcome_screen.login" = "Login";
```
Гораздо лучше! Теперь, глядя на ключ, переводчик будет знать к чему относится строка и как ее переводить. Продолжим улучшать. Подпись кнопки `welcome_screen.login` может быть переведена двояко: либо как глагол-команда, либо как существительное. Добавим также знание о качестве элемента, для которого переводится текст
```c
"welcome_screen.hello" = "Bonjour!";
"welcome_screen.action.login" = "Login"
```
Хотя элемент на экране это кнопка, я рекомендую использовать обозначения по смыслу. Элемент управления может поменяться, а суть действия — нет.

Вернемся к приветствию. С каждым ли пользователем приложение будет здороваться одинаково? К примеру, приложение-дейтинг неизбежно владеет точным портретом пользователя. Почему бы не воспользоваться знанием о собеседнике для индивидуализации диалога?
Для различных стилей общения можно использовать разные файлы локализации, изолируя в файле стиля весь "культурный язык" приложения. 
Поэтому `hello` лучше заменить более общим обозначением:
```c
"welcome_screen.greeting" = "Bonjour";
"welcome_screen.action.login" = "Login"
```

 Так значительно лучше! Вот еще несколько общих категорий для описания  элементов взаимодействия пользователя и системы:
`action`, `message`, `prompt`, `placeholder`, `warning`, `disclaimer`, `footnote`, `call_to_action`. Список можно продолжать.
Согласитесь, гораздо более информативно, чем "непрозрачные" `button` и `label`.

Я нарочно избегаю заглавных букв и разделяю части ключа точками. Ключи могут быть достаточно длинными, важно сохранить легкость чтения и избавиться от визуального шума. Сравните:

```c
"PaymentConfirmationDialog.Action.ConfirmPayment" = "Confirm";
"payment_confirmation_dialog.action.confirm_payment" = "Confirm";
```

Несколько примеров хороших, на мой взгляд, ключей для вдохновения:
```c
"lockout.action.buy_for_price" = "Buy for %@"; // обратите внимание, что ключ намекает на наличие параметра
"car_details.action.view_bookings" = "Bookings"; // здесь ключ даже более описателен, чем само действие
"location_status.recent" = "Recent location";
"address_editor.field.area" = "Locality/Area";
```

Там, где нет четких правил, компасом становится здравый смысл. Как правило, файл локализаций в проекте это темный угол, склад случайных сиюминутных решений. Однако локализацией обычно занимаются сторонние компании, и значит, в определенный момент именно на этот документ будет направлен свет всех прожекторов. Документ переводов должен быть понятным, исчерпывающим, не допускать разночтений. Осмелюсь надеяться, предложенные мной правила помогут и вам в переводах приложений на другие языки. 

