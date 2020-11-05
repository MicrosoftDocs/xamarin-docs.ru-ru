---
title: Стили устройств в Xamarin.Forms
description: Xamarin.Forms включает в себя шесть динамических стилей, известных как стили устройств, в классе Device. styles. В этой статье объясняется, как использовать стили устройств в Xamarin.Forms приложении.
ms.prod: xamarin
ms.assetid: 7FF19ED1-0822-4238-9435-AD970317A2F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5fdac3524e2213e43e1fad2ed3da9e12d7608b16
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374931"
---
# <a name="device-styles-in-no-locxamarinforms"></a>Стили устройств в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)

_Xamarin.Forms включает в себя шесть динамических стилей, известных как стили устройств, в классе Device. styles._

Стили *устройств* :

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)

Все шесть стилей могут применяться только к [`Label`](xref:Xamarin.Forms.Label) экземплярам. Например, для `Label` свойства, в котором отображается текст абзаца, может быть задано значение [`Style`](xref:Xamarin.Forms.NavigableElement.Style) [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle) .

В следующем примере кода показано использование стилей *устройств* на странице XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DeviceStylesPage" Title="Device" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="myBodyStyle" TargetType="Label"
              BaseResourceKey="BodyStyle">
                <Setter Property="TextColor" Value="Accent" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="Title style"
              Style="{DynamicResource TitleStyle}" />
            <Label Text="Subtitle text style"
              Style="{DynamicResource SubtitleStyle}" />
            <Label Text="Body style"
              Style="{DynamicResource BodyStyle}" />
            <Label Text="Caption style"
              Style="{DynamicResource CaptionStyle}" />
            <Label Text="List item detail text style"
              Style="{DynamicResource ListItemDetailTextStyle}" />
            <Label Text="List item text style"
              Style="{DynamicResource ListItemTextStyle}" />
            <Label Text="No style" />
            <Label Text="My body style"
              Style="{StaticResource myBodyStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Стили устройств привязаны к с помощью `DynamicResource` расширения разметки. Динамическую природу стилей можно увидеть в iOS, изменив параметры **специальных возможностей** для размера текста. Внешний вид стилей *устройств* на каждой платформе различается, как показано на следующих снимках экрана:

![Стили устройств на каждой платформе](device-images/device-styles.png)

Стили *устройств* также могут быть производными от, путем присвоения [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) свойству имени ключа для стиля устройства. В приведенном выше примере кода `myBodyStyle` наследуется от [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle) и задает цвет диакритических знаков. Дополнительные сведения о наследовании динамического стиля см. в разделе [динамическое наследование стиля](~/xamarin-forms/user-interface/styles/xaml/dynamic.md#dynamic-style-inheritance).

В следующем примере кода показана эквивалентная страница в C#:

```csharp
public class DeviceStylesPageCS : ContentPage
{
    public DeviceStylesPageCS ()
    {
        var myBodyStyle = new Style (typeof(Label)) {
            BaseResourceKey = Device.Styles.BodyStyleKey,
            Setters = {
                new Setter {
                    Property = Label.TextColorProperty,
                    Value = Color.Accent
                }
            }
        };

        Title = "Device";
        IconImageSource = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label { Text = "Title style", Style = Device.Styles.TitleStyle },
                new Label { Text = "Subtitle style", Style = Device.Styles.SubtitleStyle },
                new Label { Text = "Body style", Style = Device.Styles.BodyStyle },
                new Label { Text = "Caption style", Style = Device.Styles.CaptionStyle },
                new Label { Text = "List item detail text style",
                  Style = Device.Styles.ListItemDetailTextStyle },
                new Label { Text = "List item text style", Style = Device.Styles.ListItemTextStyle },
                new Label { Text = "No style" },
                new Label { Text = "My body style", Style = myBodyStyle }
            }
        };
    }
}
```

[`Style`](xref:Xamarin.Forms.NavigableElement.Style)Свойству каждого [`Label`](xref:Xamarin.Forms.Label) экземпляра присваивается соответствующее свойство из [`Devices.Styles`](xref:Xamarin.Forms.Device.Styles) класса.

## <a name="accessibility"></a>Возможности доступа

Стили *устройств* учитывают настройки специальных возможностей, поэтому размеры шрифтов изменятся по мере изменения настроек специальных возможностей на каждой платформе. Поэтому для поддержки текста с поддержкой специальных возможностей убедитесь, что стили *устройств* используются в качестве базиса для любых текстовых стилей в приложении.

На следующих снимках экрана показаны стили устройств на каждой платформе с наименьшим доступным размером шрифта:

[![Доступные небольшие стили устройств на каждой платформе](device-images/minimum-size.png)](device-images/minimum-size-large.png#lightbox "Доступные небольшие стили устройств на каждой платформе")

На следующих снимках экрана показаны стили устройств на каждой платформе с максимальным доступным размером шрифта:

![Доступные стили больших устройств на каждой платформе](device-images/maximum-size.png)

## <a name="related-links"></a>Связанные ссылки

- [Стили текста](~/xamarin-forms/user-interface/text/styles.md)
- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Динамические стили (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)
- [Работа со стилями (пример)](/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [Device. Styles](xref:Xamarin.Forms.Device.Styles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)