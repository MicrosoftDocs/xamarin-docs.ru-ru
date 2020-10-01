---
title: Значок приложения для приложений Xamarin.Mac
description: В этой статье описано, как создавать изображения, необходимые для использования значков приложения Хamarin.Mac, объединения изображений в файл ICNS и включения значков в проект Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 675b9405-d9a7-49f0-94ad-417f10a71d11
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: fdc132143905db6cc32ebd9aeb07268e69bf2070
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91434685"
---
# <a name="application-icon-for-xamarinmac-apps"></a>Значок приложения для приложений Xamarin.Mac

_В этой статье описано, как создавать изображения, необходимые для использования значков приложения Хamarin.Mac, объединения изображений в файл ICNS и включения значков в проект Xamarin.Mac._

## <a name="overview"></a>Обзор

При работе с C# и .NET в приложении Xamarin.Mac разработчик может использовать те же средства создания изображений и значков, что и при работе с *Objective-C* и *Xcode*.

Первоклассный значок должен передавать основное назначение приложения Xamarin.Mac и давать представление об ожидаемых действиях при использовании приложения. В этой статье рассматриваются все шаги по созданию необходимых для значка ресурсов изображений, упаковке этих ресурсов в файл `AppIcon.appiconset` и использовании этого файла в приложении Xamarin.Mac.

![Редактор AppIcon.appiconset](app-icon-images/intro01.png "Редактор AppIcon.appiconset")

## <a name="application-icon"></a>Значок приложения

Первоклассный значок должен передавать основное назначение приложения Xamarin.Mac и давать представление об ожидаемых действиях при использовании приложения. Каждое приложение macOS должно поддерживать несколько размеров для отображения значка в программе Finder, на панели Dock, панели запуска и в других местах на компьютере.

## <a name="designing-the-icon"></a>Разработка значков

При разработке значков рекомендуется принимать во внимание приведенные далее советы от компании Apple:

- Постарайтесь придать значку реалистичную и уникальную форму.
- Если приложение macOS имеет аналог на платформе iOS, не используйте значок приложения iOS повторно.
- Используйте универсальное и легко узнаваемое изображение.
- Стремитесь к простоте.
- Применяйте цвета и тени с осторожностью, чтобы правильно передать назначение приложения.
- Старайтесь не смешивать реальный текст с _непонятным_ текстом или строками.
- Создайте идеализированную версию темы значка, а не используйте реальную фотографию.
- Избегайте использования элементов пользовательского интерфейса macOS в значках.
- Не используйте в значках копии значков Apple.

Перед разработкой значка приложения Xamarin.Mac ознакомьтесь с разделами о [коллекции значков приложений](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Gallery.html#//apple_ref/doc/uid/20000957-CH88-SW1) и [разработке значков приложений](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Designing.html#//apple_ref/doc/uid/20000957-CH87-SW1) на странице [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) (Рекомендации по работе с человеческим интерфейсом OS X) веб-сайта Apple.

## <a name="required-image-sizes-and-filenames"></a>Требуемые размеры изображений и имена файлов

Как и для любого другого ресурса изображения, который разработчик будет использовать в приложении Xamarin.Mac, для значка приложения необходимы версии стандартного разрешения и разрешения Retina. И опять же, как и для любого другого изображения, при именовании файлов значков следует использовать формат `@2x`:

- **Стандартное разрешение**  - _имя_изображения_ **.** _расширение_имени_файла_ (например, **icon_512x512.png**).
- **Высокое разрешение**  - _имя_изображения_ **@2x.** _расширение_имени_файла_ (например, **icon_512x512@2x.png** ).

Например, чтобы указать версию значка приложения 512 x 512, именем файла будет **icon_512x512.png** и **icon_512x512@2x.png** .

Чтобы значок отлично смотрелся во всех местах, где его видят пользователи, необходимо предоставить ресурсы в приведенных ниже размерах:

|имя_файла|Размер в пикселях|
|---|---|
|icon_512x512@2x.png|1024 x 1024|
|icon_512x512.png|512 x 512|
|icon_256x256@2x.png|512 x 512|
|icon_256x256.png|256 x 256|
|icon_128x128@2x.png|256 x 256|
|icon_128x128.png|128 x 128|
|icon_32x32@2x.png|64 x 64|
|icon_32x32.png|32 x 32|
|icon_16x16@2x.png|32 x 32|
|icon_16x16.png|16 x 16|

Дополнительные сведения см. в документации Apple о [предоставлении версий высокого разрешения всех графических ресурсов приложения](https://developer.apple.com/library/mac/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Optimizing/Optimizing.html#//apple_ref/doc/uid/TP40012302-CH7-SW3).

## <a name="packaging-the-icon-resources"></a>Упаковка ресурсов значка

После разработки и сохранения значка с применением требуемых размеров и имен файлов в Visual Studio для Mac можно легко назначать их ресурсам изображений для использования в Xamarin.Mac.

Выполните следующие действия:

1. На **Панели решения** откройте **Assets.xcassets** > **AppIcons.appiconset**: 

    ![Редактирование AppIcon.appiconset](app-icon-images/intro01.png "Редактирование AppIcon.appiconset")
2. Для каждого необходимого размера значка щелкните значок и выберите соответствующий файл изображения, созданный выше: 

    [![Выбор изображения значка](app-icon-images/intro02.png "Выбор изображения значка")](app-icon-images/intro02-large.png#lightbox)
3. Сохраните изменения.

## <a name="using-the-icon"></a>Использование значка

Созданный файл `AppIcon.appiconset` необходимо назначить проекту Xamarin.Mac в Visual Studio для Mac.

Выполните следующие действия:

1. Дважды щелкните **Info.plist** на **Панели решения**, чтобы открыть окно **Параметры проекта**.
2. В разделе **Целевая платформа приложения Mac OS X** щелкните **Значки приложений**, чтобы выбрать файл `AppIcon.appiconset`: 

    [![Установка набора значков](app-icon-images/icon01.png "Установка набора значков")](app-icon-images/icon01-large.png#lightbox)
3. Сохраните изменения.

При запуске приложения на панели Dock появится новый значок:

![Пример значка приложения на панели Dock в macOS](app-icon-images/icon04.png "Пример значка приложения на панели Dock в macOS")

## <a name="summary"></a>Сводка

В этой статье описаны принципы работы с изображениями, необходимыми для создания значков приложений macOS, упаковки значков и их включения в проект Xamarin.Mac.

## <a name="related-links"></a>Связанные ссылки

- [MacImages (пример)](/samples/xamarin/mac-samples/macimages)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Работа с образами](~/mac/app-fundamentals/image.md)
- [Рекомендации по работе с человеческим интерфейсом iOS. Значки и изображения](https://developer.apple.com/macos/human-interface-guidelines/icons-and-images/image-size-and-resolution/)
- [Сведения о высоком разрешении для OS X](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
- [Построитель значков](https://itunes.apple.com/us/app/icns-builder/id554660130?mt=12)