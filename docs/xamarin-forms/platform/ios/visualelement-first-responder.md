---
title: Первый ответчик Висуалелемент в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая позволяет объекту Висуалелемент стать первым отвечающим на события касания.
ms.prod: xamarin
ms.assetid: 3A77BA02-B87A-44EC-AC51-9D3130EF314C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ebe153ab8dfe4645b6c71fc6b973a6f001f97c0d
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91564073"
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

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)