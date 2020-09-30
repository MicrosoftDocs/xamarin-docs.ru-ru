---
title: Выбор элемента TimePicker в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать особенности платформы iOS, которые используются для управления выбором элементов в TimePicker.
ms.prod: xamarin
ms.assetid: 554AC877-8698-4B8C-A676-80DD64305A06
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 372c268c13c50719953ac63bcc43cc8d9bff4bdd
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91563722"
---
# <a name="timepicker-item-selection-on-ios"></a>Выбор элемента TimePicker в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Элементы управления для конкретных платформ iOS при выборе элементов в [`TimePicker`](xref:Xamarin.Forms.TimePicker) , позволяя пользователю указать, что выбор элементов выполняется при просмотре элементов в элементе управления, или только после нажатия кнопки **done (Готово** ). Он используется в XAML путем присвоения `TimePicker.UpdateMode` свойству присоединенного свойства значения `UpdateMode` перечисления:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
       <TimePicker Time="14:00:00"
                   ios:TimePicker.UpdateMode="WhenFinished" />
       ...
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

timePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`TimePicker.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `TimePicker.SetUpdateMode`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для управления тем, когда происходит выбор элементов, с `UpdateMode` перечислением, предоставляющим два возможных значения:

- `Immediately` — Выбор элементов происходит при просмотре пользователем элементов в [`TimePicker`](xref:Xamarin.Forms.TimePicker) . Это поведение по умолчанию в Xamarin.Forms .
- `WhenFinished` — Выбор элементов происходит только после нажатия пользователем кнопки **done (Готово** ) в [`TimePicker`](xref:Xamarin.Forms.TimePicker) .

Кроме того, `SetUpdateMode` метод можно использовать для переключения значений перечисления путем вызова `UpdateMode` метода, который возвращает текущий объект `UpdateMode` :

```csharp
switch (timePicker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        timePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        timePicker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

В результате заданный объект `UpdateMode` применяется к элементу [`TimePicker`](xref:Xamarin.Forms.TimePicker) управления, который управляет тем, когда происходит выбор элементов:

[![Снимок экрана: режимы обновления TimePicker](timepicker-selection-images/timepicker-updatemode.png "TimePicker UpdateMode для конкретной платформы")](timepicker-selection-images/timepicker-updatemode-large.png#lightbox "TimePicker UpdateMode для конкретной платформы")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)