---
title: Xamarin.Forms в проектах Xamarin Native
description: В этой статье объясняется, как использовать страницы, производные от ContentPage, которые непосредственно добавляются в собственные проекты Xamarin, и как осуществляется переход между ними.
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/19/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: aec9f0ec0b3092a5f84f183fb90cfc8bc9da7324
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374229"
---
# <a name="no-locxamarinforms-in-xamarin-native-projects"></a>Xamarin.Forms в проектах Xamarin Native

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/native2forms)

Как правило, Xamarin.Forms приложение включает одну или несколько страниц, производных от [`ContentPage`](xref:Xamarin.Forms.ContentPage) , и эти страницы совместно используются всеми платформами в проекте .NET Standard библиотеки или в общем проекте. Однако собственные формы позволяют `ContentPage` добавлять страницы, производные от, непосредственно в собственные приложения Xamarin. iOS, Xamarin. Android и UWP. По сравнению с `ContentPage` созданием собственных страниц, производных от использования в проекте .NET Standard проекта библиотеки или общего проекта, преимущество добавления страниц непосредственно в собственные проекты заключается в том, что эти страницы можно расширить с помощью собственных представлений. Собственные представления могут называться в XAML с помощью `x:Name` и ссылаться на них из кода программной части. Дополнительные сведения о собственных представлениях см. в разделе [собственные представления](~/xamarin-forms/platform/native-views/index.md).

Процесс использования Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) производной страницы в проекте машинного кода выглядит следующим образом:

1. Добавьте Xamarin.Forms пакет NuGet в собственный проект.
1. Добавьте [`ContentPage`](xref:Xamarin.Forms.ContentPage) производную страницу и все зависимости в собственный проект.
1. Вызовите метод `Forms.Init`.
1. Создайте экземпляр страницы, производной от класса, [`ContentPage`](xref:Xamarin.Forms.ContentPage) и преобразуйте ее в соответствующий собственный тип с помощью одного из следующих методов расширения: `CreateViewController` для iOS, `CreateSupportFragment` для Android или `CreateFrameworkElement` для UWP.
1. Перейдите к представлению собственного типа для [`ContentPage`](xref:Xamarin.Forms.ContentPage) страницы, производной от, с помощью собственного API навигации.

Xamarin.Forms должен быть инициализирован путем вызова `Forms.Init` метода до того, как собственный проект может построить [`ContentPage`](xref:Xamarin.Forms.ContentPage) страницу, производную от. Выбор времени выполнения этого действия зависит от того, когда он наиболее удобен в потоке приложения — он может выполняться при запуске приложения или непосредственно перед созданием `ContentPage` страницы, производной от. В этой статье и прилагаемых примерах приложений `Forms.Init` метод вызывается при запуске приложения.

> [!NOTE]
> Решение примера приложения **нативеформс** не содержит Xamarin.Forms проектов. Вместо этого он состоит из проекта Xamarin. iOS, проекта Xamarin. Android и проекта UWP. Каждый проект является собственным проектом, который использует собственные формы для использования [`ContentPage`](xref:Xamarin.Forms.ContentPage) производных страниц. Однако нет причин, по которым собственные проекты не смогли использовать `ContentPage` производные страницы из проекта .NET Standard библиотеки или общего проекта.

При использовании собственных форм Xamarin.Forms такие функции, как [`DependencyService`](xref:Xamarin.Forms.DependencyService) , [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) и механизм привязки данных, все еще работают. Однако Навигация по страницам должна выполняться с помощью собственного API навигации.

## <a name="ios"></a>iOS

В iOS `FinishedLaunching` Переопределение в `AppDelegate` классе обычно является местом для выполнения задач, связанных с запуском приложения. Он вызывается после запуска приложения и обычно переопределяется для настройки главного окна и контроллера представления. В следующем примере кода показан `AppDelegate` класс в примере приложения:

