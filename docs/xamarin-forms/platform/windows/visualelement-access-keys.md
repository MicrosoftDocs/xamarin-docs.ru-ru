---
title: Ключи доступа Висуалелемент в Windows
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Windows, которая задает ключ доступа для Висуалелемент.
ms.prod: xamarin
ms.assetid: 771AF785-76B8-4372-89F5-E4D521D21E0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3cd4a9078a22c1f002cbc414490455c716ba884a
ms.sourcegitcommit: 342cfbd2502ad92cadada4fa9aec669b99d7830a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2020
ms.locfileid: "96604590"
---
# <a name="visualelement-access-keys-on-windows"></a>Ключи доступа Висуалелемент в Windows

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Ключи доступа — это сочетания клавиш, которые улучшают удобство использования и доступность приложений на универсальная платформа Windows (UWP), предоставляя интуитивно понятный способ быстрого перехода к видимому пользовательскому интерфейсу приложения и взаимодействия с ним с помощью клавиатуры, а не с помощью сенсорного ввода или мыши. Они являются сочетаниями клавиши ALT и одним или несколькими буквенно-цифровыми клавишами, которые обычно направляются последовательно. Сочетания клавиш автоматически поддерживаются для ключей доступа, использующих один алфавитно-цифровой символ.

Советы по использованию ключей доступа — это плавающие значки, отображаемые рядом с элементами управления, которые содержат ключи доступа. Каждая подсказка ключа доступа содержит буквенно-цифровые ключи, которые активируют связанный элемент управления. Когда пользователь нажимает клавишу Alt, отображаются подсказки клавиш доступа.

Эта платформа, относящаяся к платформе UWP, используется для указания ключа доступа для [`VisualElement`](xref:Xamarin.Forms.VisualElement) . Он используется в XAML путем присвоения [`VisualElement.AccessKey`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyProperty) присоединенному свойству буквенно-цифрового значения и при необходимости установки [`VisualElement.AccessKeyPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyPlacementProperty) присоединенного свойства в значение [`AccessKeyPlacement`](xref:Xamarin.Forms.AccessKeyPlacement) перечисления, [`VisualElement.AccessKeyHorizontalOffset`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyHorizontalOffsetProperty) присоединенное свойство к свойству `double` , а [`VisualElement.AccessKeyVerticalOffset`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyVerticalOffsetProperty) присоединенное свойство `double` — к:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <ContentPage Title="Page 1"
                 windows:VisualElement.AccessKey="1">
        <StackLayout Margin="20">
            ...
            <Switch windows:VisualElement.AccessKey="A" />
            <Entry Placeholder="Enter text here"
                   windows:VisualElement.AccessKey="B" />
            ...
            <Button Text="Access key F, placement top with offsets"
                    Margin="20"
                    Clicked="OnButtonClicked"
                    windows:VisualElement.AccessKey="F"
                    windows:VisualElement.AccessKeyPlacement="Top"
                    windows:VisualElement.AccessKeyHorizontalOffset="20"
                    windows:VisualElement.AccessKeyVerticalOffset="20" />
            ...
        </StackLayout>
    </ContentPage>
    ...
</TabbedPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var page = new ContentPage { Title = "Page 1" };
page.On<Windows>().SetAccessKey("1");

var switchView = new Switch();
switchView.On<Windows>().SetAccessKey("A");
var entry = new Entry { Placeholder = "Enter text here" };
entry.On<Windows>().SetAccessKey("B");
...

var button4 = new Button { Text = "Access key F, placement top with offsets", Margin = new Thickness(20) };
button4.Clicked += OnButtonClicked;
button4.On<Windows>()
    .SetAccessKey("F")
    .SetAccessKeyPlacement(AccessKeyPlacement.Top)
    .SetAccessKeyHorizontalOffset(20)
    .SetAccessKeyVerticalOffset(20);
...
```

`VisualElement.On<Windows>`Метод указывает, что данная платформа будет запускаться только на универсальная платформа Windows. [ `VisualElement.SetAccessKey` ] (Xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Висуалелемент. Сетакцесскэй ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Висуалелемент}, System. String)) в [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространстве имен используется для задания значения ключа доступа для `VisualElement` . [ `VisualElement.SetAccessKeyPlacement` ] (Xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Висуалелемент. Сетакцесскэйплацемент ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Висуалелемент}, Xamarin.Forms . Акцесскэйплацемент)) метод, при необходимости определяет, какую точку следует использовать для отображения подсказки ключа доступа, с [`AccessKeyPlacement`](xref:Xamarin.Forms.AccessKeyPlacement) перечислением, которое предоставляет следующие возможные значения:

