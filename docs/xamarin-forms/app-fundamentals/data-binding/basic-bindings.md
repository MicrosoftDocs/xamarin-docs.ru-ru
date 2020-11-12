---
title: Основные привязки Xamarin.Forms
description: В этой статье описывается использование привязки данных Xamarin.Forms, которая связывает пару свойств между двумя объектами, по крайней мере один из которых обычно является объектом пользовательского интерфейса. Эти два объекта называются целевым объектом и источником.
ms.prod: xamarin
ms.assetid: 96553DF7-12EA-4FB2-AE85-3D1D59382B40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/22/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.custom: video
ms.openlocfilehash: 107916edee01171b8ff5d4871de3b1243c385dd5
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93366429"
---
# <a name="no-locxamarinforms-basic-bindings"></a>Основные привязки Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/databindingdemos)

Привязка данных Xamarin.Forms связывает пару свойств между двумя объектами, по крайней мере один из которых обычно является объектом пользовательского интерфейса. Эти два объекта называются *целевым объектом* и *источником* :

- *Целевой объект*  — это объект (и свойство), к которому устанавливается привязка данных.
- *Источник*  — это объект (и свойство), на который ссылается привязка данных.

Это различие иногда может сбивать с толку. В самом простом случае данные поступают от источника к целевому объекту, то есть целевое свойство устанавливается на основе значения свойства источника. Однако в некоторых случаях данные могут поступать от целевого объекта к источнику или перемещаться в обоих направлениях. Чтобы избежать путаницы, помните, что привязка данных всегда устанавливается к целевому объекту, даже если он предоставляет, а не получает данные.

## <a name="bindings-with-a-binding-context"></a>Привязки с контекстом привязки

