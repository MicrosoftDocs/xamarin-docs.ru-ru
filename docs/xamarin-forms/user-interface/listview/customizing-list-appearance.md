---
Title: "внешний вид ListView" Описание: "в этой статье объясняется, как настраивать ListView в Xamarin.Forms приложениях с помощью заголовков, нижних колонтитулов, групп и ячеек с переменной высотой".
MS. произв. Xamarin MS. AssetID: DC8009B0-4371-4D60-885A-5362FC7EE3E5 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 12/13/2018 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="listview-appearance"></a>Внешний вид ListView

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)

Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) Позволяет настроить представление списка в дополнение к [`ViewCell`](xref:Xamarin.Forms.ViewCell) экземплярам для каждой строки в списке.

## <a name="grouping"></a>Группирование

Большие наборы данных могут стать неудобными, если они представлены в постоянном списке прокрутки. Включение группирования может улучшить взаимодействие с пользователем в этих случаях путем лучшего упорядочения содержимого и активации элементов управления для конкретных платформ, облегчающих навигацию по данным.

Если группирование активировано для `ListView` , для каждой группы добавляется строка заголовка.

Включение группирования:

- Создайте список списков (список групп, каждая из которых является списком элементов).
- Присвойте параметру значение в `ListView` `ItemsSource` списке.
- `IsGroupingEnabled` — присвойте значение True.
- Задайте [`GroupDisplayBinding`](xref:Xamarin.Forms.ListView.GroupDisplayBinding) для привязки к свойству групп, которое используется в качестве заголовка группы.
- Используемых Задайте [`GroupShortNameBinding`](xref:Xamarin.Forms.ListView.GroupShortNameBinding) для привязки к свойству групп, которое используется в качестве краткого имени группы. Короткое имя используется для списков переходов (правый столбец в iOS).

Начните с создания класса для групп.

```csharp
public class PageTypeGroup : List<PageModel>
    {
        public string Title { get; set; }
        public string ShortName { get; set; } //will be used for jump lists
        public string Subtitle { get; set; }
        private PageTypeGroup(string title, string shortName)
        {
            Title = title;
            ShortName = shortName;
        }

        public static IList<PageTypeGroup> All { private set; get; }
    }
```

В приведенном выше коде `All` — это список, который будет передан в ListView в качестве источника привязки. `Title`и `ShortName` — это свойства, которые будут использоваться для заголовков групп.

На этом этапе `All` является пустым списком. Добавьте статический конструктор, чтобы список был заполнен при запуске программы:

```csharp
static PageTypeGroup()
{
    List<PageTypeGroup> Groups = new List<PageTypeGroup> {
            new PageTypeGroup ("Alpha", "A"){
                new PageModel("Amelia", "Cedar", new switchCellPage(),""),
                new PageModel("Alfie", "Spruce", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ava", "Pine", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Archie", "Maple", new switchCellPage(), "grapefruit.jpg")
            },
            new PageTypeGroup ("Bravo", "B"){
                new PageModel("Brooke", "Lumia", new switchCellPage(),""),
                new PageModel("Bobby", "Xperia", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Bella", "Desire", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ben", "Chocolate", new switchCellPage(), "grapefruit.jpg")
            }
        };
        All = Groups; //set the publicly accessible list
}
```

В приведенном выше коде мы также можем вызывать `Add` для элементов `Groups` , которые являются экземплярами типа `PageTypeGroup` . Этот метод возможен, так как `PageTypeGroup` наследует от `List<PageModel>` .

Ниже приведен код XAML для отображения сгруппированного списка:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DemoListView.GroupingViewPage"
    <ContentPage.Content>
        <ListView  x:Name="GroupedView"
                   GroupDisplayBinding="{Binding Title}"
                   GroupShortNameBinding="{Binding ShortName}"
                   IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                              Detail="{Binding Subtitle}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

Этот код XAML выполняет следующие действия:

- Присвоение `GroupShortNameBinding` `ShortName` свойству, определенному в нашем классе Group
- Присвоение `GroupDisplayBinding` `Title` свойству, определенному в нашем классе Group
- Задайте значение `IsGroupingEnabled` true
- Изменено `ListView` на `ItemsSource` сгруппированный список

