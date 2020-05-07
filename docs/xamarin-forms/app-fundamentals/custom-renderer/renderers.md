---
title: Базовые классы отрисовщика и собственные элементы управления
description: Каждый элемент управления Xamarin.Forms имеет сопутствующий отрисовщик для каждой платформы, который создает экземпляр собственного элемента управления. В этой статье перечислены классы элементов управления отрисовщика и собственных элементов управления, которые реализуют каждую страницу, макет, представление и ячейку Xamarin.Forms.
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/17/2020
ms.openlocfilehash: 986b1f7dce05451b96a78e4b39b0091309d93973
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517473"
---
# <a name="renderer-base-classes-and-native-controls"></a>Базовые классы отрисовщика и собственные элементы управления

_Каждый элемент управления Xamarin.Forms имеет сопутствующий отрисовщик для каждой платформы, который создает экземпляр собственного элемента управления. В этой статье перечислены классы элементов управления отрисовщика и собственных элементов управления, которые реализуют каждую страницу, макет, представление и ячейку Xamarin.Forms._

За исключением класса `MapRenderer`, отрисовщики для конкретных платформ находятся в следующих пространствах имен:

- **iOS** — Xamarin.Forms.Platform.iOS
- **Android** — Xamarin.Forms.Platform.Android
- **Android (AppCompat)**  — Xamarin.Forms.Platform.Android.AppCompat
- **Универсальная платформа Windows (UWP)**  — Xamarin.Forms.Platform.UWP

Класс `MapRenderer` находится в следующих пространствах имен:

- **iOS** — Xamarin.Forms.Maps.iOS
- **Android** — Xamarin.Forms.Maps.Android
- **Универсальная платформа Windows (UWP)**  — Xamarin.Forms.Maps.UWP

> [!NOTE]
> Сведения о создании пользовательских отрисовщиков для приложений оболочки см. в статье [о пользовательских отрисовщиках в оболочке Xamarin.Forms](~/xamarin-forms/app-fundamentals/shell/customrenderers.md).

## <a name="pages"></a>Pages

В приведенной ниже таблице перечислены классы отрисовщиков и собственных элементов управления, которые реализуют [страницы](~/xamarin-forms/user-interface/controls/pages.md) Xamarin.Forms всех типов.

