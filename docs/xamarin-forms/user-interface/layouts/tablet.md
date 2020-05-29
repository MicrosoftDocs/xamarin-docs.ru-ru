---
title: ''
description: В этой статье объясняется, как оптимизировать Xamarin.Forms макеты приложений для планшетов, а не для телефонов.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8ce5ba09f89c2bc84b7f6ba722f724ae39c0222e
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137943"
---
# <a name="layout-for-tablet-and-desktop-apps"></a>Макет для планшетных и настольных приложений

Xamarin.Formsподдерживает все типы устройств, доступные на поддерживаемых платформах, поэтому в дополнение к телефонам приложения также могут работать в:

- iPad
- Планшеты с Android,
- Планшетные и настольные компьютеры Windows (под управлением Windows 10).

На этой странице кратко рассматриваются следующие вопросы:

- Поддерживаемые [типы устройств](#Device_Types)и
- [Оптимизация](#optimize) макетов для планшетов и телефонов.

<a name="Device_Types" />

## <a name="device-types"></a>Типы устройств

Более крупные экранные устройства доступны для всех платформ, поддерживаемых Xamarin.Forms .

### <a name="ipads-ios"></a>iPad (iOS)

Xamarin.FormsШаблон автоматически включает поддержку iPad, настроив для параметра **Devices. plist > устройства** значение **Universal** (это означает, что поддерживаются iPhone и iPad).

Чтобы обеспечить приятный запуска и убедиться, что на всех устройствах используется полноэкранное разрешение, следует убедиться в том, что предоставлен [экран запуска iPad](~/ios/app-fundamentals/images-icons/launch-screens.md) (с использованием раскадровки). Это обеспечит правильную визуализацию приложения на устройствах iPad Mini, iPad и iPad Pro.

До версии iOS 9 все приложения заняли весь экран на устройстве, но некоторые iPad теперь могут выполнять [многозадачные разбиение экрана](~/ios/platform/multitasking.md).
Это означает, что ваше приложение может занимать только тонкий столбец на стороне экрана, 50% от ширины экрана или всего экрана.

[![](tablet-images/ipad-sml.png "iPad Split Screen Example")](tablet-images/ipad.png#lightbox "iPad Split Screen Example")

Работа с разделением экрана означает, что вы должны проектировать приложение так, чтобы оно работало с шириной в 320 пикселей в ширину или в ширину 1366 пикселов.

### <a name="android-tablets"></a>Планшеты с Android

Экосистема Android имеет множество поддерживаемых размеров экрана — от небольших телефонов до крупных планшетов. Xamarin.Formsподдерживает все размеры экрана, но, как и на других платформах, может потребоваться настроить пользовательский интерфейс для более крупных устройств.

При поддержке множества различных разрешений экрана можно предоставить ресурсы машинного образа в различных размерах, чтобы оптимизировать взаимодействие с пользователем.
Дополнительные сведения о том, как структурировать папки и имена файлов в проекте приложения Android для включения в приложение оптимизированных ресурсов изображений, см. в документации по [ресурсам Android](~/android/app-fundamentals/resources-in-android/index.md) (и в конкретном руководстве по [созданию ресурсов для различных размеров экрана](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)).

### <a name="windows-tablets-and-desktops"></a>Планшеты и рабочие столы Windows

Для поддержки планшетных и настольных компьютеров под управлением Windows необходимо использовать [поддержку Windows UWP](~/xamarin-forms/platform/windows/installation/index.md), которая создает универсальные приложения, работающие в Windows 10.

В дополнение к полноэкранному режиму приложения, работающие на планшетах и настольных компьютерах под управлением Windows, можно изменять размеры до произвольных измерений.

[![](tablet-images/splitscreen-sml.png "Windows Split Screen Example")](tablet-images/splitscreen.png#lightbox "Windows Split Screen Example")

<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>Оптимизация для планшетов и настольных компьютеров

Вы можете настроить Xamarin.Forms Пользовательский интерфейс в зависимости от того, используется ли телефонное или планшетное или настольное устройство. Это означает, что вы можете оптимизировать взаимодействие с пользователем для больших экранных устройств, например планшетов и настольных компьютеров.

### <a name="deviceidiom"></a>Device. идиом

Класс можно использовать [`Device`](~/xamarin-forms/platform/device.md) для изменения поведения приложения или пользовательского интерфейса. Используя `Device.Idiom` перечисление, можно

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

Этот подход можно расширить для внесения значительных изменений в отдельные макеты страниц или даже для отображения совершенно разных страниц на более крупных экранах.

### <a name="leveraging-masterdetailpage"></a>Использование Мастердетаилпаже

[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)Идеально подходит для более крупных экранов, особенно на iPad, где используется [`UISplitViewController`](xref:UIKit.UISplitViewController) для обеспечения собственного интерфейса iOS.

Просмотрите [эту запись блога Xamarin](https://devblogs.microsoft.com/xamarin/bringing-xamarin-forms-apps-to-tablets/) , чтобы увидеть, как можно адаптировать пользовательский интерфейс, чтобы на телефонах использовался один макет, а более крупные экраны могли использовать другое (с `MasterDetailPage` ).

## <a name="related-links"></a>Связанные ссылки

- [Блог Xamarin](https://devblogs.microsoft.com/xamarin/bringing-xamarin-forms-apps-to-tablets/)
- [Пример Мишоппе](https://github.com/jamesmontemagno/myshoppe)
