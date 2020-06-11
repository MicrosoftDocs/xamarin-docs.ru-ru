---
Title: " Xamarin.Forms ContentView" Description: "в этой статье объясняется, как использовать класс ContentView для создания пользовательского элемента управления, например кардвиев".
MS. произв. Xamarin MS. AssetID: 638402E7-CA44-456B-863B-791F6B6B561D MS. Technology: Xamarin-Forms author: профексоржеек MS. author: жусжохнс МС. Дата: 08/14/2019 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-contentview"></a>Xamarin.Forms ContentView

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/)

Xamarin.Forms [`ContentView`](xref:Xamarin.Forms.ContentView) Класс является типом `Layout` , который содержит один дочерний элемент и обычно используется для создания настраиваемых, многократно используемых элементов управления. `ContentView`Класс наследует от [`TemplatedView`](xref:Xamarin.Forms.TemplatedView) . В этой статье и связанном примере объясняется, как создать пользовательский `CardView` элемент управления на основе `ContentView` класса.

На следующем снимке экрана показан `CardView` элемент управления, производный от `ContentView` класса:

[![Снимок экрана примера приложения Кардвиев](contentview-images/cardview-list-cropped.png)](contentview-images/cardview-list.png#lightbox)

`ContentView`Класс определяет одно свойство:

* [`Content`](xref:Xamarin.Forms.ContentView.Content)— Это `View` объект. Это свойство поддерживается [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектом, поэтому он может быть целевым объектом привязок данных.

Объект `ContentView` также наследует свойство от `TemplatedView` класса:

* [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate)Объект `ControlTemplate` , который может определять или переопределять внешний вид элемента управления.

Дополнительные сведения о `ControlTemplate` свойстве см. в разделе [Настройка внешнего вида с помощью ControlTemplate](#customize-appearance-with-a-controltemplate).

## <a name="create-a-custom-control"></a>Создание пользовательского элемента управления

`ContentView`Класс предлагает небольшую функциональность, но может использоваться для создания пользовательского элемента управления. Пример проекта определяет элемент `CardView` управления — элемент пользовательского интерфейса, который отображает изображение, заголовок и описание в макете, похожем на карту.

Процесс создания пользовательского элемента управления состоит в следующих целях.

1. Создайте новый класс с помощью `ContentView` шаблона в Visual Studio 2019.
1. Определите уникальные свойства или события в файле кода программной части для нового пользовательского элемента управления.
1. Создайте пользовательский интерфейс для пользовательского элемента управления.

> [!NOTE]
> Можно создать пользовательский элемент управления, макет которого определяется в коде вместо XAML. Для простоты пример приложения определяет только один `CardView` класс с макетом XAML. Однако пример приложения содержит класс **кардвиевкодепаже** , который показывает процесс использования пользовательского элемента управления в коде.

### <a name="create-code-behind-properties"></a>Создание свойств кода программной части

`CardView`Пользовательский элемент управления определяет следующие свойства:

* `CardTitle`: `string` объект, представляющий заголовок, отображаемый на карточке.
* `CardDescription`: `string` объект, представляющий описание, отображаемое на карточке.
* `IconImageSource`: `ImageSource` объект, представляющий изображение, отображаемое на карточке.
* `IconBackgroundColor`: `Color` объект, представляющий цвет фона для изображения, отображаемого на карточке.
* `BorderColor`: `Color` объект, представляющий цвет границы карточки, границы изображения и линии разделителя.
* `CardColor`: `Color` объект, представляющий цвет фона карточки.

> [!NOTE]
> `BorderColor`Свойство влияет на несколько элементов в целях демонстрации. При необходимости это свойство можно разделить на три свойства.

Каждое свойство поддерживается `BindableProperty` экземпляром. Резервное копирование `BindableProperty` позволяет создавать и привязывать каждое свойство к стилю с помощью шаблона MVVM.

В следующем примере показано, как создать резервную копию `BindableProperty` :

```csharp
public static readonly BindableProperty CardTitleProperty = BindableProperty.Create(
    "CardTitle",        // the name of the bindable property
    typeof(string),     // the bindable property type
    typeof(CardView),   // the parent object type
    string.Empty);      // the default value for the property
```

Пользовательское свойство использует `GetValue` методы и `SetValue` для получения и задания `BindableProperty` значений объекта:

```csharp
public string CardTitle
{
    get => (string)GetValue(CardView.CardTitleProperty);
    set => SetValue(CardView.CardTitleProperty, value);
}
```

Дополнительные сведения об `BindableProperty` объектах см. в разделе [свойства, допускающие привязку](~/xamarin-forms/xaml/bindable-properties.md).

### <a name="define-ui"></a>Определение пользовательского интерфейса

Пользовательский интерфейс пользовательского элемента управления использует в `ContentView` качестве корневого элемента для `CardView` элемента управления. В следующем примере показан `CardView` XAML:

```XAML
<ContentView ...
             x:Name="this"
             x:Class="CardViewDemo.Controls.CardView">
    <Frame BindingContext="{x:Reference this}"
            BackgroundColor="{Binding CardColor}"
            BorderColor="{Binding BorderColor}"
            ...>
        <Grid>
            ...
            <Frame BorderColor="{Binding BorderColor, FallbackValue='Black'}"
                   BackgroundColor="{Binding IconBackgroundColor, FallbackValue='Grey'}"
                   ...>
                <Image Source="{Binding IconImageSource}"
                       .. />
            </Frame>
            <Label Text="{Binding CardTitle, FallbackValue='Card Title'}"
                   ... />
            <BoxView BackgroundColor="{Binding BorderColor, FallbackValue='Black'}"
                     ... />
            <Label Text="{Binding CardDescription, FallbackValue='Card description text.'}"
                   ... />
        </Grid>
    </Frame>
</ContentView>
```

`ContentView`Элемент задает `x:Name` для свойства значение **this**, которое можно использовать для доступа к объекту, привязанному к `CardView` экземпляру. Элементы в привязках набора макетов для своих свойств к значениям, определенным для привязанного объекта.

Дополнительные сведения о привязке данных см. в разделе [Привязка данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

> [!NOTE]
> `FallbackValue`Свойство предоставляет значение по умолчанию, если привязка — `null` . Это также позволяет средству [предварительного просмотра XAML](~/xamarin-forms/xaml/xaml-previewer/index.md) в Visual Studio визуализировать `CardView` элемент управления.

## <a name="instantiate-a-custom-control"></a>Создание экземпляра пользовательского элемента управления

Ссылка на пространство имен пользовательского элемента управления должна быть добавлена на страницу, которая создает экземпляр пользовательского элемента управления. В следующем примере показана ссылка на пространство имен под названием **элементы управления** , добавленные в `ContentPage` экземпляр в XAML:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CardViewDemo.Controls" >
```

После добавления ссылки `CardView` можно создать экземпляр в XAML и определить его свойства:

```xaml
<controls:CardView BorderColor="DarkGray"
                   CardTitle="Slavko Vlasic"
                   CardDescription="Lorem ipsum dolor sit..."
                   IconBackgroundColor="SlateGray"
                   IconImageSource="user.png"/>
```

`CardView`Также можно создать экземпляр в коде:

```csharp
CardView card = new CardView
{
    BorderColor = Color.DarkGray,
    CardTitle = "Slavko Vlasic",
    CardDescription = "Lorem ipsum dolor sit...",
    IconBackgroundColor = Color.SlateGray,
    IconImageSource = ImageSource.FromFile("user.png")
};
```

## <a name="customize-appearance-with-a-controltemplate"></a>Настройка внешнего вида с помощью ControlTemplate

Пользовательский элемент управления, производный от `ContentView` класса, может определять внешний вид с помощью XAML, кода или вообще не определять внешний вид. Независимо от того, как определен внешний вид, `ControlTemplate` объект может переопределить внешний вид с помощью пользовательского макета.

`CardView`Макет может занимать слишком много пространства для некоторых вариантов использования. `ControlTemplate`Может переопределить `CardView` Макет, чтобы обеспечить более компактное представление, подходящее для уплотненного списка:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <ControlTemplate x:Key="CardViewCompressed">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="100" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="100" />
                    <ColumnDefinition Width="100*" />
                </Grid.ColumnDefinitions>
                <Image Grid.Row="0"
                       Grid.Column="0"
                       Source="{TemplateBinding IconImageSource}"
                       BackgroundColor="{TemplateBinding IconBackgroundColor}"
                       WidthRequest="100"
                       HeightRequest="100"
                       Aspect="AspectFill"
                       HorizontalOptions="Center"
                       VerticalOptions="Center"/>
                <StackLayout Grid.Row="0"
                             Grid.Column="1">
                    <Label Text="{TemplateBinding CardTitle}"
                           FontAttributes="Bold" />
                    <Label Text="{TemplateBinding CardDescription}" />
                </StackLayout>
            </Grid>
        </ControlTemplate>
    </ResourceDictionary>
</ContentPage.Resources>
```

Привязка данных в `ControlTemplate` использует `TemplateBinding` расширение разметки для указания привязок. `ControlTemplate`Для свойства можно задать определенный объект ControlTemplate, используя его `x:Key` значение. В следующем примере показано `ControlTemplate` свойство, заданное для `CardView` экземпляра.

```xaml
<controls:CardView ControlTemplate="{StaticResource CardViewCompressed}"/>
```

На следующих снимках экрана показан стандартный `CardView` экземпляр, который `CardView` `ControlTemplate` был переопределен:

[![Снимок экрана ControlTemplate Кардвиев](contentview-images/cardview-controltemplates-cropped.png)](contentview-images/cardview-controltemplates.png#lightbox)

Дополнительные сведения о шаблонах элементов управления см. в разделе [Шаблоны элементов управления Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/control-template.md).

## <a name="related-links"></a>Связанные ссылки

* [Пример приложения ContentView](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/)
* [Привязка данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md)
* [Привязываемые свойства](~/xamarin-forms/xaml/bindable-properties.md).
* [Шаблоны элементов управления Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/control-template.md)
