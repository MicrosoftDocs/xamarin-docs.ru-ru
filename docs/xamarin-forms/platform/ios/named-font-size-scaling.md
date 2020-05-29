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
ms.openlocfilehash: 9aa29ba4d8e6bda9126ec211af1b4febebd5bab9
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128275"
---
# <a name="accessibility-scaling-for-named-font-sizes-on-ios"></a>Масштабирование специальных возможностей для именованных размеров шрифтов в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS отключает масштабирование доступа для именованных размеров шрифтов. Он используется в XAML путем установки `Application.EnableAccessibilityScalingForNamedFontSizes` Свойства BIND в значение `false` :

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.EnableAccessibilityScalingForNamedFontSizes="false">
    ...
</Application>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetEnableAccessibilityScalingForNamedFontSizes(false);
```

`Application.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `Application.SetEnableAccessibilityScalingForNamedFontSizes`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для отключения именованных размеров шрифтов, масштабируемых с помощью параметров специальных возможностей iOS. Кроме того, `Application.GetEnableAccessibilityScalingForNamedFontSizes` метод можно использовать для того, чтобы получить возможность масштабирования именованных размеров шрифтов с помощью параметров специальных возможностей iOS.

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
