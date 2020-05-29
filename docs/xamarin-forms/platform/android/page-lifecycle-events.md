---
title: ''
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
ms.openlocfilehash: 76724ff17613fcebe35cb68518a1c932eee8aad7
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128730"
---
# <a name="page-lifecycle-events-on-android"></a>События жизненного цикла страницы в Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа для Android используется для отключения [`Disappearing`](xref:Xamarin.Forms.Page.Appearing) [`Appearing`](xref:Xamarin.Forms.Page.Appearing) событий и события страницы для приостановки и возобновления работы приложений, которые используют AppCompat. Кроме того, он включает возможность управлять отображением экранной клавиатуры при возобновлении, если она отображалась при приостановке при условии, что режим работы экранной клавиатуры установлен в значение [`WindowSoftInputModeAdjust.Resize`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize) .

> [!NOTE]
> Обратите внимание, что эти события включены по умолчанию, чтобы сохранить существующее поведение для приложений, зависящих от событий. Отключение этих событий делает цикл событий AppCompat соответствующим циклом событий AppCompat.

Эту платформу можно использовать в XAML, присвоив [`Application.SendDisappearingEventOnPause`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPauseProperty) [`Application.SendAppearingEventOnResume`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResumeProperty) [`Application.ShouldPreserveKeyboardOnResume`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResumeProperty) свойствам, и присоединенные `boolean` значения:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"             xmlns:androidAppCompat="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize"
             androidAppCompat:Application.SendDisappearingEventOnPause="false"
             androidAppCompat:Application.SendAppearingEventOnResume="false"
             androidAppCompat:Application.ShouldPreserveKeyboardOnResume="true">
  ...
</Application>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

Xamarin.Forms.Application.Current.On<Android>()
     .UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize)
     .SendDisappearingEventOnPause(false)
     .SendAppearingEventOnResume(false)
     .ShouldPreserveKeyboardOnResume(true);
```

`Application.Current.On<Android>`Метод указывает, что эта платформа будет запускаться только в Android. [ `Application.SendDisappearingEventOnPause` ] (Xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. AppCompat. Application. Сенддисаппеаринжевентонпаусе ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Приложение}, System. Boolean). в [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) пространстве имен используется для включения или отключения срабатывания [`Disappearing`](xref:Xamarin.Forms.Page.Appearing) события страницы, когда приложение переходит на задний план. [ `Application.SendAppearingEventOnResume` ] (Xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. AppCompat. Application. Сендаппеаринжевентонресуме ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Приложение}, System. Boolean)) используется для включения или отключения срабатывания [`Appearing`](xref:Xamarin.Forms.Page.Appearing) события страницы при выходе приложения из фона. [ `Application.ShouldPreserveKeyboardOnResume` ] (Xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. AppCompat. Application. Шаулдпресервекэйбоардонресуме ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Приложение}, System. Boolean). используется для управления отображением экранной клавиатуры при возобновлении, если оно было отображено при приостановке, при условии, что режим работы экранной клавиатуры установлен в значение [`WindowSoftInputModeAdjust.Resize`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize) .

В результате [`Disappearing`](xref:Xamarin.Forms.Page.Appearing) [`Appearing`](xref:Xamarin.Forms.Page.Appearing) события и страницы не будут срабатывать при приостановке приложения и возобновлении соответственно, и что если экранная клавиатура отображалась при приостановке приложения, она также будет отображаться при возобновлении работы приложения:

[![](page-lifecycle-events-images/keyboard-on-resume.png "Lifecycle Events Platform-Specific")](page-lifecycle-events-images/keyboard-on-resume-large.png#lightbox "Lifecycle Events Platform-Specific")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
