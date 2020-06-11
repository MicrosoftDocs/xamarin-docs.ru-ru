---
Title: "динамические стили в Xamarin.Forms " Description: "в этой статье объясняется, как Xamarin.Forms приложение может динамически реагировать на изменения стиля во время выполнения с использованием динамических ресурсов".
MS. произв. Xamarin MS. AssetID: 13D4FA4B-DF10-42BF-B001-2C49367FC216 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 05/28/2019 без-Loc: [ Xamarin.Forms , Xamarin.Essentials ] MS. Custom: видео
---

# <a name="dynamic-styles-in-xamarinforms"></a>Динамические стили вXamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)

_Стили не реагируют на изменения свойств и остаются неизменными в течение всего приложения. Например, после назначения стиля визуальному элементу, если один из экземпляров Setter изменяется, удаляется или добавляется новый экземпляр метода задания, изменения не будут применены к визуальному элементу. Однако приложения могут динамически реагировать на изменения стиля во время выполнения с помощью динамических ресурсов._

`DynamicResource`Расширение разметки аналогично `StaticResource` расширению разметки в том, что оба используют ключ словаря для выборки значения из [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . Однако, хотя `StaticResource` выполняет поиск по одному словарю, компонент `DynamicResource` поддерживает ссылку на ключ словаря. Таким образом, если запись словаря, связанная с ключом, заменена, это изменение применяется к визуальному элементу. Это позволяет вносить изменения в стиль среды выполнения в приложении.

В следующем примере кода показаны *динамические* стили на странице XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesPage" Title="Dynamic" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle"
                   TargetType="SearchBar"
                   BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle"
                   TargetType="SearchBar">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Placeholder="These SearchBar controls"
                       Style="{DynamicResource searchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[`SearchBar`](xref:Xamarin.Forms.SearchBar)Экземпляры используют `DynamicResource` расширение разметки для ссылки на [`Style`](xref:Xamarin.Forms.Style) именованный объект `searchBarStyle` , который не определен в XAML. Однако, поскольку [`Style`](xref:Xamarin.Forms.NavigableElement.Style) свойства `SearchBar` экземпляров задаются с помощью `DynamicResource` , отсутствующий ключ словаря не приводит к созданию исключения.

Вместо этого в файле кода программной части конструктор создает [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) запись с ключом `searchBarStyle` , как показано в следующем примере кода:

```csharp
public partial class DynamicStylesPage : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPage ()
    {
        InitializeComponent ();
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
    }

    void OnButtonClicked (object sender, EventArgs e)
    {
        if (originalStyle) {
            Resources ["searchBarStyle"] = Resources ["greenSearchBarStyle"];
            originalStyle = false;
        } else {
            Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
            originalStyle = true;
        }
    }
}
```

При `OnButtonClicked` выполнении обработчика событий `searchBarStyle` переключается между `blueSearchBarStyle` и `greenSearchBarStyle` . Результат показан на следующих снимках экрана.

Пример [ ![ динамического](dynamic-images/dynamic-style-blue.png)](dynamic-images/dynamic-style-blue-large.png#lightbox)стиля "синий" в примере с 
 [ ![ зеленым стилем](dynamic-images/dynamic-style-green.png)](dynamic-images/dynamic-style-green-large.png#lightbox)

В следующем примере кода показана эквивалентная страница в C#:

```csharp
public class DynamicStylesPageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        ...
        var searchBar1 = new SearchBar { Placeholder = "These SearchBar controls" };
        searchBar1.SetDynamicResource (VisualElement.StyleProperty, "searchBarStyle");
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = { searchBar1, searchBar2, searchBar3, searchBar4,    button    }
        };
    }
    ...
}
```

В C# [`SearchBar`](xref:Xamarin.Forms.SearchBar) экземпляры используют [`SetDynamicResource`](xref:Xamarin.Forms.Element.SetDynamicResource*) метод для ссылки `searchBarStyle` . `OnButtonClicked`Код обработчика событий идентичен примеру XAML, и при исполнении `searchBarStyle` переключается между `blueSearchBarStyle` и `greenSearchBarStyle` .

## <a name="dynamic-style-inheritance"></a>Динамическое наследование стиля

Получение стиля из динамического стиля не может быть достигнуто с помощью [`Style.BasedOn`](xref:Xamarin.Forms.Style.BasedOn) Свойства. Вместо этого [`Style`](xref:Xamarin.Forms.Style) класс включает [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) свойство, для которого можно задать ключ словаря, значение которого может динамически изменяться.

В следующем примере кода показано *динамическое* наследование стиля на странице XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesInheritancePage" Title="Dynamic Inheritance" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle" TargetType="SearchBar" BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle" TargetType="SearchBar">
              ...
            </Style>
            <Style x:Key="tealSearchBarStyle" TargetType="SearchBar" BaseResourceKey="searchBarStyle">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Text="These SearchBar controls" Style="{StaticResource tealSearchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[`SearchBar`](xref:Xamarin.Forms.SearchBar)Экземпляры используют `StaticResource` расширение разметки для ссылки на [`Style`](xref:Xamarin.Forms.Style) именованный объект `tealSearchBarStyle` . При этом `Style` задаются некоторые дополнительные свойства и используется [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) свойство для ссылки `searchBarStyle` . `DynamicResource`Расширение разметки не требуется, поскольку `tealSearchBarStyle` не изменится, за исключением того, что `Style` он является производным от. Поэтому `tealSearchBarStyle` поддерживает ссылку `searchBarStyle` и изменяется при изменении базового стиля.

В файле кода программной части конструктор создает [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) запись с ключом `searchBarStyle` , как в предыдущем примере, в котором демонстрируются динамические стили. При `OnButtonClicked` выполнении обработчика событий `searchBarStyle` переключается между `blueSearchBarStyle` и `greenSearchBarStyle` . Результат показан на следующих снимках экрана.

Пример общего [ ![ наследования динамического стиля с синим](dynamic-images/dynamic-style-inheritance-blue.png)](dynamic-images/dynamic-style-inheritance-blue-large.png#lightbox)примером 
 [ ![ ](dynamic-images/dynamic-style-inheritance-green.png)](dynamic-images/dynamic-style-inheritance-green-large.png#lightbox)

В следующем примере кода показана эквивалентная страница в C#:

```csharp
public class DynamicStylesInheritancePageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesInheritancePageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var tealSearchBarStyle = new Style (typeof(SearchBar)) {
            BaseResourceKey = "searchBarStyle",
            ...
        };
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = {
                new SearchBar { Text = "These SearchBar controls", Style = tealSearchBarStyle },
                ...
            }
        };
    }
    ...
}
```

Объект `tealSearchBarStyle` присваивается непосредственно [`Style`](xref:Xamarin.Forms.NavigableElement.Style) свойству [`SearchBar`](xref:Xamarin.Forms.SearchBar) экземпляров. При этом `Style` задаются некоторые дополнительные свойства и используется [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) свойство для ссылки `searchBarStyle` . [`SetDynamicResource`](xref:Xamarin.Forms.Element.SetDynamicResource*)Метод не требуется, так как `tealSearchBarStyle` не изменяется, за исключением того, что `Style` он является производным от. Поэтому `tealSearchBarStyle` поддерживает ссылку `searchBarStyle` и изменяется при изменении базового стиля.

## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Динамические стили (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)
- [Работа со стилями (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Dynamic-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
