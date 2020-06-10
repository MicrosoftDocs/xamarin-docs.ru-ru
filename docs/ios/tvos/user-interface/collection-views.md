---
title: Работа с представлениями коллекций tvOS в Xamarin
description: В этом документе описывается работа с представлениями коллекций в приложении tvOS, созданном с помощью Xamarin. В нем рассматриваются макеты представлений коллекций, создание ячеек и дополнительные представления, реагирование на события пользователя и многое другое.
ms.prod: xamarin
ms.assetid: 5125C4C7-2DDF-4C19-A362-17BB2B079178
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: aa03ab7a3663fa5e0704a605116b19147f14a10b
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84572589"
---
# <a name="working-with-tvos-collection-views-in-xamarin"></a>Работа с представлениями коллекций tvOS в Xamarin

Представления коллекций позволяют отображать группу содержимого с помощью произвольных макетов. Используя встроенную поддержку, они позволяют легко создавать линейные макеты, схожие с сеткой, а также поддерживают пользовательские макеты.

[![](collection-views-images/collection01.png "Sample collection view")](collection-views-images/collection01.png#lightbox)

Представление коллекции поддерживает коллекцию элементов, использующих как делегат, так и источник данных для обеспечения взаимодействия с пользователем и содержимого коллекции. Поскольку представление коллекции основано на подсистеме макета, которая не зависит от самого представления, предоставление другого макета может легко изменить представление данных представления коллекции на лету.

<a name="About-Collection-Views"></a>

## <a name="about-collection-views"></a>О представлениях коллекций

Как упоминалось выше, представление коллекции ( `UICollectionView` ) управляет упорядоченной коллекцией элементов и представляет эти элементы настраиваемыми макетами. Представления коллекций работают так же, как табличные представления ( `UITableView` ), за исключением того, что они могут использовать макеты для представления элементов не только в одном столбце.

При использовании представления коллекции в tvOS приложение несет ответственность за предоставление данных, связанных с коллекцией, с помощью источника данных ( `UICollectionViewDataSource` ). Данные представления коллекции могут быть упорядочены и представлены в разных группах (разделах).

Представление коллекции представляет отдельные элементы на экране с помощью ячейки ( `UICollectionViewCell` ), предоставляющей представление определенного фрагмента информации из коллекции (например, изображение и его название).

При необходимости в представление коллекции можно добавить дополнительные представления, которые будут использоваться в качестве верхнего и нижнего колонтитулов для разделов и ячеек. Макет представления коллекции отвечает за определение размещения этих представлений вместе с отдельными ячейками.

Представление коллекции может реагировать на взаимодействие с пользователем с помощью делегата ( `UICollectionViewDelegate` ). Этот делегат также отвечает за определение того, может ли данная ячейка получить фокус, если ячейка была выделена или если она была выбрана. В некоторых случаях делегат определяет размер отдельных ячеек.

<a name="Collection-View-Layouts"></a>

## <a name="collection-view-layouts"></a>Макеты представлений коллекций

Ключевой функцией представления коллекции является его разделение между данными, представленными для представления, и его макетом. Макет представления коллекции ( `UICollectionViewLayout` ) отвечает за предоставление Организации и расположения ячеек (и любых вспомогательных представлений) в презентации представления коллекции.

Отдельные ячейки создаются представлением коллекции из присоединенного источника данных, а затем упорядочиваются и отображаются в заданном макете представления коллекции.

Макет представления коллекции обычно предоставляется при создании представления коллекции. Однако макет представления коллекции можно изменить в любое время, и представление данных представления коллекции будет автоматически обновляться с использованием нового макета.

Макет представления коллекции предоставляет несколько методов, которые можно использовать для анимации перехода между двумя различными макетами (по умолчанию анимация не выполняется). Кроме того, макеты представлений коллекций могут работать с распознавателями жестов для дальнейшей анимации взаимодействия с пользователем, что приводит к изменению макета.

<a name="Creating-Cells-and-Supplementary-Views"></a>

## <a name="creating-cells-and-supplementary-views"></a>Создание ячеек и дополнительных представлений

Источник данных представления коллекции не только отвечает за предоставление данных для элемента коллекции, но также ячейки, используемые для отображения содержимого.

Поскольку представления коллекций создавались для работы с большими коллекциями элементов, отдельные ячейки могут быть выведены из очереди и повторно использованы для предотвращения ограничений памяти. Существует два разных метода вывода представлений из очереди:

- `DequeueReusableCell`— Создает или возвращает ячейку заданного типа (как указано в раскадровке приложения).
- `DequeueReusableSupplementaryView`— Создает или возвращает вспомогательное представление заданного типа (как указано в раскадровке приложения).

Перед вызовом любого из этих методов необходимо зарегистрировать класс, раскадровку или `.xib` файл, используемый для создания представления ячейки с представлением коллекции. Пример.

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    ...
}
```

Где `typeof(CityCollectionViewCell)` предоставляет класс, который поддерживает представление и `CityViewDatasource.CardCellId` предоставляет идентификатор, используемый, когда ячейка (или представление) выводится из очереди.

После того как ячейка выводится из очереди, она настраивается данными для элемента, который она представляет, и возвращается в представление коллекции для отображения.

<a name="About-Collection-View-Controllers"></a>

## <a name="about-collection-view-controllers"></a>Сведения об контроллерах представления коллекций

Контроллер представления коллекции ( `UICollectionViewController` ) — это специализированный контроллер представления ( `UIViewController` ), обеспечивающий следующее поведение.

- Он отвечает за загрузку представления коллекции из раскадровки или `.xib` файла и создание экземпляра представления. При создании в коде автоматически создается новое ненастроенное представление коллекции.
- После загрузки представления коллекции контроллер пытается загрузить его источник данных и делегат из раскадровки или `.xib` файла. Если они отсутствуют, он задается как источник обоих.
- Гарантирует, что данные загружаются перед заполнением представления коллекции при первом отображении, а затем перезагружаются и очищаются при каждом последующем отображении.

Кроме того, контроллер представления коллекции предоставляет переопределяемые методы, которые можно использовать для управления жизненным циклом представления коллекции, например `AwakeFromNib` и `ViewWillDisplay` .

<a name="Collection-Views-and-Storyboards"></a>

## <a name="collection-views-and-storyboards"></a>Представления коллекций и раскадровки

Самый простой способ работы с представлением коллекции в приложении Xamarin. tvOS — добавить его к раскадровке. В качестве краткого примера мы создадим пример приложения, который представляет собой изображение, заголовок и кнопку выбора. Если пользователь наберет кнопку Выбрать, отобразится представление коллекции, позволяющее пользователю выбрать новое изображение. Если выбрано изображение, представление коллекции закрывается, и отображается новое изображение и заголовок.

Давайте выполним следующие действия.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Запустите новое **приложение tvOS с одним представлением** в Visual Studio для Mac.
1. В **Обозреватель решений**дважды щелкните `Main.storyboard` файл и откройте его в конструкторе iOS.
1. Добавьте представление изображения, метку и кнопку в существующее представление и настройте их так, чтобы они выглядели следующим образом: 

    [![](collection-views-images/collection02.png "Sample layout")](collection-views-images/collection02.png#lightbox)
1. Присвойте **имя** представлению изображения и метке на **вкладке мини** -приложение **обозревателя свойств**. Пример. 

    [![](collection-views-images/collection03.png "Setting the name")](collection-views-images/collection03.png#lightbox)
1. Затем перетащите контроллер представления коллекции на раскадровку: 

    [![](collection-views-images/collection04.png "A Collection View Controller")](collection-views-images/collection04.png#lightbox)
1. Перетащите элемент управления из кнопки в контроллер представления коллекции и выберите команду **Отправить** из всплывающего окна: 

    [![](collection-views-images/collection05.png "Select Push from the popup")](collection-views-images/collection05.png#lightbox)
1. При запуске приложения это представление коллекции будет отображаться каждый раз, когда пользователь нажмет кнопку.
1. Выберите представление коллекции и введите следующие значения на **вкладке Макет** **обозревателя свойств**. 

    [![](collection-views-images/collection06.png "The Properties Explorer")](collection-views-images/collection06.png#lightbox)
1. Определяет размер отдельных ячеек и границ между ячейками и внешним ребром представления коллекции.
1. Выберите контроллер представления коллекции и задайте для его класса значение `CityCollectionViewController` на **вкладке мини**-приложение: 

    [![](collection-views-images/collection07.png "Set the class to CityCollectionViewController")](collection-views-images/collection07.png#lightbox)
1. Выберите представление коллекции и задайте для его класса значение `CityCollectionView` на **вкладке мини**-приложение: 

    [![](collection-views-images/collection08.png "Set the class to CityCollectionView")](collection-views-images/collection08.png#lightbox)
1. Выберите ячейку представление коллекции и задайте для ее класса значение `CityCollectionViewCell` на **вкладке мини**-приложение: 

    [![](collection-views-images/collection09.png "Set the class to CityCollectionViewCell")](collection-views-images/collection09.png#lightbox)
1. На **вкладке мини** -приложение убедитесь, что **Макет** имеет значение, `Flow` а **направление прокрутки** — `Vertical` для представления коллекция: 

    [![](collection-views-images/collection10.png "The Widget Tab")](collection-views-images/collection10.png#lightbox)
1. Выберите ячейку представление коллекции и задайте ее **удостоверение** на `CityCell` **вкладке мини**-приложение: 

    [![](collection-views-images/collection11.png "Set the Identity to CityCell")](collection-views-images/collection11.png#lightbox)
1. Сохраните изменения.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Запустите новое **приложение с одним представлением tvOS** в Visual Studio.
1. В **Обозреватель решений**дважды щелкните `Main.storyboard` файл и откройте его в конструкторе iOS.
1. Добавьте представление изображения, метку и кнопку в существующее представление и настройте их так, чтобы они выглядели следующим образом: 

    [![](collection-views-images/collection02vs.png "Configure the layout")](collection-views-images/collection02vs.png#lightbox)
1. Присвойте **имя** представлению изображения и метке на **вкладке мини** -приложение **обозревателя свойств**. Пример. 

    [![](collection-views-images/collection03vs.png "The Properties Explorer")](collection-views-images/collection03vs.png#lightbox)
1. Затем перетащите контроллер представления коллекции на раскадровку: 

    [![](collection-views-images/collection04vs.png "A Collection View Controller")](collection-views-images/collection04vs.png#lightbox)
1. Перетащите элемент управления из кнопки в контроллер представления коллекции и выберите команду **Отправить** из всплывающего окна: 

    [![](collection-views-images/collection05vs.png "Select Push from the popup")](collection-views-images/collection05vs.png#lightbox)
1. При запуске приложения это представление коллекции будет отображаться каждый раз, когда пользователь нажмет кнопку.
1. Выберите представление коллекции и на **вкладке Макет** **обозревателя свойств** введите **ширину** _361_ и **высоту** _256_ . 
1. Определяет размер отдельных ячеек и границ между ячейками и внешним ребром представления коллекции.
1. Выберите контроллер представления коллекции и задайте для его класса значение `CityCollectionViewController` на **вкладке мини**-приложение: 

    [![](collection-views-images/collection07vs.png "Set the class to CityCollectionViewController")](collection-views-images/collection07vs.png#lightbox)
1. Выберите представление коллекции и задайте для его класса значение `CityCollectionView` на **вкладке мини**-приложение: 

    [![](collection-views-images/collection08vs.png "Set the class to CityCollectionView")](collection-views-images/collection08vs.png#lightbox)
1. Выберите ячейку представление коллекции и задайте для ее класса значение `CityCollectionViewCell` на **вкладке мини**-приложение: 

    [![](collection-views-images/collection09vs.png "Set the class to CityCollectionViewCell")](collection-views-images/collection09vs.png#lightbox)
1. На **вкладке мини** -приложение убедитесь, что **Макет** имеет значение, `Flow` а **направление прокрутки** — `Vertical` для представления коллекция: 

    [![](collection-views-images/collection10vs.png "Tthe Widget Tab")](collection-views-images/collection10vs.png#lightbox)
1. Выберите ячейку представление коллекции и задайте ее **удостоверение** на `CityCell` **вкладке мини**-приложение: 

    [![](collection-views-images/collection11vs.png "Set the Identity to CityCell")](collection-views-images/collection11vs.png#lightbox)
1. Сохраните изменения.

-----

Если бы мы выбрали `Custom` **Макет**представления коллекции, можно было бы указать пользовательский макет. Компания Apple предоставляет встроенные `UICollectionViewFlowLayout` `UICollectionViewDelegateFlowLayout` возможности, которые могут легко представлять данные в макете на основе сетки (они используются в `flow` стиле макета). 

Дополнительные сведения о работе с раскадровками см. в статье [Hello, tvOS краткое руководство по началу работы](~/ios/tvos/get-started/hello-tvos.md).

<a name="Providing-Data-for-the-Collection-View"></a>

## <a name="providing-data-for-the-collection-view"></a>Предоставление данных для представления коллекции

Теперь, когда мы добавили в раскадровку представление коллекции (и контроллер представления коллекции), необходимо предоставить данные для коллекции. 

<a name="The-Data-Model"></a>

### <a name="the-data-model"></a>Модель данных

Во-первых, мы создадим модель для наших данных, содержащую имя файла отображаемого изображения, заголовок и флаг, позволяющий выбрать город.

Создайте `CityInfo` класс и сделайте его следующим:

```csharp
using System;

