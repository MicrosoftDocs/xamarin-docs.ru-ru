---
title: Представления коллекций в Xamarin. iOS
description: Представления коллекций позволяют отображать содержимое с помощью произвольных макетов. Они позволяют легко создавать макеты, основанные на сетке, а также поддерживать пользовательские макеты.
ms.prod: xamarin
ms.assetid: F4B85F25-0CB5-4FEA-A3B5-D22FCDC81AE4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: eccd0415fdc1db357f904b843a015df60a35a488
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939611"
---
# <a name="collection-views-in-xamarinios"></a>Представления коллекций в Xamarin. iOS

_Представления коллекций позволяют отображать содержимое с помощью произвольных макетов. Они позволяют легко создавать макеты, основанные на сетке, а также поддерживать пользовательские макеты._

Представления коллекций, доступные в `UICollectionView` классе, являются новой концепцией в iOS 6, что приводит к показу нескольких элементов на экране с помощью макетов. Шаблоны для предоставления данных для `UICollectionView` создания элементов и взаимодействия с этими элементами следуют тем же моделям делегирования и источников данных, которые обычно используются в разработке iOS.

Однако представления коллекций работают с подсистемой макета, которая не зависит от `UICollectionView` самого себя. Таким образом, простое предоставление другого макета позволяет легко изменять представление коллекции.

iOS предоставляет класс макета с именем `UICollectionViewFlowLayout` , который позволяет создавать макеты на основе строк, такие как сетка, без дополнительной работы. Кроме того, можно создать пользовательские макеты, позволяющие представить любую презентацию.

## <a name="uicollectionview-basics"></a>Основы Уиколлектионвиев

`UICollectionView`Класс состоит из трех различных элементов:

- **Ячейки** — представления, управляемые данными для каждого элемента
- **Дополнительные представления** — управляемые данными представления, связанные с разделом.
- **Представления декорирования** — представления, не управляемые данными, созданные макетом

## <a name="cells"></a>Ячейки

