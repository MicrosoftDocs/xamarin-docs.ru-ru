---
title: Таббедпаже полупрозрачная панель вкладок в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS для установки режима полупрозрачность панели вкладок в Таббедпаже.
ms.prod: xamarin
ms.assetid: 9581AE81-9557-47AD-8B07-25A1AC5F055B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f6431e2cbb187cbf89bdccad0ba5de57eed162e1
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93371330"
---
# <a name="tabbedpage-translucent-tab-bar-on-ios"></a>Таббедпаже полупрозрачная панель вкладок в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS используется для установки режима полупрозрачность для панели вкладок в [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . Он используется в XAML путем установки `TabbedPage.TranslucencyMode` Свойства BIND в `TranslucencyMode` значение перечисления:

```xaml
<TabbedPage ...
            xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
            ios:TabbedPage.TranslucencyMode="Opaque">
    ...
</TabbedPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetTranslucencyMode(TranslucencyMode.Opaque);
```

`TabbedPage.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `TabbedPage.SetTranslucencyMode`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для установки режима полупрозрачность панели вкладок на, [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) указывая одно из следующих `TranslucencyMode` значений перечисления:

- `Default`, который задает для панели вкладок режим полупрозрачность по умолчанию. Это значение по умолчанию для свойства `TabbedPage.TranslucencyMode`.
- `Translucent`, который устанавливает прозрачность панели вкладок.
- `Opaque`, который задает непрозрачность панели вкладок.

Кроме того, `GetTranslucencyMode` метод можно использовать для получения текущего значения `TranslucencyMode` перечисления, применяемого к [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) .

Результат заключается в том, что режим полупрозрачность панели вкладок в [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) может быть установлен следующим образом:

![Снимок экрана с полупрозрачными и непрозрачными панелями табуляции в iOS](tabbedpage-translucent-tabbar-images/translucencymodes.png "Полупрозрачные и непрозрачные панели вкладок")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)