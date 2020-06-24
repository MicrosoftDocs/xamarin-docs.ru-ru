---
title: Настройка ViewCell
description: ViewCell в Xamarin.Forms — это ячейка, которую можно добавить в представление ListView или TableView и которая содержит представления, определяемые разработчиком. В этой статье показано, как создать пользовательский отрисовщик для ячейки ViewCell, размещенной в элементе управления ListView Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 61F378C9-6DEF-436B-ACC3-2324B25D404E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c908da816352e8b3790ded0bef932e1485170abd
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84573876"
---
# <a name="customizing-a-viewcell"></a>Настройка ViewCell

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-viewcell)

_ViewCell в Xamarin.Forms — это ячейка, которую можно добавить в представление ListView или TableView и которая содержит представления, определяемые разработчиком. В этой статье показано, как создать пользовательский отрисовщик для ячейки ViewCell, размещенной в элементе управления ListView Xamarin.Forms. Это предотвратит постоянные вызовы вычисления макета Xamarin.Forms во время прокрутки ListView._

Каждая ячейка Xamarin.Forms имеет сопутствующий отрисовщик для каждой платформы, который создает экземпляр собственного элемента управления. Когда [`ViewCell`](xref:Xamarin.Forms.ViewCell) отображается в приложении Xamarin.Forms, на платформе iOS создается класс `ViewCellRenderer`, который, в свою очередь, создает собственный элемент управления `UITableViewCell`. На платформе Android класс `ViewCellRenderer` создает собственный элемент управления `View`. На универсальной платформе Windows (UWP) класс `ViewCellRenderer` создает собственный элемент управления `DataTemplate`. Дополнительные сведения об отрисовщике и классах собственных элементов управления, с которыми сопоставляются элементы управления Xamarin.Forms, см. в статье [Базовые классы и собственные элементы управления отрисовщика](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

На следующей схеме показана связь между классом [`ViewCell`](xref:Xamarin.Forms.ViewCell) и соответствующими собственными элементами управления, которые его реализуют:

![](viewcell-images/viewcell-classes.png "Relationship Between the ViewCell Control and the Implementing Native Controls")

Процесс отрисовки можно использовать для реализации настроек конкретных платформ путем создания пользовательского отрисовщика для [`ViewCell`](xref:Xamarin.Forms.ViewCell) на каждой платформе. Этот процесс выглядит следующим образом:

1. [Создание](#creating-the-custom-cell) пользовательской ячейки Xamarin.Forms.
1. [Использование](#consuming-the-custom-cell) пользовательской ячейки в Xamarin.Forms.
1. [Создание](#creating-the-custom-renderer-on-each-platform) пользовательского отрисовщика для ячейки для каждой платформы.

Мы обсудим каждый элемент по очереди, чтобы реализовать отрисовщик `NativeCell`, в котором используются преимущества зависящих от платформы макетов для каждой ячейки, размещенной в элементе управления [`ListView`](xref:Xamarin.Forms.ListView) Xamarin.Forms. Это предотвратит постоянные вызовы вычисления макета Xamarin.Forms во время прокрутки `ListView`.

## <a name="creating-the-custom-cell"></a>Создание пользовательской ячейки

Пользовательский элемент управления ячейки можно создать путем создания подклассов класса [`ViewCell`](xref:Xamarin.Forms.ViewCell), как показано в следующем примере кода.

```csharp
public class NativeCell : ViewCell
{
  public static readonly BindableProperty NameProperty =
    BindableProperty.Create ("Name", typeof(string), typeof(NativeCell), "");

  public string Name {
    get { return (string)GetValue (NameProperty); }
    set { SetValue (NameProperty, value); }
  }

  public static readonly BindableProperty CategoryProperty =
    BindableProperty.Create ("Category", typeof(string), typeof(NativeCell), "");

  public string Category {
    get { return (string)GetValue (CategoryProperty); }
    set { SetValue (CategoryProperty, value); }
  }

  public static readonly BindableProperty ImageFilenameProperty =
    BindableProperty.Create ("ImageFilename", typeof(string), typeof(NativeCell), "");

  public string ImageFilename {
    get { return (string)GetValue (ImageFilenameProperty); }
    set { SetValue (ImageFilenameProperty, value); }
  }
}
```

Класс `NativeCell` создается в проекте библиотеки .NET Standard и определяет API для пользовательской ячейки. Пользовательская ячейка предоставляет свойства `Name`, `Category` и `ImageFilename`, которые могут отображаться посредством привязки данных. Дополнительные сведения о привязке данных см. в статье [Основы привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

## <a name="consuming-the-custom-cell"></a>Использование пользовательской ячейки

На пользовательскую ячейку `NativeCell` можно ссылаться в XAML в проекте библиотеки .NET Standard, объявив пространство имен для ее расположения и используя префикс пространства имен в элементе пользовательской ячейки. В следующем примере кода показано, как пользовательская ячейка `NativeCell` может использоваться страницей XAML.

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <ContentPage.Content>
          <StackLayout>
              <Label Text="Xamarin.Forms native cell" HorizontalTextAlignment="Center" />
              <ListView x:Name="listView" CachingStrategy="RecycleElement" ItemSelected="OnItemSelected">
                  <ListView.ItemTemplate>
                      <DataTemplate>
                          <local:NativeCell Name="{Binding Name}" Category="{Binding Category}" ImageFilename="{Binding ImageFilename}" />
                      </DataTemplate>
                  </ListView.ItemTemplate>
              </ListView>
          </StackLayout>
      </ContentPage.Content>
</ContentPage>
```

Префикс пространства имен `local` может иметь любое имя. Тем не менее значения `clr-namespace` и `assembly` должны соответствовать данным пользовательского элемента управления. После объявления пространства имен префикс используется для ссылки на пользовательскую ячейку.

В следующем примере кода показано, как пользовательская ячейка `NativeCell` может использоваться страницей C#.

```csharp
public class NativeCellPageCS : ContentPage
{
    ListView listView;

    public NativeCellPageCS()
    {
        listView = new ListView(ListViewCachingStrategy.RecycleElement)
        {
            ItemsSource = DataSource.GetList(),
            ItemTemplate = new DataTemplate(() =>
            {
                var nativeCell = new NativeCell();
                nativeCell.SetBinding(NativeCell.NameProperty, "Name");
                nativeCell.SetBinding(NativeCell.CategoryProperty, "Category");
                nativeCell.SetBinding(NativeCell.ImageFilenameProperty, "ImageFilename");

                return nativeCell;
            })
        };

        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
                Padding = new Thickness(0, 20, 0, 0);
                break;
            case Device.Android:
            case Device.UWP:
                Padding = new Thickness(0);
                break;
        }

        Content = new StackLayout
        {
            Children = {
                new Label { Text = "Xamarin.Forms native cell", HorizontalTextAlignment = TextAlignment.Center },
                listView
            }
        };
        listView.ItemSelected += OnItemSelected;
    }
    ...
}
```

Элемент управления [`ListView`](xref:Xamarin.Forms.ListView) Xamarin.Forms используется для отображения списка данных, который заполняется с помощью свойства [`ItemSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource). Стратегия кэширования [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) пытается свести к минимуму объем занимаемой памяти `ListView` и время выполнения путем повторного использования ячеек списка. Дополнительные сведения см. в разделе [Стратегия кэширования](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy).

Каждая строка в списке содержит три элемента данных — имя, категорию и имя файла изображения. Макет каждой строки в списке определяется `DataTemplate` со ссылкой через привязываемое свойство [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate). `DataTemplate` определяет, что каждая строка данных в списке будет ячейкой `NativeCell`, отображающей свойства `Name`, `Category` и `ImageFilename` посредством привязки данных. Дополнительные сведения об элементе управления `ListView`см. в статье [ListView](~/xamarin-forms/user-interface/listview/index.md).

Теперь в каждый проект приложения можно добавить пользовательский отрисовщик для настройки определяемого платформой макета для каждой ячейки.

## <a name="creating-the-custom-renderer-on-each-platform"></a>Создание пользовательского отрисовщика на каждой платформе

Процесс создания класса пользовательского отрисовщика выглядит следующим образом:

1. Создайте подкласс класса `ViewCellRenderer`, который отрисовывает пользовательскую ячейку.
1. Переопределите зависящий от платформы метод, который отрисовывает пользовательскую ячейку, и напишите логику для его настройки.
1. Добавьте атрибут `ExportRenderer` в класс настраиваемого отрисовщика, чтобы указать, что он будет использоваться для отрисовки пользовательской ячейки Xamarin.Forms. Этот атрибут используется для регистрации пользовательского отрисовщика в Xamarin.Forms.

> [!NOTE]
> Для большинства элементов Xamarin.Forms предоставлять пользовательский отрисовщик в проекте для каждой платформы необязательно. Если пользовательский отрисовщик не зарегистрирован, будет использоваться отрисовщик по умолчанию для базового класса элемента управления. Однако пользовательские отрисовщики необходимы в каждом зависящем от платформы проекте при отрисовке элемента [ViewCell](xref:Xamarin.Forms.ViewCell).

На следующей схеме показаны обязанности каждого проекта в примере приложения, а также связи между ними:

![](viewcell-images/solution-structure.png "NativeCell Custom Renderer Project Responsibilities")

Пользовательская ячейка `NativeCell` отрисовывается с помощью зависящих от платформы классов отрисовщика, которые являются производными от класса `ViewCellRenderer` каждой платформы. Это приводит к тому, что каждая пользовательская ячейка `NativeCell` отрисовывается с помощью зависящих от платформы макетов, как показано на следующих снимках экрана.

![](viewcell-images/screenshots.png "NativeCell on each Platform")

Класс `ViewCellRenderer` предоставляет зависящие от платформы методы для отрисовки пользовательской ячейки. Метод `GetCell` предназначен для платформы iOS, метод `GetCellCore` — для платформы Android, а метод `GetTemplate` — для универсальной платформы Windows.

Каждый класс пользовательского отрисовщика дополняется атрибутом `ExportRenderer`, который регистрирует отрисовщик в Xamarin.Forms. Атрибут принимает два параметра — имя типа отрисовываемой ячейки Xamarin.Forms и имя типа пользовательского отрисовщика. Префикс `assembly` в атрибуте указывает, что атрибут применяется ко всей сборке.

В следующих разделах рассматривается реализация класса пользовательского отрисовщика для каждой платформы.

### <a name="creating-the-custom-renderer-on-ios"></a>Создание пользовательского отрисовщика в iOS

В следующем примере кода показан пользовательский отрисовщик для платформы iOS:

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeiOSCellRenderer))]
namespace CustomRenderer.iOS
{
    public class NativeiOSCellRenderer : ViewCellRenderer
    {
        NativeiOSCell cell;

