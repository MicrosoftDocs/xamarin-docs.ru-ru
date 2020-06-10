---
Title: " Xamarin.Forms расширитель" Description: " Xamarin.Forms элемент управления Expander предоставляет расширяемый контейнер для размещения любого содержимого. Содержимое отображается или скрывается путем касания заголовка расширителя.
MS. произв. Xamarin MS. AssetID: 381DCB55-522D-4414-B45B-E8DD70AA9985 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч MS. Дата: 04/15/2020 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-expander"></a>Xamarin.FormsExpander

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos/)

Xamarin.Forms `Expander` Элемент управления предоставляет расширяемый контейнер для размещения любого содержимого. Элемент управления имеет заголовок и содержимое, и содержимое отображается или скрыто путем касания `Expander` заголовка. Если отображается только `Expander` заголовок, `Expander` *сворачивается*. Если `Expander` содержимое видимо, `Expander` *разворачивается*.

На следующих снимках экрана `Expander` в свернутом и развернутом состоянии отображаются красные поля, указывающие заголовок и содержимое:

![Снимок экрана элемента развертывания в свернутом и развернутом состоянии в iOS и Android](expander-images/expander.png "Расширитель в iOS и Android")

> [!IMPORTANT]
> `Expander`в настоящее время экспериментально и может использоваться только путем установки `Expander_Experimental` флага. Дополнительные сведения см. в разделе [экспериментальные флаги](~/xamarin-forms/internals/experimental-flags.md).
>
> Кроме того, `Expander` элемент управления полностью реализован в `Xamarin.Forms` пространстве имен. Таким образом, он доступен на всех платформах, поддерживаемых Xamarin.Forms .

`Expander`Элемент управления определяет следующие свойства:

- `CollapseAnimationEasing`Тип [`Easing`](xref:Xamarin.Forms.Easing) , который представляет функцию плавности, применяемую к `Expander` содержимому при свертывании.
- `CollapseAnimationLength`Тип `uint` , который определяет длительность анимации при `Expander` свертывании. Значение этого свойства по умолчанию — 250 мс.
- `Command`Тип `ICommand` , который выполняется при `Expander` касании заголовка.
- `CommandParameter` с типом `object`, который передается как параметр в `Command`.
- `Content`Тип [`View`](xref:Xamarin.Forms.View) , который определяет содержимое, отображаемое при `Expander` развертывании.
- `ContentTemplate`Тип [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , который является шаблоном, используемым для динамического расширения содержимого `Expander` .
- `ExpandAnimationEasing`Тип [`Easing`](xref:Xamarin.Forms.Easing) , который представляет функцию плавности, применяемую к `Expander` содержимому во время расширения.
- `ExpandAnimationLength`Тип `uint` , который определяет длительность анимации при `Expander` развертывании. Значение этого свойства по умолчанию — 250 мс.
- `ForceUpdateSizeCommand`Тип `ICommand` , который определяет команду, выполняемую при `Expander` принудительном обновлении размера. Это свойство использует `OneWayToSource` режим привязки.
- `Header`Тип [`View`](xref:Xamarin.Forms.View) , который определяет содержимое заголовка.
- `IsExpanded`Тип `bool` , который определяет, `Expander` развернут ли объект. Это свойство использует `TwoWay` режим привязки и имеет значение по умолчанию `false` .
- `Spacing`Тип `double` , который представляет пространство между заголовком и его содержимым. Значение по умолчанию для этого свойства равно 0.
- `State`Тип `ExpanderState` , который представляет состояние `Expander` . Это свойство использует `OneWayToSource` режим привязки.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

> [!NOTE]
> `Content`Свойство является свойством Content `Expander` класса, поэтому его не нужно явно ЗАДАВАТЬ из XAML.

Перечисление `ExpanderState` определяет следующие члены:

- `Expanding`Указывает, что развернут `Expander` .
- `Expanded`Указывает, что `Expander` развернут.
- `Collapsing`Указывает, что параметр `Expander` является сворачиванием.
- `Collapsed`Указывает, что объект `Expander` свернут.

`Expander`Элемент управления также определяет `Tapped` событие, которое возникает при `Expander` касании заголовка. Кроме того, `Expander` включает `ForceUpdateSize` метод, который может быть вызван для программного изменения размера `Expander` во время выполнения.

## <a name="create-an-expander"></a>Создание расширителя

В следующем примере показано, как создать экземпляр `Expander` в XAML:

```xaml
<Expander>
    <Expander.Header>
        <Label Text="Baboon"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Grid Padding="10">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>
        <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
               Aspect="AspectFill"
               HeightRequest="120"
               WidthRequest="120" />
        <Label Grid.Column="1"
               Text="Baboons are African and Arabian Old World monkeys belonging to the genus Papio, part of the subfamily Cercopithecinae."
               FontAttributes="Italic" />
    </Grid>
</Expander>
```

В этом примере объект `Expander` свернут по умолчанию и отображает в [`Label`](xref:Xamarin.Forms.Label) качестве заголовка. При нажатии на заголовок происходит `Expander` расширение, чтобы отобразить его содержимое, которое [`Grid`](xref:Xamarin.Forms.Grid) содержит дочерние элементы управления. Когда `Expander` разворачивается, при касании его заголовка сворачивается `Expander` .

> [!IMPORTANT]
> При задании `Expander.Content` свойства явно или неявно, `Expander` содержимое создается при переходе к странице, содержащей этот элемент, даже если `Expander` свернут. Однако `Expander.ContentTemplate` свойство может быть задано для содержимого, которое только при `Expander` развертывании разворачивается в первый раз. Дополнительные сведения см. [в разделе Создание содержимого расширителя по запросу](#create-expander-content-on-demand).

Кроме того, `Expander` можно создать в коде:

```csharp
Expander expander = new Expander
{
    Header = new Label
    {
        Text = "Baboon",
        FontAttributes = FontAttributes.Bold,
        FontSize = Device.GetNamedSize(NamedSize.Medium, typeof(Label))
    }
};

Grid grid = new Grid
{
    Padding = new Thickness(10),
    ColumnDefinitions =
    {
        new ColumnDefinition { Width = GridLength.Auto },
        new ColumnDefinition { Width = GridLength.Auto }
    }
};

grid.Children.Add(new Image
{
    Source = "http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg",
    Aspect = Aspect.AspectFill,
    HeightRequest = 120,
    WidthRequest = 120
});

grid.Children.Add(new Label
{
    Text = "Baboons are African and Arabian Old World monkeys belonging to the genus Papio, part of the subfamily Cercopithecinae.",
    FontAttributes = FontAttributes.Italic
}, 1, 0);

expander.Content = grid;
```

## <a name="create-expander-content-on-demand"></a>Создание содержимого расширителя по запросу

`Expander`содержимое может быть создано по запросу в ответ на `Expander` расширение. Это можно сделать, задав `Expander.ContentTemplate` свойству значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , которое содержит содержимое:

```xaml
<Expander>
    <Expander.Header>
        <Label Text="{Binding Name}"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Expander.ContentTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="120"
                       WidthRequest="120" />
                <Label Grid.Column="1"
                       Text="{Binding Details}"
                       FontAttributes="Italic" />
            </Grid>
        </DataTemplate>
    </Expander.ContentTemplate>
</Expander>
```

В этом примере `Expander` содержимое развертывается только при `Expander` первом развертывании.

Преимуществом этого подхода является то, что если страница содержит несколько `Expander` объектов, содержимое для создается `Expander` только при первом развертывании пользователем.

## <a name="add-an-expansion-indicator"></a>Добавление индикатора расширения

[`Image`](xref:Xamarin.Forms.Image)Можно добавить в `Expander` заголовок, чтобы предоставить визуальную индикацию состояния расширения. [`DataTrigger`](xref:Xamarin.Forms.DataTrigger)Может быть присоединен к `Image` , который изменяет `Source` свойство на основе значения `Expander.IsExpanded` Свойства:

```xaml
<Expander>
    <Expander.Header>
        <Grid>
            <Label Text="{Binding Name}"
                   FontAttributes="Bold"
                   FontSize="Medium" />
            <Image Source="expand.png"
                   HorizontalOptions="End"
                   VerticalOptions="Start">
                <Image.Triggers>
                    <DataTrigger TargetType="Image"
                                 Binding="{Binding Source={RelativeSource AncestorType={x:Type Expander}}, Path=IsExpanded}"
                                 Value="True">
                        <Setter Property="Source"
                                Value="collapse.png" />
                    </DataTrigger>
                </Image.Triggers>
            </Image>
        </Grid>
    </Expander.Header>
    <Expander.ContentTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="120"
                       WidthRequest="120" />
                <Label Grid.Column="1"
                       Text="{Binding Details}"
                       FontAttributes="Italic" />
            </Grid>
        </DataTemplate>
    </Expander.ContentTemplate>
</Expander>
```

В этом примере по [`Image`](xref:Xamarin.Forms.Image) `expand` умолчанию отображается значок.

![Снимок экрана значка развертывания в свернутом состоянии, в iOS и Android](expander-images/icon-expand.png "Значок развертывания в iOS и Android")

`IsExpanded`Свойство становится `true` при `Expander` касании заголовка, что приводит к `collapse` отображению значка:

![Снимок экрана значка развертывания в состоянии "развернуть" в iOS и Android](expander-images/icon-collapse.png "Значок развертывания в iOS и Android")

Дополнительные сведения о триггерах см. в разделе [ Xamarin.Forms Triggers](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="define-the-space-between-header-and-content"></a>Определение пробела между заголовком и содержимым

По умолчанию содержимое в `Expander` отображается непосредственно под его заголовком. Однако это поведение можно изменить, задав `Spacing` свойству `double` значение, представляющее пустое пространство между содержимым и его заголовком:

```xaml
<Expander Spacing="50"
          IsExpanded="true">
    <Expander.Header>
        <Label Text="Baboon"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Grid Padding="10">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>
        <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
               Aspect="AspectFill"
               HeightRequest="120"
               WidthRequest="120" />
        <Label Grid.Column="1"
               Text="Baboons are African and Arabian Old World monkeys belonging to the genus Papio, part of the subfamily Cercopithecinae."
               FontAttributes="Italic" />
    </Grid>
</Expander>
```

В этом примере `Expander` содержимое отображается 50 единиц, независимых от устройства, под заголовком:

![Снимок экрана: расширитель с заданными промежутками в iOS и Android](expander-images/expander-spacing.png "Расширитель с набором расстояний в iOS и Android")

## <a name="embed-an-expander-in-an-expander"></a>Внедрение расширителя в расширитель

Содержимое `Expander` может быть задано для другого `Expander` элемента управления, чтобы обеспечить несколько уровней расширения. В следующем коде XAML показан объект, `Expander` содержимое которого является другим `Expander` объектом:

```xaml
<Expander Spacing="10">
    <Expander.Header>
        <Label Text="{Binding Name}"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Expander Padding="10"
              Spacing="10">
        <Expander.Header>
            <Label Text="{Binding Location}"
                   FontSize="Medium" />
        </Expander.Header>
            <Expander.ContentTemplate>
                <DataTemplate>
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto" />
                            <ColumnDefinition Width="Auto" />
                        </Grid.ColumnDefinitions>
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="120"
                               WidthRequest="120" />
                        <Label Grid.Column="1"
                               Text="{Binding Details}"
                               FontAttributes="Italic" />
                    </Grid>
                </DataTemplate>
            </Expander.ContentTemplate>
    </Expander>
</Expander>
```

В этом примере при касании корневого `Expander` заголовка открывается заголовок для дочернего элемента `Expander` :

![Снимок экрана встроенного расширителя в iOS и Android](expander-images/embedded-expander1.png "Встроенный расширитель в iOS и Android")

При касании дочернего `Expander` заголовка происходит его содержимое и отображается:

![Снимок экрана встроенного расширителя в iOS и Android](expander-images/embedded-expander2.png "Встроенный расширитель в iOS и Android")

## <a name="define-the-expand-and-collapse-animation"></a>Определение анимации развертывания и свертывания

Анимация, возникающая, когда `Expander` можно определить развернутые или свернутые элементы, задав `ExpandAnimationEasing` `CollapseAnimationEasing` Свойства и для любой из функций плавности, включенных в Xamarin.Forms , или пользовательских функций плавности. По умолчанию анимация разворачивания и сворачивания выполняется через 250 мс. Однако эти длительности можно изменить, задав `ExpandAnimationLength` `CollapseAnimationLength` Свойства и равными `uint` значениям.

В следующем коде XAML показан пример определения анимации, возникающей при `Expander` развертывании или сворачивании пользователем.

```xaml
<Expander ExpandAnimationEasing="{x:Static Easing.CubicIn}"
          ExpandAnimationLength="500"
          CollapseAnimationEasing="{x:Static Easing.CubicOut}"
          CollapseAnimationLength="500">
    <Expander.Header>
        <Label Text="{Binding Name}"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Expander.ContentTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="120"
                       WidthRequest="120" />
                <Label Grid.Column="1"
                       Text="{Binding Details}"
                       FontAttributes="Italic" />
            </Grid>
        </DataTemplate>
    </Expander.ContentTemplate>
</Expander>
```

В этом примере `CubicIn` функция плавности медленно ускоряет анимацию развертывания по сравнению с 500 мс, а `CubicOut` функция плавности быстро замедляет сворачивание анимации по 500 мс.

Дополнительные сведения о функциях плавности см. в разделе [ Xamarin.Forms функции плавности](~/xamarin-forms/user-interface/animation/easing.md).

## <a name="resize-an-expander-at-runtime"></a>Изменение размера расширителя во время выполнения

`Expander`Можно программно изменить размер в среде выполнения с помощью `ForceUpdateSize` метода.

При наличии `Expander` именованного объекта `expander` , содержимое которого включает в себя объект с [`Label`](xref:Xamarin.Forms.Label) `TapGestureRecognizer` присоединенным к нему, в следующем примере кода показан вызов `ForceUpdateSize` метода:

```csharp
void OnLabelTapped(object sender, EventArgs e)
{
    Label label = sender as Label;
    Expander expander = label.Parent.Parent.Parent as Expander;

    if (label.FontSize == Device.GetNamedSize(NamedSize.Default, label))
    {
        label.FontSize = Device.GetNamedSize(NamedSize.Large, label);
    }
    else
    {
        label.FontSize = Device.GetNamedSize(NamedSize.Default, label);
    }
    expander.ForceUpdateSize();
}
```

В этом примере объект `FontSize` [`Label`](xref:Xamarin.Forms.Label) изменяется при `Label` нажатии кнопки. Из-за размера изменяемого шрифта необходимо обновить размер объекта, `Expander` вызвав его `ForceUpdateSize` метод.

## <a name="disable-an-expander"></a>Отключение расширителя

Приложение может ввести состояние, при котором расширение не является `Expander` допустимой операцией. В таких случаях `Expander` можно отключить, задав `IsEnabled` свойству значение false. Это не позволит пользователям разворачивать или сворачивать `Expander` .

## <a name="related-links"></a>Связанные ссылки

- [Демо-расширители (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos/)
- [Xamarin.FormsФункции плавности](~/xamarin-forms/user-interface/animation/easing.md)
- [Триггеры Xamarin.Forms](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.FormsМакеты с возможностью привязки](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