Хотя привязки данных обычно указываются полностью в XAML, полезно посмотреть привязки данных в коде. Страница **Базовая привязка кода** содержит файл XAML с `Label` и `Slider`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BasicCodeBindingPage"
             Title="Basic Code Binding">
    <StackLayout Padding="10, 0">
        <Label x:Name="label"
               Text="TEXT"
               FontSize="48"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Slider` имеет значение в диапазоне от 0 до 360. Цель этой программы состоит в повороте `Label` с помощью объекта `Slider`.

Без привязки данных необходимо настроить событие `ValueChanged` объекта `Slider` в обработчике событий, который обращается к свойству `Value` объекта `Slider` и задает это значение свойству `Rotation` объекта `Label`. Привязка данных автоматизирует эту задачу — обработчик событий и код в ней больше не нужны.

Вы можете задать привязку к экземпляру любого класса, производного от [`BindableObject`](xref:Xamarin.Forms.BindableObject), который включает производные `Element`, `VisualElement`, `View` и `View`.  Привязка всегда настраивается в целевом объекте. Привязка ссылается на исходный объект. Чтобы настроить привязку данных, используйте следующие два члена класса цели.

- Свойство [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) задает исходный объект.
- Метод [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) указывает целевое свойство и исходное свойство.

В этом примере `Label` является целевым объектом привязки, а `Slider` — источником привязки. Изменения в источнике `Slider` влияют на угол поворота целевого объекта `Label`. Данные поступают от источника к целевому объекту.

Метод `SetBinding`, определенный объектом `BindableObject`, имеет аргумент типа [`BindingBase`](xref:Xamarin.Forms.BindingBase), из которого производится класс [`Binding`](xref:Xamarin.Forms.Binding), но есть другие методы `SetBinding`, определенные классом [`BindableObjectExtensions`](xref:Xamarin.Forms.BindableObjectExtensions). Файл с выделенным кодом в примере **базовой привязки кода** использует более простой метод расширения [`SetBinding`](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*) из этого класса.

```csharp
public partial class BasicCodeBindingPage : ContentPage
{
    public BasicCodeBindingPage()
    {
        InitializeComponent();

        label.BindingContext = slider;
        label.SetBinding(Label.RotationProperty, "Value");
    }
}
```

Объект `Label` является целевым объектом привязки, поэтому на этом объекте устанавливается это свойство и в нем вызывается метод. Свойство `BindingContext` указывает источник привязки, то есть `Slider`.

Метод `SetBinding` вызывается в целевом объекте привязки, но указывает свойство целевого объекта и свойство источника. Свойство целевого объекта указывается как объект `BindableProperty`: `Label.RotationProperty`. Свойство источника указывается как строка и определяет свойство `Value` объекта `Slider`.

Метод `SetBinding` раскрывает одно из самых важных правил привязки данных:

*целевое свойство должно поддерживаться свойством, подходящим для привязки.*

Это правило означает, что целевой объект должен быть экземпляром класса, производного от `BindableObject`. Сведения о привязываемых объектах и свойствах см. в разделе [**Привязываемые свойства**](~/xamarin-forms/xaml/bindable-properties.md).

Для свойства источника такого правила нет. Оно указывается как строка. На внутреннем уровне используется отражение для доступа к фактическому свойству. В данном случае, однако, свойство `Value` также поддерживается привязываемым свойством.

Код можно упростить. Привязываемое свойство `RotationProperty` определяется элементом `VisualElement` и наследуется `Label` и `ContentPage`, поэтому имя класса не требуется в вызове метода `SetBinding`:

```csharp
label.SetBinding(RotationProperty, "Value");
```

Но лучше включить имя класса, чтобы помнить о целевом объекте.

При изменении свойства объекта `Slider` объект `Label` поворачивается соответствующим образом:

[![Базовая привязка кода](basic-bindings-images/basiccodebinding-small.png "Базовая привязка кода")](basic-bindings-images/basiccodebinding-large.png#lightbox "Базовая привязка кода")

Страница **Базовая привязка Xaml** идентична странице **Базовая привязка кода** , но она определяет всю привязку данных в XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BasicXamlBindingPage"
             Title="Basic XAML Binding">
    <StackLayout Padding="10, 0">
        <Label Text="TEXT"
               FontSize="80"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               BindingContext="{x:Reference Name=slider}"
               Rotation="{Binding Path=Value}" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Как и в коде, привязка данных задается в целевом объекте, то есть `Label`. Используются два расширения разметки XAML. Их легко узнать по разделителям в виде фигурной скобки:

- Расширение разметки `x:Reference` требуется для ссылки на исходный объект, то есть `Slider` с именем `slider`.
- Расширения разметки `Binding` связывает свойство `Rotation` объекта `Label` со свойством `Value` объекта `Slider`.

Дополнительные сведения о расширениях разметки XAML см. в статье [Расширения разметки XAML](~/xamarin-forms/xaml/markup-extensions/index.md). Расширение разметки `x:Reference` поддерживается классом [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension); `Binding` поддерживается классом [`BindingExtension`](xref:Xamarin.Forms.Xaml.BindingExtension). Как указывают префиксы пространства имен XML, `x:Reference` является частью спецификации XAML 2009, хотя `Binding` является частью Xamarin.Forms. Обратите внимание, что в фигурных скобках нет кавычек.

Легко забыть расширения разметки `x:Reference` при задании `BindingContext`. Нередко разработчики ошибочно задают свойство непосредственно для имени источника привязки следующим образом.

```xaml
BindingContext="slider"
```

Это неправильно. Эта разметка задает свойство `BindingContext` для объекта `string`, в котором символы складываются в слово slider.

Обратите внимание, что свойство источника указано в свойстве [`Path`](xref:Xamarin.Forms.Xaml.BindingExtension.Path) класса `BindingExtension`, которое соответствует свойству [`Path`](xref:Xamarin.Forms.Binding.Path) класса [`Binding`](xref:Xamarin.Forms.Binding).

Разметку, показанную на странице **Базовая привязка XAML** , можно упростить. Расширения разметки XAML, такие как `x:Reference` и `Binding`, могут иметь определенные атрибуты *свойства содержимого*. Для расширения разметки XAML это означает, что имя свойства не нужно отображать. Свойство `Name` является свойством содержимого `x:Reference`, а свойство `Path` является свойством содержимого `Binding`, то есть их можно удалить из выражения:

```xaml
<Label Text="TEXT"
       FontSize="80"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       BindingContext="{x:Reference slider}"
       Rotation="{Binding Value}" />