На следующем снимке экрана показан итоговый пользовательский интерфейс:

![](customizing-list-appearance-images/grouping-depth.png "ListView Grouping Example")

### <a name="customizing-grouping"></a>Настройка группирования

Если группирование включено в списке, можно также настроить заголовок группы.

Аналогично тому, как в `ListView` имеет значение `ItemTemplate` для определения способа отображения строк, `ListView` имеет значение `GroupHeaderTemplate` .

Ниже показан пример настройки заголовка группы в XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DemoListView.GroupingViewPage">
    <ContentPage.Content>
        <ListView x:Name="GroupedView"
                  GroupDisplayBinding="{Binding Title}"
                  GroupShortNameBinding="{Binding ShortName}"
                  IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                              Detail="{Binding Subtitle}"
                              TextColor="#f35e20"
                              DetailColor="#503026" />
                </DataTemplate>
            </ListView.ItemTemplate>
            <!-- Group Header Customization-->
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                              Detail="{Binding ShortName}"
                              TextColor="#f35e20"
                              DetailColor="#503026" />
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
            <!-- End Group Header Customization -->
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

## <a name="headers-and-footers"></a>Верхние и нижние колонтитулы

ListView может представлять верхний и нижний колонтитулы, которые прокручивается с элементами списка. Верхний и нижний колонтитулы могут быть строками текста или более сложным макетом. Это поведение отделено от [групп разделов](#grouping).

Можно задать `Header` значение для параметра и/или, а также `Footer` `string` задать более сложный макет. Существуют также `HeaderTemplate` и `FooterTemplate` свойства, позволяющие создавать более сложные макеты для верхнего и нижнего колонтитулов, поддерживающих привязку данных.

Чтобы создать базовый верхний или нижний колонтитул, просто задайте для свойств верхнего или нижнего колонтитула текст, который требуется отобразить. В коде:

```csharp
ListView HeaderList = new ListView()
{
    Header = "Header",
    Footer = "Footer"
};
```

В XAML:

```xaml
<ListView x:Name="HeaderList" 
          Header="Header"
          Footer="Footer">
    ...
</ListView>
```

![](customizing-list-appearance-images/header-default.png "ListView with Header and Footer")

Чтобы создать настраиваемый верхний и нижний колонтитулы, определите представления верхнего и нижнего колонтитулов.

```xaml
<ListView.Header>
    <StackLayout Orientation="Horizontal">
        <Label Text="Header"
               TextColor="Olive"
               BackgroundColor="Red" />
    </StackLayout>
</ListView.Header>
<ListView.Footer>
    <StackLayout Orientation="Horizontal">
        <Label Text="Footer"
               TextColor="Gray"
               BackgroundColor="Blue" />
    </StackLayout>
</ListView.Footer>
```

![](customizing-list-appearance-images/header-custom.png "ListView with Customized Header and Footer")

## <a name="scrollbar-visibility"></a>Видимость полосы прокрутки

[`ListView`](xref:Xamarin.Forms.ListView)Класс имеет `HorizontalScrollBarVisibility` Свойства и `VerticalScrollBarVisibility` , которые получают или задают [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) значение, представляющее, когда отображается горизонтальная или вертикальная полоса прокрутки. Для обоих свойств можно задать следующие значения:

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility)Указывает поведение полосы прокрутки по умолчанию для платформы и является значением по умолчанию `HorizontalScrollBarVisibility` для `VerticalScrollBarVisibility` свойств и.
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility)Указывает, что полосы прокрутки будут видимы, даже если содержимое умещается в представлении.
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility)Указывает, что полосы прокрутки не будут видны, даже если содержимое не умещается в представлении.

## <a name="row-separators"></a>Разделители строк

Разделительные линии отображаются между `ListView` элементами по умолчанию в iOS и Android. Если вы предпочитаете скрыть разделительные линии в iOS и Android, задайте `SeparatorVisibility` свойство в ListView. Параметры для `SeparatorVisibility` :

