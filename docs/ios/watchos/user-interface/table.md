---
title: Элементы управления таблицы watchOS в Xamarin
description: В этом документе описывается, как использовать элементы управления таблицы watchOS в Xamarin. В нем обсуждается добавление таблицы, Добавление контроллера строк, создание и заполнение строк, реагирование на касания и многое другое.
ms.prod: xamarin
ms.assetid: 7C14126D-9591-4387-A588-3C4521F11C55
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 015f0732e4d8cdf771af3e1d0b3cc3e31b6e806c
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84572273"
---
# <a name="watchos-table-controls-in-xamarin"></a>Элементы управления таблицы watchOS в Xamarin

`WKInterfaceTable`Элемент управления watchOS гораздо проще, чем его аналог в iOS, но выполняет аналогичную роль. Он создает прокручиваемый список строк, которые могут иметь пользовательские макеты и реагировать на события касания.

![](table-images/table-list-sml.png "Список отслеживаемых таблиц") ![](table-images/table-detail-sml.png)
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="adding-a-table"></a>Добавление таблицы

Перетащите элемент управления **Table** в сцену. По умолчанию он будет выглядеть следующим образом (отображая одну неопределенную структуру строк):

[![](table-images/add-table-sml.png "Adding a table")](table-images/add-table.png#lightbox)

Присвойте таблице имя в поле **имя** панели **свойств** , чтобы на нее можно было ссылаться в коде.

## <a name="add-a-row-controller"></a>Добавление контроллера строк

Таблица автоматически включает одну строку, представленную контроллером строк, который по умолчанию содержит элемент управления " **Группа** ".

Чтобы задать **класс** для контроллера строк, выберите строку в **структуре документа** и введите имя класса на панели **свойств** :

[![](table-images/add-row-controller-sml.png "Entering a class name in the Properties pad")](table-images/add-row-controller.png#lightbox)

После установки класса для контроллера строки интегрированная среда разработки создаст в проекте соответствующий файл C#. Перетащите элементы управления (например, метки) в строку и присвойте им имена, чтобы на них можно было ссылаться в коде.

## <a name="create-and-populate-rows"></a>Создание и заполнение строк

`SetNumberOfRows`создает классы контроллеров строк для каждой строки, используя для выбора нужного для нее класса `Identifier` . Если вы назначаете свой контроллер строк настраиваемому `Identifier` , измените **значение по умолчанию** в следующем фрагменте кода на используемый идентификатор. `RowController` *Для каждой строки* создается при `SetNumberOfRows` вызове и отображении таблицы.

```csharp
myTable.SetNumberOfRows ((nint)rows.Count, "default");
    // loads row controller by identifier
```

> [!IMPORTANT]
> Строки таблицы не виртуализованы, как в iOS. Попробуйте ограничить число строк (Apple рекомендует меньше 20).

После создания строк необходимо заполнить каждую ячейку (например `GetCell` , в iOS). Этот фрагмент кода из [примера ватчтаблес](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchtables) обновляет метку в каждой строке.

```csharp
for (var i = 0; i < rows.Count; i++) {
    var elementRow = (RowController)myTable.GetRowController (i);
    elementRow.myRowLabel.SetText (rows [i]);
}
```

> [!IMPORTANT]
> Использование `SetNumberOfRows` и последующее выполнение цикла по использованию `GetRowController` приводит к тому, что вся таблица будет отправлена в контрольное значение. В последующих представлениях таблицы, если необходимо добавить или удалить определенные строки, используйте `InsertRowsAt` `RemoveRowsAt` для повышения производительности.

## <a name="respond-to-taps"></a>Реагирование на касания

Вы можете ответить на выбор строк двумя разными способами:

- Реализуйте `DidSelectRow` метод в контроллере интерфейса или
- Создайте перехода для раскадровки и реализуйте, `GetContextForSegue` если хотите, чтобы выбор строк открывал другую сцену.

### <a name="didselectrow"></a>дидселектров

Чтобы программно обрабатывал выбор строк, реализуйте `DidSelectRow` метод. Чтобы открыть новую сцену, используйте `PushController` и передайте идентификатор сцены и используемый контекст данных:

```csharp
public override void DidSelectRow (WKInterfaceTable table, nint rowIndex)
{
    var rowData = rows [(int)rowIndex];
    Console.WriteLine ("Row selected:" + rowData);
    // if selection should open a new scene
    PushController ("secondInterface", rows[(int)rowIndex]);
}
```

### <a name="getcontextforsegue"></a>жетконтекстфорсегуе

Перетащите перехода на раскадровку из строки таблицы в другую сцену (удерживайте нажатой клавишу **CTRL** при перетаскивании).
Обязательно выберите перехода и присвойте ему идентификатор на панели **свойств** (например, `secondLevel` в примере ниже).

В контроллере интерфейса реализуйте `GetContextForSegue` метод и верните контекст данных, который должен быть предоставлен для сцены, представленной перехода.

```csharp
public override NSObject GetContextForSegue (string segueIdentifier, WKInterfaceTable table, nint rowIndex)
{
    if (segueIdentifier == "secondLevel") {
        return new NSString (rows[(int)rowIndex]);
    }
    return null;
}
```

Эти данные передаются в целевую сцену раскадровки в `Awake` методе.

## <a name="multiple-row-types"></a>Несколько типов строк

По умолчанию элемент управления "Таблица" имеет один тип строки, который можно спроектировать. Чтобы добавить дополнительные строки "Templates", используйте поле " **строки** " на панели **свойств** , чтобы создать дополнительные контроллеры строк.

![](table-images/prototype-rows1.png "Setting the number of Prototype rows")

Если задать для свойства **Rows** значение **3** , будут созданы дополнительные заполнители для перетаскивания элементов управления в. Для каждой строки задайте имя **класса** на панели **свойств** , чтобы обеспечить создание класса контроллера строк.

![](table-images/prototype-rows2.png "The prototype rows in the designer")

Чтобы заполнить таблицу с разными типами строк, используйте метод, `SetRowTypes` чтобы указать тип контроллера строки, используемый для каждой строки в таблице. Используйте идентификаторы строк, чтобы указать, какой контроллер строки следует использовать для каждой строки.

Число элементов в этом массиве должно соответствовать числу строк, которые должны находиться в таблице:

```csharp
myTable.SetRowTypes (new [] {"type1", "default", "default", "type2", "default"});
```

При заполнении таблицы с несколькими контроллерами строк необходимо отследить, какой тип предполагается использовать при заполнении пользовательского интерфейса:

```csharp
for (var i = 0; i < rows.Count; i++) {
    if (i == 0) {
        var elementRow = (Type1RowController)myTable.GetRowController (i);
        // populate UI controls
    } else if (i == 3) {
        var elementRow = (Type2RowController)myTable.GetRowController (i);
        // populate UI controls
    } else {
        var elementRow = (DefaultRowController)myTable.GetRowController (i);
        // populate UI controls
    }
}
```

## <a name="vertical-detail-paging"></a>Разбиение на страницы по вертикали

в watchOS 3 появилась новая функция для таблиц: возможность прокрутки страниц сведений, связанных с каждой строкой, без возврата к таблице и выбора другой строки. Экраны сведений можно прокручивать путем прокрутки вверх или вниз или с помощью Digital Crown.

![](table-images/table-scroll-sml.png "Пример вертикального разбиения на страницы") ![](table-images/table-detail-sml.png)

> [!IMPORTANT]
> Эта функция в настоящее время доступна только путем редактирования раскадровки в Xcode Interface Builder.

Чтобы включить эту функцию, выберите элемент в `WKInterfaceTable` области конструктора и установите флажок **вертикальная детализация разбиения на страницы** :

![](table-images/vertical-detail-paging-sml.png "Selecting the Vertical Detail Paging option")

Как [описано в Apple](https://developer.apple.com/reference/watchkit/wkinterfacetable#1682023) , для работы функции разбиения по страницам необходимо использовать переходов. Перепишите любой существующий код, использующий `PushController` вместо него переходов.

<a name="add_row_controller"></a>

## <a name="appendix-row-controller-code-example"></a>Приложение. пример кода контроллера строки

Интегрированная среда разработки автоматически создает два файла кода при создании контроллера строк в конструкторе. Код в этих созданных файлах приведен ниже для справки.

Первый будет называться для класса, например **RowController.CS**, следующим образом:

```csharp
using System;
using Foundation;

namespace WatchTablesExtension
{
    public partial class RowController : NSObject
    {
        public RowController ()
        {
        }
    }
}
```

Другой **Designer.CS** -файл — это частичное определение класса, которое содержит сведения о параметрах и действиях, создаваемых на поверхности конструктора, например в этом примере с одним `WKInterfaceLabel` элементом управления:

```csharp
using Foundation;
using System;
using System.CodeDom.Compiler;
using UIKit;

namespace WatchTables.OnWatchExtension
{
    [Register ("RowController")]
    partial class RowController
    {
        [Outlet]
        [GeneratedCode ("iOS Designer", "1.0")]
        public WatchKit.WKInterfaceLabel MyLabel { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (MyLabel != null) {
                MyLabel.Dispose ();
                MyLabel = null;
            }
        }
    }
}
```

В коде можно ссылаться на приведенные здесь возможности и действия, однако **Designer.CS** -файл не должен редактироваться напрямую.

## <a name="related-links"></a>Связанные ссылки

- [Ватчтаблес (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchtables)
- [Ватчкиткаталог (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Doc в таблице Apple](https://developer.apple.com/reference/watchkit/wkinterfacetable)