        public override UITableViewCell GetCell(Cell item, UITableViewCell reusableCell, UITableView tv)
        {
            var nativeCell = (NativeCell)item;

            cell = reusableCell as NativeiOSCell;
            if (cell == null)
                cell = new NativeiOSCell(item.GetType().FullName, nativeCell);
            else
                cell.NativeCell.PropertyChanged -= OnNativeCellPropertyChanged;

            nativeCell.PropertyChanged += OnNativeCellPropertyChanged;
            cell.UpdateCell(nativeCell);
            return cell;
        }
        ...
    }
}
```

Метод `GetCell` вызывается для создания каждой отображаемой ячейки. Каждая ячейка является экземпляром элемента `NativeiOSCell`, который определяет макет ячейки и ее данные. Работа метода `GetCell` зависит от стратегии кэширования [`ListView`](xref:Xamarin.Forms.ListView).

- Если стратегией кэширования [`ListView`](xref:Xamarin.Forms.ListView) является [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement), метод `GetCell` будет вызываться для каждой ячейки. Экземпляр `NativeiOSCell` будет создан для каждого экземпляра `NativeCell`, который отображается на экране. Когда пользователь прокручивает `ListView`, экземпляры `NativeiOSCell` будут использоваться повторно. Дополнительные сведения о повторном использовании ячеек в iOS см. в разделе [Повторное использование ячеек](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

  > [!NOTE]
  > Этот код пользовательского отрисовщика выполнит повторное использование некоторых ячеек, даже если для [`ListView`](xref:Xamarin.Forms.ListView) задано сохранение ячеек.

  Данные, отображаемые каждым экземпляром `NativeiOSCell`, созданным или повторно используемым, будут заменены данными из каждого экземпляра `NativeCell` методом `UpdateCell`.

  > [!NOTE]
  > Метод `OnNativeCellPropertyChanged` никогда не будет вызываться, если в качестве стратегии кэширования [`ListView`](xref:Xamarin.Forms.ListView) задано сохранение ячеек.

- Если стратегией кэширования [`ListView`](xref:Xamarin.Forms.ListView) является [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement), метод `GetCell` будет вызываться для каждой ячейки, которая изначально отображалась на экране. Экземпляр `NativeiOSCell` будет создан для каждого экземпляра `NativeCell`, который отображается на экране. Данные, отображаемые каждым экземпляром `NativeiOSCell`, будут заменены данными из экземпляра `NativeCell` методом `UpdateCell`. Тем не менее метод `GetCell` не будет вызываться, когда пользователь прокручивает `ListView`. Вместо этого экземпляры `NativeiOSCell` будут использоваться повторно. События `PropertyChanged` будут вызваны для экземпляра `NativeCell` при изменении его данных, и обработчик событий `OnNativeCellPropertyChanged` обновит данные в каждом повторно используемом экземпляре `NativeiOSCell`.

В следующем примере кода показан метод `OnNativeCellPropertyChanged`, который вызывается при вызове события `PropertyChanged`.

```csharp
namespace CustomRenderer.iOS
{
    public class NativeiOSCellRenderer : ViewCellRenderer
    {
        ...

