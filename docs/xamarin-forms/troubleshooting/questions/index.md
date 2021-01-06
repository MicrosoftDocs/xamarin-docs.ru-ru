---
title: Xamarin.Forms Часто задаваемые вопросы
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 89364175-53BA-4A09-B3E2-44AC67DD971C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/15/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e520d222029e0d29a25f16aacfb6273da52cbf9e
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2021
ms.locfileid: "97940217"
---
# <a name="no-locxamarinforms-frequently-asked-questions"></a>Xamarin.Forms Часто задаваемые вопросы

## <a name="how-do-i-migrate-my-app-to-no-locxamarinforms-50"></a>[Разделы справки перенести приложение в Xamarin.Forms 5,0?](forms5-migration.md)

Для переноса приложения в Xamarin.Forms 5,0 требуется знание его критических изменений.

## <a name="can-i-update-the-no-locxamarinforms-default-template-to-a-newer-nuget-package"></a>[Можно ли обновить Xamarin.Forms шаблон по умолчанию до более нового пакета NuGet?](update-forms-template.md)

В этом руководством в Xamarin.Forms качестве примера используется шаблон библиотеки .NET Standard, но один и тот же общий метод также будет работать для Xamarin.Forms шаблона общего проекта.

## <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-no-locxamarinforms-xaml-files"></a>[Почему конструктор XAML Visual Studio не работает с Xamarin.Forms файлами XAML?](forms-xaml-designer.md)

Xamarin.Forms Сейчас не поддерживает визуальные конструкторы для файлов XAML.

## <a name="android-build-error-the-linkassemblies-task-failed-unexpectedly"></a>[Ошибка сборки Android: непредвиденный сбой задачи "Линкассемблиес"](android-linkassemblies-error.md)

`The "LinkAssemblies" task failed unexpectedly`При построении проекта Xamarin. Android, использующего формы, может появиться сообщение об ошибке. Это происходит, если компоновщик активен (обычно в сборке *выпуска* , чтобы уменьшить размер пакета приложения); Это происходит потому, что целевые объекты Android не обновляются до последней версии платформы.

## <a name="why-does-my-no-locxamarinformsmaps-android-project-fail-with-compiletodalvik--unexpected-top-level-error"></a>[«Почему Xamarin.Forms . Произошел сбой проекта Android с КОМПИЛЕТОДАЛВИК: НЕПРЕДВИДЕНная ошибка верхнего уровня? "](maps-compiletodalvik-error.md)

Эта ошибка может отображаться на панели ошибок Visual Studio для Mac или в окне вывода сборки Visual Studio. в проектах Android с помощью Xamarin.Forms . Указания. Чаще всего это устраняется путем увеличения размера кучи Java для проекта Xamarin. Android.
