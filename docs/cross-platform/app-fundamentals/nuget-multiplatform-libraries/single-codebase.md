---
title: Создание новой многоплатформенной библиотеки для NuGet
description: В этом документе описывается создание многоплатформенной библиотеки для использования с NuGet. Этот метод подходит для бизнес-логики и алгоритмов, которые могут быть полностью выражены в библиотеке базовых классов .NET и, таким образом, выполняться на всех целевых платформах без кода, зависящего от платформы.
ms.prod: xamarin
ms.assetid: E7B55354-9BBE-4122-BCE3-3506B79090DD
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 5e63e6470a7dac0f9148147a0303d35cf33adb1b
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571175"
---
# <a name="creating-a-new-multiplatform-library-for-nuget"></a>Создание новой многоплатформенной библиотеки для NuGet

Создание проекта многоплатформенной библиотеки с использованием PCL или .NET Standard означает, что полученный NuGet можно добавить в любой проект .NET, поддерживающий целевой профиль, включая проекты ASP.NET или классические приложения с помощью WinForms, WPF или UWP.

Библиотека может содержать только код, поддерживаемый выбранным профилем PCL или .NET Standard, а также любые другие добавленные NuGet.
Это подходит для бизнес-логики и алгоритмов, которые могут быть полностью выражены в библиотеке базовых классов .NET.

Отдельная сборка создается и встроена в пакет NuGet.

Если в дальнейшем требуются функциональные возможности платформы, [можно добавлять проекты для конкретных платформ](#add-platforms).

## <a name="steps-to-create-a-multiplatform-library-nuget"></a>Действия по созданию многоплатформенной библиотеки NuGet

1. Выберите **файл > создать решение** (или щелкните правой кнопкой мыши существующее решение и выберите **Добавить > новый проект**).

2. Выберите **многоплатформенную библиотеку** из раздела **многоплатформенная > библиотека** :

   [![](single-codebase-images/mulitplatform-library-sml.png "Configure multi-platform library for a single code base")](single-codebase-images/mulitplatform-library.png#lightbox)

3. Введите **имя** и **Описание**и выберите **Single для всех платформ**:

   [![](single-codebase-images/single-configure-sml.png "Configure multi-platform library for a single code base")](single-codebase-images/single-configure.png#lightbox)

4. Завершите работу мастера. В решении создается один проект библиотеки.

5. Щелкните правой кнопкой мыши новый проект библиотеки и выберите пункт **Параметры**. Раздел **Build > General** позволяет задать **целевую платформу** — выберите переносимый профиль PCL .NET или версию .NET Standard:

   [![](single-codebase-images/single-choose-type-sml.png "Choose PCL or .NET Standard for library type")](single-codebase-images/single-choose-type.png#lightbox)

6. Кроме того, в окне **Параметры проекта** откройте раздел **NuGet Package > метаданных** и введите [необходимые метаданные](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (а также любые необязательные метаданные):

   [![](single-codebase-images/single-metadata-sml.png "Enter required metadata")](single-codebase-images/single-metadata.png#lightbox)

7. Щелкните правой кнопкой мыши проект библиотеки и выберите команду **создать пакет NuGet** (или выполните сборку или развертывание решения), а файл пакета NuGet **. nupkg** будет сохранен в папке **/bin/** (Отладка или выпуск, в зависимости от конфигурации):

   ![](single-codebase-images/create-nuget-package.png "The NuGet package file will be saved in the bin folder either Debug or Release, depending on configuration")

## <a name="verifying-the-output"></a>Проверка выходных данных

Пакеты NuGet также являются ZIP-файлами, поэтому можно проверить внутреннюю структуру созданного пакета.

На этом снимке экрана показано содержимое NuGet на основе PCL — включена только одна сборка PCL:

![](single-codebase-images/nuget-output.png "Files contained in the NuGet package")

<a name="add-platforms"></a>

## <a name="adding-platform-specific-code"></a>Добавление кода для конкретной платформы

Проекты на основе PCL и проекты на основе .NET Standard не могут содержать ссылки для конкретных платформ (например, функции iOS или Android).

Если существующий проект PCL или проект .NET Standard необходимо расширить для включения кода, зависящего от платформы, это можно сделать, щелкнув правой кнопкой мыши проект и выбрав **добавить > добавить реализацию платформы...**:

[![](single-codebase-images/add-later-sml.png "Add platform implementation menu")](single-codebase-images/add-later.png#lightbox)

В решение можно добавить один или несколько проектов платформы, а при необходимости можно преобразовать существующую библиотеку PCL или .NET Standard в общий проект:

[![](single-codebase-images/add-later-platforms-sml.png "Add platform options such as iOS, Android, and Shared Project")](single-codebase-images/add-later-platforms-sml.png#lightbox)

После преобразования в общий проект перейдите в раздел **Параметры проекта > пакет NuGet > справочные сборки** 
 [section](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/platform-specific.md) и убедитесь, что выбраны все необходимые профили (чтобы NuGet продолжал быть совместимыми с проектами, которые ранее использовались в).

## <a name="related-links"></a>Связанные ссылки

- [Руководство по метаданным](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
