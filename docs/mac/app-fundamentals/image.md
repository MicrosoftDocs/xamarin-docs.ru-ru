---
title: Образы в Xamarin. Mac
description: В этой статье рассматривается работа с изображениями и значками в приложении Xamarin. Mac. Здесь описывается создание и обслуживание образов, необходимых для создания значка приложения, и использование изображений в коде C# и в Interface Builder Xcode.
ms.prod: xamarin
ms.assetid: C6B539C2-FC6A-4C38-B839-32BFFB9B16A7
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/15/2017
ms.openlocfilehash: b6681832ce83bbc2783ba874c5902c30013c8c94
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84572563"
---
# <a name="images-in-xamarinmac"></a>Образы в Xamarin. Mac

_В этой статье рассматривается работа с изображениями и значками в приложении Xamarin. Mac. Здесь описывается создание и обслуживание образов, необходимых для создания значка приложения, и использование изображений в коде C# и в Interface Builder Xcode._

## <a name="overview"></a>Обзор

При работе с C# и .NET в приложении Xamarin. Mac у вас есть доступ к тем же изображениям и инструментам значков, что и разработчик, работающий на *уровне цели-C* и *Xcode* .

Существует несколько способов использования ресурсов изображения внутри приложения macOS (ранее известного как Mac OS X). С помощью простого отображения изображения в пользовательском интерфейсе приложения и присвоения его элементу управления пользовательского интерфейса, например панели инструментов или элемента списка источников, для предоставления значков Xamarin. Mac упрощает добавление в приложения macOS отличных иллюстраций следующими способами: 

- **Элементы пользовательского интерфейса** — изображения могут отображаться в виде фона или как часть приложения в представлении изображений ( `NSImageView` ).
- **Кнопки** — изображения могут отображаться в кнопках ( `NSButton` ).
- **Ячейка изображения** — как часть элемента управления на основе таблицы ( `NSTableView` или `NSOutlineView` ), изображения можно использовать в ячейке изображения ( `NSImageCell` ).
- **Элемент панели инструментов** — изображения можно добавлять на панель инструментов ( `NSToolbar` ) как элемент панели инструментов изображения ( `NSToolbarItem` ).
- **Значок исходного списка** — как часть исходного списка (в специально отформатированном виде `NSOutlineView` ).
- **Значок приложения** — последовательность изображений может быть объединена в `.icns` набор и использоваться в качестве значка приложения. Дополнительные сведения см. в документации по [значку приложения](~/mac/deploy-test/app-icon.md) .

Кроме того, macOS предоставляет набор предопределенных образов, которые можно использовать во всем приложении.

