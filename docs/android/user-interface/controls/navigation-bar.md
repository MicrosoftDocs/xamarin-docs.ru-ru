---
title: Панель навигации Xamarin. Android
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/01/2017
ms.openlocfilehash: 67c5655c3bbea8cd0a8c21f27719221f599bf481
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457436"
---
# <a name="xamarinandroid-navigation-bar"></a>Панель навигации Xamarin. Android

В Android 4 появилась новая функция пользовательского интерфейса системы, называемая *панелью навигации*, которая предоставляет элементы управления навигацией на устройствах, которые не включают аппаратные кнопки для **домашних**, **задних**и **меню**.
На следующем снимке экрана показана панель навигации из устройства для создания хранилища:

 [![Пример панели навигации Android](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png#lightbox)

Доступно несколько новых флагов, контролирующих видимость панели навигации и ее элементов управления, а также видимость системной панели, представленной в Android 3. Флаги определены в `Android.View.View` классе и перечислены ниже:

- `SystemUiFlagVisible`&ndash;Делает панель навигации видимой.
- `SystemUiFlagLowProfile`&ndash;Затемнение элементов управления на панели навигации.
- `SystemUiFlagHideNavigation`&ndash;Скрывает панель навигации.

Эти флаги можно применить к любому представлению в иерархии представлений, задав `SystemUiVisibility` свойство. Если для нескольких представлений задано это свойство, система объединяет их с операцией или и применяет их до тех пор, пока в окне, в котором установлены флаги, остается фокус. При удалении представления все установленные флаги также будут удалены.

В следующем примере показано простое приложение, при щелчке на любой из кнопок изменяется `SystemUiVisibility` :

 [![Снимки экрана, демонстрирующие видимые, низкие профили и скрытые Системуивисибилити](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png#lightbox)

Код для изменения `SystemUiVisibility` задает свойство для `TextView` обработчика событий нажатия для каждой кнопки, как показано ниже:

```csharp
var tv = FindViewById<TextView> (Resource.Id.systemUiFlagTextView);
var lowProfileButton = FindViewById<Button>(Resource.Id.lowProfileButton);
var hideNavButton = FindViewById<Button> (Resource.Id.hideNavigation);
var visibleButton = FindViewById<Button> (Resource.Id.visibleButton);

lowProfileButton.Click += delegate {
    tv.SystemUiVisibility =
        (StatusBarVisibility)View.SystemUiFlagLowProfile;
};

hideNavButton.Click += delegate {
    tv.SystemUiVisibility =
       (StatusBarVisibility)View.SystemUiFlagHideNavigation;        
};

visibleButton.Click += delegate {
    tv.SystemUiVisibility = (StatusBarVisibility)View.SystemUiFlagVisible;
}
```

Кроме того, `SystemUiVisibility` изменение вызывает `SystemUiVisibilityChange` событие. Как и при задании `SystemUiVisibility` свойства, обработчик `SystemUiVisibilityChange` события может быть зарегистрирован для любого представления в иерархии. Например, приведенный ниже код использует `TextView` экземпляр для регистрации события:

```csharp
tv.SystemUiVisibilityChange +=
  delegate(object sender, View.SystemUiVisibilityChangeEventArgs e) {
        tv.Text = String.Format ("Visibility = {0}", e.Visibility);
  };
```

## <a name="related-links"></a>Связанные ссылки

- [Системуивисибилитидемо (пример)](/samples/xamarin/monodroid-samples/systemuivisibilitydemo)