namespace tvCollection
{
    public class CityInfo
    {
        #region Computed Properties
        public string ImageFilename { get; set; }
        public string Title { get; set; }
        public bool CanSelect{ get; set; }
        #endregion

        #region Constructors
        public CityInfo (string filename, string title, bool canSelect)
        {
            // Initialize
            this.ImageFilename = filename;
            this.Title = title;
            this.CanSelect = canSelect;
        }
        #endregion
    }
}
```

### <a name="the-collection-view-cell"></a>Ячейка представления коллекции

Теперь необходимо определить, как будут представлены данные для каждой ячейки. Измените `CityCollectionViewCell.cs` файл (созданный автоматически из файла раскадровки) и сделайте его следующим:

```csharp
using System;
using Foundation;
using UIKit;
using CoreGraphics;

namespace tvCollection
{
    public partial class CityCollectionViewCell : UICollectionViewCell
    {
        #region Private Variables
        private CityInfo _city;
        #endregion

        #region Computed Properties
        public UIImageView CityView { get ; set; }
        public UILabel CityTitle { get; set; }

        public CityInfo City {
            get { return _city; }
            set {
                _city = value;
                CityView.Image = UIImage.FromFile (City.ImageFilename);
                CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
                CityTitle.Text = City.Title;
            }
        }
        #endregion

