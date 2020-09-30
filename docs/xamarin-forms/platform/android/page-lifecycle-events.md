---
title: События жизненного цикла страницы в Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Android, которая отключает отображение и отображение событий страницы для приостановки и возобновления работы приложения соответственно.
ms.prod: xamarin
ms.assetid: F6E3759C-D347-407A-91A2-CF9B3B7D4CBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d4a8690e7361d58a07f4fbfa7aac8aac839c2ea3
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91564021"
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

[![События жизненного цикла — специфичные для платформы](page-lifecycle-events-images/keyboard-on-resume.png)](page-lifecycle-events-images/keyboard-on-resume-large.png#lightbox "События жизненного цикла — специфичные для платформы")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)