---
title: Visual Basic и .NET Standard
description: В этом руководство объясняется, как Visual Basic можно использовать для написания проектов .NET Standard, которые можно использовать в решениях, предназначенных для Xamarin. iOS и Xamarin. Android.
ms.prod: xamarin
ms.assetid: f264c632-8feb-4015-a5e5-cb9c681c787d
author: davidortinau
ms.author: daortin
ms.date: 04/24/2019
ms.openlocfilehash: 8ccf96e266f9d5eae69a178cfcad3d1e48fb6962
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457538"
---
# <a name="visual-basic-and-net-standard"></a>Visual Basic и .NET Standard

Проекты Xamarin Android и iOS изначально не поддерживают Visual Basic; Однако разработчики могут использовать библиотеки [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) для переноса существующего кода Visual Basic в Android и iOS или писать значительную часть логики приложения в Visual Basic. Приложения Xamarin. Forms можно полностью создавать в Visual Basic (за исключением пользовательских модулей подготовки отчетов, служб зависимостей и кода XAML).

## <a name="requirements"></a>Требования

Для создания и компиляции Visual Basic библиотек .NET Standard необходимо использовать Visual Studio в Windows (Visual Studio 2017 или более поздней версии).

> [!NOTE]
> Библиотеки Visual Basic можно создавать и компилировать только с помощью Visual Studio. Xamarin. Android и Xamarin. iOS не поддерживают язык Visual Basic.
>
> Если вы работаете только в Visual Studio, вы можете ссылаться на проект Visual Basic из проектов Xamarin. Android и Xamarin. iOS.
>
> Если проекты Android и iOS также должны быть загружены в Visual Studio для Mac следует ссылаться на выходную сборку из сборки Visual Basic.

## <a name="creating-a-visual-basicnet-net-standard-library"></a>Создание библиотеки .NET Standard Visual Basic.NET

В этом разделе описано, как создать библиотеку Visual Basic .NET Standard с помощью Visual Studio 2019.
Затем на библиотеку можно ссылаться в других проектах, включая приложения Xamarin. Android, Xamarin. iOS и Xamarin. Forms.

При добавлении библиотеки .NET Standard Visual Basic в Visual Studio необходимо внимательно выбрать правильный тип проекта:

1. В Visual Studio 2019 выберите **создать новый проект**.

2. Введите **Visual Basic библиотеку** , чтобы отфильтровать параметры проекта, и выберите параметр **библиотека классов (.NET Standard)** со значком Visual Basic:

    [![Фильтр для библиотеки Visual Basic](xamarin-forms-images/06-sml.png)](xamarin-forms-images/06.png#lightbox)

3. На следующем экране введите имя проекта и нажмите кнопку **создать**.

4. Проект Visual Basic будет выглядеть так, как показано в  **Обозреватель решений** следующим образом:

    [![Пустой проект Visual Basic](images/new-library-sml.png)](images/new-library.png#lightbox)

Теперь проект готов к добавлению кода Visual Basic. На .NET Standard проекты могут ссылаться другие проекты (проекты приложений или проекты библиотек).

## <a name="summary"></a>Сводка

В этой статье показано, как использовать код Visual Basic в приложениях Xamarin с помощью Visual Studio. Несмотря на то, что Xamarin не поддерживает Visual Basic напрямую, компиляция Visual Basic в библиотеку .NET Standard позволяет включать код, написанный с Visual Basic, в приложения Android и iOS.

На следующих страницах описывается использование библиотек .NET Standard Visual Basic.NET в приложениях в машинном или Xamarin. Forms.

- [Создание собственных приложений Xamarin. iOS и Xamarin. Android, использующих VB](native-apps.md)
- [Создание приложений Xamarin. Forms с помощью VB](xamarin-forms.md)

## <a name="related-links"></a>Связанные ссылки

- [Таскивб (пример)](/samples/xamarin/mobile-samples/visualbasic-taskyvb/)
- [Ксамаринформсвб (пример)](/samples/xamarin/mobile-samples/visualbasic-xamarinformsvb/)
- [.NET Standard и Xamarin](~/cross-platform/app-fundamentals/net-standard.md)
- [.NET Standard](/dotnet/standard/net-standard/)