[![Пример выполнения приложения](image-images/intro01.png "Пример выполнения приложения")](image-images/intro01-large.png#lightbox)

В этой статье рассматриваются основные принципы работы с изображениями и значками в приложении Xamarin. Mac. Мы настоятельно рекомендуем сначала ознакомиться со статьей [Hello, Mac](~/mac/get-started/hello-mac.md) , в частности [Знакомство с Xcode и Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) , а также с разделом "возможности [и действия](~/mac/get-started/hello-mac.md#outlets-and-actions) ", так как в нем рассматриваются основные понятия и методы, которые мы будем использовать в этой статье.

## <a name="adding-images-to-a-xamarinmac-project"></a>Добавление изображений в проект Xamarin. Mac

При добавлении образа для использования в приложении Xamarin. Mac существует несколько мест и способов, которые разработчик может включить файл изображения в источник проекта:

- **Главное дерево проекта [устарело]** — изображения можно добавлять непосредственно в дерево проектов. При вызове изображений, хранящихся в главном дереве проектов, из кода расположение папки не указывается. Например: `NSImage image = NSImage.ImageNamed("tags.png");`. 
- **Папка Resources [устарела]** . папка "специальные **ресурсы** " предназначена для любого файла, который станет частью пакета приложения, например значка, экрана запуска или общих изображений (или любого другого изображения или файла, которые разработчик хочет добавить). При вызове образов, хранящихся в папке **ресурсов** из кода, как и в изображениях, хранящихся в главном дереве проектов, расположение папки не указывается. Например: `NSImage.ImageNamed("tags.png")`.
- **Пользовательская папка или подпапка [устарела]** — разработчик может добавить пользовательскую папку в дерево исходного кода проектов и сохранить в нем образы. Расположение, в которое добавляется файл, может быть вложено во вложенную папку для дальнейшей помощи в упорядочении проекта. Например, если разработчик добавил в `Card` проект папку и вложенную папку `Hearts` в этой папке, сохраните образ **гнездо. png** в `Hearts` папке, чтобы `NSImage.ImageNamed("Card/Hearts/Jack.png")` загрузить образ во время выполнения.
- **Наборы образов каталога активов [Предпочитаемые]** — ДОБАВЛЕНЫ в OS X El Capitan, **наборы образов каталогов активов** содержат все версии или представления образа, необходимые для поддержки различных устройств и коэффициентов масштабирования вашего приложения. Вместо того, чтобы полагаться на имя файла ресурсов образа ( **@1x** , **@2x** ).

<a name="asset-catalogs"></a>

### <a name="adding-images-to-an-asset-catalog-image-set"></a>Добавление изображений в набор изображений каталога активов

Как указано выше, **наборы изображений каталога активов** содержат все версии или представления образа, необходимые для поддержки различных устройств и коэффициентов масштабирования для приложения. Вместо того, чтобы полагаться на имя файла ресурсов образа (см. сведения о независимых изображениях и образах), **наборы образов** используют редактор ресурсов для указания того, какой образ относится к какому устройству и (или) разрешению.

1. В **панель решения**дважды щелкните файл **Assets. xcassets** , чтобы открыть его для редактирования: 

    ![Выбор ресурсов. xcassets](image-images/imageset01.png "Выбор ресурсов. xcassets")
2. Щелкните правой кнопкой мыши **список активы** и выберите **создать набор изображений**: 

    [![Добавление нового набора изображений](image-images/imageset02.png "Добавление нового набора изображений")](image-images/imageset02-large.png#lightbox)
3. Выберите новый набор изображений, и откроется редактор: 

    [![Выбор нового набора изображений](image-images/imageset03.png "Выбор нового набора изображений")](image-images/imageset03-large.png#lightbox)
4. Здесь можно перетаскивать изображения для каждого из различных устройств и необходимых разрешений. 
5. Дважды щелкните **имя** нового набора изображений в **списке ресурсов** , чтобы изменить его: 

    [![Изменение имени набора изображений](image-images/imageset04.png "Изменение имени набора изображений")](image-images/imageset04-large.png#lightbox)
    
Специальный класс **vector** добавлен к **наборам изображений** , что позволяет включать в кассет изображения векторного формата _PDF_ , включая отдельные файлы битовой карты с разными разрешениями. Используя этот метод, вы предоставляете один векторный файл для **@1x** разрешения (в формате векторного PDF-файла), **@2x** а **@3x** версии и файла будут создаваться во время компиляции и включаться в пакет приложения.

[![Интерфейс редактора набора изображений](image-images/imageset05.png "Интерфейс редактора набора изображений")](image-images/imageset05-large.png#lightbox)

Например, если включить `MonkeyIcon.pdf` файл в качестве вектора каталога активов с разрешением 150px x 150px, то в окончательный пакет приложений будут включены следующие ресурсы растрового изображения:

1. **MonkeyIcon@1x.png**-150px x 150px разрешение.
2. **MonkeyIcon@2x.png**-300 пикселей x 300 пикселей разрешение.
3. **MonkeyIcon@3x.png**-450px x 450px разрешение.

При использовании векторных изображений PDF в каталогах активов следует учитывать следующее:

- Это не полная поддержка вектора, так как PDF-файл будет располагаться в растровом изображении во время компиляции и поставляются в окончательном приложении.
- Размер образа нельзя изменить после его настройки в каталоге активов. При попытке изменить размер изображения (в коде или с помощью классов автоматической разметки и размера) изображение будет искажено точно так же, как и любой другой точечный рисунок.

При использовании **набора изображений** в Interface Builder Xcode можно просто выбрать имя набора из раскрывающегося списка в **инспекторе атрибутов**:

![Выбор набора изображений в Interface Builder Xcode](image-images/imageset06.png "Выбор набора изображений в Interface Builder Xcode")

<a name="Adding-new-Assets-Collections"></a>

### <a name="adding-new-assets-collections"></a>Добавление новых коллекций активов

При работе с изображениями в каталогах активов иногда требуется создать новую коллекцию вместо добавления всех изображений в коллекцию **Assets. xcassets** . Например, при проектировании ресурсов по запросу.

Чтобы добавить новый каталог ресурсов в проект, выполните следующие действия.

1. Щелкните правой кнопкой мыши проект в **панель решения** и выберите **Добавить**  >  **новый файл...**
2. Выберите **Mac**  >  **Каталог ресурсов**Mac, введите **имя** коллекции и нажмите кнопку **создать** : 

    ![Добавление нового каталога активов](image-images/asset01.png "Добавление нового каталога активов")

Отсюда можно работать с коллекцией таким же образом, как и коллекция **Assets. xcassets** по умолчанию, которая автоматически включается в проект.

### <a name="adding-images-to-resources"></a>Добавление изображений в ресурсы

> [!IMPORTANT]
> Этот метод работы с образами в приложении macOS устарел с помощью Apple. Вместо этого следует использовать [наборы образов каталога активов](#asset-catalogs) для управления образами приложения.

Прежде чем использовать файл изображения в приложении Xamarin. Mac (в коде C# или Interface Builder), его необходимо добавить в папку **ресурсов** проекта в качестве **ресурса пакета**. Чтобы добавить файл в проект, выполните следующие действия.

1. Щелкните правой кнопкой мыши папку **ресурсов** в проекте в **панель решения** и выберите **добавить**  >  **Добавить файлы...**: 

    ![Добавление файла](image-images/add01.png "Добавление файла")
2. В диалоговом окне **Добавление файлов** выберите файлы изображений для добавления в проект, выберите `BundleResource` для **действия переопределение сборки** и нажмите кнопку **Открыть** :

    [![Выбор файлов для добавления](image-images/add02.png "Выбор файлов для добавления")](image-images/add02-large.png#lightbox)
3. Если файлы еще не находятся в папке **Resources** , вам будет предложено выполнить **копирование**, **Перемещение** или **связывание** файлов. Выберите каждую из ваших потребностей, обычно которая будет **копироваться**:

    ![Выбор действия "Добавить"](image-images/add04.png "Выбор действия "Добавить"")
4. Новые файлы будут добавлены в проект и прочитаны для использования: 

    ![Новые файлы изображений, добавленные в Панель решения](image-images/add03.png "Новые файлы изображений, добавленные в Панель решения")
5. Повторите эту процедуру для всех необходимых файлов изображений.

Вы можете использовать любой файл PNG, JPG или PDF в качестве исходного изображения в приложении Xamarin. Mac. В следующем разделе мы рассмотрим добавление версий изображений и значков с высоким разрешением для поддержки компьютеров Макинтош на основе Retina.

> [!IMPORTANT]
> Если вы добавляете изображения в папку **Resources** , можно оставить для параметра **действие сборки переопределения** значение **по умолчанию**. По умолчанию для этой папки используется действие сборки `BundleResource` .

## <a name="provide-high-resolution-versions-of-all-app-graphics-resources"></a>Предоставление версий всех графических ресурсов приложения с высоким разрешением

Все графические ресурсы, добавляемые в приложение Xamarin. Mac (значки, пользовательские элементы управления, пользовательские курсоры, пользовательские иллюстрации и т. д.), должны иметь версии с высоким разрешением в дополнение к их стандартным версиям разрешения. Это необходимо для того, чтобы приложение было лучше выглядеть при запуске на Retina дисплее компьютера Mac.

### <a name="adopt-the-2x-naming-convention"></a>Принятие @2x соглашения об именовании

> [!IMPORTANT]
> Этот метод работы с образами в приложении macOS устарел с помощью Apple. Вместо этого следует использовать [наборы образов каталога активов](#asset-catalogs) для управления образами приложения.

При создании версий образа Standard и High-Solution следуйте этому соглашению об именовании для пары образов при их включении в проект Xamarin. Mac:

- **Стандартное разрешение**   -  **ImageName. filename — расширение** (например: **Tags. png**)
- **Высокое разрешение**   -  **ImageName@2x.filename-extension** (Пример: **tags@2x.png** )

При добавлении в проект они будут выглядеть следующим образом:

![Файлы изображений в Панель решения](image-images/add03.png "Файлы изображений в Панель решения")

Когда изображение назначается элементу пользовательского интерфейса в Interface Builder вы просто выбираете файл в _imageName_**.** формат _имени файла_ (например: **Tags. png**). То же самое для использования изображения в коде C#, вы выбираете файл в _imageName_**.** формат _имени файла_ .

При запуске приложения Xamarin. Mac на компьютере Mac _imageName_**.** _имя файла —_ изображение формата расширения будет использоваться в стандартном дисплее разрешения, **ImageName@2x.filename-extension** изображение будет автоматически выбрано на Retina отображения компьютеров Макинтош.

## <a name="using-images-in-interface-builder"></a>Использование образов в Interface Builder

Любой ресурс изображения, добавленный в папку **Resources** в проекте Xamarin. Mac и настроил действие сборки **BundleResource** , будет автоматически отображаться в Interface Builder и может быть выбран как часть элемента пользовательского интерфейса (если он обрабатывает изображения).

Чтобы использовать изображение в конструкторе интерфейсов, выполните следующие действия.

1. Добавьте образ в папку **Resources** с **действием сборки** `BundleResource` : 

     ![Ресурс изображения в Панель решения](image-images/ib00.png "Ресурс изображения в Панель решения")
2. Дважды щелкните файл **Main. Storyboard** , чтобы открыть его для редактирования в Interface Builder: 

     [![Изменение основной раскадровки](image-images/ib01.png "Изменение основной раскадровки")](image-images/ib01-large.png#lightbox)
3. Перетащите элемент пользовательского интерфейса, который принимает изображения на поверхность разработки (например, **элемент панели инструментов изображения**): 

     ![Изменение элемента панели инструментов](image-images/ib02.png "Изменение элемента панели инструментов")
4. Выберите изображение, добавленное в папку **ресурсы** в раскрывающемся списке **имя образа** : 

     [![Выбор изображения для элемента панели инструментов](image-images/ib03.png "Выбор изображения для элемента панели инструментов")](image-images/ib03-large.png#lightbox)
5. Выбранное изображение будет отображаться в области конструктора: 

     ![Изображение, отображаемое в редакторе панелей инструментов](image-images/ib04.png "Изображение, отображаемое в редакторе панелей инструментов")
6. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Описанные выше действия выполняются для любого элемента пользовательского интерфейса, который позволяет задать свойство Image в **инспекторе атрибутов**. Опять же, если вы включили **@2x** версию файла изображения, он будет автоматически использоваться на компьютерах Макинтош, отображаемых на Retina.

> [!IMPORTANT]
> Если изображение недоступно в раскрывающемся списке **имя образа** , закройте проект. Storyboard в Xcode и снова откройте его из Visual Studio для Mac. Если образ по-прежнему недоступен, убедитесь, что его **действие сборки** имеет значение `BundleResource` и что изображение Добавлено в папку **Resources** .

## <a name="using-images-in-c-code"></a>Использование изображений в коде C#

При загрузке изображения в память с помощью кода C# в приложении Xamarin. Mac изображение будет храниться в `NSImage` объекте. Если файл изображения был включен в набор приложений Xamarin. Mac (включенный в ресурсы), используйте следующий код для загрузки изображения:

```csharp
NSImage image = NSImage.ImageNamed("tags.png");
```

Приведенный выше код использует статический `ImageNamed("...")` метод `NSImage` класса для загрузки заданного изображения в память из папки **Resources** , если изображение не найдено, `null` будет возвращено. Как и изображения, назначенные в Interface Builder, если вы включили **@2x** версию файла изображения, она будет автоматически использоваться на компьютерах Макинтош, отображаемых на Retina.

Чтобы загрузить изображения за пределами пакета приложения (из файловой системы Mac), используйте следующий код:

```csharp
NSImage image = new NSImage("/Users/KMullins/Documents/photo.jpg")
```

<a name="Working-with-Template-Images"></a>

## <a name="working-with-template-images"></a>Работа с образами шаблонов

В зависимости от структуры приложения macOS могут возникнуть ситуации, когда необходимо настроить значок или изображение в пользовательском интерфейсе в соответствии с изменениями цветовой схемы (например, в зависимости от настроек пользователя).

Чтобы добиться этого результата, переключите _режим рендеринга_ ресурса изображения на **образ шаблона**:

[![Настройка образа шаблона](image-images/templateimage01.png "Настройка образа шаблона")](image-images/templateimage01-large.png#lightbox)

В Interface Builder Xcode назначьте ресурс изображения элементу управления ИП:

![Выбор изображения в Interface Builder Xcode](image-images/templateimage02.png "Выбор изображения в Interface Builder Xcode")

Или при необходимости задайте источник изображения в коде:

```csharp
MyIcon.Image = NSImage.ImageNamed ("MessageIcon");
```

Добавьте следующую общую функцию в контроллер представления:

```csharp
public NSImage ImageTintedWithColor(NSImage sourceImage, NSColor tintColor)
    => NSImage.ImageWithSize(sourceImage.Size, false, rect => {
        // Draw the original source image
        sourceImage.DrawInRect(rect, CGRect.Empty, NSCompositingOperation.SourceOver, 1f);

        // Apply tint
        tintColor.Set();
        NSGraphics.RectFill(rect, NSCompositingOperation.SourceAtop);

        return true;
    });
```

> [!IMPORTANT]
> В частности, с появлением темного режима в macOS Можаве, важно избегать `LockFocus` API при реатинг пользовательских `NSImage` объектов. Такие изображения становятся статическими и не будут автоматически обновляться для учета изменений внешнего вида или плотности отображения.
>
> Применяя механизм, основанный на обработчиках выше, повторная визуализация для динамических условий будет происходить автоматически при `NSImage` размещении, например, в `NSImageView` .

Наконец, чтобы оттенок изображения шаблона, вызовите эту функцию для изображения, чтобы зацветировать:

```csharp
MyIcon.Image = ImageTintedWithColor (MyIcon.Image, NSColor.Red);
```

<a name="Using_Images_with_Table_Views"></a>

## <a name="using-images-with-table-views"></a>Использование изображений с табличными представлениями

Чтобы включить изображение как часть ячейки в `NSTableView` , необходимо изменить способ возврата данных методом табличного представления `NSTableViewDelegate's` `GetViewForItem` для использования `NSTableCellView` вместо типичного `NSTextField` . Пример.

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
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
        view.TextField.Editable = true;

        view.TextField.EditingEnded += (sender, e) => {

            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.TextField.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tags.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

Здесь есть несколько интересующих вас строк. Во-первых, для столбцов, которые нужно включить в изображение, мы создаем новый `NSImageView` требуемый размер и расположение. Мы также создадим новый `NSTextField` и поместим его расположение по умолчанию на основе того, используется ли для этого изображения:

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

Во вторых, необходимо включить новое представление изображения и текстовое поле в родительский элемент `NSTableCellView` :

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

Наконец, нам нужно сообщить текстовому полю, что оно может сжиматься и увеличиваться в ячейке табличного представления:

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Выходные данные примера:

[![Пример отображения изображения в приложении](image-images/tables01.png "Пример отображения изображения в приложении")](image-images/tables01-large.png#lightbox)

Дополнительные сведения о работе с табличными представлениями см. в документации по [табличным представлениям](~/mac/user-interface/table-view.md) .

<a name="Using_Images_with_Outline_Views"></a>

## <a name="using-images-with-outline-views"></a>Использование изображений с представлениями структуры

Чтобы включить изображение как часть ячейки в `NSOutlineView` , необходимо изменить способ возврата данных методом представления структуры, `NSTableViewDelegate's` `GetView` чтобы использовать `NSTableCellView` вместо типичного `NSTextField` . Пример.

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

Здесь есть несколько интересующих вас строк. Во-первых, для столбцов, которые нужно включить в изображение, мы создаем новый `NSImageView` требуемый размер и расположение. Мы также создадим новый `NSTextField` и поместим его расположение по умолчанию на основе того, используется ли для этого изображения:

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

Во вторых, необходимо включить новое представление изображения и текстовое поле в родительский элемент `NSTableCellView` :

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

Наконец, нам нужно сообщить текстовому полю, что оно может сжиматься и увеличиваться в ячейке табличного представления:

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Выходные данные примера:

[![Пример изображения, отображаемого в режиме структуры](image-images/outline01.png "Пример изображения, отображаемого в режиме структуры")](image-images/outline01-large.png#lightbox)

Дополнительные сведения о работе с представлениями структуры см. в документации по [представлениям структуры](~/mac/user-interface/outline-view.md) .

## <a name="summary"></a>Сводка

В этой статье подробно рассматривается работа с изображениями и значками в приложении Xamarin. Mac. Мы видели различные типы и использование изображений, как использовать изображения и значки в Interface Builder Xcode и как работать с изображениями и значками в коде C#.

## <a name="related-links"></a>Связанные ссылки

- [MacImages (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/macimages)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Табличные представления](~/mac/user-interface/table-view.md)
- [Представления структуры](~/mac/user-interface/outline-view.md)
- [Рекомендации по использованию интерфейса пользователя macOS X](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Сведения о высоком разрешении для OS X](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
