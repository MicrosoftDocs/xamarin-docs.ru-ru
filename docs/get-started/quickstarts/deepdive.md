---
title: Подробное изучение кратких руководств по Xamarin.Forms
description: Эта статья содержит общие сведения о разработке приложений с использованием Оболочки в Xamarin.Forms. Помимо прочего, здесь описываются структура приложения Оболочки в Xamarin.Forms и принципы его работы, а также пользовательский интерфейс.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.custom: video
ms.assetid: F65C83B7-BC9F-425F-8662-931B652A2946
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/25/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8e7a5a196dbbd55f8d10d1e1960f6b4c989021dc
ms.sourcegitcommit: 1b542afc0f6f2f6adbced527ae47b9ac90eaa1de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101757659"
---
# <a name="xamarinforms-quickstart-deep-dive"></a>Подробное изучение кратких руководств по Xamarin.Forms

В [кратком руководстве по Xamarin.Forms](~/get-started/index.yml) было создано приложение Notes. В этой статье выполняется проверка созданных компонентов, позволяющая получить представление об основных принципах работы приложений Оболочки в Xamarin.Forms.

::: zone pivot="windows"

## <a name="introduction-to-visual-studio"></a>Введение в Visual Studio

Код в Visual Studio упорядочен по *решениям* и *проектам*. Решение — это контейнер для одного или нескольких проектов. Проект может представлять собой приложение, вспомогательную библиотеку, тестовое приложение и т. д. Приложение Notes включает в себя одно решение, содержащее три проекта, как показано на следующем снимке экрана:

![Обозреватель решений Visual Studio](deepdive-images/vs/solution.png)

Решение содержит следующие проекты:

- Notes — это проект библиотеки .NET Standard, который содержит весь общий код и общий пользовательский интерфейс.
- Notes.Android — этот проект содержит код для Android и представляет собой точку входа для приложения Android.
- Notes.iOS — этот проект содержит код для iOS и представляет собой точку входа для приложения iOS.

## <a name="anatomy-of-a-xamarinforms-application"></a>Структура приложения Xamarin.Forms

На следующих снимках экрана показано содержимое проекта библиотеки .NET Standard Notes в Visual Studio:

![Содержимое проекта .NET Standard Phoneword](deepdive-images/vs/net-standard-project.png)

В этом проекте есть узел **Зависимости**, который содержит узлы **NuGet** и **SDK**:

- Пакеты **NuGet** &ndash; Xamarin.Forms, Xamarin.Essentials, Newtonsoft.Json и sqlite-net-pcl, добавленные в проект.
- **SDK** &ndash; метапакет `NETStandard.Library`, который ссылается на полный набор пакетов NuGet, определяющих библиотеку .NET Standard.

::: zone-end
::: zone pivot="macos"

## <a name="introduction-to-visual-studio-for-mac"></a>Введение в Visual Studio для Mac

В [Visual Studio для Mac](/visualstudio/mac/), так же как в Visual Studio, код упорядочивается по *решениям* и *проектам*. Решение — это контейнер для одного или нескольких проектов. Проект может представлять собой приложение, вспомогательную библиотеку, тестовое приложение и т. д. Приложение Notes включает в себя одно решение, содержащее три проекта, как показано на следующем снимке экрана:

![Область решений в Visual Studio для Mac](deepdive-images/vsmac/solution.png)

Решение содержит следующие проекты:

- Notes — это проект библиотеки .NET Standard, который содержит весь общий код и общий пользовательский интерфейс.
- Notes.Android — этот проект содержит код для Android и представляет собой точку входа для приложений Android.
- Notes.iOS — этот проект содержит код для iOS и представляет собой точку входа для приложений iOS.

## <a name="anatomy-of-a-xamarinforms-application"></a>Структура приложения Xamarin.Forms

На следующих снимках экрана показано содержимое проекта библиотеки .NET Standard Notes в Visual Studio для Mac:

![Содержимое проекта библиотеки .NET Standard Phoneword](deepdive-images/vsmac/net-standard-project.png)

В этом проекте есть узел **Зависимости**, который содержит узлы **NuGet** и **SDK**:

- Пакеты **NuGet** &ndash; Xamarin.Forms, Xamarin.Essentials, Newtonsoft.Json и sqlite-net-pcl, добавленные в проект.
- **SDK** &ndash; метапакет `NETStandard.Library`, который ссылается на полный набор пакетов NuGet, определяющих библиотеку .NET Standard.

