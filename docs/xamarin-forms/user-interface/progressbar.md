---
title: Xamarin.FormsProgressBar
description: Xamarin.FormsProgressBar — это элемент управления, который визуально представляет ход выполнения как горизонтальную линию, заполняемую на основе свойства float.
ms.prod: ''
ms.assetId: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b4ac6231c0483c0c44755c2ac9539f237dd64251
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136283"
---
# <a name="xamarinforms-progressbar"></a>Xamarin.FormsProgressBar
[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)

Xamarin.Forms [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) Элемент управления визуально представляет ход выполнения в виде горизонтальной линии, заполненной в процентах, представленном `float` значением. `ProgressBar`Класс наследует от [`View`](xref:Xamarin.Forms.View) .

На снимках экрана ниже показана страница `ProgressBar` в iOS и Android.

![Снимок экрана элемента ProgressBar в iOS и Android](progressbar-images/progressbars-default.png "ProgressBar в iOS и Android")

`ProgressBar`Элемент управления определяет два свойства:

* [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress)`float`значение, представляющее текущий ход выполнения в виде значения от 0 до 1. `Progress`значения меньше 0 будут относиться к 0, а значения больше 1 будут относиться к 1.
* [`ProgressColor`](xref:Xamarin.Forms.ProgressBar.ProgressColor)значение типа `Color` , которое влияет на цвет внутренней полосы, представляющей текущий ход выполнения.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что `ProgressBar` можно использовать стиль и цель привязок данных.

`ProgressBar`Элемент управления также определяет `ProgressTo` метод, который анимирует отрезок от текущего значения до указанного значения. Дополнительные сведения см. [в разделе Анимация элемента ProgressBar](#animate-a-progressbar).

> [!NOTE]
> Объект не `ProgressBar` принимает манипуляции с пользователем, поэтому он пропускается при использовании клавиши TAB для выбора элементов управления.

## <a name="create-a-progressbar"></a>Создание элемента ProgressBar

`ProgressBar`Экземпляр можно создать в XAML. Его `Progress` свойство определяет процент заполнения внутренней цветной полосы. Значение свойства по умолчанию `Progress` — 0. В следующем примере показано, как создать экземпляр `ProgressBar` в XAML с помощью необязательного `Progress` набора свойств:

```xaml
<ProgressBar Progress="0.5" />
```

`ProgressBar`Также можно создать в коде:

```csharp
ProgressBar progressBar = new ProgressBar { Progress = 0.5f };
```

> [!WARNING]
> Не используйте неограниченные горизонтальные параметры макета, такие как `Center` , `Start` или `End` с `ProgressBar` . В UWP объект `ProgressBar` сворачивается в полоску нулевой ширины. `HorizontalOptions`Не используйте значение по умолчанию `Fill` и `Auto` При размещении `ProgressBar` в макете не следует использовать ширину `Grid` .

## <a name="progressbar-appearance-properties"></a>Свойства внешнего вида ProgressBar

`ProgressColor`Свойство определяет цвет внутреннего столбца, если `Progress` свойство больше нуля. В следующем примере показано, как создать экземпляр `ProgressBar` в XAML с помощью `ProgressColor` набора свойств:

```xaml
<ProgressBar ProgressColor="Orange" />
```

Это `ProgressColor` свойство также может быть задано при создании `ProgressBar` в коде:

```csharp
ProgressBar progressBar = new ProgressBar { ProgressColor = Color.Orange };
```

На следующих снимках экрана показано, `ProgressBar` `ProgressColor` для чего свойство имеет значение `Color.Orange` в iOS и Android:

![Снимок экрана с стилем ProgressBar в iOS и Android](progressbar-images/progressbars-styled.png "Стили ProgressBar в iOS и Android")

## <a name="animate-a-progressbar"></a>Анимация элемента ProgressBar

`ProgressTo`Метод выполняет анимацию `ProgressBar` от текущего `Progress` значения до указанного значения с течением времени. Метод принимает `float` значение хода выполнения, `uint` продолжительность в миллисекундах, `Easing` значение перечисления и возвращает `Task<bool>` . В следующем коде показано, как анимировать `ProgressBar` :

```csharp
// animate to 75% progress over 500 milliseconds with linear easing
await progressBar.ProgressTo(0.75, 500, Easing.Linear);
```

Дополнительные сведения о `Easing` перечислении см. [в разделе ускорение Xamarin.Forms функций в ](~/xamarin-forms/user-interface/animation/easing.md).

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации ProgressBar](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)