```

## <a name="bindings-without-a-binding-context"></a>Привязки без контекста привязки

Свойство `BindingContext` является важным компонентом привязок данных, но оно не всегда необходимо. Вместо этого можно указать исходный объект в вызове `SetBinding` или расширении разметки `Binding`.

Это показано в примере **Альтернативной привязки кода**. Файл XAML аналогичен примеру **Базовая привязка кода** , только объект `Slider` определен для управления свойством `Scale` объекта `Label`. Поэтому `Slider` имеет значение в диапазоне от &ndash;2 до 2:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.AlternativeCodeBindingPage"
             Title="Alternative Code Binding">
    <StackLayout Padding="10, 0">
        <Label x:Name="label"
               Text="TEXT"
               FontSize="40"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Minimum="-2"
                Maximum="2"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Файл с выделенным кодом задает привязку с помощью метода [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)), определенного объектом `BindableObject`. Этот аргумент является [конструктором](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object)) для класса [`Binding`](xref:Xamarin.Forms.Binding):

```csharp
public partial class AlternativeCodeBindingPage : ContentPage
{
    public AlternativeCodeBindingPage()
    {
        InitializeComponent();

        label.SetBinding(Label.ScaleProperty, new Binding("Value", source: slider));
    }
}
```

Конструктор `Binding` имеет 6 параметров, поэтому параметр `source` указан с именованным аргументом. Аргумент является объектом `slider`.

Выполнение программы может быть немного неожиданным:

[![Альтернативная привязка кода](basic-bindings-images/alternativecodebinding-small.png "Альтернативная привязка кода")](basic-bindings-images/alternativecodebinding-large.png#lightbox "Альтернативная привязка кода")

На экране iOS слева показано, как выглядит экран, когда страница открывается в первый раз. Где объект `Label`?

Проблема в том, что `Slider` имеет начальное значение 0. В результате свойство `Scale` объекта `Label` также имеет значение 0 и его значение по умолчанию 1 переопределяется. Поэтому объект `Label` поначалу не видно. Как показано на снимках экрана Android, вы можете управлять объектом `Slider`, чтобы объект `Label` снова появился, но его первоначальное исчезновение сбивает с толку.

Из [следующей статьи](binding-mode.md) вы узнаете, как избежать этой проблемы с помощью инициализации объекта `Slider` из значения по умолчанию свойства `Scale`.

> [!NOTE]
> Класс [`VisualElement`](xref:Xamarin.Forms.VisualElement) также определяет свойства [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) и [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY), которые могут масштабировать `VisualElement` по-разному в горизонтальном и вертикальном направлениях.

Страница **Альтернативная привязка XAML** показывает эквивалентную привязку полностью в XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.AlternativeXamlBindingPage"
             Title="Alternative XAML Binding">
    <StackLayout Padding="10, 0">
        <Label Text="TEXT"
               FontSize="40"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               Scale="{Binding Source={x:Reference slider},
                               Path=Value}" />

        <Slider x:Name="slider"
                Minimum="-2"
                Maximum="2"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Теперь расширение разметки `Binding` имеет два свойства — `Source` и `Path`, разделенные запятой. Если вы предпочитаете, они могут отображаться на одной строке:

```xaml
Scale="{Binding Source={x:Reference slider}, Path=Value}" />
```

Свойство `Source` задано встроенному расширению разметки `x:Reference`, которое в противном случае имеет тот же синтаксис, что и параметр `BindingContext`. Обратите внимание, что в фигурных скобках нет кавычек и эти два свойства должны быть разделены запятой.

Свойство содержимого расширения разметки `Binding` — `Path`, но часть `Path=` расширения разметки можно исключить только в том случае, если это первое свойство в выражении. Чтобы исключить часть `Path=`, необходимо поменять два свойства местами:

```xaml
Scale="{Binding Value, Source={x:Reference slider}}" />
```

Хотя расширения разметки XAML обычно разделены с помощью фигурных скобок, они также могут быть выражены как элементы объекта:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand">
    <Label.Scale>
        <Binding Source="{x:Reference slider}"
                 Path="Value" />
    </Label.Scale>
</Label>
```