::: zone-end

Проект также состоит из нескольких файлов:

- **Data\NoteDatabase.cs** — этот класс содержит код, чтобы создать базу данных, читать и записывать данные в ней, а также удалять из нее данные.
- **Models\Note.cs** — этот класс определяет модель `Note`, в экземплярах которой хранятся данные о каждой заметке в приложении.
- **Views\AboutPage.xaml** — разметка XAML для класса `AboutPage`, который определяет пользовательский интерфейс страницы About.
- **Views\AboutPage.xaml.cs** — код программной части для класса `AboutPage`, который содержит бизнес-логику, выполняемую при взаимодействии пользователя со страницей.
- **Views\NotesPage.xaml** — разметка XAML для класса `NotesPage`, который определяет пользовательский интерфейс страницы, отображаемой при запуске приложения.
- **Views\NotesPage.xaml.cs** — код программной части для класса `NotesPage`, который содержит бизнес-логику, выполняемую при взаимодействии пользователя со страницей.
- **Views\NoteEntryPage.xaml** — разметка XAML для класса `NoteEntryPage`, который определяет пользовательский интерфейс страницы, отображаемой при вводе заметки пользователем.
- **Views\NoteEntryPage.xaml.cs** — код программной части для класса `NoteEntryPage`, который содержит бизнес-логику, выполняемую при взаимодействии пользователя со страницей.
- **App.xaml** — разметка XAML для класса `App`, который определяет словарь ресурсов для приложения.
- **App.xaml.cs** — код программной части для класса `App`, который обеспечивает создание экземпляра приложения Оболочки, а также обработку событий жизненного цикла приложения.
- **AppShell.xaml** — разметка XAML для класса `AppShell`, определяющего визуальную иерархию приложения.
- **AppShell.xaml.cs** — код программной части для класса `AppShell`, который создает маршрут для `NoteEntryPage`, чтобы к нему можно было перейти программным способом.
- **AssemblyInfo.cs** — этот файл содержит атрибут приложения, указывающий на проект, который применяется на уровне сборки.

Дополнительные сведения о структуре приложения Xamarin.iOS см. [здесь](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy-of-a-xamarinios-application). Дополнительные сведения о структуре приложения Xamarin.Android см. [здесь](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy).

## <a name="architecture-and-application-fundamentals"></a>Архитектура и принципы работы приложения

По своей архитектуре приложения Xamarin.Forms не отличаются от традиционных кроссплатформенных приложений. Общий код обычно помещается в библиотеку .NET Standard и используется приложениями для конкретных платформ. На следующей схеме показаны эти взаимосвязи для приложения Notes:

![Архитектура приложения Notes](deepdive-images/architecture.png)

Чтобы код запуска как можно больше использовался повторно, в приложениях Xamarin.Forms есть отдельный класс `App`, который отвечает за создание экземпляра приложения на каждой платформе, как показано в следующем примере кода:

```csharp
using Xamarin.Forms;

namespace Notes
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new AppShell();
        }
        // ...
    }
}
```