```csharp
[Register("AppDelegate")]
public class AppDelegate : UIApplicationDelegate
{
    public static AppDelegate Instance;
    UIWindow _window;
    AppNavigationController _navigation;

    public static string FolderPath { get; private set; }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        Forms.Init();

        Instance = this;
        _window = new UIWindow(UIScreen.MainScreen.Bounds);

        UINavigationBar.Appearance.SetTitleTextAttributes(new UITextAttributes
        {
            TextColor = UIColor.Black
        });

        FolderPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData));
        UIViewController mainPage = new NotesPage().CreateViewController();
        mainPage.Title = "Notes";

        _navigation = new AppNavigationController(mainPage);
        _window.RootViewController = _navigation;
        _window.MakeKeyAndVisible();

        return true;
    }
    // ...
}
```

Метод `FinishedLaunching` выполняет следующие задачи:

- Xamarin.Forms инициализируется путем вызова `Forms.Init` метода.
- Ссылка на `AppDelegate` класс хранится в `static` `Instance` поле. Это необходимо для предоставления другим классам механизма вызова методов, определенных в `AppDelegate` классе.
- `UIWindow`Создается объект, который является основным контейнером для представлений в собственных приложениях iOS.
- `FolderPath`Свойство инициализируется по пути на устройстве, где будут храниться данные о заметках.
- `NotesPage`Класс, являющийся Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) производной страницей, определенной в XAML, создается и преобразуется в с `UIViewController` помощью `CreateViewController` метода расширения.
- `Title`Свойство объекта `UIViewController` задается, которое будет отображаться в `UINavigationBar` .
- `AppNavigationController`Для управления иерархической навигацией создается. Это класс пользовательского контроллера навигации, производный от `UINavigationController` . `AppNavigationController`Объект управляет стеком контроллеров представления, а `UIViewController` переданный в конструктор значение будет представлено первоначально при `AppNavigationController` загрузке.
- `AppNavigationController`Объект задается в качестве верхнего уровня `UIViewController` для `UIWindow` , а `UIWindow` задается как ключевое окно для приложения и становится видимым.

После `FinishedLaunching` выполнения метода будет отображен пользовательский интерфейс, определенный в Xamarin.Forms `NotesPage` классе, как показано на следующем снимке экрана:

[![Снимок экрана приложения Xamarin. iOS, использующего пользовательский интерфейс, определенный в XAML](native-forms-images/ios-notespage.png "Приложение Xamarin. iOS с пользовательским интерфейсом XAML")](native-forms-images/ios-notespage-large.png#lightbox "Приложение Xamarin. iOS с пользовательским интерфейсом XAML")

При взаимодействии с пользовательским интерфейсом, например нажатием кнопки **+** [`Button`](xref:Xamarin.Forms.Button) , будет создан следующий обработчик событий в `NotesPage` коде программной части:

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToNoteEntryPage(new Note());
}
```

`static` `AppDelegate.Instance` Поле позволяет `AppDelegate.NavigateToNoteEntryPage` вызывать метод, который показан в следующем примере кода:

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    var noteEntryPage = new NoteEntryPage
    {
        BindingContext = note
    }.CreateViewController();
    noteEntryPage.Title = "Note Entry";
    _navigation.PushViewController(noteEntryPage, true);
}
```

`NavigateToNoteEntryPage`Метод преобразует Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) производную страницу в `UIViewController` с `CreateViewController` методом расширения и задает `Title` свойство объекта `UIViewController` . `UIViewController`Затем объект передается `AppNavigationController` `PushViewController` методу. Таким образом, Пользовательский интерфейс, определенный в Xamarin.Forms `NoteEntryPage` классе, будет отображаться, как показано на следующем снимке экрана:

