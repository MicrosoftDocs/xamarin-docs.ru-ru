---
title: Настройка внешнего вида таблицы в Xamarin. iOS
description: В этом документе описывается настройка внешнего вида таблицы в Xamarin. iOS. В нем обсуждаются стили ячеек, принадлежности, разделители ячеек и пользовательские макеты ячеек.
ms.prod: xamarin
ms.assetid: 8A83DE38-0028-CB61-66F9-0FB9DE552286
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: dd79c2059d53702d947a966327bf22b2043d36c5
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91429935"
---
# <a name="customizing-a-tables-appearance-in-xamarinios"></a>Настройка внешнего вида таблицы в Xamarin. iOS

Самый простой способ изменить внешний вид таблицы — использовать другой стиль ячейки. Можно изменить стиль ячейки, используемый при создании каждой ячейки в `UITableViewSource` `GetCell` методе.

## <a name="cell-styles"></a>Стили ячеек

Существует четыре встроенных стиля:

- **По умолчанию** — поддерживает `UIImageView` .
- **Подзаголовок** — поддерживает `UIImageView` подзаголовок.
- **Значение1** — подзаголовок с правом краю, поддерживает `UIImageView` .
- **Значение2** — заголовок по правому краю, а подзаголовок — по левому краю (без изображения).

