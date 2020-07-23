---
title: Демонстрация жизненного цикла приложения для Xamarin. iOS
description: В этом документе рассматриваются различные события жизненного цикла, обрабатываемые делегатом приложения в приложении iOS, демонстрирующие, когда и как обрабатываются эти события.
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/17/2018
ms.openlocfilehash: bb3fd0623d0361a42c573cf2b2bcb8249d32181c
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86933196"
---
# <a name="application-lifecycle-demo-for-xamarinios"></a>Демонстрация жизненного цикла приложения для Xamarin. iOS

В этой статье и [образце кода](https://docs.microsoft.com/samples/xamarin/ios-samples/lifecycledemo) демонстрируются четыре состояния приложений в iOS, а также роль `AppDelegate` методов уведомления приложения о том, что состояния изменяются. Приложение будет печатать обновления в консоли при каждом изменении состояния приложения:

[![Пример приложения](application-lifecycle-demo-images/image3-sml.png)](application-lifecycle-demo-images/image3.png#lightbox)

[![Приложение будет печатать обновления в консоли при каждом изменении состояния приложения](application-lifecycle-demo-images/image4.png)](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>Пошаговое руководство

1. Откройте проект **жизненного цикла** в решении **лифецикледемо** .
1. Откройте `AppDelegate` класс. В методы жизненного цикла добавлено ведение журнала, указывающее, когда приложение изменило состояние:

    ```csharp
    public override void OnActivated(UIApplication application)
    {
        Console.WriteLine("OnActivated called, App is active.");
    }
    public override void WillEnterForeground(UIApplication application)
    {
        Console.WriteLine("App will enter foreground");
    }
    public override void OnResignActivation(UIApplication application)
    {
        Console.WriteLine("OnResignActivation called, App moving to inactive state.");
    }
    public override void DidEnterBackground(UIApplication application)
    {
        Console.WriteLine("App entering background state.");
    }
    // not guaranteed that this will run
    public override void WillTerminate(UIApplication application)
    {
        Console.WriteLine("App is terminating.");
    }
    ```

1. Запустите приложение в симуляторе или на устройстве. `OnActivated`будет вызываться при запуске приложения. Теперь приложение находится в _активном_ состоянии.
1. Нажмите кнопку "домой" на симуляторе или устройстве, чтобы перенести приложение в фоновый режим. `OnResignActivation`и `DidEnterBackground` будут вызываться при переходе приложения из `Active` в `Inactive` `Backgrounded` состояние и. Так как для выполнения в фоновом режиме не задан код приложения, приложение считается _приостановленным_ в памяти.
1. Вернитесь к приложению, чтобы вернуть его на передний план. `WillEnterForeground`и `OnActivated` оба будут вызваны:

    ![Изменения состояния, выводимые на консоль](application-lifecycle-demo-images/image4.png)

    Следующая строка кода в контроллере представления выполняется, когда приложение перешел на передний план из фона и изменяет текст, отображаемый на экране:

    ```csharp
    UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
        label.Text = "Welcome back!";
    });
    ```

1. Нажмите кнопку **Главная** , чтобы перевести приложение в фоновый режим. Затем дважды коснитесь кнопки " **Главная** ", чтобы открыть переключатель приложения. На iPhone X проведите вверх от нижней части экрана:

    [![Переключатель приложений](application-lifecycle-demo-images/app-switcher-sml.png "Переключатель приложений")](application-lifecycle-demo-images/app-switcher.png#lightbox)
  
1. Выберите приложение в переключателе приложений и проведите вверх, чтобы удалить его (в iOS 11, длинное нажатие, пока Красные значки не появятся в углу):

    [![Проведите вверх, чтобы удалить работающее приложение](application-lifecycle-demo-images/app-switcher-swipe-sml.png "Проведите вверх, чтобы удалить работающее приложение")](application-lifecycle-demo-images/app-switcher-swipe.png#lightbox)

iOS завершит работу приложения. Обратите внимание, что `WillTerminate` не вызывается, так как приложение уже _приостановлено_ в фоновом режиме.

## <a name="related-links"></a>Связанные ссылки

- [Лифецикледемо (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/lifecycledemo)
