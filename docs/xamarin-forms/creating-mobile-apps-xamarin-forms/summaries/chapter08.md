---
title: Сводная информация о Главе 8. Совместное использование кода и XAML
description: Создание мобильных приложений с помощью Xamarin.Forms. Сводная информация о Главе 8. Совместное использование кода и XAML
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5970DEEB-1FC9-4F78-B4F6-D403E16D22ED
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a88f0ef4e04bf6884f1b49811663f611d3e54a0a
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93366221"
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>Сводная информация о Главе 8. Совместное использование кода и XAML

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)

> [!NOTE]
> Эта книга была опубликована весной 2016 года и с тех пор не обновлялась. Многое в этой книге остается ценным, но некоторые материалы устарели, а некоторые разделы перестали быть полностью верными или полными.

В этой главе более подробно рассматривается XAML и, в частности, взаимодействие кода и XAML.

## <a name="passing-arguments"></a>Передача аргументов

В общем случае класс, созданный в XAML, должен иметь открытый конструктор без параметров. Результирующий объект инициализируется с помощью параметров свойств. Однако существует два других способа создания и инициализации объектов.

Хотя это методы общего назначения, они используются преимущественно в связи с моделями MVVM View.

### <a name="constructors-with-arguments"></a>Конструкторы с аргументами

В примере [**ParameteredConstructorDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo) показано, как использовать тег `x:Arguments` для указания аргументов конструктора. Эти аргументы должны быть разделены тегами элементов, указывающими тип аргумента. Для базовых типов данных .NET доступны такие теги:

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

### <a name="can-i-call-methods-from-xaml"></a>Можно ли вызывать методы из XAML?

В примере [**FactoryMethodDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo) показано, как использовать элемент `x:FactoryMethod`, чтобы указать фабричный метод, который вызывается для создания объекта. Такой фабричный метод должен быть открытым и статическим и должен создавать объект типа, в котором он определен. (Например, таким является метод [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)), так как он открытый, статический и возвращает значение типа `Color`.) Аргументы для фабричного метода указываются в тегах `x:Arguments`.

## <a name="the-xname-attribute"></a>Атрибут x:Name

Атрибут `x:Name` позволяет присвоить объекту, созданному в XAML, имя. Правила для этих имен такие же, как и для имен переменных в C#. После возврата вызова `InitializeComponent` в конструкторе файл кода программной части может ссылаться на эти имена для доступа к соответствующему элементу XAML. Фактически имена преобразуются синтаксическим анализатором XAML в закрытые поля в созданном разделяемом классе.

В примере [**XamlClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock) демонстрируется использование `x:Name`, чтобы файл кода программной части оставался в состоянии обновлять два элемента `Label`, определенных в XAML, текущими датой и временем.

Одно и то же имя нельзя использовать для нескольких элементов на одной странице. Эта проблема особенно важна, если для создания параллельно именованных объектов для каждой платформы используется `OnPlatform`. В примере [**PlatformSpecificLabele**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels) демонстрируется более удобный способ сделать что-то вроде этого.

## <a name="custom-xaml-based-views"></a>Пользовательские представления на основе XAML

Существует несколько способов избежать повторения разметки в XAML. Одним из распространенных способов является создание нового класса на основе XAML, производного от [`ContentView`](xref:Xamarin.Forms.ContentView). Этот метод показан в примере [**ColorViewList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList). Класс `ColorView` является производным от `ContentView` и служит для вывода определенного цвета и его имени, в то время как класс `ColorViewListPage` является производным от `ContentPage` и как обычно и явно создает 17 экземпляров `ColorView`.

Для доступа к классу `ColorView` в XAML требуется другое объявление пространства имен XML, обычно именуемое `local` для классов в той же сборке.

## <a name="events-and-handlers"></a>События и обработчики

События можно назначить обработчикам событий в XAML, но сам обработчик событий должен быть реализован в файле кода программной части. В примере [**XamlKeypad**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad) демонстрируется создание пользовательского интерфейса с клавиатурой на языке XAML и реализация обработчиков `Clicked` в файле кода программной части.

## <a name="tap-gestures"></a>Жесты при касаниях

Любой объект `View` может получать данные от сенсорного устройства и создавать события из этих входных данных. Класс `View` определяет свойство коллекции [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers), которое может содержать один или несколько экземпляров классов, производных от [`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer).

[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) создает события [`Tapped`](xref:Xamarin.Forms.TapGestureRecognizer.Tapped). Программа [**MonkeyTap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap) демонстрирует присоединение `TapGestureRecognizer` объектов к четырем элементам `BoxView` для создания имитационной игры:

[![Тройной снимок экрана в MonkeyTap](images/ch08fg07-small.png "Имитационная игра")](images/ch08fg07-large.png#lightbox "Имитационная игра")

Хотя программе **MonkeyTap** все же нужен звук. (См. [следующую главу](chapter09.md).)

## <a name="related-links"></a>Связанные ссылки

- [Полный текст главы 8 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [Примеры для Главы 8](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [Примеры на F# для Главы 8](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
- [Передача аргументов в XAML](~/xamarin-forms/xaml/passing-arguments.md)
