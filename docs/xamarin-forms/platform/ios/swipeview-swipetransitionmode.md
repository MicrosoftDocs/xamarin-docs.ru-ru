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
ms.openlocfilehash: 4b2030461025c1cd647595a1ecc22c5589e99fef
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137050"
---
# <a name="swipeview-swipe-transition-mode-on-ios"></a>Режим перехода Свипевиевного считывания в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Этот элемент управления, относящийся к платформе iOS, управляет переходом, который используется при открытии `SwipeView` . Он используется в XAML путем задания `SwipeView.SwipeTransitionMode` свойству BIND значения `SwipeTransitionMode` перечисления.

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SwipeView ios:SwipeView.SwipeTransitionMode="Drag">
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
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

SwipeView swipeView = new Xamarin.Forms.SwipeView();
swipeView.On<iOS>().SetSwipeTransitionMode(SwipeTransitionMode.Drag);
// ...
```

`SwipeView.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `SwipeView.SetSwipeTransitionMode`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для управления переходом, который используется при открытии `SwipeView` . `SwipeTransitionMode`Перечисление предоставляет два возможных значения:

- `Reveal`Указывает, что прокрутка элементов будет выдаваться по мере `SwipeView` прокрутки содержимого, и является значением свойства по умолчанию `SwipeView.SwipeTransitionMode` .
- `Drag`Указывает, что прокрутка элементов будет перемещена в представление при `SwipeView` прокрутке содержимого.

Кроме того, `SwipeView.GetSwipeTransitionMode` метод можно использовать для возврата объекта `SwipeTransitionMode` , который применяется к `SwipeView` .

В результате заданное `SwipeTransitionMode` значение применяется к элементу `SwipeView` , который управляет переходом, используемым при открытии `SwipeView` :

[![Снимок экрана Свипевиев Свипетранситионмодес в iOS](swipeview-swipetransitionmode-images/swipetransitionmode.png "Свипетранситионмодес в iOS")](swipeview-swipetransitionmode-images/swipetransitionmode-large.png#lightbox "Свипетранситионмодес в iOS")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
