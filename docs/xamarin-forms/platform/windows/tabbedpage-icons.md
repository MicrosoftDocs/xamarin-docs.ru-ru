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
ms.openlocfilehash: f6db5014050ad3f037869120d017e51803a7c48f
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136543"
---
# <a name="tabbedpage-icons-on-windows"></a>Значки Таббедпаже в Windows

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Этот универсальная платформа Windows, зависящий от платформы, позволяет отображать значки страниц на [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) панели инструментов и позволяет дополнительно указывать размер значка. Он используется в XAML путем присвоения [`TabbedPage.HeaderIconsEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsEnabledProperty) присоединенному свойству `true` значения, а также при необходимости установки [`TabbedPage.HeaderIconsSize`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsSizeProperty) присоединенного свойства в [`Size`](xref:Xamarin.Forms.Size) значение:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:TabbedPage.HeaderIconsEnabled="true">
    <windows:TabbedPage.HeaderIconsSize>
        <Size>
            <x:Arguments>
                <x:Double>24</x:Double>
                <x:Double>24</x:Double>
            </x:Arguments>
        </Size>
    </windows:TabbedPage.HeaderIconsSize>
    <ContentPage Title="Todo" IconImageSource="todo.png">
        ...
    </ContentPage>
    <ContentPage Title="Reminders" IconImageSource="reminders.png">
        ...
    </ContentPage>
    <ContentPage Title="Contacts" IconImageSource="contacts.png">
        ...
    </ContentPage>
</TabbedPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

public class WindowsTabbedPageIconsCS : Xamarin.Forms.TabbedPage
{
  public WindowsTabbedPageIconsCS()
  {
    On<Windows>().SetHeaderIconsEnabled(true);
    On<Windows>().SetHeaderIconsSize(new Size(24, 24));

    Children.Add(new ContentPage { Title = "Todo", IconImageSource = "todo.png" });
    Children.Add(new ContentPage { Title = "Reminders", IconImageSource = "reminders.png" });
    Children.Add(new ContentPage { Title = "Contacts", IconImageSource = "contacts.png" });
  }
}
```

`TabbedPage.On<Windows>`Метод указывает, что данная платформа будет запускаться только на универсальная платформа Windows. [ `TabbedPage.SetHeaderIconsEnabled` ] (Xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Таббедпаже. Сесеадериконсенаблед ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Таббедпаже}, System. Boolean)) в [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространстве имен используется для включения или отключения значков заголовка. [ `TabbedPage.SetHeaderIconsSize` ] (Xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Таббедпаже. Сесеадериконссизе ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Таббедпаже}, Xamarin.Forms . Size)) метод, при необходимости, задает размер значка заголовка со [`Size`](xref:Xamarin.Forms.Size) значением.

Кроме `TabbedPage` того, класс в `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` пространстве имен также имеет [`EnableHeaderIcons`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.EnableHeaderIcons*) метод, который включает значки заголовка, [`DisableHeaderIcons`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.DisableHeaderIcons*) метод, который отключает значки заголовка, и [`IsHeaderIconsEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.IsHeaderIconsEnabled*) метод, возвращающий `boolean` значение, которое указывает, включены ли значки заголовка.

В результате значки страницы могут отображаться на [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) панели инструментов с размером значка, который при необходимости имеет желаемый размер:

![Значки Таббедпаже, связанные с платформой](tabbedpage-icons-images/tabbedpage-icons.png "Значки Таббедпаже, связанные с платформой")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ВиндовсспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