        void OnNativeCellPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            var nativeCell = (NativeCell)sender;
            if (e.PropertyName == NativeCell.NameProperty.PropertyName)
            {
                cell.HeadingLabel.Text = nativeCell.Name;
            }
            else if (e.PropertyName == NativeCell.CategoryProperty.PropertyName)
            {
                cell.SubheadingLabel.Text = nativeCell.Category;
            }
            else if (e.PropertyName == NativeCell.ImageFilenameProperty.PropertyName)
            {
                cell.CellImageView.Image = cell.GetImage(nativeCell.ImageFilename);
            }
        }
    }
}
```

Этот метод обновляет данные, отображаемые повторно используемыми экземплярами `NativeiOSCell`. Выполняется проверка изменяемого свойства, так как метод может вызываться несколько раз.

Класс `NativeiOSCell` определяет макет для каждой ячейки и показан в следующем примере кода.

```csharp
internal class NativeiOSCell : UITableViewCell, INativeElementView
{
  public UILabel HeadingLabel { get; set; }
  public UILabel SubheadingLabel { get; set; }
  public UIImageView CellImageView { get; set; }

  public NativeCell NativeCell { get; private set; }
  public Element Element => NativeCell;

  public NativeiOSCell(string cellId, NativeCell cell) : base(UITableViewCellStyle.Default, cellId)
  {
    NativeCell = cell;

    SelectionStyle = UITableViewCellSelectionStyle.Gray;
    ContentView.BackgroundColor = UIColor.FromRGB(255, 255, 224);
    CellImageView = new UIImageView();

    HeadingLabel = new UILabel()
    {
      Font = UIFont.FromName("Cochin-BoldItalic", 22f),
      TextColor = UIColor.FromRGB(127, 51, 0),
      BackgroundColor = UIColor.Clear
    };

    SubheadingLabel = new UILabel()
    {
      Font = UIFont.FromName("AmericanTypewriter", 12f),
      TextColor = UIColor.FromRGB(38, 127, 0),
      TextAlignment = UITextAlignment.Center,
      BackgroundColor = UIColor.Clear
    };

    ContentView.Add(HeadingLabel);
    ContentView.Add(SubheadingLabel);
    ContentView.Add(CellImageView);
  }

