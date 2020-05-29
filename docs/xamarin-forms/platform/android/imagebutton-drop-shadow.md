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
ms.openlocfilehash: 5e2ad97eb5e7db3b832e8fb4340c86904b766b9a
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140004"
---
# <a name="imagebutton-drop-shadows-on-android"></a>Теневые тени ImageButton на Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа для Android используется для включения тени на `ImageButton` . Он используется в XAML путем установки `ImageButton.IsShadowEnabled` Свойства BIND в `true` , а также ряда дополнительных необязательных свойств, которые могут быть привязаны для управления тенью.

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
       <ImageButton ...
                    Source="XamarinLogo.png"
                    BackgroundColor="GhostWhite"
                    android:ImageButton.IsShadowEnabled="true"
                    android:ImageButton.ShadowColor="Gray"
                    android:ImageButton.ShadowRadius="12">
            <android:ImageButton.ShadowOffset>
                <Size>
                    <x:Arguments>
                        <x:Double>10</x:Double>
                        <x:Double>10</x:Double>
                    </x:Arguments>
                </Size>
            </android:ImageButton.ShadowOffset>
        </ImageButton>
        ...
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var imageButton = new Xamarin.Forms.ImageButton { Source = "XamarinLogo.png", BackgroundColor = Color.GhostWhite, ... };
imageButton.On<Android>()
           .SetIsShadowEnabled(true)
           .SetShadowColor(Color.Gray)
           .SetShadowOffset(new Size(10, 10))
           .SetShadowRadius(12);
```

> [!IMPORTANT]
> Тень рисуется как часть `ImageButton` фона, а фон рисуется только в том случае, если `BackgroundColor` свойство задано. Таким образом, тень не будет отображаться, если `ImageButton.BackgroundColor` свойство не задано.

`ImageButton.On<Android>`Метод указывает, что эта платформа будет запускаться только в Android. `ImageButton.SetIsShadowEnabled`Метод в [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространстве имен используется для управления включением тени на `ImageButton` . Кроме того, можно вызвать следующие методы для управления тенью тени:

- `SetShadowColor`— Задает цвет тени. Цвет по умолчанию — [`Color.Default`](xref:Xamarin.Forms.Color.Default*) .
- `SetShadowOffset`— Задает смещение тени. Смещение изменяет направление, в котором происходит приведение тени, и указывается как [`Size`](xref:Xamarin.Forms.Size) значение. `Size`Значения структуры выражаются в единицах, не зависящих от устройства, где первое значение равно расстоянию слева (отрицательное значение) или правому (положительное значение), а второе значение равно расстоянию выше (отрицательное значение) или ниже (положительное значение). Значение этого свойства по умолчанию равно (0,0, 0,0), что приводит к приведению тени вокруг каждой стороны `ImageButton` .
- `SetShadowRadius`— задает радиус размытия, используемый для визуализации тени. Значение радиуса по умолчанию — 10,0.

> [!NOTE]
> Состояние тени можно запрашивать путем вызова `GetIsShadowEnabled` `GetShadowColor` методов,, `GetShadowOffset` и `GetShadowRadius` .

В результате тень может быть включена в `ImageButton` :

![](imagebutton-drop-shadow-images/imagebutton-drop-shadow.png "ImageButton with drop shadow")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
