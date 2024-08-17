---
layout: post
title:  "Misconceptions about MVC"
date:   2014-11-22 11:57:07 +0200
categories: hands on coding
---
The Cocoa Touch ecosystem offers a vast array of tools for displaying data. Developers have access to components ranging from the low-level CALayer to high-level abstractions like UINavigationController. For data representation and storage, there’s a whole framework, Core Data. However, there is no abstraction level specifically dedicated to the core of an application that addresses business logic.

This freedom has given rise to a whole family of architectural patterns, each with its proponents and critics, advantages, and disadvantages. One area where opinions often converge is that MVC is seen as the least effective approach. It doesn’t support effective separation of concerns, leading to tangled code and errors.

However, Apple itself promotes the MVC pattern, or Model View Controller, as “the key to good Cocoa design.”

Let’s address some common misconceptions about MVC. These misconceptions, in my opinion, lead to a misunderstanding and misuse of MVC, preventing its advantages from being fully appreciated.

**Misconception 1: MVC is an architecture**

In discussions about which design pattern is best, MVC, MVVM, MVP, or VIPER are often referred to as architectures. This is incorrect. An architectural style describes the overarching principles of a system’s operation and approaches to solving core business problems. Structural patterns, on the other hand, are applied as needed. A single software product may use various structural patterns that are most suitable for the situation. Therefore, patterns should be viewed as tools for solving problems rather than religious doctrines or philosophies.

**Misconception 2: MVC does not address separation of concerns**

In Apple’s examples and guides, the logical layer is always handled by either the data model or the view controller. This simplification is likely for ease of explanation. However, this omission leads to the perception that MVC involves both the view and data storage layers handling important business logic.

Apple describes the Model layer as:

>Model objects encapsulate the data specific to an application and define the logic and computation that manipulate and process that data.

According to Apple’s definition, the Model layer handles data operations, but a model object doesn’t have to be a data object itself.

Thus, the Model layer serves as the core of the program where all business operations take place, with data handling as a component of these operations.

The issue of displaying the model’s state arises. Some actions happen directly on the screen, which inevitably leaks logic into the ViewController.

One solution is the active model approach, where the model not only performs work but also uses bindings to provide the view with the necessary data for display.

**Misconception 3: MVC and MVC with an active model are two different approaches**

In reality, they are not, and the diagram on Apple’s site clearly indicates this:
<img src="/assets/0002_mvc_misconceptions/mvc_apple.png" alt="MVC diagram" width="75%"/>

The method of notification is not explicitly stated. It could be delegation or, more appropriately, KVO.

**Apple’s Concept**

The conclusions below are just my understanding, but extensive practice confirms its correctness.

The foundation of constructing a flexible, extensible, and maintainable software system lies in the principle of separating logic from presentation.

MVC achieves this with the minimal set of elements in the Cocoa ecosystem:

M — the application as understood by its user. Reminders of birthdays, shopping in stores, storing and creating notes, all tasks related to the business problem are performed by objects in this layer. They interact with peripherals or services such as data storage, network clients, etc.

V — the view. The view updates in response to changes in the corresponding logic object.

C — perhaps the most complex layer to understand. I can imagine a program that only uses logic and presentation. UIKit doesn’t forbid placing arbitrary views directly on UIWindow and managing them from an associated logic class. However, the ViewController hierarchy was originally designed as a bus for broader interaction of views beyond just screen placement. Lifecycle control and response to application events are tasks not directly related to presentation, and it makes sense to separate views and view controllers. Since such a bus exists, it can be enhanced to serve as a link between logic and presentation, performing binding configuration and response handling.

**Not only view**

The name “View” in the MVC pattern leads one to think of it as UIView or UIViewController at the very least. However, View can be understood more broadly, as “representation” or a way to present information to the user and handle data input. For example, a voice menu with speech synthesis or a screen with Braille alphabet. In this case, the controller layer will likely undergo significant changes, serving as an adapter to the alternative representation system, while the core logic layer may remain unchanged.

**Conclusion**

MVC is recommended as an optimal pattern because it achieves the separation of a software system into layers with a minimal set of tools. In specific cases where needed, MVC can be supplemented and extended, but even in complex structural solutions, the separation of logic and presentation will still be implemented, and thus MVC will implicitly be present.

#### Resources:

<a href="https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/MVC.html">Model-View-Controller</a>








----

Экосистема Cocoa Touch предлагает обширнейший набор средств для отображения данных. В распоряжении разработчика компоненты как самого низкого уровня, начиная с CALayer, и до самых высокоуровневых абстракций, например, UINavigationController. Представлению и хранению данных посвящен целый фреймворк Core Data. Однако для самой сути приложения как программы, решающей бизнес-задачу, уровня абстракции не предусмотрено.
Такая свобода действий породила целое семейство структурных шаблонов. У каждого есть свои защитники и противники, преимущества и недостатки. 
И в одном, пожалуй, мнения сходятся: MVC это самый неудачный подход. Он не предполагает эффективного разделения ответственностей, что приводит к путаному коду и ошибкам.

