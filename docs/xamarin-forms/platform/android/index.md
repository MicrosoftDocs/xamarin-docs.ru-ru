---
title: Возможности на платформе Android
description: В этой статье объясняется, как добавить в приложения функциональные возможности, относящиеся к Android Xamarin.Forms .
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3045db1248aa16529d4e43b9a8afc97377cfd9cb
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84128973"
---
# <a name="android-platform-features"></a>Возможности на платформе Android

Xamarin.FormsДля разработки приложений для Android требуется Visual Studio. На [странице Поддерживаемые платформы](~/get-started/supported-platforms.md) содержатся дополнительные сведения о предварительных требованиях.

## <a name="platform-specifics"></a>Особенности платформы

Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов.

Для Xamarin.Forms представлений, страниц и макетов в Android предусмотрены следующие функции для конкретных платформ:

- Управление Z-порядком визуальных элементов для определения порядка рисования. Дополнительные сведения см. [в статье повышение прав висуалелемент на Android](visualelement-elevation.md).
- Отключение устаревшего цветового режима в поддерживаемом формате [`VisualElement`](xref:Xamarin.Forms.VisualElement) . Дополнительные сведения см. [в разделе режим Висуалелемент прежних цветов в Android](legacy-color-mode.md).

Для представлений на Android доступны следующие функции для конкретных платформ Xamarin.Forms :

- Использование заполнения по умолчанию и значений тени для кнопок Android. Дополнительные сведения см. [в разделе Заполнение и тени кнопок на устройствах Android](button-padding-shadow.md).
- Настройка параметров редактора метода ввода для экранной клавиатуры для [`Entry`](xref:Xamarin.Forms.Entry) . Дополнительные сведения см. [в разделе Параметры редактора метода ввода для Android](entry-ime-options.md).
- Включение тени на `ImageButton` . Дополнительные сведения см. [в статье Drop Shadows on Android](imagebutton-drop-shadow.md).
- Включение быстрой прокрутки в [`ListView`](xref:Xamarin.Forms.ListView) . Дополнительные сведения см. в статье [Быстрая прокрутка ListView в Android](listview-fast-scrolling.md).
- Управление переходом, используемым при открытии `SwipeView` . Дополнительные сведения см. в разделе [Свипевиев считывание режима перехода](swipeview-swipetransitionmode.md).
- Управление [`WebView`](xref:Xamarin.Forms.WebView) возможностью отображать смешанное содержимое. Дополнительные сведения см. [в статье WebView Mixed Content in Android](webview-mixed-content.md).
- Включение масштабирования в [`WebView`](xref:Xamarin.Forms.WebView) . Дополнительные сведения см. [в разделе WebView Zoom on Android](webview-zoom-controls.md).

Для ячеек на платформе Android предоставляются следующие функции, зависящие Xamarin.Forms от платформы:

- Включение [`ViewCell`](xref:Xamarin.Forms.ViewCell) контекстных действий в устаревшем режиме, чтобы меню контекстных действий не обновлялось при изменении выбранного элемента [`ListView`](xref:Xamarin.Forms.ListView) . Дополнительные сведения см. [в статье ViewCell context Actions on Android](viewcell-context-actions.md).

Для страниц на платформе Android предоставляются следующие функции, зависящие от платформы Xamarin.Forms :

- Задание высоты панели навигации на [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) . Дополнительные сведения см. [в разделе высота Навигатионпаже Bar на устройстве Android](navigationpage-bar-height.md).
- Отключение анимации переходов при переходе по страницам в [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . Дополнительные сведения см. [в разделе Анимация перехода на страницу таббедпаже в Android](tabbedpage-transition-animations.md).
- Включение прокрутки между страницами в [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . Дополнительные сведения см. [в разделе Таббедпаже Page прокрутка на Android](tabbedpage-page-swiping.md).
- Задание расположения и цвета панели инструментов в [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . Дополнительные сведения см. [в статье размещение и цвет панели инструментов таббедпаже в Android](tabbedpage-toolbar-placement-color.md).

Для Xamarin.Forms класса на Android предоставлены следующие специальные функции платформы [`Application`](xref:Xamarin.Forms.Application) :

- Установка режима работы экранной клавиатуры. Дополнительные сведения см. [в разделе режим ввода с клавиатуры](soft-keyboard-input-mode.md).
- Отключение [`Disappearing`](xref:Xamarin.Forms.Page.Appearing) событий и [`Appearing`](xref:Xamarin.Forms.Page.Appearing) жизненного цикла страницы при приостановке и возобновлении соответственно для приложений, использующих AppCompat. Дополнительные сведения см. [в разделе события жизненного цикла страницы в Android](page-lifecycle-events.md).

## <a name="platform-support"></a>Поддержка платформ

Изначально проект Android по умолчанию Xamarin.Forms использовал устаревший стиль визуализации элементов управления, который был распространен до Android 5,0. Приложения, созданные с помощью шаблона, имеют `FormsApplicationActivity` базовый класс для своего основного действия.

## <a name="material-design-via-appcompat"></a>Проектирование материалов с помощью AppCompat

Xamarin.FormsПроекты Android теперь используют в `FormsAppCompatActivity` качестве базового класса их основного действия. Этот класс использует функции **AppCompat** , предоставляемые Android, для реализации тем дизайна материалов.

Чтобы добавить темы дизайна материалов в Xamarin.Forms проект Android, следуйте инструкциям по [установке для поддержки AppCompat](appcompat-material-design.md) .

Ниже приведен пример **TODO** со значением по умолчанию `FormsApplicationActivity` .

[![](images/before-appcompat-sml.png "Todo Sample Application Without AppCompat")](images/before-appcompat.png#lightbox "Todo Sample Application Without AppCompat")

И это тот же код после обновления проекта для использования `FormsAppCompatActivity` (и добавления дополнительных сведений о теме):

[![](images/post-appcompat-sml.png "Todo Sample Application With AppCompat and Theming")](images/post-appcompat.png#lightbox "Todo Sample Application With AppCompat and Theming")

> [!NOTE]
> При использовании `FormsAppCompatActivity` [базовые классы для некоторых настраиваемых модулей подготовки Android](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md) будут отличаться.

## <a name="androidx-migration"></a>Миграция AndroidX

Андроидкс заменяет библиотеку поддержки Android. Дополнительные сведения о Андроидкс и о том, как перенести Xamarin.Forms приложение для использования библиотек андроидкс, см. [в Xamarin.Forms разделе Миграция андроидкс в ](~/xamarin-forms/platform/android/androidx-migration.md).

## <a name="related-links"></a>Связанные ссылки

- [Добавление поддержки дизайна материалов](appcompat-material-design.md)
