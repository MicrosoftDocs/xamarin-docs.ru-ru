---
title: Анимация перехода на страницу Таббедпаже в Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Android, которая отключает анимацию переходов при переходе по страницам в Таббедпаже.
ms.prod: xamarin
ms.assetid: 2DB4EA6D-9CED-4137-BAB2-B20A457B1CA3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3a7fafcd154dc30df5d33f5728cf8fd8ae592de3
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562448"
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

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)