---
title: Введение в iOS 6
description: В этом документе содержатся ссылки на руководства, в которых описываются функции iOS 6. Обсуждаются представления коллекций, PassKit, социальные платформы и изменения в StoreKit.
ms.prod: xamarin
ms.assetid: 242DA7E3-8FD8-5F20-285D-603259CA622D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: b81e7b980c37f238fe9c2a299aa360cc01294ebe
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997193"
---
# <a name="introduction-to-ios-6"></a>Введение в iOS 6

_в iOS 6 реализовано множество новых технологий для разработки приложений, которые Xamarin. iOS 6 предоставляет разработчикам на C#._

[![Логотип iOS 6](images/ios6-large.jpg)](images/ios6-large.jpg#lightbox)

В iOS 6 и Xamarin. iOS 6 разработчики теперь имеют множество возможностей при их реализации для создания приложений iOS, включая те, которые нацелены на iPhone 5.
В этом документе перечислены некоторые из наиболее интересных новых функций и ссылки на статьи по каждому разделу. Кроме того, он касается нескольких изменений, которые будут важны, когда разработчики переходят на iOS 6 и новое разрешение iPhone 5.

## <a name="introduction-to-collection-views"></a>[Общие сведения о представлениях коллекций](~/ios/user-interface/controls/uicollectionview.md)

Представления коллекций позволяют отображать содержимое с помощью произвольных макетов. Они позволяют легко создавать макеты, основанные на сетке, при поддержке пользовательских макетов. Дополнительные сведения см. в разделе [Введение в представления коллекции](~/ios/user-interface/controls/uicollectionview.md) .

## <a name="introduction-to-passkit"></a>[Введение в PassKit](~/ios/platform/passkit.md)

Платформа PassKit позволяет приложениям взаимодействовать с цифровыми проходами, управляемыми в приложении расчетной книжки. Дополнительные сведения см. в статье [Введение в проход по пакету](~/ios/platform/passkit.md).

## <a name="introduction-to-eventkit"></a>[Введение в EventKit](~/ios/platform/eventkit.md)

Платформа EventKit предоставляет способ доступа к календарям, событиям календаря и данным напоминаний, которые хранятся в базе данных календаря. Доступ к календарям и событиям календаря был доступен с iOS 4, но теперь iOS 6 предоставляет доступ к данным напоминаний. Дополнительные сведения см [. в разделе](~/ios/platform/eventkit.md) [нтродуктион to EventKit](~/ios/platform/eventkit.md) Guide.

## <a name="introduction-to-the-social-framework"></a>[Введение в социальные платформы](~/ios/platform/social-framework.md)

Социальные платформы предоставляют унифицированный API для взаимодействия с социальными сетями, включая Twitter и Facebook, а также Синавеибо для пользователей в Китае. Дополнительные сведения см. в разделе Введение в руководством по [социальной инфраструктуре](~/ios/platform/social-framework.md) .

## <a name="changes-to-storekit"></a>[Изменения в StoreKit](changes-to-storekit.md)

Компания Apple предоставила две новые функции в магазине Store Kit: приобретение и скачивание содержимого iTunes или App Store из приложения, а также размещение файлов содержимого для покупок в приложении!. Дополнительные сведения см. в разделе [изменения для Store Kit](changes-to-storekit.md) Guide.

## <a name="other-changes"></a>Другие изменения

### <a name="viewwillunload-and-viewdidunload-deprecated"></a>Виеввиллунлоад и Виевдидунлоад устарели

`ViewWillUnload`Методы и `ViewDidUnload` `UIViewController` в iOS 6 больше не вызываются. В предыдущих версиях iOS эти методы могли использоваться приложениями для сохранения состояния перед выгрузкой представления и кодом очистки соответственно.

Например, Visual Studio для Mac создаст метод `ReleaseDesignerOutlets` с именем, показанный ниже, который затем будет вызываться из `ViewDidUnload` :

```csharp
void ReleaseDesignerOutlets ()
{
    if (myOutlet != null) {
        myOutlet.Dispose ();
        myOutlet = null;
    }
}
```

Однако в iOS 6 больше не требуется вызывать `ReleaseDesignerOutlets` .   

Для кода очистки приложения iOS 6 должны использовать `DidReceiveMemoryWarning` . Однако код, который вызывает, `Dispose` следует использовать экономно и только для объектов, интенсивно использующих память, как показано ниже:

```csharp
if (myImageView != null){
    if (myImageView.Superview == null){
        myImageView.Dispose();
        myImageView = null;
    }
}
```

Опять же, вызов `Dispose` , приведенный выше, должен быть редко нужен. Как правило, большинство приложений должны удалить обработчики событий.

В случае сохранения состояния приложения могут выполнять это в `ViewWillDisappear` и `ViewDidDisappear` вместо `ViewWillUnload` .

### <a name="iphone-5-resolution"></a>Разрешение iPhone 5

устройства iPhone 5 имеют разрешение 640x1136. Приложения, нацеленные на предыдущие версии iOS, будут отображаться леттербоксед при запуске на iPhone 5, как показано ниже:

 [![Приложения, нацеленные на предыдущие версии iOS, будут отображаться леттербоксед при запуске на iPhone 5](images/01-letterboxed.png)](images/01-letterboxed.png#lightbox)

Чтобы приложение отображалось на устройстве iPhone 5 в полноэкранном режиме, просто добавьте образ с `Default-568h@2x.png` разрешением 640x1136. На следующем снимке экрана показано приложение, запущенное после включения этого образа:

 [![На этом снимке экрана показано приложение, запущенное после включения этого образа](images/02-fullscreen.png)](images/02-fullscreen.png#lightbox)

### <a name="subclassing-uinavigationbar"></a>Подклассировать Уинавигатионбар

В iOS 6 `UINavigationBar` можно создавать подклассы. Это позволяет получить дополнительный контроль над внешним видом и поведением `UINavigationBar` . Например, приложения могут создавать подклассы для добавления подпредставлений, анимировать эти представления и изменять границы `UINavigationBar` .

В приведенном ниже коде показан пример подкласса `UINavigationBar` , который добавляет `UIImageView` :

```csharp
public class CustomNavBar : UINavigationBar
{
    UIImageView iv;
    public CustomNavBar (IntPtr h) : base(h)
    {
        iv = new UIImageView (UIImage.FromFile ("monkey.png"));
        iv.Frame = new CGRect (75, 0, 30, 39);
    }
    public override void Draw (RectangleF rect)
    {
        base.Draw (rect);
        TintColor = UIColor.Purple;
        AddSubview (iv);
    }
}
```

Чтобы добавить в объект подкласс `UINavigationBar` `UINavigationController` , используйте `UINavigationController` конструктор, принимающий тип `UINavigationBar` и `UIToolbar` , как показано ниже:

```csharp
navController = new UINavigationController (typeof(CustomNavBar), typeof(UIToolbar));
```

Использование этого `UINavigationBar` подкласса приводит к отображению представления изображения, как показано на следующем снимке экрана:

 [![Использование этого подкласса Уинавигатионбар приводит к отображению представления изображения, как показано на этом снимке экрана](images/03-navbar.png)](images/03-navbar.png#lightbox)

### <a name="interface-orientation"></a>Ориентация интерфейса

До появления приложений iOS 6 можно было переопределять `ShouldAutorotateToInterfaceOrientation` , возвращая значение true для любой ориентации, поддерживаемой определенным контроллером. Например, следующий код будет использоваться для поддержки только книжной ориентации:

```csharp
public override bool ShouldAutorotateToInterfaceOrientation (UIInterfaceOrientation toInterfaceOrientation)
    {
        return (toInterfaceOrientation == UIInterfaceOrientation.Portrait);
    }
```

В iOS 6 `ShouldAutorotateToInterfaceOrientation` является устаревшим.
Вместо этого приложения могут переопределяться `GetSupportedInterfaceOrientations` на контроллере корневого представления, как показано ниже:

```csharp
public override UIInterfaceOrientationMask GetSupportedInterfaceOrientations ()
    {
        return UIInterfaceOrientationMask.Portrait;
    }
```

На iPad это значение по умолчанию равно всем четырем ориентация, если `GetSupportedInterfaceOrientation` не реализовано. На iPhone и iPod touch по умолчанию заданы все ориентации, кроме `PortraitUpsideDown` .
