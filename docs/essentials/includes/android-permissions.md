---
ms.topic: include
author: jamesmontemagno
ms.author: jamont
ms.date: 05/11/2020
ms.openlocfilehash: b5396061de657690fa3f4cdf68e8a94382918613
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/13/2020
ms.locfileid: "83149735"
---
Этот API использует разрешения среды выполнения для Android. Проверьте, что набор Xamarin.Essentials полностью инициализирован и в вашем приложении настроена обработка разрешений.

В `MainLauncher` проекта Android или в любом запущенном действии `Activity` следует инициализировать Xamarin.Essentials в методе `OnCreate` следующим образом:

```csharp
protected override void OnCreate(Bundle savedInstanceState) 
{
    //...
    base.OnCreate(savedInstanceState);
    Xamarin.Essentials.Platform.Init(this, savedInstanceState); // add this line to your code, it may also be called: bundle
    //...
}    
```

Для обработки на устройстве Android разрешений среды выполнения Xamarin.Essentials нужно получить любой `OnRequestPermissionsResult`. Добавьте следующий код во все классы `Activity`:

```csharp
public override void OnRequestPermissionsResult(int requestCode, string[] permissions, Android.Content.PM.Permission[] grantResults)
{
    Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

    base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
}
```