  public void UpdateCell(NativeCell cell)
  {
    HeadingLabel.Text = cell.Name;
    SubheadingLabel.Text = cell.Category;
    CellImageView.Image = GetImage(cell.ImageFilename);
  }

  public UIImage GetImage(string filename)
  {
    return (!string.IsNullOrWhiteSpace(filename)) ? UIImage.FromFile("Images/" + filename + ".jpg") : null;
  }

  public override void LayoutSubviews()
  {
    base.LayoutSubviews();

    HeadingLabel.Frame = new CGRect(5, 4, ContentView.Bounds.Width - 63, 25);
    SubheadingLabel.Frame = new CGRect(100, 18, 100, 20);
    CellImageView.Frame = new CGRect(ContentView.Bounds.Width - 63, 5, 33, 33);
  }
}
```

Этот класс определяет элементы управления, используемые для отрисовки содержимого ячейки и макета. Этот класс реализует интерфейс [`INativeElementView`](xref:Xamarin.Forms.INativeElementView), который является обязательным, если [`ListView`](xref:Xamarin.Forms.ListView) использует стратегию кэширования [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement). Этот интерфейс указывает, что класс должен реализовывать свойство [`Element`](xref:Xamarin.Forms.INativeElementView.Element), которое должно возвращать данные пользовательской ячейки для повторно используемых ячеек.

Конструктор `NativeiOSCell` инициализирует внешний вид свойств `HeadingLabel`, `SubheadingLabel` и `CellImageView`. Эти свойства используются для отображения данных, хранящихся в экземпляре `NativeCell`, с помощью метода `UpdateCell`, вызываемого для задания значения каждого свойства. Кроме того, когда [`ListView`](xref:Xamarin.Forms.ListView) использует стратегию кэширования [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement), данные, отображаемые свойствами `HeadingLabel`, `SubheadingLabel` и `CellImageView`, могут быть обновлены методом `OnNativeCellPropertyChanged` в пользовательском отрисовщике.

Макет ячейки осуществляется переопределением `LayoutSubviews`, которое задает координаты `HeadingLabel`, `SubheadingLabel` и `CellImageView` в ячейке.

### <a name="creating-the-custom-renderer-on-android"></a>Создание пользовательского отрисовщика в Android

В следующем примере кода показан пользовательский отрисовщик для платформы Android:

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeAndroidCellRenderer))]
namespace CustomRenderer.Droid
{
    public class NativeAndroidCellRenderer : ViewCellRenderer
    {
        NativeAndroidCell cell;

        protected override Android.Views.View GetCellCore(Cell item, Android.Views.View convertView, ViewGroup parent, Context context)
        {
            var nativeCell = (NativeCell)item;
            Console.WriteLine("\t\t" + nativeCell.Name);

            cell = convertView as NativeAndroidCell;
            if (cell == null)
            {
                cell = new NativeAndroidCell(context, nativeCell);
            }
            else
            {
                cell.NativeCell.PropertyChanged -= OnNativeCellPropertyChanged;
            }

            nativeCell.PropertyChanged += OnNativeCellPropertyChanged;

            cell.UpdateCell(nativeCell);
            return cell;
        }
        ...
    }
}
```

