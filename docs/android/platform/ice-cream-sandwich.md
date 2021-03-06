---
title: Возможности Ice Cream Sandwich
description: В этой статье описан ряд новых возможностей, предоставляемых разработчикам приложений в API Android 4 — Ice Cream Sandwich. В статье рассматривается несколько новых технологий пользовательского интерфейса, а также разнообразные новые возможности, предоставляемые Android 4 для совместной работы с данными в разных приложениях и на разных устройствах.
ms.prod: xamarin
ms.assetid: 78E18A62-C12F-A699-37FA-44B9F6B44273
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 1e72a09acc08fc4db49da0e94eb64fbd523e9ecf
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91453693"
---
# <a name="ice-cream-sandwich-features"></a>Возможности Ice Cream Sandwich

_В этой статье описан ряд новых возможностей, предоставляемых разработчикам приложений в API Android 4 — Ice Cream Sandwich. В статье рассматривается несколько новых технологий пользовательского интерфейса, а также разнообразные новые возможности, предоставляемые Android 4 для совместной работы с данными в разных приложениях и на разных устройствах._

## <a name="overview"></a>Обзор

ОС Android версии 4.0 (API уровня 14) — это существенно доработанная операционная система Android, которая включает ряд важных изменений и обновлений, в том числе следующие.

- **Обновленный пользовательский интерфейс** — ряд новых возможностей пользовательского интерфейса предоставляет разработчикам больше свободы и пространства для действий при создании пользовательских интерфейсов приложений. Среди них: `GridLayout`, `PopupMenu`, мини-приложение `Switch` и `TextureView`.
- **Улучшенное аппаратное ускорение** — двухмерная отрисовка всех элементов управления Android теперь выполняется на GPU. Кроме того, по умолчанию во всех приложениях, разработанных для Android 4.0, включено аппаратное ускорение.
- **Новые интерфейсы API данных** — появился новый доступ к данным, которые ранее не были официально доступны, например к данным календаря и профилю пользователя владельца устройства.
- **Совместное использование данных в приложениях** — теперь можно с легкостью обеспечить совместное использование данных в разных приложениях и на различных устройствах с помощью таких технологий, как `ShareActionProvider`, упрощающей создание действия совместного доступа на панели действий, а также *Android Beam* для *связи малого радиуса действия (NFC)* , которая позволяет без труда обмениваться данными между устройствами, находящимися в зоне досягаемости друг друга.

В данной статье рассматриваются эти возможности и другие изменения, внесенные в API-интерфейс Android 4.0, а также объясняется, как использовать каждую функцию с Xamarin.Android.

## <a name="user-interface-features"></a>Возможности пользовательского интерфейса

В Android 4 доступно множество новых технологий пользовательского интерфейса, в том числе следующие.

- **[GridLayout](~/android/user-interface/layouts/grid-layout.md)**  — поддерживает макет плоской сетки элементов управления.
- **[Мини-приложение Switch](~/android/user-interface/controls/switch.md)** — позволяет переключаться между режимами "ВКЛ." и "ВЫКЛ.".
- **[TextureView](~/android/user-interface/controls/texture-view.md)**  — обеспечивает отображение видео и содержимого OpenGL в представлении.
- **[Панель навигации](~/android/user-interface/controls/navigation-bar.md)** — содержит виртуальные кнопки для возврата назад, перехода на домашнюю страницу и переключения между задачами.

Кроме того, улучшены другие элементы пользовательского интерфейса, например меню `<a href"/guides/android/user_interface/popup_menus">PopupMenu</a>`, работать с которым стало еще проще, и вкладки, которые теперь выглядят безупречно.

## <a name="sharing-features"></a>Возможности общего доступа

В Android 4 есть несколько новых технологий, которые позволяют обмениваться данными между устройствами и приложениями. Он также предоставляет доступ к различным типам ранее недоступных данных, например сведениям в календаре и профилю пользователя владельца устройства. В этом разделе мы рассмотрим множество таких функций Android 4, в том числе следующие.

- **[Android Beam](~/android/platform/android-beam.md)**  — обеспечивает обмен данными через NFC.
- **[ShareActionProvider](~/android/user-interface/controls/action-bar.md)**  — создает поставщик, позволяющий разработчикам указывать действия совместного доступа на панели действий.
- **[Профиль пользователя](~/android/user-interface/user-profile.md)**  — предоставляет доступ к данным профиля владельца устройства.
- **[API календаря](~/android/user-interface/controls/calendar.md)**  — предоставляет доступ к данным календаря из поставщика календаря.

## <a name="x86-emulators"></a>Эмуляторы x86

ICS пока не поддерживает разработку с использованием эмулятора x86. Эмуляторы x86 поддерживаются только для Android 2.3.3, API уровня 10. Дополнительные сведения см. в статье [Настройка эмулятора Android](~/android/get-started/installation/android-emulator/index.md).

## <a name="summary"></a>Сводка

В этой статье были рассмотрены разнообразные новые технологии, которые теперь доступны в Android 4. Мы рассматривали новые функции пользовательского интерфейса, такие как *GridLayout*, *PopupMenu* и мини-приложение *Switch*. Мы также рассмотрели ряд новых поддерживаемых способов управления пользовательским интерфейсом системы, а также рассказали о том, как работать с *TextureView*. Затем мы обсудили различные новые технологии совместного использования. Мы рассмотрели, как с помощью *Android Beam* обмениваться информацией между устройствами, использующими *NFC*, обсудили новый *API календаря*, а также показали, как использовать встроенную технологию *ShareActionProvider*.
Наконец, мы изучили, как использовать поставщик *ContactsContract* для доступа к данным профиля пользователя.

## <a name="related-links"></a>Связанные ссылки

- [TextureViewDemo (пример)](/samples/xamarin/monodroid-samples/textureviewdemo)
- [CalendarDemo (пример)](/samples/xamarin/monodroid-samples/calendardemo)
- [Учебник по макету вкладок](~/android/user-interface/layouts/tab-layout/index.md)
- [Ice Cream Sandwich](https://developer.android.com/about/versions/android-4.0-highlights.html)
- [Платформа Android 4.0](https://developer.android.com/about/versions/android-4.0.html)