Этот код задает для свойства `MainPage` `App` `AppShell`. Класс `AppShell` определяет визуальную иерархию приложения. Оболочка использует эту визуальную иерархию и создает для нее пользовательский интерфейс. Дополнительные сведения об определении визуальной иерархии приложения см. в [этом разделе](#application-visual-hierarchy).

Кроме того, файл **AssemblyInfo.cs** содержит один атрибут приложения, который применяется на уровне сборки:

```csharp
using Xamarin.Forms.Xaml;

[assembly: XamlCompilation(XamlCompilationOptions.Compile)]
```

Атрибут [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) включает компилятор XAML, что позволяет компилировать код XAML непосредственно в промежуточный язык. Дополнительные сведения см. в разделе [Компиляция XAML](~/xamarin-forms/xaml/xamlc.md).

## <a name="launch-the-application-on-each-platform"></a>Запуск приложения на каждой платформе

Процесс запуска приложения на каждой платформе зависит от платформы.

### <a name="ios"></a>iOS

Для запуска начальной страницы Xamarin.Forms в iOS в проекте Notes.iOS определяется класс `AppDelegate`, который наследуется от класса `FormsApplicationDelegate`:

```csharp
namespace Notes.iOS
{
    [Register("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
            global::Xamarin.Forms.Forms.Init();
            LoadApplication(new App());
            return base.FinishedLaunching(app, options);
        }
    }
}
```

Переопределение `FinishedLaunching` инициализирует платформу Xamarin.Forms, вызывая метод `Init`. В результате реализация Xamarin.Forms для iOS загружается в приложении, прежде чем корневой контроллер представления задается путем вызова метода `LoadApplication`.

### <a name="android"></a>Android

Чтобы начальную страницу Xamarin.Forms можно было запустить в Android, проект Notes.Android включает код, создающий действие `Activity` с атрибутом `MainLauncher`, которое наследуется от класса `FormsAppCompatActivity`:

```csharp
namespace Notes.Droid
{
    [Activity(Label = "Notes",
              Icon = "@mipmap/icon",
              Theme = "@style/MainTheme",
              MainLauncher = true,
              ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle savedInstanceState)
        {
            TabLayoutResource = Resource.Layout.Tabbar;
            ToolbarResource = Resource.Layout.Toolbar;

            base.OnCreate(savedInstanceState);
            global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
            LoadApplication(new App());
        }
    }
}
```

Переопределение `OnCreate` инициализирует платформу Xamarin.Forms, вызывая метод `Init`. В результате реализация Xamarin.Forms для Android загружается в приложении перед загрузкой самого приложения Xamarin.Forms.

## <a name="application-visual-hierarchy"></a>Визуальная иерархия приложения

Приложения Оболочки в Xamarin.Forms определяют визуальную иерархию приложения в классе, подклассом которого является класс `Shell`. В приложении Notes это класс `Appshell`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Notes.Views"
       x:Class="Notes.AppShell">
    <TabBar>
        <ShellContent Title="Notes"
                      Icon="icon_feed.png"
                      ContentTemplate="{DataTemplate views:NotesPage}" />
        <ShellContent Title="About"
                      Icon="icon_about.png"
                      ContentTemplate="{DataTemplate views:AboutPage}" />
    </TabBar>
</Shell>
```

Этот код XAML состоит из двух основных объектов:

- `TabBar`. Объект `TabBar` представляет собой вкладки на нижней панели. Он требуется, если для шаблона навигации в приложении используются нижние вкладки. Объект `TabBar` является дочерним для объекта `Shell`.
- Объект `ShellContent`, представляющий объекты `ContentPage` для каждой вкладки в объекте `TabBar`. Каждый объект `ShellContent` является дочерним для объекта `TabBar`.

Эти объекты не представляют собой какие-либо элементы пользовательского интерфейса, они служат лишь для организации визуальной структуры приложения. На основе этих объектов оболочка создает пользовательский интерфейс навигации по содержимому. Поэтому в классе `AppShell` определяются две страницы, на которые можно перейти с нижних вкладок. Страницы создаются по запросу в ответ на навигацию.

Дополнительные сведения о приложениях Оболочки см. в статье [Оболочка в Xamarin.Forms](~/xamarin-forms/app-fundamentals/shell/index.md).

## <a name="user-interface"></a>Пользовательский интерфейс

Для создания пользовательского интерфейса приложения Xamarin.Forms используются несколько групп элементов управления:

1. **Страницы** — страницы Xamarin.Forms представляют экраны в кроссплатформенных мобильных приложениях. В приложении Notes для отображения отдельных экранов используется класс [`ContentPage`](xref:Xamarin.Forms.ContentPage). Дополнительные сведения о страницах см. в разделе [Страницы Xamarin.Forms](~/xamarin-forms/user-interface/controls/pages.md).
1. **Представления** — представления Xamarin.Forms являются элементами управления, отображаемыми в пользовательском интерфейсе, например метки, кнопки и поля ввода текста. В готовом приложении Notes используются представления [`CollectionView`](xref:Xamarin.Forms.CollectionView), [`Editor`](xref:Xamarin.Forms.Editor) и [`Button`](xref:Xamarin.Forms.Button). Дополнительные сведения о представлениях см. в разделе [Представления Xamarin.Forms](~/xamarin-forms/user-interface/controls/views.md).
1. **Макеты** — макеты Xamarin.Forms представляют собой контейнеры, которые служат для объединения представлений в логические структуры. В приложении Notes используются класс [`StackLayout`](xref:Xamarin.Forms.StackLayout) для компоновки представлений в вертикальном стеке, а также класс [`Grid`](xref:Xamarin.Forms.Grid) для компоновки кнопок по горизонтали. Дополнительные сведения о макетах см. в статье [Макеты Xamarin.Forms](~/xamarin-forms/user-interface/controls/layouts.md).

Во время выполнения каждый элемент управления сопоставляется с собственным аналогом, который и будет отрисовываться.

### <a name="layout"></a>Макет

В приложении Notes используется класс [`StackLayout`](xref:Xamarin.Forms.StackLayout), который упрощает разработку кроссплатформенных приложений благодаря автоматической компоновке представлений на экране независимо от его размера. Дочерние элементы размещаются поочередно (по горизонтали или по вертикали) в порядке добавления. Область, занимаемая макетом `StackLayout`, зависит от значений свойств [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) и [`VerticalOptions`](xref:Xamarin.Forms.View.HorizontalOptions), но по умолчанию макет `StackLayout` будет пытаться использовать весь экран.

Следующий код XAML представляет собой пример использования макета [`StackLayout`](xref:Xamarin.Forms.StackLayout) для размещения `NoteEntryPage`:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Notes.Views.NoteEntryPage"
             Title="Note Entry">
    ...    
    <StackLayout Margin="{StaticResource PageMargin}">
        <Editor Placeholder="Enter your note"
                Text="{Binding Text}"
                HeightRequest="100" />
        <Grid>
            ...
        </Grid>
    </StackLayout>    
</ContentPage>
```

[`StackLayout`](xref:Xamarin.Forms.StackLayout) по умолчанию предполагает вертикальную ориентацию. При необходимости можно изменить ее на горизонтальную ориентацию, присвоив свойству [`StackLayout.Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) значение члена перечисления [`StackOrientation.Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal).

> [!NOTE]
> Размер представлений можно задавать с помощью свойств `HeightRequest` и `WidthRequest`.

Дополнительные сведения о классе [`StackLayout`](xref:Xamarin.Forms.StackLayout) см. в статье [Xamarin.Forms StackLayout](~/xamarin-forms/user-interface/layouts/stacklayout.md).

### <a name="responding-to-user-interaction"></a>Реакция на действия пользователя

Объект, определенный в XAML, может инициировать событие, которое обрабатывается в файле кода программной части. В следующем примере кода показан метод `OnSaveButtonClicked` в коде программной части для класса `NoteEntryPage`, который выполняется в ответ на событие [`Clicked`](xref:Xamarin.Forms.Button.Clicked), инициируемое при нажатии кнопки *Save* (Сохранить).

```csharp
async void OnSaveButtonClicked(object sender, EventArgs e)
{
    var note = (Note)BindingContext;
    note.Date = DateTime.UtcNow;
    if (!string.IsNullOrWhiteSpace(note.Text))
    {
        await App.Database.SaveNoteAsync(note);
    }
    await Shell.Current.GoToAsync("..");
}
```

Метод `OnSaveButtonClicked` сохраняет заметку в базе данных и осуществляет возврат на предыдущую страницу. Дополнительные сведения о навигации см. в [этом разделе](#navigation).

> [!NOTE]
> Файл кода программной части для класса XAML может получать доступ к объекту, определенному в XAML, по имени, которое назначено ему в атрибуте `x:Name`. Значение, присваиваемое этому атрибуту, должно соответствовать правилам C# в отношении переменных, то есть должно начинаться с буквы или знака подчеркивания и не содержать внутренних пробелов.

Привязка кнопки сохранения к методу `OnSaveButtonClicked` выполняется в разметке XAML для класса `NoteEntryPage`:

```xaml
<Button Text="Save"
        Clicked="OnSaveButtonClicked" />
```

### <a name="lists"></a>Списки

[`CollectionView`](xref:Xamarin.Forms.CollectionView) отвечает за отображение коллекции элементов в списке. По умолчанию элементы списка отображаются вертикально, а каждый элемент отображается в одной строке.

В следующем примере показан пример метода [`CollectionView`](xref:Xamarin.Forms.CollectionView) из `NotesPage`:

```xaml
<CollectionView x:Name="collectionView"
                Margin="{StaticResource PageMargin}"
                SelectionMode="Single"
                SelectionChanged="OnSelectionChanged">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           ItemSpacing="10" />
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <Label Text="{Binding Text}"
                       FontSize="Medium" />
                <Label Text="{Binding Date}"
                       TextColor="{StaticResource TertiaryColor}"
                       FontSize="Small" />
            </StackLayout>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Макет каждой строки в [`CollectionView`](xref:Xamarin.Forms.CollectionView) определяется в элементе [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) и использует привязку данных для отображения всех заметок, извлеченных приложением. В свойстве [`CollectionView.ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) задается источник данных в **NotesPage.xaml.cs**:

```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    collectionView.ItemsSource = await App.Database.GetNotesAsync();
}
```    

Этот код заполняет [`CollectionView`](xref:Xamarin.Forms.CollectionView) с помощью любых заметок, хранящихся в базе данных, и выполняется при появлении страницы.

При выборе элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView) создается событие `SelectionChanged`. При создании этого события выполняется обработчик `OnSelectionChanged`:

