---
title: Создание NuGet из существующих проектов библиотек
description: В этом документе описывается, как создать пакет NuGet на основе существующего проекта библиотеки, что позволяет совместно использовать код с другими разработчиками.
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
author: davidortinau
ms.author: daortin
ms.date: 04/20/2017
ms.openlocfilehash: 058904236e100a670e8a38417dcaec805178d6d2
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936812"
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>Создание NuGet из существующих проектов библиотек

Существующие библиотеки PCL или .NET Standard можно включить в NuGet через окно " **Параметры проекта** ":

1. Щелкните правой кнопкой мыши проект библиотеки в **панель решения** и выберите пункт **Параметры**.

2. Перейдите в раздел **метаданных пакета NuGet >** и введите все [необходимые сведения](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) на вкладке **Общие** .

   [![Введите необходимые метаданные](existing-library-images/existing-metadata-sml.png)](existing-library-images/existing-metadata.png#lightbox)

3. При необходимости [добавьте дополнительные метаданные](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) на вкладке **сведения** .

4. После настройки метаданных можно щелкнуть правой кнопкой мыши проект и выбрать пункт **создать пакет NuGet** , а файл пакета NuGet **. nupkg** будет сохранен в папке **/bin/** (Отладка или выпуск, в зависимости от конфигурации).

   ![Выберите пункт Создать пакет NuGet в контекстном меню.](existing-library-images/create-nuget-package.png)

5. Чтобы создать пакет NuGet при _каждой_ сборке или развертывании, перейдите в раздел **NuGet Package > Build** и Tick ( **создать пакет NuGet) при сборке проекта**:

    [![Такт для создания пакета NuGet](existing-library-images/existing-tickbox-sml.png)](existing-library-images/existing-tickbox.png#lightbox)

> [!NOTE]
> Создание пакета NuGet может замедлить процесс сборки. Если это поле не является тактовым, вы по-прежнему можете создать пакет NuGet вручную в любое время из контекстного меню проекта (см. шаг 4 выше).

## <a name="verifying-the-output"></a>Проверка выходных данных

Пакеты NuGet также являются ZIP-файлами, поэтому можно проверить внутреннюю структуру созданного пакета.

На этом снимке экрана показано содержимое NuGet на основе PCL — включена только одна сборка PCL:

![Файлы, содержащиеся в пакете NuGet](existing-library-images/nuget-output.png)

## <a name="related-links"></a>Связанные ссылки

- [Руководство по метаданным](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
