---
Title: "начало работы со страницами данных" Описание: "в этой статье объясняется, как приступить к созданию простой страницы, управляемой данными, с помощью Xamarin.Forms страниц данных".
MS. произв. Xamarin MS. AssetID: 6416E5FA-6384-4298-BAA1-A89381E47210 MS. Technology: автор Xamarin-Forms: давидбритч MS. author: дабритч MS. Дата: 12/01/2017 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="getting-started-with-datapages"></a>начало работы со страницами с данными

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)

![](~/media/shared/preview.png "This API is currently in preview")

> [!IMPORTANT]
> Для отображения страниц с наборами элементов требуется Xamarin.Forms ссылка на тему. Это включает установку [ Xamarin.Forms . Тему. базовый](https://www.nuget.org/packages/Xamarin.Forms.Theme.Base/) пакет NuGet в проекте, а затем — [ Xamarin.Forms . Theme. light](https://www.nuget.org/packages/Xamarin.Forms.Theme.Light/) или [ Xamarin.Forms . Themes. темные](https://www.nuget.org/packages/Xamarin.Forms.Theme.Dark/) пакеты NuGet.

Чтобы приступить к созданию простой страницы, управляемой данными, с помощью предварительной версии страниц данных, выполните следующие действия. В этой демонстрации используется жестко закодированный стиль ("события") в сборках предварительного просмотра, которые работают только с конкретным форматом JSON в коде.

[![](get-started-images/demo-sml.png "DataPages Sample Application")](get-started-images/demo.png#lightbox "DataPages Sample Application")

## <a name="1-add-nuget-packages"></a>1. Добавление пакетов NuGet

Добавьте эти пакеты NuGet в Xamarin.Forms библиотеку .NET Standard и проекты приложений:

- Xamarin.Forms. См
- Xamarin.Forms. Тема. базовый
- Набор NuGet реализации темы (например, Xamarin.Forms. Theme. Light)

## <a name="2-add-theme-reference"></a>2. Добавление ссылки на тему

В файле **app. XAML** добавьте пользовательский элемент `xmlns:mytheme` для темы и убедитесь, что тема объединена в словарь ресурсов приложения:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
  xmlns:mytheme="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Light"
  x:Class="DataPagesDemo.App">
    <Application.Resources>
        <ResourceDictionary MergedWith="mytheme:LightThemeResources" />
    </Application.Resources>
</Application>
```

> [!IMPORTANT]
> Также необходимо выполнить действия по [загрузке сборок темы (приведенных ниже)](#troubleshooting) , добавив в iOS `AppDelegate` и Android некоторый стандартный код `MainActivity` . Это будет улучшено в будущих выпусках.

## <a name="3-add-a-xaml-page"></a>3. Добавление страницы XAML

Добавьте в приложение новую страницу XAML Xamarin.Forms и *измените базовый класс* с `ContentPage` на `Xamarin.Forms.Pages.ListDataPage` . Это необходимо сделать как в C#, так и в XAML:

**Файл C#**

```csharp
public partial class SessionDataPage : Xamarin.Forms.Pages.ListDataPage // was ContentPage
{
  public SessionDataPage ()
  {
    InitializeComponent ();
  }
}
```

**XAML-файл**

Помимо изменения корневого элемента на `<p:ListDataPage>` пользовательское пространство имен для `xmlns:p` необходимо также добавить:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage">

    <ContentPage.Content></ContentPage.Content>

</p:ListDataPage>
```

**Подкласс приложения**

Измените `App` конструктор класса таким образом, чтобы `MainPage` для свойства было задано значение, `NavigationPage` содержащее новый объект `SessionDataPage` . *Необходимо* использовать страницу навигации.

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3. Добавление источника данных

Удалите `Content` элемент и замените его на, `p:ListDataPage.DataSource` чтобы заполнить страницу данными. В примере ниже удаленный файл данных JSON загружается из URL-адреса.

> [!NOTE]
> Предварительный просмотр *требует наличия* `StyleClass` атрибута для предоставления подсказок отрисовки для источника данных. Объект `StyleClass="Events"` ссылается на макет, предопределенный в предварительной версии, и содержит стили с *жестко* заданными в соответствии с используемым источником данных JSON.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage"
             Title="Sessions" StyleClass="Events">

    <p:ListDataPage.DataSource>
        <p:JsonDataSource Source="http://demo3143189.mockable.io/sessions" />
    </p:ListDataPage.DataSource>

</p:ListDataPage>
```

**Данные JSON**

Ниже приведен пример данных JSON из демонстрационного источника.

```json
[{
  "end": "2016-04-27T18:00:00Z",
  "start": "2016-04-27T17:15:00Z",
  "abstract": "The new Apple TV has been released, and YOU can be one of the first developers to write apps for it. To make things even better, you can build these apps in C#! This session will introduce the basics of how to create a tvOS app with Xamarin, including: differences between tvOS and iOS APIs, TV user interface best practices, responding to user input, as well as the capabilities and limitations of building apps for a television. Grab some popcorn—this is going to be good!",
  "title": "As Seen On TV … Bringing C# to the Living Room",
  "presenter": "Matthew Soucoup",
  "biography": "Matthew is a Xamarin MVP and Certified Xamarin Developer from Madison, WI. He founded his company Code Mill Technologies and started the Madison Mobile .Net Developers Group.  Matt regularly speaks on .Net and Xamarin development at user groups, code camps and conferences throughout the Midwest. Matt gardens hot peppers, rides bikes, and loves Wisconsin micro-brews and cheese.",
  "image": "http://i.imgur.com/ASj60DP.jpg",
  "avatar": "http://i.imgur.com/ASj60DP.jpg",
  "room": "Crick"
}]
```

## <a name="4-run"></a>4. Запустите!

Приведенные выше действия должны привести к созданию рабочей страницы данных:

[![](get-started-images/demo-sml.png "DataPages Sample Application")](get-started-images/demo.png#lightbox "DataPages Sample Application")

Это работает потому, что предварительно построенный стиль **"события"** существует в пакете NuGet светлой темы и имеет определенные стили, соответствующие источнику данных (например, "Title", "Image", "Presenter").

Событие `StyleClass` создается для того, чтобы отобразить `ListDataPage` элемент управления с пользовательским `CardView` элементом управления, который определен в Xamarin.Forms . См. `CardView`Элемент управления имеет три свойства: `ImageSource` , `Text` и `Detail` . Тема жестко привязана для привязки трех полей DataSource (из JSON-файла) к этим свойствам для вывода.

## <a name="5-customize"></a>5. Настройка

Наследуемый стиль может быть переопределен путем указания шаблона и использования привязок к источнику данных. В приведенном ниже коде XAML объявляется пользовательский шаблон для каждой строки с использованием нового `ListItemControl` `{p:DataSourceBinding}` синтаксиса и, включенного в ** Xamarin.Forms . Страницы** NuGet:

```xaml
<p:ListDataPage.DefaultItemTemplate>
    <DataTemplate>
        <ViewCell>
            <p:ListItemControl
                Title="{p:DataSourceBinding title}"
                Detail="{p:DataSourceBinding room}"
                ImageSource="{p:DataSourceBinding image}"
                DataSource="{Binding Value}"
                HeightRequest="90"
            >
            </p:ListItemControl>
        </ViewCell>
    </DataTemplate>
</p:ListDataPage.DefaultItemTemplate>
```

Предоставляя `DataTemplate` этот код, переопределяет `StyleClass` и вместо этого использует макет по умолчанию для `ListItemControl` .

[![](get-started-images/custom-sml.png "DataPages Sample Application")](get-started-images/custom.png#lightbox "DataPages Sample Application")

Разработчики, предпочитающие C# to XAML, могут также создавать привязки к источникам данных (не забывайте включать `using Xamarin.Forms.Pages;` инструкцию):

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```

Создавать темы с нуля немного сложнее, но будущие выпуски предварительной версии сделают это проще.

## <a name="troubleshooting"></a>Устранение неполадок

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Не удалось загрузить файл или сборку Xamarin.Forms . Theme. Light ' или одна из его зависимостей

В предварительной версии темы не могут загружаться во время выполнения. Добавьте приведенный ниже код в соответствующие проекты, чтобы устранить эту ошибку.

**iOS**

В **AppDelegate.CS** добавьте следующие строки после`LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

В **MainActivity.CS** добавьте следующие строки после`LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```

## <a name="related-links"></a>Связанные ссылки

- [Пример Датапажесдемо](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)
