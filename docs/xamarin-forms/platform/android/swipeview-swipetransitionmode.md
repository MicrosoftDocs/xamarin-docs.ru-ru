---
title: Режим перехода Свипевиевного считывания в Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать зависящую от платформы Android платформу, управляющую переходом, который используется при открытии Свипевиев.
ms.prod: xamarin
ms.assetid: 6B1F8213-9D62-4C40-9F04-881F1371B5AA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c420fe65b020067169230dd06dbcd5ce65c036ab
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84128626"
---
# <a name="swipeview-swipe-transition-mode-on-android"></a>Режим перехода Свипевиевного считывания в Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Этот элемент управления, относящийся к платформе Android, управляет переходом, который используется при открытии `SwipeView` . Он используется в XAML путем задания `SwipeView.SwipeTransitionMode` свойству BIND значения `SwipeTransitionMode` перечисления.

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core" >
    <StackLayout>
        <SwipeView android:SwipeView.SwipeTransitionMode="Drag">
            <SwipeView.LeftItems>
                <SwipeItems>
                    <SwipeItem Text="Delete"
                               IconImageSource="delete.png"
                               BackgroundColor="LightPink"
                               Invoked="OnDeleteSwipeItemInvoked" />
                </SwipeItems>
            </SwipeView.LeftItems>
            <!-- Content -->
        </SwipeView>
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

SwipeView swipeView = new Xamarin.Forms.SwipeView();
swipeView.On<Android>().SetSwipeTransitionMode(SwipeTransitionMode.Drag);
// ...
```

`SwipeView.On<Android>`Метод указывает, что эта платформа будет запускаться только в Android. `SwipeView.SetSwipeTransitionMode`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для управления переходом, который используется при открытии `SwipeView` . `SwipeTransitionMode`Перечисление предоставляет два возможных значения:

- `Reveal`Указывает, что прокрутка элементов будет выдаваться по мере `SwipeView` прокрутки содержимого, и является значением свойства по умолчанию `SwipeView.SwipeTransitionMode` .
- `Drag`Указывает, что прокрутка элементов будет перемещена в представление при `SwipeView` прокрутке содержимого.

Кроме того, `SwipeView.GetSwipeTransitionMode` метод можно использовать для возврата объекта `SwipeTransitionMode` , который применяется к `SwipeView` .

В результате заданное `SwipeTransitionMode` значение применяется к элементу `SwipeView` , который управляет переходом, используемым при открытии `SwipeView` :

[![Снимок экрана Свипевиев Свипетранситионмодес на Android](swipeview-swipetransitionmode-images/swipetransitionmode.png "Свипетранситионмодес в Android")](swipeview-swipetransitionmode-images/swipetransitionmode-large.png#lightbox "Свипетранситионмодес в Android")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
