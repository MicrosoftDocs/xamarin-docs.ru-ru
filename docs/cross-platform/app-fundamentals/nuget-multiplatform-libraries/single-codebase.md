---
title: Создание новой многоплатформенной библиотеки для NuGet
description: В этом документе описывается создание многоплатформенной библиотеки для использования с NuGet. Этот метод подходит для бизнес-логики и алгоритмов, которые могут быть полностью выражены в библиотеке базовых классов .NET и, таким образом, выполняться на всех целевых платформах без кода, зависящего от платформы.
ms.prod: xamarin
ms.assetid: E7B55354-9BBE-4122-BCE3-3506B79090DD
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: ca874149874c420801c839fcde7afac11f46c5e1
ms.sourcegitcommit: e27e29c14b783263e063baaa65d4eecb8dd31f57
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98628921"
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

   [![Снимок экрана: выберите шаблон с выбранной многоплатформенной библиотекой.](single-codebase-images/mulitplatform-library-sml.png)](single-codebase-images/mulitplatform-library.png#lightbox)

3. Введите **имя** и **Описание** и выберите **Single для всех платформ**:

   [![Снимок экрана показывает значения, указанные для имени, описания и реализации.](single-codebase-images/single-configure-sml.png)](single-codebase-images/single-configure.png#lightbox)

4. Завершите работу мастера. В решении создается один проект библиотеки.

5. Щелкните правой кнопкой мыши новый проект библиотеки и выберите пункт **Параметры**. Раздел **Build > General** позволяет задать **целевую платформу** — выберите переносимый профиль PCL .NET или версию .NET Standard:

   [![Выберите PCL или .NET Standard для типа библиотеки](single-codebase-images/single-choose-type-sml.png)](single-codebase-images/single-choose-type.png#lightbox)

6. Кроме того, в окне **Параметры проекта** откройте раздел **NuGet Package > метаданных** и введите [необходимые метаданные](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (а также любые необязательные метаданные):

   [![Введите необходимые метаданные](single-codebase-images/single-metadata-sml.png)](single-codebase-images/single-metadata.png#lightbox)

7. Щелкните правой кнопкой мыши проект библиотеки и выберите команду **создать пакет NuGet** (или выполните сборку или развертывание решения), а файл пакета NuGet **. nupkg** будет сохранен в папке **/bin/** (Отладка или выпуск, в зависимости от конфигурации):

   ![Файл пакета NuGet будет сохранен в папке bin как отладка, так и выпуск, в зависимости от конфигурации.](single-codebase-images/create-nuget-package.png)

## <a name="verifying-the-output"></a>Проверка выходных данных

Пакеты NuGet также являются ZIP-файлами, поэтому можно проверить внутреннюю структуру созданного пакета.

На этом снимке экрана показано содержимое NuGet на основе PCL — включена только одна сборка PCL:

![Файлы, содержащиеся в пакете NuGet](single-codebase-images/nuget-output.png)

<a name="add-platforms"></a>

## <a name="adding-platform-specific-code"></a>Добавление кода Platform-Specific

Проекты на основе PCL и проекты на основе .NET Standard не могут содержать ссылки для конкретных платформ (например, функции iOS или Android).

Если существующий проект PCL или проект .NET Standard необходимо расширить для включения кода, зависящего от платформы, это можно сделать, щелкнув правой кнопкой мыши проект и выбрав **добавить > добавить реализацию платформы...**:

[![Меню "добавить реализацию платформы"](single-codebase-images/add-later-sml.png)](single-codebase-images/add-later.png#lightbox)

В решение можно добавить один или несколько проектов платформы, а при необходимости можно преобразовать существующую библиотеку PCL или .NET Standard в общий проект:

[![Добавление параметров платформы, таких как iOS, Android и Shared Project](single-codebase-images/add-later-platforms-sml.png)](single-codebase-images/add-later-platforms-sml.png#lightbox)

После преобразования в общий проект перейдите в раздел **Параметры проекта > пакет NuGet > справочные сборки** 
 [](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/platform-specific.md) и убедитесь, что выбраны все необходимые профили (чтобы NuGet продолжал быть совместимыми с проектами, которые ранее использовались в).

## <a name="related-links"></a>Связанные ссылки

- [Путеводитель по метаданным](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
