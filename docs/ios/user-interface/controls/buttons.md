---
title: Кнопки в Xamarin. iOS
description: Класс Уибуттон используется для представления различных стилей кнопок на экранах iOS. В этом руководством представлены различные варианты работы с кнопками в iOS.
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/11/2018
ms.openlocfilehash: 2de52400241d45046f58222231b8d865ecf6666d
ms.sourcegitcommit: 4bbf54d2bc1df96af69814e2e5dae47be12e0474
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2021
ms.locfileid: "102602805"
---
# <a name="buttons-in-xamarinios"></a>Кнопки в Xamarin. iOS

В iOS `UIButton` класс представляет элемент управления Button.

Свойства кнопки можно изменить программно или с помощью Interface Builder Xcode.

## <a name="creating-a-button-programmatically"></a>Создание кнопки программным способом

`UIButton`Можно создать только с помощью нескольких строк кода.

- Создайте экземпляр кнопки и укажите ее тип:

  ```csharp
  UIButton myButton = new UIButton(UIButtonType.System);
  ```

  Тип кнопки задается `UIButtonType` свойством:

  - `UIButtonType.System` — Кнопка общего назначения
  - `UIButtonType.DetailDisclosure` — Указывает доступность подробных сведений, обычно о конкретном элементе в таблице.
  - `UIButtonType.InfoDark` — Указывает доступность сведений о конфигурации; темно-цветной
  - `UIButtonType.InfoLight` — Указывает доступность сведений о конфигурации; светло-цветной
  - `UIButtonType..AddContact` — Указывает, что контакт можно добавить
  - `UIButtonType.Custom` — Настраиваемая кнопка

  Дополнительные сведения о различных типах кнопок см. в следующих статьях:
  
  - Раздел ["пользовательские типы кнопок"](#custom-button-types) этого документа
  - Рецепт [типов кнопок](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/create_different_types_of_buttons)
  - [Рекомендации по работе с человеческим интерфейсом iOS](https://developer.apple.com/design/human-interface-guidelines/ios/controls/buttons/)в Apple.

- Определите размер и расположение кнопки:

  ```csharp
  myButton.Frame = new CGRect(25, 25, 300, 150);
  ```

- Задайте текст кнопки. Используйте `SetTitle` метод, для которого требуется текст и `UIControlState` значение для состояния кнопки:

  ```csharp
  myButton.SetTitle("Hello, World!", UIControlState.Normal);
  ```
  
  Ниже перечислены типы состояния кнопки.
  
  - `UIControlState.Normal`
  - `UIControlState.Highlighted`
  - `UIControlState.Disabled`
  - `UIControlState.Selected`
  - `UIControlState.Focused`
  - `UIControlState.Application`
  - `UIControlState.Reserved`
  
  Дополнительные сведения о оформлении кнопки и задании ее текста см. в следующих источниках:

  - Стиль раздела для [кнопки](#styling-a-button) в этом документе
  - Рецепт [ввода для кнопки Set](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/set_button_text) .

## <a name="handling-a-button-tap"></a>Обработка касания кнопки

Чтобы ответить на касание кнопки, укажите обработчик для `TouchUpInside` события кнопки:

```csharp
myButton.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

> [!NOTE]
> `TouchUpInside` не является единственным доступным событием кнопки. `UIButton` является дочерним классом `UIControl` , который определяет [множество различных событий](xref:UIKit.UIControlEvent).

## <a name="styling-a-button"></a>Стилизация кнопки

`UIButton` элементы управления могут находиться в разных состояниях, каждое из которых задается `UIControlState` значением — `Normal` ,,, и `Disabled` `Focused` `Highlighted` т. д. Каждому состоянию можно присвоить уникальный стиль, заданный программно или с помощью конструктора iOS.

> [!NOTE]
> Чтобы получить полный список всех `UIControlState` значений, Взгляните на [`UIKit.UIControlState enumeration`](xref:UIKit.UIControlState)
> документации.

Например, чтобы задать цвет заголовка и цвет тени для `UIControlState.Normal` :

```csharp
myButton.SetTitleColor(UIColor.White, UIControlState.Normal);
myButton.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Следующий код задает в качестве заголовка кнопки строку с атрибутом (стилизацию) для `UIControlState.Normal` и `UIControlState.Highlighted` :

```csharp
var normalAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle(normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle(highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>Типы настраиваемых кнопок

Кнопки с параметром `UIButtonType` `Custom` не имеют стилей по умолчанию. Однако можно настроить внешний вид кнопки, задав для нее изображение в различных состояниях.

```csharp
myButton.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
myButton.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
myButton.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```

В зависимости от того, касается ли пользователь кнопки, она будет отображаться как одно из следующих изображений ( `UIControlState.Normal` `UIControlState.Highlighted` и `UIControlState.Selected` состояний соответственно):

![Уиконтролстате. Обычная](buttons-images/image22.png "Уиконтролстате. Обычная") 
 ![Уиконтролстате. выделенный](buttons-images/image23.png "Уиконтролстате. выделенный") 
 ![Уиконтролстате. выбрано](buttons-images/image24.png "Уиконтролстате. выбрано")

Дополнительные сведения о работе с пользовательскими кнопками см. в разделе [использование изображения для](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/use_an_image_for_a_button) рецепта кнопки.
