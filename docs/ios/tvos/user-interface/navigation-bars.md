---
title: Работа с панелями навигации tvOS в Xamarin
description: В этом документе описывается работа с панелями навигации в приложении tvOS, созданном с помощью Xamarin. В нем обсуждается настройка навигационных панелей в раскадровке и реагирование на события этих кнопок.
ms.prod: xamarin
ms.assetid: 74E396B7-87F0-46F7-BC6C-827DB8884C97
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 0f3c91e175e5ccdefeaf3d6c9c83e9eb3e012e14
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86935399"
---
# <a name="working-with-tvos-navigation-bars-in-xamarin"></a>Работа с панелями навигации tvOS в Xamarin

Панели навигации можно добавить в верхнюю часть представлений для отображения заголовка и необязательных кнопок панели навигации. Обычно они используются при переходе пользователя с главной страницы, например представления таблицы, коллекции или меню к вложенному представлению, в котором отображаются подробные сведения о выбранном элементе.

[![Пример панели навигации](navigation-bars-images/navbar01.png)](navigation-bars-images/navbar01.png#lightbox)

В дополнение к названию (отображаемому в центре) панели навигации могут содержать одну или несколько кнопок панели навигации ( `UIBarButtonItem` ) в левой и правой частях панели.

> [!IMPORTANT]
> По умолчанию панели навигации полностью прозрачны. Следует соблюдать осторожность, чтобы содержимое панели навигации оставалось доступным для чтения над содержимым под ним. Например, когда содержимое в представлении таблицы или коллекции прокручивается под ним.

<a name="Navigation-Bars-and-Storyboards"></a>

## <a name="navigation-bars-and-storyboards"></a>Панели навигации и раскадровки

Самый простой способ работы с панелями навигации в приложении Xamarin. tvOS — добавить их в пользовательский интерфейс приложения с помощью конструктора iOS.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. В **панель решения**дважды щелкните `Main.storyboard` файл и откройте его для редактирования.
1. Перетащите **панель навигации** из **панели элементов** и поместите ее в представление в верхней части экрана:

    [![Панель навигации](navigation-bars-images/navbar02.png)](navigation-bars-images/navbar02.png#lightbox)
1. Дважды щелкните **панель навигации** , чтобы выбрать **элемент навигации**. На вкладке **мини** -приложение **панель свойств**можно задать **заголовок**:

    [![Задание заголовка](navigation-bars-images/navbar03.png)](navigation-bars-images/navbar03.png#lightbox)
1. Затем можно добавить один или несколько **элементов панели** на одну из них.

    [![Элемент панели инструментов](navigation-bars-images/navbar04.png)](navigation-bars-images/navbar04.png#lightbox)
1. Наконец, подсоединить **элементы кнопки линейчатой панели** к действиям на вкладке **события** **обозревателя свойств**:

    [![Действие "элемент панели инструментов"](navigation-bars-images/navbar05.png)](navigation-bars-images/navbar05.png#lightbox)
1. Сохраните изменения.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. В **Обозреватель решений**дважды щелкните `Main.storyboard` файл и откройте его для редактирования.
1. Перетащите **панель навигации** из **панели элементов** и поместите ее в представление в верхней части экрана:

    [![Панель навигации](navigation-bars-images/navbar02-vs.png)](navigation-bars-images/navbar02-vs.png#lightbox)
1. Дважды щелкните **панель навигации** , чтобы выбрать **элемент навигации**. На вкладке **мини** -приложение **обозревателя свойств**можно задать **заголовок**:

    [![Задание заголовка](navigation-bars-images/navbar03-vs.png)](navigation-bars-images/navbar03-vs.png#lightbox)
1. Затем можно добавить один или несколько **элементов панели** на одну из них.

    [![Элементы панели инструментов](navigation-bars-images/navbar04-vs.png)](navigation-bars-images/navbar04-vs.png#lightbox)
1. Наконец, подсоединить **элементы кнопки линейчатой панели** к действиям на вкладке **события** **обозревателя свойств**:

    [![Действия элемента панели инструментов](navigation-bars-images/navbar05-vs.png)](navigation-bars-images/navbar05-vs.png#lightbox)
1. Сохраните изменения.

-----

> [!IMPORTANT]
> Хотя можно назначить такие события, как `TouchUpInside` элемент пользовательского интерфейса (например, уибуттон) в конструкторе iOS, он никогда не будет вызываться, так как Apple TV не имеет сенсорного экрана или поддержки сенсорных событий. Следует всегда использовать `Primary Action` событие при создании обработчиков событий для элементов пользовательского интерфейса tvOS.

В следующем коде приводится пример обработчиков событий для трех различных Барбуттонитемс: `ShowFirstHotel` , `ShowSecondHotel` и `ShowThirdHotel` . При щелчке по каждому элементу изменяется фоновое изображение `HotelImage` . Это редактируется в файле контроллера представления (пример `ViewController.cs` ):

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Custom Actions
        partial void ShowFirstHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel01.jpg");
        }

        partial void ShowSecondHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel02.jpg");
        }

        partial void ShowThirdHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel03.jpg");
        }
        #endregion
    }
}
```

При условии, что `Enabled` свойство кнопки не `true` охватывается другим элементом управления или представлением, можно сделать элемент с фокусом с помощью Siri Remote.

Дополнительные сведения о работе с раскадровками см. в статье [Hello, tvOS краткое руководство по началу работы](~/ios/tvos/get-started/hello-tvos.md).

<a name="Summary"></a>

## <a name="summary"></a>Итоги

В этой статье рассматривается проектирование и работа с панелями навигации в приложении Xamarin. tvOS.

## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [Руководства по tvOSму интерфейсу](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Руководством по программированию приложений для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