Ячейки — это объекты, представляющие один элемент в наборе данных, представленном представлением коллекции. Каждая ячейка является экземпляром `UICollectionViewCell` класса, который состоит из трех различных представлений, как показано на рисунке ниже.

 [![Каждая ячейка состоит из трех различных представлений, как показано ниже.](uicollectionview-images/01-uicollectionviewcell.png)](uicollectionview-images/01-uicollectionviewcell.png#lightbox)

`UICollectionViewCell`Класс имеет следующие свойства для каждого из этих представлений:

- `ContentView`— Это представление содержит содержимое, которое представляет ячейка. Он отображается в самом верхнем z-порядке на экране.
- `SelectedBackgroundView`— Ячейки имеют встроенную поддержку выбора. Это представление позволяет визуально отметьте, что ячейка выбрана. Он отображается непосредственно под тем, `ContentView` когда выбрана ячейка.
- `BackgroundView`— Ячейки могут также отображать фон, который представлен в `BackgroundView` . Это представление отображается под `SelectedBackgroundView` .

Устанавливая `ContentView` таким образом, что он меньше `BackgroundView` и `SelectedBackgroundView` , `BackgroundView` можно использовать для визуального выделения содержимого, тогда `SelectedBackgroundView` как при выборе ячейки, как показано ниже:

 [![Различные элементы ячейки](uicollectionview-images/02-cells.png)](uicollectionview-images/02-cells.png#lightbox)

Ячейки на снимке экрана выше создаются путем наследования из `UICollectionViewCell` и установки `ContentView` `SelectedBackgroundView` свойств, и `BackgroundView` соответственно, как показано в следующем коде:

```csharp
public class AnimalCell : UICollectionViewCell
{
        UIImageView imageView;

        [Export ("initWithFrame:")]
        public AnimalCell (CGRect frame) : base (frame)
        {
            BackgroundView = new UIView{BackgroundColor = UIColor.Orange};

            SelectedBackgroundView = new UIView{BackgroundColor = UIColor.Green};

            ContentView.Layer.BorderColor = UIColor.LightGray.CGColor;
            ContentView.Layer.BorderWidth = 2.0f;
            ContentView.BackgroundColor = UIColor.White;
            ContentView.Transform = CGAffineTransform.MakeScale (0.8f, 0.8f);

            imageView = new UIImageView (UIImage.FromBundle ("placeholder.png"));
            imageView.Center = ContentView.Center;
            imageView.Transform = CGAffineTransform.MakeScale (0.7f, 0.7f);

            ContentView.AddSubview (imageView);
        }

        public UIImage Image {
            set {
                imageView.Image = value;
            }
        }
}
```

 <a name="Supplementary_Views"></a>

## <a name="supplementary-views"></a>Дополнительные представления

Дополнительные представления — это представления, которые представляют сведения, связанные с каждым разделом `UICollectionView` . Как и ячейки, дополнительные представления управляются данными. Если ячейки представляют данные элемента из источника данных, дополнительные представления представляют данные раздела, такие как категории книги на полке или жанр музыки в музыкальной библиотеке.

Например, можно использовать вспомогательное представление для представления заголовка для определенного раздела, как показано на рисунке ниже.

 [![Вспомогательное представление, используемое для представления заголовка для определенного раздела, как показано здесь](uicollectionview-images/02a-supplementary-view.png)](uicollectionview-images/02a-supplementary-view.png#lightbox)

Чтобы использовать вспомогательное представление, сначала необходимо зарегистрировать его в `ViewDidLoad` методе:

```csharp
CollectionView.RegisterClassForSupplementaryView (typeof(Header), UICollectionElementKindSection.Header, headerId);
```

Затем представление должно возвращаться с помощью `GetViewForSupplementaryElement` , созданного с помощью `DequeueReusableSupplementaryView` , и наследует от `UICollectionReusableView` . В следующем фрагменте кода создается Супплементаривиев, показанный на снимке экрана выше:

```csharp
public override UICollectionReusableView GetViewForSupplementaryElement (UICollectionView collectionView, NSString elementKind, NSIndexPath indexPath)
        {
            var headerView = (Header)collectionView.DequeueReusableSupplementaryView (elementKind, headerId, indexPath);
            headerView.Text = "Supplementary View";
            return headerView;
        }

```

Дополнительные представления являются более универсальными, чем просто верхние и нижние колонтитулы.
Они могут располагаться в любом месте в представлении коллекции и могут состоять из любых представлений, что делает их внешний вид полностью настраиваемым.

 <a name="Decoration_Views"></a>

## <a name="decoration-views"></a>Представления оформления

Декорированные представления — это чисто визуальные представления, которые могут отображаться в `UICollectionView` . В отличие от ячеек и дополнительных представлений они не управляются данными. Они всегда создаются в подклассе макета и затем могут меняться в качестве макета содержимого. Например, представление декорирования можно использовать для представления фона, который прокручивается с содержимым в `UICollectionView` , как показано ниже:

 [![Представление "Оформление" с красным фоном](uicollectionview-images/02c-decoration-view.png)](uicollectionview-images/02c-decoration-view.png#lightbox)

 Приведенный ниже фрагмент кода изменяет фон на красный в классе Samples `CircleLayout` :

 ```csharp
 public class MyDecorationView : UICollectionReusableView
  {
    [Export ("initWithFrame:")]
    public MyDecorationView (CGRect frame) : base (frame)
    {
      BackgroundColor = UIColor.Red;
    }
  }
 ```

## <a name="data-source"></a>Источник данных

Как и в других частях iOS, таких как `UITableView` и `MKMapView` , `UICollectionView` получает свои данные из *источника данных*, который предоставляется в Xamarin. iOS через **`UICollectionViewDataSource`** класс. Этот класс отвечает за предоставление содержимого следующим `UICollectionView` образом:

- **Ячейки** — возвращаются из `GetCell` метода.
- **Дополнительные представления** — возвращаются из `GetViewForSupplementaryElement` метода.
- **Число разделов,** возвращаемое `NumberOfSections` методом. Значение по умолчанию равно 1, если не реализовано.
- **Количество элементов в разделе** , возвращаемых `GetItemsCount` методом.

### <a name="uicollectionviewcontroller"></a>уиколлектионвиевконтроллер
Для удобства `UICollectionViewController` класс доступен. Он автоматически настраивается как делегат, который обсуждается в следующем разделе, и источник данных для его `UICollectionView` представления.

Как `UITableView` и в случае, `UICollectionView` класс будет вызывать его источник данных только для получения ячеек для элементов, наявляющихся на экране.
Ячейки, проходящие за пределами экрана, помещаются в очередь для повторного использования, как показано на следующем рисунке:

 [![Ячейки, проходящие за пределами экрана, помещаются в очередь для повторного использования, как показано ниже.](uicollectionview-images/03-cell-reuse.png)](uicollectionview-images/03-cell-reuse.png#lightbox)

Повторное использование ячеек было упрощено с помощью `UICollectionView` и `UITableView` . Больше не нужно создавать ячейку непосредственно в источнике данных, если она недоступна в очереди повторного использования, так как ячейки регистрируются в системе. Если ячейка недоступна, когда вызов удаляет ячейку из очереди повторного использования, iOS создаст его автоматически на основе зарегистрированного типа или NIB.
Аналогичная методика также доступна для дополнительных представлений.

Например, рассмотрим следующий код, который регистрирует `AnimalCell` класс:

```csharp
static NSString animalCellId = new NSString ("AnimalCell");
CollectionView.RegisterClassForCell (typeof(AnimalCell), animalCellId);
```

Если `UICollectionView` требуется ячейка, так как ее элемент находится на экране, `UICollectionView` вызывается метод источника данных `GetCell` . Подобно тому, как это работает с Уитаблевиев, этот метод отвечает за настройку ячейки из резервных данных, которая `AnimalCell` в данном случае является классом.

В следующем коде показана реализация `GetCell` , которая возвращает `AnimalCell` экземпляр:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, Foundation.NSIndexPath indexPath)
{
        var animalCell = (AnimalCell)collectionView.DequeueReusableCell (animalCellId, indexPath);

        var animal = animals [indexPath.Row];

        animalCell.Image = animal.Image;

        return animalCell;
}
```

Вызов `DequeReusableCell` — это место, где ячейка будет либо выведена из очереди повторного использования, либо, если ячейка недоступна в очереди, созданной на основе типа, зарегистрированного в вызове `CollectionView.RegisterClassForCell` .

В этом случае, зарегистрировав `AnimalCell` класс, iOS создает новый `AnimalCell` внутренний и возвращает его при вызове отмены в очередь ячейки, после чего она настраивается с помощью изображения, содержащегося в классе «Animal», и возвращается для отображения в `UICollectionView` .

 <a name="Delegate"></a>

### <a name="delegate"></a>Делегат

`UICollectionView`Класс использует делегат типа `UICollectionViewDelegate` для поддержки взаимодействия с содержимым в `UICollectionView` . Это позволяет управлять:

- **Выбор ячейки** — определяет, выбрана ли ячейка.
- **Выделение ячеек** — определяет, затрагивается ли ячейка в данный момент.
- Меню **ячеек** — меню, отображаемое для ячейки в ответ на длительный жест нажатия.

Как и в случае с источником данных, по `UICollectionViewController` умолчанию настроено как делегат для `UICollectionView` .

 <a name="Cell_HighLighting"></a>

#### <a name="cell-highlighting"></a>Выделение ячеек

При нажатии ячейки ячейка переходит в выделенное состояние и не выбирается до тех пор, пока пользователь не отрывает палец от ячейки. Это позволяет временно изменить внешний вид ячейки до ее фактического выбора. После выбора отображается ячейка `SelectedBackgroundView` . На рисунке ниже показано выделенное состояние непосредственно перед выбором.

 [![На этом рисунке показано выделенное состояние непосредственно перед выбором.](uicollectionview-images/04-cell-highlight.png)](uicollectionview-images/04-cell-highlight.png#lightbox)

Для реализации выделения `ItemHighlighted` `ItemUnhighlighted` `UICollectionViewDelegate` можно использовать методы и. Например, следующий код будет применять желтый фон элемента, `ContentView` когда ячейка выделяется, и белый фон при невыделенном виде, как показано на рисунке выше:

```csharp
public override void ItemHighlighted (UICollectionView collectionView, NSIndexPath indexPath)
{
        var cell = collectionView.CellForItem(indexPath);
        cell.ContentView.BackgroundColor = UIColor.Yellow;
}

public override void ItemUnhighlighted (UICollectionView collectionView, NSIndexPath indexPath)
{
        var cell = collectionView.CellForItem(indexPath);
        cell.ContentView.BackgroundColor = UIColor.White;
}
```

 <a name="Disabling_Selection"></a>

#### <a name="disabling-selection"></a>Отключение выбора

Выбор по умолчанию включен в `UICollectionView` . Чтобы отключить выбор, переопределите `ShouldHighlightItem` и возвратите false, как показано ниже.

```csharp
public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath)
{
        return false;
}
```

Если выделение отключено, процесс выбора ячейки также отключается. Кроме того, существует также `ShouldSelectItem` метод, который управляет выбором напрямую, хотя если `ShouldHighlightItem` реализуется и возвращает значение false, `ShouldSelectItem` не вызывается.

 `ShouldSelectItem`позволяет включать или отключать выбор для отдельных элементов, если `ShouldHighlightItem` не реализован. Он также позволяет выделять выделение без выделения, если `ShouldHighlightItem` реализуется и возвращает значение true, тогда как `ShouldSelectItem` возвращает значение false.

 <a name="Cell_Menus"></a>

#### <a name="cell-menus"></a>Меню ячеек

Каждая ячейка в может `UICollectionView` Отображать меню, которое позволяет при необходимости поддерживать вырезание, копирование и вставку. Чтобы создать меню редактирования для ячейки, выполните следующие действия.

1. Переопределяйте `ShouldShowMenu` и возвращают значение true, если элемент должен отображать меню.
1. Переопределяйте `CanPerformAction` и возвращают значение true для каждого действия, которое может выполнять элемент, что может быть любым из вырезания, копирования или вставки.
1. Переопределите `PerformAction` для выполнения операции редактирования, копирования и вставки.

На следующем снимке экрана показано меню при длительной нажатии ячейки:

 [![На этом снимке экрана показано меню при длительной нажатии ячейки](uicollectionview-images/04a-menu.png)](uicollectionview-images/04a-menu.png#lightbox)

 <a name="Layout"></a>

## <a name="layout"></a>Layout

`UICollectionView`поддерживает систему макета, которая позволяет управлять расположением всех ее элементов, ячеек, дополнительных представлений и Декорирований, независимо от `UICollectionView` самого себя.
Используя систему макета, приложение может поддерживать макеты, такие как сетка, как показано в этой статье, а также предоставлять пользовательские макеты.

 <a name="Layout_Basics"></a>

### <a name="layout-basics"></a>Основы макета

Макеты в `UICollectionView` определяются в классе, который наследует от `UICollectionViewLayout` . Реализация макета отвечает за создание атрибутов макета для каждого элемента в `UICollectionView` . Существует два способа создания макета.

- Используйте встроенный `UICollectionViewFlowLayout` .
- Предоставьте пользовательский макет, наследуя от `UICollectionViewLayout` .

 <a name="Flow_Layout"></a>

### <a name="flow-layout"></a>Потоковый макет

`UICollectionViewFlowLayout`Класс предоставляет макет на основе строк, который подходит для упорядочения содержимого в сетке ячеек, как мы видели.

Чтобы использовать потоковый макет:

- Создайте экземпляр `UICollectionViewFlowLayout` :

```csharp
var layout = new UICollectionViewFlowLayout ();
```

- Передайте экземпляр в конструктор класса `UICollectionView` :

```csharp
simpleCollectionViewController = new SimpleCollectionViewController (layout);
```

Это все, что необходимо для размещения содержимого в сетке. Кроме того, при изменении ориентации `UICollectionViewFlowLayout` маркеры соответствующим образом переупорядочивают содержимое, как показано ниже:

 [![Пример изменения ориентации](uicollectionview-images/05-layout-orientation.png)](uicollectionview-images/05-layout-orientation.png#lightbox)

 <a name="Section_Inset"></a>

#### <a name="section-inset"></a>Отступ раздела

Чтобы предоставить некоторое пространство вокруг `UIContentView` , макеты имеют `SectionInset` свойство типа `UIEdgeInsets` . Например, приведенный ниже код предоставляет буфер размером 50 пикселей вокруг каждого раздела в, `UIContentView` когда `UICollectionViewFlowLayout` :

```csharp
var layout = new UICollectionViewFlowLayout ();
layout.SectionInset = new UIEdgeInsets (50,50,50,50);
```

Это приводит к отступу вокруг раздела, как показано ниже.

 [![Отступ вокруг раздела, как показано здесь](uicollectionview-images/06-sectioninset.png)](uicollectionview-images/06-sectioninset.png#lightbox)

 <a name="Subclassing_UICollectionViewFlowLayout"></a>

#### <a name="subclassing-uicollectionviewflowlayout"></a>Подклассировать Уиколлектионвиевфловлайаут

В выпуске для `UICollectionViewFlowLayout` непосредственного использования он также может быть подклассом для дальнейшей настройки макета содержимого вдоль линии. Например, это можно использовать для создания макета, который не заключает ячейки в сетку, а вместо этого создает одну строку с горизонтальной прокруткой, как показано ниже:

 [![Одна строка с горизонтальным прокруткой](uicollectionview-images/07-line-layout.png)](uicollectionview-images/07-line-layout.png#lightbox)

Для реализации этого с помощью подклассов `UICollectionViewFlowLayout` требуется:

- Инициализация любых свойств макета, которые применяются к самому макету или ко всем элементам в макете в конструкторе.
- Переопределение `ShouldInvalidateLayoutForBoundsChange` , возвращающее значение true, чтобы при `UICollectionView` изменении границ изменений Макет ячеек будет пересчитан. В этом случае в этом случае код для преобразования, применяемый к ячейке центермост, будет применен во время прокрутки.
- Переопределение `TargetContentOffset` для того, чтобы привязать ячейку центермост к центру по `UICollectionView` мере остановки прокрутки.
- Переопределение `LayoutAttributesForElementsInRect` для возврата массива `UICollectionViewLayoutAttributes` . Каждый из них `UICollectionViewLayoutAttribute` содержит сведения о разметке конкретного элемента, включая такие свойства, `Center` как `Size` , `ZIndex` и `Transform3D` .

В следующем коде показана такая реализация:

```csharp
using System;
using CoreGraphics;
using Foundation;
using UIKit;
using CoreGraphics;
using CoreAnimation;

namespace SimpleCollectionView
{
  public class LineLayout : UICollectionViewFlowLayout
  {
    public const float ITEM_SIZE = 200.0f;
    public const int ACTIVE_DISTANCE = 200;
    public const float ZOOM_FACTOR = 0.3f;

    public LineLayout ()
    {
      ItemSize = new CGSize (ITEM_SIZE, ITEM_SIZE);
      ScrollDirection = UICollectionViewScrollDirection.Horizontal;
            SectionInset = new UIEdgeInsets (400,0,400,0);
      MinimumLineSpacing = 50.0f;
    }

    public override bool ShouldInvalidateLayoutForBoundsChange (CGRect newBounds)
    {
      return true;
    }

    public override UICollectionViewLayoutAttributes[] LayoutAttributesForElementsInRect (CGRect rect)
    {
      var array = base.LayoutAttributesForElementsInRect (rect);
            var visibleRect = new CGRect (CollectionView.ContentOffset, CollectionView.Bounds.Size);

      foreach (var attributes in array) {
        if (attributes.Frame.IntersectsWith (rect)) {
          float distance = (float)(visibleRect.GetMidX () - attributes.Center.X);
          float normalizedDistance = distance / ACTIVE_DISTANCE;
          if (Math.Abs (distance) < ACTIVE_DISTANCE) {
            float zoom = 1 + ZOOM_FACTOR * (1 - Math.Abs (normalizedDistance));
            attributes.Transform3D = CATransform3D.MakeScale (zoom, zoom, 1.0f);
            attributes.ZIndex = 1;
          }
        }
      }
      return array;
    }

    public override CGPoint TargetContentOffset (CGPoint proposedContentOffset, CGPoint scrollingVelocity)
    {
      float offSetAdjustment = float.MaxValue;
      float horizontalCenter = (float)(proposedContentOffset.X + (this.CollectionView.Bounds.Size.Width / 2.0));
      CGRect targetRect = new CGRect (proposedContentOffset.X, 0.0f, this.CollectionView.Bounds.Size.Width, this.CollectionView.Bounds.Size.Height);
      var array = base.LayoutAttributesForElementsInRect (targetRect);
      foreach (var layoutAttributes in array) {
        float itemHorizontalCenter = (float)layoutAttributes.Center.X;
        if (Math.Abs (itemHorizontalCenter - horizontalCenter) < Math.Abs (offSetAdjustment)) {
          offSetAdjustment = itemHorizontalCenter - horizontalCenter;
        }
      }
            return new CGPoint (proposedContentOffset.X + offSetAdjustment, proposedContentOffset.Y);
    }

  }
}
```

 <a name="Custom_Layout"></a>

### <a name="custom-layout"></a>Пользовательский макет

Помимо использования `UICollectionViewFlowLayout` , макеты также можно полностью настроить, наследуя непосредственно от `UICollectionViewLayout` .

Ниже приведены ключевые методы переопределения.

- `PrepareLayout`— Используется для выполнения начальных геометрических вычислений, которые будут использоваться в процессе макета.
- `CollectionViewContentSize`— Возвращает размер области, используемой для вывода содержимого.
- `LayoutAttributesForElementsInRect`— Как и в приведенном ранее примере Уиколлектионвиевфловлайаут, этот метод используется для предоставления сведений о `UICollectionView` том, как разметка каждого элемента. Однако, в отличие от `UICollectionViewFlowLayout` , при создании пользовательского макета можно располагать элементы, но вы выбираете их.

Например, одно и то же содержимое может быть представлено в циклической структуре, как показано ниже:

 [![Круглый пользовательский макет, как показано здесь](uicollectionview-images/08-circle-layout.png)](uicollectionview-images/08-circle-layout.png#lightbox)

Очень мощное дело в макетах заключается в том, что для того, чтобы изменить макет с горизонтальной прокруткой, а затем к этому циклу, необходимо изменить только класс макета, предоставленный для `UICollectionView` . Ничего в `UICollectionView` , его делегат или код источника данных вообще не меняются.

## <a name="changes-in-ios-9"></a>Изменения в iOS 9

В iOS 9 представление коллекции ( `UICollectionView` ) теперь поддерживает перетаскивание переупорядочения элементов из поля путем добавления нового распознавателя жестов по умолчанию и нескольких новых методов поддержки.

Используя эти новые методы, можно легко реализовать перетаскивание для изменения порядка в представлении коллекции и иметь возможность настройки внешнего вида элементов на любом этапе процесса изменения порядка.

[![Пример процесса изменения порядка](uicollectionview-images/intro01.png)](uicollectionview-images/intro01.png#lightbox)

В этой статье мы рассмотрим реализацию перетаскивания в приложении Xamarin. iOS, а также некоторые другие изменения, внесенные в iOS 9 в элемент управления "представление коллекции":

- [Простое изменение порядка элементов](#Easy-Reordering-of-Items)
  - [Простой пример переупорядочивания](#Simple-Reordering-Example)
  - [Использование пользовательского распознавателя жестов](#Using-a-Custom-Gesture-Recognizer)
  - [Пользовательские макеты и изменение порядка](#Custom-Layouts-and-Reording)
- [Изменения в представлении коллекции](#collection-view-changes)

<a name="Easy-Reordering-of-Items"></a>

## <a name="reordering-of-items"></a>Изменение порядка элементов

Как упоминалось выше, одно из наиболее значительных изменений в представлении коллекции в iOS 9 было добавлением простой перетаскивания функциональности из поля.

В iOS 9 самый быстрый способ добавить изменение порядка в представление коллекции — использовать `UICollectionViewController` .
Контроллер представления коллекции теперь имеет `InstallsStandardGestureForInteractiveMovement` свойство, добавляющее Стандартный *распознаватель жестов* , поддерживающий перетаскивание для изменения порядка элементов в коллекции.
Так как значение по умолчанию равно `true` , необходимо реализовать `MoveItem` метод `UICollectionViewDataSource` класса для поддержки перетаскивания. Например.

```csharp
public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
  // Reorder our list of items
  ...
}
```

<a name="Simple-Reordering-Example"></a>

### <a name="simple-reordering-example"></a>Простой пример переупорядочивания

В качестве краткого примера запустите новый проект Xamarin. iOS и измените файл **Main. Storyboard** . Перетащите элемент на `UICollectionViewController` поверхность конструктора:

[![Добавление Уиколлектионвиевконтроллер](uicollectionview-images/quick01.png)](uicollectionview-images/quick01.png#lightbox)

Выберите представление коллекции (это проще всего сделать в структуре документа). На вкладке Макет Панель свойств задайте следующие размеры, как показано на снимке экрана ниже:

- **Размер ячейки**: ширина — 60 | Высота — 60
- **Размер заголовка**: ширина — 0 | Высота — 0
- **Размер нижнего колонтитула**: ширина — 0 | Высота — 0
- **Минимальный промежуток**: для ячеек — 8 | Для строк — 8
- **Раздел инсетс**: Top – 16 | Снизу — 16 | Left — 16 | Справа — 16

[![Настройка размеров представлений коллекций](uicollectionview-images/quick04.png)](uicollectionview-images/quick04.png#lightbox)

Затем измените ячейку по умолчанию:

- Изменить цвет фона на синий
- Добавить метку, которая будет использоваться в качестве заголовка для ячейки
- Присвоить идентификатору повторного использования значение **Cell**

[![Изменение ячейки по умолчанию](uicollectionview-images/quick02.png)](uicollectionview-images/quick02.png#lightbox)

Добавьте ограничения, чтобы поместить метку в центре ячейки по мере изменения размера:

На панели **свойств** для _Коллектионвиевцелл_ и задайте **классу** значение `TextCollectionViewCell` :

[![Присвойте классу значение Текстколлектионвиевцелл](uicollectionview-images/quick05.png)](uicollectionview-images/quick05.png#lightbox)

В качестве **представления для повторного использования коллекции** задайте `Cell` следующие значения:

[![Задать представление коллекции для повторного использования в ячейке](uicollectionview-images/quick06.png)](uicollectionview-images/quick06.png#lightbox)

Наконец, выберите метку и назовите ее `TextLabel` :

[![имя метки Текстлабел](uicollectionview-images/quick07.png)](uicollectionview-images/quick07.png#lightbox)

Измените `TextCollectionViewCell` класс и добавьте следующие свойства:

```csharp
using System;
using Foundation;
using UIKit;

namespace CollectionView
{
  public partial class TextCollectionViewCell : UICollectionViewCell
  {
    #region Computed Properties
    public string Title {
      get { return TextLabel.Text; }
      set { TextLabel.Text = value; }
    }
    #endregion

    #region Constructors
    public TextCollectionViewCell (IntPtr handle) : base (handle)
    {
    }
    #endregion
  }
}
```

Здесь `Text` свойство метки представлено как заголовок ячейки, поэтому его можно задать из кода.

Добавьте в проект новый класс C# и вызовите его `WaterfallCollectionSource` . Измените файл и сделайте его следующим:

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;

namespace CollectionView
{
  public class WaterfallCollectionSource : UICollectionViewDataSource
  {
    #region Computed Properties
    public WaterfallCollectionView CollectionView { get; set;}
    public List<int> Numbers { get; set; } = new List<int> ();
    #endregion

    #region Constructors
    public WaterfallCollectionSource (WaterfallCollectionView collectionView)
    {
      // Initialize
      CollectionView = collectionView;

      // Init numbers collection
      for (int n = 0; n < 100; ++n) {
        Numbers.Add (n);
      }
    }
    #endregion

    #region Override Methods
    public override nint NumberOfSections (UICollectionView collectionView) {
      // We only have one section
      return 1;
    }

    public override nint GetItemsCount (UICollectionView collectionView, nint section) {
      // Return the number of items
      return Numbers.Count;
    }

    public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
    {
      // Get a reusable cell and set {~~it's~>its~~} title from the item
      var cell = collectionView.DequeueReusableCell ("Cell", indexPath) as TextCollectionViewCell;
      cell.Title = Numbers [(int)indexPath.Item].ToString();

      return cell;
    }

    public override bool CanMoveItem (UICollectionView collectionView, NSIndexPath indexPath) {
      // We can always move items
      return true;
    }

    public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
    {
      // Reorder our list of items
      var item = Numbers [(int)sourceIndexPath.Item];
      Numbers.RemoveAt ((int)sourceIndexPath.Item);
      Numbers.Insert ((int)destinationIndexPath.Item, item);
    }
    #endregion
  }
}
```

Этот класс будет источником данных для нашего представления коллекции и предоставит сведения для каждой ячейки в коллекции.
Обратите внимание, что `MoveItem` метод реализуется, чтобы разрешить перетаскивание элементов в коллекции.

Добавьте в проект еще один новый класс C# и вызовите его `WaterfallCollectionDelegate` . Измените этот файл и сделайте его следующим:

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;

namespace CollectionView
{
  public class WaterfallCollectionDelegate : UICollectionViewDelegate
  {
    #region Computed Properties
    public WaterfallCollectionView CollectionView { get; set;}
    #endregion

    #region Constructors
    public WaterfallCollectionDelegate (WaterfallCollectionView collectionView)
    {

      // Initialize
      CollectionView = collectionView;

    }
    #endregion

    #region Overrides Methods
    public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath) {
      // Always allow for highlighting
      return true;
    }

    public override void ItemHighlighted (UICollectionView collectionView, NSIndexPath indexPath)
    {
      // Get cell and change to green background
      var cell = collectionView.CellForItem(indexPath);
      cell.ContentView.BackgroundColor = UIColor.FromRGB(183,208,57);
    }

    public override void ItemUnhighlighted (UICollectionView collectionView, NSIndexPath indexPath)
    {
      // Get cell and return to blue background
      var cell = collectionView.CellForItem(indexPath);
      cell.ContentView.BackgroundColor = UIColor.FromRGB(164,205,255);
    }
    #endregion
  }
}
```

Это будет действовать в качестве делегата для нашего представления коллекции. Методы были переопределены для выделения ячейки, когда пользователь взаимодействует с ней в представлении коллекции.

Добавьте в проект один последний класс C# и вызовите его `WaterfallCollectionView` . Измените этот файл и сделайте его следующим:

```csharp
using System;
using UIKit;
using System.Collections.Generic;
using Foundation;

namespace CollectionView
{
  [Register("WaterfallCollectionView")]
  public class WaterfallCollectionView : UICollectionView
  {

    #region Constructors
    public WaterfallCollectionView (IntPtr handle) : base (handle)
    {
    }
    #endregion

    #region Override Methods
    public override void AwakeFromNib ()
    {
      base.AwakeFromNib ();

      // Initialize
      DataSource = new WaterfallCollectionSource(this);
      Delegate = new WaterfallCollectionDelegate(this);

    }
    #endregion
  }
}
```

Обратите внимание, что `DataSource` и `Delegate` , что мы создали выше, задаются при создании представления коллекции из его раскадровки (или файла **XIB** ).

Снова измените файл **Main. Storyboard** и выберите представление коллекции и перейдите к **свойствам**. Присвойте **классу** пользовательский `WaterfallCollectionView` класс, который мы определили выше:

Сохраните изменения, внесенные в пользовательский интерфейс, и запустите приложение.
Если пользователь выбирает элемент из списка и перетаскивает его в новое место, другие элементы будут анимироваться автоматически по мере того, как они переходят за пределы элемента.
Когда пользователь удаляет элемент в новом расположении, он перейдет в это место. Например.

[![Пример перетаскивания элемента в новое место](uicollectionview-images/intro01.png)](uicollectionview-images/intro01.png#lightbox)

<a name="Using-a-Custom-Gesture-Recognizer"></a>

### <a name="using-a-custom-gesture-recognizer"></a>Использование пользовательского распознавателя жестов

В случаях, когда вы не можете использовать `UICollectionViewController` и должен использовать обычный `UIViewController` или если вы хотите повысить управляемость жеста перетаскивания, можно создать собственный распознаватель жестов и добавить его в представление коллекции при загрузке представления. Например.

```csharp
public override void ViewDidLoad ()
{
  base.ViewDidLoad ();

  // Create a custom gesture recognizer
  var longPressGesture = new UILongPressGestureRecognizer ((gesture) => {

    // Take action based on state
    switch(gesture.State) {
    case UIGestureRecognizerState.Began:
      var selectedIndexPath = CollectionView.IndexPathForItemAtPoint(gesture.LocationInView(View));
      if (selectedIndexPath !=null) {
        CollectionView.BeginInteractiveMovementForItem(selectedIndexPath);
      }
      break;
    case UIGestureRecognizerState.Changed:
      CollectionView.UpdateInteractiveMovementTargetPosition(gesture.LocationInView(View));
      break;
    case UIGestureRecognizerState.Ended:
      CollectionView.EndInteractiveMovement();
      break;
    default:
      CollectionView.CancelInteractiveMovement();
      break;
    }

  });

  // Add the custom recognizer to the collection view
  CollectionView.AddGestureRecognizer(longPressGesture);
}
```

Здесь мы используем несколько новых методов, добавленных в представление коллекции для реализации и управления операцией перетаскивания:

- `BeginInteractiveMovementForItem`— Отмечает начало операции перемещения.
- `UpdateInteractiveMovementTargetPosition`— Отправляется при обновлении расположения элемента.
- `EndInteractiveMovement`— Отмечает конец перемещения элемента.
- `CancelInteractiveMovement`— Помечает пользователя, который отменяет операцию перемещения.

При запуске приложения операция перетаскивания будет работать точно так же, как распознаватель по умолчанию, который поставляется с представлением коллекции.

<a name="Custom-Layouts-and-Reording"></a>

### <a name="custom-layouts-and-reordering"></a>Пользовательские макеты и изменение порядка

В iOS 9 были добавлены несколько новых методов для работы с перетаскиванием и пользовательскими макетами в представлении коллекции. Чтобы изучить эту функцию, добавим к коллекции пользовательский макет.

Сначала добавьте в проект новый класс C# с именем `WaterfallCollectionLayout` . Измените его и сделайте так, чтобы он выглядел следующим образом:

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;
using CoreGraphics;

namespace CollectionView
{
  [Register("WaterfallCollectionLayout")]
  public class WaterfallCollectionLayout : UICollectionViewLayout
  {
    #region Private Variables
    private int columnCount = 2;
    private nfloat minimumColumnSpacing = 10;
    private nfloat minimumInterItemSpacing = 10;
    private nfloat headerHeight = 0.0f;
    private nfloat footerHeight = 0.0f;
    private UIEdgeInsets sectionInset = new UIEdgeInsets(0, 0, 0, 0);
    private WaterfallCollectionRenderDirection itemRenderDirection = WaterfallCollectionRenderDirection.ShortestFirst;
    private Dictionary<nint,UICollectionViewLayoutAttributes> headersAttributes = new Dictionary<nint, UICollectionViewLayoutAttributes>();
    private Dictionary<nint,UICollectionViewLayoutAttributes> footersAttributes = new Dictionary<nint, UICollectionViewLayoutAttributes>();
    private List<CGRect> unionRects = new List<CGRect>();
    private List<nfloat> columnHeights = new List<nfloat>();
    private List<UICollectionViewLayoutAttributes> allItemAttributes = new List<UICollectionViewLayoutAttributes>();
    private List<List<UICollectionViewLayoutAttributes>> sectionItemAttributes = new List<List<UICollectionViewLayoutAttributes>>();
    private nfloat unionSize = 20;
    #endregion

    #region Computed Properties
    [Export("ColumnCount")]
    public int ColumnCount {
      get { return columnCount; }
      set {
        WillChangeValue ("ColumnCount");
        columnCount = value;
        DidChangeValue ("ColumnCount");

        InvalidateLayout ();
      }
    }

    [Export("MinimumColumnSpacing")]
    public nfloat MinimumColumnSpacing {
      get { return minimumColumnSpacing; }
      set {
        WillChangeValue ("MinimumColumnSpacing");
        minimumColumnSpacing = value;
        DidChangeValue ("MinimumColumnSpacing");

        InvalidateLayout ();
      }
    }

    [Export("MinimumInterItemSpacing")]
    public nfloat MinimumInterItemSpacing {
      get { return minimumInterItemSpacing; }
      set {
        WillChangeValue ("MinimumInterItemSpacing");
        minimumInterItemSpacing = value;
        DidChangeValue ("MinimumInterItemSpacing");

        InvalidateLayout ();
      }
    }

    [Export("HeaderHeight")]
    public nfloat HeaderHeight {
      get { return headerHeight; }
      set {
        WillChangeValue ("HeaderHeight");
        headerHeight = value;
        DidChangeValue ("HeaderHeight");

        InvalidateLayout ();
      }
    }

    [Export("FooterHeight")]
    public nfloat FooterHeight {
      get { return footerHeight; }
      set {
        WillChangeValue ("FooterHeight");
        footerHeight = value;
        DidChangeValue ("FooterHeight");

        InvalidateLayout ();
      }
    }

    [Export("SectionInset")]
    public UIEdgeInsets SectionInset {
      get { return sectionInset; }
      set {
        WillChangeValue ("SectionInset");
        sectionInset = value;
        DidChangeValue ("SectionInset");

        InvalidateLayout ();
      }
    }

    [Export("ItemRenderDirection")]
    public WaterfallCollectionRenderDirection ItemRenderDirection {
      get { return itemRenderDirection; }
      set {
        WillChangeValue ("ItemRenderDirection");
        itemRenderDirection = value;
        DidChangeValue ("ItemRenderDirection");

        InvalidateLayout ();
      }
    }
    #endregion

    #region Constructors
    public WaterfallCollectionLayout ()
    {
    }

    public WaterfallCollectionLayout(NSCoder coder) : base(coder) {

    }
    #endregion

    #region Public Methods
    public nfloat ItemWidthInSectionAtIndex(int section) {

      var width = CollectionView.Bounds.Width - SectionInset.Left - SectionInset.Right;
      return (nfloat)Math.Floor ((width - ((ColumnCount - 1) * MinimumColumnSpacing)) / ColumnCount);
    }
    #endregion

    #region Override Methods
    public override void PrepareLayout ()
    {
      base.PrepareLayout ();

      // Get the number of sections
      var numberofSections = CollectionView.NumberOfSections();
      if (numberofSections == 0)
        return;

      // Reset collections
      headersAttributes.Clear ();
      footersAttributes.Clear ();
      unionRects.Clear ();
      columnHeights.Clear ();
      allItemAttributes.Clear ();
      sectionItemAttributes.Clear ();

      // Initialize column heights
      for (int n = 0; n < ColumnCount; n++) {
        columnHeights.Add ((nfloat)0);
      }

      // Process all sections
      nfloat top = 0.0f;
      var attributes = new UICollectionViewLayoutAttributes ();
      var columnIndex = 0;
      for (nint section = 0; section < numberofSections; ++section) {
        // Calculate section specific metrics
        var minimumInterItemSpacing = (MinimumInterItemSpacingForSection == null) ? MinimumColumnSpacing :
          MinimumInterItemSpacingForSection (CollectionView, this, section);

        // Calculate widths
        var width = CollectionView.Bounds.Width - SectionInset.Left - SectionInset.Right;
        var itemWidth = (nfloat)Math.Floor ((width - ((ColumnCount - 1) * MinimumColumnSpacing)) / ColumnCount);

        // Calculate section header
        var heightHeader = (HeightForHeader == null) ? HeaderHeight :
          HeightForHeader (CollectionView, this, section);

        if (heightHeader > 0) {
          attributes = UICollectionViewLayoutAttributes.CreateForSupplementaryView (UICollectionElementKindSection.Header, NSIndexPath.FromRowSection (0, section));
          attributes.Frame = new CGRect (0, top, CollectionView.Bounds.Width, heightHeader);
          headersAttributes.Add (section, attributes);
          allItemAttributes.Add (attributes);

          top = attributes.Frame.GetMaxY ();
        }

        top += SectionInset.Top;
        for (int n = 0; n < ColumnCount; n++) {
          columnHeights [n] = top;
        }

        // Calculate Section Items
        var itemCount = CollectionView.NumberOfItemsInSection(section);
        List<UICollectionViewLayoutAttributes> itemAttributes = new List<UICollectionViewLayoutAttributes> ();

        for (nint n = 0; n < itemCount; n++) {
          var indexPath = NSIndexPath.FromRowSection (n, section);
          columnIndex = NextColumnIndexForItem (n);
          var xOffset = SectionInset.Left + (itemWidth + MinimumColumnSpacing) * (nfloat)columnIndex;
          var yOffset = columnHeights [columnIndex];
          var itemSize = (SizeForItem == null) ? new CGSize (0, 0) : SizeForItem (CollectionView, this, indexPath);
          nfloat itemHeight = 0.0f;

          if (itemSize.Height > 0.0f && itemSize.Width > 0.0f) {
            itemHeight = (nfloat)Math.Floor (itemSize.Height * itemWidth / itemSize.Width);
          }

          attributes = UICollectionViewLayoutAttributes.CreateForCell (indexPath);
          attributes.Frame = new CGRect (xOffset, yOffset, itemWidth, itemHeight);
          itemAttributes.Add (attributes);
          allItemAttributes.Add (attributes);
          columnHeights [columnIndex] = attributes.Frame.GetMaxY () + MinimumInterItemSpacing;
        }
        sectionItemAttributes.Add (itemAttributes);

        // Calculate Section Footer
        nfloat footerHeight = 0.0f;
        columnIndex = LongestColumnIndex();
        top = columnHeights [columnIndex] - MinimumInterItemSpacing + SectionInset.Bottom;
        footerHeight = (HeightForFooter == null) ? FooterHeight : HeightForFooter(CollectionView, this, section);

        if (footerHeight > 0) {
          attributes = UICollectionViewLayoutAttributes.CreateForSupplementaryView (UICollectionElementKindSection.Footer, NSIndexPath.FromRowSection (0, section));
          attributes.Frame = new CGRect (0, top, CollectionView.Bounds.Width, footerHeight);
          footersAttributes.Add (section, attributes);
          allItemAttributes.Add (attributes);
          top = attributes.Frame.GetMaxY ();
        }

        for (int n = 0; n < ColumnCount; n++) {
          columnHeights [n] = top;
        }
      }

      var i =0;
      var attrs = allItemAttributes.Count;
      while(i < attrs) {
        var rect1 = allItemAttributes [i].Frame;
        i = (int)Math.Min (i + unionSize, attrs) - 1;
        var rect2 = allItemAttributes [i].Frame;
        unionRects.Add (CGRect.Union (rect1, rect2));
        i++;
      }

    }

    public override CGSize CollectionViewContentSize {
      get {
        if (CollectionView.NumberOfSections () == 0) {
          return new CGSize (0, 0);
        }

        var contentSize = CollectionView.Bounds.Size;
        contentSize.Height = columnHeights [0];
        return contentSize;
      }
    }

    public override UICollectionViewLayoutAttributes LayoutAttributesForItem (NSIndexPath indexPath)
    {
      if (indexPath.Section >= sectionItemAttributes.Count) {
        return null;
      }

      if (indexPath.Item >= sectionItemAttributes [indexPath.Section].Count) {
        return null;
      }

      var list = sectionItemAttributes [indexPath.Section];
      return list [(int)indexPath.Item];
    }

    public override UICollectionViewLayoutAttributes LayoutAttributesForSupplementaryView (NSString kind, NSIndexPath indexPath)
    {
      var attributes = new UICollectionViewLayoutAttributes ();

      switch (kind) {
      case "header":
        attributes = headersAttributes [indexPath.Section];
        break;
      case "footer":
        attributes = footersAttributes [indexPath.Section];
        break;
      }

      return attributes;
    }

    public override UICollectionViewLayoutAttributes[] LayoutAttributesForElementsInRect (CGRect rect)
    {
      var begin = 0;
      var end = unionRects.Count;
      List<UICollectionViewLayoutAttributes> attrs = new List<UICollectionViewLayoutAttributes> ();

      for (int i = 0; i < end; i++) {
        if (rect.IntersectsWith(unionRects[i])) {
          begin = i * (int)unionSize;
        }
      }

      for (int i = end - 1; i >= 0; i--) {
        if (rect.IntersectsWith (unionRects [i])) {
          end = (int)Math.Min ((i + 1) * (int)unionSize, allItemAttributes.Count);
          break;
        }
      }

      for (int i = begin; i < end; i++) {
        var attr = allItemAttributes [i];
        if (rect.IntersectsWith (attr.Frame)) {
          attrs.Add (attr);
        }
      }

      return attrs.ToArray();
    }

    public override bool ShouldInvalidateLayoutForBoundsChange (CGRect newBounds)
    {
      var oldBounds = CollectionView.Bounds;
      return (newBounds.Width != oldBounds.Width);
    }
    #endregion

    #region Private Methods
    private int ShortestColumnIndex() {
      var index = 0;
      var shortestHeight = nfloat.MaxValue;
      var n = 0;

      // Scan each column for the shortest height
      foreach (nfloat height in columnHeights) {
        if (height < shortestHeight) {
          shortestHeight = height;
          index = n;
        }
        ++n;
      }

      return index;
    }

    private int LongestColumnIndex() {
      var index = 0;
      var longestHeight = nfloat.MinValue;
      var n = 0;

      // Scan each column for the shortest height
      foreach (nfloat height in columnHeights) {
        if (height > longestHeight) {
          longestHeight = height;
          index = n;
        }
        ++n;
      }

      return index;
    }

    private int NextColumnIndexForItem(nint item) {
      var index = 0;

      switch (ItemRenderDirection) {
      case WaterfallCollectionRenderDirection.ShortestFirst:
        index = ShortestColumnIndex ();
        break;
      case WaterfallCollectionRenderDirection.LeftToRight:
        index = ColumnCount;
        break;
      case WaterfallCollectionRenderDirection.RightToLeft:
        index = (ColumnCount - 1) - ((int)item / ColumnCount);
        break;
      }

      return index;
    }
    #endregion

    #region Events
    public delegate CGSize WaterfallCollectionSizeDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, NSIndexPath indexPath);
    public delegate nfloat WaterfallCollectionFloatDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, nint section);
    public delegate UIEdgeInsets WaterfallCollectionEdgeInsetsDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, nint section);

    public event WaterfallCollectionSizeDelegate SizeForItem;
    public event WaterfallCollectionFloatDelegate HeightForHeader;
    public event WaterfallCollectionFloatDelegate HeightForFooter;
    public event WaterfallCollectionEdgeInsetsDelegate InsetForSection;
    public event WaterfallCollectionFloatDelegate MinimumInterItemSpacingForSection;
    #endregion
  }
}
```

Этот класс можно использовать для предоставления пользовательского двух столбцов, каскадного макета типа в представление коллекции.
В коде используется кодирование значений ключа (с помощью `WillChangeValue` методов и `DidChangeValue` ) для предоставления привязки данных для наших вычисленных свойств в этом классе.

Затем измените `WaterfallCollectionSource` и внесите следующие изменения и дополнения:

```csharp
private Random rnd = new Random();
...

public List<nfloat> Heights { get; set; } = new List<nfloat> ();
...

public WaterfallCollectionSource (WaterfallCollectionView collectionView)
{
  // Initialize
  CollectionView = collectionView;

  // Init numbers collection
  for (int n = 0; n < 100; ++n) {
    Numbers.Add (n);
    Heights.Add (rnd.Next (0, 100) + 40.0f);
  }
}
```

При этом будет создана случайная высота для каждого элемента, который будет отображаться в списке.

Затем измените `WaterfallCollectionView` класс и добавьте следующее вспомогательное свойство:

```csharp
public WaterfallCollectionSource Source {
  get { return (WaterfallCollectionSource)DataSource; }
}
```

Это облегчит получение данных в пользовательском макете (и высоте элементов).

Наконец, измените контроллер представления и добавьте следующий код:

```csharp
public override void AwakeFromNib ()
{
  base.AwakeFromNib ();

  var waterfallLayout = new WaterfallCollectionLayout ();

  // Wireup events
  waterfallLayout.SizeForItem += (collectionView, layout, indexPath) => {
    var collection = collectionView as WaterfallCollectionView;
    return new CGSize((View.Bounds.Width-40)/3,collection.Source.Heights[(int)indexPath.Item]);
  };

  // Attach the custom layout to the collection
  CollectionView.SetCollectionViewLayout(waterfallLayout, false);
}
```

При этом создается экземпляр пользовательского макета, устанавливается событие для предоставления размера каждого элемента и подключается новый макет к представлению коллекции.

Если снова запустить приложение Xamarin. iOS, представление коллекции будет выглядеть следующим образом:

[![Представление коллекции теперь будет выглядеть следующим образом:](uicollectionview-images/custom01.png)](uicollectionview-images/custom01.png#lightbox)

Мы по-прежнему можем перетаскивать элементы, как и раньше, но теперь размер элементов изменится в соответствии с новым расположением при их удалении.

## <a name="collection-view-changes"></a>Изменения в представлении коллекции

В следующих разделах мы подробно рассмотрим изменения, внесенные в каждый класс в представлении коллекции с помощью iOS 9.

### <a name="uicollectionview"></a>UICollectionView

В класс для iOS 9 были внесены следующие изменения или дополнения `UICollectionView` :

- `BeginInteractiveMovementForItem`— Отмечает начало операции перетаскивания.
- `CancelInteractiveMovement`— Информирует представление коллекции о том, что пользователь отменил операцию перетаскивания.
- `EndInteractiveMovement`— Информирует представление коллекции о том, что пользователь завершил операцию перетаскивания.
- `GetIndexPathsForVisibleSupplementaryElements`— Возвращает элемент `indexPath` верхнего или нижнего колонтитула в разделе представления коллекции.
- `GetSupplementaryView`— Возвращает заданный верхний или нижний колонтитул.
- `GetVisibleSupplementaryViews`— Возвращает список всех видимых верхних и нижних колонтитулов.
- `UpdateInteractiveMovementTargetPosition`— Информирует представление коллекции о том, что пользователь переместил или перемещает элемент во время операции перетаскивания.

### <a name="uicollectionviewcontroller"></a>уиколлектионвиевконтроллер

В класс в iOS 9 были внесены следующие изменения или дополнения `UICollectionViewController` .

- `InstallsStandardGestureForInteractiveMovement`— Если `true` будет использоваться новый распознаватель жестов, который автоматически поддерживает возможность перетаскивания.
- `CanMoveItem`— Информирует представление коллекции, если для данного элемента можно изменить порядок перетаскивания.
- `GetTargetContentOffset`— Используется для получения смещения заданного элемента представления коллекции.
- `GetTargetIndexPathForMove`— Получает объект `indexPath` для заданного элемента для операции перетаскивания.
- `MoveItem`— Перемещает порядок заданного элемента в списке.

### <a name="uicollectionviewdatasource"></a>уиколлектионвиевдатасаурце

В класс в iOS 9 были внесены следующие изменения или дополнения `UICollectionViewDataSource` .

- `CanMoveItem`— Информирует представление коллекции, если для данного элемента можно изменить порядок перетаскивания.
- `MoveItem`— Перемещает порядок заданного элемента в списке.

### <a name="uicollectionviewdelegate"></a>уиколлектионвиевделегате

В класс в iOS 9 были внесены следующие изменения или дополнения `UICollectionViewDelegate` .

- `GetTargetContentOffset`— Используется для получения смещения заданного элемента представления коллекции.
- `GetTargetIndexPathForMove`— Получает объект `indexPath` для заданного элемента для операции перетаскивания.

### <a name="uicollectionviewflowlayout"></a>уиколлектионвиевфловлайаут

В класс в iOS 9 были внесены следующие изменения или дополнения `UICollectionViewFlowLayout` .

- `SectionFootersPinToVisibleBounds`— Фиксирует нижние колонтитулы раздела до видимых границ представления коллекции.
- `SectionHeadersPinToVisibleBounds`— Фиксирует заголовки разделов в видимых границах представления коллекции.

### <a name="uicollectionviewlayout"></a>уиколлектионвиевлайаут

В класс в iOS 9 были внесены следующие изменения или дополнения `UICollectionViewLayout` .

- `GetInvalidationContextForEndingInteractiveMovementOfItems`— Возвращает контекст недействительности в конце операции перетаскивания, когда пользователь либо завершает перетаскивание, либо отменяет его.
- `GetInvalidationContextForInteractivelyMovingItems`— Возвращает контекст недействительности в начале операции перетаскивания.
- `GetLayoutAttributesForInteractivelyMovingItem`— Получает атрибуты макета для данного элемента при перетаскивании элемента.
- `GetTargetIndexPathForInteractivelyMovingItem`— Возвращает объект `indexPath` элемента, который находится в заданной точке при перетаскивании элемента.

### <a name="uicollectionviewlayoutattributes"></a>уиколлектионвиевлайаутаттрибутес

В класс в iOS 9 были внесены следующие изменения или дополнения `UICollectionViewLayoutAttributes` .

- `CollisionBoundingPath`— Возвращает путь конфликта двух элементов во время операции перетаскивания.
- `CollisionBoundsType`— Возвращает тип конфликта (как `UIDynamicItemCollisionBoundsType` ), произошедший во время операции перетаскивания.

### <a name="uicollectionviewlayoutinvalidationcontext"></a>уиколлектионвиевлайаутинвалидатионконтекст

В класс в iOS 9 были внесены следующие изменения или дополнения `UICollectionViewLayoutInvalidationContext` .

- `InteractiveMovementTarget`— Возвращает целевой элемент операции перетаскивания.
- `PreviousIndexPathsForInteractivelyMovingItems`— Возвращает `indexPaths` список других элементов, вовлеченных в операцию перетаскивания для изменения порядка.
- `TargetIndexPathsForInteractivelyMovingItems`— Возвращает `indexPaths` элементы, которые будут переупорядочены в результате операции перетаскивания.

### <a name="uicollectionviewsource"></a>уиколлектионвиевсаурце

В класс в iOS 9 были внесены следующие изменения или дополнения `UICollectionViewSource` .

- `CanMoveItem`— Информирует представление коллекции, если для данного элемента можно изменить порядок перетаскивания.
- `GetTargetContentOffset`— Возвращает смещения элементов, которые будут перемещены с помощью операции перетаскивания.
- `GetTargetIndexPathForMove`— Возвращает объект `indexPath` , который будет перемещен во время операции перетаскивания.
- `MoveItem`— Перемещает порядок заданного элемента в списке.

## <a name="summary"></a>Сводка

В этой статье были рассмотрены изменения в представлениях коллекций в iOS 9 и описаны способы их реализации в Xamarin. iOS.
Он охватывает реализацию простого действия перетаскивания в представлении коллекции; Использование пользовательского распознавателя жестов с переупорядочением; и способ перетягивания влияет на макет пользовательского представления коллекции.

## <a name="related-links"></a>Связанные ссылки

- [Примеры для iOS 9](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [Пример представления коллекции](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-collectionview)
- [Симплеколлектионвиев (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/simplecollectionview)
- [События, протоколы и делегаты](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Работа с таблицами и ячейками](~/ios/user-interface/controls/tables/index.md)
