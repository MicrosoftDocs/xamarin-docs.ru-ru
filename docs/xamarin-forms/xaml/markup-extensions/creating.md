---
title: Создание расширений разметки XAML
description: В этой статье объясняется, как определить собственные пользовательские Xamarin.Forms расширения разметки XAML. Расширение разметки XAML — это класс, реализующий интерфейс Имаркупекстенсион или Имаркупекстенсион <T> .
ms.prod: xamarin
ms.assetid: 797C1EF9-1C8E-4208-8610-9B79CCF17D46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 43c8cd0dd7b50e3a5bfbd15d9858bd4502fedacc
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91558782"
---
# <a name="creating-xaml-markup-extensions"></a>Создание расширений разметки XAML

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

На программном уровне расширение разметки XAML является классом, реализующим [`IMarkupExtension`](xref:Xamarin.Forms.Xaml.IMarkupExtension) интерфейс или [`IMarkupExtension<T>`](xref:Xamarin.Forms.Xaml.IMarkupExtension`1) . Исходный код стандартных расширений разметки, описанных ниже, можно просмотреть в каталоге [ **расширений MarkupExtension** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) Xamarin.Forms репозитория GitHub.

Также можно определить собственные расширения разметки XAML, производные от `IMarkupExtension` или `IMarkupExtension<T>` . Используйте универсальную форму, если расширение разметки получает значение определенного типа. В этом случае есть несколько Xamarin.Forms расширений разметки:

- `TypeExtension` происходит от `IMarkupExtension<Type>`.
- `ArrayExtension` происходит от `IMarkupExtension<Array>`.
- `DynamicResourceExtension` происходит от `IMarkupExtension<DynamicResource>`.
- `BindingExtension` происходит от `IMarkupExtension<BindingBase>`.
- `ConstraintExpression` происходит от `IMarkupExtension<Constraint>`.

Два `IMarkupExtension` интерфейса определяют только один метод, названный `ProvideValue` :

```csharp
public interface IMarkupExtension
{
    object ProvideValue(IServiceProvider serviceProvider);
}

public interface IMarkupExtension<out T> : IMarkupExtension
{
    new T ProvideValue(IServiceProvider serviceProvider);
}
```

Поскольку `IMarkupExtension<T>` является производным от `IMarkupExtension` и включает `new` ключевое слово в `ProvideValue` , оно содержит оба `ProvideValue` метода.

Очень часто расширения разметки XAML определяют свойства, которые вносят вклад в возвращаемое значение. (Очевидное исключение — `NullExtension` , в котором `ProvideValue` просто возвращается `null` .) `ProvideValue` Метод имеет единственный аргумент типа `IServiceProvider` , который будет рассмотрен далее в этой статье.

## <a name="a-markup-extension-for-specifying-color"></a>Расширение разметки для указания цвета

Следующее расширение разметки XAML позволяет создавать `Color` значения с помощью оттенков, насыщенности и компонентов яркости. Он определяет четыре свойства для четырех компонентов цвета, включая альфа-компонент, который инициализируется значением 1. Класс является производным от, `IMarkupExtension<Color>` чтобы указать `Color` возвращаемое значение:

```csharp
public class HslColorExtension : IMarkupExtension<Color>
{
    public double H { set; get; }

    public double S { set; get; }

    public double L { set; get; }

    public double A { set; get; } = 1.0;

    public Color ProvideValue(IServiceProvider serviceProvider)
    {
        return Color.FromHsla(H, S, L, A);
    }

    object IMarkupExtension.ProvideValue(IServiceProvider serviceProvider)
    {
        return (this as IMarkupExtension<Color>).ProvideValue(serviceProvider);
    }
}
```

Поскольку `IMarkupExtension<T>` класс является производным от `IMarkupExtension` класса, он должен содержать два `ProvideValue` метода, один из которых возвращает `Color` `object` , а второй метод — просто вызвать первый метод.

На странице **демонстрационный цвет для HSL** показаны различные способы, которые `HslColorExtension` могут ПОЯВИТЬСЯ в файле XAML для указания цвета для `BoxView` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.HslColorDemoPage"
             Title="HSL Color Demo">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="BoxView">
                <Setter Property="WidthRequest" Value="80" />
                <Setter Property="HeightRequest" Value="80" />
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <BoxView>
            <BoxView.Color>
                <local:HslColorExtension H="0" S="1" L="0.5" A="1" />
            </BoxView.Color>
        </BoxView>

        <BoxView>
            <BoxView.Color>
                <local:HslColor H="0.33" S="1" L="0.5" />
            </BoxView.Color>
        </BoxView>

        <BoxView Color="{local:HslColorExtension H=0.67, S=1, L=0.5}" />

        <BoxView Color="{local:HslColor H=0, S=0, L=0.5}" />

        <BoxView Color="{local:HslColor A=0.5}" />
    </StackLayout>
</ContentPage>
```

Обратите внимание, что, когда `HslColorExtension` является XML-тегом, четыре свойства задаются как атрибуты, но если они появляются между фигурными скобками, четыре свойства разделяются запятыми без кавычек. Значения по умолчанию для параметров `H` , `S` и `L` равны 0, а значение по умолчанию `A` равно 1, поэтому эти свойства можно опустить, если требуется, чтобы они были установлены в значения по умолчанию. В последнем примере показан пример, в котором яркость равна 0, что обычно приводит к черному, но альфа-каналу является 0,5, поэтому он находится на половину прозрачной и отображается серым на белом фоне страницы:

[![Демонстрация цвета для HSL](creating-images/hslcolordemo-small.png "Демонстрация цвета для HSL")](creating-images/hslcolordemo-large.png#lightbox "Демонстрация цвета для HSL")

## <a name="a-markup-extension-for-accessing-bitmaps"></a>Расширение разметки для доступа к точечным рисункам

Аргумент для `ProvideValue` — это объект, реализующий [`IServiceProvider`](xref:System.IServiceProvider) интерфейс, который определен в `System` пространстве имен .NET. Этот интерфейс содержит один член — метод с именем и `GetService` `Type` аргумент.

`ImageResourceExtension`Класс, показанный ниже, показывает одно возможное использование `IServiceProvider` и `GetService` для получения `IXmlLineInfoProvider` объекта, который может предоставить сведения о строке и символах, указывающие, где была обнаружена определенная ошибка. В этом случае возникает исключение, если `Source` свойство не задано:

```csharp
[ContentProperty("Source")]
class ImageResourceExtension : IMarkupExtension<ImageSource>
{
    public string Source { set; get; }

    public ImageSource ProvideValue(IServiceProvider serviceProvider)
    {
        if (String.IsNullOrEmpty(Source))
        {
            IXmlLineInfoProvider lineInfoProvider = serviceProvider.GetService(typeof(IXmlLineInfoProvider)) as IXmlLineInfoProvider;
            IXmlLineInfo lineInfo = (lineInfoProvider != null) ? lineInfoProvider.XmlLineInfo : new XmlLineInfo();
            throw new XamlParseException("ImageResourceExtension requires Source property to be set", lineInfo);
        }

        string assemblyName = GetType().GetTypeInfo().Assembly.GetName().Name;
        return ImageSource.FromResource(assemblyName + "." + Source, typeof(ImageResourceExtension).GetTypeInfo().Assembly);
    }

    object IMarkupExtension.ProvideValue(IServiceProvider serviceProvider)
    {
        return (this as IMarkupExtension<ImageSource>).ProvideValue(serviceProvider);
    }
}
```

`ImageResourceExtension` полезен, когда XAML-файл должен получить доступ к файлу изображения, хранящемуся как внедренный ресурс в проекте библиотеки .NET Standard. `Source`Для вызова статического метода используется свойство `ImageSource.FromResource` . Для этого метода требуется полное имя ресурса, состоящее из имени сборки, имени папки и имени файла, разделенного точками. Второй аргумент `ImageSource.FromResource` метода предоставляет имя сборки и является обязательным только для сборок выпуска в UWP. Независимо от этого, он `ImageSource.FromResource` должен вызываться из сборки, содержащей точечный рисунок. Это означает, что это расширение ресурса XAML не может быть частью внешней библиотеки, если изображения также не находятся в этой библиотеке. (Дополнительные сведения о доступе к точечным рисункам, хранящимся в виде внедренных ресурсов, см. в статье [**внедренные изображения**](~/xamarin-forms/user-interface/images.md#embedded-images) .)

Хотя `ImageResourceExtension` требует `Source` установки свойства, `Source` свойство указывается в атрибуте как свойство Content класса. Это означает, что `Source=` часть выражения в фигурных скобках может быть опущена. В **демонстрационной странице ресурса изображения** элементы получают `Image` два изображения, используя имя папки и имя файла, разделенные точками:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.ImageResourceDemoPage"
             Title="Image Resource Demo">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Image Source="{local:ImageResource Images.SeatedMonkey.jpg}"
               Grid.Row="0" />

        <Image Source="{local:ImageResource Images.FacePalm.jpg}"
               Grid.Row="1" />

    </Grid>
</ContentPage>
```

Вот работающая программа:

[![Демонстрация ресурса изображения](creating-images/imageresourcedemo-small.png "Демонстрация ресурса изображения")](creating-images/imageresourcedemo-large.png#lightbox "Демонстрация ресурса изображения")

## <a name="service-providers"></a>Поставщики услуг

С помощью `IServiceProvider` аргумента в `ProvideValue` расширения разметки XAML могут получить доступ к полезной информации о файле XAML, в котором они используются. Но для успешного использования `IServiceProvider` аргумента необходимо знать, какие типы служб доступны в определенных контекстах. Лучший способ получить представление об этой функции — изучить исходный код существующих расширений разметки XAML в [папке **расширений MarkupExtension** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) в Xamarin.Forms репозитории на сайте GitHub. Имейте в виду, что некоторые типы служб являются внутренними для Xamarin.Forms .

В некоторых расширениях разметки XAML эта служба может оказаться полезной:

```csharp
 IProvideValueTarget provideValueTarget = serviceProvider.GetService(typeof(IProvideValueTarget)) as IProvideValueTarget;
```

`IProvideValueTarget`Интерфейс определяет два свойства: `TargetObject` и `TargetProperty` . Когда эта информация получается в `ImageResourceExtension` классе, параметр `TargetObject` является `Image` объектом и `TargetProperty` является `BindableProperty` объект для `Source` свойства `Image` . Это свойство, для которого было задано расширение разметки XAML.

`GetService`Вызов с аргументом `typeof(IProvideValueTarget)` фактически возвращает объект типа `SimpleValueTargetProvider` , который определен в `Xamarin.Forms.Xaml.Internals` пространстве имен. При приведении возвращаемого значения `GetService` к этому типу можно также получить доступ к `ParentObjects` свойству, которое является массивом, содержащим `Image` элемент, `Grid` родительский объект и `ImageResourceDemoPage` родительский `Grid` объект для.

## <a name="conclusion"></a>Заключение

Расширения разметки XAML играют важную роль в XAML, расширяя возможность устанавливать атрибуты из различных источников. Более того, если существующие расширения разметки XAML не предоставляют именно то, что вам нужно, вы также можете написать собственный.

## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки (пример)](/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [Глава о расширениях разметки XAML из Xamarin.Forms книги](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)