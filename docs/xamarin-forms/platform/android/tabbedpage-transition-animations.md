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
ms.openlocfilehash: d3ae03ec6cbc3469422e6a2d57f186254e87f40c
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140027"
---
# <a name="tabbedpage-page-transition-animations-on-android"></a>Анимация перехода на страницу Таббедпаже в Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа для Android используется для отключения анимации переходов при переходе по страницам либо программно, либо при использовании панели вкладок в [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . Он используется в XAML путем установки `TabbedPage.IsSmoothScrollEnabled` Свойства BIND в значение `false` :

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.IsSmoothScrollEnabled="false">
    ...
</TabbedPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetIsSmoothScrollEnabled(false);
```

`TabbedPage.On<Android>`Метод указывает, что эта платформа будет запускаться только в Android. `TabbedPage.SetIsSmoothScrollEnabled`Метод в [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространстве имен используется для управления отображением анимаций переходов при переходе между страницами в [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . Кроме `TabbedPage` того, класс в `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` пространстве имен также имеет следующие методы:

- `IsSmoothScrollEnabled`, который используется для получения информации о том, будут ли отображаться анимированные анимации при переходе между страницами в `TabbedPage` .
- `EnableSmoothScroll`, который используется для включения анимации переходов при переходе между страницами в `TabbedPage` .
- `DisableSmoothScroll`, который используется для отключения анимации переходов при переходе между страницами в `TabbedPage` .

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