        #region Constructors
        public CityCollectionViewCell (IntPtr handle) : base (handle)
        {
            // Initialize
            CityView = new UIImageView(new CGRect(22, 19, 320, 171));
            CityView.AdjustsImageWhenAncestorFocused = true;
            AddSubview (CityView);

            CityTitle = new UILabel (new CGRect (22, 209, 320, 21)) {
                TextAlignment = UITextAlignment.Center,
                TextColor = UIColor.White,
                Alpha = 0.0f
            };
            AddSubview (CityTitle);
        }
        #endregion
    

    }
}
```

Для нашего приложения tvOS будет отображаться изображение и необязательное название. Если заданный город не может быть выбран, представление изображения будет затемнено с помощью следующего кода:

```csharp
CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
```

Если ячейка, содержащая изображение, становится фокусом пользователя, мы хотим использовать для него встроенный фокусировки, задавая следующее свойство:

```csharp
CityView.AdjustsImageWhenAncestorFocused = true;
```

Дополнительные сведения о навигации и фокусе см. в документации по [работе с навигацией и фокусами](~/ios/tvos/app-fundamentals/navigation-focus.md) и [Siri удаленными устройствами и контроллерами Bluetooth](~/ios/tvos/platform/remote-bluetooth.md) .

<a name="The-Collection-View-Data-Provider"></a>

### <a name="the-collection-view-data-provider"></a>Поставщик данных представления коллекции

После создания нашей модели данных и определения структуры ячеек давайте создадим источник данных для нашего представления коллекции. Источник данных несет ответственность за не только предоставление резервных данных, но также вывод в очередь ячеек для отображения отдельных ячеек на экране.

Создайте `CityViewDatasource` класс и сделайте его следующим:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using ObjCRuntime;

namespace tvCollection
{
    public class CityViewDatasource : UICollectionViewDataSource
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Static Constants
        public static NSString CardCellId = new NSString ("CityCell");
        #endregion

        #region Computed Properties
        public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
        public CityCollectionView ViewController { get; set; }
        #endregion

        #region Constructors
        public CityViewDatasource (CityCollectionView controller)
        {
            // Initialize
            this.ViewController = controller;
            PopulateCities ();
        }
        #endregion

        #region Public Methods
        public void PopulateCities() {

            // Clear existing cities
            Cities.Clear();

            // Add new cities
            Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
            Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
            Cities.Add(new CityInfo("City03.jpg", "Skyline at Night", true));
            Cities.Add(new CityInfo("City04.jpg", "Golden Gate Bridge", true));
            Cities.Add(new CityInfo("City05.jpg", "Roads by Night", true));
            Cities.Add(new CityInfo("City06.jpg", "Church Domes", true));
            Cities.Add(new CityInfo("City07.jpg", "Mountain Lights", true));
            Cities.Add(new CityInfo("City08.jpg", "City Scene", false));
            Cities.Add(new CityInfo("City09.jpg", "House in Winter", true));
            Cities.Add(new CityInfo("City10.jpg", "By the Lake", true));
            Cities.Add(new CityInfo("City11.jpg", "At the Dome", true));
            Cities.Add(new CityInfo("City12.jpg", "Cityscape", true));
            Cities.Add(new CityInfo("City13.jpg", "Model City", true));
            Cities.Add(new CityInfo("City14.jpg", "Taxi, Taxi!", true));
            Cities.Add(new CityInfo("City15.jpg", "On the Sidewalk", true));
            Cities.Add(new CityInfo("City16.jpg", "Midnight Walk", true));
            Cities.Add(new CityInfo("City17.jpg", "Lunchtime Cafe", true));
            Cities.Add(new CityInfo("City18.jpg", "Coffee Shop", true));
            Cities.Add(new CityInfo("City19.jpg", "Rustic Tavern", true));
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView)
        {
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section)
        {
            return Cities.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
            var city = Cities [indexPath.Row];

            // Initialize city
            cityCell.City = city;

            return cityCell;
        }
        #endregion
    }
}
```

