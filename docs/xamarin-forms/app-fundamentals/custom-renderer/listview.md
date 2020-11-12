---
title: Настройка ListView
description: Xamarin.Forms ListView — это представление, в котором коллекция данных отображается в виде вертикального списка. В этой статье показано, как создать пользовательское средство отрисовки, содержащее зависящий от платформы список элементов управления и собственные макеты ячеек, предоставляя больше контроля над производительностью списка собственных элементов управления.
ms.prod: xamarin
ms.assetid: 2FBCB8C8-4F32-45E7-954F-63AD29D5F1B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c2ae9d7bd2195e48fede30082b70f8a1a6d49ac3
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93367378"
---
# <a name="customizing-a-listview"></a>Настройка ListView

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/customrenderers-listview)

_Xamarin.Forms ListView — это представление, в котором коллекция данных отображается в виде вертикального списка. В этой статье показано, как создать пользовательское средство отрисовки, содержащее зависящий от платформы список элементов управления и собственные макеты ячеек, предоставляя больше контроля над производительностью списка собственных элементов управления._

Каждое представление Xamarin.Forms имеет сопутствующий отрисовщик для каждой платформы, который создает экземпляр собственного элемента управления. Когда [`ListView`](xref:Xamarin.Forms.ListView) отображается в приложении Xamarin.Forms, на платформе iOS создается класс `ListViewRenderer`, который, в свою очередь, создает собственный элемент управления `UITableView`. На платформе Android класс `ListViewRenderer` создает собственный элемент управления `ListView`. На универсальной платформе Windows (UWP) класс `ListViewRenderer` создает экземпляр собственного элемента управления `ListView`. Дополнительные сведения об отрисовщике и классах собственных элементов управления, с которыми сопоставляются элементы управления Xamarin.Forms, см. в статье [Базовые классы и собственные элементы управления отрисовщика](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

На следующей схеме показана связь между элементом управления [`ListView`](xref:Xamarin.Forms.ListView) и соответствующими собственными элементами управления, которые его реализуют:

![Связь между элементом управления ListView и реализацией нативных элементов управления](listview-images/listview-classes.png)

Процесс отрисовки можно использовать для реализации настроек конкретных платформ путем создания пользовательского отрисовщика для [`ListView`](xref:Xamarin.Forms.ListView) на каждой платформе. Этот процесс выглядит следующим образом:

1. [Создание](#creating-the-custom-listview-control) пользовательского элемента управления Xamarin.Forms.
1. [Использование](#consuming-the-custom-control) пользовательского элемента управления в Xamarin.Forms.
1. [Создание](#creating-the-custom-renderer-on-each-platform) пользовательского отрисовщика для элемента управления на каждой платформе.

Мы обсудим каждый элемент по очереди, чтобы реализовать отрисовщик `NativeListView`, в котором используются преимущества зависящих от платформы элементов управления "Список" и собственных макетов ячеек. Этот сценарий полезен при переносе существующего собственного приложения, содержащего код списка и ячеек, который можно использовать повторно. Кроме того, он обеспечивает детальную настройку возможностей элемента управления "Список", которые могут повлиять на производительность, например виртуализацию данных.

## <a name="creating-the-custom-listview-control"></a>Создание пользовательского элемента управления ListView

Пользовательский элемент управления [`ListView`](xref:Xamarin.Forms.ListView) можно создать путем создания подклассов класса `ListView`, как показано в следующем примере кода.

```csharp
public class NativeListView : ListView
{
  public static readonly BindableProperty ItemsProperty =
    BindableProperty.Create ("Items", typeof(IEnumerable<DataSource>), typeof(NativeListView), new List<DataSource> ());

  public IEnumerable<DataSource> Items {
    get { return (IEnumerable<DataSource>)GetValue (ItemsProperty); }
    set { SetValue (ItemsProperty, value); }
  }

  public event EventHandler<SelectedItemChangedEventArgs> ItemSelected;

  public void NotifyItemSelected (object item)
  {
    if (ItemSelected != null) {
      ItemSelected (this, new SelectedItemChangedEventArgs (item));
    }
  }
}
```

`NativeListView` создается в проекте библиотеки .NET Standard и определяет API для пользовательского элемента управления. Этот элемент управления предоставляет свойство `Items`, используемое для заполнения `ListView` данными, к которому можно привязать данные для отображения. Он также предоставляет событие `ItemSelected`, которое будет срабатывать каждый раз, когда элемент выбирается в зависящем от платформы собственном элементе управления "Список". Дополнительные сведения о привязке данных см. в статье [Основы привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

## <a name="consuming-the-custom-control"></a>Использование пользовательского элемента управления

На пользовательский элемент управления `NativeListView` можно ссылаться в XAML в проекте библиотеки .NET Standard, объявив пространство имен для его расположения и используя префикс пространства имен в элементе управления. В следующем примере кода показано, как пользовательский элемент управления `NativeListView` может использоваться страницей XAML.

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <ContentPage.Content>
          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*" />
            </Grid.RowDefinitions>
          <Label Text="{x:Static local:App.Description}" HorizontalTextAlignment="Center" />
            <local:NativeListView Grid.Row="1" x:Name="nativeListView" ItemSelected="OnItemSelected" VerticalOptions="FillAndExpand" />
          </Grid>
      </ContentPage.Content>
</ContentPage>
```

Префикс пространства имен `local` может иметь любое имя. Тем не менее значения `clr-namespace` и `assembly` должны соответствовать данным пользовательского элемента управления. После объявления пространства имен префикс используется для ссылки на пользовательский элемент управления.

В следующем примере кода показано, как пользовательский элемент управления `NativeListView` может использоваться страницей C#.

```csharp
public class MainPageCS : ContentPage
{
    NativeListView nativeListView;

    public MainPageCS()
    {
        nativeListView = new NativeListView
        {
            Items = DataSource.GetList(),
            VerticalOptions = LayoutOptions.FillAndExpand
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

        Content = new Grid
        {
            RowDefinitions = {
                new RowDefinition { Height = GridLength.Auto },
                new RowDefinition { Height = new GridLength (1, GridUnitType.Star) }
            },
            Children = {
                new Label { Text = App.Description, HorizontalTextAlignment = TextAlignment.Center },
                nativeListView
            }
        };
        nativeListView.ItemSelected += OnItemSelected;
    }
    ...
}
```

Пользовательский элемент управления `NativeListView` использует зависящие от платформы пользовательские отрисовщики для отображения списка данных, который заполняется с помощью свойства `Items`. Каждая строка в списке содержит три элемента данных — имя, категорию и имя файла изображения. Макет каждой строки в списке определяется зависящим от платформы пользовательским отрисовщиком.

> [!NOTE]
> Так как пользовательский элемент управления `NativeListView` отрисовывается с помощью зависящих от платформы элементов управления "Список", которые включают возможность прокрутки, пользовательский элемент управления не должен размещаться в элементах управления макета с возможностью прокрутки, например [`ScrollView`](xref:Xamarin.Forms.ScrollView).

Пользовательский отрисовщик теперь можно добавлять в каждый проект приложения для создания зависящих от платформы элементов управления "Список" и собственных макетов ячеек.

## <a name="creating-the-custom-renderer-on-each-platform"></a>Создание пользовательского отрисовщика на каждой платформе

Процесс создания класса пользовательского отрисовщика выглядит следующим образом:

1. Создайте подкласс класса `ListViewRenderer`, который отрисовывает пользовательский элемент управления.
1. Переопределите метод `OnElementChanged`, который отрисовывает пользовательский элемент управления, и напишите логику для его настройки. Этот метод вызывается, когда создается соответствующий Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView).
1. Добавьте атрибут `ExportRenderer` в класс настраиваемого отрисовщика, чтобы указать, что он будет использоваться для отрисовки пользовательского элемента управления Xamarin.Forms. Этот атрибут используется для регистрации пользовательского отрисовщика в Xamarin.Forms.

> [!NOTE]
> Предоставлять пользовательский отрисовщик в проекте для каждой платформы не обязательно. Если пользовательский отрисовщик не зарегистрирован, будет использоваться отрисовщик по умолчанию для базового класса ячейки.

На следующей схеме показаны обязанности каждого проекта в примере приложения, а также связи между ними:

![Задачи проекта пользовательского отрисовщика NativeListView](listview-images/solution-structure.png)

Пользовательский элемент управления `NativeListView` отрисовывается с помощью зависящих от платформы классов отрисовщика, которые являются производными от класса `ListViewRenderer` каждой платформы. Это приводит к тому, что каждый пользовательский элемент управления `NativeListView` отрисовывается с помощью зависящих от платформы элементов управления "Список" и собственных макетов ячеек, как показано на следующих снимках экрана.

![NativeListView на каждой платформе](listview-images/screenshots.png)

Класс `ListViewRenderer` предоставляет метод `OnElementChanged`, который вызывается при создании пользовательского элемента управления Xamarin.Forms для отрисовки соответствующего собственного элемента управления. Этот метод принимает параметр `ElementChangedEventArgs`, содержащий свойства `OldElement` и `NewElement`. Эти свойства представляют элемент Xamarin.Forms, к которому *был* присоединен отрисовщик, и элемент Xamarin.Forms, к которому *присоединен* отрисовщик, соответственно. В примере приложения свойство `OldElement` будет иметь значение `null`, а свойство `NewElement` будет содержать ссылку на экземпляр `NativeListView`.

Настройка собственного элемента управления выполняется в переопределенной версии метода `OnElementChanged` в классе отрисовщика для каждой платформы. Типизированную ссылку на собственный элемент управления, используемый на платформе, можно получить через свойство `Control`. Кроме того, ссылку на отрисовываемый элемент управления Xamarin.Forms можно получить с помощью свойства `Element`.

Будьте осторожны при подписке на обработчики событий в методе `OnElementChanged`, как показано в следующем примере кода.

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the native control and subscribe to event handlers
  }
}
```

Собственный элемент управления следует настраивать и подписывать на обработчики событий только при присоединении настраиваемого отрисовщика к новому элементу Xamarin.Forms. Аналогично от всех обработчиков событий, на которые были созданы подписки, следует отписываться только при изменении элемента с подключенным отрисовщиком. Реализовав этот подход, вы сможете создать пользовательский отрисовщик, на который не влияют утечки памяти.

Переопределенная версия метода `OnElementPropertyChanged` в классе отрисовщика, зависящем от платформы, отвечает на изменения привязываемого свойства в пользовательском элементе управления Xamarin.Forms. Следует всегда проверять наличие измененного свойства, так как это переопределение может вызываться многократно.

Каждый класс пользовательского отрисовщика дополняется атрибутом `ExportRenderer`, который регистрирует отрисовщик в Xamarin.Forms. Атрибут принимает два параметра — имя типа отрисовываемого пользовательского элемента управления Xamarin.Forms и имя типа пользовательского отрисовщика. Префикс `assembly` в атрибуте указывает, что атрибут применяется ко всей сборке.

В следующих разделах рассматривается реализация класса пользовательского отрисовщика для каждой платформы.

### <a name="creating-the-custom-renderer-on-ios"></a>Создание пользовательского отрисовщика в iOS

В следующем примере кода показан пользовательский отрисовщик для платформы iOS:

```csharp
[assembly: ExportRenderer (typeof(NativeListView), typeof(NativeiOSListViewRenderer))]
namespace CustomRenderer.iOS
{
    public class NativeiOSListViewRenderer : ListViewRenderer
    {
        protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null) {
                // Unsubscribe
            }

            if (e.NewElement != null) {
                Control.Source = new NativeiOSListViewSource (e.NewElement as NativeListView);
            }
        }
    }
}
```

Элемент управления `UITableView` настраивается путем создания экземпляра класса `NativeiOSListViewSource` при условии, что пользовательский отрисовщик подключен к новому элементу Xamarin.Forms. Этот класс предоставляет данные для элемента управления `UITableView` путем переопределения методов `RowsInSection` и `GetCell` из класса `UITableViewSource` и предоставления свойства `Items`, содержащего список данных для отображения. Этот класс также предоставляет переопределение метода `RowSelected`, которое вызывает событие `ItemSelected`, предоставленное пользовательским элементом управления `NativeListView`. Дополнительные сведения о переопределениях метода см. в разделе [Создание подклассов UITableViewSource](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). Метод `GetCell` возвращает `UITableCellView`, который заполняется данными для каждой строки в списке и показан в следующем примере кода.

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
  // request a recycled cell to save memory
  NativeiOSListViewCell cell = tableView.DequeueReusableCell (cellIdentifier) as NativeiOSListViewCell;

  // if there are no cells to reuse, create a new one
  if (cell == null) {
    cell = new NativeiOSListViewCell (cellIdentifier);
  }

  if (String.IsNullOrWhiteSpace (tableItems [indexPath.Row].ImageFilename)) {
    cell.UpdateCell (tableItems [indexPath.Row].Name
      , tableItems [indexPath.Row].Category
      , null);
  } else {
    cell.UpdateCell (tableItems [indexPath.Row].Name
      , tableItems [indexPath.Row].Category
      , UIImage.FromFile ("Images/" + tableItems [indexPath.Row].ImageFilename + ".jpg"));
  }

  return cell;
}
```

Этот метод создает экземпляр `NativeiOSListViewCell` для каждой строки данных, которая будет отображаться на экране. Экземпляр `NativeiOSCell` определяет макет каждой ячейки и данных ячейки. Если ячейка пропадает с экрана из-за прокрутки, ячейка будет доступна для повторного использования. Это позволяет сэкономить память и гарантировать, что присутствуют только экземпляры `NativeiOSCell` для данных, отображаемых на экране, а не для всех данных в списке. Дополнительные сведения о повторном использовании ячеек см. в разделе [Повторное использование ячеек](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). Метод `GetCell` также считывает свойство `ImageFilename` каждой строки данных при условии, что оно существует, считывает изображение и сохраняет его как экземпляр `UIImage`, прежде чем внести в экземпляр `NativeiOSListViewCell` новые данные (имя, категорию и изображение) для строки.

Класс `NativeiOSListViewCell` определяет макет для каждой ячейки и показан в следующем примере кода.

```csharp
public class NativeiOSListViewCell : UITableViewCell
{
  UILabel headingLabel, subheadingLabel;
  UIImageView imageView;

  public NativeiOSListViewCell (NSString cellId) : base (UITableViewCellStyle.Default, cellId)
  {
    SelectionStyle = UITableViewCellSelectionStyle.Gray;

    ContentView.BackgroundColor = UIColor.FromRGB (218, 255, 127);

    imageView = new UIImageView ();

    headingLabel = new UILabel () {
      Font = UIFont.FromName ("Cochin-BoldItalic", 22f),
      TextColor = UIColor.FromRGB (127, 51, 0),
      BackgroundColor = UIColor.Clear
    };

    subheadingLabel = new UILabel () {
      Font = UIFont.FromName ("AmericanTypewriter", 12f),
      TextColor = UIColor.FromRGB (38, 127, 0),
      TextAlignment = UITextAlignment.Center,
      BackgroundColor = UIColor.Clear
    };

    ContentView.Add (headingLabel);
    ContentView.Add (subheadingLabel);
    ContentView.Add (imageView);
  }

  public void UpdateCell (string caption, string subtitle, UIImage image)
  {
    headingLabel.Text = caption;
    subheadingLabel.Text = subtitle;
    imageView.Image = image;
  }

  public override void LayoutSubviews ()
  {
    base.LayoutSubviews ();

    headingLabel.Frame = new CoreGraphics.CGRect (5, 4, ContentView.Bounds.Width - 63, 25);
    subheadingLabel.Frame = new CoreGraphics.CGRect (100, 18, 100, 20);
    imageView.Frame = new CoreGraphics.CGRect (ContentView.Bounds.Width - 63, 5, 33, 33);
  }
}
```

Этот класс определяет элементы управления, используемые для отрисовки содержимого ячейки и макета. Конструктор `NativeiOSListViewCell` создает экземпляры элементов управления `UILabel` и `UIImageView` и инициализирует их появление. Эти элементы управления используются для отображения каждой строки данных с помощью метода `UpdateCell`, который используется для настройки этих данных в экземплярах `UILabel` и `UIImageView`. Расположение этих экземпляров задается переопределенным методом `LayoutSubviews` путем указания их координат в ячейке.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Реагирование на изменение свойства в пользовательском элементе управления

Если свойство `NativeListView.Items` меняется в связи с добавлением или удалением элементов из списка, пользовательский отрисовщик должен отреагировать и отобразить эти изменения. Это можно сделать путем переопределения метода `OnElementPropertyChanged`, как показано в следующем примере кода.

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Source = new NativeiOSListViewSource (Element as NativeListView);
  }
}
```

Метод создает новый экземпляр класса `NativeiOSListViewSource`, который предоставляет данные для элемента управления `UITableView`, при условии что привязываемое свойство `NativeListView.Items` изменилось.

### <a name="creating-the-custom-renderer-on-android"></a>Создание пользовательского отрисовщика в Android

В следующем примере кода показан пользовательский отрисовщик для платформы Android:

```csharp
[assembly: ExportRenderer(typeof(NativeListView), typeof(NativeAndroidListViewRenderer))]
namespace CustomRenderer.Droid
{
    public class NativeAndroidListViewRenderer : ListViewRenderer
    {
        Context _context;

        public NativeAndroidListViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // unsubscribe
                Control.ItemClick -= OnItemClick;
            }

            if (e.NewElement != null)
            {
                // subscribe
                Control.Adapter = new NativeAndroidListViewAdapter(_context as Android.App.Activity, e.NewElement as NativeListView);
                Control.ItemClick += OnItemClick;
            }
        }
        ...

        void OnItemClick(object sender, Android.Widget.AdapterView.ItemClickEventArgs e)
        {
            ((NativeListView)Element).NotifyItemSelected(((NativeListView)Element).Items.ToList()[e.Position - 1]);
        }
    }
}
```

Собственный элемент управления `ListView` настраивается, если пользовательский отрисовщик подключен к новому элементу Xamarin.Forms. Эта конфигурация требует создания экземпляра класса `NativeAndroidListViewAdapter`, который предоставляет данные собственному элементу управления `ListView`, и регистрации обработчика событий для обработки события `ItemClick`. В свою очередь, этот обработчик вызывает событие `ItemSelected`, предоставляемое пользовательским элементом управления `NativeListView`. Подписка на событие `ItemClick` отменяется, если изменяется элемент Xamarin.Forms, к которому подключен отрисовщик.

`NativeAndroidListViewAdapter` является производным от класса `BaseAdapter` и предоставляет свойство `Items`, содержащее список данных для отображения, а также переопределение методов `Count`, `GetView`, `GetItemId` и `this[int]`. Дополнительные сведения о переопределении этих методов см. в разделе [Реализация ListAdapter](~/android/user-interface/layouts/list-view/populating.md). Метод `GetView` возвращает представление для каждой строки, заполненной данными, и показан в следующем примере кода.

```csharp
public override View GetView (int position, View convertView, ViewGroup parent)
{
  var item = tableItems [position];

  var view = convertView;
  if (view == null) {
    // no view to re-use, create new
    view = context.LayoutInflater.Inflate (Resource.Layout.NativeAndroidListViewCell, null);
  }
  view.FindViewById<TextView> (Resource.Id.Text1).Text = item.Name;
  view.FindViewById<TextView> (Resource.Id.Text2).Text = item.Category;

  // grab the old image and dispose of it
  if (view.FindViewById<ImageView> (Resource.Id.Image).Drawable != null) {
    using (var image = view.FindViewById<ImageView> (Resource.Id.Image).Drawable as BitmapDrawable) {
      if (image != null) {
        if (image.Bitmap != null) {
          //image.Bitmap.Recycle ();
          image.Bitmap.Dispose ();
        }
      }
    }
  }

  // If a new image is required, display it
  if (!String.IsNullOrWhiteSpace (item.ImageFilename)) {
    context.Resources.GetBitmapAsync (item.ImageFilename).ContinueWith ((t) => {
      var bitmap = t.Result;
      if (bitmap != null) {
        view.FindViewById<ImageView> (Resource.Id.Image).SetImageBitmap (bitmap);
        bitmap.Dispose ();
      }
    }, TaskScheduler.FromCurrentSynchronizationContext ());
  } else {
    // clear the image
    view.FindViewById<ImageView> (Resource.Id.Image).SetImageBitmap (null);
  }

  return view;
}
```

Метод `GetView` вызывается для возвращения ячейки для отрисовки, как `View`, для каждой строки данных в списке. Он создает экземпляр `View` для каждой строки данных, которая будет отображаться на экране, а внешний вид экземпляра `View` определяется в файле макета. Если ячейка пропадает с экрана из-за прокрутки, ячейка будет доступна для повторного использования. Это позволяет сэкономить память и гарантировать, что присутствуют только экземпляры `View` для данных, отображаемых на экране, а не для всех данных в списке. Дополнительные сведения о повторном использовании представления см. в разделе [Повторное использование представления строк](~/android/user-interface/layouts/list-view/populating.md).

Метод `GetView` также заполняет экземпляр `View` данными, включая чтение данных изображения из файла с именем, указанным свойством `ImageFilename`.

Макет каждой ячейки, отображаемой собственным `ListView`, определяется в файле макета `NativeAndroidListViewCell.axml`, который увеличивается с помощью метода `LayoutInflater.Inflate`. В следующем примере кода показано определение макета.

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
            android:id="@+id/Text1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textColor="#FF7F3300"
            android:textSize="20dip"
            android:textStyle="italic" />
        <TextView
            android:id="@+id/Text2"
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

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Реагирование на изменение свойства в пользовательском элементе управления

Если свойство `NativeListView.Items` меняется в связи с добавлением или удалением элементов из списка, пользовательский отрисовщик должен отреагировать и отобразить эти изменения. Это можно сделать путем переопределения метода `OnElementPropertyChanged`, как показано в следующем примере кода.

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Adapter = new NativeAndroidListViewAdapter (_context as Android.App.Activity, Element as NativeListView);
  }
}
```

Метод создает новый экземпляр класса `NativeAndroidListViewAdapter`, который предоставляет данные для собственного элемента управления `ListView`, при условии что привязываемое свойство `NativeListView.Items` изменилось.

### <a name="creating-the-custom-renderer-on-uwp"></a>Создание пользовательского отрисовщика на универсальной платформе Windows

В следующем примере кода показан пользовательский отрисовщик для платформы UWP:

```csharp
[assembly: ExportRenderer(typeof(NativeListView), typeof(NativeUWPListViewRenderer))]
namespace CustomRenderer.UWP
{
    public class NativeUWPListViewRenderer : ListViewRenderer
    {
        ListView listView;

        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged(e);

            listView = Control as ListView;

            if (e.OldElement != null)
            {
                // Unsubscribe
                listView.SelectionChanged -= OnSelectedItemChanged;
            }

            if (e.NewElement != null)
            {
                listView.SelectionMode = ListViewSelectionMode.Single;
                listView.IsItemClickEnabled = false;
                listView.ItemsSource = ((NativeListView)e.NewElement).Items;             
                listView.ItemTemplate = App.Current.Resources["ListViewItemTemplate"] as Windows.UI.Xaml.DataTemplate;
                // Subscribe
                listView.SelectionChanged += OnSelectedItemChanged;
            }  
        }

        void OnSelectedItemChanged(object sender, SelectionChangedEventArgs e)
        {
            ((NativeListView)Element).NotifyItemSelected(listView.SelectedItem);
        }
    }
}
```

Собственный элемент управления `ListView` настраивается, если пользовательский отрисовщик подключен к новому элементу Xamarin.Forms. Эта конфигурация включает настройку реагирования собственного элемента управления `ListView` на выбор элементов, заполнение данных, отображаемых элементом управления, определение внешнего вида и содержимого каждой ячейки и регистрацию обработчика событий для обработки события `SelectionChanged`. В свою очередь, этот обработчик вызывает событие `ItemSelected`, предоставляемое пользовательским элементом управления `NativeListView`. Подписка на событие `SelectionChanged` отменяется, если изменяется элемент Xamarin.Forms, к которому подключен отрисовщик.

Внешний вид и содержимое каждой собственной ячейки `ListView` определяется `DataTemplate` с именем `ListViewItemTemplate`. `DataTemplate` хранится в словаре ресурсов на уровне приложения и приводится в следующем примере кода.

```xaml
<DataTemplate x:Key="ListViewItemTemplate">
    <Grid Background="#DAFF7F">
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

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Реагирование на изменение свойства в пользовательском элементе управления

Если свойство `NativeListView.Items` меняется в связи с добавлением или удалением элементов из списка, пользовательский отрисовщик должен отреагировать и отобразить эти изменения. Это можно сделать путем переопределения метода `OnElementPropertyChanged`, как показано в следующем примере кода.

```csharp
protected override void OnElementPropertyChanged(object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
    base.OnElementPropertyChanged(sender, e);

    if (e.PropertyName == NativeListView.ItemsProperty.PropertyName)
    {
        listView.ItemsSource = ((NativeListView)Element).Items;
    }
}
```

Метод повторно заполняет собственный элемент управления `ListView` измененными данными, если привязываемое свойство `NativeListView.Items` изменилось.

## <a name="summary"></a>Сводка

В этой статье вы узнали, как создать пользовательский отрисовщик, содержащий зависящие от платформы элементы управления "Список" и собственные макеты ячеек, предоставляя больше контроля над производительностью собственных элементов управления "Список".

## <a name="related-links"></a>Связанные ссылки

- [CustomRendererListView (пример)](/samples/xamarin/xamarin-forms-samples/customrenderers-listview)