```csharp
async void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (e.CurrentSelection != null)
    {
        // ...
    }
}
```

Событие `SelectionChanged` может получать доступ к объекту, который был связан с элементом с помощью свойства `e.CurrentSelection`.

Дополнительные сведения о классе [`CollectionView`](xref:Xamarin.Forms.CollectionView) см. в статье [Xamarin.Forms CollectionView](~/xamarin-forms/user-interface/collectionview/index.md).

## <a name="navigation"></a>Навигация

Навигация в приложении оболочки выполняется путем указания URI для перехода. URI навигации содержат три компонента:

- *Маршрут* определяет путь к содержимому, которое существует в визуальной иерархии оболочки.
- *Страница*. Страницы не отражены в визуальной иерархии оболочки и могут добавляться в стек навигации из любого места в приложении оболочки. Например, объект `NoteEntryPage` не определен в визуальной иерархии Оболочки, но может быть отправлен в стек навигации при необходимости.
- Один или несколько *параметров запроса*. Параметры запроса могут передаваться странице назначения при переходе на нее.

URI навигации не обязательно должен включать все три компонента, но при этом структура выглядит следующим образом: //route/page?queryParameters

> [!NOTE]
> Маршруты можно определять для элементов в визуальной иерархии Оболочки с помощью свойства `Route`. Однако если свойство `Route` не задано, как в приложении Notes, маршрут будет создан во время выполнения.