На этих снимках экрана показано, как выглядит каждый стиль:

 [![На этих снимках экрана показано, как выглядит каждый стиль](customizing-table-appearance-images/image7.png)](customizing-table-appearance-images/image7.png#lightbox)

Пример **целлдефаулттабле** содержит код для создания этих экранов. Стиль ячейки задается в `UITableViewCell` конструкторе следующим образом:

```csharp
cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Subtitle, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value1, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value2, cellIdentifier);
```

Затем можно задать [Поддерживаемые свойства](xref:UIKit.UITableViewCell) стиля ячейки:

```csharp
cell.TextLabel.Text = tableItems[indexPath.Row].Heading;
cell.DetailTextLabel.Text = tableItems[indexPath.Row].SubHeading;
cell.ImageView.Image = UIImage.FromFile("Images/" + tableItems[indexPath.Row].ImageName); // don't use for Value2
```

## <a name="accessories"></a>Accessories

В ячейки могут быть добавлены следующие аксессуары справа от представления:

- **Флажок** — можно использовать для указания множественного выбора в таблице.
- **Детаилбуттон** — реагирует на касание независимо от остальной части ячейки, позволяя ей выполнять различные функции для обращения к самой ячейке (например, открытие всплывающего окна или нового окна, которое не является частью `UINavigationController` стека).
- **Дисклосуреиндикатор** — обычно используется для указания того, что при прикосновении к ячейке откроется другое представление.
- **Детаилдисклосуребуттон** — сочетание `DetailButton` и `DisclosureIndicator` .

Вот как они выглядят:

 [![Образцы аксессуаров](customizing-table-appearance-images/image8.png)](customizing-table-appearance-images/image8.png#lightbox)

Чтобы отобразить одно из этих аксессуаров, можно задать `Accessory` свойство в `GetCell` методе:

```csharp
cell.Accessory = UITableViewCellAccessory.Checkmark;
//cell.Accessory = UITableViewCellAccessory.DisclosureIndicator;
//cell.Accessory = UITableViewCellAccessory.DetailDisclosureButton; // implement AccessoryButtonTapped
//cell.Accessory = UITableViewCellAccessory.None; // to clear the accessory
```

Когда `DetailButton` отображаются или `DetailDisclosureButton` , необходимо также переопределить, `AccessoryButtonTapped` чтобы выполнить какое-либо действие, когда оно затронуто.

```csharp
public override void AccessoryButtonTapped (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("DetailDisclosureButton Touched", tableItems[indexPath.Row].Heading, UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    owner.PresentViewController (okAlertController, true, null);

    tableView.DeselectRow (indexPath, true);
}
```

В примере **целлакцессоритабле** показан пример с использованием аксессуаров.

## <a name="cell-separators"></a>Разделители ячеек

Разделители ячеек — это ячейки таблицы, используемые для разделения таблицы. Свойства задаются для таблицы.

```csharp
TableView.SeparatorColor = UIColor.Blue;
TableView.SeparatorStyle = UITableViewCellSeparatorStyle.DoubleLineEtched;
```

Можно также добавить к разделителю эффект размытия или вибрация:

```csharp
// blur effect
TableView.SeparatorEffect =
    UIBlurEffect.FromStyle(UIBlurEffectStyle.Dark);

//vibrancy effect
var effect = UIBlurEffect.FromStyle(UIBlurEffectStyle.Light);
TableView.SeparatorEffect = UIVibrancyEffect.FromBlurEffect(effect);
```

Разделитель может также иметь отступ:

```csharp
TableView.SeparatorInset.InsetRect(new CGRect(4, 4, 150, 2));
```

## <a name="creating-custom-cell-layouts"></a>Создание пользовательских макетов ячеек

Чтобы изменить визуальный стиль таблицы, необходимо предоставить пользовательские ячейки для отображения. Пользовательская ячейка может иметь разные цвета и макеты элементов управления.

В примере Целлкустомтабле реализуется `UITableViewCell` подкласс, который определяет пользовательский макет `UILabel` s и `UIImage` с различными шрифтами и цветами. Итоговые ячейки выглядят следующим образом:

 [![Пользовательские макеты ячеек](customizing-table-appearance-images/image9.png)](customizing-table-appearance-images/image9.png#lightbox)

Класс пользовательской ячейки состоит только из трех методов:

- **Конструктор** — создает элементы управления пользовательского интерфейса и задает свойства пользовательского стиля (например, начертание, размер и цвет шрифта).
- **Упдатецелл** — метод,  `UITableView.GetCell` используемый для задания свойств ячейки.
- **Лайаутсубвиевс** — задает расположение элементов управления пользовательского интерфейса. В примере каждая ячейка имеет одинаковый макет, но более сложная ячейка (особенно с различными размерами) может потребовать различных макетов в зависимости от отображаемого содержимого.

Полный пример кода в **целлкустомтабле > CustomVegeCell.CS** приведен ниже.

```csharp
public class CustomVegeCell : UITableViewCell  {
    UILabel headingLabel, subheadingLabel;
    UIImageView imageView;
    public CustomVegeCell (NSString cellId) : base (UITableViewCellStyle.Default, cellId)
    {
        SelectionStyle = UITableViewCellSelectionStyle.Gray;
        ContentView.BackgroundColor = UIColor.FromRGB (218, 255, 127);
        imageView = new UIImageView();
        headingLabel = new UILabel () {
            Font = UIFont.FromName("Cochin-BoldItalic", 22f),
            TextColor = UIColor.FromRGB (127, 51, 0),
            BackgroundColor = UIColor.Clear
        };
        subheadingLabel = new UILabel () {
            Font = UIFont.FromName("AmericanTypewriter", 12f),
            TextColor = UIColor.FromRGB (38, 127, 0),
            TextAlignment = UITextAlignment.Center,
            BackgroundColor = UIColor.Clear
        };
        ContentView.AddSubviews(new UIView[] {headingLabel, subheadingLabel, imageView});

    }
    public void UpdateCell (string caption, string subtitle, UIImage image)
    {
        imageView.Image = image;
        headingLabel.Text = caption;
        subheadingLabel.Text = subtitle;
    }
    public override void LayoutSubviews ()
    {
        base.LayoutSubviews ();
        imageView.Frame = new CGRect (ContentView.Bounds.Width - 63, 5, 33, 33);
        headingLabel.Frame = new CGRect (5, 4, ContentView.Bounds.Width - 63, 25);
        subheadingLabel.Frame = new CGRect (100, 18, 100, 20);
    }
}
```

`GetCell`Метод нужно изменить, `UITableViewSource` чтобы создать пользовательскую ячейку:

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (cellIdentifier) as CustomVegeCell;
    if (cell == null)
        cell = new CustomVegeCell (cellIdentifier);
    cell.UpdateCell (tableItems[indexPath.Row].Heading
            , tableItems[indexPath.Row].SubHeading
            , UIImage.FromFile ("Images/" + tableItems[indexPath.Row].ImageName) );
    return cell;
}
```

## <a name="related-links"></a>Связанные ссылки

- [Воркингвистаблес (пример)](/samples/xamarin/ios-samples/workingwithtables)