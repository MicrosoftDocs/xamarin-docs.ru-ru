---
title: Жизненный цикл приложения Xamarin.Forms
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3793a54f04b2c028752e18e2a5a238c275c2958a
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84129679"
---
# <a name="xamarinforms-app-lifecycle"></a>Жизненный цикл приложения Xamarin.Forms

Базовый класс [`Application`](xref:Xamarin.Forms.Application) предоставляет следующие возможности:

- [методы жизненного цикла](#Lifecycle_Methods) `OnStart`, `OnSleep` и `OnResume`;
- [события навигации по страницам](#page) [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) и [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing);
- [события модальной навигации](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping` и `ModalPopped`.

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>Методы жизненного цикла

Класс [`Application`](xref:Xamarin.Forms.Application) содержит три виртуальных метода, которые можно переопределить для реагирования на изменения жизненного цикла:

- `OnStart` — вызывается при запуске приложения;
- `OnSleep` — вызывается каждый раз, когда приложение переводится в фоновый режим;
- `OnResume` — вызывается каждый раз, когда приложение выводится из фонового режима и его работа возобновляется.

> [!NOTE]
> Метода, отвечающего за завершение работы приложения, *не существует*. В обычных условиях (то есть не в случае сбоя) завершение работы приложения происходит из состояния *OnSleep* без дополнительных уведомлений в коде.

Чтобы пронаблюдать за вызовом этих методов, реализуйте вызов `WriteLine` в каждом из них (как показано ниже) и протестируйте приложение на каждой платформе.

```csharp
protected override void OnStart()
{
    Debug.WriteLine ("OnStart");
}
protected override void OnSleep()
{
    Debug.WriteLine ("OnSleep");
}
protected override void OnResume()
{
    Debug.WriteLine ("OnResume");
}
```

> [!IMPORTANT]
> В Android метод `OnStart` будет вызываться при каждом повороте экрана, а не только при первом запуске приложения, если для основного действия не указано `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation` в атрибуте `[Activity()]`.

<a name="page" />

## <a name="page-notification-events"></a>События уведомлений о страницах

В классе [`Application`](xref:Xamarin.Forms.Application) есть два события, которые уведомляют о появлении страниц на экране и их исчезновении с экрана:

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) — возникает, когда страница скоро появится на экране;
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) — возникает, когда страница скоро пропадет с экрана.

Эти события можно использовать, когда нужно отслеживать появление страниц на экране.

> [!NOTE]
> События [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) и [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) вызываются из базового класса [`Page`](xref:Xamarin.Forms.Page) сразу после событий [`Page.Appearing`](xref:Xamarin.Forms.Page.Appearing) и [`Page.Disappearing`](xref:Xamarin.Forms.Page.Disappearing) соответственно.

<a name="modal" />

## <a name="modal-navigation-events"></a>События модальной навигации

В классе [`Application`](xref:Xamarin.Forms.Application) есть четыре события (каждый с собственными аргументами), которые позволяют реагировать на открытие и закрытие модальных страниц:

- `ModalPushing` — возникает при отправке страницы в модальном режиме.
- `ModalPushed` — возникает после отправки страницы в модальном режиме.
- `ModalPopping` — возникает при извлечении страницы в модельном режиме.
- `ModalPopped` — возникает после извлечения страницы в модельном режиме.

> [!NOTE]
> Аргументы событий `ModalPopping` с типом `ModalPoppingEventArgs` содержат свойство `Cancel`. когда свойству `Cancel` присваивается значение `true`, модальное всплывающее окно отменяется;