Дополнительные сведения о навигации по Оболочке см. в разделе [Навигация по Оболочке в Xamarin.Forms](~/xamarin-forms/app-fundamentals/shell/navigation.md).

### <a name="register-routes"></a>Регистрация маршрутов

Для перехода к странице, не существующей в визуальной иерархии Оболочки, необходимо сначала зарегистрировать ее в системе маршрутизации Оболочки с помощью метода `Routing.RegisterRoute`. В приложении Notes это происходит в конструкторе `AppShell`:

```csharp
public partial class AppShell : Shell
{
    public AppShell()
    {
        // ...
        Routing.RegisterRoute(nameof(NoteEntryPage), typeof(NoteEntryPage));
    }
}
```

В этом примере маршрут с именем `NoteEntryPage` регистрируется для типа `NoteEntryPage`. К таким страницам можно переходить по URI из любой точки в приложении.

### <a name="perform-navigation"></a>Выполнение перемещения

Навигация выполняется методом `GoToAsync`, который принимает аргумент, представляющий собой маршрут, по которому необходимо перейти:

```csharp
await Shell.Current.GoToAsync("NoteEntryPage");
```

В этом примере переход выполняется к объекту `NoteEntryPage`.

> [!IMPORTANT]
> При переходе к странице, которая не входит в визуальную иерархию Оболочки, создается стек навигации.

При переходе на страницу данные можно передать на страницу в качестве параметра запроса:

```csharp
async void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (e.CurrentSelection != null)
    {
        // Navigate to the NoteEntryPage, passing the ID as a query parameter.
        Note note = (Note)e.CurrentSelection.FirstOrDefault();
        await Shell.Current.GoToAsync($"{nameof(NoteEntryPage)}?{nameof(NoteEntryPage.ItemId)}={note.ID.ToString()}");
    }
}
```

В этом примере извлекается выбранный в данный момент элемент в объекте [`CollectionView`](xref:Xamarin.Forms.CollectionView) и осуществляется переход к объекту `NoteEntryPage` со значением свойства `ID` объекта `Note`, передаваемого в качестве параметра запроса в свойство `NoteEntryPage.ItemId`.

Для получения переданных данных класс `NoteEntryPage` снабжается атрибутом `QueryPropertyAttribute`.

