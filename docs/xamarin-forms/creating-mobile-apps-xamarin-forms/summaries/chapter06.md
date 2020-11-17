---
title: Сводка по главе 6. Нажатия кнопки
description: Создание мобильных приложений с помощью Xamarin.Forms. Сводка по главе 6. Нажатия кнопки
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D4F9C429-A6CF-40FA-AC68-3F149307A5F9
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8a95f4a88ea0adff78475dab8699308fbe49aa46
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93370212"
---
# <a name="summary-of-chapter-6-button-clicks"></a>Сводка по главе 6. Нажатия кнопки

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)

> [!NOTE]
> Эта книга была опубликована весной 2016 года и с тех пор не обновлялась. Многое в этой книге остается ценным, но некоторые материалы устарели, а некоторые разделы перестали быть полностью верными или полными.

[`Button`](xref:Xamarin.Forms.Button) — это представление, позволяющее пользователю инициировать команду. `Button` определяется текстом (и, при необходимости, изображением, как показано в [главе 13 "Растровые изображения"](chapter13.md)). Следовательно, `Button` определяет множество тех же свойств, что и `Label`:

- [`Text`](xref:Xamarin.Forms.Button.Text)
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)
- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)

`Button` также определяет три свойства, которые регулируют внешний вид его границы, но поддержка этих свойств и их взаимная независимость является платформенно-зависимой:

- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) типа `Color`
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) типа `Double`
- [`BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius) типа `Double`

`Button` также наследует все свойства `VisualElement` и `View`, включая `BackgroundColor`, `HorizontalOptions` и `VerticalOptions`.

## <a name="processing-the-click"></a>Обработка щелчка

Класс `Button` определяет событие [`Clicked`](xref:Xamarin.Forms.Button.Clicked), которое запускается при нажатии пользователем кнопки `Button`. Обработчик `Click` имеет тип `EventHandler`. Первым аргументом является объект `Button`, создающий событие. Вторым аргументом является объект `EventArgs`, который не предоставляет дополнительных сведений.

В примере [**ButtonLogger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger) демонстрируется простая обработка `Clicked`.

## <a name="sharing-button-clicks"></a>Нажатия кнопки общего доступа

Несколько представлений `Button` могут совместно использовать один и тот же обработчик `Clicked`, но обычно обработчику необходимо определить, какой `Button` отвечает за определенное событие. Один из подходов состоит в том, чтобы хранить различные объекты `Button` в виде полей и проверять, какие из них обрабатывали событие в обработчике.

Этот метод демонстрируется в примере [**TwoButtons**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons). Программа также демонстрирует, как задать свойство [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) для `Button` в `false`, когда нажатие клавиши `Button` перестает быть действенным. Отключенный `Button` не создает событие `Clicked`.

## <a name="anonymous-event-handlers"></a>Анонимные обработчики событий

Можно определить обработчики `Clicked` как анонимные лямбда-функции, как показывает пример [**ButtonLambdas**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas). Однако анонимные обработчики не могут совместно использоваться без какого-либо кода с более запутанным отражением.

## <a name="distinguishing-views-with-ids"></a>Различение представлений с помощью идентификаторов

Несколько объектов `Button` также можно различить путем установки свойства [`StyleId`](xref:Xamarin.Forms.Element.StyleId) или [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) в `string`. Это свойство определяется `Element`, но не используется в Xamarin.Forms. Оно предназначено исключительно для использования программами приложений.

В примере [**SimplestKeypad**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad) используется один и тот же обработчик событий для всех 10 числовых клавиш на цифровой клавиатуре, который различается с помощью свойства `StyleId`:

[![Тройной снимок экрана простейшей клавиатуры](images/ch06fg04-small.png "Calculator")](images/ch06fg04-large.png#lightbox "Calculator")

## <a name="saving-transient-data"></a>Сохранение временных данных

Многие приложения должны сохранять данные при завершении программы и перезагружать эти данные при повторном запуске программы. Класс [`Application`](xref:Xamarin.Forms.Application) определяет несколько элементов, которые помогают программе сохранять и восстанавливать временные данные.

- Свойство [`Properties`](xref:Xamarin.Forms.Application.Properties) — это словарь с ключами `string` и элементами `object`. Содержимое словаря автоматически сохраняется в локальном хранилище приложения перед завершением программы и перезагружается при запуске программы.
- Класс `Application` определяет три защищенных виртуальных метода, переопределяемых стандартным классом `App` программы: [`OnStart`](xref:Xamarin.Forms.Application.OnStart), [`OnSleep`](xref:Xamarin.Forms.Application.OnSleep) и [`OnResume`](xref:Xamarin.Forms.Application.OnResume). Они относятся к событиям *жизненного цикла приложения*.
- Метод [`SavePropertiesAsync`](xref:Xamarin.Forms.Application.SavePropertiesAsync) сохраняет содержимое словаря.

Не нужно вызывать `SavePropertiesAsync`. Содержимое словаря автоматически сохраняется перед завершением программы и извлекается при ее запуске. Это полезно во время тестирования программы для сохранения данных в случае сбоя.

Также полезно:

- [`Application.Current`](xref:Xamarin.Forms.Application.Current) — статическое свойство, возвращающее текущий объект `Application`, который затем можно использовать для получения словаря `Properties`.

Первым шагом является указание всех переменных на странице, которые необходимо сохранить при завершении программы. Если вы знаете все места, в которых изменяются эти переменные, можете просто добавить их в словарь `Properties`. В конструкторе страницы можно задать переменные из словаря `Properties`, если ключ существует.

Более крупной программе, вероятно, придется иметь дело с событиями жизненного цикла приложения. Самым важным является метод `OnSleep`. Вызов этого метода означает, что программа ушла с переднего плана. Возможно, пользователь нажимает кнопку **Главная** на устройстве, отображает все приложения или завершает работу телефона. Вызов `OnSleep` является единственным уведомлением, которое программа получает перед завершением работы. Программа должна принять эту возможность, чтобы гарантировать актуальность словаря `Properties`.

Вызов `OnResume` указывает на то, что программа не завершилась после последнего вызова `OnSleep`, а теперь снова выполняется на переднем плане. Эта возможность может использоваться программой для обновления подключений к Интернету (например).

Вызов `OnStart` происходит во время запуска программы. Поскольку содержимое уже было восстановлено при вызове конструктора `App`, не нужно ждать, пока этот вызов метода попытается получить доступ к словарю `Properties`.

Пример [**PersistentKeypad**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad) очень похож на **SimplestKeypad**, за исключением того, что программа использует переопределение `OnSleep` для сохранения текущей записи клавиатуры, а конструктор страниц — для восстановления этих данных.

> [!NOTE]
> Другой подход к сохранению параметров программы обеспечивается с помощью класса Xamarin.Essentials [предпочтений](~/essentials/preferences.md).

## <a name="related-links"></a>Связанные ссылки

- [Полный текст главы 6 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [Примеры для главы 6](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [Примеры F# для главы 6](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
- [Кнопка Xamarin.Forms](~/xamarin-forms/user-interface/button.md)