Подробно рассмотрим этот класс. Сначала наследуется от `UICollectionViewDataSource` и указать ярлык для идентификатора ячейки (который мы присвоили в конструкторе iOS):

```csharp
public static NSString CardCellId = new NSString ("CityCell");
```

Далее мы предоставляем хранилище для данных коллекции и предоставляем класс для заполнения данных:

```csharp
public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
...

public void PopulateCities() {

    // Clear existing cities
    Cities.Clear();

    // Add new cities
    Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
    Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
    ...
}
```

Затем мы переопределяем `NumberOfSections` метод и возвращаем количество разделов (групп элементов), которое имеет представление коллекции. В этом случае существует только один:

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    return 1;
}
```

Затем мы возвращаем количество элементов в коллекции, используя следующий код:

```csharp
public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    return Cities.Count;
}
```

Наконец, мы выносим в очередь повторно используемую ячейку при запросе представления коллекции со следующим кодом:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
    var city = Cities [indexPath.Row];

    // Initialize city
    cityCell.City = city;

    return cityCell;
}
```

После получения ячейки представления коллекции нашего `CityCollectionViewCell` типа мы заполняем ее данным элементом.

<a name="Responding-to-User-Events"></a>

## <a name="responding-to-user-events"></a>Реагирование на события пользователя

