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
ms.openlocfilehash: f5f0bcdc2d2c8eb1b51ad8dcd1014c649af80c90
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137765"
---
# <a name="inputview-reading-order-on-windows"></a>Инпутвиев порядок чтения в Windows

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта универсальная платформа Windows зависит от конкретной платформы, а также от того, в каком экземпляре, и в каком-то конкретном [`Entry`](xref:Xamarin.Forms.Entry) случае [`Editor`](xref:Xamarin.Forms.Editor) [`Label`](xref:Xamarin.Forms.Label) будет определяться динамический текст (слева направо или справа налево). Он используется в XAML путем установки [`InputView.DetectReadingOrderFromContent`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) значения (для `Entry` `Editor` экземпляров и) или [`Label.DetectReadingOrderFromContent`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) присоединенного свойства к `boolean` значению:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

`Editor.On<Windows>`Метод указывает, что данная платформа будет запускаться только на универсальная платформа Windows. [ `InputView.SetDetectReadingOrderFromContent` ] (Xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Инпутвиев. Сетдетектреадингордерфромконтент ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Инпутвиев}, System. Boolean)) в [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространстве имен используется для управления тем, был ли обнаружен порядок чтения из содержимого в [`InputView`](xref:Xamarin.Forms.InputView) . Кроме того, `InputView.SetDetectReadingOrderFromContent` метод можно использовать для переключения между обнаружением порядка чтения из содержимого путем вызова метода [ `InputView.GetDetectReadingOrderFromContent` ] (xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Инпутвиев. Жетдетектреадингордерфромконтент ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Инпутвиев})) метод, который возвращает текущее значение:

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

В результате [`Entry`](xref:Xamarin.Forms.Entry) [`Editor`](xref:Xamarin.Forms.Editor) экземпляры, и [`Label`](xref:Xamarin.Forms.Label) могут динамически определять порядок чтения содержимого:

[![Инпутвиев обнаружение порядка чтения из платформы содержимого](inputview-reading-order-images/editor-readingorder.png "Инпутвиев обнаружение порядка чтения из платформы содержимого")](inputview-reading-order-images/editor-readingorder-large.png#lightbox "Инпутвиев обнаружение порядка чтения из платформы содержимого")

> [!NOTE]
> В отличие от установки [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) свойства, логика представлений, которые определяют порядок чтения из их текстового содержимого, не влияет на выравнивание текста в представлении. Вместо этого он корректирует порядок расположения блоков двунаправленного текста.

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ВиндовсспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
