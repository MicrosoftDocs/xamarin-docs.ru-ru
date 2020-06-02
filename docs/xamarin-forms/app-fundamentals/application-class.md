---
title: Класс приложения Xamarin.Forms
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
ms.custom: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2f4b568809f949d813de86ae88cbe621cfa3161c
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84129692"
---
# <a name="xamarinforms-app-class"></a>Класс приложения Xamarin.Forms

Базовый класс `Application` предлагает следующие возможности, которые предоставляются в используемом по умолчанию подклассе `App` проекта:

* Свойство `MainPage` используется для задания начальной страницы для приложения.
* Сохраняемый [словарь `Properties`](#Properties_Dictionary) для хранения простых значений в рамках изменений состояния жизненного цикла.
* Статическое свойство `Current`, содержащее ссылку на текущий объект приложения.

Он также предоставляет [методы жизненного цикла](~/xamarin-forms/app-fundamentals/app-lifecycle.md), например `OnStart`, `OnSleep` и `OnResume`, а также события модальной навигации.

В зависимости от выбранного шаблона класс `App` можно определить одним из двух способов:

* **C#** или
* **XAML и C#**

Чтобы создать класс **App** с помощью XAML, необходимо заменить класс **App** по умолчанию классом **App** XAML и соответствующим кодом программной части, как показано в следующем примере кода:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Photos.App">

</Application>
```

В следующем примере показан соответствующий код программной части:

```csharp
public partial class App : Application
{
    public App ()
    {
        InitializeComponent ();
        MainPage = new HomePage ();
    }
    ...
}
```

Помимо задания свойства [`MainPage`](xref:Xamarin.Forms.Application.MainPage), в коде программной части должен вызываться метод `InitializeComponent` для загрузки и анализа соответствующего кода XAML.

## <a name="mainpage-property"></a>Свойство MainPage

Свойство `MainPage` в классе `Application` задает корневую страницу приложения.

Например, вы можете создать в своем классе `App` логику для отображения другой страницы в зависимости от того, вошел ли пользователь в систему.

Свойство `MainPage` должно быть задано в конструкторе `App`.

```csharp
public class App : Xamarin.Forms.Application
{
    public App ()
    {
        MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
    }
}
```

<a name="Properties_Dictionary" />

## <a name="properties-dictionary"></a>Словарь Properties

Подкласс `Application` имеет статический словарь `Properties`, который позволяет хранить данные, в частности, для использования в методах `OnStart`, `OnSleep` и `OnResume`. Обратиться к нему можно из любой части кода Xamarin.Forms с помощью `Application.Current.Properties`.

Словарь `Properties` использует ключ `string` и сохраняет значение `object`.

Например, можно задать постоянное свойство `"id"` в любой части кода (при выборе элемента в методе `OnDisappearing` страницы или в методе `OnSleep`) следующим образом:

```csharp
Application.Current.Properties ["id"] = someClass.ID;
```

В методах `OnStart` или `OnResume` это значение можно использовать для воссоздания взаимодействия с пользователем некоторым образом. Словарь `Properties` хранит объекты `object`, поэтому вам не требуется приводить его значение перед использованием.

```csharp
if (Application.Current.Properties.ContainsKey("id"))
{
    var id = Application.Current.Properties ["id"] as int;
    // do something with id
}
```

Всегда проверяйте наличие ключа перед обращением, чтобы предотвратить непредвиденные ошибки.

> [!NOTE]
> Словарь `Properties` может сериализировать только примитивные типы для хранения. Попытка сохранить другие типы (например, `List<string>`) может завершиться сбоем без каких-либо сообщений.

<!-- bugzilla 28657 -->

### <a name="persistence"></a>Сохраняемость

Словарь `Properties` автоматически сохраняется на устройстве.
Данные, добавленные в словарь, будут доступны при возврате приложения в фоновый режим или даже после его перезапуска.

В Xamarin.Forms 1.4 представлен дополнительный метод класса `Application` — `SavePropertiesAsync()`, который можно вызвать для упреждающего сохранения словаря `Properties`. Это позволяет сохранить свойства после важных обновлений, не рискуя, что они будут отсериализированы из-за сбоя либо завершены операционной системой.

Ссылки на использование словаря `Properties` можно найти в главах [6](https://developer.xamarin.com/r/xamarin-forms/book/chapter06.pdf), [15](https://developer.xamarin.com/r/xamarin-forms/book/chapter15.pdf) и [20](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) книги о **создании мобильных приложений с помощью Xamarin.Forms** , а также в соответствующих [примерах](https://github.com/xamarin/xamarin-forms-book-preview-2).

## <a name="the-application-class"></a>Класс Application

Полная реализация класса `Application` приведена ниже для справки:

```csharp
public class App : Xamarin.Forms.Application
{
    public App ()
    {
        MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
    }

    protected override void OnStart()
    {
        // Handle when your app starts
        Debug.WriteLine ("OnStart");
    }

    protected override void OnSleep()
    {
        // Handle when your app sleeps
        Debug.WriteLine ("OnSleep");
    }

    protected override void OnResume()
    {
        // Handle when your app resumes
        Debug.WriteLine ("OnResume");
    }
}
```

Затем в каждом проекте конкретной платформы создается экземпляр этого класса, который затем передается в метод `LoadApplication`. Там `MainPage` загружается и отображается для пользователя.
Код для каждой из платформ приведен в разделах ниже. Последние шаблоны решений Xamarin.Forms уже содержат весь этот код, предварительно настроенный для вашего приложения.

### <a name="ios-project"></a>Проект iOS

Класс `AppDelegate` iOS наследует от `FormsApplicationDelegate`. Он должен:

* вызывать `LoadApplication` с экземпляром класса `App`;

* всегда возвращать `base.FinishedLaunching (app, options);`.

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate :
    global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate // superclass new in 1.3
{
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init ();

        LoadApplication (new App ());  // method is new in 1.3

        return base.FinishedLaunching (app, options);
    }
}
```

### <a name="android-project"></a>Проект Android

Класс `MainActivity` Android наследует от `FormsAppCompatActivity`. В переопределении `OnCreate` метод `LoadApplication` вызывается с помощью экземпляра класса `App`.

```csharp
[Activity (Label = "App Lifecycle Sample", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true,
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity : FormsAppCompatActivity
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

### <a name="universal-windows-project-uwp-for-windows-10"></a>Проект для универсальной платформы Windows (UWP) для Windows 10

Главная страница в проекте UWP должна быть производной от `WindowsPage`:

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
   ...>
</forms:WindowsPage>
```

Код C#, отвечающий за создание, должен вызывать `LoadApplication` для создания экземпляра вашего `App` Xamarin.Forms. Обратите внимание, что рекомендуется явно использовать пространство имен приложения для уточнения `App`, так как приложения UWP также имеют свой собственный класс `App`, не связанный с Xamarin.Forms.

```csharp
public sealed partial class MainPage
{
    public MainPage()
    {
        InitializeComponent();

        LoadApplication(new YOUR_NAMESPACE.App());
    }
 }
```

Обратите внимание, что `Forms.Init()` в проекте UWP следует вызывать из **App.xaml.cs**.

Дополнительные сведения см. в разделе [Настройка проектов Windows](~/xamarin-forms/platform/windows/installation/index.md), в котором указаны шаги для добавления проекта UWP в существующее решение Xamarin.Forms, которое не предназначено для UWP.

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Series/Xamarin-101/Xamarin-Solution-Architecture-4-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