[![Снимок экрана приложения Xamarin. iOS, использующего пользовательский интерфейс, определенный в XAML](native-forms-images/ios-noteentrypage.png "Приложение Xamarin. iOS с пользовательским интерфейсом XAML")](native-forms-images/ios-noteentrypage-large.png#lightbox "Приложение Xamarin. iOS с пользовательским интерфейсом XAML")

Когда `NoteEntryPage` отображается, обратная Навигация будет открывать `UIViewController` `NoteEntryPage` класс для класса из `AppNavigationController` , возвращая пользователя к `UIViewController` `NotesPage` классу для класса. Однако при извлечении `UIViewController` из собственного стека навигации для iOS не удаляется `UIViewController` и присоединенный `Page` объект автоматически. Таким образом, `AppNavigationController` класс переопределяет `PopViewController` метод, чтобы удалить контроллеры представления при обратной навигации:

```csharp
public class AppNavigationController : UINavigationController
{
    //...
    public override UIViewController PopViewController(bool animated)
    {
        UIViewController topView = TopViewController;
        if (topView != null)
        {
            // Dispose of ViewController on back navigation.
            topView.Dispose();
        }
        return base.PopViewController(animated);
    }
}
```

`PopViewController`Переопределение вызывает `Dispose` метод для `UIViewController` объекта, извлеченного из собственного стека навигации iOS. Невыполнение этого действия приведет к `UIViewController` `Page` потере потерянного и присоединенного объекта.

> [!IMPORTANT]
> Потерянные объекты не могут быть собраны в мусор и поэтому вызывают утечку памяти.

## <a name="android"></a>Android

В Android `OnCreate` Переопределение в `MainActivity` классе обычно является местом для выполнения задач, связанных с запуском приложения. В следующем примере кода показан `MainActivity` класс в примере приложения:

```csharp
public class MainActivity : AppCompatActivity
{
    public static string FolderPath { get; private set; }

    public static MainActivity Instance;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);

        Forms.Init(this, bundle);
        Instance = this;

        SetContentView(Resource.Layout.Main);
        var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
        SetSupportActionBar(toolbar);
        SupportActionBar.Title = "Notes";

        FolderPath = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.LocalApplicationData));
        Android.Support.V4.App.Fragment mainPage = new NotesPage().CreateSupportFragment(this);
        SupportFragmentManager
            .BeginTransaction()
            .Replace(Resource.Id.fragment_frame_layout, mainPage)
            .Commit();
        ...
    }
    ...
}
```

Метод `OnCreate` выполняет следующие задачи:

- Xamarin.Forms инициализируется путем вызова `Forms.Init` метода.
- Ссылка на `MainActivity` класс хранится в `static` `Instance` поле. Это необходимо для предоставления другим классам механизма вызова методов, определенных в `MainActivity` классе.
- `Activity`Содержимое задается из ресурса макета. В примере приложения макет состоит из объекта `LinearLayout` , содержащего `Toolbar` , и, `FrameLayout` который будет использоваться в качестве контейнера фрагментов.
- `Toolbar`Извлекается и устанавливается в качестве панели действий для, а также задается `Activity` заголовок панели действий.
- `FolderPath`Свойство инициализируется по пути на устройстве, где будут храниться данные о заметках.
- `NotesPage`Класс, являющийся Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) производной страницей, определенной в XAML, создается и преобразуется в с `Fragment` помощью `CreateSupportFragment` метода расширения.
- `SupportFragmentManager`Класс создает и фиксирует транзакцию, которая заменяет `FrameLayout` экземпляр `Fragment` `NotesPage` классом для класса.

Дополнительные сведения о фрагментах см. в разделе [фрагменты](~/android/platform/fragments/index.md).

После `OnCreate` выполнения метода будет отображен пользовательский интерфейс, определенный в Xamarin.Forms `NotesPage` классе, как показано на следующем снимке экрана:

