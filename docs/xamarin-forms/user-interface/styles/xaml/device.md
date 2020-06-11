---
Title: "стили устройств в Xamarin.Forms описании:" Xamarin.Forms включает шесть динамических стилей, известных как стили устройств, в классе Device. styles. В этой статье объясняется, как использовать стили устройств в Xamarin.Forms приложении. "
MS. произв. Xamarin MS. AssetID: 7FF19ED1-0822-4238-9435-AD970317A2F8 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 02/17/2016 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="device-styles-in-xamarinforms"></a>Стили устройств вXamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)

_Xamarin. Forms включает в себя шесть динамических стилей, известных как стили устройств, в классе Device. styles._

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

![](device-images/device-styles.png "Device Styles on Each Platform")

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

## <a name="accessibility"></a>Специальные возможности

Стили *устройств* учитывают настройки специальных возможностей, поэтому размеры шрифтов изменятся по мере изменения настроек специальных возможностей на каждой платформе. Поэтому для поддержки текста с поддержкой специальных возможностей убедитесь, что стили *устройств* используются в качестве базиса для любых текстовых стилей в приложении.

На следующих снимках экрана показаны стили устройств на каждой платформе с наименьшим доступным размером шрифта:

[![](device-images/minimum-size.png "Accessible Small Device Styles on Each Platform")](device-images/minimum-size-large.png#lightbox "Accessible Small Device Styles on Each Platform")

На следующих снимках экрана показаны стили устройств на каждой платформе с максимальным доступным размером шрифта:

![](device-images/maximum-size.png "Accessible Large Device Styles on Each Platform")

## <a name="related-links"></a>Связанные ссылки

- [Стили текста](~/xamarin-forms/user-interface/text/styles.md)
- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Динамические стили (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)
- [Работа со стилями (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [Device. Styles](xref:Xamarin.Forms.Device.Styles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
