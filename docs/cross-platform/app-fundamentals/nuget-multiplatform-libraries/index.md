---
title: Проекты многоплатформенных библиотек NuGet (Нужетизер 3000)
description: В этом документе описывается использование средства Нужетизер 3000 для автоматического создания пакетов NuGet для совместного использования кода на разных платформах.
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
author: davidortinau
ms.author: daortin
ms.date: 07/25/2018
ms.openlocfilehash: f0d1195d9159623ec865b1ea1fec26d9969925ca
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91456604"
---
# <a name="nuget-multiplatform-library-projects-nugetizer-3000"></a>Проекты многоплатформенных библиотек NuGet (Нужетизер 3000)

_Автоматическое создание пакетов NuGet для совместного использования кода на разных платформах с помощью "Нужетизер 3000"!_

Можно автоматически создать пакеты NuGet для совместного использования кода на разных платформах с помощью _нужетизер 3000_. Это позволяет создать пакеты NuGet на основе существующих проектов библиотеки или создать новый **проект многоплатформенной библиотеки**.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Нужетизер 3000 входит в состав Visual Studio для Mac &ndash; найдите тип проекта **Библиотека > библиотеки Мулитплатформ** в **файле > новом** окне:

[![Создать новое окно многоплатформенной библиотеки](images/mulitplatform-library-sml.png)](images/mulitplatform-library.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Чтобы использовать Нужетизер 3000 в Visual Studio, [скачайте и запустите УСТАНОВЩИК VSIX](https://bit.ly/nugetizer-2017).

-----

## <a name="building-nuget-packages"></a>Создание пакетов NuGet

После настройки каждая сборка проекта выводит полный пакет NuGet, который можно использовать для внутреннего совместного использования кода с другими приложениями или передачи в [NuGet.org](https://www.nuget.org).

Существует три сценария использования этой функции:

- [Имеющиеся проекты библиотеки](existing-library.md)

  Создание пакета NuGet на основе существующих проектов PCL (или .NET Standard).

- [Создание проекта многоплатформенной библиотеки](single-codebase.md)

  Создайте новую библиотеку для совместного использования общего кода с помощью NuGet, используя PCL или .NET Standard.

- [Создание новых проектов библиотек для конкретной платформы](platform-specific.md)

  Создайте новую библиотеку и NuGet, которая включает код, зависящий от платформы, для iOS и Android и использует общий проект для хранения общего кода и проектов для конкретной платформы для поддержки функций iOS или Android.

Дополнительные сведения о необходимых и необязательных метаданных, которые необходимо добавить в любой пакет NuGet, см. в [руководстве по метаданным](metadata.md) .

## <a name="further-nuget-information"></a>Дополнительные сведения о NuGet

Узнайте больше о [создании NuGet для Xamarin вручную](~/cross-platform/app-fundamentals/nuget-manual.md) и о том, как [включить пакет NuGet в приложение](/visualstudio/mac/nuget-walkthrough).

Документация по Microsoft [NuGet](/nuget/) содержит более подробные сведения о формате **nupkg** и использовании пакетов NuGet в Visual Studio.

Обсуждение разработки для проектов пакетов NuGet ( Нужетизер 3000) доступен в [репозитории GitHub для NuGet](https://github.com/NuGet/Home/wiki/NuGetizer-3000).

## <a name="related-links"></a>Связанные ссылки

- [Примеры использования Нужетизер-3000](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [Создание пакетов NuGet для Xamarin вручную](~/cross-platform/app-fundamentals/nuget-manual.md)
- [Документация по NuGet](/nuget/)