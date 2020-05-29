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
ms.openlocfilehash: d8bd539c2bb0e8963afae3392b6f8e99d79af9af
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136973"
---
# <a name="visualelement-first-responder-on-ios"></a>Первый ответчик Висуалелемент в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS позволяет [`VisualElement`](xref:Xamarin.Forms.VisualElement) объекту стать первым респондентом для сенсорных событий, а не со страницы, содержащей элемент. Он используется в XAML путем установки `VisualElement.CanBecomeFirstResponder` Свойства BIND в значение `true` :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Entry Placeholder="Enter text" />
        <Button ios:VisualElement.CanBecomeFirstResponder="True"
                Text="OK" />
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Entry entry = new Entry { Placeholder = "Enter text" };
Button button = new Button { Text = "OK" };
button.On<iOS>().SetCanBecomeFirstResponder(true);
```

`VisualElement.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `VisualElement.SetCanBecomeFirstResponder`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для того, чтобы сделать его `VisualElement` первым ответчиком для событий касания. Кроме того, `VisualElement.CanBecomeFirstResponder` метод можно использовать, чтобы вернуть `VisualElement` первый ответчик к событиям касания.

Результатом является то, что [`VisualElement`](xref:Xamarin.Forms.VisualElement) может стать первым респондентом для событий касания, а не страницей, содержащей элемент. Это позволяет сценариям, таким как приложения разговора, не отменять клавиатуру при [`Button`](xref:Xamarin.Forms.Button) касании.

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
