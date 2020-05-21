---
title: Стиль представления модальной страницы в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS для установки стиля представления модальной страницы.
ms.prod: xamarin
ms.assetid: C791F7CF-330A-44BA-987A-4CFCCBB9278B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2020
ms.openlocfilehash: 19175a6d97afb9d2d985d8a1bf8e1ce4c0179242
ms.sourcegitcommit: 87035b654434c22ca1b0d70ee8d2496dcb63b365
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/18/2020
ms.locfileid: "83546510"
---
# <a name="modal-page-presentation-style-on-ios"></a>Стиль представления модальной страницы в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS предназначена для задания стиля представления модальной страницы, а также может использоваться для отображения модальных страниц с прозрачными фоновыми рисунками. Он используется в XAML путем установки `Page.ModalPresentationStyle` Свойства BIND в `UIModalPresentationStyle` значение перечисления:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.ModalPresentationStyle="OverFullScreen">
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
        On<iOS>().SetModalPresentationStyle(UIModalPresentationStyle.OverFullScreen);
        ...
    }
}
```

`Page.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `Page.SetModalPresentationStyle`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для установки модального стиля представления на, [`Page`](xref:Xamarin.Forms.Page) указывая одно из следующих `UIModalPresentationStyle` значений перечисления:

- `FullScreen`, который устанавливает модальный стиль представления, охватывающий весь экран. По умолчанию модальные страницы отображаются с использованием этого стиля презентации.
- `FormSheet`, который задает отображение модального стиля презентации по центру и меньше, чем экран.
- `Automatic`, который устанавливает в качестве модального стиля представления значение по умолчанию, выбранное системой. Для большинства контроллеров представления `UIKit` сопоставляется с `UIModalPresentationStyle.PageSheet` , но некоторые контроллеры системного представления могут сопоставлять его с другим стилем.
- `OverFullScreen`, который задает модальный стиль представления для покрытия экрана.
- `PageSheet`, который задает модальный стиль представления для покрытия базового содержимого.

Кроме того, `GetModalPresentationStyle` метод можно использовать для получения текущего значения `UIModalPresentationStyle` перечисления, применяемого к [`Page`](xref:Xamarin.Forms.Page) .

В результате можно установить модальный стиль представления для [`Page`](xref:Xamarin.Forms.Page) .

[![](page-presentation-style-images/modal-presentation-style-small.png "Modal Presentation Styles")](page-presentation-style-images/modal-presentation-style-large.png#lightbox "Modal Presentation Styles")

> [!NOTE]
> Страницы, использующие эту платформу для установки модального стиля представления, должны использовать модальную навигацию. Дополнительные сведения см. в разделе [модальные страницы Xamarin. Forms](~/xamarin-forms/app-fundamentals/navigation/modal.md).

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
