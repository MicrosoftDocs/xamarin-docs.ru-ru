---
title: Передача аргументов в XAML
description: В этой статье показано использование атрибутов XAML, которые можно использовать для передачи аргументов в конструкторы, отличные от по умолчанию, для вызова методов фабрики и для указания типа универсального аргумента.
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b07ec0ef50670aef5b933d5010d523989bb19eff
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374307"
---
# <a name="passing-arguments-in-xaml"></a>Передача аргументов в XAML

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/xaml-passingconstructorarguments)

_В этой статье показано использование атрибутов XAML, которые можно использовать для передачи аргументов в конструкторы, отличные от по умолчанию, для вызова методов фабрики и для указания типа универсального аргумента._

## <a name="overview"></a>Обзор

Часто требуется создавать экземпляры объектов с конструкторами, которые требуют аргументов, или путем вызова статического метода создания. Это можно сделать в XAML с помощью `x:Arguments` `x:FactoryMethod` атрибутов и:

- `x:Arguments`Атрибут используется для указания аргументов конструктора для конструктора, не являющегося конструктором по умолчанию, или для объявления объекта фабричного метода. Дополнительные сведения см. в разделе [Передача аргументов конструктора](#passing-constructor-arguments).
- `x:FactoryMethod`Атрибут используется для указания фабричного метода, который можно использовать для инициализации объекта. Дополнительные сведения см. в разделе [вызов фабричных методов](#calling-factory-methods).

Кроме того, `x:TypeArguments` атрибут можно использовать для указания аргументов универсального типа для конструктора универсального типа. Дополнительные сведения см. [в разделе Указание аргумента универсального типа](#specifying-a-generic-type-argument).

## <a name="passing-constructor-arguments"></a>Передача аргументов конструктора

Аргументы могут передаваться в конструктор, не используемый по умолчанию, с помощью `x:Arguments` атрибута. Каждый аргумент конструктора должен разделяться внутри XML-элемента, представляющего тип аргумента. Xamarin.Forms поддерживает следующие элементы для базовых типов:

- `x:Array`
- `x:Boolean`
- `x:Byte`
- `x:Char`
- `x:DateTime`
- `x:Decimal`
- `x:Double`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Object`
- `x:Single`
- `x:String`
- `x:TimeSpan`

В следующем примере кода показано использование `x:Arguments` атрибута с тремя [`Color`](xref:Xamarin.Forms.Color) конструкторами:

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.9</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.25</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.75</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.8</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.2</x:Double>
        <x:Double>0.5</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

Число элементов в `x:Arguments` теге и типы этих элементов должны соответствовать одному из [`Color`](xref:Xamarin.Forms.Color) конструкторов. Для `Color` [конструктора](xref:Xamarin.Forms.Color.%23ctor(System.Double)) с одним параметром требуется значение оттенка серого от 0 (черный) до 1 (белый). Для `Color` [конструктора](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double)) с тремя параметрами требуется красный, зеленый и синий значения в диапазоне от 0 до 1. `Color` [Конструктор](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double)) с четырьмя параметрами добавляет альфа-канал в качестве четвертого параметра.

На следующих снимках экрана показан результат вызова каждого [`Color`](xref:Xamarin.Forms.Color) конструктора с указанными значениями аргументов:

![Боксвиев. Color, заданный с помощью x:Arguments](passing-arguments-images/passing-arguments.png)

## <a name="calling-factory-methods"></a>Вызов методов фабрики

Фабричные методы могут быть вызваны в XAML путем указания имени метода с помощью `x:FactoryMethod` атрибута и его аргументов с помощью `x:Arguments` атрибута. Фабричный метод — это `public static` метод, возвращающий объекты или значения того же типа, что и класс или структура, определяющие методы.

[`Color`](xref:Xamarin.Forms.Color)Структура определяет ряд фабричных методов, и в следующем примере кода демонстрируется вызов трех из них:

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromRgba">
      <x:Arguments>
        <x:Int32>192</x:Int32>
        <x:Int32>75</x:Int32>
        <x:Int32>150</x:Int32>                        
        <x:Int32>128</x:Int32>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHsla">
      <x:Arguments>
        <x:Double>0.23</x:Double>
        <x:Double>0.42</x:Double>
        <x:Double>0.69</x:Double>
        <x:Double>0.7</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHex">
      <x:Arguments>
        <x:String>#FF048B9A</x:String>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

Число элементов в `x:Arguments` теге и типы этих элементов должны соответствовать аргументам вызываемого метода фабрики. [`FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32))Фабричный метод требует четыре [`Int32`](/dotnet/api/system.int32) параметра, представляющих красные, зеленые, синие и альфа-значения, в диапазоне от 0 до 255 соответственно. [`FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double))Для метода фабрики требуется четыре [`Double`](/dotnet/api/system.double) параметра, представляющие оттенок, насыщенность, яркость и альфа-значения в диапазоне от 0 до 1 соответственно. Для [`FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String)) метода фабрики требуется [`String`](/dotnet/api/system.string) , представляющий шестнадцатеричный цвет (a) RGB.

На следующих снимках экрана показан результат вызова каждого [`Color`](xref:Xamarin.Forms.Color) метода фабрики с указанными значениями аргументов:

![Боксвиев. Color, заданный с помощью x:FactoryMethod и x:Arguments](passing-arguments-images/factory-methods.png)

## <a name="specifying-a-generic-type-argument"></a>Указание аргумента универсального типа

Аргументы универсального типа для конструктора универсального типа можно указать с помощью `x:TypeArguments` атрибута, как показано в следующем примере кода:

```xaml
<ContentPage ...>
  <StackLayout>
    <StackLayout.Margin>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,20,0,0" />
        <On Platform="Android" Value="5, 10" />
        <On Platform="UWP" Value="10" />
      </OnPlatform>
    </StackLayout.Margin>
  </StackLayout>
</ContentPage>
```

[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1)Класс является универсальным классом и должен быть создан с помощью `x:TypeArguments` атрибута, соответствующего целевому типу. В [`On`](xref:Xamarin.Forms.On) классе [`Platform`](xref:Xamarin.Forms.On.Platform) атрибут может принимать одно `string` значение или несколько значений, разделенных запятыми `string` . В этом примере [`StackLayout.Margin`](xref:Xamarin.Forms.View.Margin) свойству присваивается значение для конкретной платформы [`Thickness`](xref:Xamarin.Forms.Thickness) .

Дополнительные сведения об аргументах универсального типа см. [в разделе Универсальные шаблоны в Xamarin.Forms XAML](generics.md).

## <a name="related-links"></a>Связанные ссылки

- [Передача аргументов конструктора (пример)](/samples/xamarin/xamarin-forms-samples/xaml-passingconstructorarguments)
- [Методы вызова фабрики (пример)](/samples/xamarin/xamarin-forms-samples/xaml-callingfactorymethods)
- [Пространства имен языка XAML](~/xamarin-forms/xaml/namespaces.md)
- [Универсальные шаблоны в Xamarin.Forms XAML](generics.md)