Метод `GetCellCore` вызывается для создания каждой отображаемой ячейки. Каждая ячейка является экземпляром элемента `NativeAndroidCell`, который определяет макет ячейки и ее данные. Работа метода `GetCellCore` зависит от стратегии кэширования [`ListView`](xref:Xamarin.Forms.ListView).

- Если стратегией кэширования [`ListView`](xref:Xamarin.Forms.ListView) является [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement), метод `GetCellCore` будет вызываться для каждой ячейки. `NativeAndroidCell` будет создан для каждого экземпляра `NativeCell`, который отображается на экране. Когда пользователь прокручивает `ListView`, экземпляры `NativeAndroidCell` будут использоваться повторно. Дополнительные сведения о повторном использовании ячеек в Android см. в разделе [Повторное использование представления строк](~/android/user-interface/layouts/list-view/populating.md).

  > [!NOTE]
  > Учтите, что этот код пользовательского отрисовщика выполнит повторное использование некоторых ячеек, даже если для [`ListView`](xref:Xamarin.Forms.ListView) задано сохранение ячеек.

  Данные, отображаемые каждым экземпляром `NativeAndroidCell`, созданным или повторно используемым, будут заменены данными из каждого экземпляра `NativeCell` методом `UpdateCell`.

  > [!NOTE]
  > Обратите внимание, что, хотя метод `OnNativeCellPropertyChanged` будет вызываться, если в качестве стратегии [`ListView`](xref:Xamarin.Forms.ListView) задано сохранение ячеек, он не будет обновлять значения свойства `NativeAndroidCell`.

- Если стратегией кэширования [`ListView`](xref:Xamarin.Forms.ListView) является [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement), метод `GetCellCore` будет вызываться для каждой ячейки, которая изначально отображалась на экране. Экземпляр `NativeAndroidCell` будет создан для каждого экземпляра `NativeCell`, который отображается на экране. Данные, отображаемые каждым экземпляром `NativeAndroidCell`, будут заменены данными из экземпляра `NativeCell` методом `UpdateCell`. Тем не менее метод `GetCellCore` не будет вызываться, когда пользователь прокручивает `ListView`. Вместо этого экземпляры `NativeAndroidCell` будут использоваться повторно.  События `PropertyChanged` будут вызваны для экземпляра `NativeCell` при изменении его данных, и обработчик событий `OnNativeCellPropertyChanged` обновит данные в каждом повторно используемом экземпляре `NativeAndroidCell`.

