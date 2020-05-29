---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4d934fd2155a6a088dd543658555bf104b38f302
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138545"
---
# <a name="entry-cursor-color-on-ios"></a>Цвет курсора ввода в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS задает цвет курсора [`Entry`](xref:Xamarin.Forms.Entry) для определенного цвета. Он используется в XAML путем установки [`Entry.CursorColor`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.CursorColorProperty) Свойства BIND в значение [`Color`](xref:Xamarin.Forms.Color) :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Entry ... ios:Entry.CursorColor="LimeGreen" />
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var entry = new Xamarin.Forms.Entry();
entry.On<iOS>().SetCursorColor(Color.LimeGreen);
```

`Entry.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. [ `Entry.SetCursorColor` ] (Xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Entry. Сеткурсорколор ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Entry}, Xamarin.Forms . Цвет). в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен в качестве цвета курсора задается указанный [`Color`](xref:Xamarin.Forms.Color) . Кроме того, [ `Entry.GetCursorColor` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Entry. Жеткурсорколор ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Entry})). для получения текущего цвета курсора можно использовать метод.

В результате цвет курсора в [`Entry`](xref:Xamarin.Forms.Entry) может быть установлен для конкретного [`Color`](xref:Xamarin.Forms.Color) :

![](entry-cursor-color-images/entry-cursorcolor.png "Entry Cursor Color")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
