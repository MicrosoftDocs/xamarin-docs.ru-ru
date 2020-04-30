---
title: функции платформы iOS в Xamarin. Forms
description: Добавление специальных функций для iOS в приложения Xamarin. Forms.
ms.prod: xamarin
ms.assetid: 634AB62E-68C8-454C-838B-F1CC4E4E21BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2020
ms.openlocfilehash: 97b9b70a1df61beb7b064c99721b4277e2570b41
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517059"
---
# <a name="ios-platform-features-in-xamarinforms"></a>функции платформы iOS в Xamarin. Forms

Для разработки приложений Xamarin. Forms для iOS требуется Visual Studio. На [странице Поддерживаемые платформы](~/get-started/supported-platforms.md) содержатся дополнительные сведения о предварительных требованиях.

## <a name="platform-specifics"></a>Особенности платформы

Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов.

Для представлений, страниц и макетов Xamarin. Forms в iOS доступны следующие функции для конкретной платформы:

- Поддержка размытия для [`VisualElement`](xref:Xamarin.Forms.VisualElement)любого. Дополнительные сведения см. [в разделе Висуалелемент размытие в iOS](visualelement-blur.md).
- Отключение устаревшего цветового режима в [`VisualElement`](xref:Xamarin.Forms.VisualElement)поддерживаемом формате. Дополнительные сведения см. [в разделе режим Висуалелемент прежних цветов в iOS](legacy-color-mode.md).
- Включение тени на [`VisualElement`](xref:Xamarin.Forms.VisualElement). Дополнительные сведения см. [в статье Висуалелемент Drop Shadows on IOS](visualelement-drop-shadow.md).
- Включение [`VisualElement`](xref:Xamarin.Forms.VisualElement) объекта в качестве первого респондента для событий касания. Дополнительные сведения см. в статье [Висуалелемент First ответчика](visualelement-first-responder.md).

Для представлений Xamarin. Forms в iOS предусмотрены следующие функции, зависящие от платформы.

- Настройка цвета [`Cell`](xref:Xamarin.Forms.Cell) фона. Дополнительные сведения см. [в разделе цвет фона ячейки в iOS](cell-background-color.md).
- Управление выбором элементов в [`DatePicker`](xref:Xamarin.Forms.DatePicker). Дополнительные сведения см. [в разделе Выбор элементов DatePicker в iOS](datepicker-selection.md).
- Проверка того, что выводимый текст умещается в [`Entry`](xref:Xamarin.Forms.Entry) , путем настройки размера шрифта. Дополнительные сведения см. [в разделе Размер шрифта записи в iOS](entry-font-size.md).
- Установка цвета курсора в [`Entry`](xref:Xamarin.Forms.Entry). Дополнительные сведения см. [в разделе Ввод цвета курсора в iOS](entry-cursor-color.md).
- Управление тем [`ListView`](xref:Xamarin.Forms.ListView) , будут ли ячейки заголовка перемещаться во время прокрутки. Дополнительные сведения см. [в разделе стиль заголовка группы ListView в iOS](listview-group-header-style.md).
- Управление тем, будет ли отключена анимация [`ListView`](xref:Xamarin.Forms.ListView) строк при обновлении коллекции Items. Дополнительные сведения см. [в статье анимация строк ListView в iOS](listview-row-animations.md).
- Установка стиля разделителя для [`ListView`](xref:Xamarin.Forms.ListView). Дополнительные сведения см. [в разделе стиль разделителя ListView в iOS](listview-separator-style.md).
- Управление выбором элементов в [`Picker`](xref:Xamarin.Forms.Picker). Дополнительные сведения см. [в разделе Выбор элементов средства выбора в iOS](picker-selection.md).
- Управление наличием [`SearchBar`](xref:Xamarin.Forms.SearchBar) фона. Дополнительные сведения см. [в статье сеарчбар Style on IOS](searchbar-style.md).
- Включение [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) свойства для установки путем касания на положении на [`Slider`](xref:Xamarin.Forms.Slider) панели, а не перетаскивания `Slider` бегунка. Дополнительные сведения см. [в разделе ползунок бегунка в iOS](slider-thumb.md).
- Управление переходом, используемым при открытии `SwipeView`. Дополнительные сведения см. в разделе [Свипевиев считывание режима перехода](swipeview-swipetransitionmode.md).
- Управление выбором элементов в [`TimePicker`](xref:Xamarin.Forms.TimePicker). Дополнительные сведения см. [в статье Выбор элемента TimePicker в iOS](timepicker-selection.md).