В следующем примере кода показан метод `OnNativeCellPropertyChanged`, который вызывается при вызове события `PropertyChanged`.

```csharp
namespace CustomRenderer.Droid
{
    public class NativeAndroidCellRenderer : ViewCellRenderer
    {
        ...

        void OnNativeCellPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            var nativeCell = (NativeCell)sender;
            if (e.PropertyName == NativeCell.NameProperty.PropertyName)
            {
                cell.HeadingTextView.Text = nativeCell.Name;
            }
            else if (e.PropertyName == NativeCell.CategoryProperty.PropertyName)
            {
                cell.SubheadingTextView.Text = nativeCell.Category;
            }
            else if (e.PropertyName == NativeCell.ImageFilenameProperty.PropertyName)
            {
                cell.SetImage(nativeCell.ImageFilename);
            }
        }
    }
}
```

Этот метод обновляет данные, отображаемые повторно используемыми экземплярами `NativeAndroidCell`. Выполняется проверка изменяемого свойства, так как метод может вызываться несколько раз.

Класс `NativeAndroidCell` определяет макет для каждой ячейки и показан в следующем примере кода.

```csharp
internal class NativeAndroidCell : LinearLayout, INativeElementView
{
  public TextView HeadingTextView { get; set; }
  public TextView SubheadingTextView { get; set; }
  public ImageView ImageView { get; set; }

  public NativeCell NativeCell { get; private set; }
  public Element Element => NativeCell;

  public NativeAndroidCell(Context context, NativeCell cell) : base(context)
  {
    NativeCell = cell;

    var view = (context as Activity).LayoutInflater.Inflate(Resource.Layout.NativeAndroidCell, null);
    HeadingTextView = view.FindViewById<TextView>(Resource.Id.HeadingText);
    SubheadingTextView = view.FindViewById<TextView>(Resource.Id.SubheadingText);
    ImageView = view.FindViewById<ImageView>(Resource.Id.Image);

    AddView(view);
  }

  public void UpdateCell(NativeCell cell)
  {
    HeadingTextView.Text = cell.Name;
    SubheadingTextView.Text = cell.Category;

    // Dispose of the old image
    if (ImageView.Drawable != null)
    {
      using (var image = ImageView.Drawable as BitmapDrawable)
      {
        if (image != null)
        {
          if (image.Bitmap != null)
          {
            image.Bitmap.Dispose();
          }
        }
      }
    }

    SetImage(cell.ImageFilename);
  }

  public void SetImage(string filename)
  {
    if (!string.IsNullOrWhiteSpace(filename))
    {
      // Display new image
      Context.Resources.GetBitmapAsync(filename).ContinueWith((t) =>
      {
        var bitmap = t.Result;
        if (bitmap != null)
        {
          ImageView.SetImageBitmap(bitmap);
          bitmap.Dispose();
        }
      }, TaskScheduler.FromCurrentSynchronizationContext());
    }
    else
    {
      // Clear the image
      ImageView.SetImageBitmap(null);
    }
  }
}
```

Этот класс определяет элементы управления, используемые для отрисовки содержимого ячейки и макета. Этот класс реализует интерфейс [`INativeElementView`](xref:Xamarin.Forms.INativeElementView), который является обязательным, если [`ListView`](xref:Xamarin.Forms.ListView) использует стратегию кэширования [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement). Этот интерфейс указывает, что класс должен реализовывать свойство [`Element`](xref:Xamarin.Forms.INativeElementView.Element), которое должно возвращать данные пользовательской ячейки для повторно используемых ячеек.

Конструктор `NativeAndroidCell` расширяет макет `NativeAndroidCell` и инициализирует свойства `HeadingTextView`, `SubheadingTextView` и `ImageView` в элементы управления в расширенном макете. Эти свойства используются для отображения данных, хранящихся в экземпляре `NativeCell`, с помощью метода `UpdateCell`, вызываемого для задания значения каждого свойства. Кроме того, когда [`ListView`](xref:Xamarin.Forms.ListView) использует стратегию кэширования [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement), данные, отображаемые свойствами `HeadingTextView`, `SubheadingTextView` и `ImageView`, могут быть обновлены методом `OnNativeCellPropertyChanged` в пользовательском отрисовщике.

