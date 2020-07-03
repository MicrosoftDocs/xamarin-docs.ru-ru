---
title: Привязка библиотек iOS
description: В этом документе описывается, как создавать привязки C# к коду цели-C, что позволяет использовать собственные библиотеки и CocoaPods в приложении Xamarin. iOS.
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 1e0342a41587b7479381ad763790227aba2ef414
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853086"
---
# <a name="binding-ios-libraries"></a>Привязка библиотек iOS

> [!IMPORTANT]
> Сейчас мы изучаем использование пользовательской привязки на платформе Xamarin. Примите участие в [**опросе**](https://www.surveymonkey.com/r/KKBHNLT) , чтобы проинформировать будущие усилия по разработке.

Используйте следующие ссылки, чтобы узнать о привязке целевых библиотек C и CocoaPods для Xamarin. iOS и Xamarin. Mac:

- [**Общие сведения**](~/cross-platform/macios/binding/overview.md) -
   Описывает, как работает привязка.
- [**Цель привязки-библиотеки C**](~/cross-platform/macios/binding/objective-c-libraries.md) -
   Инструкции по привязке библиотек цели-C для использования в проектах Xamarin.
- [**Справочное руководство по**](~/cross-platform/macios/binding/binding-types-reference.md) -
   определению типа Описывает все атрибуты, доступные для привязки авторов, чтобы управлять процессом создания привязки.

## <a name="objective-sharpie"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Цель Шарпие — это средство командной строки, помогающее выполнить начальную загрузку первого прохода привязки.
Он работает путем анализа файлов заголовков собственной библиотеки, чтобы сопоставлять открытый API с [определением привязки](~/cross-platform/macios/binding/objective-c-libraries.md) (процесс, который в противном случае выполняется вручную). Цель Шарпие не создает привязку самостоятельно, но может помочь вам приступить к работе!

Цель Шарпие 3,0 предоставила возможность непосредственной привязки Cocoapods!

## <a name="walkthrough---binding-an-ios-objective-c-library"></a>[Пошаговое руководство. привязка библиотеки цели iOS-C](walkthrough.md)

На этой странице представлено пошаговое руководство по созданию проекта привязки iOS с использованием проекта [**инфколорпиккер**](https://github.com/InfinitApps/InfColorPicker) цели-C с открытым исходным кодом в качестве примера. Библиотека **инфколорпиккер** предоставляет доступный для повторного использования контроллер представлений, позволяющий пользователю выбирать цвет на основе его представления HSB, что делает выбор цветов более удобным для пользователя.
Целевое Шарпие будет использоваться для помощи в процессе привязки.

## <a name="video"></a>Видео

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**Привязки iOS в видео на C/C++**

## <a name="related-links"></a>Связанные ссылки

- [Привязка Objective-C](~/cross-platform/macios/binding/index.md)
- [Привязка Mac](~/mac/platform/binding.md)
