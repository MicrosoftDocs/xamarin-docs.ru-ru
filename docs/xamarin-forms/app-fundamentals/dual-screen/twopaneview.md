---
title: Макет для двух экранов Xamarin.Forms
description: В этом руководстве рассматривается использование контейнера TwoPaneView из Xamarin.Forms для оптимизации интерфейса приложения на двухэкранных устройствах, таких как Surface Duo и Surface Neo.
ms.prod: xamarin
ms.assetid: 17ee8afa-5e7c-4a4f-a9b6-2aca03f30fe3
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c1f5da6ed6a2c7f130e98a3f05ef2c11afe6995e
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373813"
---
# <a name="no-locxamarinforms-twopaneview-layout"></a>Xamarin.Forms Макет TwoPaneView

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)

Класс `TwoPaneView` — это контейнер с двумя представлениями, которые задают размер и расположение содержимого в рамках доступного на экране пространства: либо слева и справа, либо вверху и внизу. `TwoPaneView` наследует от элемента `Grid`, поэтому рекомендуется рассматривать свойства так же, как если бы они применялись к сетке.

## <a name="set-up-twopaneview"></a>Настройка TwoPaneView

Чтобы создать макет для двух экранов в приложении, выполните указанные ниже действия.

1. Выполните [начальные](index.md) инструкции, чтобы добавить NuGet и настроить класс Android `MainActivity`.
1. Начните с базового макета `TwoPaneView`, используя следующий код XAML:

    ```xaml
    <ContentPage
        xmlns:dualScreen="clr-namespace:Xamarin.Forms.DualScreen;assembly=Xamarin.Forms.DualScreen">
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

> [!TIP]
> В приведенном выше коде XAML в элементе `ContentPage` опущены многие стандартные атрибуты. При добавлении `TwoPaneView` в приложение не забудьте объявить пространство имен `xmlns:dualScreen`, как показано в примере.

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

- `TallModeConfiguration` — в вертикальном режиме это свойство задает расположение сверху и снизу либо отображение только одной области, как определено свойством TwoPaneViewPriority.
- `WideModeConfiguration` — в горизонтальном режиме это свойство позволяет задать расположение слева и справа либо отображение только одной области, как определено свойством TwoPaneViewPriority.
- `PanePriority` определяет, какая область (Pane1 или Pane2) будет отображаться в режиме SinglePane.

## <a name="related-links"></a>Связанные ссылки

- [DualScreen (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)