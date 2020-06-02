---
title: Скомпилированные привязки Xamarin.Forms
description: В этой статье описывается использование скомпилированных привязок для оптимизации производительности привязки данных в приложениях Xamarin.Forms.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1f811de95009900016bb8b442265a9a079e0f612
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139760"
---
# <a name="xamarinforms-compiled-bindings"></a>Скомпилированные привязки Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

_Скомпилированные привязки разрешаются быстрее классических, что позволяет повысить производительность привязки данных в приложениях Xamarin.Forms._

С привязками данных связаны две основные проблемы:

1. Не выполняется проверка выражений привязки во время компиляции. Вместо этого привязки разрешаются во время выполнения. Таким образом, недопустимые привязки проявляются только во время выполнения в виде нарушений в поведении приложения или сообщений об ошибке.
1. Затрачиваются избыточные ресурсы. Привязки разрешаются во время выполнения с использованием проверки объектов общего назначения (отражение), причем объем затрачиваемых на это ресурсов зависит от платформы.

Использование скомпилированных привязок позволяет оптимизировать производительность привязки данных в приложениях Xamarin.Forms за счет разрешения выражений привязки во время компиляции, а не во время выполнения. Кроме того, такая проверка выражений привязки во время компиляции позволяет повысить эффективность устранения неполадок, поскольку недопустимые привязки выявляются как ошибки построения.

Процесс использования скомпилированных привязок выглядит следующим образом:

1. Включите компиляцию XAML. Дополнительные сведения о компиляции XAML см. в статье [Компиляция XAML](~/xamarin-forms/xaml/xamlc.md).
1. Присвойте атрибуту `x:DataType` объекта [`VisualElement`](xref:Xamarin.Forms.VisualElement) тип объекта, к которому будут привязываться объект `VisualElement` и его дочерние объекты.

> [!NOTE]
> Атрибут `x:DataType` рекомендуется задавать на том же уровне иерархии представления, на котором задается [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext). Однако этот атрибут может быть переопределен в любом месте иерархии представлений.

Чтобы использовать скомпилированные привязки, атрибуту `x:DataType` необходимо назначить строковый литерал или тип, использующий расширение разметки `x:Type`. Во время компиляции XAML любые недопустимые выражения привязки будут выявляться как ошибки построения. Тем не менее компилятор XAML будет возвращать ошибку построения только для первого обнаруженного недопустимого выражения привязки. Любые допустимые выражения привязки, которые определены для объекта `VisualElement` или его дочерних объектов, будут скомпилированы независимо от того, задается ли [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) в XAML или в коде. При компиляции выражения привязки создается скомпилированный код, который получает значение из свойства *источника* и присваивает его свойству *адресата*, который указан в разметке. Кроме того, в зависимости от выражения привязки созданный код может наблюдать изменения в значении свойства *источника* и обновляет свойство *адресата*, а также может отправлять изменения из *адресата* обратно в *источник*.

> [!IMPORTANT]
> Скомпилированные привязки на данный момент отключены для любых выражений привязки, которые определяют свойство [`Source`](xref:Xamarin.Forms.Binding.Source). Это связано с тем, что свойство `Source` всегда задается с использованием расширения разметки `x:Reference`, которое не может быть разрешено во время компиляции.

## <a name="use-compiled-bindings"></a>Использование скомпилированных привязок

На странице **скомпилированного средства выбора цвета** демонстрируется использование скомпилированных привязок между представлениями Xamarin.Forms и свойствами модели представления:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.CompiledColorSelectorPage"
             Title="Compiled Color Selector">
    ...
    <StackLayout x:DataType="local:HslColorViewModel">
        <StackLayout.BindingContext>
            <local:HslColorViewModel Color="Sienna" />
        </StackLayout.BindingContext>
        <BoxView Color="{Binding Color}"
                 ... />
        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />
            <Slider Value="{Binding Hue}" />
            <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />
            <Slider Value="{Binding Saturation}" />
            <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />
            <Slider Value="{Binding Luminosity}" />
            <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
        </StackLayout>
    </StackLayout>    
