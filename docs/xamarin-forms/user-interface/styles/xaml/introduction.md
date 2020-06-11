---
Title: "Общие сведения о Xamarin.Forms стилях" Описание: "стили позволяют настраивать внешний вид визуальных элементов. Стили определяются для конкретного типа и содержат значения для свойств, доступных для этого типа. "
MS. произв. Xamarin MS. AssetID: 3FF899C0-6CFB-4C1D-837D-9E9E10181967 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 04/27/2016 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="introduction-to-xamarinforms-styles"></a>Общие сведения о Xamarin.Forms стилях

_Стили позволяют настраивать внешний вид визуальных элементов. Стили определяются для конкретного типа и содержат значения для свойств, доступных для этого типа._

Xamarin.Formsприложения часто содержат несколько элементов управления, имеющих одинаковый внешний вид. Например, приложение может иметь несколько [`Label`](xref:Xamarin.Forms.Label) экземпляров с одинаковыми параметрами шрифта и параметрами макета, как показано в следующем примере кода XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Styles.NoStylesPage"
    Title="No Styles"
    IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="are not"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="using styles"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

В следующем примере кода создается эквивалентная страница на языке C#:

```csharp
public class NoStylesPageCS : ContentPage
{
    public NoStylesPageCS ()
    {
        Title = "No Styles";
        IconImageSource = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label {
                    Text = "These labels",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "are not",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "using styles",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                }
            }
        };
    }
}
```

Каждый [`Label`](xref:Xamarin.Forms.Label) экземпляр имеет одинаковые значения свойств для управления внешним видом текста, отображаемого `Label` . Результат показан на следующих снимках экрана.

[![Внешний вид метки без стилей](introduction-images/no-styles.png)](introduction-images/no-styles-large.png#lightbox)

Настройка внешнего вида каждого отдельного элемента управления может быть повторена и подвержена ошибкам. Вместо этого можно создать стиль, определяющий внешний вид, а затем применить его к необходимым элементам управления.

## <a name="create-a-style"></a>Создание стиля

Класс [`Style`](xref:Xamarin.Forms.Style) объединяет коллекцию значений свойств в один объект, который затем можно применять к нескольким экземплярам визуальных элементов. Это помогает уменьшить повторяющуюся разметку и позволяет упростить изменение внешнего вида приложений.

Хотя стили разрабатывались в основном для приложений на основе XAML, их также можно создать на языке C#:

- [`Style`](xref:Xamarin.Forms.Style)экземпляры, созданные в XAML, обычно определяются в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) , который назначается [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) коллекции элемента управления, страницы или [`Resources`](xref:Xamarin.Forms.Application.Resources) коллекции приложения.
- [`Style`](xref:Xamarin.Forms.Style)экземпляры, созданные в C#, обычно определяются в классе страницы или в классе, доступ к которому можно получить глобально.

От того, где определен шаблон [`Style`](xref:Xamarin.Forms.Style), зависит то, где его можно использовать.

- [`Style`](xref:Xamarin.Forms.Style)экземпляры, определенные на уровне элемента управления, могут быть применены только к элементу управления и его дочерним элементам.
- [`Style`](xref:Xamarin.Forms.Style)экземпляры, определенные на уровне страницы, могут быть применены только к странице и ее дочерним элементам.
- Экземпляры [`Style`](xref:Xamarin.Forms.Style), определенные на уровне приложения, можно применять по всему приложению.

Каждый экземпляр [`Style`](xref:Xamarin.Forms.Style) содержит коллекцию из одного или нескольких объектов [`Setter`](xref:Xamarin.Forms.Setter), каждый из которых `Setter` имеет свойство ([`Property`](xref:Xamarin.Forms.Setter.Property)) и значение ([`Value`](xref:Xamarin.Forms.Setter.Value)). `Property` — это имя привязываемого свойства элемента, к которому применяется стиль, а `Value` — это применяемое к этому свойству значение.

Каждый [`Style`](xref:Xamarin.Forms.Style) экземпляр может быть *явным*или *неявным*:

- *Явный* [`Style`](xref:Xamarin.Forms.Style) экземпляр определяется с помощью указания [`TargetType`](xref:Xamarin.Forms.Style.TargetType) и `x:Key` значения, а также путем присвоения [`Style`](xref:Xamarin.Forms.NavigableElement.Style) свойству целевого элемента `x:Key` ссылки. Дополнительные сведения о *явных* стилях см. в разделе [явные стили](~/xamarin-forms/user-interface/styles/explicit.md).
- *Неявный* [`Style`](xref:Xamarin.Forms.Style) экземпляр определяется путем указания только [`TargetType`](xref:Xamarin.Forms.Style.TargetType) . `Style`Затем экземпляр будет автоматически применен ко всем элементам этого типа. Обратите внимание, что в подклассах объекта не `TargetType` `Style` применяется автоматически. Дополнительные сведения о *неявных* стилях см. в разделе [Неявные стили](~/xamarin-forms/user-interface/styles/implicit.md).

При создании [`Style`](xref:Xamarin.Forms.Style) свойство [`TargetType`](xref:Xamarin.Forms.Style.TargetType) является обязательным. В следующем примере кода показан *явный* стиль (Примечание), `x:Key` созданный в XAML:

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

Чтобы применить объект `Style` , целевой объект должен иметь [`VisualElement`](xref:Xamarin.Forms.VisualElement) значение, совпадающее со [`TargetType`](xref:Xamarin.Forms.Style.TargetType) значением свойства `Style` , как показано в следующем примере кода XAML:

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

Стили ниже в иерархии представлений имеют приоритет над теми, которые определены выше. Например, задание [`Style`](xref:Xamarin.Forms.Style) , которое задает значение [`Label.TextColor`](xref:Xamarin.Forms.Label.TextColor) на `Red` уровне приложения, будет переопределено стилем уровня страницы, для которого устанавливается `Label.TextColor` значение `Green` . Аналогичным образом, стиль уровня страницы будет переопределен стилем уровня элемента управления. Кроме того, если `Label.TextColor` свойство задано непосредственно для свойства элемента управления, это имеет приоритет над любыми стилями.

В статьях этого раздела показано, как создавать и применять *явные* и *неявные* стили, как создавать глобальные стили, наследование стилей, как реагировать на изменения стиля во время выполнения, а также как использовать встроенные стили, входящие в Xamarin.Forms .

> [!NOTE]
> **Что такое Стилеид?**
>
> До Xamarin.Forms 2,2 [`StyleId`](xref:Xamarin.Forms.Element.StyleId) свойство было использовано для идентификации отдельных элементов в приложении для идентификации в тестировании пользовательского интерфейса, а также в механизмах тем, например пиксате. Однако в Xamarin.Forms 2,2 введено [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) свойство, заменяющее [`StyleId`](xref:Xamarin.Forms.Element.StyleId) свойство.

## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
