---
title: 'Xamarin.Essentials: Действия приложения'
description: Класс Accelerometer в Xamarin.Essentials позволяет создавать сочетания клавиш для значка приложения и настраивать реакцию на них.
ms.assetid: 5edf9bc5-b721-448c-a8a2-0a9d4d0c792c
author: jamesmontemagno
ms.author: jamont
ms.date: 01/04/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8b1da7d07c042ff10b48948c4f411d65c6c05002
ms.sourcegitcommit: d4d293174a8324ce82b8f961ae6eadce294cafd7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/14/2021
ms.locfileid: "98194891"
---
# <a name="no-locxamarinessentials-app-actions"></a>Xamarin.Essentials: Действия приложения

Класс **AppActions** позволяет создавать сочетания клавиш для значка приложения и настраивать реакцию на них.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

Для доступа к функции **AppActions** нужно создать описанную ниже конфигурацию для конкретной платформы.

# <a name="android"></a>[Android](#tab/android)

Добавьте фильтр намерения в класс `MainActivity`:

```csharp
[IntentFilter(
        new[] { Xamarin.Essentials.Platform.Intent.ActionAppAction },
        Categories = new[] { Android.Content.Intent.CategoryDefault })]
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
{
    ...
```

Затем добавьте следующую логику для обработки действий:

```csharp
protected override void OnResume()
{
    base.OnResume();

    Xamarin.Essentials.Platform.OnResume(this);
}

protected override void OnNewIntent(Android.Content.Intent intent)
{
    base.OnNewIntent(intent);

    Xamarin.Essentials.Platform.OnNewIntent(intent);
}
```

# <a name="ios"></a>[iOS](#tab/ios)

В `AppDelegate.cs` добавьте следующую логику для обработки действий:

```csharp
public override void PerformActionForShortcutItem(UIApplication application, UIApplicationShortcutItem shortcutItem, UIOperationHandler completionHandler)
    => Xamarin.Essentials.Platform.PerformActionForShortcutItem(application, shortcutItem, completionHandler);
```

# <a name="uwp"></a>[UWP](#tab/uwp)

В файле `App.xaml.cs` в методе `OnLaunched` добавьте следующую логику в конце метода:

```csharp
Xamarin.Essentials.Platform.OnLaunched(e);
```

-----

## <a name="create-actions"></a>Создание действий

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```
Действия приложения можно создавать в любое время, но зачастую они создаются при запуске приложения. Вызовите метод `SetAsync`, чтобы создать список действий для приложения.


```csharp
try
{
    await AppActions.SetAsync(
        new AppAction("app_info", "App Info", icon: "app_info_action_icon"),
        new AppAction("battery_info", "Battery Info"));
}
catch (FeatureNotSupportedException ex)
{
    Debug.WriteLine("App Actions not supported");
}
```

Если действия приложения не поддерживаются в конкретной версии операционной системы, будет вызвано исключение `FeatureNotSupportedException`. 

Для `AppAction` можно задать следующие свойства:

* Id — уникальный идентификатор, используемый для ответа на действие касания.
* Title — отображаемый заголовок.
* Subtitle — подзаголовок для отображения под заголовком, если поддерживается.
* Значок: должен соответствовать значкам в соответствующем каталоге ресурсов на каждой платформе.

![Действия приложения на главном экране](images/appactions.png)

## <a name="responding-to-actions"></a>Реагирование на действия

При запуске приложения зарегистрируйтесь для получения события `OnAppAction`. При выборе действия приложения будет отправлено событие со сведениями о том, какое действие было выбрано.

```csharp
public App()
{
    //...
    AppActions.OnAppAction += AppActions_OnAppAction;
}

void AppActions_OnAppAction(object sender, AppActionEventArgs e)
{
    // Don't handle events fired for old application instances
    // and cleanup the old instance's event handler
    if (Application.Current != this && Application.Current is App app)
    {
        AppActions.OnAppAction -= app.AppActions_OnAppAction;
        return;
    }
    MainThread.BeginInvokeOnMainThread(async () =>
    {
        await Shell.Current.GoToAsync($"//{e.AppAction.Id}");
    });
}
```

## <a name="getactions"></a>GetActions
Текущий список действий приложения можно получить, вызвав `AppActions.GetAsync()`.

## <a name="api"></a>API

- [Исходный код AppActions](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/AppActions)
- [Документация по API AppActions](xref:Xamarin.Essentials.AppActions)
