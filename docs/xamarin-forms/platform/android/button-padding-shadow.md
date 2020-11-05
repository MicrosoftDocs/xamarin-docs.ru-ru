---
title: Заполнение и тени кнопок на Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать зависящую от платформы Android платформу, которая использует заполнение по умолчанию и значения тени для кнопок Android.
ms.prod: xamarin
ms.assetid: BD2B60D1-DE6E-4691-A777-8EC5F560A4E9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0548b706fc59021b63e0edfcf5c72eb00645f7a1
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373449"
---
# <a name="button-padding-and-shadows-on-android"></a>Заполнение и тени кнопок на Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Данная платформа Android определяет Xamarin.Forms , используются ли кнопки для заполнения по умолчанию и значений тени для кнопок Android. Он используется в XAML путем установки [`Button.UseDefaultPadding`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPaddingProperty) [`Button.UseDefaultShadow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadowProperty) свойств и присоединенные `boolean` значения:

```xaml
<ContentPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button ...
                android:Button.UseDefaultPadding="true"
                android:Button.UseDefaultShadow="true" />         
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

button.On<Android>().SetUseDefaultPadding(true).SetUseDefaultShadow(true);
```

`Button.On<Android>`Метод указывает, что эта платформа будет запускаться только в Android. [ `Button.SetUseDefaultPadding` ] (Xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Button. Сетуседефаултпаддинг ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Button}, System. Boolean) и [ `Button.SetUseDefaultShadow` ] (xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Button. Сетуседефаултшадов ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Кнопка}, System. Boolean). в [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространстве имен используются для управления тем, используются ли Xamarin.Forms на кнопках заполнение по умолчанию и значения тени для кнопок Android. Кроме того, [ `Button.UseDefaultPadding` ] (xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Button. Уседефаултпаддинг ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Button})) и [ `Button.UseDefaultShadow` ] (xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Button. Уседефаултшадов ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Button})). можно использовать методы, чтобы возвратить, использует ли кнопка значение заполнения по умолчанию и значение тени по умолчанию соответственно.

В результате Xamarin.Forms кнопки могут использовать заполнение по умолчанию и значения тени для кнопок Android:

![Заполнение и затенение значений по умолчанию на кнопках Android](button-padding-shadow-images/button-padding-and-shadow.png)

Обратите внимание, что на снимке экрана выше каждый [`Button`](xref:Xamarin.Forms.Button) имеет идентичные определения, за исключением того, что в правой части `Button` используется заполнение по умолчанию и значения тени для кнопок Android.

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)