Для страниц Xamarin. Forms в iOS предусмотрены следующие функции, зависящие от платформы.

- Управление тем, применяется ли к ней [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) теневая страница, при раскрытии главной страницы. Дополнительные сведения см. в разделе [Мастердетаилпаже Shadow](masterdetailpage-shadow.md).
- Скрытие разделителя панели навигации на [`NavigationPage`](xref:Xamarin.Forms.NavigationPage). Дополнительные сведения см. [в разделе разделитель Навигатионпаже Bar в iOS](navigation-bar-separator.md).
- Управление прозрачностью панели навигации. Дополнительные сведения см. [в разделе Панель навигации полупрозрачность в iOS](navigation-bar-translucent.md).
- Управление тем, изменяется ли цвет [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) текста строки состояния в, в соответствии с яркостью панели навигации. Дополнительные сведения см. [в статье режим цвета текста Навигатионпаже Bar в iOS](status-bar-text-color.md).
- Управление отображением заголовка страницы в виде большого заголовка на панели навигации по странице. Дополнительные сведения см. [в статье заголовки больших страниц в iOS](page-large-title.md).
- Установка видимости индикатора Home на [`Page`](xref:Xamarin.Forms.Page). Дополнительные сведения см. [в статье видимость индикатора в iOS](page-home-indicator.md).
- Установка видимости в строке состояния для [`Page`](xref:Xamarin.Forms.Page). Дополнительные сведения см. [в разделе Видимость строки состояния страницы в iOS](page-status-bar-visibility.md).
- Обеспечение расположения содержимого страницы в области экрана, которая является надежной для всех устройств iOS. Дополнительные сведения см. [в разделе Путеводитель по макету защищенной области в iOS](page-safe-area-layout.md).
- Установка стиля представления для модальных страниц. Дополнительные сведения см. в разделе [стиль представления модальной страницы](page-presentation-style.md).
- Установка режима полупрозрачность для панели вкладок на [`TabbedPage`](xref:Xamarin.Forms.TabbedPage). Дополнительные сведения см. [в статье Таббедпаже таббар in iOS](tabbedpage-translucent-tabbar.md).

Для макетов Xamarin. Forms в iOS предусмотрены следующие функции, зависящие от платформы.

- Управление тем, [`ScrollView`](xref:Xamarin.Forms.ScrollView) обрабатывает ли объект сенсорный жест или передает его содержимому. Дополнительные сведения см. [в разделе Скроллвиев Content соприкасается on IOS](scrollview-content-touches.md).

Для класса Xamarin. Forms [`Application`](xref:Xamarin.Forms.Application) в iOS предусмотрены следующие функции для конкретной платформы:

- Отключение масштабирования специальных возможностей для именованных размеров шрифтов. Дополнительные сведения см. [в разделе масштабирование специальных возможностей для именованных размеров шрифтов в iOS](named-font-size-scaling.md).
- Включение макета элемента управления и обновлений отрисовки для выполнения в основном потоке. Дополнительные сведения см. [в разделе Основные обновления управления потоком в iOS](main-thread-updates-ui.md).
- Включение [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) в представлении прокрутки для захвата и совместного использования жеста панорамирования с прокруткой. Дополнительные сведения см. [в разделе одновременный распознавание жестов в iOS](application-pan-gesture.md).

## <a name="ios-specific-formatting"></a>форматирование, относящееся к iOS

Xamarin. Forms позволяет устанавливать стили и цвета пользовательского интерфейса на разных платформах, но существуют и другие варианты настройки темы для iOS с помощью API платформы в проекте iOS.

[Узнайте больше](formatting.md) о форматировании пользовательского интерфейса с помощью интерфейсов API для iOS, таких как конфигурация **info. plist** и `UIAppearance` API.

![](images/status-white-sml.png "iOS Theming")

## <a name="other-ios-features"></a>Другие функции iOS

Используя [пользовательские модули подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)отчетов, [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)и [мессагингцентер](~/xamarin-forms/app-fundamentals/messaging-center.md), можно внедрить разнообразные собственные функции в приложения Xamarin. Forms для iOS.