Так как мы хотим, чтобы пользователь мог выбрать элемент из нашей коллекции, необходимо предоставить делегат представления коллекции для работы с этим взаимодействием. И нам нужно предоставить способ, позволяющий нашему вызывающему представлению знать, какой элемент выбран пользователем.

<a name="The-App-Delegate"></a>

### <a name="the-app-delegate"></a>Делегат приложения

Нам нужен способ связать выбранный в данный момент элемент из представления коллекции обратно в вызывающее представление. Мы будем использовать пользовательское свойство на нашем `AppDelegate` . Измените `AppDelegate.cs` файл и добавьте следующий код:

```csharp
public CityInfo SelectedCity { get; set;} = new CityInfo("City02.jpg", "Turning Circle", true);
```

Это определяет свойство и задает город по умолчанию, который изначально будет отображаться. Позже мы будем использовать это свойство, чтобы отобразить Выбор пользователя и разрешить изменение объекта Select.

<a name="The-Collection-View-Delegate"></a>

### <a name="the-collection-view-delegate"></a>Делегат представления коллекции

Затем добавьте `CityViewDelegate` в проект новый класс и сделайте его следующим:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;

namespace tvCollection
{
    public class CityViewDelegate : UICollectionViewDelegateFlowLayout
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        public CityViewDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
        {
            return new CGSize (361, 256);
        }

        public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
        {
            if (indexPath == null) {
                return false;
            } else {
                var controller = collectionView as CityCollectionView;
                return controller.Source.Cities[indexPath.Row].CanSelect;
            }
        }