Тем не менее шаблон MVC, или Model View Controller, самой компанией Apple подается как "залог хорошего дизайна приложения Cocoa".

Давайте рассмотрим несколько распространенных заблуждений об MVC. Заблуждений, из-за которых его, на мой взгляд, неверно понимают, неверно используют, и потому не могут оценить его преимуществ.

#### **Заблуждение 1: MVC это архитектура**
В баталиях на тему какой шаблон проектирования лучше, часто можно слышать, как MVC, MVVM, MVP или VIPER называют именно архитектурой.
Это не так. Архитектурный стиль, как и в реальном мире, описывает общие принципы функционирования системы, подходы к решению основных бизнес-задач. Структурные же шаблоны применяются по месту. В одном программном  продукте могут быть использованы различные структурные шаблоны, наиболее подходящие к ситуации. Поэтому рассматривать шаблоны нужно лишь как средство решения задач, а не религию или философию.

#### **Заблуждение 2: MVC  не решает задачу разделения ответственностей**
В примерах и руководствах Apple задачу логического слоя всегда выполняет либо модель данных, либо контроллер вида. Полагаю, сделано это для простоты изложения. Однако именно это упущение в примерах и заставляет думать об MVC как о подходе, где важную бизнес-логику выполняет и слой представления, и слой хранения данных.
Эпл о слое Model пишет так:
>Model objects encapsulate the data specific to an application and define the logic and computation that manipulate and process that data.

Следуя определению эпл, слой логики заключает в себе работу с данными, но сам объект модели не обязан быть объектом данных.
Таким образом, слою Model отводится роль ядра программы, где происходят все бизнес-операции, и работа с данными как составная их часть.

Но здесь встает вопрос отображения состояния модели. Часть действий происходит непосредственно на экране, поэтому их логика неизбежно утекает во ViewController. 

Как решение, предлагается подход MVC с активной моделью, где модель не просто выполняет работу, а через байндинги передает в представление данные, необходимые для отображения

#### **Заблуждение 3: MVC  и MVC с активной моделью это два разных подхода**

На самом деле нет, и схема на сайте ЭПЛ явно указывает на это:

<img src="/assets/0002_mvc_misconceptions/mvc_apple.png" alt="MVC diagram" width="75%"/>

Способ уведомления явно не указан. Это может быть, например, делегирование, или куда более подходящий механизм KVO.

#### **Идея, заложенная Эпл**
Выводы ниже это лишь мое понимание, но длительная практика лишь подтверждает его правильность.

В основе конструирования гибкой, расширяемой, обслуживаемой программной системы лежит принцип разделения логики и представления.
MVC справляется с этой задачей минимально возможным в экосистеме Cocoa набором элементов:
M — программа в понимании ее заказчиком. Напоминания о днях рождения, или покупки в магазине, хранение и создание заметок, все, что является сутью бизнес-задачи, выполняют объекты этого слоя. Для этого они обращаются к периферии, или сервисам — хранилищу данных, сетевому клиенту и прочее.

V — представление, или вид. Вид обновляется по подписке на изменения в соответствующем ему объекте логики. 

С — пожалуй, самый сложный для понимания слой. Я вполне могу представить себе программу, обходящуюся лишь логикой и представлением. UIKit не запрещает поместить на UIWindow произвольный вид напрямую и управлять им из сопряженного класса логики. Однако же иерархия ViewController изначально задумался как шина для более широкого взаимодействия видов, чем просто расположение на экране. Контроль жизненного цикла, отклики на события в приложении, это задачи не связанные с представлением напрямую, и вполне разумно было разделить виды и контроллеры видов. И раз уж такая шина есть, ее можно дополнить, возложив на нее роль связующего звена между логикой и представлением, а именно выполняя в нем конфигурацию байндингов и передачу откликов.


#### **Интересное наблюдение**
Название слоя "View" в связке MVC заставляет думать о нем именно как о UIView, или UIViewController на худой конец. Однако View можно понимать шире, а именно как "представление", или способ передавать пользователю информацию и обрабатывать ввод данных. Например,голосовое меню с синтезатором речи, или экран с азбукой брайля. В таком случае слой контроллера наверняка претерпит серьезные изменения, он будет служить переходником к альтернативной системе представления, а вот самый ценный слой логики может остаться неизменным.

#### **Вывод**
MVC не даром предлагается как оптимальный шаблон, поскольку он решает задачу разделения программной системы на слои минимумом средств. В конкретных случаях, где это требуется, MVC может быть дополнен и расширен, но в основе даже сложных структурных решений все равно будет реализовано разделение логики и представления, а стало быть, неявно присутствовать MVC.