[![Снимок экрана приложения Xamarin. Android, использующего пользовательский интерфейс, определенный в XAML](native-forms-images/android-notespage.png "Приложение Xamarin. Android с пользовательским интерфейсом XAML")](native-forms-images/android-notespage-large.png#lightbox "Приложение Xamarin. Android с пользовательским интерфейсом XAML")

При взаимодействии с пользовательским интерфейсом, например нажатием кнопки **+** [`Button`](xref:Xamarin.Forms.Button) , будет создан следующий обработчик событий в `NotesPage` коде программной части:

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToNoteEntryPage(new Note());
}
```

`static` `MainActivity.Instance` Поле позволяет `MainActivity.NavigateToNoteEntryPage` вызывать метод, который показан в следующем примере кода:

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    Android.Support.V4.App.Fragment noteEntryPage = new NoteEntryPage
    {
        BindingContext = note
    }.CreateSupportFragment(this);
    SupportFragmentManager
        .BeginTransaction()
        .AddToBackStack(null)
        .Replace(Resource.Id.fragment_frame_layout, noteEntryPage)
        .Commit();
}
```

`NavigateToNoteEntryPage`Метод преобразует страницу, Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) производную от, в `Fragment` с `CreateSupportFragment` методом расширения и добавляет в `Fragment` стек обратного фрагмента. Таким образом, Пользовательский интерфейс, определенный в, Xamarin.Forms `NoteEntryPage` будет отображаться, как показано на следующем снимке экрана:

[![Снимок экрана приложения Xamarin. Android, использующего пользовательский интерфейс, определенный в XAML](native-forms-images/android-noteentrypage.png "Приложение Xamarin. Android с пользовательским интерфейсом XAML")](native-forms-images/android-noteentrypage-large.png#lightbox "Приложение Xamarin. Android с пользовательским интерфейсом XAML")

Когда `NoteEntryPage` отобразится окно, коснитесь стрелки назад, `Fragment` `NoteEntryPage` чтобы перейти из стека фрагмента обратно на экран, возвращая пользователя в `Fragment` для `NotesPage` класса.

### <a name="enable-back-navigation-support"></a>Включить поддержку обратной навигации

`SupportFragmentManager`Класс содержит `BackStackChanged` событие, которое срабатывает при каждом изменении содержимого стека фрагмента. `OnCreate`Метод в `MainActivity` классе содержит анонимный обработчик событий для этого события:

```csharp
SupportFragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = SupportFragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Note Entry" : "Notes";
};
```

Этот обработчик событий отображает кнопку назад на панели действий при условии, что в `Fragment` стеке фрагмента кода имеется один или несколько экземпляров. Ответ на нажатие кнопки назад обрабатывается `OnOptionsItemSelected` переопределением:

```csharp
public override bool OnOptionsItemSelected(Android.Views.IMenuItem item)
{
    if (item.ItemId == global::Android.Resource.Id.Home && SupportFragmentManager.BackStackEntryCount > 0)
    {
        SupportFragmentManager.PopBackStack();
        return true;
    }
    return base.OnOptionsItemSelected(item);
}
```

`OnOptionsItemSelected`Переопределение вызывается всякий раз, когда выбран элемент в меню Параметры. Эта реализация выводит текущий фрагмент из стека фрагмента, при условии что выбрана кнопка "назад" и имеется один или несколько `Fragment` экземпляров в стеке фрагментов кода.

### <a name="multiple-activities"></a>Несколько действий

Если приложение состоит из нескольких действий, [`ContentPage`](xref:Xamarin.Forms.ContentPage) то страницы, производные от, могут быть внедрены в каждое из действий. В этом сценарии `Forms.Init` метод должен вызываться только в `OnCreate` переопределении первого объекта `Activity` , который внедряет Xamarin.Forms `ContentPage` . Однако это имеет следующие последствия.

- Значение `Xamarin.Forms.Color.Accent` будет взято из объекта `Activity` , который вызвал `Forms.Init` метод.
- Значение `Xamarin.Forms.Application.Current` будет связано с `Activity` методом, который вызвал `Forms.Init` метод.

### <a name="choose-a-file"></a>Выбор файла

При внедрении [`ContentPage`](xref:Xamarin.Forms.ContentPage) производной страницы, использующей [`WebView`](xref:Xamarin.Forms.WebView) , которая должна поддерживать HTML-кнопку "выбрать файл", потребуется `Activity` переопределить `OnActivityResult` метод:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

В платформе UWP собственный `App` класс обычно используется для выполнения задач, связанных с запуском приложения. Xamarin.Forms обычно инициализируется в Xamarin.Forms приложениях UWP в `OnLaunched` переопределении в собственном `App` классе для передачи `LaunchActivatedEventArgs` аргумента в `Forms.Init` метод. По этой причине собственные приложения UWP, использующие Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) производную страницу, могут наиболее просто вызывать `Forms.Init` метод из `App.OnLaunched` метода.