        public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var controller = collectionView as CityCollectionView;
            App.SelectedCity = controller.Source.Cities [indexPath.Row];

            // Close Collection
            controller.ParentController.DismissViewController(true,null);
        }
        #endregion
    }
}
```

Давайте подробнее рассмотрим этот класс. Сначала наследуется от `UICollectionViewDelegateFlowLayout` . Причина, по которой мы наследуем от этого класса, `UICollectionViewDelegate` заключается в том, что мы используем встроенную `UICollectionViewFlowLayout` для представления наших элементов, а не пользовательский тип макета.

Далее мы возвращаем размер отдельных элементов, используя следующий код:

```csharp
public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
{
    return new CGSize (361, 256);
}
```

Затем мы решили, может ли данная ячейка получить фокус, используя следующий код: 

```csharp
public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
{
    if (indexPath == null) {
        return false;
    } else {
        var controller = collectionView as CityCollectionView;
        return controller.Source.Cities[indexPath.Row].CanSelect;
    }
}
```

Мы проверяем, имеет ли установленный флаг для определенного фрагмента резервных данных `CanSelect` `true` значение и возвращают его. Дополнительные сведения о навигации и фокусе см. в документации по [работе с навигацией и фокусами](~/ios/tvos/app-fundamentals/navigation-focus.md) и [Siri удаленными устройствами и контроллерами Bluetooth](~/ios/tvos/platform/remote-bluetooth.md) .

Наконец, мы реагируем на пользователя, который выбирает элемент с помощью следующего кода:

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    var controller = collectionView as CityCollectionView;
    App.SelectedCity = controller.Source.Cities [indexPath.Row];

    // Close Collection
    controller.ParentController.DismissViewController(true,null);
}
```

Здесь мы устанавливаем `SelectedCity` свойство `AppDelegate` для элемента, который выбрал пользователь, и закрыл контроллер представления коллекции, вернувшись к представлению, вызвавшему US. Мы еще не определили `ParentController` свойство нашего представления коллекции, мы сделаем это дальше.

<a name="Configuring-the-Collection-View"></a>

## <a name="configuring-the-collection-view"></a>Настройка представления коллекции

Теперь нам нужно изменить представление коллекции и назначить наш источник данных и делегат. Измените `CityCollectionView.cs` файл (созданный для нас автоматически из раскадровки) и сделайте его следующим:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionView : UICollectionView
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public CityViewDatasource Source {
            get { return DataSource as CityViewDatasource;}
        }

        public CityCollectionViewController ParentController { get; set;}
        #endregion

        #region Constructors
        public CityCollectionView (IntPtr handle) : base (handle)
        {
            // Initialize
            RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
            DataSource = new CityViewDatasource (this);
            Delegate = new CityViewDelegate ();
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections ()
        {
            return 1;
        }

        public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
            if (previousItem != null) {
                Animate (0.2, () => {
                    previousItem.CityTitle.Alpha = 0.0f;
                });
            }

            var nextItem = context.NextFocusedView as CityCollectionViewCell;
            if (nextItem != null) {
                Animate (0.2, () => {
                    nextItem.CityTitle.Alpha = 1.0f;
                });
            }
        }
        #endregion
    }
}
```

Сначала мы предоставляем ярлык для доступа к нашему `AppDelegate` : 

```csharp
public static AppDelegate App {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
```

Далее мы предоставляем ярлык для источника данных представления коллекции и свойство для доступа к контроллеру представления коллекции (используется нашим делегатом выше, чтобы закрыть коллекцию, когда пользователь сделает выбор):

```csharp
public CityViewDatasource Source {
    get { return DataSource as CityViewDatasource;}
}

public CityCollectionViewController ParentController { get; set;}
```

Затем мы используем следующий код, чтобы инициализировать представление коллекции и назначить наш класс Cell, источник данных и делегат:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    DataSource = new CityViewDatasource (this);
    Delegate = new CityViewDelegate ();
}
```

Наконец, мы хотим, чтобы заголовок под изображением отображался только тогда, когда пользователь выделил его (в фокусе). Мы делаем это с помощью следующего кода:

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
    if (previousItem != null) {
        Animate (0.2, () => {
            previousItem.CityTitle.Alpha = 0.0f;
        });
    }

    var nextItem = context.NextFocusedView as CityCollectionViewCell;
    if (nextItem != null) {
        Animate (0.2, () => {
            nextItem.CityTitle.Alpha = 1.0f;
        });
    }
}
```

Мы устанавливаем прозрачность предыдущего элемента, теряя фокус на ноль (0), и прозрачность следующего элемента получит фокус на 100%. Эти переходы также походят из анимации.

## <a name="configuring-the-collection-view-controller"></a>Настройка контроллера представления коллекции

Теперь необходимо выполнить окончательную настройку в нашем представлении коллекции и позволить контроллеру установить свойство, которое мы определили, чтобы это представление коллекции можно было закрыть после того, как пользователь сделает выбор.

Измените `CityCollectionViewController.cs` файл (созданный автоматически из нашей раскадровки) и сделайте его следующим:

```csharp
// This file has been autogenerated from a class added in the UI designer.