- **По умолчанию** — показывает разделительную линию в iOS и Android.
- **Нет** — скрывает разделитель на всех платформах.

Видимость по умолчанию:

C#:

```csharp
SeparatorDemoListView.SeparatorVisibility = SeparatorVisibility.Default;
```

КОДА

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="Default" />
```

![](customizing-list-appearance-images/separator-default.png "ListView with Default Row Separators")

Нет.

C#:

```csharp
SeparatorDemoListView.SeparatorVisibility = SeparatorVisibility.None;
```

КОДА

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="None" />
```

![](customizing-list-appearance-images/separator-none.png "ListView without Row Separators")

Можно также задать цвет разделительной линии через `SeparatorColor` свойство:

C#:

```csharp
SeparatorDemoListView.SeparatorColor = Color.Green;
```

КОДА

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorColor="Green" />
```

![](customizing-list-appearance-images/separator-custom.png "ListView with Green Row Separators")

> [!NOTE]
> Установка любого из этих свойств на Android после загрузки приводит к `ListView` значительному снижению производительности.

## <a name="row-height"></a>Высота строки

По умолчанию все строки в ListView имеют одинаковую высоту. ListView имеет два свойства, которые можно использовать для изменения этого поведения:

- `HasUnevenRows`&ndash; `true`/`false` значение, строки имеют различные значения высоты, если для задано значение `true` . По умолчанию — `false`.
- `RowHeight`&ndash;задает высоту каждой строки `HasUnevenRows` , если имеет значение `false` .

Можно задать высоту всех строк, задав `RowHeight` свойство в `ListView` .

### <a name="custom-fixed-row-height"></a>Пользовательская фиксированная высота строки

C#:

```csharp
RowHeightDemoListView.RowHeight = 100;
```

КОДА

```xaml
<ListView x:Name="RowHeightDemoListView" RowHeight="100" />
```

![](customizing-list-appearance-images/height-custom.png "ListView with Fixed Row Height")

### <a name="uneven-rows"></a>Нечетные строки

Если вы хотите, чтобы отдельные строки имели разную высоту, можно задать `HasUnevenRows` для свойства значение `true` . Высоту строк не нужно задавать вручную `HasUnevenRows` `true` , так как значения высоты будут автоматически вычисляться Xamarin.Forms .

C#:

```csharp
RowHeightDemoListView.HasUnevenRows = true;
```

КОДА

```xaml
<ListView x:Name="RowHeightDemoListView" HasUnevenRows="true" />
```

![](customizing-list-appearance-images/height-uneven.png "ListView with Uneven Rows")

### <a name="resize-rows-at-runtime"></a>Изменение размера строк во время выполнения

`ListView`Размер отдельных строк может быть программным образом изменен во время выполнения при условии, что `HasUnevenRows` свойство имеет значение `true` . [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize)Метод обновляет размер ячейки, даже если она не видна в данный момент, как показано в следующем примере кода:

```csharp
void OnImageTapped (object sender, EventArgs args)
{
    var image = sender as Image;
    var viewCell = image.Parent.Parent as ViewCell;

    if (image.HeightRequest < 250) {
        image.HeightRequest = image.Height + 100;
        viewCell.ForceUpdateSize ();
    }
}
```

`OnImageTapped`Обработчик событий выполняется в ответ на [`Image`](xref:Xamarin.Forms.Image) Нажатие в ячейке и увеличивает размер `Image` отображаемой в ячейке ячейки, чтобы его было легко просмотреть.

![](customizing-list-appearance-images/dynamic-row-resizing.png "ListView with Runtime Row Resizing")

> [!WARNING]
> Чрезмерное изменение размера строк во время выполнения может привести к снижению производительности.

## <a name="related-links"></a>Связанные ссылки

- [Группирование (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)
- [Настраиваемое представление модуля подготовки отчетов (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [Динамическое изменение размера строк (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-dynamicunevenlistcells)
- [Заметки о выпуске 1,4](https://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [Заметки о выпуске 1,3](https://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