По умолчанию исходный `App` класс запускает `MainPage` класс в качестве первой страницы приложения. В следующем примере кода показан `MainPage` класс в примере приложения:

```csharp
public sealed partial class MainPage : Page
{
    NotesPage notesPage;
    NoteEntryPage noteEntryPage;
    
    public static MainPage Instance;
    public static string FolderPath { get; private set; }

    public MainPage()
    {
        this.InitializeComponent();
        this.NavigationCacheMode = NavigationCacheMode.Enabled;
        Instance = this;
        FolderPath = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.LocalApplicationData));
        notesPage = new Notes.UWP.Views.NotesPage();
        this.Content = notesPage.CreateFrameworkElement();
        // ...        
    } 
    // ...
}
```

`MainPage`Конструктор выполняет следующие задачи:

- Кэширование включается для страницы, так что новая функция `MainPage` не создается, когда пользователь переходит на страницу.
- Ссылка на `MainPage` класс хранится в `static` `Instance` поле. Это необходимо для предоставления другим классам механизма вызова методов, определенных в `MainPage` классе.
- `FolderPath`Свойство инициализируется по пути на устройстве, где будут храниться данные о заметках.
- `NotesPage`Класс, который является Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) производной страницей, определенной в XAML, создается и преобразуется в с `FrameworkElement` помощью `CreateFrameworkElement` метода расширения, а затем устанавливается в качестве содержимого `MainPage` класса.

После `MainPage` выполнения конструктора будет отображен пользовательский интерфейс, определенный в Xamarin.Forms `NotesPage` классе, как показано на следующем снимке экрана:

[![Снимок экрана приложения UWP, использующего пользовательский интерфейс, определенный с помощью::: No-Loc (Xamarin. Forms)::: XAML](native-forms-images/uwp-notespage.png "Приложение UWP с пользовательским интерфейсом::: No-Loc (Xamarin. Forms)::: XAML")](native-forms-images/uwp-notespage-large.png#lightbox "Приложение UWP с пользовательским интерфейсом::: No-Loc (Xamarin. Forms)::: XAML")

При взаимодействии с пользовательским интерфейсом, например нажатием кнопки **+** [`Button`](xref:Xamarin.Forms.Button) , будет создан следующий обработчик событий в `NotesPage` коде программной части:

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainPage.Instance.NavigateToNoteEntryPage(new Note());
}
```

`static` `MainPage.Instance` Поле позволяет `MainPage.NavigateToNoteEntryPage` вызывать метод, который показан в следующем примере кода:

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    noteEntryPage = new Notes.UWP.Views.NoteEntryPage
    {
        BindingContext = note
    };
    this.Frame.Navigate(noteEntryPage);
}
```

Навигация в UWP обычно выполняется с помощью `Frame.Navigate` метода, который принимает `Page` аргумент. Xamarin.Forms Определяет `Frame.Navigate` метод расширения, принимающий [`ContentPage`](xref:Xamarin.Forms.ContentPage) экземпляр страницы, производной от. Поэтому при `NavigateToNoteEntryPage` выполнении метода отображается пользовательский интерфейс, определенный в элементе Xamarin.Forms `NoteEntryPage` , как показано на следующем снимке экрана:

