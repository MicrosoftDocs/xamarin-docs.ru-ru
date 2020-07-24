---
title: Работа с tvOS текста и полями поиска в Xamarin
description: В этом документе описывается работа с текстом и полями поиска в приложении tvOS, созданном с помощью Xamarin. В нем представлен общий обзор полей текста и поиска, обсуждаются клавиатуры, Интеграция раскадровки, модели данных поиска и многое другое.
ms.prod: xamarin
ms.assetid: 9EE63CA6-2F31-4EE0-AAE5-82E18CFAC06C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 3822a03a78c91777ad9614ec298bbac1ab2332b1
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939481"
---
# <a name="working-with-tvos-text-and-search-fields-in-xamarin"></a>Работа с tvOS текста и полями поиска в Xamarin

При необходимости приложение Xamarin. tvOS может запрашивать от пользователя небольшие фрагменты текста (например, идентификаторы пользователей и пароли), используя текстовое поле и экранную клавиатуру:

[![Пример поля поиска](text-fields-and-search-images/intro01.png)](text-fields-and-search-images/intro01.png#lightbox)

При необходимости можно указать возможность поиска по ключевым словам для содержимого приложения с помощью поля поиска:

[![Примеры результатов поиска](text-fields-and-search-images/intro02.png)](text-fields-and-search-images/intro02.png#lightbox)

В этом документе рассматриваются сведения о работе с полями текста и поиска в приложении Xamarin. tvOS.

<a name="About-Text-and-Search-Fields"></a>

## <a name="about-text-and-search-fields"></a>О тексте и полях поиска

Как упоминалось выше, Xamarin. tvOS может представлять одно или несколько текстовых полей для получения небольшого объема текста от пользователя с помощью экранной (или дополнительной клавиатуры Bluetooth в зависимости от версии tvOS, установленной пользователем).

Кроме того, если приложение предоставляет пользователям большие объемы содержимого (например, музыку, фильмы или коллекцию изображений), может потребоваться включить поле поиска, позволяющее пользователю ввести небольшой объем текста для фильтрации списка доступных элементов.

<a name="Text-Fields"></a>

## <a name="text-fields"></a>Текстовые поля

В tvOS текстовое поле представляется как поле ввода с фиксированной высотой и круглым углом, которое выводит экранную клавиатуру при нажатии пользователем.

[![Текстовые поля в tvOS](text-fields-and-search-images/text01.png)](text-fields-and-search-images/text01.png#lightbox)

Когда пользователь перемещает [фокус](~/ios/tvos/app-fundamentals/navigation-focus.md) на заданное текстовое поле, его размер увеличивается и отображается глубокая тень. Это необходимо учитывать при проектировании пользовательского интерфейса, так как текстовые поля могут перекрывать другие элементы пользовательского интерфейса при наличии фокуса.

В Apple имеются следующие рекомендации по работе с текстовыми полями:

- **Старайтесь использовать текстовый ввод с осторожностью** — из-за особенностей экранной клавиатуры ввод длинных разделов текста или заполнение нескольких текстовых полей является утомительным пользователем. Лучшим решением является ограничение объема текстового ввода с помощью списков выбора или [кнопок](~/ios/tvos/user-interface/buttons.md).
- **Использовать подсказки для связи** . текстовое поле может отображать заполнители "подсказки", если они пусты. Там, где это применимо, используйте указания для описания назначения текстового поля вместо отдельной метки.
- **Выберите подходящий тип клавиатуры по умолчанию** -tvOS предоставляет несколько различных, назначенных типов клавиатуры, которые можно указать для текстового поля. Например, клавиатура адреса электронной почты может упростить ввод, позволяя пользователю выбрать из списка недавно введенных адресов.
- **При необходимости используйте безопасные текстовые поля** — в безопасном текстовом поле отображаются символы, введенные в виде точек (вместо реальных букв). При сборе конфиденциальной информации, такой как пароли, всегда следует использовать защищенное текстовое поле.

<a name="Keyboards"></a>

## <a name="keyboards"></a>Клавиатуры

Когда пользователь щелкает текстовое поле в пользовательском интерфейсе, отображается линейная экранная клавиатура. Пользователь использует сенсорную поверхность [Siri Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) для выбора отдельных букв с клавиатуры и ввода требуемой информации:

[![Удаленная клавиатура Siri](text-fields-and-search-images/keyboard01.png)](text-fields-and-search-images/keyboard01.png#lightbox)

Если в текущем представлении имеется более одного текстового поля, будет автоматически отображена кнопка **Далее** , чтобы перейти к следующему текстовому полю. Кнопка **done (Готово** ) будет отображаться для последнего текстового поля, завершающего ввод текста и возвращающего пользователя на предыдущий экран.

В любой момент времени пользователь может также нажать кнопку **меню** в текстовом элементе Siri Remote to End Text и снова вернуться к предыдущему экрану.

У Apple есть следующие рекомендации по работе с экранными клавиатурами:

- **Выберите подходящий тип клавиатуры по умолчанию** -tvOS предоставляет несколько различных, назначенных типов клавиатуры, которые можно указать для текстового поля. Например, клавиатура адреса электронной почты может упростить ввод, позволяя пользователю выбрать из списка недавно введенных адресов.
- **При необходимости используйте представления** для назначений клавиатуры — помимо стандартных сведений, которые всегда отображаются, дополнительные представления аксессуаров (например, изображения или метки) можно добавить на экранную клавиатуру, чтобы уточнить назначение текстового ввода или помочь пользователю ввести необходимую информацию.

Дополнительные сведения о работе с экранной клавиатурой см. в разделе Apple [уикэйбоардтипе](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITextInputTraits_Protocol/index.html#//apple_ref/c/tdef/UIKeyboardType), [управление клавиатурой](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html#//apple_ref/doc/uid/TP40009542-CH5-SW1), [пользовательские представления для ввода данных](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/InputViews/InputViews.html#//apple_ref/doc/uid/TP40009542-CH12-SW1) и [руководство по программированию текста в](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/Introduction/Introduction.html) документации по iOS.

<a name="Search"></a>

## <a name="search"></a>Поиск

Поле поиска представляет собой специализированный экран, предоставляющий текстовое поле и экранную клавиатуру, позволяющие пользователю отфильтровать коллекцию элементов, отображаемых под клавиатурой:

[![Примеры результатов поиска](text-fields-and-search-images/search01.png)](text-fields-and-search-images/search01.png#lightbox)

По мере того, как пользователь вводит буквы в поле поиска, приведенные ниже результаты будут автоматически отражать результаты поиска. Пользователь может в любое время переместить фокус на результаты и выбрать один из представленных элементов.

В Apple имеются следующие рекомендации по работе с полями поиска:

- **Предоставить последние поисковые** запросы. так как ввод текста с помощью Siri удаленного может быть утомительным, а пользователи, как правило, выполняют поиск, рекомендуется добавить раздел последних результатов поиска перед текущими результатами в области клавиатуры.
- **По возможности ограничьте количество результатов,** так как большой список элементов может быть затруднен для пользователя при анализе и перемещении, попробуйте ограничить число возвращаемых результатов.
- **Если это уместно, предоставьте фильтры результатов поиска** . Если содержимое, предоставленное приложением, предоставляется самому себе, рассмотрите возможность добавления полос области, чтобы разрешить пользователю выполнять дальнейшую фильтрацию возвращаемых результатов поиска.

Дополнительные сведения см. в [справочнике по классу Уисеарчконтроллер](https://developer.apple.com/library/tvos/documentation/UIKit/Reference/UISearchController/index.html)Apple.

<a name="Working-with-Text-Fields"></a>

## <a name="working-with-text-fields"></a>Работа с текстовыми полями

Самым простым способом работы с текстовыми полями в приложении Xamarin. tvOS является добавление их в проект пользовательского интерфейса с помощью конструктора iOS.

Выполните следующие действия.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. В **панель решения**дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования.
1. Перетащите одно или несколько **текстовых полей из поля** int в область конструктора в представление:

    [![Текстовое поле](text-fields-and-search-images/text02.png)](text-fields-and-search-images/text02.png#lightbox)
1. Выберите **текстовые поля** и присвойте каждому уникальному **имени** на вкладке **мини** -приложение **панель свойств**:

    [![Вкладка мини-приложения Панель свойств](text-fields-and-search-images/text03.png)](text-fields-and-search-images/text03.png#lightbox)
1. В разделе **текстовое поле** можно определить такие элементы, как указание **заполнителя** и **значение**по умолчанию:

    [![Раздел текстового поля](text-fields-and-search-images/text04.png)](text-fields-and-search-images/text04.png#lightbox)
1. Прокрутите вниз, чтобы определить такие свойства, как **Проверка орфографии**, **регистр букв** и **тип клавиатуры**по умолчанию:

    [![Проверка орфографии, регистр букв и тип клавиатуры по умолчанию](text-fields-and-search-images/text05.png)](text-fields-and-search-images/text05.png#lightbox)
1. Сохраните изменения в раскадровке.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. В **обозревателе решений** дважды щелкните файл `Main.storyboard`, чтобы открыть его для редактирования.
1. Перетащите одно или несколько **текстовых полей из поля** int в область конструктора в представление:

    [![Текстовое поле](text-fields-and-search-images/text02-vs.png)](text-fields-and-search-images/text02-vs.png#lightbox)
1. Выберите **текстовые поля** и присвойте каждому из них **уникальное имя** на вкладке **мини** -приложение **обозревателя свойств**:

    [![Вкладка мини-приложения](text-fields-and-search-images/text03-vs.png)](text-fields-and-search-images/text03-vs.png#lightbox)
1. В разделе **текстовое поле** можно определить такие элементы, как указание **заполнителя** и **значение**по умолчанию:

    [![Раздел текстового поля](text-fields-and-search-images/text04-vs.png)](text-fields-and-search-images/text04-vs.png#lightbox)
1. Прокрутите вниз, чтобы определить такие свойства, как **Проверка орфографии**, **регистр букв** и **тип клавиатуры**по умолчанию:

    [![Проверка орфографии, регистр букв и тип клавиатуры по умолчанию](text-fields-and-search-images/text05-vs.png)](text-fields-and-search-images/text05-vs.png#lightbox)
1. Сохраните изменения в раскадровке.

-----

В коде можно получить или задать значение текстового поля с помощью его `Text` Свойства:

```csharp
Console.WriteLine ("User ID {0} and Password {1}", UserId.Text, Password.Text);
```

При необходимости можно использовать `Started` `Ended` события текстового поля и, чтобы реагировать на начало и конец ввода текста.

<a name="Working-with-Search-Fields"></a>

## <a name="working-with-search-fields"></a>Работа с полями поиска

Самым простым способом работы с полями поиска в приложении Xamarin. tvOS является добавление их в структуру пользовательского интерфейса с помощью конструктора интерфейсов.

Выполните следующие действия.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. В **панель решения**дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования.
1. Перетащите новый контроллер представления коллекции в раскадровку для представления результатов поиска пользователя:

    [![Контроллер представления коллекции](text-fields-and-search-images/search02.png)](text-fields-and-search-images/search02.png#lightbox)
1. На вкладке **мини** -приложение **панель свойств**используйте `SearchResultsViewController` для **класса** и `SearchResults` для **идентификатора раскадровки**:

    [![Вкладка мини-приложения](text-fields-and-search-images/search03.png)](text-fields-and-search-images/search03.png#lightbox)
1. Выберите **прототип ячейки** в области конструктора.
1. На вкладке **мини** -приложение **обозревателя свойств**используйте `SearchResultCell` для **класса** и `ImageCell` для **идентификатора**:

    [![Вкладка мини-приложения](text-fields-and-search-images/search04.png)](text-fields-and-search-images/search04.png#lightbox)
1. Создайте макет для **прототипа ячейки** и предоставьте каждому элементу уникальное **имя** на вкладке **мини** -приложение **обозревателя свойств**:

    [![Создание макета прототипа ячейки](text-fields-and-search-images/search05.png)](text-fields-and-search-images/search05.png#lightbox)
1. Сохраните изменения в раскадровке.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. В **обозревателе решений** дважды щелкните файл `Main.storyboard`, чтобы открыть его для редактирования.
1. Перетащите новый контроллер представления коллекции в раскадровку для представления результатов поиска пользователя:

    [![Контроллер представления коллекции](text-fields-and-search-images/seach02-vs.png)](text-fields-and-search-images/seach02-vs.png#lightbox)
1. На вкладке **мини** -приложение **обозревателя свойств**используйте `SearchResultsViewController` для **класса** и `SearchResults` для **идентификатора раскадровки**:

    [![Вкладка мини-приложения](text-fields-and-search-images/search03-vs.png)](text-fields-and-search-images/search03-vs.png#lightbox)
1. Выберите **прототип ячейки** в области конструктора.
1. На вкладке **мини** -приложение **обозревателя свойств**используйте `SearchResultCell` для **класса** и `ImageCell` для **идентификатора**:

    [![Вкладка мини-приложения](text-fields-and-search-images/search04-vs.png)](text-fields-and-search-images/search04-vs.png#lightbox)
1. Создайте макет для **прототипа ячейки** и предоставьте каждому элементу уникальное **имя** на вкладке **мини** -приложение **обозревателя свойств**:

    [![Создание макета прототипа ячейки](text-fields-and-search-images/search05-vs.png)](text-fields-and-search-images/search05-vs.png#lightbox)
1. Сохраните изменения в раскадровке.

-----

<a name="Provide-a-Data-Model"></a>

### <a name="provide-a-data-model"></a>Предоставление модели данных

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Далее необходимо указать класс, который будет использоваться в качестве модели данных для результатов, которые будет искать пользователь. В **Обозреватель решений**щелкните правой кнопкой мыши имя проекта и выберите **Добавить**  >  **новый файл...**  >  **Общие**  >  сведения **Пустой класс** и укажите **имя**:

[![Выберите пустой класс и укажите имя](text-fields-and-search-images/search06.png)](text-fields-and-search-images/search06.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Далее необходимо указать класс, который будет использоваться в качестве модели данных для результатов, которые будет искать пользователь. В **Обозреватель решений**щелкните правой кнопкой мыши имя проекта и выберите команду **Добавить**  >  **новый элемент...**  >  **Apple**  >  **Прочие**  >  **И укажите** **имя**:

[![Выбор класса и указание имени](text-fields-and-search-images/search06-vs.png)](text-fields-and-search-images/search06-vs.png#lightbox)

-----

Например, приложение, которое позволяет пользователю искать коллекцию изображений по названию и ключевому слову, может выглядеть следующим образом:

```csharp
using System;
using Foundation;

namespace tvText
{
    public class PictureInformation : NSObject
    {
        #region Computed Properties
        public string Title { get; set;}
        public string ImageName { get; set;}
        public string Keywords { get; set;}
        #endregion

        #region Constructors
        public PictureInformation (string title, string imageName, string keywords)
        {
            // Initialize
            this.Title = title;
            this.ImageName = imageName;
            this.Keywords = keywords;
        }
        #endregion
    }
}
```

<a name="The-Collection-View-Cell"></a>

### <a name="the-collection-view-cell"></a>Ячейка представления коллекции

После создания модели данных измените **ячейку прототипа** ( `SearchResultViewCell.cs` ) и убедитесь, что она выглядит следующим образом:

```csharp
using Foundation;
using System;
using UIKit;

namespace tvText
{
    public partial class SearchResultViewCell : UICollectionViewCell
    {
        #region Private Variables
        private PictureInformation _pictureInfo = null;
        #endregion

        #region Computed Properties
        public PictureInformation PictureInfo {
            get { return _pictureInfo; }
            set {
                _pictureInfo = value;
                UpdateUI ();
            }
        }
        #endregion

        #region Constructors
        public SearchResultViewCell (IntPtr handle) : base (handle)
        {
            // Initialize
            UpdateUI ();
        }
        #endregion

        #region Private Methods
        private void UpdateUI ()
        {
            // Anything to process?
            if (PictureInfo == null) return;

            try {
                Picture.Image = UIImage.FromBundle (PictureInfo.ImageName);
                Picture.AdjustsImageWhenAncestorFocused = true;
                Title.Text = PictureInfo.Title;
                TextColor = UIColor.LightGray;
            } catch {
                // Ignore errors if view isn't fully loaded
            }
        }
        #endregion
    }

}
```

`UpdateUI`Метод будет использоваться для вывода отдельных полей элементов **пиктуреинформатион** ( `PictureInfo` свойство) в ИМЕНОВАННЫХ элементах пользовательского интерфейса каждый раз при обновлении свойства. Например, изображение и заголовок, связанные с изображением.

<a name="The-Collection-View-Controller"></a>

### <a name="the-collection-view-controller"></a>Контроллер представления коллекции

Затем измените контроллер представления коллекции результатов поиска ( `SearchResultsViewController.cs` ) и сделайте так, чтобы он выглядел следующим образом:

```csharp
using Foundation;
using System;
using UIKit;
using System.Collections.Generic;

namespace tvText
{
    public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
    {
        #region Constants
        public const string CellID = "ImageCell";
        #endregion

        #region Private Variables
        private string _searchFilter = "";
        #endregion

        #region Computed Properties
        public List<PictureInformation> AllPictures { get; set;}
        public List<PictureInformation> FoundPictures { get; set; }
        public string SearchFilter {
            get { return _searchFilter; }
            set {
                _searchFilter = value.ToLower();
                FindPictures ();
                CollectionView?.ReloadData ();
            }
        }
        #endregion

        #region Constructors
        public SearchResultsViewController (IntPtr handle) : base (handle)
        {
            // Initialize
            this.AllPictures = new List<PictureInformation> ();
            this.FoundPictures = new List<PictureInformation> ();
            PopulatePictures ();
            FindPictures ();

        }
        #endregion

        #region Private Methods
        private void PopulatePictures ()
        {
            // Clear list
            AllPictures.Clear ();

            // Add images
            AllPictures.Add (new PictureInformation ("Antipasta Platter","Antipasta","cheese,grapes,tomato,coffee,meat,plate"));
            AllPictures.Add (new PictureInformation ("Cheese Plate", "CheesePlate", "cheese,plate,bread"));
            AllPictures.Add (new PictureInformation ("Coffee House", "CoffeeHouse", "coffee,people,menu,restaurant,cafe"));
            AllPictures.Add (new PictureInformation ("Computer and Expresso", "ComputerExpresso", "computer,coffee,expresso,phone,notebook"));
            AllPictures.Add (new PictureInformation ("Hamburger", "Hamburger", "meat,bread,cheese,tomato,pickle,lettus"));
            AllPictures.Add (new PictureInformation ("Lasagna Dinner", "Lasagna", "salad,bread,plate,lasagna,pasta"));
            AllPictures.Add (new PictureInformation ("Expresso Meeting", "PeopleExpresso", "people,bag,phone,expresso,coffee,table,tablet,notebook"));
            AllPictures.Add (new PictureInformation ("Soup and Sandwich", "SoupAndSandwich", "soup,sandwich,bread,meat,plate,tomato,lettus,egg"));
            AllPictures.Add (new PictureInformation ("Morning Coffee", "TabletCoffee", "tablet,person,man,coffee,magazine,table"));
            AllPictures.Add (new PictureInformation ("Evening Coffee", "TabletMagCoffee", "tablet,magazine,coffee,table"));
        }

        private void FindPictures ()
        {
            // Clear list
            FoundPictures.Clear ();

            // Scan each picture for a match
            foreach (PictureInformation picture in AllPictures) {
                if (SearchFilter == "") {
                    // If no search term, everything matches
                    FoundPictures.Add (picture);
                } else if (picture.Title.Contains (SearchFilter) || picture.Keywords.Contains (SearchFilter)) {
                    // If the search term is in the title or keywords, we've found a match
                    FoundPictures.Add (picture);
                }
            }
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView)
        {
            // Only one section in this collection
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section)
        {
            // Return the number of matching pictures
            return FoundPictures.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get a new cell and return it
            var cell = collectionView.DequeueReusableCell (CellID, indexPath);
            return (UICollectionViewCell)cell;
        }

        public override void WillDisplayCell (UICollectionView collectionView, UICollectionViewCell cell, NSIndexPath indexPath)
        {
            // Grab the cell
            var currentCell = cell as SearchResultViewCell;
            if (currentCell == null)
                throw new Exception ("Expected to display a `SearchResultViewCell`.");

            // Display the current picture info in the cell
            var item = FoundPictures [indexPath.Row];
            currentCell.PictureInfo = item;
        }

        public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // If this Search Controller was presented as a modal view, close
            // it before continuing
            // DismissViewController (true, null);

            // Grab the picture being selected and report it
            var picture = FoundPictures [indexPath.Row];
            Console.WriteLine ("Selected: {0}", picture.Title);
        }

        public void UpdateSearchResultsForSearchController (UISearchController searchController)
        {
            // Save the search filter and update the Collection View
            SearchFilter = searchController.SearchBar.Text ?? string.Empty;
        }

        public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            var previousItem = context.PreviouslyFocusedView as SearchResultViewCell;
            if (previousItem != null) {
                UIView.Animate (0.2, () => {
                    previousItem.TextColor = UIColor.LightGray;
                });
            }

            var nextItem = context.NextFocusedView as SearchResultViewCell;
            if (nextItem != null) {
                UIView.Animate (0.2, () => {
                    nextItem.TextColor = UIColor.Black;
                });
            }
        }
        #endregion
    }
}
```

Сначала `IUISearchResultsUpdating` интерфейс добавляется в класс для управления фильтром контроллера поиска, обновляемым пользователем:

```csharp
public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
```

Константа также определяется для указания идентификатора **ячейки прототипа** (которая соответствует идентификатору, определенному в конструкторе интерфейсов выше), который будет использоваться позже, когда контроллер коллекции запрашивает новую ячейку:

```csharp
public const string CellID = "ImageCell";
```

Хранилище создается для полного списка искомых элементов, условия фильтра поиска и списка элементов, соответствующих этому термину:

```csharp
private string _searchFilter = "";
...

public List<PictureInformation> AllPictures { get; set;}
public List<PictureInformation> FoundPictures { get; set; }
public string SearchFilter {
    get { return _searchFilter; }
    set {
        _searchFilter = value.ToLower();
        FindPictures ();
        CollectionView?.ReloadData ();
    }
}
```

При `SearchFilter` изменении списка совпадающих элементов обновляется и содержимое представления коллекции перезагружается. `FindPictures`Подпрограммы отвечают за поиск элементов, соответствующих новому условию поиска:

```csharp
private void FindPictures ()
{
    // Clear list
    FoundPictures.Clear ();

    // Scan each picture for a match
    foreach (PictureInformation picture in AllPictures) {
        if (SearchFilter == "") {
            // If no search term, everything matches
            FoundPictures.Add (picture);
        } else if (picture.Title.Contains (SearchFilter) || picture.Keywords.Contains (SearchFilter)) {
            // If the search term is in the title or keywords, we've found a match
            FoundPictures.Add (picture);
        }
    }
}
```

Значение `SearchFilter` будет Обновлено (что приведет к обновлению представления коллекции результатов), когда пользователь изменит фильтр в контроллере поиска:

```csharp
public void UpdateSearchResultsForSearchController (UISearchController searchController)
{
    // Save the search filter and update the Collection View
    SearchFilter = searchController.SearchBar.Text ?? string.Empty;
}
```

`PopulatePictures`Метод первоначально заполняет коллекцию доступных элементов:

```csharp
private void PopulatePictures ()
{
    // Clear list
    AllPictures.Clear ();

    // Add images
    AllPictures.Add (new PictureInformation ("Antipasta Platter","Antipasta","cheese,grapes,tomato,coffee,meat,plate"));
    ...
}
```

В этом примере все образцы данных создаются в памяти при загрузке контроллера представления коллекции. В реальных приложениях эти данные, вероятно, будут считываться из базы данных или веб-службы и только при необходимости поддерживать ограниченную память Apple TV.

`NumberOfSections`Методы и `GetItemsCount` предоставляют количество сопоставленных элементов:

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    // Only one section in this collection
    return 1;
}

public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    // Return the number of matching pictures
    return FoundPictures.Count;
}
```

`GetCell`Метод возвращает новую **ячейку прототипа** (на основе `CellID` определенной выше в раскадровке) для каждого элемента в представлении коллекции:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Get a new cell and return it
    var cell = collectionView.DequeueReusableCell (CellID, indexPath);
    return (UICollectionViewCell)cell;
}
```

`WillDisplayCell`Метод вызывается до отображения отображаемой ячейки, поэтому ее можно настроить:

```csharp
public override void WillDisplayCell (UICollectionView collectionView, UICollectionViewCell cell, NSIndexPath indexPath)
{
    // Grab the cell
    var currentCell = cell as SearchResultViewCell;
    if (currentCell == null)
        throw new Exception ("Expected to display a `SearchResultViewCell`.");

    // Display the current picture info in the cell
    var item = FoundPictures [indexPath.Row];
    currentCell.PictureInfo = item;
}
```

`DidUpdateFocus`Метод предоставляет пользователю визуальную реакцию на выделение элементов в представлении коллекции результатов:

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    var previousItem = context.PreviouslyFocusedView as SearchResultViewCell;
    if (previousItem != null) {
        UIView.Animate (0.2, () => {
            previousItem.TextColor = UIColor.LightGray;
        });
    }

    var nextItem = context.NextFocusedView as SearchResultViewCell;
    if (nextItem != null) {
        UIView.Animate (0.2, () => {
            nextItem.TextColor = UIColor.Black;
        });
    }
}
```

Наконец, `ItemSelected` метод обрабатывает пользователя, который выбирает элемент (щелкнув сенсорную поверхность с удаленным Siri) в представлении коллекции результатов:

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    // If this Search Controller was presented as a modal view, close
    // it before continuing
    // DismissViewController (true, null);

    // Grab the picture being selected and report it
    var picture = FoundPictures [indexPath.Row];
    Console.WriteLine ("Selected: {0}", picture.Title);
}
```

Если поле поиска было представлено в виде модального диалогового окна (в верхней части представления, вызывающего его), используйте `DismissViewController` метод, чтобы закрыть представление поиска, когда пользователь выберет элемент. В этом примере поле поиска представлено в виде содержимого вкладки представления вкладок, поэтому оно не закрывается здесь.

Дополнительные сведения о представлениях коллекций см. в документации по [работе с представлениями коллекций](~/ios/tvos/user-interface/collection-views.md) .

<a name="Presenting the Search Field"></a>

### <a name="presenting-the-search-field"></a>Представление поля поиска

Существует два основных способа, которым поле поиска (и связанная с ним экранная клавиатура и результаты поиска) могут быть представлены пользователю в tvOS:

- **Модальное диалоговое окно** — поле поиска может быть представлено на основе текущего представления и контроллера представления в виде модального диалогового окна с полноэкранным просмотром. Обычно это делается в ответ на нажатие пользователем кнопки или другого элемента пользовательского интерфейса. Диалоговое окно закрывается, когда пользователь выбирает элемент из результатов поиска.
- **Просмотр содержимого** — поле поиска является прямой частью данного представления. Например, в качестве содержимого вкладки поиска в контроллере представления вкладок.

В качестве примера списка изображений, представленных выше, поле поиска отображается как содержимое представления на вкладке Поиск, а контроллер представления вкладки поиска выглядит следующим образом:

```csharp
using System;
using UIKit;

namespace tvText
{
    public partial class SecondViewController : UIViewController
    {
        #region Constants
        public const string SearchResultsID = "SearchResults";
        #endregion

        #region Computed Properties
        public SearchResultsViewController ResultsController { get; set;}
        #endregion

        #region Constructors
        public SecondViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Private Methods
        public void ShowSearchController ()
        {
            // Build an instance of the Search Results View Controller from the Storyboard
            ResultsController = Storyboard.InstantiateViewController (SearchResultsID) as SearchResultsViewController;
            if (ResultsController == null)
                throw new Exception ("Unable to instantiate a SearchResultsViewController.");

            // Create an initialize a new search controller
            var searchController = new UISearchController (ResultsController) {
                SearchResultsUpdater = ResultsController,
                HidesNavigationBarDuringPresentation = false
            };

            // Set any required search parameters
            searchController.SearchBar.Placeholder = "Enter keyword (e.g. coffee)";

            // The Search Results View Controller can be presented as a modal view
            // PresentViewController (searchController, true, null);

            // Or in the case of this sample, the Search View Controller is being
            // presented as the contents of the Search Tab directly. Use either one
            // or the other method to display the Search Controller (not both).
            var container = new UISearchContainerViewController (searchController);
            var navController = new UINavigationController (container);
            AddChildViewController (navController);
            View.Add (navController.View);
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // If the Search Controller is being displayed as the content
            // of the search tab, include it here.
            ShowSearchController ();
        }

        public override void ViewDidAppear (bool animated)
        {
            base.ViewDidAppear (animated);

            // If the Search Controller is being presented as a modal view,
            // call it here to display it over the contents of the Search
            // tab.
            // ShowSearchController ();
        }
        #endregion
    }
}
```

Во-первых, определяется константа, соответствующая **идентификатору раскадровки** , назначенному контроллеру представления коллекции результатов поиска в конструкторе интерфейсов:

```csharp
public const string SearchResultsID = "SearchResults";
```

Затем `ShowSearchController` метод создает новый контроллер коллекции представления поиска и отображает его.

```csharp
public void ShowSearchController ()
{
    // Build an instance of the Search Results View Controller from the Storyboard
    ResultsController = Storyboard.InstantiateViewController (SearchResultsID) as SearchResultsViewController;
    if (ResultsController == null)
        throw new Exception ("Unable to instantiate a SearchResultsViewController.");

    // Create an initialize a new search controller
    var searchController = new UISearchController (ResultsController) {
        SearchResultsUpdater = ResultsController,
        HidesNavigationBarDuringPresentation = false
    };

    // Set any required search parameters
    searchController.SearchBar.Placeholder = "Enter keyword (e.g. coffee)";

    // The Search Results View Controller can be presented as a modal view
    // PresentViewController (searchController, true, null);

    // Or in the case of this sample, the Search View Controller is being
    // presented as the contents of the Search Tab directly. Use either one
    // or the other method to display the Search Controller (not both).
    var container = new UISearchContainerViewController (searchController);
    var navController = new UINavigationController (container);
    AddChildViewController (navController);
    View.Add (navController.View);
}
```

В приведенном выше методе после `SearchResultsViewController` создания экземпляра из раскадровки создается новый объект, `UISearchController` который позволяет предоставить пользователю поле поиска и экранную клавиатуру. На этой клавиатуре будет отображаться Коллекция результатов поиска (как определено в `SearchResultsViewController` ).

Далее `SearchBar` настраивается со сведениями, такими как указание **заполнителя** . Это предоставляет пользователю сведения о типе поиска.

Затем пользователю будет предоставлено поле поиска одним из двух способов:

- **Модальное представление диалогового окна** — `PresentViewController` метод вызывается для представления поиска по существующему представлению в полноэкранном режиме.
- **Представление содержимого** — `UISearchContainerViewController` создается для размещения контроллера поиска. `UINavigationController`Создается для хранения контейнера поиска, затем контроллер навигации добавляется к контроллеру представления `AddChildViewController (navController)` и представленному представлению `View.Add (navController.View)` .

Наконец, и снова, в зависимости от типа представления, `ViewDidLoad` метод или `ViewDidAppear` вызывает `ShowSearchController` метод, чтобы предоставить пользователю поиск.

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // If the Search Controller is being displayed as the content
    // of the search tab, include it here.
    ShowSearchController ();
}

public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);

    // If the Search Controller is being presented as a modal view,
    // call it here to display it over the contents of the Search
    // tab.
    // ShowSearchController ();
}
```

При запуске приложения и выборе пользователем вкладки поиска полный список неотфильтрованных элементов будет представлен пользователю:

[![Результаты поиска по умолчанию](text-fields-and-search-images/intro02.png)](text-fields-and-search-images/intro02.png#lightbox)

По мере того, как пользователь начинает вводить условие поиска, список результатов будет отфильтрован по этому термину и автоматически обновлен:

[![Отфильтрованные результаты поиска](text-fields-and-search-images/intro03.png)](text-fields-and-search-images/intro03.png#lightbox)

В любой момент времени пользователь может переключить фокус на элемент в результатах поиска и щелкнуть сенсорную поверхность Siri удаленно, чтобы выбрать ее.

<a name="Summary"></a>

## <a name="summary"></a>Сводка

Эта статья посвящена проектированию и работе с текстовыми полями и поиском в приложении Xamarin. tvOS. В нем было показано, как создать текст и найти содержимое коллекции в конструкторе интерфейсов, а также два разных способа представления поля поиска пользователю в tvOS.

## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [Руководства по tvOSму интерфейсу](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Руководством по программированию приложений для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
