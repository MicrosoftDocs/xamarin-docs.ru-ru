---
title: Универсальные шаблоны в Xamarin.Forms XAML
description: Xamarin.Forms XAML обеспечивает поддержку использования универсальных типов CLR путем указания универсальных ограничений в качестве аргументов типа.
ms.prod: xamarin
ms.assetid: 97B73048-4F90-41AD-AB48-8EB804C4998B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e6856e0ef513905a6300dcaf661ea33f4a89852c
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91563917"
---
# <a name="generics-in-no-locxamarinforms-xaml"></a>Универсальные шаблоны в Xamarin.Forms XAML

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-generics/)

Xamarin.Forms XAML обеспечивает поддержку использования универсальных типов CLR путем указания универсальных ограничений в качестве аргументов типа. Эта поддержка обеспечивается `x:TypeArguments` директивой, которая передает аргументы универсального типа с ограничением по отношению к конструктору универсального типа.

> [!IMPORTANT]
> Определение универсальных классов в Xamarin.Forms XAML с помощью `x:TypeArguments` директивы не поддерживается.

Аргументы типа указываются в виде строки и обычно являются префиксами, такими как `sys:String` и `sys:Int32` . Требуется префикс, поскольку стандартные типы универсальных ограничений CLR берутся из библиотек, которые не сопоставлены с Xamarin.Forms пространством имен по умолчанию. Однако встроенные типы XAML 2009, такие как `x:String` и `x:Int32` , также могут быть указаны в виде аргументов типа, где `x` — это пространство имен языка xaml для XAML 2009. Дополнительные сведения о встроенных типах XAML 2009 см. в разделе [Языковые примитивы xaml 2009](/dotnet/desktop-wpf/xaml-services/types-for-primitives#xaml-2009-language-primitives).

Несколько аргументов типа можно указать с помощью разделителя запятыми. Кроме того, если универсальное ограничение использует универсальные типы, аргументы типа вложенного ограничения должны содержаться в круглых скобках.

> [!NOTE]
> `x:Type`Расширение разметки предоставляет ссылку на тип CLR для универсального типа и имеет аналогичную функцию для `typeof` оператора в C#. Дополнительные сведения см. в разделе [расширение разметки x:Type](~/xamarin-forms/xaml/markup-extensions/consuming.md#xtype-markup-extension).

## <a name="single-primitive-type-argument"></a>Один аргумент примитивного типа

Один аргумент примитивного типа может быть указан как префикс строкового аргумента с помощью `x:TypeArguments` директивы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="x:String">
                <x:String>Baboon</x:String>
                <x:String>Capuchin Monkey</x:String>
                <x:String>Blue Monkey</x:String>
                <x:String>Squirrel Monkey</x:String>
                <x:String>Golden Lion Tamarin</x:String>
                <x:String>Howler Monkey</x:String>
                <x:String>Japanese Macaque</x:String>
            </scg:List>
        </CollectionView.ItemsSource>
    </CollectionView>
</ContentPage>
```

В этом примере `System.Collections.Generic` определен как `scg` пространство имен XAML. Свойству присваивается значение `CollectionView.ItemsSource` `List<T>` , которое создается с `string` помощью аргумента типа, используя встроенный тип XAML 2009 `x:String` . `List<string>`Коллекция инициализируется несколькими `string` элементами.

Кроме того, `List<T>` можно создать экземпляр коллекции с типом CLR, но эквивалентным `String` образом.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             xmlns:sys="clr-namespace:System;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="sys:String">
                <sys:String>Baboon</sys:String>
                <sys:String>Capuchin Monkey</sys:String>
                <sys:String>Blue Monkey</sys:String>
                <sys:String>Squirrel Monkey</sys:String>
                <sys:String>Golden Lion Tamarin</sys:String>
                <sys:String>Howler Monkey</sys:String>
                <sys:String>Japanese Macaque</sys:String>
            </scg:List>
        </CollectionView.ItemsSource>
    </CollectionView>
</ContentPage>
```

## <a name="single-object-type-argument"></a>Аргумент одного типа объекта

Один аргумент типа объекта может быть указан как префикс строкового аргумента с помощью `x:TypeArguments` директивы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:models="clr-namespace:GenericsDemo.Models"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="models:Monkey">
                <models:Monkey Name="Baboon"
                               Location="Africa and Asia"
                               ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg" />
                <models:Monkey Name="Capuchin Monkey"
                               Location="Central and South America"
                               ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg" />
                <models:Monkey Name="Blue Monkey"
                               Location="Central and East Africa"
                               ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg" />
            </scg:List>
        </CollectionView.ItemsSource>
        <CollectionView.ItemTemplate>
            <DataTemplate>
                <Grid Padding="10">
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="Auto" />
                    </Grid.ColumnDefinitions>
                    <Image Grid.RowSpan="2"
                           Source="{Binding ImageUrl}"
                           Aspect="AspectFill"
                           HeightRequest="60"
                           WidthRequest="60" />
                    <Label Grid.Column="1"
                           Text="{Binding Name}"
                           FontAttributes="Bold" />
                    <Label Grid.Row="1"
                           Grid.Column="1"
                           Text="{Binding Location}"
                           FontAttributes="Italic"
                           VerticalOptions="End" />
                </Grid>
            </DataTemplate>
        </CollectionView.ItemTemplate>
    </CollectionView>
</ContentPage>
```

В этом примере определяется `GenericsDemo.Models` как `models` пространство имен XAML и `System.Collections.Generic` определяется как `scg` пространство имен XAML. `CollectionView.ItemsSource`Свойство имеет значение `List<T>` , для которого создается экземпляр с `Monkey` аргументом типа. `List<Monkey>`Коллекция инициализируется с помощью нескольких `Monkey` элементов, а объект [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , определяющий внешний вид каждого объекта, `Monkey` задается в виде объекта `ItemTemplate` [`CollectionView`](xref:Xamarin.Forms.CollectionView) .

## <a name="multiple-type-arguments"></a>Несколько аргументов типа

Несколько аргументов типа могут быть указаны в виде префиксных строковых аргументов, разделенных запятыми с помощью `x:TypeArguments` директивы. Если универсальное ограничение использует универсальные типы, аргументы типа вложенного ограничения содержатся в круглых скобках:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:models="clr-namespace:GenericsDemo.Models"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="scg:KeyValuePair(x:String,models:Monkey)">
                <scg:KeyValuePair x:TypeArguments="x:String,models:Monkey">
                    <x:Arguments>
                        <x:String>Baboon</x:String>
                        <models:Monkey Location="Africa and Asia"
                                       ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg" />
                    </x:Arguments>
                </scg:KeyValuePair>
                <scg:KeyValuePair x:TypeArguments="x:String,models:Monkey">
                    <x:Arguments>
                        <x:String>Capuchin Monkey</x:String>
                        <models:Monkey Location="Central and South America"
                                       ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg" />   
                    </x:Arguments>
                </scg:KeyValuePair>
                <scg:KeyValuePair x:TypeArguments="x:String,models:Monkey">
                    <x:Arguments>
                        <x:String>Blue Monkey</x:String>
                        <models:Monkey Location="Central and East Africa"
                                       ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg" />
                    </x:Arguments>
                </scg:KeyValuePair>
            </scg:List>
        </CollectionView.ItemsSource>
        <CollectionView.ItemTemplate>
            <DataTemplate>
                <Grid Padding="10">
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="Auto" />
                    </Grid.ColumnDefinitions>
                    <Image Grid.RowSpan="2"
                           Source="{Binding Value.ImageUrl}"
                           Aspect="AspectFill"
                           HeightRequest="60"
                           WidthRequest="60" />
                    <Label Grid.Column="1"
                           Text="{Binding Key}"
                           FontAttributes="Bold" />
                    <Label Grid.Row="1"
                           Grid.Column="1"
                           Text="{Binding Value.Location}"
                           FontAttributes="Italic"
                           VerticalOptions="End" />
                </Grid>
            </DataTemplate>
        </CollectionView.ItemTemplate>
    </CollectionView>
</ContentPage    
```

В этом примере определяется `GenericsDemo.Models` как `models` пространство имен XAML и `System.Collections.Generic` определяется как `scg` пространство имен XAML. Свойству присваивается значение `CollectionView.ItemsSource` `List<T>` , которое создается с `KeyValuePair<TKey, TValue>` ограничением, с аргументами типа внутреннего ограничения `string` и `Monkey` . `List<KeyValuePair<string,Monkey>>`Коллекция инициализируется с `KeyValuePair` помощью нескольких элементов, с использованием конструктора, не являющегося конструктором по умолчанию `KeyValuePair` , и [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , определяющего внешний вид каждого `Monkey` объекта, задается в виде объекта `ItemTemplate` [`CollectionView`](xref:Xamarin.Forms.CollectionView) . Дополнительные сведения о передаче аргументов конструктору, не являющемуся по умолчанию, см. в разделе [Передача аргументов конструктора](~/xamarin-forms/xaml/passing-arguments.md#passing-constructor-arguments).

## <a name="related-links"></a>Связанные ссылки

- [Универсальные шаблоны в XAML (пример)](/samples/xamarin/xamarin-forms-samples/xaml-generics/)
- [Примитивы языка XAML 2009](/dotnet/desktop-wpf/xaml-services/types-for-primitives#xaml-2009-language-primitives)
- [Расширение разметки x:Type](~/xamarin-forms/xaml/markup-extensions/consuming.md#xtype-markup-extension)
- [Передача аргументов конструктора](~/xamarin-forms/xaml/passing-arguments.md#passing-constructor-arguments)