</ContentPage>
```

Корневое представление [`StackLayout`](xref:Xamarin.Forms.StackLayout) создает экземпляр `HslColorViewModel` и инициализирует свойство `Color` в тегах элемента свойства для свойство [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext). Это корневое представление `StackLayout` также определяет атрибут `x:DataType` как тип модели представления, указывая, что любое выражение привязки в иерархии корневого представления `StackLayout` будет компилироваться. Это можно проверить, изменив любое выражение привязки так, чтобы оно было привязано к несуществующему свойству модели представления, что приведет к ошибке сборки. Хотя в этом примере атрибуту `x:DataType` присваивается строковый литерал, для него также можно задать тип с расширением разметки `x:Type`. Дополнительные сведения о расширении разметки `x:Type` см. в разделе [Расширение разметки x:Type](~/xamarin-forms/xaml/markup-extensions/consuming.md#type).

> [!IMPORTANT]
> Обратите внимание, что атрибут `x:DataType` может быть переопределен в любом месте иерархии представления.

Элементы [`BoxView`](xref:Xamarin.Forms.BoxView), [`Label`](xref:Xamarin.Forms.Label) и представления [`Slider`](xref:Xamarin.Forms.Slider) наследуют контекст привязки из представления [`StackLayout`](xref:Xamarin.Forms.StackLayout). Эти представления являются целевыми объектами привязки, которые ссылаются на свойства источника в модели представления. Для свойств [`BoxView.Color`](xref:Xamarin.Forms.BoxView.Color) и [`Label.Text`](xref:Xamarin.Forms.Label.Text) привязки данных имеют режим `OneWay`: свойства в представлении задаются по свойствам в модели представления. Тем не менее свойство [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) использует привязку `TwoWay`. Поэтому каждый объект `Slider` задается из модели представления, а модель представления задается из каждого объекта `Slider`.

При первом запуске приложения элементы [`BoxView`](xref:Xamarin.Forms.BoxView), [`Label`](xref:Xamarin.Forms.Label) и [`Slider`](xref:Xamarin.Forms.Slider) получают значения из модели представления в зависимости от первоначального свойства `Color`, заданного при создании экземпляра модели представления. Эти действия показаны на следующих снимках экрана.

[![Скомпилированное средство выбора цвета](compiled-bindings-images/compiledcolorselector-small.png "Скомпилированное средство выбора цвета")](compiled-bindings-images/compiledcolorselector-large.png#lightbox "Скомпилированное средство выбора цвета")

При выполнении действий с ползунками элементы [`BoxView`](xref:Xamarin.Forms.BoxView) и [`Label`](xref:Xamarin.Forms.Label) соответствующим образом обновляются.

Дополнительные сведения о средстве выбора цвета см. в статье [Уведомления об изменении свойства и модели представления](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#viewmodels-and-property-change-notifications).

## <a name="use-compiled-bindings-in-a-datatemplate"></a>Использование скомпилированных привязок в DataTemplate

Привязки в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) интерпретируются в контексте объекта, к которому применяется шаблон. Таким образом, при использовании скомпилированных привязок в `DataTemplate` объект `DataTemplate` должен объявлять тип своего объекта данных с использованием атрибута `x:DataType`.

На странице **скомпилированного списка цветов** демонстрируется использование скомпилированных привязок в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.CompiledColorListPage"
             Title="Compiled Color List">
    <Grid>
        ...
        <ListView x:Name="colorListView"
                  ItemsSource="{x:Static local:NamedColor.All}"
                  ... >
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:NamedColor">
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <BoxView Color="{Binding Color}"
                                     ... />
                            <Label Text="{Binding FriendlyName}"
                                   ... />
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
        <!-- The BoxView doesn't use compiled bindings -->
        <BoxView Color="{Binding Source={x:Reference colorListView}, Path=SelectedItem.Color}"
                 ... />
    </Grid>
</ContentPage>
```

Свойство [`ListView.ItemsSource`](xref:Xamarin.Forms.ListView) получает статическое свойство `NamedColor.All`. Класс `NamedColor` использует отражение .NET для перечисления всех открытых статических полей в структуре [`Color`](xref:Xamarin.Forms.Color) и их хранения с именами в коллекции, доступной из статического свойства `All`. Таким образом, объект `ListView` заполняется экземплярами `NamedColor`. Для каждого элемента в `ListView` в качестве контекста привязки для элемента задается объект `NamedColor`. Элементы [`BoxView`](xref:Xamarin.Forms.BoxView) и [`Label`](xref:Xamarin.Forms.Label) в объекте [`ViewCell`](xref:Xamarin.Forms.ViewCell) привязываются к свойствам `NamedColor`.

