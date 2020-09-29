---
title: Работа с элементами управления страницы tvOS в Xamarin
description: В этом документе описывается работа с элементами управления страницы tvOS в приложении, созданном с помощью Xamarin. Он содержит обобщенное описание элементов управления страницы, объясняет, как их настроить в раскадровках, а также как реагировать на события изменения страницы.
ms.prod: xamarin
ms.assetid: 19198D46-7BBE-4D04-9BFA-7D1C5C9F9FC6
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 47e9c1ed4a043f7d11b5dfd52dca903e0a613b33
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436428"
---
# <a name="working-with-tvos-page-controls-in-xamarin"></a>Работа с элементами управления страницы tvOS в Xamarin

Иногда может потребоваться отобразить ряд страниц или изображений в приложении Xamarin. tvOS. Элемент управления "страница" предназначен для четкого отображения страницы, с которой пользователь выходит из максимального числа страниц. Элемент управления страницы отображает ряд точек на фоне темной, овалной в форме фона. На текущей странице отображается заполненная точка, все остальные страницы отображаются как пустые точки. Элемент управления "страница" будет обрезать внешние наиболее точки, если в области фона слишком много.

[![Пример элемента управления страницы](page-controls-images/page01.png)](page-controls-images/page01.png#lightbox)

Элемент управления страницы в неинтерактивном элементе, предназначенный для предоставления отзывов только пользователю. Необходимо добавить другие элементы управления для изменения номера текущей страницы (например, жестов или кнопок).

При использовании элемента управления "страница" Apple предлагает следующие рекомендации:

- **Использование только для полных коллекций** . элементы управления страницы лучше всего работают в полноэкранном режиме для отображения нескольких страниц, существующих в одной коллекции.
- **Ограничьте число страниц** — элементы управления страницы лучше подходят для десяти (10) или меньше страниц и не более двадцати (20) страниц. Для более двадцати страниц рассмотрите возможность использования [представления коллекций](~/ios/tvos/user-interface/collection-views.md) и отображения страниц в сетке.

<a name="Page-Controls-and-Storyboards"></a>

## <a name="page-controls-and-storyboards"></a>Элементы управления страницы и раскадровки

Самым простым способом работы с элементами управления страницы в приложении Xamarin. tvOS является добавление их в пользовательский интерфейс приложения с помощью конструктора iOS.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. В **панель решения**дважды щелкните `Main.storyboard` файл и откройте его для редактирования.
1. Перетащите **элемент управления страницы** из **панели элементов** и поместите его в представление:

    [![Элемент управления страницы](page-controls-images/page02.png)](page-controls-images/page02.png#lightbox)
1. На **вкладке мини** -приложение **панель свойств**можно настроить несколько свойств элемента управления страницы, таких как **Текущая страница** и **число страниц**:

    [![Вкладка мини-приложения](page-controls-images/page03.png)](page-controls-images/page03.png#lightbox)
1. Затем добавьте элементы управления или жесты в представление, чтобы перемещаться назад и вперед по коллекции страниц.
1. Наконец, назначьте **имена** элементам управления, чтобы вы могли реагировать на них в коде C#. Пример:

    [![Назовите элемент управления](page-controls-images/page04.png)](page-controls-images/page04.png#lightbox)
1. Сохраните изменения.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. В **Обозреватель решений**дважды щелкните `Main.storyboard` файл и откройте его для редактирования.
1. Перетащите **элемент управления страницы** из **панели элементов** и поместите его в представление:

    [![Элемент управления страницы](page-controls-images/page02-vs.png)](page-controls-images/page02-vs.png#lightbox)
1. На **вкладке мини** -приложение **обозревателя свойств**можно настроить несколько свойств элемента управления страницы, таких как **Текущая страница** и **число страниц**:

    [![Вкладка мини-приложения](page-controls-images/page03-vs.png)](page-controls-images/page03-vs.png#lightbox)
1. Затем добавьте элементы управления или жесты в представление, чтобы перемещаться назад и вперед по коллекции страниц.
1. Наконец, назначьте **имена** элементам управления, чтобы вы могли реагировать на них в коде C#. Пример:

    [![Назовите элемент управления](page-controls-images/page04-vs.png)](page-controls-images/page04-vs.png#lightbox)
1. Сохраните изменения.

-----

> [!IMPORTANT]
> Хотя можно назначить такие события, как `TouchUpInside` элемент пользовательского интерфейса (например, уибуттон) в конструкторе iOS, он никогда не будет вызываться, так как Apple TV не имеет сенсорного экрана или поддержки сенсорных событий. Следует всегда использовать `Primary Action` событие при создании обработчиков событий для элементов пользовательского интерфейса tvOS.

Измените файл контроллера представления (example `ViewController.cs` ) и добавьте код для управления изменяемыми страницами. Пример:

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Computed Properties
        public nint PageNumber { get; set; } = 0;
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            PageView.Pages = 6;
            ShowCat ();
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Custom Actions
        partial void NextCat (UIBarButtonItem sender) {

            // Display next Cat
            if (++PageNumber > 5) {
                PageNumber = 5;
            }
            ShowCat();

        }

        partial void PreviousCat (UIBarButtonItem sender) {
            // Display previous cat
            if (--PageNumber < 0) {
                PageNumber = 0;
            }
            ShowCat();
        }
        #endregion

        #region Private Methods
        private void ShowCat() {

            // Adjust UI
            PreviousButton.Enabled = (PageNumber > 0);
            NextButton.Enabled = (PageNumber < 5);
            PageView.CurrentPage = PageNumber;

            // Display new cat
            CatView.Image = UIImage.FromFile(string.Format("Cat{0:00}.jpg",PageNumber+1));
        }
        #endregion
    }
}
```

Давайте подробнее рассмотрим два свойства элемента управления страницы. Прежде всего, чтобы указать максимальное количество страниц, используйте следующую команду:

```csharp
PageView.Pages = 6;
```

Чтобы изменить номер текущей страницы, используйте следующий код:

```csharp
PageView.CurrentPage = PageNumber;
```

Значение `CurrentPage` свойства равно нулю (0), поэтому первая страница будет равняться нулю, а последняя будет равна 1 минус максимальному количеству страниц.

Дополнительные сведения о работе с раскадровками см. в статье [Hello, tvOS краткое руководство по началу работы](~/ios/tvos/get-started/hello-tvos.md).

<a name="Summary"></a>

## <a name="summary"></a>Сводка

В этой статье описано проектирование и работа с элементом управления страницы в приложении Xamarin. tvOS.

## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](/samples/browse/?products=xamarin&term=Xamarin.iOS%2btvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [Руководства по tvOSму интерфейсу](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Руководством по программированию приложений для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)