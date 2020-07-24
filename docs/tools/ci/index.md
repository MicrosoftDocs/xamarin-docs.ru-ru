---
title: Непрерывная интеграция с помощью Xamarin
description: В этом документе содержатся ссылки на руководства, в которых описывается непрерывная интеграция с Xamarin. Связанное содержимое содержит общие сведения о непрерывной интеграции и обсуждение сборки центра приложений, TeamCity и Jenkins.
ms.prod: xamarin
ms.assetid: 99484E96-DC69-4697-8BBB-1B44C5CBB5ED
author: davidortinau
ms.author: daortin
ms.date: 10/23/2018
ms.openlocfilehash: 9c87a65481ca58b2861c40a420459d629852f6b6
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997427"
---
# <a name="continuous-integration-with-xamarin"></a>Непрерывная интеграция с помощью Xamarin

> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]

## <a name="introduction-to-continuous-integration"></a>[Введение в непрерывную интеграцию](~/tools/ci/intro-to-ci.md)

В этом разделе рассматриваются различные компоненты, связанные с непрерывной интеграцией и их связями. В нем описываются среды непрерывной интеграции, которые обсуждаются в конкретных разделах ниже.

## <a name="devops-with-xamarin"></a>[DevOps с Xamarin](~/tools/ci/devops.md)

В этом разделе вы можете определить, какие функции DevOps в Azure и Visual Studio хорошо подходят для проекта Xamarin.

## <a name="working-with-continuous-integration-environments"></a>Работа с средами непрерывной интеграции

### <a name="build-xamarin-apps-with-azure-pipelines"></a>[Создание приложений Xamarin с помощью Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/languages/xamarin/)

Используйте Azure Pipelines для автоматической сборки приложений Xamarin для Android и iOS.

### <a name="build-xamarin-apps-using-app-center"></a>[Создание приложений Xamarin с помощью центра приложений](https://docs.microsoft.com/appcenter/build/xamarin/)

Создавайте решения Xamarin. iOS и Xamarin. Android с помощью центра приложений, прямо из GitHub, Azure DevOps или BitBucket.

### <a name="build-xamarin-apps-with-teamcity"></a>[Создание приложений Xamarin с помощью TeamCity](~/tools/ci/teamcity.md)

В этом руководстве рассматриваются шаги, связанные с использованием TeamCity для компиляции мобильных приложений, а затем их отправка в тест в центре приложений.

### <a name="build-xamarin-apps-with-jenkins"></a>[Создание приложений Xamarin с помощью Jenkins](~/tools/ci/jenkins-walkthrough.md)

В этом руководство показано, как настроить Jenkins в качестве сервера непрерывной интеграции и автоматизировать компиляцию мобильных приложений, созданных с помощью Xamarin. Здесь описывается установка Jenkins в OS X, Настройка и Настройка заданий для компиляции приложений Xamarin. iOS и Xamarin. Android при фиксации изменений в системе управления версиями.