|Страница|Обработчик|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](xref:Xamarin.Forms.ContentPage)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|UIViewController|ViewGroup||FrameworkElement|
|[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)|PhoneMasterDetailRenderer (iOS для телефонов), TabletMasterDetailPageRenderer (iOS для планшетов), MasterDetailRenderer (Android), MasterDetailPageRenderer (Android AppCompat), MasterDetailPageRenderer (UWP)|UIViewController (телефон), UISplitViewController (планшет)|DrawerLayout (версия 4)|DrawerLayout (версия 4)|FrameworkElement (пользовательский элемент управления)|
|[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)|NavigationRenderer (iOS и Android), NavigationPageRenderer (Android AppCompat), NavigationPageRenderer (UWP)|UIToolbar|ViewGroup|ViewGroup|FrameworkElement (пользовательский элемент управления)|
|[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)|TabbedRenderer (iOS и Android), TabbedPageRenderer (Android AppCompat), TabbedPageRenderer (UWP)|UIView|ViewPager|ViewPager|FrameworkElement (Pivot)|
|[`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)|PageRenderer|UIViewController|ViewGroup||FrameworkElement|
|[`CarouselPage`](xref:Xamarin.Forms.CarouselPage)|CarouselPageRenderer|UIScrollView|ViewPager|ViewPager|FrameworkElement (FlipView)|

## <a name="layouts"></a>Макеты

В приведенной ниже таблице перечислены классы отрисовщиков и собственных элементов управления, которые реализуют [макеты](~/xamarin-forms/user-interface/controls/layouts.md) Xamarin.Forms всех типов.

|Макет|Обработчик|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)|ViewRenderer|UIView|Просмотр|FrameworkElement|
|[`ContentView`](xref:Xamarin.Forms.ContentView)|ViewRenderer|UIView|Просмотр|FrameworkElement|
|[`FlexLayout`](xref:Xamarin.Forms.FlexLayout)|ViewRenderer|UIView|Просмотр|FrameworkElement|
|[`Frame`](xref:Xamarin.Forms.Frame)|FrameRenderer|UIView|ViewGroup|Border|
|[`ScrollView`](xref:Xamarin.Forms.ScrollView)|ScrollViewRenderer|UIScrollView|ScrollView|ScrollViewer|
|[`TemplatedView`](xref:Xamarin.Forms.TemplatedView)|ViewRenderer|UIView|Просмотр|FrameworkElement|
|[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)|ViewRenderer|UIView|Просмотр|FrameworkElement|
|[`Grid`](xref:Xamarin.Forms.Grid)|ViewRenderer|UIView|Просмотр|FrameworkElement|
|[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)|ViewRenderer|UIView|Просмотр|FrameworkElement|
|[`StackLayout`](xref:Xamarin.Forms.StackLayout)|ViewRenderer|UIView|Просмотр|FrameworkElement|

## <a name="views"></a>Представления

В приведенной ниже таблице перечислены классы отрисовщиков и собственных элементов управления, которые реализуют [представления](~/xamarin-forms/user-interface/controls/views.md) Xamarin.Forms всех типов.

|Представления|Обработчик|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)|ActivityIndicatorRenderer|UIActivityIndicator|ProgressBar||ProgressBar|
|[`BoxView`](xref:Xamarin.Forms.BoxView)|BoxRenderer (iOS и Android), BoxViewRenderer (UWP)|UIView|ViewGroup||Прямоугольник|
|[`Button`](xref:Xamarin.Forms.Button)|ButtonRenderer|UIButton|Кнопка|AppCompatButton|Кнопка|
|[`CarouselView`](xref:Xamarin.Forms.CarouselView)|CarouselViewRenderer|UICollectionView||RecyclerView|ListViewBase|
|`CheckBox`|CheckBoxRenderer|UIButton||AppCompatCheckBox|CheckBox|
|[`CollectionView`](xref:Xamarin.Forms.CollectionView)|CollectionViewRenderer|UICollectionView||RecyclerView|ListViewBase|
|[`DatePicker`](xref:Xamarin.Forms.DatePicker)|DatePickerRenderer|UITextField|EditText||DatePicker|
|[`Editor`](xref:Xamarin.Forms.Editor)|EditorRenderer|UITextView|EditText||TextBox|
|[`Entry`](xref:Xamarin.Forms.Entry)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|UITextField|EditText||TextBox|
|[`Image`](xref:Xamarin.Forms.Image)|ImageRenderer|UIImageView|ImageView||Изображение|
|[`ImageButton`](xref:Xamarin.Forms.ImageButton)|ImageButtonRenderer|UIButton||AppCompatImageButton|Кнопка|
|`IndicatorView`|IndicatorViewRenderer|UIPageControl||LinearLayout||
|[`Label`](xref:Xamarin.Forms.Label)|LabelRenderer|UILabel|TextView||TextBlock|
|[`ListView`](xref:Xamarin.Forms.ListView)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|ListView||ListView|
|[`Map`](xref:Xamarin.Forms.Maps.Map)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)|MKMapView|MapView||MapControl|
|[`MediaElement`](xref:Xamarin.Forms.MediaElement)|MediaElementRenderer|UIView||VideoView|MediaElement|
|[`Picker`](xref:Xamarin.Forms.Picker)|PickerRenderer|UITextField|EditText|EditText|ComboBox|
|`RadioButton`|RadioButtonRenderer|UIButton||AppCompatRadioButton|RadioButton|
|[`ProgressBar`](xref:Xamarin.Forms.ProgressBar)|ProgressBarRenderer|UIProgressView|ProgressBar||ProgressBar|
|[`RefreshView`](xref:Xamarin.Forms.RefreshView)|RefreshViewRenderer|UIView||SwipeRefreshLayout|RefreshContainer|
|[`SearchBar`](xref:Xamarin.Forms.SearchBar)|SearchBarRenderer|UISearchBar|SearchView||AutoSuggestBox|
|[`Slider`](xref:Xamarin.Forms.Slider)|SliderRenderer|UISlider|SeekBar||Slider|
|[`Stepper`](xref:Xamarin.Forms.Stepper)|StepperRenderer|UIStepper|LinearLayout||Элемент управления|
|`SwipeView`|SwipeViewRenderer|UIView||Просмотр|SwipeControl|
|[`Switch`](xref:Xamarin.Forms.Switch)|SwitchRenderer|UISwitch|Параметр|SwitchCompat|ToggleSwitch|
|[`TableView`](xref:Xamarin.Forms.TableView)|TableViewRenderer|UITableView|ListView||ListView|
|[`TimePicker`](xref:Xamarin.Forms.TimePicker)|TimePickerRenderer|UITextField|EditText||TimePicker|
|[`WebView`](xref:Xamarin.Forms.WebView)|WkWebViewRenderer (iOS), WebViewRenderer (Android и UWP)|WkWebView|WebView||WebView|

> [!NOTE]
> Элемент управления `Expander` реализуется с помощью [`StackLayout`](xref:Xamarin.Forms.StackLayout) с анимацией. Поэтому у него нет отрисовщиков платформы.

## <a name="cells"></a>Ячейки

В приведенной ниже таблице перечислены классы отрисовщиков и собственных элементов управления, которые реализуют [ячейки](~/xamarin-forms/user-interface/controls/cells.md) Xamarin.Forms всех типов.

|Ячейки|Обработчик|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](xref:Xamarin.Forms.EntryCell)|EntryCellRenderer|UITableViewCell с UITextField|LinearLayout с TextView и EditText|DataTemplate с TextBox|
|[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)|SwitchCellRenderer|UITableViewCell с UISwitch|Параметр|DataTemplate с объектом Grid, содержащим TextBlock и ToggleSwitch|
|[`TextCell`](xref:Xamarin.Forms.TextCell)|TextCellRenderer|UITableViewCell|LinearLayout с двумя элементами TextView|DataTemplate с элементом StackPanel, содержащим два элемента TextBlock|
|[`ImageCell`](xref:Xamarin.Forms.ImageCell)|ImageCellRenderer|UITableViewCell с UIImage|LinearLayout с двумя элементами TextView и элементом ImageView|DataTemplate с объектом Grid, содержащим элемент Image и два элемента TextBlock|
|[`ViewCell`](xref:Xamarin.Forms.ViewCell)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|Просмотр|DataTemplate с ContentPresenter|

## <a name="summary"></a>Сводка

В этой статье были перечислены классы отрисовщиков и собственных элементов управления, которые реализуют каждую страницу, макет, представление и ячейку Xamarin.Forms. Каждый элемент управления Xamarin.Forms имеет сопутствующий отрисовщик для каждой платформы, который создает экземпляр собственного элемента управления.