В следующем примере кода показано определение макета для файла макета `NativeAndroidCell.axml`.

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="8dp"
    android:background="@drawable/CustomSelector">
    <LinearLayout
        android:id="@+id/Text"
        android:orientation="vertical"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:paddingLeft="10dip">
        <TextView
            android:id="@+id/HeadingText"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textColor="#FF7F3300"
            android:textSize="20dip"
            android:textStyle="italic" />
        <TextView
            android:id="@+id/SubheadingText"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="14dip"
            android:textColor="#FF267F00"
            android:paddingLeft="100dip" />
    </LinearLayout>
    <ImageView
        android:id="@+id/Image"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:padding="5dp"
        android:src="@drawable/icon"
        android:layout_alignParentRight="true" />
</RelativeLayout>
```

Этот макет указывает, что два элемента управления `TextView` и элемент управления `ImageView` используются для отображения содержимого ячейки. Два элемента управления `TextView` расположены вертикально в элементе управления `LinearLayout`, и все элементы управления содержатся в `RelativeLayout`.

### <a name="creating-the-custom-renderer-on-uwp"></a>Создание пользовательского отрисовщика на универсальной платформе Windows

В следующем примере кода показан пользовательский отрисовщик для платформы UWP:

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeUWPCellRenderer))]
namespace CustomRenderer.UWP
{
    public class NativeUWPCellRenderer : ViewCellRenderer
    {
        public override Windows.UI.Xaml.DataTemplate GetTemplate(Cell cell)
        {
            return App.Current.Resources["ListViewItemTemplate"] as Windows.UI.Xaml.DataTemplate;
        }
    }
}
```

Метод `GetTemplate` вызывается для возвращения ячейки для отрисовки для каждой строки данных в списке. Он создает `DataTemplate` для каждого экземпляра `NativeCell`, который будет отображаться на экране с элементом `DataTemplate`, определяющим внешний вид и содержимое ячейки.

`DataTemplate` хранится в словаре ресурсов на уровне приложения и приводится в следующем примере кода.

```xaml
<DataTemplate x:Key="ListViewItemTemplate">
    <Grid Background="LightYellow">
        <Grid.Resources>
            <local:ConcatImageExtensionConverter x:Name="ConcatImageExtensionConverter" />
        </Grid.Resources>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.40*" />
            <ColumnDefinition Width="0.40*"/>
            <ColumnDefinition Width="0.20*" />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.ColumnSpan="2" Foreground="#7F3300" FontStyle="Italic" FontSize="22" VerticalAlignment="Top" Text="{Binding Name}" />
        <TextBlock Grid.RowSpan="2" Grid.Column="1" Foreground="#267F00" FontWeight="Bold" FontSize="12" VerticalAlignment="Bottom" Text="{Binding Category}" />
        <Image Grid.RowSpan="2" Grid.Column="2" HorizontalAlignment="Left" VerticalAlignment="Center" Source="{Binding ImageFilename, Converter={StaticResource ConcatImageExtensionConverter}}" Width="50" Height="50" />
        <Line Grid.Row="1" Grid.ColumnSpan="3" X1="0" X2="1" Margin="30,20,0,0" StrokeThickness="1" Stroke="LightGray" Stretch="Fill" VerticalAlignment="Bottom" />
    </Grid>
</DataTemplate>
```

`DataTemplate` определяет элементы управления, используемые для отображения содержимого ячейки, его макета и внешнего вида. Два элемента управления `TextBlock` и элемент управления `Image` используются для отображения содержимого ячейки с помощью привязки данных. Кроме того, экземпляр `ConcatImageExtensionConverter` используется для сцепления расширения файла `.jpg` с каждым именем файла изображения. Это гарантирует, что элемент управления `Image` может загрузить и отрисовать изображение, когда его свойство `Source` задано.

## <a name="summary"></a>Сводка

В этой статье показано, как создать пользовательский отрисовщик для [`ViewCell`](xref:Xamarin.Forms.ViewCell), размещенного в элементе управления Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView). Это предотвратит постоянные вызовы вычисления макета Xamarin.Forms во время прокрутки `ListView`.

## <a name="related-links"></a>Связанные ссылки

- [Производительность элемента управления ListView](~/xamarin-forms/user-interface/listview/performance.md)
- [CustomRendererViewCell (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-viewcell)
