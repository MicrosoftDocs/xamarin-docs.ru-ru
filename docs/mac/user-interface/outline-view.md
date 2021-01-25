---
title: Структурированные представления в Xamarin. Mac
description: В этой статье рассматривается работа с представлениями структуры в приложении Xamarin. Mac. Здесь описывается создание и обслуживание представлений структуры в Xcode и Interface Builder и работа с ними программными средствами.
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: f10c2f54c2440e97faa6491efcaa48ec109d0008
ms.sourcegitcommit: 424eaef56fd2933c98e72f1d3e7ac71730fe4835
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/25/2021
ms.locfileid: "98758101"
---
# <a name="outline-views-in-xamarinmac"></a>Структурированные представления в Xamarin. Mac

_В этой статье рассматривается работа с представлениями структуры в приложении Xamarin. Mac. Здесь описывается создание и обслуживание представлений структуры в Xcode и Interface Builder и работа с ними программными средствами._

При работе с C# и .NET в приложении Xamarin. Mac у вас есть доступ к тем же представлениям структуры, что и разработчик, работающий на *уровне цели-C* и *Xcode* . Так как Xamarin. Mac интегрируется напрямую с Xcode, можно использовать _Interface Builder_ Xcode для создания и обслуживания представлений структуры (или при необходимости создавать их непосредственно в коде C#).

Представление структуры — это тип таблицы, позволяющий пользователю разворачивать или сворачивать строки иерархических данных. Как и табличное представление, в представлении структуры отображаются данные для набора связанных элементов, строки которых представляют отдельные элементы и столбцы, представляющие атрибуты этих элементов. В отличие от табличного представления элементы в представлении структуры не расположены в плоском списке, они организованы в виде иерархии, например файлов и папок на жестком диске.

