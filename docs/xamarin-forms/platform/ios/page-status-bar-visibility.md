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
ms.openlocfilehash: 69594924f26afff133d8f211199cac44e66254d9
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128032"
---
# <a name="page-status-bar-visibility-on-ios"></a>Видимость строки состояния страницы в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS предназначена для настройки видимости строки состояния в [`Page`](xref:Xamarin.Forms.Page) , а также позволяет управлять отображением или выходом строки состояния `Page` . Он используется в XAML путем задания `Page.PrefersStatusBarHidden` для присоединенного свойства значения `StatusBarHiddenMode` перечисления, а также при необходимости `Page.PreferredStatusBarUpdateAnimation` присоединенного свойства к значению `UIStatusBarAnimation` перечисления:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

`Page.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `Page.SetPrefersStatusBarHidden`Метод в `Xamarin.Forms.PlatformConfiguration.iOSSpecific` пространстве имен используется для установки видимости строки состояния в, [`Page`](xref:Xamarin.Forms.Page) задавая одно из `StatusBarHiddenMode` значений перечисления: `Default` , `True` или `False` . `StatusBarHiddenMode.True`Значения и `StatusBarHiddenMode.False` задают видимость строки состояния независимо от ориентации устройства, и `StatusBarHiddenMode.Default` значение скрывает строку состояния в вертикальной компактной среде.

В результате можно установить видимость строки состояния в [`Page`](xref:Xamarin.Forms.Page) .

![](page-status-bar-visibility-images/hide-status-bar.png "Status Bar Visibility Platform-Specific")

> [!NOTE]
> В [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) `StatusBarHiddenMode` значение указанного перечисления также будет обновлять строку состояния на всех дочерних страницах. Для всех других [`Page`](xref:Xamarin.Forms.Page) типов, производных от, указанное `StatusBarHiddenMode` значение перечисления будет обновлять строку состояния только на текущей странице.

`Page.SetPreferredStatusBarUpdateAnimation`Метод используется для задания того, как строка состояния вводит или оставляет элемент [`Page`](xref:Xamarin.Forms.Page) , указывая одно из `UIStatusBarAnimation` значений перечисления: `None` , `Fade` или `Slide` . Если `Fade` `Slide` указано значение перечисления или, то анимация 0,25 секунды выполняется, когда строка состояния переходит или оставляет `Page` .

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