Обратите внимание, что [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) определяет атрибут `x:DataType` как тип `NamedColor`, указывая, что любое выражение привязки в иерархии представления `DataTemplate` будет компилироваться. Это можно проверить, изменив любое выражение привязки так, чтобы оно было привязано к несуществующему свойству `NamedColor`, что приведет к ошибке построения.  Хотя в этом примере атрибуту `x:DataType` присваивается строковый литерал, для него также можно задать тип с расширением разметки `x:Type`. Дополнительные сведения о расширении разметки `x:Type` см. в разделе [Расширение разметки x:Type](~/xamarin-forms/xaml/markup-extensions/consuming.md#type).

При первом запуске приложения объект [`ListView`](xref:Xamarin.Forms.ListView) заполняется экземплярами `NamedColor`. При выборе элемента в объекте `ListView` свойству [`BoxView.Color`](xref:Xamarin.Forms.BoxView.Color) присваивается цвет элемента, выбранного в `ListView`:

[![Скомпилированный список цветов](compiled-bindings-images/compiledcolorlist-small.png "Скомпилированный список цветов")](compiled-bindings-images/compiledcolorlist-large.png#lightbox "Скомпилированный список цветов")

При выборе других элементов в [`ListView`](xref:Xamarin.Forms.BoxView) обновляется цвет элемента [`BoxView`](xref:Xamarin.Forms.BoxView).

## <a name="combine-compiled-bindings-with-classic-bindings"></a>Объединение скомпилированных привязок с классическими привязками

Выражения привязки компилируются только для иерархии представления, в которой определен атрибут `x:DataType`. И наоборот, любые представления в иерархии, в которой не определен атрибут `x:DataType`, будут использовать классические привязки. Таким образом, на одной странице могут объединяться скомпилированные и классические привязки. Например, в предыдущем разделе представления в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) используют скомпилированные привязки, а элемент [`BoxView`](xref:Xamarin.Forms.BoxView), которому присваивается выбранный в [`ListView`](xref:Xamarin.Forms.ListView) цвет, не использует их.

Таким образом, при тщательной проработке структуры атрибутов `x:DataType` на одной странице могут одновременно использоваться скомпилированные и классические привязки. Кроме того, атрибут `x:DataType` может в любом месте иерархии быть переопределен как `null` с использованием расширения разметки `x:Null`. Это указывает, что любые выражения привязки в иерархии этого представления будут использовать классические привязки. Этот подход демонстрируется на странице *смешанных привязок*:

```xaml
<StackLayout x:DataType="local:HslColorViewModel">
    <StackLayout.BindingContext>
        <local:HslColorViewModel Color="Sienna" />
    </StackLayout.BindingContext>
    <BoxView Color="{Binding Color}"
             VerticalOptions="FillAndExpand" />
    <StackLayout x:DataType="{x:Null}"
                 Margin="10, 0">
        <Label Text="{Binding Name}" />
        <Slider Value="{Binding Hue}" />
        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />
        <Slider Value="{Binding Saturation}" />
        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />
        <Slider Value="{Binding Luminosity}" />
        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
    </StackLayout>
</StackLayout>   
```

Корневое представление [`StackLayout`](xref:Xamarin.Forms.StackLayout) задает атрибут `x:DataType` как тип `HslColorViewModel`, указывая, что любое выражение привязки в иерархии корневого представления `StackLayout` будет компилироваться. Тем не менее внутреннее представление `StackLayout` переопределяет атрибут `x:DataType` как `null` с использованием расширения разметки `x:Null`. Таким образом, в выражениях привязки внутреннего представления `StackLayout` используются классические привязки. Скомпилированные привязки используются только элементом [`BoxView`](xref:Xamarin.Forms.BoxView) в иерархии корневого представления `StackLayout`.

Дополнительные сведения о расширении разметки `x:Null` см. в статье [Расширение разметки x:Null](~/xamarin-forms/xaml/markup-extensions/consuming.md#null).

## <a name="performance"></a>Производительность

Использование скомпилированных привязок позволяет повысить производительность привязки данных с учетом различных факторов. Результаты модульного тестирования демонстрируют следующее:

- Скомпилированные привязки, использующие уведомление об изменении свойства (то есть привязки в режиме `OneWay`, `OneWayToSource` или `TwoWay`), разрешаются примерно в 8 раз быстрее относительно классических привязок.
- Скомпилированные привязки, не использующие уведомление об изменении свойства (то есть привязки в режиме `OneTime`), разрешаются примерно в 20 раз быстрее относительно классических привязок.
- Задание [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) для скомпилированных привязок, использующих уведомление об изменении свойства (то есть для привязок в режиме `OneWay`, `OneWayToSource` или `TwoWay`), выполняется примерно в 5 раз быстрее, чем задание `BindingContext` для классических привязок.
- Задание [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) для скомпилированных привязок, не использующих уведомление об изменении свойства (то есть для привязок в режиме `OneTime`), выполняется примерно в 7 раз быстрее, чем задание `BindingContext` для классических привязок.

Выигрыш в производительности может быть еще более весомым на мобильных устройствах в зависимости от используемой платформы, версии операционной системы и типа устройства, на котором выполняется приложение.

## <a name="related-links"></a>Связанные ссылки

- [Демоверсии привязок данных (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
