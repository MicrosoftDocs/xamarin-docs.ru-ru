---
title: Тень Мастердетаилпаже на iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая определяет, применена ли к странице сведений Мастердетаилпаже теневая копия при раскрытии главной страницы.
ms.prod: xamarin
ms.assetid: FB907EA2-C00A-43A5-84B1-A43584C867A5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fc6185a303e8fd7bdf68903d3d7bacc646cde601
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93372422"
---
# <a name="masterdetailpage-shadow-on-ios"></a>Тень Мастердетаилпаже на iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа управляет тем [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) , применяется ли к ней теневая страница сведений при раскрытии главной страницы. Он используется в XAML путем установки `MasterDetailPage.ApplyShadow` Свойства BIND в значение `true` :

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                  ios:MasterDetailPage.ApplyShadow="true">
    ...
</MasterDetailPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSMasterDetailPageCS : MasterDetailPage
{
    public iOSMasterDetailPageCS(ICommand restore)
    {
        On<iOS>().SetApplyShadow(true);
        // ...
    }
}
```

`MasterDetailPage.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `MasterDetailPage.SetApplyShadow`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для управления тем, применена ли к странице детализации к [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) ней теневая страница при раскрытии главной страницы. Кроме того, `GetApplyShadow` метод можно использовать для определения того, применяется ли тень к странице сведений `MasterDetailPage` .

В результате на страницу детализации [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) может быть применена теневая копия при раскрытии главной страницы:

[![Снимок экрана Мастердетаилпаже с тенью и без нее](masterdetailpage-shadow-images/shadow.png "Мастердетаилпаже с тенью и без нее")](masterdetailpage-shadow-images/shadow-large.png#lightbox "Мастердетаилпаже с тенью и без нее")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)