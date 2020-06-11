---
Title: "глобальные стили в Xamarin.Forms описании:" стили можно сделать доступными глобально, добавив их в словарь ресурсов приложения. Это помогает избежать дублирования стилей на страницах или элементах управления ".
MS. произв. Xamarin MS. AssetID: BDC65F82-65E0-4C8E-BB91-8E340EB2D15A MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 02/17/2016 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="global-styles-in-xamarinforms"></a>Глобальные стили вXamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_Стили можно сделать доступными глобально, добавив их в словарь ресурсов приложения. Это помогает избежать дублирования стилей на страницах или в элементах управления._

## <a name="create-a-global-style-in-xaml"></a>Создание глобального стиля в XAML

По умолчанию все Xamarin.Forms приложения, созданные на основе шаблона, используют класс **app** для реализации этого [`Application`](xref:Xamarin.Forms.Application) подкласса. Чтобы объявить на [`Style`](xref:Xamarin.Forms.Style) уровне приложения, в приложении, [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) использующем XAML, класс **приложения** по умолчанию должен быть ЗАМЕНЕН классом **приложения** XAML и связанным кодом программной части. Дополнительные сведения см. [в разделе Работа с классом App](~/xamarin-forms/app-fundamentals/application-class.md).

В следующем примере кода показана [`Style`](xref:Xamarin.Forms.Style) объявленная на уровне приложения:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.App">
    <Application.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BorderColor" Value="Lime" />
                <Setter Property="BorderRadius" Value="5" />
                <Setter Property="BorderWidth" Value="5" />
                <Setter Property="WidthRequest" Value="200" />
                <Setter Property="TextColor" Value="Teal" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Он [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) определяет один *явный* стиль, `buttonStyle` который будет использоваться для установки внешнего вида [`Button`](xref:Xamarin.Forms.Button) экземпляров. Однако глобальные стили могут быть *явно* или *неявными*.

В следующем примере кода показана страница XAML, которая применяет `buttonStyle` к [`Button`](xref:Xamarin.Forms.Button) экземплярам страницы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Результат показан на следующих снимках экрана.

[![](application-images/application-styles-1.png "Global Styles Example")](application-images/application-styles-1-large.png#lightbox "Global Styles Example")

Сведения о создании стилей в странице [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) см. в разделе [явные стили](~/xamarin-forms/user-interface/styles/explicit.md) и [Неявные стили](~/xamarin-forms/user-interface/styles/implicit.md).

### <a name="override-styles"></a>Переопределить стили

Стили ниже в иерархии представлений имеют приоритет над теми, которые определены выше. Например, задание [`Style`](xref:Xamarin.Forms.Style) , которое задает значение [`Button.TextColor`](xref:Xamarin.Forms.Button.TextColor) на `Red` уровне приложения, будет переопределено стилем уровня страницы, для которого устанавливается `Button.TextColor` значение `Green` . Аналогичным образом, стиль уровня страницы будет переопределен стилем уровня элемента управления. Кроме того, если `Button.TextColor` свойство задано непосредственно в свойстве элемента управления, оно имеет приоритет над любыми стилями. Этот приоритет показан в следующем примере кода:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                ...
                <Setter Property="TextColor" Value="Red" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="buttonStyle" TargetType="Button">
                        ...
                        <Setter Property="TextColor" Value="Blue" />
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Исходный объект `buttonStyle` , определенный на уровне приложения, переопределяется `buttonStyle` экземпляром, определенным на уровне страницы. Кроме того, стиль уровня страницы переопределяется уровнем управления `buttonStyle` . Таким образом, [`Button`](xref:Xamarin.Forms.Button) экземпляры отображаются синим текстом, как показано на следующих снимках экрана:

[![](application-images/application-styles-2.png "Overriding Styles Example")](application-images/application-styles-2-large.png#lightbox "Overriding Styles Example")

## <a name="create-a-global-style-in-c35"></a>Создание глобального стиля на языке C&#35;

[`Style`](xref:Xamarin.Forms.Style)экземпляры можно добавить в [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) коллекцию приложения в C#, создав новый объект [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) , а затем добавив `Style` экземпляры в `ResourceDictionary` , как показано в следующем примере кода:

```csharp
public class App : Application
{
    public App ()
    {
        var buttonStyle = new Style (typeof(Button)) {
            Setters = {
                ...
                new Setter { Property = Button.TextColorProperty,    Value = Color.Teal }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("buttonStyle", buttonStyle);
        ...
    }
    ...
}
```

Конструктор определяет один *явный* стиль для применения к [`Button`](xref:Xamarin.Forms.Button) экземплярам во всем приложении. *Явный* [`Style`](xref:Xamarin.Forms.Style) экземпляры добавляются в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) метод с помощью [`Add`](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) метода, указывая `key` строку для ссылки на `Style` экземпляр. `Style`Затем экземпляр можно применить к любым элементам управления правильного типа в приложении. Однако глобальные стили могут быть *явно* или *неявными*.

В следующем примере кода показана страница C#, которая применяет `buttonStyle` к [`Button`](xref:Xamarin.Forms.Button) экземплярам страницы:

```csharp
public class ApplicationStylesPageCS : ContentPage
{
    public ApplicationStylesPageCS ()
    {
        ...
        Content = new StackLayout {
            Children = {
                new Button { Text = "These buttons", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "are demonstrating", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "application styles", Style = (Style)Application.Current.Resources ["buttonStyle"]
                }
            }
        };
    }
}
```

`buttonStyle`Применяется к [`Button`](xref:Xamarin.Forms.Button) экземплярам путем задания их [`Style`](xref:Xamarin.Forms.NavigableElement.Style) свойств и управления внешним видом `Button` экземпляров.

## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Базовые стили (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [Работа со стилями (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
