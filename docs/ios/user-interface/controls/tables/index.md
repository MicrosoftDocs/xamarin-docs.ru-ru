---
title: Работа с таблицами и ячейками в Xamarin. iOS
description: В этом документе содержатся ссылки на различные руководства, описывающие отображение данных с помощью элемента управления Уитаблевиев в приложении Xamarin. iOS.
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 01/06/2016
ms.openlocfilehash: 4974992890348541538ea54823f8f0ea1f22b24e
ms.sourcegitcommit: 4bbf54d2bc1df96af69814e2e5dae47be12e0474
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2021
ms.locfileid: "102602701"
---
# <a name="working-with-tables-and-cells-in-xamarinios"></a>Работа с таблицами и ячейками в Xamarin. iOS

> [!WARNING]
> Конструктор iOS устарел в Visual Studio 2019 версии 16,8 и Visual Studio 2019 для Mac версии 8,8 и удален в Visual Studio 2019 версии 16,9 и Visual Studio для Mac версии 8,9.
> Рекомендуемый способ создания пользовательских интерфейсов iOS — непосредственно на компьютере Mac с Interface Builder. Дополнительные сведения см. в разделе [Разработка пользовательских интерфейсов с помощью Xcode](~/ios/user-interface/storyboards/index.md). 

В этом разделе описываются классы, используемые для создания и отображения таблиц, а также приводятся примеры их использования в Xamarin. iOS. Он охватывает использование внешнего вида по умолчанию для таблиц, настройку макета, реализацию редактирования и использование конструктора Xamarin iOS для визуального проектирования таблицы. Иногда отображение представляет собой список строк (например, музыкальное приложение) и в других случаях трудно распознать элемент управления таблицы (например, редактирование в приложении "Контакты" или диалог в приложении "сообщения").

Для тех, кто работает с приложениями на разных платформах с помощью Xamarin. Android, элемент управления Уитаблевиев аналогичен классу ListView в Android (и класс Уитаблевиевсаурце похож на классы адаптеров Android).

В этих статьях подробно рассматривается работа с таблицами, в том числе:

- **Части таблицы** — знакомство с визуальными элементами элемента управления и их объяснение  `UITableView` . 
- **Отображение данных в таблицах** — демонстрация создания и заполнения таблицы, использование различных стилей таблиц и ячеек и избежание проблем с памятью путем повторного запуска объектов ячеек. 
- **Расширенное использование** — создание пользовательских ячеек и использование функций редактирования класса уитаблевиев. 
- **Визуальное создание таблицы** с помощью Xamarin Designer для iOS для создания интерфейса с раскадровкой, основанного на таблицах. 

## <a name="contents"></a>Содержимое

 [&amp;Функциональные возможности частей таблиц](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [Заполнение таблицы данными](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [Настройка вида таблицы](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [Редактирование](~/ios/user-interface/controls/tables/editing.md)

 [Действия со строками](~/ios/user-interface/controls/tables/row-action.md)

 [Создание таблиц в раскадровке](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)

 [Автоматическое изменение высоты строки](~/ios/user-interface/controls/tables/autosizing-row-height.md)

## <a name="related-links"></a>Связанные ссылки

- [Воркингвистаблес (пример)](/samples/xamarin/ios-samples/workingwithtables)
- [Таблицы в раскадровках (пример)](/samples/xamarin/ios-samples/storyboardtable)
- [Введение в раскадровку](~/ios/user-interface/storyboards/index.md)
- [Раскадровка для рецепта Таблевиев](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/storyboard_a_tableview)
- [Введение в бескасание. Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Пример Таблидитинг на GitHub](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [Пример Таблепартс на GitHub](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [Пример Таблеандцеллстилес на GitHub](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [Справочник по классам Уитаблевиев](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [Справочник по классам Уитаблевиевцелл](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [уитаблевиевделегате](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [уитаблевиевдатасаурце](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)