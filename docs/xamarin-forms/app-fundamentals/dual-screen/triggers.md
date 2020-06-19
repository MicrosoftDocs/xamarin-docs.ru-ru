---
title: Триггеры двухэкранного режима в Xamarin.Forms
description: В этой статье содержатся сведения об использовании триггеров двухэкранного режима Xamarin.Forms для реагирования на изменения пользовательского интерфейса с помощью XAML.
ms.prod: xamarin
ms.assetid: 2181715D-3995-4E71-9A21-6B892F0B3B59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: df10327b2ac12d2b119f1ab558d7f27e8c319507
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84131200"
---
# <a name="xamarinforms-dual-screen-triggers"></a>Триггеры двухэкранного режима в Xamarin.Forms

![](~/media/shared/preview.png "This API is currently pre-release")

Пространство имен [`Xamarin.Forms.DualScreen`](xref:Xamarin.Forms.DualScreen) содержит два триггера состояния:

- [`SpanModeStateTrigger`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger) активирует изменение [`VisualState`](xref:Xamarin.Forms.VisualState) при изменении режима просмотра присоединенного макета.
- `WindowSpanModeStateTrigger` активирует изменение [`VisualState`](xref:Xamarin.Forms.VisualState) при изменении режима просмотра окна.

Дополнительные сведения о триггерах состояния см. в разделе [Триггеры состояния](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers).

## <a name="span-mode-state-trigger"></a>Триггер состояния режима интервала

[`SpanModeStateTrigger`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger) активирует изменение [`VisualState`](xref:Xamarin.Forms.VisualState) при изменении режима интервала присоединенного макета. У этого триггера одно привязываемое свойство:

- [`SpanMode`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger.SpanMode) типа [`TwoPaneViewMode`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger.SpanMode) указывает режим интервала, к которому следует применять [`VisualState`](xref:Xamarin.Forms.VisualState).

> [!NOTE]
> [`SpanModeStateTrigger`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger) является производным от класса [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) и может присоединить обработчик событий к событию [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged).

В следующем примере XAML показан элемент [`Grid`](xref:Xamarin.Forms.Grid), предусматривающий объекты `SpanModeStateTrigger`:

```xaml
<Grid>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="GridSingle">
                <VisualState.StateTriggers>
                    <dualScreen:SpanModeStateTrigger SpanMode="SinglePane"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Green" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="GridWide">
                <VisualState.StateTriggers>
                    <dualScreen:SpanModeStateTrigger SpanMode="Wide" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Red" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="GridTall">
                <VisualState.StateTriggers>
                    <dualScreen:SpanModeStateTrigger SpanMode="Tall" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Purple" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
    ...
</Grid>
```

В этом примере визуальные состояния задаются в объекте [`Grid`](xref:Xamarin.Forms.Grid). Цвет фона `Grid` является зеленым, когда отображается только одна панель, красным, когда панели отображаются рядом друг с другом, и сиреневым, когда панели отображаются сверху вниз.

## <a name="window-span-mode-state-trigger"></a>Триггер состояния режима интервала окна

`WindowSpanModeStateTrigger` активирует изменение [`VisualState`](xref:Xamarin.Forms.VisualState) при изменении режима интервала окна. У этого триггера одно привязываемое свойство:

- [`SpanMode`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger.SpanMode) типа [`TwoPaneViewMode`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger.SpanMode) указывает режим интервала, к которому следует применять [`VisualState`](xref:Xamarin.Forms.VisualState).

> [!NOTE]
> `WindowSpanModeStateTrigger` является производным от класса [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) и может присоединить обработчик событий к событию [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged).

В следующем примере XAML показан элемент [`Grid`](xref:Xamarin.Forms.Grid), предусматривающий объекты `WindowSpanModeStateTrigger`:

```xaml
<Grid>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="NotSpanned">
                <VisualState.StateTriggers>
                    <dualScreen:WindowSpanModeStateTrigger SpanMode="SinglePane"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Red" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Spanned">
                <VisualState.StateTriggers>
                    <dualScreen:WindowSpanModeStateTrigger SpanMode="Wide" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Green" />
                </VisualState.Setters>
            </VisualState>
                <VisualState x:Name="Tall">
                    <VisualState.StateTriggers>
                        <dualScreen:WindowSpanModeStateTrigger SpanMode="Tall" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor" Value="Yellow" />
                    </VisualState.Setters>
                </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
    ...
</Grid>    
```

В этом примере визуальные состояния задаются в объекте [`Grid`](xref:Xamarin.Forms.Grid). Цвет фона `Grid` является красным, когда отображается только одна панель, зеленым, когда панели отображаются рядом друг с другом, и желтым, когда панели отображаются сверху вниз.

## <a name="related-links"></a>Связанные ссылки

- [Триггеры Xamarin.Forms](~/xamarin-forms/app-fundamentals/triggers.md)
- [Диспетчер визуального представления состояний Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md)
