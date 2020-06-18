---
title: 'title: "Xamarin.Forms Два экрана" description: "Это руководство рассказывает, как создавать приложения Xamarin.Forms с поддержкой двухэкранных устройств".'
description: 'ms.prod: xamarin ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e ms.technology: xamarin-forms author: davidortinau ms.author: daortin ms.date: 02/08/2020 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: aeaaeb732adaea45446d6baf833027801abf4d2a
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/10/2020
ms.locfileid: "84138909"
---
# <a name="xamarinforms-dual-screen"></a>Два экрана в Xamarin.Forms

![](~/media/shared/preview.png "This API is currently pre-release")

Теперь на устройствах Surface Duo (Android) и Surface Neo (Windows 10X) доступны новые шаблоны разработки для приложений с сенсорным управлением. Xamarin.Forms включает классы `TwoPaneView` и `DualScreenInfo`, позволяющие разрабатывать приложения для этих устройств.

## <a name="dual-screen-design-patterns"></a>[Конструктивные шаблоны для двухэкранных устройств](design-patterns.md)

Наше руководство по шаблонам поможет вам подобрать оптимальный вариант использования интерфейса приложения на устройстве с двумя экранами.

## <a name="dual-screen-layout"></a>[Макет для двух экранов](twopaneview.md)

Класс `TwoPaneView` в Xamarin.Forms, созданный в стиле элемента управления UWP с таким же именем, представляет собой кроссплатформенный макет, оптимизированный для двухэкранных устройств.

## <a name="dual-screen-device-capabilities"></a>[Возможности устройства с двумя экранами](dual-screen-info.md)

Класс `DualScreenInfo` позволяет определить, в какой области отображается ваше представление, сколько места оно занимает, в каком положении находится устройство, каков угол сгиба и т. д.

## <a name="dual-screen-platform-helpers"></a>[Вспомогательные функции платформы для двух экранов](dual-screen-helper.md)

Класс `DualScreenHelper` позволяет проверить, поддерживает ли платформа открытие нового окна в режиме "картинка в картинке". Если вы используете устройство Neo в режиме создания, окно откроется на панели WonderBar.

## <a name="dual-screen-triggers"></a>[Триггеры для двух экранов](triggers.md)

Пространство имен [`Xamarin.Forms.DualScreen`](xref:Xamarin.Forms.DualScreen) включает два триггера состояния, которые активируют изменение [`VisualState`](xref:Xamarin.Forms.VisualState) при изменении режима просмотра в присоединенном макете или окне.
