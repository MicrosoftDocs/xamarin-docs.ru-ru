---
title: Возможности платформы Windows
description: В этой статье описывается поддержка платформы Windows, доступная в Xamarin.Forms .
ms.prod: xamarin
ms.assetid: F6EA9E49-FB3E-442F-AF13-B7AD0C80D11F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0cdbd39f0217b0e4d175eb2443250eb3df8dd046
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936179"
---
# <a name="windows-platform-features"></a>Возможности платформы Windows

Xamarin.FormsДля разработки приложений для платформ Windows требуется Visual Studio. На [странице Поддерживаемые платформы](~/get-started/supported-platforms.md) содержатся дополнительные сведения о предварительных требованиях.

![Xamarin.FormsПриложения, работающие в Windows](images/allhanselman.png)

## <a name="platform-specifics"></a>Особенности платформы

Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов.

Для Xamarin.Forms представлений, страниц и макетов на универсальная платформа Windows (UWP) предоставляются следующие специальные функции платформы.

- Задание ключа доступа для [`VisualElement`](xref:Xamarin.Forms.VisualElement) . Дополнительные сведения см. [в разделе ключи доступа висуалелемент в Windows](visualelement-access-keys.md).
- Отключение устаревшего цветового режима в поддерживаемом формате [`VisualElement`](xref:Xamarin.Forms.VisualElement) . Дополнительные сведения см. [в разделе режим Висуалелемент прежних цветов в Windows](legacy-color-mode.md).

Для представлений в UWP предусмотрены следующие функции для конкретных платформ Xamarin.Forms :

- Обнаружение порядка чтения из текстового содержимого в [`Entry`](xref:Xamarin.Forms.Entry) [`Editor`](xref:Xamarin.Forms.Editor) экземплярах, и [`Label`](xref:Xamarin.Forms.Label) . Дополнительные сведения см. [в разделе Инпутвиев Read Order on Windows](inputview-reading-order.md).
- Включение поддержки жестов касания в [`ListView`](xref:Xamarin.Forms.ListView) . Дополнительные сведения см. [в разделе ListView SelectionMode в Windows](listview-selectionmode.md).
- Включение направления извлечения объекта `RefreshView` для изменения. Дополнительные сведения см. [в статье направление извлечения рефрешвиев в Windows](refreshview-pulldirection.md).
- Включение [`SearchBar`](xref:Xamarin.Forms.SearchBar) для взаимодействия с модулем проверки орфографии. Дополнительные сведения см. [в разделе сеарчбар проверка орфографии on Windows](searchbar-spell-check.md).
- Включение [`WebView`](xref:Xamarin.Forms.WebView) для отображение оповещений JavaScript в диалоговом окне сообщения UWP. Дополнительные сведения см. [в статье WebView JavaScript Alerts on Windows](webview-javascript-alert.md).

Для страниц на платформе UWP предоставляются следующие специальные функциональные возможности Xamarin.Forms .

- Свертывание [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) панели навигации. Дополнительные сведения см. [в разделе Мастердетаилпаже навигационная Bar on Windows](masterdetailpage-navigation-bar.md).
- Настройка параметров размещения панели инструментов. Дополнительные сведения см. [в разделе расположение панели инструментов страницы в Windows](page-toolbar-placement.md).
- Включение отображения значков страниц на [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) панели инструментов. Дополнительные сведения см. в статье о [значках TabbedPage в Windows](tabbedpage-icons.md).

Для Xamarin.Forms класса в UWP предусмотрены следующие функции для конкретных платформ [`Application`](xref:Xamarin.Forms.Application) :

- Указание каталога в проекте, из которого будут загружаться ресурсы изображений. Дополнительные сведения см. [в разделе Каталог образов по умолчанию в Windows](default-image-directory.md).

## <a name="platform-support"></a>Поддержка платформы

Xamarin.FormsШаблоны, доступные в Visual Studio, содержат проект универсальная платформа Windows (UWP).

> [!NOTE]
> Xamarin.Forms1. x и 2. x поддерживают _Windows Phone 8 Silverlight_, _Windows Phone 8,1_и _Windows 8.1_ разработки приложений. Однако эти типы проектов являются устаревшими.

## <a name="getting-started"></a>Начало работы

Перейдите в раздел **файл > новый проект >** в Visual Studio и выберите один из нескольких **многоплатформенных > шаблонов пустое приложение ( Xamarin.Forms )** , чтобы начать работу.

В более старых Xamarin.Forms решениях, созданных на macOS, не будут содержаться все перечисленные выше проекты Windows (но их необходимо добавить вручную). Если нужная платформа Windows еще не находится в решении, ознакомьтесь с [инструкциями по установке](installation/index.md) , чтобы добавить нужный тип проекта Windows/с.

## <a name="samples"></a>примеры

[Все примеры](https://github.com/xamarin/xamarin-forms-book-preview-2) для документации по Чарльз Петцольд. [*Создание мобильных приложений с Xamarin.Forms помощью*](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) проектов include универсальная платформа Windows (для Windows 10).

[Демонстрационное приложение "Скотт Hanselman"](https://github.com/jamesmontemagno/Hanselman.Forms) доступно отдельно, а также включает Apple Watch и проекты износа Android (с использованием Xamarin. iOS и Xamarin. Android соответственно, не Xamarin.Forms выполняются на этих платформах).

## <a name="related-links"></a>Связанные ссылки

- [Настройка проектов Windows](~/xamarin-forms/platform/windows/installation/index.md)