```csharp
[QueryProperty(nameof(ItemId), nameof(ItemId))]
public partial class NoteEntryPage : ContentPage
{
    public string ItemId
    {
        set
        {
            LoadNote(value);
        }
    }
    // ...
}
```

Первый аргумент для `QueryPropertyAttribute` указывает свойство `ItemId`, которое будет получать переданные данные, а второй аргумент задает идентификатор параметра запроса. Таким образом, атрибут `QueryPropertyAttribute` в примере выше указывает, что свойство `ItemId` будет получать данные из параметра запроса `ItemId`, переданного в URI, через вызов метода `GoToAsync`. Затем свойство `ItemId` вызывает метод `LoadNote` для получения заметки с устройства.

Для выполнения обратной навигации можно указать ".." в качестве аргумента для метода `GoToAsync`:

```csharp
await Shell.Current.GoToAsync("..");
```

Дополнительные сведения об обратной навигации см. в [этом разделе](~/xamarin-forms/app-fundamentals/shell/navigation.md#backwards-navigation).

## <a name="data-binding"></a>привязка данных,

Привязка данных используется для упрощения способа, которым приложение Xamarin.Forms отображает данные и взаимодействует с ними. Она устанавливает связь между пользовательским интерфейсом и базовым приложением. Класс [`BindableObject`](xref:Xamarin.Forms.BindableObject) содержит основную часть инфраструктуры для поддержки привязки данных.

Привязка данных связывает два объекта, которые называются *источником* и *целевым объектом*. *Источник* предоставляет данные. *Целевой* объект будет использовать (и часто отображать) данные из источника. Например, свойство [`Text`](xref:Xamarin.Forms.InputView.Text) элемента [`Editor`](xref:Xamarin.Forms.Editor) (*целевого* объекта) часто связывается с открытым свойством `string` объекта *источника*. На следующей схеме показано отношение привязки:

![Привязка данных](deepdive-images/data-binding.png)

Главным преимуществом привязки данных является то, что вам больше не нужно беспокоиться о синхронизации данных между представлениями и источником данных. Изменения в *источнике* автоматически передаются в *целевой* объект платформой привязки. При необходимости изменения в целевом объекте также могут передаваться в *источник*.

Установление привязки данных состоит из двух этапов:

- Свойству [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)*целевого* объекта должен быть присвоен *источник*.
- Между *целевым* объектом и *источником* должна быть установлена привязка. В XAML для этого используется расширение разметки [`Binding`](xref:Xamarin.Forms.Xaml.BindingExtension).

В приложении Notes в качестве целевого объекта привязки выступает [`Editor`](xref:Xamarin.Forms.Editor), который отображает заметку. В качестве источника привязки выступает экземпляр `Note`, задаваемый как [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) для `NoteEntryPage`. Изначально свойство `BindingContext` объекта `NoteEntryPage` задается при выполнении конструктора страницы:

```csharp
public NoteEntryPage()
{
    // ...
    BindingContext = new Note();
}
```

В этом примере для свойства `BindingContext` страницы задается новое значение `Note` при создании `NoteEntryPage`. Это обеспечивает выполнение сценария добавления новой заметки в приложение.

Кроме того, свойство `BindingContext` страницы можно задать при выполнении перехода к `NoteEntryPage` при условии, что имеющаяся заметка выбрана в `NotesPage`:

```csharp
[QueryProperty(nameof(ItemId), nameof(ItemId))]
public partial class NoteEntryPage : ContentPage
{
    public string ItemId
    {
        set
        {
            LoadNote(value);
        }

        async void LoadNote(string itemId)
        {
            try
            {
                int id = Convert.ToInt32(itemId);
                // Retrieve the note and set it as the BindingContext of the page.
                Note note = await App.Database.GetNoteAsync(id);
                BindingContext = note;
            }
            catch (Exception)
            {
                Console.WriteLine("Failed to load note.");
            }
        }    
        // ...    
    }
}
```

В этом примере, когда выполняется переход по странице, для свойства `BindingContext` страницы задается выбранный объект `Note` после его извлечения из базы данных.

> [!IMPORTANT]
> Хотя свойство [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) каждого *целевого* объекта можно задавать по отдельности, это не является обязательным. `BindingContext` — это особое свойство, которое наследуется всеми дочерними объектами. Поэтому, когда свойству `BindingContext` объекта [`ContentPage`](xref:Xamarin.Forms.ContentPage) присваивается экземпляр `Note`, все дочерние объекты `ContentPage` имеют то же значение `BindingContext` и могут привязываться к открытым свойствам объекта `Note`.

Затем [`Editor`](xref:Xamarin.Forms.Editor) в `NoteEntryPage` привязывается к свойству `Text` объекта `Note`:

```xaml
<Editor Placeholder="Enter your note"
        Text="{Binding Text}" />
```

Создается привязка между свойством [`Editor.Text`](xref:Xamarin.Forms.InputView.Text) и свойством `Text`*источника*. Изменения, вносимые в `Editor`, будут автоматически применяться к объекту `Note`. Аналогичным образом, если изменяется свойство `Note.Text`, обработчик привязки Xamarin.Forms также изменит содержимое `Editor`. Это называется *двусторонней привязкой*.

Дополнительные сведения о привязке данных см. в разделе [Привязка данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="styling"></a>Задание стиля

Приложения Xamarin.Forms часто содержат несколько визуальных элементов с одинаковым внешним видом. Настройка внешнего вида каждого визуального элемента может занимать много времени и приводить к возникновению ошибок. Вместо этого можно создать стили, которые определяют внешний вид и при необходимости применяются к нужным визуальным элементам.

Класс [`Style`](xref:Xamarin.Forms.Style) объединяет коллекцию значений свойств в один объект, который затем можно применять к нескольким экземплярам визуальных элементов. Стили хранятся в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) на уровне приложения, страницы или представления. От того, где определен стиль (`Style`), зависит, где его можно использовать:

- Экземпляры [`Style`](xref:Xamarin.Forms.Style), определенные на уровне приложения, можно применять по всему приложению.
- Экземпляры [`Style`](xref:Xamarin.Forms.Style), определенные на уровне страницы, можно применять только к странице и ее дочерним элементам.
- Экземпляры [`Style`](xref:Xamarin.Forms.Style), определенные на уровне представления, можно применять только к представлению и его дочерним элементам.

> [!IMPORTANT]
> Во избежание дублирования все стили, используемые в приложении, хранятся в словаре ресурсов приложения. Тем не менее XAML, соответствующий странице, не следует включать в словарь ресурсов приложения, так как анализ ресурсов будет осуществляться при запуске приложения, а не когда это необходимо для страницы. Дополнительные сведения см. в разделе [Уменьшение размера словаря ресурсов приложения](~/xamarin-forms/deploy-test/performance.md#reduce-the-application-resource-dictionary-size).

Каждый экземпляр [`Style`](xref:Xamarin.Forms.Style) содержит коллекцию из одного или нескольких объектов [`Setter`](xref:Xamarin.Forms.Setter), каждый из которых `Setter` имеет свойство ([`Property`](xref:Xamarin.Forms.Setter.Property)) и значение ([`Value`](xref:Xamarin.Forms.Setter.Value)). `Property` — это имя привязываемого свойства элемента, к которому применяется стиль, а `Value` — это применяемое к этому свойству значение. В следующем примере кода показан стиль из `NoteEntryPage`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Notes.Views.NoteEntryPage"
             Title="Note Entry">
    <ContentPage.Resources>
        <!-- Implicit styles -->
        <Style TargetType="{x:Type Editor}">
            <Setter Property="BackgroundColor"
                    Value="{StaticResource AppBackgroundColor}" />
        </Style>
        ...
    </ContentPage.Resources>
    ...
</ContentPage>
```

Этот стиль применяется к любым экземплярам [`Editor`](xref:Xamarin.Forms.Editor) на странице.

При создании [`Style`](xref:Xamarin.Forms.Style) свойство [`TargetType`](xref:Xamarin.Forms.Style.TargetType) является обязательным.

> [!NOTE]
> Для применения стилей к приложению Xamarin.Forms, как правило, используются стили XAML. Тем не менее Xamarin.Forms также поддерживает применение стилей к визуальным элементам с использованием каскадных таблиц стилей (CSS). Дополнительные сведения см. в разделе [Задание стиля приложений Xamarin.Forms с помощью каскадных таблиц стилей (CSS)](~/xamarin-forms/user-interface/styles/css/index.md).

Дополнительные сведения о стилях XAML см. в руководстве по [оформлению приложений Xamarin.Forms с использованием стилей XAML](~/xamarin-forms/user-interface/styles/xaml/index.md).

## <a name="test-and-deployment"></a>Тестирование и развертывание

Как Visual Studio для Mac, так и Visual Studio предоставляют множество возможностей для тестирования и развертывания приложения. Одной из неотъемлемых стадий жизненного цикла приложения является отладка, которая позволяет выявить проблемы в коде. Дополнительные сведения см. в разделах [Установка точки останова](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [Пошаговая отладка кода](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code) и [Вывод данных в окно журнала](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window).

Симуляторы позволяют начать тестирование и развертывание приложения, реализуя полезные функции для тестирования приложений. Тем не менее пользователи будут работать с готовым приложением не в симуляторе, поэтому любое приложение необходимо тестировать на реальных устройствах как можно раньше и чаще. Дополнительные сведения о процессе подготовки для устройств iOS см. в разделе [Подготовка устройства](~/ios/get-started/installation/device-provisioning/index.md). Дополнительные сведения о процессе подготовки для устройств Android см. в разделе [Настройка устройства для разработки](~/android/get-started/installation/set-up-device-for-development.md).

## <a name="next-steps"></a>Следующие шаги

В этой статье приводятся общие сведения о разработке приложений с использованием Оболочки в Xamarin.Forms. Далее рекомендуется ознакомиться с перечисленными ниже функциональными возможностями.

- Оболочка Xamarin.Forms упрощает разработку мобильных приложений, предоставляя основные возможности, которые необходимы для большинства мобильных приложений. Дополнительные сведения см. в разделе [Оболочка Xamarin.Forms](~/xamarin-forms/app-fundamentals/shell/index.md).
- Для создания пользовательского интерфейса приложения Xamarin.Forms используются несколько групп элементов управления. Более подробную информацию см. в разделе [Справочник по элементам управления](~/xamarin-forms/user-interface/controls/index.md).
- Привязка данных — это способ связывания свойств двух объектов так, чтобы изменения в одном свойстве автоматически отражались в другом. Более подробную информацию см. в разделе [Привязка данных](~/xamarin-forms/app-fundamentals/data-binding/index.md).
- Xamarin.Forms предоставляет несколько различных способов перехода по страницам в зависимости от используемого типа страницы. Дополнительные сведения см. в разделе [Переходы](~/xamarin-forms/app-fundamentals/navigation/index.md).
- Стили помогают снизить повторяемость разметки и упрощают изменение внешнего вида приложения. Дополнительные сведения см. в разделе [Задание стиля приложений Xamarin.Forms](~/xamarin-forms/user-interface/styles/index.md).
- Шаблоны данных дают возможность настраивать представление данных в поддерживаемых представлениях. Дополнительные сведения см. в статье [Шаблоны данных](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).
- Эффекты также позволяют настраивать собственные элементы управления на каждой платформе. Эффекты создаются в проектах для конкретных платформ путем создания подклассов класса [`PlatformEffect`](xref:Xamarin.Forms.PlatformEffect`2) и используются путем их присоединения к соответствующему элементу управления Xamarin.Forms. Дополнительные сведения см. в статье [Эффекты](~/xamarin-forms/app-fundamentals/effects/index.md).
- Каждая страница, макет и представление отрисовываются по-разному на разных платформах с помощью класса `Renderer`, который создает собственный элемент управления, размещает его на экране и реализует поведение, определенное в общем коде. Разработчики могут реализовывать пользовательские классы `Renderer` для настройки внешнего вида или работы элемента управления. Дополнительные сведения см. в статье [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).
- Общий код может получать доступ к собственным функциональным возможностям посредством класса [`DependencyService`](xref:Xamarin.Forms.DependencyService). Дополнительные сведения см. в статье, посвященной [доступу к собственным функциональным возможностям с помощью DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

## <a name="related-links"></a>Связанные ссылки

- [Оболочка Xamarin.Forms](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Язык XAML](~/xamarin-forms/xaml/index.yml)
- [Привязка данных](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Справочник по элементам управления](~/xamarin-forms/user-interface/controls/index.md)
- [Примеры для начала работы](/samples/browse/?products=xamarin&term=Xamarin.Forms%2bget%2bstarted)
- [Примеры для Xamarin.Forms](/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Справочник по API Xamarin.Forms](xref:Xamarin.Forms)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Series/Xamarin-101/Xamarin-Solution-Architecture-4-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