Теперь свойства `Source` и `Path` являются обычными атрибутами XAML: значения заключены в кавычки и атрибуты не разделяются запятыми. Расширение разметки `x:Reference` также может стать элементом объекта:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand">
    <Label.Scale>
        <Binding Path="Value">
            <Binding.Source>
                <x:Reference Name="slider" />
            </Binding.Source>
        </Binding>
    </Label.Scale>
</Label>
```

Это нестандартный синтаксис, но иногда он необходим, если используются составные объекты.

Пока в примерах мы задавали свойство `BindingContext` и свойство `Source` класса `Binding` для расширения разметки `x:Reference`, чтобы ссылаться на другое представление на странице. Эти два свойства имеют тип `Object`, и их можно задать для любого объекта, который содержит свойства, подходящие для источников привязки.

В следующих статьях вы увидите, что можно задать свойство `BindingContext` или `Source` для расширения разметки `x:Static`, чтобы ссылаться на значение статического свойства или поля, или расширения разметки `StaticResource`, чтобы ссылаться на объект, хранящийся в словаре ресурсов, или непосредственно для объекта, который обычно (но не всегда) является экземпляром модели представления.

Свойство `BindingContext` также может быть задано для объекта `Binding`, чтобы свойства `Source` и `Path` объекта `Binding` определяли контекст привязки.

## <a name="binding-context-inheritance"></a>Наследование контекста привязки

В этой статье вы увидели, что можно указать исходный объект с помощью свойства `BindingContext` или `Source` объекта `Binding`. Если заданы оба свойства, свойство `Source` объекта `Binding` имеет приоритет над `BindingContext`.

Свойство `BindingContext` имеет очень важную характеристику:

*настройка свойства `BindingContext` наследуется через визуальное дерево.*

Как вы увидите, это может быть очень удобно для упрощения выражений привязки, а в некоторых случаях &mdash;, особенно в сценариях "модель — представление — модель представления" (MVVM) &mdash;, даже необходимо.

Пример **Наследование контекста привязки**  — это простая демонстрация наследования контекста привязки:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BindingContextInheritancePage"
             Title="BindingContext Inheritance">
    <StackLayout Padding="10">

        <StackLayout VerticalOptions="FillAndExpand"
                     BindingContext="{x:Reference slider}">

            <Label Text="TEXT"
                   FontSize="80"
                   HorizontalOptions="Center"
                   VerticalOptions="EndAndExpand"
                   Rotation="{Binding Value}" />

            <BoxView Color="#800000FF"
                     WidthRequest="180"
                     HeightRequest="40"
                     HorizontalOptions="Center"
                     VerticalOptions="StartAndExpand"
                     Rotation="{Binding Value}" />
        </StackLayout>

        <Slider x:Name="slider"
                Maximum="360" />

    </StackLayout>
</ContentPage>
```

Свойство `BindingContext` объекта `StackLayout` устанавливается для объекта `slider`. Этот контекст привязки наследуется объектами `Label` и `BoxView`, оба из которых имеют свойства `Rotation`, зависящие от свойства `Value` объекта `Slider`:

[![Наследование контекста привязки](basic-bindings-images/bindingcontextinheritance-small.png "Наследование контекста привязки")](basic-bindings-images/bindingcontextinheritance-large.png#lightbox "Наследование контекста привязки")

В [следующей статье](binding-mode.md) вы увидите, как *режим привязки* может изменить поток данных между целевым и исходным объектами.

## <a name="related-links"></a>Связанные ссылки

- [Демоверсии привязок данных (пример)](/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Глава, посвященная привязкам данных, из книги о Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Data-Binding/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]