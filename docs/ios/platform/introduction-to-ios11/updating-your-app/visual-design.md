---
title: Обновления визуальной разработки в iOS 11
description: В этом документе описываются обновления визуального дизайна, появившиеся в iOS 11. В нем обсуждаются изменения в панелях навигации, контроллерах поиска, полях, полноэкранном содержимом и табличных представлениях.
ms.prod: xamarin
ms.assetid: 7C300B94-0FAF-492E-A326-877419A1824B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/13/2016
ms.openlocfilehash: 06a322a92158cf413e7b33109806010c068cf733
ms.sourcegitcommit: 4bbf54d2bc1df96af69814e2e5dae47be12e0474
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2021
ms.locfileid: "102602961"
---
# <a name="visual-design-updates-in-ios-11"></a>Обновления визуальной разработки в iOS 11

В iOS 11 в Apple появились новые визуальные изменения, включая обновления панели навигации, панели поиска и табличных представлений. Кроме того, были сделаны улучшения для обеспечения большей гибкости по сравнению с полями и полноэкранным содержимым. Эти изменения описаны в этом пошаговом руководству. 

Дополнительные сведения о проектировании для iPhone X см. в статье [Разработка Apple для iPhone x](https://developer.apple.com/videos/play/fall2017/801/) видео.

## <a name="uikit"></a>UIKit

Панели UIKit были адаптированы в iOS 11, чтобы сделать их более доступными для конечных пользователей.

Одним из таких изменений является новый HUD, отображаемый при длительных нажатиях пользователем на элементе Bar. Чтобы включить эту функцию, установите `largeContentSizeImage` свойство в `UIBarItem` и добавьте больший образ с помощью [каталога активов](~/ios/app-fundamentals/images-icons/displaying-an-image.md):

```csharp
barItem.LargeContentSizeImage = UIImage.FromBundle("AccessibleImage");
```

### <a name="navigation-bar"></a>Панель переходов
в iOS 11 появились новые функции для упрощения чтения названий панелей навигации. Приложения могут отображать эту крупную должность, присваивая `PrefersLargeTitles` свойству значение true:

```csharp
NavigationController.NavigationBar.PrefersLargeTitles = true;
```

Задание большего размера заголовков в приложении приводит к увеличению _всех_ названий панелей навигации в приложении, как показано на следующем снимке экрана:

![Заголовок большой навигации](visual-design-images/image7.png)

Чтобы управлять отображением большого заголовка на панели навигации, задайте для `LargeTitleDisplayMode` элемента навигации значение `Always` , `Never` или `Automatic` .

### <a name="search-controller"></a>Контроллер поиска

iOS 11 упростила Добавление контроллера поиска непосредственно на панель навигации. После создания контроллера поиска добавьте его в панель навигации со `SearchController` свойством:

```csharp
NavigationItem.SearchController = searchController;
```

[![Заголовок большой навигации с панелью поиска](visual-design-images/image8-sml.png)](visual-design-images/image8-sml.png#lightbox)

В зависимости от функциональных возможностей приложения, панель поиска может быть скрыта, когда пользователь прокручивается по списку. Это можно настроить с помощью `HidesSearchBarWhenScrolling` Свойства.

## <a name="margins"></a>Margins

Компания Apple создала новое свойство — `directionalLayoutMargins` , которое можно использовать для задания пространства между представлениями и подпредставлениями. Используйте `directionalLayoutMargins` WITH `leading` или `trailing` инсетс. Независимо от того, является ли система языком с письмом слева направо или справа налево, в приложении iOS устанавливается соответствующий промежуток времени.

В iOS 10 и более не все представления имели минимальный размер поля, до которого они были бы выглядели. в iOS 11 появился параметр для переопределения этого метода с помощью `ViewRespectsSystemMinimumLayoutMargins` . Например, если задать для этого свойства значение false, вы можете настроить ребро инсетс на ноль:

```csharp
ViewRespectsSystemMinimumLayoutMargins = false;
View.LayoutMargins = UIEdgeInsets.Zero;
```

![Изображение, показывающее поле с нулевым отступом в iOS 11](visual-design-images/image9.png)

<a name="fullscreen"></a>

## <a name="full-screen-content"></a>Содержимое во весь экран

В iOS 7 [появились](~/ios/platform/introduction-to-ios7/ios7-ui.md#fullscreen) `topLayoutGuide` и `bottomLayoutGuide` способы ограничения представлений, чтобы они не были скрыты с помощью UIKit полос и находятся в видимой области экрана. Они не рекомендуются в iOS 11 в пользу максимальной _защищенной области_.

Область безопасности — это новый способ подумать о видимом пространстве приложения и о том, как ограничения добавляются между представлениями и супер-представлением. Например, рассмотрим следующее изображение:

[![Область безопасности и верхняя и Нижняя структура макета](visual-design-images/image10-sml.png)](visual-design-images/image10.png#lightbox)

Ранее, если было добавлено представление и требовалось, чтобы оно отображалось в зеленой области выше, оно будет ограничено _нижним_ `TopLayoutGuide` и _верхним границами_ `BottomLayoutGuide` . В iOS 11 его можно ограничить _максимальным_ и _нижним_ частями защищенной области. См. пример ниже.

```csharp
var safeGuide = View.SafeAreaLayoutGuide;
imageView.TopAnchor.ConstraintEqualTo(safeGuide.TopAnchor).Active = true;
safeGuide.BottomAnchor.ConstraintEqualTo(imageView.BottomAnchor).Active = true;
```

## <a name="table-view"></a>Представление таблицы

Уитаблевиев имеет ряд небольших, но существенных изменений в iOS 11.

По умолчанию размер верхних, нижних колонтитулов и ячеек теперь автоматически изменяется в зависимости от их содержимого. Чтобы отказаться от этого поведения автоматического изменения размера, установите значение `EstimatedRowHeight` , `EstimatedSectionHeaderHeight` или `EstimatedSectionFooterHeight` равным нулю.

Однако в некоторых случаях (например, при добавлении Уитаблевиевконтроллер при использовании существующих раскадровок в Interface Builder) может потребоваться вручную включить ячейки с самостоятельным размером. Для этого убедитесь, что в табличном представлении установлены следующие свойства для ячеек, верхних и нижних колонтитулов, соответственно:

```csharp
// Cells
TableView.RowHeight = UITableView.AutomaticDimension;
TableView.EstimatedRowHeight = UITableView.AutomaticDimension;

// Header
TableView.SectionHeaderHeight = UITableView.AutomaticDimension;
TableView.EstimatedSectionHeaderHeight = 40f;

//Footer
TableView.SectionFooterHeight = UITableView.AutomaticDimension;
TableView.EstimatedSectionFooterHeight = 40f;

```

iOS 11 расширил функциональность действий строк. `UISwipeActionsConfiguration` было введено, чтобы определить набор действий, которые должны выполняться, когда пользователь просматривает любое направление в строке в табличном представлении. Такое поведение аналогично поведению собственного mail. app. Дополнительные сведения см. в разделе Пошаговое руководств по [действиям со строками](~/ios/user-interface/controls/tables/row-action.md) .

Табличные представления поддерживают перетаскивание в iOS 11. Дополнительные сведения см. в разделе с инструкциями по [перетаскиванию](~/ios/platform/introduction-to-ios11/drag-and-drop.md#uitableview) .

## <a name="related-links"></a>Связанные ссылки

- [Новые возможности iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Обновленная страница продукта App Store (Apple)](https://developer.apple.com/app-store/product-page/)
- [Проектирование для iPhone X (Apple) (видео)](https://developer.apple.com/videos/play/fall2017/801/)
- [Обновление приложения для iOS 11 (ВВДК) (видео)](https://developer.apple.com/videos/play/wwdc2017/204/)