[![Снимок экрана приложения UWP, использующего пользовательский интерфейс, определенный с помощью::: No-Loc (Xamarin. Forms)::: XAML](native-forms-images/uwp-noteentrypage.png "Приложение UWP с пользовательским интерфейсом::: No-Loc (Xamarin. Forms)::: XAML")](native-forms-images/uwp-noteentrypage-large.png#lightbox "Приложение UWP с пользовательским интерфейсом::: No-Loc (Xamarin. Forms)::: XAML")

Когда `NoteEntryPage` отобразится окно, коснитесь стрелки назад, `FrameworkElement` `NoteEntryPage` чтобы перейти из стека в приложении назад и вернуть пользователя к `FrameworkElement` `NotesPage` классу.

### <a name="enable-page-resizing-support"></a>Включить поддержку изменения размера страниц

При изменении размера окна приложения UWP Xamarin.Forms содержимое также должно быть изменено. Это достигается путем регистрации обработчика событий для `Loaded` события в `MainPage` конструкторе:

```csharp
public MainPage()
{
    // ...
    this.Loaded += OnMainPageLoaded;
    // ...
}
```

`Loaded`Событие срабатывает, когда страница располагается, готовится к просмотру и готова к взаимодействию, и выполняет `OnMainPageLoaded` метод в ответе:

```csharp
void OnMainPageLoaded(object sender, RoutedEventArgs e)
{
    this.Frame.SizeChanged += (o, args) =>
    {
        if (noteEntryPage != null)
            noteEntryPage.Layout(new Xamarin.Forms.Rectangle(0, 0, args.NewSize.Width, args.NewSize.Height));
        else
            notesPage.Layout(new Xamarin.Forms.Rectangle(0, 0, args.NewSize.Width, args.NewSize.Height));
    };
}
```

`OnMainPageLoaded`Метод регистрирует анонимный обработчик событий для `Frame.SizeChanged` события, который вызывается при `ActualHeight` `ActualWidth` изменении свойств или `Frame` . В ответ Xamarin.Forms Размер содержимого активной страницы изменяется путем вызова `Layout` метода.

### <a name="enable-back-navigation-support"></a>Включить поддержку обратной навигации

В UWP приложения должны включить обратную навигацию для всех кнопок возврата оборудования и программного обеспечения в различных конструктивных параметрах устройства. Это можно сделать, зарегистрировав обработчик событий для `BackRequested` события, которое может быть выполнено в `MainPage` конструкторе:

```csharp
public MainPage()
{
    // ...
    SystemNavigationManager.GetForCurrentView().BackRequested += OnBackRequested;
}
```

При запуске приложения `GetForCurrentView` метод получает `SystemNavigationManager` объект, связанный с текущим представлением, а затем регистрирует обработчик событий для `BackRequested` события. Приложение получает это событие только в том случае, если оно является приложением переднего плана, а в ответе вызывает `OnBackRequested` обработчик событий:

```csharp
void OnBackRequested(object sender, BackRequestedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        e.Handled = true;
        rootFrame.GoBack();
        noteEntryPage = null;
    }
}
```

`OnBackRequested`Обработчик событий вызывает `GoBack` метод в корневом фрейме приложения и устанавливает свойство в значение, чтобы `BackRequestedEventArgs.Handled` `true` Пометить событие как обработанное. Сбой пометки события как обработанного может привести к тому, что событие будет пропущено.

Приложение выбирает, следует ли отображать кнопку "назад" в строке заголовка. Это достигается путем присвоения `AppViewBackButtonVisibility` свойству одного из `AppViewBackButtonVisibility` значений перечисления в `App` классе:

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

`OnNavigated`Обработчик событий, выполняемый в ответ на `Navigated` срабатывание события, обновляет видимость кнопки «назад» в строке заголовка при переходе на страницу. Это гарантирует, что кнопка «назад» в строке заголовка будет видна, если стек в приложении не пуст или удален из строки заголовка, если резервный стек в приложении пуст.

Дополнительные сведения о поддержке обратной навигации в UWP см. в статье [журнал навигации и обратная Навигация для приложений UWP](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/).

## <a name="related-links"></a>Связанные ссылки

- [Нативеформс (пример)](/samples/xamarin/xamarin-forms-samples/native2forms)
- [Собственные представления](~/xamarin-forms/platform/native-views/index.md)