---
title: Режим ввода с программируемой клавиатуры в Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать зависящую от платформы Android платформу, которая устанавливает режим работы для области ввода с мягкими клавиатурами.
ms.prod: xamarin
ms.assetid: AFCDC92F-F61E-42F6-904B-50B5C4949970
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0c7a379fa0128f73af471509974a043dbf2475d3
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86933319"
---
# <a name="soft-keyboard-input-mode-on-android"></a>Режим ввода с программируемой клавиатуры в Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа для Android предназначена для установки режима работы для области ввода с мягкими клавиатурами и используется в XAML путем задания [`Application.WindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty) для присоединенного свойства значения [`WindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) перечисления:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize">
  ...
</Application>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

App.Current.On<Android>().UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize);
```

`Application.On<Android>`Метод указывает, что эта платформа будет запускаться только в Android. [ `Application.UseWindowSoftInputModeAdjust` ] (Xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Application. Усевиндовсофтинпутмодеаджуст ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Приложение}, Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Виндовсофтинпутмодеаджуст)). в [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространстве имен используется для задания режима работы с экранной клавиатурой с возможностью ввода с помощью [`WindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) перечислений, предоставляющих два значения: [`Pan`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan) и [`Resize`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize) . Это `Pan` значение использует [`AdjustPan`](xref:Android.Views.SoftInput.AdjustPan) параметр корректировки, который не изменяет размер окна, если фокус ввода находится на элементе управления вводом. Вместо этого содержимое окна будет сдвигаемых, чтобы текущий фокус не скрывался экранной клавиатурой. Это `Resize` значение использует [`AdjustResize`](xref:Android.Views.SoftInput.AdjustResize) параметр корректировки, который изменяет размер окна, когда фокус ввода находится на элементе управления вводом, чтобы освободить место для экранной клавиатуры.

Результат заключается в том, что режим работы с экранной клавиатурой может быть установлен, когда фокус ввода находится на элементе управления вводом:

[![Горячая клавиатура режим операционной системы — зависит от платформы](soft-keyboard-input-mode-images/pan-resize.png)](soft-keyboard-input-mode-images/pan-resize-large.png#lightbox "Горячая клавиатура режим операционной системы — зависит от платформы")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