- [`Auto`](xref:Xamarin.Forms.AccessKeyPlacement.Auto) — Указывает, что размещение подсказки ключа доступа будет определяться операционной системой.
- [`Top`](xref:Xamarin.Forms.AccessKeyPlacement.Top) — Указывает, что подсказка ключа доступа будет отображаться над верхней границей `VisualElement` .
- [`Bottom`](xref:Xamarin.Forms.AccessKeyPlacement.Bottom) — Указывает, что подсказка ключа доступа будет отображаться под нижней границей `VisualElement` .
- [`Right`](xref:Xamarin.Forms.AccessKeyPlacement.Right) — Указывает, что подсказка ключа доступа будет отображаться справа от правого края `VisualElement` .
- [`Left`](xref:Xamarin.Forms.AccessKeyPlacement.Left) — Указывает, что подсказка ключа доступа будет отображаться слева от левого края `VisualElement` .
- [`Center`](xref:Xamarin.Forms.AccessKeyPlacement.Center) — Указывает, что подсказка ключа доступа будет отмечена в центре `VisualElement` .

> [!NOTE]
> Как правило, [`Auto`](xref:Xamarin.Forms.AccessKeyPlacement.Auto) достаточно разместить ключевую подсказку, которая включает поддержку адаптивных пользовательских интерфейсов.

[ `VisualElement.SetAccessKeyHorizontalOffset` ] (Xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Висуалелемент. Сетакцесскэйхоризонталоффсет ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Висуалелемент}, System. Double) и [ `VisualElement.SetAccessKeyVerticalOffset` ] (xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Висуалелемент. Сетакцесскэйвертикалоффсет ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Висуалелемент}, System. Double)). можно использовать методы для более детального управления расположением подсказок по клавишам доступа. Аргумент `SetAccessKeyHorizontalOffset` метода указывает, насколько далеко переместить клавишу доступа влево или вправо, а аргумент `SetAccessKeyVerticalOffset` метода указывает, насколько далеко переместить подсказку клавиши доступа вверх или вниз.

>[!NOTE]
> Невозможно задать смещения для ключа доступа, если задано размещение ключа доступа `Auto` .

Кроме того, [ `GetAccessKey` ] (xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Висуалелемент. Жетакцесскэй ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Висуалелемент})), [ `GetAccessKeyPlacement` ] (xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Висуалелемент. Жетакцесскэйплацемент ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Висуалелемент})), [ `GetAccessKeyHorizontalOffset` ] (xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Висуалелемент. Жетакцесскэйхоризонталоффсет ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Висуалелемент})) и [ `GetAccessKeyVerticalOffset` ] (xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Висуалелемент. Жетакцесскэйвертикалоффсет ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Висуалелемент})). можно использовать методы для получения значения ключа доступа и его расположения.

Результат заключается в том, что советы по использованию ключа доступа можно отобразить рядом с любыми [`VisualElement`](xref:Xamarin.Forms.VisualElement) экземплярами, которые определяют ключи доступа, нажав клавишу Alt:

![Ключи доступа Висуалелемент, зависящие от платформы](visualelement-access-keys-images/visualelement-accesskeys.png "Ключи доступа Висуалелемент, зависящие от платформы")

Когда пользователь активирует клавишу доступа, нажимая клавишу Alt, а затем клавишу доступа, действие по умолчанию для элемента `VisualElement` будет выполнено. Например, когда пользователь активирует клавишу доступа на [`Switch`](xref:Xamarin.Forms.Switch) , `Switch` он переключается. Когда пользователь активирует клавишу доступа на [`Entry`](xref:Xamarin.Forms.Entry) , `Entry` фокус прибыли. Когда пользователь активирует ключ доступа в [`Button`](xref:Xamarin.Forms.Button) , выполняется обработчик события для [`Clicked`](xref:Xamarin.Forms.Button.Clicked) события.

> [!WARNING]
> По умолчанию ключи доступа могут быть активированы при отображении модального диалогового окна, например `DisplayAlert` `DisplayPromptAsync` методами и. Однако пользовательская логика может быть написана для отключения ключей доступа в этом сценарии. Это можно сделать, обрабатывая `Dispatcher.AcceleratorKeyActivated` событие в `MainPage` классе проекта UWP, и в обработчике событий задавая `Handled` свойству аргументов события значение `true` при отображении модального диалогового окна.

Дополнительные сведения о ключах доступа см. в разделе [ключи доступа](/windows/uwp/design/input/access-keys).

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ВиндовсспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
