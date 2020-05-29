---
title: Почему Xamarin.Forms . Произошел сбой проекта Android с КОМПИЛЕТОДАЛВИК НЕПРЕДВИДЕНной ошибкой верхнего уровня?
ms.topic: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e29535e71cb77b05da41c043c6fd932ae4f5ce95
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84135854"
---
# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>Почему Xamarin.Forms . Произошел сбой проекта Android с КОМПИЛЕТОДАЛВИК НЕПРЕДВИДЕНной ошибкой верхнего уровня?

Эта ошибка может отображаться на панели ошибок Visual Studio для Mac или в окне вывода сборки Visual Studio. в проектах Android с помощью Xamarin.Forms . Указания.

Чаще всего это устраняется путем увеличения размера кучи Java для проекта Xamarin. Android. Чтобы увеличить размер кучи, выполните следующие действия:

## <a name="visual-studio"></a>Visual Studio

1. Щелкните правой кнопкой мыши проект Android & откройте параметры проекта.
2. Выберите **Параметры Android — > дополнительно** .
3. В текстовом поле Размер кучи Java введите 1 ГБ.
4. Выполните повторную сборку проекта.

![Снимок экрана: параметры проекта Visual Studio](maps-compiletodalvik-error-images/vsjavaheap.png "Параметры сборки Android в Visual Studio")

## <a name="visual-studio-for-mac"></a>Visual Studio для Mac

1. Щелкните правой кнопкой мыши проект Android & откройте параметры проекта.
2. Последовательно выберите **Сборка-> сборка Android — > дополнительно** .
3. В текстовом поле Размер кучи Java введите 1 ГБ.
4. Выполните повторную сборку проекта.  

![Снимок экрана параметров проекта Visual Studio для Mac](maps-compiletodalvik-error-images/xsjavaheap.png "Параметры сборки Android в Visual Studio для Mac")