[![Пример выполнения приложения](outline-view-images/populate03.png)](outline-view-images/populate03.png#lightbox)

В этой статье рассматриваются основные принципы работы с представлениями структуры в приложении Xamarin. Mac. Мы настоятельно рекомендуем сначала ознакомиться со статьей [Hello, Mac](~/mac/get-started/hello-mac.md) , в частности [Знакомство с Xcode и Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) , а также с разделом "возможности [и действия](~/mac/get-started/hello-mac.md#outlets-and-actions) ", так как в нем рассматриваются основные понятия и методы, которые мы будем использовать в этой статье.

Возможно, вы захотите ознакомиться с [предоставлением классов и методов C# для цели-c](~/mac/internals/how-it-works.md) в документе о внутренних компонентах [Xamarin. Mac](~/mac/internals/how-it-works.md) . здесь также объясняются `Register` команды и, `Export` используемые для подключения классов c# к объектам и элементам пользовательского интерфейса на языке c.

<a name="Introduction_to_Outline_Views"></a>

## <a name="introduction-to-outline-views"></a>Общие сведения о представлениях структуры

Представление структуры — это тип таблицы, позволяющий пользователю разворачивать или сворачивать строки иерархических данных. Как и табличное представление, в представлении структуры отображаются данные для набора связанных элементов, строки которых представляют отдельные элементы и столбцы, представляющие атрибуты этих элементов. В отличие от табличного представления элементы в представлении структуры не расположены в плоском списке, они организованы в виде иерархии, например файлов и папок на жестком диске.

Если элемент в режиме структуры содержит другие элементы, он может быть развернут или свернут пользователем. Развертываемый элемент отображает треугольник раскрытия, который указывает вправо, когда элемент свернут, и указывает на то, что элемент развернут. Щелчок на треугольнике раскрытия приводит к тому, что элемент разворачивается или сворачивается.

Представление структуры ( `NSOutlineView` ) является подклассом табличного представления ( `NSTableView` ) и, как таковое, наследует большую часть его поведения от родительского класса. В результате многие операции, поддерживаемые табличным представлением, такие как выбор строк или столбцов, изменение расположения столбцов путем перетаскивания заголовков столбцов и т. д., также поддерживаются в режиме структуры. Приложение Xamarin. Mac управляет этими функциями и может настраивать параметры представления структуры (в коде или Interface Builder), чтобы разрешить или запретить определенные операции.

В режиме структуры не сохраняются собственные данные, вместо этого используется источник данных ( `NSOutlineViewDataSource` ) для предоставления требуемых строк и столбцов по мере необходимости.

Поведение представления структуры можно настроить путем предоставления подкласса делегата представления структуры ( `NSOutlineViewDelegate` ) для поддержки управления столбцами структуры, ввода для выбора функциональных возможностей, выбора строк и редактирования, настраиваемого отслеживания и пользовательских представлений для отдельных столбцов и строк.

Так как представление структуры использует большую часть поведения и функций в табличном представлении, перед продолжением работы с этой статьей может потребоваться перейти к документации по [табличным представлениям](~/mac/user-interface/table-view.md) .

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode"></a>

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>Создание и обслуживание представлений структуры в Xcode

При создании нового приложения Xamarin. Mac Cocoa по умолчанию получается стандартное пустое окно. Эти окна определяются в файле, который `.storyboard` автоматически включается в проект. Чтобы изменить структуру Windows, в **Обозреватель решений** дважды щелкните `Main.storyboard` файл:

[![Выбор основной раскадровки](outline-view-images/edit01.png)](outline-view-images/edit01.png#lightbox)

Это приведет к открытию структуры окна в Interface Builder Xcode:

[![Изменение пользовательского интерфейса в Xcode](outline-view-images/edit02.png)](outline-view-images/edit02.png#lightbox)

Введите `outline` в поле поиска **инспектора библиотеки** , чтобы упростить поиск элементов управления в режиме структуры:

[![Выбор представления структуры из библиотеки](outline-view-images/edit03.png)](outline-view-images/edit03.png#lightbox)

Перетащите представление структуры на контроллер представления в **редакторе интерфейса**, сделайте его частью области содержимого контроллера представления и установите его в то место, где оно сжимается и растет с окном в **редакторе ограничений**:

[![Изменение ограничений](outline-view-images/edit04.png)](outline-view-images/edit04.png#lightbox)

Выберите представление структуры в **иерархии интерфейс** , и в **инспекторе атрибутов** доступны следующие свойства:

[![На снимке экрана показаны свойства, доступные в инспекторе атрибутов.](outline-view-images/edit05.png)](outline-view-images/edit05.png#lightbox)

- **Столбец структуры** — столбец таблицы, в котором отображаются иерархические данные.
- **Автосохранение столбца структуры** — если `true` столбец структуры будет автоматически сохранен и восстановлен между запусками приложений.
- **Отступ** — Величина отступа для столбцов в развернутом элементе.
- **Отступы следуют за ячейками** — если `true` в качестве отступа будет использоваться отступ вместе с ячейками.
- **Автосохранение расширенных элементов** `true` . при этом развернутое или свернутое состояние элементов будет автоматически сохранено и восстановлено между запусками приложений.
- **Режим содержимого** . позволяет использовать представления ( `NSView` ) или ячейки ( `NSCell` ) для отображения данных в строках и столбцах. Начиная с macOS 10,7, следует использовать представления.
- **Строки группы с плавающей запятой** — если `true` в табличном представлении будут отображаться сгруппированные ячейки, как если бы они были плавающими.
- **Columns (столбцы** ) — определяет количество отображаемых столбцов.
- **Headers** — если `true` столбцы будут иметь заголовки.
- **Изменение порядка** — если `true` пользователь сможет переупорядочить столбцы в таблице.
- **Изменение размера** — если `true` пользователь будет иметь возможность перетаскивать заголовки столбцов для изменения размера столбцов.
- **Изменение размера столбца** — управляет тем, как таблица будет иметь автоматический размер столбцов.
- **Выделение** — управляет типом выделения, используемым таблицей при выборе ячейки.
- **Альтернативные строки** — если `true` , когда-нибудь другая строка, будет иметь другой цвет фона.
- **Горизонтальная сетка** — выбор типа границы, нарисованной между ячейками по горизонтали.
- **Вертикальная сетка** — выбор типа границы, нарисованной между ячейками по вертикали.
- **Цвет сетки** — задает цвет границы ячейки.
- **Фон** — задает цвет фона ячейки.
- **Выбор** — позволяет контролировать, как пользователь может выбирать ячейки в таблице следующим образом:
  - **Несколько** — если `true` пользователь может выбрать несколько строк и столбцов.
  - **Столбец** — если `true` пользователь может выбрать столбцы.
  - **Введите SELECT** — если `true` пользователь может ввести символ для выбора строки.
  - **Пусто** — если `true` пользователю не нужно выбирать строку или столбец, в таблице не допускается выбор.
- **Автосохранение** — имя, в котором автоматически сохраняется формат таблиц.
- **Сведения о столбце** — если `true` значение равно, порядок и ширина столбцов будут сохраняться автоматически.
- **Разрывы строк** — выберите, как ячейка обрабатывает разрывы строк.
- **Усекает последнюю видимую строку** — если `true` ячейка будет обрезана, данные не будут помещаться в границы.

> [!IMPORTANT]
> Если вы не обслуживаете устаревшее приложение Xamarin. Mac, на основе представлений `NSView` таблиц следует использовать представления структуры, основанные на структуре `NSCell` . `NSCell` считается устаревшим и может не поддерживаться в дальнейшем.

Выберите столбец таблицы в **иерархии интерфейсов** , и в **инспекторе атрибутов** доступны следующие свойства:

[![На снимке экрана показаны свойства, доступные для выбранного столбца таблицы в инспекторе атрибутов.](outline-view-images/edit06.png)](outline-view-images/edit06.png#lightbox)

- **Заголовок** — задает заголовок столбца.
- **Выравнивание** — задает выравнивание текста внутри ячеек.
- **Шрифт заголовка** — выбирает шрифт для текста заголовка ячейки.
- **Ключ сортировки** — ключ, используемый для сортировки данных в столбце. Оставьте пустым, если пользователь не может сортировать этот столбец.
- **Selector** — **действие** , используемое для выполнения сортировки. Оставьте пустым, если пользователь не может сортировать этот столбец.
- **Order** — порядок сортировки данных столбцов.
- **Изменение размера** — выбор типа изменения размера столбца.
- **Редактируемый** — если `true` пользователь может изменять ячейки в таблице на основе ячейки.
- **Hidden** — если `true` Столбец скрыт.

Можно также изменить размер столбца, перетащив его маркер (вертикально на правой стороне столбца) влево или вправо.

Давайте выберем каждый столбец в представлении таблицы и присвоить первому столбцу **заголовок** `Product` , а второй — `Details` .

Выберите табличное представление ( `NSTableViewCell` ) в **иерархии интерфейсов** , и в **инспекторе атрибутов** доступны следующие свойства:

[![На снимке экрана показаны свойства, доступные для выбранной ячейки таблицы в инспекторе атрибутов.](outline-view-images/edit07.png)](outline-view-images/edit07.png#lightbox)

Это все свойства стандартного представления. Кроме того, здесь можно изменить размер строк для этого столбца.

Выберите ячейку представления таблицы (по умолчанию это `NSTextField` ) в **иерархии интерфейсов** , и в **инспекторе атрибутов** доступны следующие свойства:

[![На снимке экрана показаны свойства, доступные для выбранной ячейки табличного представления в инспекторе атрибутов.](outline-view-images/edit08.png)](outline-view-images/edit08.png#lightbox)

У вас будут все свойства стандартного текстового поля, которые будут заданы здесь. По умолчанию для вывода данных для ячейки в столбце используется стандартное текстовое поле.

Выберите табличное представление ( `NSTableFieldCell` ) в **иерархии интерфейсов** , и в **инспекторе атрибутов** доступны следующие свойства:

[![На снимке экрана показаны свойства, доступные для выбранной ячейки табличного представления.](outline-view-images/edit09.png)](outline-view-images/edit09.png#lightbox)

Ниже приведены наиболее важные параметры.

- **Макет** — выберите порядок расположения ячеек в этом столбце.
- **Использует однострочный режим** — если `true` ячейка ограничена одной строкой.
- **Ширина первой структуры среды выполнения** . Если значение равно `true` , то для ячейки будет предпочтительнее задать ширину (вручную или автоматически), если она отображается при первом запуске приложения.
- **Действие** — управляет тем, когда **действие** редактирования отправляется для ячейки.
- **Поведение** — определяет, является ли ячейка выбираемой или изменяемой.
- **Форматированный текст** — если `true` в ячейке может отображаться форматированный текст и стиль текста.
- **Отменить** — если `true` значение равно, ячейка принимает ответственность за выполнение отмены.

Выберите табличное представление ячейки ( `NSTableFieldCell` ) в нижней части столбца таблицы в **иерархии интерфейсов**:

[![Выбор представления ячеек таблицы](outline-view-images/edit11.png)](outline-view-images/edit10.png#lightbox)

Это позволяет изменить представление ячейки таблицы, используемое в качестве базового _шаблона_ для всех ячеек, созданных для данного столбца.

<a name="Adding_Actions_and_Outlets"></a>

### <a name="adding-actions-and-outlets"></a>Добавление действий и розеток

Точно так же, как и любой другой элемент управления пользовательского интерфейса Cocoa, нам нужно предоставить представление структуры, а также столбцы и ячейки в код C# с помощью **действий** и **розеток** (в зависимости от требуемых функций).

Этот процесс одинаков для любого элемента представления структуры, который мы хотим предоставить:

1. Перейдите в **Редактор помощника** и убедитесь, что `ViewController.h` файл выбран:

    [![Выбор правильного файла. h](outline-view-images/edit11.png)](outline-view-images/edit11.png#lightbox)
2. Выберите представление структуры в **иерархии интерфейс**, нажмите клавишу Control и перетащите его в `ViewController.h` файл.
3. Создайте **выход** для представления структуры с именем `ProductOutline` :

    [![На снимке экрана показана розетка с именем Продуктаутлине в инспекторе атрибутов.](outline-view-images/edit13.png)](outline-view-images/edit13.png#lightbox)
4. Создание **розеток** для столбцов таблиц также вызывается `ProductColumn` и `DetailsColumn` :

    [![На снимке экрана показана розетка с именем Детаилсколумн в инспекторе атрибутов.](outline-view-images/edit14.png)](outline-view-images/edit14.png#lightbox)
5. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Далее мы напишем код, отображающий некоторые данные для структуры при запуске приложения.

<a name="Populating_the_Table_View"></a>

## <a name="populating-the-outline-view"></a>Заполнение представления структуры

С представлением структуры, разработанным в Interface Builder и предоставляемых через **розетку**, далее необходимо создать код C# для его заполнения.

Сначала создадим новый `Product` класс для хранения информации по отдельным строкам и группам вспомогательных продуктов. В **Обозреватель решений** щелкните правой кнопкой мыши проект и выберите **Добавить**  >  **новый файл...** Выберите **Общий**  >  **пустой класс**, введите `Product` для **имени** и нажмите кнопку **создать** :

[![Создание пустого класса](outline-view-images/populate01.png)](outline-view-images/populate01.png#lightbox)

Сделайте так, чтобы `Product.cs` файл выглядел следующим образом:

```csharp
using System;
using Foundation;
using System.Collections.Generic;

namespace MacOutlines
{
    public class Product : NSObject
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Computed Properties
        public string Title { get; set;} = "";
        public string Description { get; set;} = "";
        public bool IsProductGroup {
            get { return (Products.Count > 0); }
        }
        #endregion

        #region Constructors
        public Product ()
        {
        }

        public Product (string title, string description)
        {
            this.Title = title;
            this.Description = description;
        }
        #endregion
    }
}
```

Далее необходимо создать подкласс класса `NSOutlineDataSource` для предоставления данных о структуре по запросу. В **Обозреватель решений** щелкните правой кнопкой мыши проект и выберите **Добавить**  >  **новый файл...** Выберите **Общий**  >  **пустой класс**, введите `ProductOutlineDataSource` в качестве **имени** и нажмите кнопку **создать** .

Измените `ProductTableDataSource.cs` файл и сделайте его следующим:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDataSource : NSOutlineViewDataSource
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Constructors
        public ProductOutlineDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override nint GetChildrenCount (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products.Count;
            } else {
                return ((Product)item).Products.Count;
            }

        }

        public override NSObject GetChild (NSOutlineView outlineView, nint childIndex, NSObject item)
        {
            if (item == null) {
                return Products [childIndex];
            } else {
                return ((Product)item).Products [childIndex];
            }

        }

        public override bool ItemExpandable (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products [0].IsProductGroup;
            } else {
                return ((Product)item).IsProductGroup;
            }

        }
        #endregion
    }
}
```

Этот класс имеет хранилище для элементов представления структуры и переопределяет, `GetChildrenCount` чтобы вернуть число строк в таблице. Метод `GetChild` возвращает конкретный родительский или дочерний элемент (в соответствии с запросом в представлении структуры) и `ItemExpandable` определяет указанный элемент как родительский или дочерний.

Наконец, необходимо создать подкласс класса, `NSOutlineDelegate` чтобы обеспечить поведение для нашего контура. В **Обозреватель решений** щелкните правой кнопкой мыши проект и выберите **Добавить**  >  **новый файл...** Выберите **Общий**  >  **пустой класс**, введите `ProductOutlineDelegate` в качестве **имени** и нажмите кнопку **создать** .

Измените `ProductOutlineDelegate.cs` файл и сделайте его следующим:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDelegate : NSOutlineViewDelegate
    {
        #region Constants
        private const string CellIdentifier = "ProdCell";
        #endregion

        #region Private Variables
        private ProductOutlineDataSource DataSource;
        #endregion

        #region Constructors
        public ProductOutlineDelegate (ProductOutlineDataSource datasource)
        {
            this.DataSource = datasource;
        }
        #endregion

        #region Override Methods

        public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)outlineView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Cast item
            var product = item as Product;

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Product":
                view.StringValue = product.Title;
                break;
            case "Details":
                view.StringValue = product.Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

При создании экземпляра класса `ProductOutlineDelegate` мы также передаем экземпляр объекта `ProductOutlineDataSource` , который предоставляет данные для структуры. `GetView`Метод отвечает за возврат представления (данных) для отображения ячейки для столбца и строки предоставления. Если это возможно, для отображения ячейки будет использоваться существующее представление, если не нужно создавать новое представление.

Чтобы заполнить структуру, отредактируйте `MainWindow.cs` файл и сделайте `AwakeFromNib` метод следующим:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create data source and populate
    var DataSource = new ProductOutlineDataSource ();

    var Vegetables = new Product ("Vegetables", "Greens and Other Produce");
    Vegetables.Products.Add (new Product ("Cabbage", "Brassica oleracea - Leaves, axillary buds, stems, flowerheads"));
    Vegetables.Products.Add (new Product ("Turnip", "Brassica rapa - Tubers, leaves"));
    Vegetables.Products.Add (new Product ("Radish", "Raphanus sativus - Roots, leaves, seed pods, seed oil, sprouting"));
    Vegetables.Products.Add (new Product ("Carrot", "Daucus carota - Root tubers"));
    DataSource.Products.Add (Vegetables);

    var Fruits = new Product ("Fruits", "Fruit is a part of a flowering plant that derives from specific tissues of the flower");
    Fruits.Products.Add (new Product ("Grape", "True Berry"));
    Fruits.Products.Add (new Product ("Cucumber", "Pepo"));
    Fruits.Products.Add (new Product ("Orange", "Hesperidium"));
    Fruits.Products.Add (new Product ("Blackberry", "Aggregate fruit"));
    DataSource.Products.Add (Fruits);

    var Meats = new Product ("Meats", "Lean Cuts");
    Meats.Products.Add (new Product ("Beef", "Cow"));
    Meats.Products.Add (new Product ("Pork", "Pig"));
    Meats.Products.Add (new Product ("Veal", "Young Cow"));
    DataSource.Products.Add (Meats);

    // Populate the outline
    ProductOutline.DataSource = DataSource;
    ProductOutline.Delegate = new ProductOutlineDelegate (DataSource);

}
```

При запуске приложения отображается следующее:

[![Свернутое представление](outline-view-images/populate02.png)](outline-view-images/populate02.png#lightbox)

Если развернуть узел в режиме структуры, он будет выглядеть следующим образом:

[![Развернутое представление](outline-view-images/populate03.png)](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column"></a>

## <a name="sorting-by-column"></a>Сортировка по столбцу

Давайте разрешите пользователю сортировать данные в структуре, щелкая заголовок столбца. Сначала дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в Interface Builder. Выберите `Product` столбец, введите `Title` для **ключа сортировки** `compare:` для **селектора** и выберите `Ascending` для **заказа**:

[![Задание порядка ключей сортировки](outline-view-images/sort01.png)](outline-view-images/sort01.png#lightbox)

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Теперь изменим `ProductOutlineDataSource.cs` файл и добавим следующие методы:

```csharp
public void Sort(string key, bool ascending) {

    // Take action based on key
    switch (key) {
    case "Title":
        if (ascending) {
            Products.Sort ((x, y) => x.Title.CompareTo (y.Title));
        } else {
            Products.Sort ((x, y) => -1 * x.Title.CompareTo (y.Title));
        }
        break;
    }
}

public override void SortDescriptorsChanged (NSOutlineView outlineView, NSSortDescriptor[] oldDescriptors)
{
    // Sort the data
    Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
    outlineView.ReloadData ();
}
```

`Sort`Метод позволяет нам сортировать данные в источнике данных на основе заданного `Product` поля класса в порядке возрастания или убывания. Переопределенный `SortDescriptorsChanged` метод будет вызываться при каждом щелчке по заголовку столбца. Ему будет передано значение **ключа** , установленное в Interface Builder, и порядок сортировки для этого столбца.

Если запустить приложение и щелкнуть в заголовках столбцов, строки будут отсортированы по этому столбцу:

[![Пример отсортированного вывода](outline-view-images/sort02.png)](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection"></a>

## <a name="row-selection"></a>Выбор строк

Если вы хотите разрешить пользователю выбирать одну строку, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в Interface Builder. Выберите режим структуры в **иерархии интерфейсов** и снимите флажок **несколько** в **инспекторе атрибутов**:

[![На снимке экрана показан инспектор атрибутов, в котором можно изменить несколько параметров.](outline-view-images/select01.png)](outline-view-images/select01.png#lightbox)

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Затем измените `ProductOutlineDelegate.cs` файл и добавьте следующий метод:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Это позволит пользователю выбрать одну строку в режиме структуры. Вернитесь к `false` `ShouldSelectItem` элементу для любого элемента, который пользователь не должен иметь возможность выбирать или `false` для каждого элемента, если вы не хотите, чтобы пользователь мог выбрать какие бы то ни было элементы.

<a name="Multiple_Row_Selection"></a>

## <a name="multiple-row-selection"></a>Выбор нескольких строк

Если вы хотите разрешить пользователю выбирать несколько строк, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в Interface Builder. Выберите режим структуры в **иерархии интерфейс** и установите флажок **несколько** в **инспекторе атрибутов**:

[![На снимке экрана показан инспектор атрибутов, в котором можно выбрать несколько.](outline-view-images/select02.png)](outline-view-images/select02.png#lightbox)

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Затем измените `ProductOutlineDelegate.cs` файл и добавьте следующий метод:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Это позволит пользователю выбрать одну строку в режиме структуры. Вернитесь к `false` `ShouldSelectRow` элементу для любого элемента, который пользователь не должен иметь возможность выбирать или `false` для каждого элемента, если вы не хотите, чтобы пользователь мог выбрать какие бы то ни было элементы.

<a name="Type_to_Select_Row"></a>

## <a name="type-to-select-row"></a>Введите, чтобы выбрать строку

Если вы хотите разрешить пользователю вводить символ с выбранным режимом структуры и выбрать первую строку с этим символом, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в Interface Builder. Выберите режим структуры в **иерархии интерфейс** и установите флажок **тип SELECT** в **инспекторе атрибутов**:

[![Изменение типа строки](outline-view-images/type01.png)](outline-view-images/type01.png#lightbox)

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Теперь изменим `ProductOutlineDelegate.cs` файл и добавим следующий метод:

```csharp
public override NSObject GetNextTypeSelectMatch (NSOutlineView outlineView, NSObject startItem, NSObject endItem, string searchString)
{
    foreach(Product product in DataSource.Products) {
        if (product.Title.Contains (searchString)) {
            return product;
        }
    }

    // Not found
    return null;
}
```

`GetNextTypeSelectMatch`Метод принимает заданный объект `searchString` и возвращает элемент первой `Product` строки, в которой находится эта строка `Title` .

<a name="Reordering_Columns"></a>

## <a name="reordering-columns"></a>Изменение порядка столбцов

Если вы хотите разрешить пользователю перетаскивать столбцы переупорядочения в режиме структуры, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в Interface Builder. Выберите режим структуры в **иерархии интерфейс** и установите флажок **изменить порядок** в **инспекторе атрибутов**:

[![На снимке экрана показан инспектор атрибутов, в котором можно выбрать Переупорядочение.](outline-view-images/reorder01.png)](outline-view-images/reorder01.png#lightbox)

Если присвоить значение свойству **автосохранения** и проверить поле **сведения о столбце** , все изменения, внесенные в макет таблицы, будут автоматически сохранены для нас и восстановлены при следующем запуске приложения.

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Теперь изменим `ProductOutlineDelegate.cs` файл и добавим следующий метод:

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder`Метод должен возвращать значение `true` для любого столбца, который требуется перетаскивать в `newColumnIndex` , иначе возвращает `false` .

При запуске приложения можно перетаскивать заголовки столбцов для изменения порядка наших столбцов:

[![Пример переупорядочения столбцов](outline-view-images/reorder02.png)](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells"></a>

## <a name="editing-cells"></a>Редактирование ячеек

Если вы хотите разрешить пользователю изменять значения для данной ячейки, измените `ProductOutlineDelegate.cs` файл и измените `GetViewForItem` метод следующим образом:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTextField view = (NSTextField)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTextField ();
        view.Identifier = tableColumn.Title;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.StringValue;
            break;
        case "Details":
            prod.Description = view.StringValue;
            break;
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.StringValue = product.Title;
        break;
    case "Details":
        view.StringValue = product.Description;
        break;
    }

    return view;
}
```

Теперь, если приложение запускается, пользователь может изменять ячейки в представлении таблицы:

[![Пример редактирования ячеек](outline-view-images/editing01.png)](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views"></a>

## <a name="using-images-in-outline-views"></a>Использование изображений в представлениях структуры

Чтобы включить изображение как часть ячейки в `NSOutlineView` , необходимо изменить способ возврата данных методом представления структуры, `NSTableViewDelegate's` `GetView` чтобы использовать `NSTableCellView` вместо типичного `NSTextField` . Вот несколько примеров.

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.TextField.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.TextField.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.TextField.StringValue;
            break;
        case "Details":
            prod.Description = view.TextField.StringValue;
            break;
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed (product.IsProductGroup ? "tags.png" : "tag.png");
        view.TextField.StringValue = product.Title;
        break;
    case "Details":
        view.TextField.StringValue = product.Description;
        break;
    }

    return view;
}
```

Дополнительные сведения см. в разделе [Использование образов с представлениями структуры](~/mac/app-fundamentals/image.md) в документации по [работе с изображением](~/mac/app-fundamentals/image.md) .

<a name="Data_Binding_Outline_Views"></a>

## <a name="data-binding-outline-views"></a>Представления структуры привязки данных

С помощью Key-Value кодирования и методов привязки данных в приложении Xamarin. Mac можно значительно сократить объем кода, который необходимо писать и поддерживать для заполнения и работы с элементами пользовательского интерфейса. Кроме того, у вас есть преимущество в дальнейшем разделять резервные данные (_модель данных_) от интерфейса пользователя переднего плана (_модель-представление-контроллер_), что упрощает обслуживание, более гибкое проектирование приложений.

Key-Value кодирование (КВК) — это механизм неявного доступа к свойствам объекта с помощью клавиш (специально отформатированных строк) для обнаружения свойств вместо доступа к ним через переменные экземпляра или методы доступа ( `get/set` ). Реализуя Key-Value совместимые с кодированием методы доступа в приложении Xamarin. Mac, вы получаете доступ к другим macOS функциям, таким как Key-Value наблюдение (кво), привязка данных, основные данные, привязки Cocoa и сценарии.

Дополнительные сведения см. в разделе [Привязка данных в режиме структуры](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding) [привязки данных и Key-Value документации по написанию кода](~/mac/app-fundamentals/databinding.md) .

<a name="Summary"></a>

## <a name="summary"></a>Сводка

В этой статье подробно рассматривается работа с представлениями структуры в приложении Xamarin. Mac. Мы видели различные типы и использование представлений структуры, как создавать и поддерживать представления структуры в Interface Builder Xcode и как работать с представлениями структуры в коде C#.

## <a name="related-links"></a>Связанные ссылки

- [Макаутлинес (пример)](/samples/xamarin/mac-samples/macoutlines)
- [MacImages (пример)](/samples/xamarin/mac-samples/macimages)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Представления таблиц](~/mac/user-interface/table-view.md)
- [Списки источников](~/mac/user-interface/source-list.md)
- [Привязка данных и кодирование типа "ключ — значение"](~/mac/app-fundamentals/databinding.md)
- [Рекомендации по работе с человеческим интерфейсом OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Общие сведения о представлениях структуры](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [нсаутлиневиев](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [нсаутлиневиевдатасаурце](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [нсаутлиневиевделегате](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)