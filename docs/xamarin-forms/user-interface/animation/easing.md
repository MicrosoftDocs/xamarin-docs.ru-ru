---
title: Ускорение функций в Xamarin.Forms
description: Xamarin.Forms включает класс замедления, позволяющий указать функцию передачи, которая управляет скоростью анимации или замедляется при их выполнении. В этой статье показано, как использовать предварительно определенные функции плавности и как создавать пользовательские функции плавности.
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 50b64b394314ae2f63ab1f756f1cc73ba29e59e7
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93372851"
---
# <a name="easing-functions-in-no-locxamarinforms"></a>Ускорение функций в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-animation-easing)

_Xamarin.Forms включает класс замедления, позволяющий указать функцию передачи, которая управляет скоростью анимации или замедляется при их выполнении. В этой статье показано, как использовать предварительно определенные функции плавности и как создавать пользовательские функции плавности._

[`Easing`](xref:Xamarin.Forms.Easing)Класс определяет ряд функций плавности, которые могут использоваться анимациями:

- [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn)Функция плавности посылает анимацию в начале.
- [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut)Функция плавности посылает анимацию в конце.
- [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn)Функция плавности медленно ускоряет анимацию.
- [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut)Функция плавно ускоряет анимацию в начале и замедляет анимацию в конце.
- [`CubicOut`](xref:Xamarin.Forms.Easing.CubicOut)Функция плавности быстро замедляет анимацию.
- [`Linear`](xref:Xamarin.Forms.Easing.Linear)Функция плавности использует постоянную скорость, а — функцию плавности по умолчанию.
- [`SinIn`](xref:Xamarin.Forms.Easing.SinIn)Функция плавности плавно ускоряет анимацию.
- [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut)Функция плавности плавно ускоряет анимацию в начале и плавно замедляет анимацию в конце.
- [`SinOut`](xref:Xamarin.Forms.Easing.SinOut)Функция плавности плавно замедляет анимацию.
- [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn)Функция плавности приводит к тому, что анимация очень быстро ускоряется в сторону конца.
- [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut)Функция плавности приводит к быстрому замедлению анимации в направлении конца.

`In` `Out` Суффиксы и указывают, является ли эффект, предоставленный функцией плавности, заметным в начале анимации, в конце или в обоих случаях.

Кроме того, можно создавать пользовательские функции плавности. Дополнительные сведения см. в разделе [пользовательские функции плавности](#custom-easing-functions).

## <a name="consuming-an-easing-function"></a>Использование функции плавности

Методы расширения анимации [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) класса позволяют указать функцию плавности в качестве завершающего параметра метода, как показано в следующем примере кода:

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

Задавая функцию плавности анимации, скорость анимации преобразуется в нелинейную и выдает эффект, предоставляемый функцией плавности. Пропуск функции плавности при создании анимации приводит к тому, что анимация использует [`Linear`](xref:Xamarin.Forms.Easing.Linear) функцию плавности по умолчанию, которая создает линейную скорость.

Дополнительные сведения об использовании методов расширения анимации в [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) классе см. в разделе [простая анимация](~/xamarin-forms/user-interface/animation/simple.md). Функции плавности могут также использоваться [`Animation`](xref:Xamarin.Forms.Animation) классом. Дополнительные сведения см. в разделе [пользовательские анимации](~/xamarin-forms/user-interface/animation/custom.md).

## <a name="custom-easing-functions"></a>Пользовательские функции плавности

Существует три основных подхода к созданию пользовательской функции плавности:

1. Создайте метод, который принимает `double` аргумент и возвращает `double` результат.
1. Создайте таблицу `Func<double, double>`.
1. Укажите функцию плавности в качестве аргумента [`Easing`](xref:Xamarin.Forms.Easing) конструктора.

Во всех трех случаях пользовательская функция плавности должна возвращать значение 0 для аргумента 0 и значение 1 для аргумента, равного 1. Однако любое значение может возвращаться между значениями аргументов 0 и 1. Теперь каждый подход будет рассмотрен в свою очередь.

### <a name="custom-easing-method"></a>Пользовательский метод плавности

Пользовательская функция плавности может быть определена как метод, принимающий `double` аргумент, и возвращает `double` результат, как показано в следующем примере кода:

```csharp
double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}

await image.TranslateTo(0, 200, 2000, (Easing)CustomEase);
```

`CustomEase`Метод усекает входящее значение до значений 0, 0,2, 0,4, 0,6, 0,8 и 1. Таким образом, [`Image`](xref:Xamarin.Forms.Image) экземпляр преобразуется в отдельные переходы, а не плавно.

### <a name="custom-easing-func"></a>Пользовательская замедление Func

Пользовательская функция плавности может также быть определена как `Func<double, double>` , как показано в следующем примере кода:

```csharp
Func<double, double> CustomEaseFunc = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEaseFunc);
```

`CustomEaseFunc`Представляет функцию плавности, которая запускается быстро, замедляется и отменяет курс, а затем снова обращается к концу, чтобы быстро ускорить работу. Таким образом, в то время как общее перемещение [`Image`](xref:Xamarin.Forms.Image) экземпляра направлено вниз, оно также временно обращается к курсу на расстоянии по всей анимации.

### <a name="custom-easing-constructor"></a>Пользовательский конструктор плавности

Пользовательская функция плавности также может быть определена в качестве аргумента для [`Easing`](xref:Xamarin.Forms.Easing) конструктора, как показано в следующем примере кода:

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

Пользовательская функция плавности указывается в качестве аргумента лямбда-функции для [`Easing`](xref:Xamarin.Forms.Easing) конструктора и использует `Math.Cos` метод для создания медленных эффектов перетаскивания, которые допускают `Math.Exp` метод. Таким образом, [`Image`](xref:Xamarin.Forms.Image) экземпляр преобразуется таким образом, что он перемещается в окончательное место размещения.

## <a name="summary"></a>Сводка

В этой статье показано, как использовать предварительно определенные функции плавности и как создавать пользовательские функции плавности. Xamarin.Forms включает [`Easing`](xref:Xamarin.Forms.Easing) класс, который позволяет указать функцию передачи, которая управляет скоростью анимации или замедляется при их выполнении.

## <a name="related-links"></a>Связанные ссылки

- [Общие сведения о поддержке асинхронного выполнения](~/cross-platform/platform/async.md)
- [Функции плавности (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-animation-easing)
- [Медлен](xref:Xamarin.Forms.Easing)
- [виевекстенсионс](xref:Xamarin.Forms.ViewExtensions)