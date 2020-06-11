---
Title: "явные стили в Xamarin.Forms описании:" явный стиль — это тот, который выборочно применяется к элементам управления путем установки их свойств стиля. В этой статье объясняется, как использовать явные стили в Xamarin.Forms приложении ".
MS. произв. Xamarin MS. AssetID: C0DF9F8F-B431-4374-A574-325BC3C41A3B MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 02/17/2016 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="explicit-styles-in-xamarinforms"></a>Явные стили вXamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_Явный стиль — это тот, который выборочно применяется к элементам управления путем установки их свойств стиля._

## <a name="create-an-explicit-style-in-xaml"></a>Создание явного стиля в XAML

Чтобы объявить объект на [`Style`](xref:Xamarin.Forms.Style) уровне страницы, [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) необходимо добавить на страницу, а затем `Style` в объект включить одно или несколько объявлений `ResourceDictionary` . Объект создается `Style` *явным образом* путем присвоения `x:Key` атрибуту объявления атрибута, который предоставляет его описательный ключ в `ResourceDictionary` . *Явные* стили должны быть применены к конкретным визуальным элементам путем задания их [`Style`](xref:Xamarin.Forms.NavigableElement.Style) свойств.

В следующем примере кода показаны *явные* стили, объявленные в XAML на странице `ResourceDictionary` и применяемые к [`Label`](xref:Xamarin.Forms.Label) экземплярам страницы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="labelRedStyle" TargetType="Label">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
            <Style x:Key="labelGreenStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Green" />
            </Style>
            <Style x:Key="labelBlueStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelRedStyle}" />
            <Label Text="are demonstrating"
                   Style="{StaticResource labelGreenStyle}" />
            <Label Text="explicit styles,"
                   Style="{StaticResource labelBlueStyle}" />
            <Label Text="and an explicit style override"
                   Style="{StaticResource labelBlueStyle}"
                   TextColor="Teal" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)Определяет три *явных* стиля, которые применяются к [`Label`](xref:Xamarin.Forms.Label) экземплярам страницы. Каждый `Style` из них используется для вывода текста в другом цвете, а также для настройки размера шрифта и параметров макета по горизонтали и вертикали. Каждый `Style` из них применяется к другому `Label` путем установки его [`Style`](xref:Xamarin.Forms.NavigableElement.Style) свойств с помощью `StaticResource` расширения разметки. Результат показан на следующих снимках экрана.

[![Пример явных стилей](explicit-images/explicit-styles.png)](explicit-images/explicit-styles-large.png#lightbox)

Кроме того, к конечному [`Label`](xref:Xamarin.Forms.Label) элементу [`Style`](xref:Xamarin.Forms.Style) применен атрибут, но также переопределяет [`TextColor`](xref:Xamarin.Forms.Label.TextColor) свойство на другое `Color` значение.

### <a name="create-an-explicit-style-at-the-control-level"></a>Создание явного стиля на уровне элемента управления

Помимо создания *явных* стилей на уровне страницы их также можно создать на уровне элемента управления, как показано в следующем примере кода:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelRedStyle" TargetType="Label">
                      ...
                    </Style>
                    ...
                </ResourceDictionary>
            </StackLayout.Resources>
            <Label Text="These labels" Style="{StaticResource labelRedStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

В этом примере *явные* [`Style`](xref:Xamarin.Forms.Style) экземпляры присваиваются [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) коллекции [`StackLayout`](xref:Xamarin.Forms.StackLayout) элемента управления. Затем стили можно применить к элементу управления и его дочерним элементам.

Сведения о создании стилей в приложении [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) см. в разделе [глобальные стили](~/xamarin-forms/user-interface/styles/application.md).

## <a name="create-an-explicit-style-in-c35"></a>Создание явного стиля на языке C&#35;

[`Style`](xref:Xamarin.Forms.Style)экземпляры можно добавить в [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) коллекцию страницы в C#, создав новый объект [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) , а затем добавив `Style` экземпляры в `ResourceDictionary` , как показано в следующем примере кода:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Red    }
            }
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Green }
            }
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Blue }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("labelRedStyle", labelRedStyle);
        Resources.Add ("labelGreenStyle", labelGreenStyle);
        Resources.Add ("labelBlueStyle", labelBlueStyle);
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels",
                            Style = (Style)Resources ["labelRedStyle"] },
                new Label { Text = "are demonstrating",
                            Style = (Style)Resources ["labelGreenStyle"] },
                new Label { Text = "explicit styles,",
                            Style = (Style)Resources ["labelBlueStyle"] },
                new Label {    Text = "and an explicit style override",
                            Style = (Style)Resources ["labelBlueStyle"], TextColor = Color.Teal }
            }
        };
    }
}
```

Конструктор определяет три *явных* стиля, которые применяются к [`Label`](xref:Xamarin.Forms.Label) экземплярам страницы. Каждое *явное* [`Style`](xref:Xamarin.Forms.Style) значение добавляется в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) метод с помощью [`Add`](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) метода, указывая `key` строку для ссылки на `Style` экземпляр. Каждый `Style` из них применяется к другому `Label` путем установки их [`Style`](xref:Xamarin.Forms.NavigableElement.Style) свойств.

Однако здесь нет никаких преимуществ [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . Вместо этого [`Style`](xref:Xamarin.Forms.Style) экземпляры можно назначать непосредственно [`Style`](xref:Xamarin.Forms.NavigableElement.Style) свойствам требуемых визуальных элементов, а `ResourceDictionary` можно удалить, как показано в следующем примере кода:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            ...
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            ...
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            ...
        };
        ...
        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelRedStyle },
                new Label { Text = "are demonstrating", Style = labelGreenStyle },
                new Label { Text = "explicit styles,", Style = labelBlueStyle },
                new Label { Text = "and an explicit style override", Style = labelBlueStyle,
                            TextColor = Color.Teal }
            }
        };
    }
}
```

Конструктор определяет три *явных* стиля, которые применяются к [`Label`](xref:Xamarin.Forms.Label) экземплярам страницы. Каждый `Style` из них используется для вывода текста в другом цвете, а также для настройки размера шрифта и параметров макета по горизонтали и вертикали. Каждый `Style` из них применяется к другому `Label` путем установки его [`Style`](xref:Xamarin.Forms.NavigableElement.Style) свойств. Кроме того, к конечному `Label` элементу `Style` применен атрибут, но также переопределяет `TextColor` свойство на другое `Color` значение.

## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Базовые стили (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [Работа со стилями (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
