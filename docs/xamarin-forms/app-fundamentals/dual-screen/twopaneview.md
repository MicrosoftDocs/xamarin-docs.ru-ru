---
title: Макет для двух экранов Xamarin.Forms
description: В этом руководстве рассматривается использование контейнера TwoPaneView из Xamarin.Forms для оптимизации интерфейса приложения на двухэкранных устройствах, таких как Surface Duo и Surface Neo.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 28d4b3da44cc1a022b70c0de0720be747e047f9f
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138896"
---
# <a name="xamarinforms-dual-screen-layout"></a>Макет для двух экранов Xamarin.Forms

![](~/media/shared/preview.png "This API is currently pre-release")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)

Класс `TwoPaneView` — это контейнер с двумя представлениями, которые задают размер и расположение содержимого в рамках доступного на экране пространства: либо слева и справа, либо вверху и внизу. `TwoPaneView` наследует от элемента `Grid`, поэтому рекомендуется рассматривать свойства так же, как если бы они применялись к сетке.

## <a name="set-up-twopaneview"></a>Настройка TwoPaneView

Свойство `TwoPaneView.Source` может принимать универсальный код ресурса (URI) или локальный путь к файлу. Воспроизведение начнется сразу после открытия элемента мультимедиа:

```xaml
<ContentPage xmlns:dualScreen="clr-namespace:Xamarin.Forms.DualScreen;assembly=Xamarin.Forms.DualScreen">
    <dualScreen:TwoPaneView>
        <dualScreen:TwoPaneView.Pane1>
            <StackLayout>
                <Label Text="Pane1 Content" />
            </StackLayout>
        </dualScreen:TwoPaneView.Pane1>
        <dualScreen:TwoPaneView.Pane2>
            <StackLayout>
                <Label Text="Pane2 Content" />
            </StackLayout>
        </dualScreen:TwoPaneView.Pane2>
    </dualScreen:TwoPaneView>
</ContentPage>
```

## <a name="understand-twopaneview-modes"></a>Сведения о режимах TwoPaneView

Активным может быть только один из следующих режимов:

- `SinglePane` — сейчас отображается только одна область.
- `Wide` — две области располагаются горизонтально. Одна область находится слева, а другая — справа. На двухэкранном устройстве это режим с книжной ориентацией.
- `Tall` — две области располагаются вертикально. Одна область находится вверху, а другая — внизу. На двухэкранном устройстве это режим с альбомной ориентацией.

## <a name="control-twopaneview-when-its-only-on-one-screen"></a>Управление TwoPaneView только на одном экране

Следующие свойства применяются, когда `TwoPaneView` занимает один экран:

- `MinTallModeHeight` указывает минимальную высоту элемента управления для перехода в вертикальный режим.
- `MinWideModeWidth` указывает минимальную ширину элемента управления для перехода в горизонтальный режим.
- `Pane1Length` задает ширину области Pane1 в горизонтальном режиме (Wide) и высоту области Pane1 в вертикальном режиме (Tall). В режиме SinglePane не действует.
- `Pane2Length` задает ширину области Pane2 в горизонтальном режиме (Wide), высоту области Pane2 в вертикальном режиме (Tall). В режиме SinglePane не действует.

> [!IMPORTANT]
> Если `TwoPaneView` размещается на двух экранах, эти свойства не действуют.

## <a name="properties-that-apply-when-on-one-screen-or-two"></a>Свойства для одного и двух экранов

Следующие свойства применяются, когда `TwoPaneView` занимает один экран или два экрана:

- `TallModeConfiguration` — в вертикальном режиме это свойство задает расположение слева и справа либо отображение только одной области, как определено свойством TwoPaneViewPriority.
- `WideModeConfiguration` — в горизонтальном режиме это свойство задает расположение сверху и снизу либо отображение только одной области, как определено свойством TwoPaneViewPriority.
- `PanePriority` определяет, какая область (Pane1 или Pane2) будет отображаться в режиме SinglePane.

## <a name="related-links"></a>Связанные ссылки

- [DualScreen (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)
