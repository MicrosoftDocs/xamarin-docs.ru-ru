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
ms.openlocfilehash: 005e8216b887b694b33916179ca276cf8091e006
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84135984"
---
# <a name="main-thread-control-updates-on-ios"></a>Основные обновления управления потоком в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS позволяет выполнять обновления элементов управления и отрисовки в основном потоке, а не выполнять в фоновом потоке. Он должен быть редко необходим, но в некоторых случаях может препятствовать сбоям. Его использование в XAML путем установки `Application.HandleControlUpdatesOnMainThread` Свойства BIND в значение `true` :

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.HandleControlUpdatesOnMainThread="true">
    ...
</Application>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetHandleControlUpdatesOnMainThread(true);
```

`Application.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `Application.SetHandleControlUpdatesOnMainThread`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для управления разметкой элементов управления и обновлениями отрисовки в основном потоке, а не для выполнения в фоновом потоке. Кроме того, `Application.GetHandleControlUpdatesOnMainThread` метод можно использовать для того, чтобы возвращать управление макетом элемента управления и обновлением отрисовки в основном потоке.

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
