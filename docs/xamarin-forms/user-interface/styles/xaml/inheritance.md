---
Title: "наследование стилей в Xamarin.Forms " Description: "стили можно наследовать от других стилей для сокращения дублирования и включения повторного использования. В этой статье объясняется, как выполнить наследование стиля в Xamarin.Forms приложении. "
MS. произв. Xamarin MS. AssetID: 67A3A39C-8CC0-446D-8162-FFA73582D3B8 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч MS. Дата: 02/17/2016 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="style-inheritance-in-xamarinforms"></a>Наследование стилей вXamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_Стили могут наследоваться от других стилей для сокращения дублирования и включения повторного использования._

## <a name="style-inheritance-in-xaml"></a>Наследование стилей в XAML

Наследование стилей выполняется путем присвоения [`Style.BasedOn`](xref:Xamarin.Forms.Style.BasedOn) свойству существующего объекта [`Style`](xref:Xamarin.Forms.Style) . В XAML это достигается путем присвоения `BasedOn` свойству `StaticResource` расширения разметки, ссылающегося на ранее созданное `Style` . В C# это достигается путем присвоения `BasedOn` свойству `Style` экземпляра.

Стили, которые наследуются от базового стиля [`Setter`](xref:Xamarin.Forms.Setter) , могут включать экземпляры для новых свойств или использовать их для переопределения стилей из базового стиля. Кроме того, стили, которые наследуются от базового стиля, должны ориентироваться на тот же тип или тип, производный от типа, на который ссылается базовый стиль. Например, если базовый стиль предназначен для [`View`](xref:Xamarin.Forms.View) экземпляров, то стили, основанные на базовом стиле, могут ориентироваться на `View` экземпляры или типы, производные от `View` класса, [`Label`](xref:Xamarin.Forms.Label) например [`Button`](xref:Xamarin.Forms.Button) экземпляры и.

В следующем коде показано *явное* наследование стиля на странице XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
            </Style>
            <Style x:Key="labelStyle" TargetType="Label"
                   BasedOn="{StaticResource baseStyle}">
                ...
                <Setter Property="TextColor" Value="Teal" />
            </Style>
            <Style x:Key="buttonStyle" TargetType="Button"
                   BasedOn="{StaticResource baseStyle}">
                <Setter Property="BorderColor" Value="Lime" />
                ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelStyle}" />
            ...
            <Button Text="So is the button"
                    Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

`baseStyle`Целевые [`View`](xref:Xamarin.Forms.View) экземпляры и устанавливают [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) Свойства и. Параметр `baseStyle` не задается непосредственно в каких-либо элементах управления. Вместо этого `labelStyle` и `buttonStyle` наследуются от него, устанавливая дополнительные значения свойств, допускающих привязку. `labelStyle`И `buttonStyle` затем применяются к экземплярам [`Label`](xref:Xamarin.Forms.Label) и [`Button`](xref:Xamarin.Forms.Button) экземпляру путем задания их [`Style`](xref:Xamarin.Forms.NavigableElement.Style) свойств. Результат показан на следующих снимках экрана.

[![](inheritance-images/style-inheritance.png)](inheritance-images/style-inheritance-large.png#lightbox)

> [!NOTE]
> Неявный стиль может быть производным от явного стиля, но явный стиль не может быть производным от неявного стиля.

### <a name="respecting-the-inheritance-chain"></a>Уважение цепочки наследования

Стиль может наследоваться только от стилей на том же уровне или выше в иерархии представления. Это означает следующее.

- Ресурс уровня приложения может наследовать только от других ресурсов уровня приложения.
- Ресурс уровня страницы может наследовать от ресурсов уровня приложения и других ресурсов на уровне страницы.
- Ресурс уровня элемента управления может наследовать от ресурсов уровня приложения, ресурсов на уровне страницы и других ресурсов уровня управления.

Эта цепочка наследования показана в следующем примере кода:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelStyle" TargetType="Label" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                    <Style x:Key="buttonStyle" TargetType="Button" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

В этом примере `labelStyle` и `buttonStyle` являются ресурсами уровня управления, а `baseStyle` — ресурсом уровня страницы. Однако хотя `labelStyle` и `buttonStyle` наследуются от `baseStyle` , невозможно `baseStyle` наследовать от `labelStyle` или `buttonStyle` , из-за их соответствующих расположений в иерархии представлений.

## <a name="style-inheritance-in-c35"></a>Наследование стилей в C&#35;

Эквивалентная страница C#, где [`Style`](xref:Xamarin.Forms.Style) экземпляры присваиваются непосредственно [`Style`](xref:Xamarin.Forms.NavigableElement.Style) свойствам необходимых элементов управления, показана в следующем примере кода:

```csharp
public class StyleInheritancePageCS : ContentPage
{
    public StyleInheritancePageCS ()
    {
        var baseStyle = new Style (typeof(View)) {
            Setters = {
                new Setter {
                    Property = View.HorizontalOptionsProperty, Value = LayoutOptions.Center    },
                ...
            }
        };

        var labelStyle = new Style (typeof(Label)) {
            BasedOn = baseStyle,
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Teal    }
            }
        };

        var buttonStyle = new Style (typeof(Button)) {
            BasedOn = baseStyle,
            Setters = {
                new Setter { Property = Button.BorderColorProperty, Value =    Color.Lime },
                ...
            }
        };
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelStyle },
                ...
                new Button { Text = "So is the button", Style = buttonStyle }
            }
        };
    }
}
```

`baseStyle`Целевые [`View`](xref:Xamarin.Forms.View) экземпляры и устанавливают [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) Свойства и. Параметр `baseStyle` не задается непосредственно в каких-либо элементах управления. Вместо этого `labelStyle` и `buttonStyle` наследуются от него, устанавливая дополнительные значения свойств, допускающих привязку. `labelStyle`И `buttonStyle` затем применяются к экземплярам [`Label`](xref:Xamarin.Forms.Label) и [`Button`](xref:Xamarin.Forms.Button) экземпляру путем задания их [`Style`](xref:Xamarin.Forms.NavigableElement.Style) свойств.

## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Базовые стили (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [Работа со стилями (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