using System;

using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionViewController : UICollectionViewController
    {
        #region Computed Properties
        public CityCollectionView Collection {
            get { return CollectionView as CityCollectionView; }
        }
        #endregion

        #region Constructors
        public CityCollectionViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Save link to controller
            Collection.ParentController = this;
        }
        #endregion
    }
}

```

## <a name="putting-it-all-together"></a>Совместное размещение 

Теперь, когда все части объединены для заполнения и управления представлением коллекции, необходимо внести окончательные изменения в главное представление, чтобы объединить все вместе.

Измените `ViewController.cs` файл (созданный автоматически из нашей раскадровки) и сделайте его следующим:

```csharp
using System;
using Foundation;
using UIKit;
using tvCollection;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
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
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update image with the currently selected one
            CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
            BackgroundView.Image = CityView.Image;
            CityTitle.Text = App.SelectedCity.Title;
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

Следующий код изначально отображает выбранный элемент из `SelectedCity` Свойства и переводит `AppDelegate` его, когда пользователь сделал выбор из представления коллекции:

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update image with the currently selected one
    CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
    BackgroundView.Image = CityView.Image;
    CityTitle.Text = App.SelectedCity.Title;
}
```

<a name="Testing-the-app"></a>

## <a name="testing-the-app"></a>Проверка приложения

При наличии всех действий при сборке и запуске приложения основное представление отображается со значением города по умолчанию:

[![](collection-views-images/run01.png "The main screen")](collection-views-images/run01.png#lightbox)

Если пользователь **наберет кнопку "выбрать представление** ", отобразится представление коллекции:

[![](collection-views-images/run02.png "The collection view")](collection-views-images/run02.png#lightbox)

Любой город, свойство которого имеет `CanSelect` значение, `false` будет отображаться серым цветом, и пользователь не сможет установить фокус на него. Когда пользователь выделяет элемент (сделать его активным), заголовок отображается и может использовать эффект фокусировки для тонкого наклона изображения в трехмерном виде.

Когда пользователь щелкает выбранное изображение, представление коллекции закрывается, а главное представление снова отображается с новым изображением:

[![](collection-views-images/run03.png "A new image on the home screen")](collection-views-images/run03.png#lightbox)

<a name="Creating-Custom-Layout-and-Reordering-Items"></a>

## <a name="creating-custom-layout-and-reordering-items"></a>Создание пользовательского макета и переупорядочения элементов

Одной из основных функций использования представления коллекций является возможность создавать пользовательские макеты. Поскольку tvOS наследует от iOS, процесс создания пользовательского макета такой же. Дополнительные сведения см. в документации по [представлениям коллекций](~/ios/user-interface/controls/uicollectionview.md) .

Недавно добавленные в представления коллекций для iOS 9 — это возможность легко разрешить изменение порядка элементов в коллекции. Опять же, поскольку tvOS 9 является подмножеством iOS 9, это выполняется так же. Дополнительные сведения см. в нашем документе об [изменениях в представлении коллекции](~/ios/user-interface/controls/uicollectionview.md) .

<a name="Summary"></a>

## <a name="summary"></a>Сводка

В этой статье рассматривается проектирование и работа с представлениями коллекций в приложении Xamarin. tvOS. Во первых, он рассматривал все элементы, составляющие представление коллекции. Далее показано, как спроектировать и реализовать представление коллекции с помощью раскадровки. Наконец, предоставляются ссылки на сведения о создании пользовательских макетов и переупорядочении элементов.

## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [Руководства по tvOSму интерфейсу](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Руководством по программированию приложений для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
