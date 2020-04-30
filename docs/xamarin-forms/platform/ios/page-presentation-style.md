---
title: Стиль представления модальной страницы в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать специфические для платформы iOS стили представления модальной страницы.
ms.prod: xamarin
ms.assetid: C791F7CF-330A-44BA-987A-4CFCCBB9278B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2020
ms.openlocfilehash: 5078b280499929e0e2e3691539cf1927b4c79fe7
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517542"
---
# <a name="modal-page-presentation-style-on-ios"></a>Стиль представления модальной страницы в iOS

[![Скачать пример](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS используется для задания стиля представления модальной страницы. Он используется в XAML путем установки свойства `Page.ModalPresentationStyle` BIND в значение `UIModalPresentationStyle` перечисления:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.ModalPresentationStyle="FormSheet">
    ...
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSModalFormSheetPageCS : ContentPage
{
    public iOSModalFormSheetPageCS()
    {
        On<iOS>().SetModalPresentationStyle(UIModalPresentationStyle.FormSheet);
        ...
    }
}
```

`Page.On<iOS>` Метод указывает, что эта платформа будет запускаться только в iOS. `Page.SetModalPresentationStyle` Метод [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) в пространстве имен используется для установки модального стиля [`Page`](xref:Xamarin.Forms.Page) представления на, указывая одно из следующих `UIModalPresentationStyle` значений перечисления:

- `FullScreen`, который устанавливает модальный стиль представления, охватывающий весь экран. По умолчанию модальные страницы отображаются с использованием этого стиля презентации.
- `FormSheet`, который задает отображение модального стиля презентации по центру и меньше, чем экран.
- `Automatic`, который устанавливает в качестве модального стиля представления значение по умолчанию, выбранное системой. Для большинства контроллеров представления `UIKit` сопоставляется с `UIModalPresentationStyle.PageSheet`, но некоторые контроллеры системного представления могут сопоставлять его с другим стилем.
- `OverFullScreen`, который задает модальный стиль представления для покрытия экрана.
- `PageSheet`, который задает модальный стиль представления для покрытия базового содержимого.

Кроме того, `GetModalPresentationStyle` метод можно использовать для получения текущего значения `UIModalPresentationStyle` перечисления, применяемого к. [`Page`](xref:Xamarin.Forms.Page)

В результате [`Page`](xref:Xamarin.Forms.Page) можно установить модальный стиль представления для.

[![](page-presentation-style-images/modal-presentation-style-small.png "Modal Presentation Styles")](page-presentation-style-images/modal-presentation-style-large.png#lightbox "Modal Presentation Styles")

> [!NOTE]
> Страницы, использующие эту платформу для установки модального стиля представления, должны использовать модальную навигацию. Дополнительные сведения см. в разделе [модальные страницы Xamarin. Forms](~/xamarin-forms/app-fundamentals/navigation/modal.md).

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
