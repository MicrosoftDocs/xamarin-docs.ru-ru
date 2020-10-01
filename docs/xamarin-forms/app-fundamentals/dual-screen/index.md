---
title: Два экрана в Xamarin.Forms
description: Это руководство рассказывает, как создавать приложения с поддержкой двухэкранных устройств с помощью Xamarin.Forms.
ms.prod: xamarin
ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e1d2a443a6005050c518e21e4e0f2df64c2aab0c
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562630"
---
# <a name="no-locxamarinforms-dual-screen"></a>Два экрана в Xamarin.Forms

Устройства с двумя экранами, такие как Microsoft Surface Duo, упрощают реализацию новых возможностей работы для пользователей в приложениях. Xamarin.Forms включает в себя классы `TwoPaneView` и `DualScreenInfo`, позволяющие разрабатывать приложения для устройств с двумя экранами.

## <a name="get-started"></a>Начало работы

Чтобы добавить возможности работы с двумя экранами в приложение Xamarin.Forms, выполните указанные ниже действия.

1. Откройте диалоговое окно **Диспетчер пакетов NuGet** для вашего решения.
2. На вкладке **Обзор** выполните поиск по `Xamarin.Forms.DualScreen`.
3. Установите пакет `Xamarin.Forms.DualScreen` в решении.
4. Добавьте следующий вызов метода инициализации в класс `MainActivity` проекта Android в событии `OnCreate`:

    ```csharp
    Xamarin.Forms.DualScreen.DualScreenService.Init(this);
    ```

    Этот метод необходим для того, чтобы приложение могло обнаруживать изменения в состоянии приложения, например, разбиение на два экрана.

5. Измените атрибут `Activity` в классе `MainActivity` проекта Android так, чтобы он включал _все_ следующие параметры `ConfigurationChanges`:

    ```@csharp
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation
        | ConfigChanges.ScreenLayout | ConfigChanges.SmallestScreenSize | ConfigChanges.UiMode
    ```

    Эти значения необходимы для более надежного получения сообщений об изменениях конфигурации и состояния разбиения. По умолчанию в проекты Xamarin.Forms добавляются только два из них, поэтому обязательно добавьте остальные для надежной поддержки двух экранов.

## <a name="troubleshooting"></a>Устранение неполадок

Если класс `DualScreenInfo` или макет `TwoPaneView` работают не так, как нужно, тщательно проверьте инструкции по настройке на этой странице. Распространенными причинами ошибок являются отсутствие или неправильная настройка метода `Init` или значений атрибутов `ConfigurationChanges`.

Дополнительные рекомендации и эталонную реализацию см. в [образцах Xamarin.Forms с поддержкой двух экранов](/dual-screen/xamarin/samples).

## <a name="next-steps"></a>Следующие шаги

После добавления NuGet добавьте в приложение функции работы с двумя экранами, следуя приведенным ниже рекомендациям.

- [Шаблоны проектирования для двух экранов](design-patterns.md). Наше руководство по шаблонам поможет вам подобрать оптимальный вариант использования интерфейса приложения на устройстве с двумя экранами.
- [Макет TwoPaneView](twopaneview.md). Класс `TwoPaneView` в Xamarin.Forms, созданный в стиле элемента управления UWP с таким же именем, представляет собой кроссплатформенный макет, оптимизированный для двухэкранных устройств.
- [Вспомогательный класс DualScreenInfo](dual-screen-info.md). Класс `DualScreenInfo` позволяет определить, в какой области отображается ваше представление, сколько места оно занимает, в каком положении находится устройство, каков угол сгиба и т. д.
- [Триггеры для двух экранов](triggers.md). Пространство имен [`Xamarin.Forms.DualScreen`](xref:Xamarin.Forms.DualScreen) включает два триггера состояния, которые активируют изменение [`VisualState`](xref:Xamarin.Forms.VisualState) при изменении режима просмотра в присоединенном макете или окне.

Дополнительные сведения см. в [документации по поддержке двух экранов для разработчиков](/dual-screen/).