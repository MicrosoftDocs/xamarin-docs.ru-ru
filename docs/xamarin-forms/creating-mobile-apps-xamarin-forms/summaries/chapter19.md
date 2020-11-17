---
title: Аннотация к главе 19. Представления коллекций
description: Создание мобильных приложений с помощью Xamarin.Forms. Аннотация к главе 19. Представления коллекций
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 0AEC3A5C-586E-4D0F-9895-67E99A053A79
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9bdab5d64f1edc60ca58993b7848f97b7125023b
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374047"
---
# <a name="summary-of-chapter-19-collection-views"></a>Аннотация к главе 19. Представления коллекций

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)

> [!NOTE]
> Эта книга была опубликована весной 2016 года и с тех пор не обновлялась. Многое в этой книге остается ценным, но некоторые материалы устарели, а некоторые разделы перестали быть полностью верными или полными.

Xamarin.Forms определяет три представления, которые поддерживают коллекции и показывают их элементы:

- [`Picker`](xref:Xamarin.Forms.Picker) — это относительно короткий список строковых элементов, позволяющий пользователю выбрать один из них.
- [`ListView`](xref:Xamarin.Forms.ListView) — часто является длинным списком элементов, у которых обычно одинаковые тип и форматирование, здесь также пользователь может выбрать один из них.
- [`TableView`](xref:Xamarin.Forms.TableView) — это коллекция *ячеек* (обычно различных типов и вида) для отображения данных или управления вводом пользователя.

Обычно приложения MVVM используют `ListView` для отображения доступной для выбора коллекции объектов.

## <a name="program-options-with-picker"></a>Варианты программы с использованием Picker

[`Picker`](xref:Xamarin.Forms.Picker) — хороший выбор, если необходимо разрешить пользователю выбирать вариант из относительно короткого списка элементов `string`.

### <a name="the-picker-and-event-handling"></a>Picker и обработка событий

В примере [**PickerDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) показано, как использовать XAML, чтобы задать свойство [`Title`](xref:Xamarin.Forms.Picker.Title) для `Picker` и добавить элементы `string` в коллекцию [`Items`](xref:Xamarin.Forms.Picker.Items). Когда пользователь выбирает `Picker`, отображаются элементы в коллекции `Items` в зависимости от платформы.

Событие [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) указывает, что пользователь выбрал элемент. Свойство [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) отсчитывается от нуля, затем указывает выбранный элемент. Если элемент не выбран, значение свойства `SelectedIndex` — 1.

Можно также использовать `SelectedIndex` для инициализации выбранного элемента, но это свойство должно быть задано после заполнения коллекции `Items`. В XAML это означает, что вы, вероятно, используете элемент свойства для установки `SelectedIndex`.

### <a name="data-binding-the-picker"></a>Привязка данных с использованием Picker

Свойство `SelectedIndex` поддерживается связываемым свойством, а `Items` — нет, поэтому использование привязки данных с `Picker` усложняется. Одним из решений является использование `Picker` в сочетании с [`ObjectToIndexConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs), например из библиотеки [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit). Пример [**PickerBinding**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding) демонстрирует, как это работает.

> [!NOTE]
> Xamarin.Forms `Picker` теперь включает в себя свойства `ItemsSource` и `SelectedItem`, поддерживающие привязку данных. Дополнительную информацию см. в статье о [Picker](~/xamarin-forms/user-interface/picker/index.md).

## <a name="rendering-data-with-listview"></a>Преобразование данных с помощью ListView для просмотра

[`ListView`](xref:Xamarin.Forms.ListView) — единственный класс, производный от [`ItemsView<TVisual>`](xref:Xamarin.Forms.ItemsView`1), от которого наследуются свойства [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) и [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate).

Свойство `ItemsSource` имеет тип `IEnumerable` и значение `null` по умолчанию и должно быть явно инициализировано или (зачастую) задано коллекцией путем привязки данных. Элементы в этой коллекции могут быть любого типа.

`ListView` определяет свойство [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem), для которого либо задан один из элементов в коллекции `ItemsSource`, либо — `null`, если ни один элемент не выбран. `ListView` запускает событие [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) при выборе нового элемента.

### <a name="collections-and-selections"></a>Коллекции и выделения

Пример [**ListViewList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) заполняет `ListView`, указывая 17 значений для `Color` в коллекции `List<Color>`. Элементы можно выбирать, но по умолчанию они отображаются со своими непривлекательными представлениями `ToString`. Несколько примеров в этой главе показывают, как исправить это отображение и сделать его привлекательным при необходимости.

### <a name="the-row-separator"></a>Разделитель строк

На дисплеях устройств iOS и Android тонкая линия разделяет строки. Ее отображение можно контролировать с помощью свойств [`SeparatorVisibility`](xref:Xamarin.Forms.ListView.SeparatorVisibility) и [`SeparatorColor`](xref:Xamarin.Forms.ListView.SeparatorColor). Свойство `SeparatorVisibility` имеет тип [`SeparatorVisibility`](xref:Xamarin.Forms.SeparatorVisibility) — перечисление с двумя членами:

- [`Default`](xref:Xamarin.Forms.SeparatorVisibility.Default) (стандартный вариант)
- [`None`](xref:Xamarin.Forms.SeparatorVisibility.None)

### <a name="data-binding-the-selected-item"></a>Привязка данных к выбранному элементу

Свойство `SelectedItem` поддерживает привязываемое свойство, поэтому оно может быть или источником, или целью привязки данных. Значение свойства по умолчанию `BindingMode` равно `OneWayToSource`, но обычно оно является целевым объектом двухсторонней привязки данных, особенно в сценариях MVVM. Этот тип привязки показан в примере [**ListViewArray**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray).

### <a name="the-observablecollection-difference"></a>Отличие ObservableCollection

В примере [**ListViewLogger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) задается свойство `ItemsSource` для `ListView` в коллекции `List<DateTime>`, а затем в коллекцию последовательно каждую секунду с использованием таймера добавляется новый объект `DateTime`.

Но `ListView` не обновляется автоматически, так как в коллекции `List<T>` нет механизма уведомления, указывающего, когда элементы добавляются в коллекцию или удаляются из нее.

В таких сценариях гораздо лучше использовать класс [`ObservableCollection<T>`](xref:System.Collections.ObjectModel.ObservableCollection`1), определенный в пространстве имен `System.Collections.ObjectModel`. Этот класс реализует интерфейс [`INotifyCollectionChanged`](xref:System.Collections.Specialized.INotifyCollectionChanged) и затем вызывает событие [`CollectionChanged`](xref:System.Collections.ObjectModel.ObservableCollection`1.CollectionChanged) при добавлении элементов в коллекцию или их удалении из нее, а также при их замене или перемещении в коллекции. Когда `ListView` внутренне обнаруживает, что классу, реализующему `INotifyCollectionChanged`, задано свойство `ItemsSource`, представление присоединяет обработчик к событию `CollectionChanged` и обновляет отображение при изменении коллекции.

В примере [**ObservableLogger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) показано использование `ObservableCollection`.

### <a name="templates-and-cells"></a>Шаблоны и ячейки

По умолчанию `ListView` показывает элементы в коллекции с помощью метода `ToString` каждого элемента. Лучший подход — определить шаблон для отображения элементов.

Чтобы опробовать эту возможность, можно использовать класс [`NamedColor`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) из библиотеки [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit). Этот класс определяет статическое свойство `All` типа `IList<NamedColor>`, которое содержит 141 объект `NamedColor`, соответствующий открытым полям структуры `Color`.

В примере [**NaiveNamedColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) задается `ItemsSource` представления `ListView` для свойства `NamedColor.All`, но отображаются только полные имена классов объектов `NamedColor`.

Для `ListView` требуется шаблон отображения этих элементов. В коде можно задать для свойства [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate), определяемого `ItemsView<TVisual>`, объект [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) с помощью [конструктора `DataTemplate`](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)), ссылающегося на производный класс [`Cell`](xref:Xamarin.Forms.Cell). У `Cell` пять производных:

- [`TextCell`](xref:Xamarin.Forms.TextCell) &mdash; содержит два представления `Label` (концептуально).
- [`ImageCell`](xref:Xamarin.Forms.ImageCell) &mdash; добавляет представление `Image` в `TextCell`.
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) &mdash; содержит представление `Entry` с `Label`.
- [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) &mdash; содержит `Switch` с `Label`.
- [`ViewCell`](xref:Xamarin.Forms.ViewCell) &mdash; может быть любым `View` (возможно, с дочерними элементами).

Затем вызовите [`SetValue`](xref:Xamarin.Forms.DataTemplate.SetValue(Xamarin.Forms.BindableProperty,System.Object)) и [`SetBinding`](xref:Xamarin.Forms.DataTemplate.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) для объекта `DataTemplate` и свяжите значения со свойствами `Cell` или задайте привязки данных в свойствах `Cell`, сославшись на свойства элементов в коллекции `ItemsSource`. Это показано в примере [**TextCellListCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode).

`ListView` отображает каждый элемент, поэтому строится небольшое визуальное дерево на основе шаблона, а между элементом и свойствами элементов в этом визуальном дереве устанавливаются привязки данных. Чтобы понять процесс, установите обработчики для событий [`ItemAppearing`](xref:Xamarin.Forms.ListView.ItemAppearing) и [`ItemDisappearing`](xref:Xamarin.Forms.ListView.ItemDisappearing) представления `ListView` или используйте альтернативный [конструктор `DataTemplate`](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Func{System.Object})), который применяет функцию, вызываемую при каждом создании визуального дерева элементов.

[**TextCellListXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) показывает полностью функционально идентичную программу в XAML. Для тега `DataTemplate` устанавливается свойство `ItemTemplate` представления `ListView`, а затем для `TextCell` устанавливается `DataTemplate`. Привязки к свойствам элементов в коллекции задаются напрямую свойствам [`Text`](xref:Xamarin.Forms.TextCell.Text) и [`Detail`](xref:Xamarin.Forms.TextCell.Detail) для `TextCell`.

### <a name="custom-cells"></a>Пользовательские ячейки

В XAML можно задать [`ViewCell`](xref:Xamarin.Forms.ViewCell) для `DataTemplate`, а затем определить пользовательское визуальное дерево как свойство [`View`](xref:Xamarin.Forms.ViewCell.View) для `ViewCell`. (`View` — это свойство содержимого `ViewCell`, поэтому теги `ViewCell.View` не требуются.) Этот метод показан в примере [**CustomNamedColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList).

[![Снимок экрана с изображением списков настраиваемых именованных цветов на двух устройствах](images/ch19fg11-small.png "Список настраиваемых именованных цветов")](images/ch19fg11-large.png#lightbox "Список настраиваемых именованных цветов")

Правильно подобрать размеры для всех платформ может быть непросто. Свойство [`RowHeight`](xref:Xamarin.Forms.ListView.RowHeight) полезно для этого, но в некоторых случаях необходимо прибегнуть к свойству [`HasUnevenRows`](xref:Xamarin.Forms.ListView.HasUnevenRows), которое менее эффективно, но принуждает `ListView` задавать размер строк. Для iOS и Android необходимо использовать одно из этих двух свойств, чтобы получить строки соответствующего размера.

### <a name="grouping-the-listview-items"></a>Группирование элементов ListView

`ListView` поддерживает группирование элементов и навигацию между этими группами. Для свойства `ItemsSource` следует задать коллекцию коллекций. Для объекта с заданным свойством `ItemsSource` следует реализовать `IEnumerable`, а для каждого элемента в коллекции также следует реализовывать `IEnumerable`. У каждой группы должно быть два свойства: текстовое описание группы и трехбуквенное сокращение.

Класс [`NamedColorGroup`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) из библиотеки [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) создает семь групп объектов `NamedColor`. В примере [**ColorGroupList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) показано, как использовать эти группы со свойством [`IsGroupingEnabled`](xref:Xamarin.Forms.ListView.IsGroupingEnabled) представления `ListView` и значением `true`, а также свойства [`GroupDisplayBinding`](xref:Xamarin.Forms.ListView.GroupDisplayBinding) и [`GroupShortNameBinding`](xref:Xamarin.Forms.ListView.GroupShortNameBinding), привязанные к свойствам в каждой группе.

### <a name="custom-group-headers"></a>Настраиваемые заголовки групп

Можно создавать настраиваемые заголовки для групп `ListView`, заменяя свойство `GroupDisplayBinding` на [`GroupHeaderTemplate`](xref:Xamarin.Forms.ListView.GroupHeaderTemplate), определяющее шаблон для заголовков.

### <a name="listview-and-interactivity"></a>Интерактивность и ListView

Как правило, приложение получает взаимодействие пользователя через `ListView` путем присоединения обработчика к событию `ItemSelected` или [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) или путем привязки данных в свойстве `SelectedItem`. Но некоторые типы ячеек (`EntryCell` и `SwitchCell`) также могут взаимодействовать с пользователем, поэтому можно создавать настраиваемые ячейки, чтобы они взаимодействовали с пользователем. [**InteractiveListView**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView) создает 100 экземпляров [`ColorViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) и позволяет пользователю изменять каждый цвет с помощью тройки элементов `Slider`. Кроме того, программа использует [`ColorToContrastColorConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs) из библиотеки [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

## <a name="listview-and-mvvm"></a>ListView и MVVM

`ListView` играет важную роль в сценариях MVVM. Если во ViewModel существует коллекция `IEnumerable`, она зачастую привязана к `ListView`. Кроме того, элементы в коллекции часто реализуют `INotifyPropertyChanged` для привязки со свойствами в шаблоне.

### <a name="a-collection-of-viewmodels"></a>Коллекция ViewModel

Чтобы изучить эту возможность, библиотека [**SchoolOfFineArts**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) создает несколько классов на основе [файла данных XML](https://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml) и изображений вымышленных учащихся в вымышленном учебном заведении.

Класс [`Student`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) является производным от [`ViewModelBase`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs). Класс [`StudentBody`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) представляет собой коллекцию объектов `Student`, а также является производным от `ViewModelBase`. [`SchoolViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) скачивает файл XML и собирает все объекты.

Программа [**StudentList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList) использует `ImageCell` для отображения данных учащихся и их изображений в `ListView`:

[![Снимок экрана с изображением списка учащихся на двух устройствах](images/ch19fg18-small.png "Список учащихся")](images/ch19fg18-large.png#lightbox "Список учащихся")

В примере [**ListViewHeader**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) добавляется свойство [`Header`](xref:Xamarin.Forms.ListView.Header), но оно отображается только в Android.

### <a name="selection-and-the-binding-context"></a>Выделение и контекст привязок

Программа [**SelectedStudentDetail**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) выполняет привязку `BindingContext` `StackLayout` к свойству `SelectedItem` представления `ListView`. Это позволяет программе показывать подробные сведения о выбранном учащемся.

### <a name="context-menus"></a>Контекстные меню

Ячейка может определить контекстное меню, реализованное для конкретной платформы. Чтобы создать это меню, добавьте объекты [`MenuItem`](xref:Xamarin.Forms.MenuItem) в свойство [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) объекта `Cell`.

`MenuItem` определяет пять свойств:

- [`Text`](xref:Xamarin.Forms.MenuItem.Text) типа `string`
- [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) типа `FileImageSource`
- [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive) типа `bool`
- [`Command`](xref:Xamarin.Forms.MenuItem.Command) типа `ICommand`
- [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter) типа `object`

Свойства `Command` и `CommandParameter` подразумевают, что ViewModel для каждого элемента содержит методы, чтобы выполнять нужные команды меню. В сценариях, где не используется MVVM, `MenuItem` также определяет событие [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked).

[**CellContextMenu**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu) демонстрирует этот метод. Свойство `Command` каждого `MenuItem` привязано к свойству типа `ICommand` в классе `Student`. Задайте для свойства `IsDestructive` значение `true` для `MenuItem`, это удалит выбранный объект.

### <a name="varying-the-visuals"></a>Разные визуальные элементы

Иногда может потребоваться, чтобы было несколько вариантов визуальных объектов элементов в `ListView` в зависимости от свойства. Например, если среднее значение оценок учащегося снижается ниже 2,0, то в примере [**ColorCodedStudents**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) имя учащегося будет отображаться красным цветом.
Для этого используется преобразователь величин привязки [`ThresholdToObjectConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs) из библиотеки [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

### <a name="refreshing-the-content"></a>Обновление содержимого

`ListView` поддерживает жест развертывания для обновления данных. Для этого программа должна задать для свойства [`IsPullToRefresh`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) значение `true`. `ListView` реагирует на жест развертывания, устанавливая для свойства [`IsRefreshing`](xref:Xamarin.Forms.ListView.IsRefreshing) значение `true`, а также вызывая событие [`Refreshing`](xref:Xamarin.Forms.ListView.Refreshing) и (для сценариев MVVM) метод `Execute` свойства [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand).

Затем код, обрабатывающий событие `Refresh`, или команда `RefreshCommand`, возможно, обновит данные, отображаемые `ListView`, и возвратит `IsRefreshing` значение `false`.

В примере [**RssFeed**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) демонстрируется использование [`RssFeedViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs), реализующей свойства `RefreshCommand` и `IsRefreshing` для привязки данных.

## <a name="the-tableview-and-its-intents"></a>Представление TableView и его намерения

Если `ListView` обычно показывает несколько экземпляров одного типа, то [`TableView`](xref:Xamarin.Forms.TableView), как правило, предоставляет пользовательский интерфейс для нескольких свойств различных типов. Каждый элемент связан с собственным производным [`Cell`](xref:Xamarin.Forms.Cell) для отображения свойства или предоставления ему пользовательского интерфейса.

### <a name="properties-and-hierarchies"></a>Свойства и иерархии

`TableView` определяет только четыре свойства:

- [`Intent`](xref:Xamarin.Forms.TableView.Intent) типа [`TableIntent`](xref:Xamarin.Forms.TableIntent) — перечисление.
- [`Root`](xref:Xamarin.Forms.TableView.Root) с типом [`TableRoot`](xref:Xamarin.Forms.TableRoot) — это свойство содержимого `TableView`.
- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight) типа `int`
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) типа `bool`

Перечисление `TableIntent` указывает, как предполагается использовать `TableView`.

- [`Data`](xref:Xamarin.Forms.TableIntent.Data)
- [`Form`](xref:Xamarin.Forms.TableIntent.Form)
- [`Settings`](xref:Xamarin.Forms.TableIntent.Settings)
- [`Menu`](xref:Xamarin.Forms.TableIntent.Menu)

Эти члены также предлагают некоторые варианты использования `TableView`.

При определении таблицы участвуют и другие классы:

- [`TableSectionBase`](xref:Xamarin.Forms.TableSectionBase) — это абстрактный класс, производный от `BindableObject` и определяющий свойство [`Title`](xref:Xamarin.Forms.TableSectionBase.Title).

- [`TableSectionBase<T>`](xref:Xamarin.Forms.TableSectionBase`1) — это абстрактный класс, производный от `TableSectionBase` и реализующий `IList<T>` и `INotifyCollectionChanged`.

- [`TableSection`](xref:Xamarin.Forms.TableSection) происходит от `TableSectionBase<Cell>`.

- [`TableRoot`](xref:Xamarin.Forms.TableRoot) происходит от `TableSectionBase<TableSection>`.

У `TableView` есть свойство `Root`, для которого задан объект `TableRoot`, представляющий собой коллекцию объектов `TableSection`, каждый из которых является коллекцией объектов `Cell`. Таблица состоит из нескольких разделов, и в каждом разделе содержится несколько ячеек. Заголовок может быть у самой таблицы, а также у каждого раздела. `TableView` использует производные от `Cell`, но не использует `DataTemplate`.

### <a name="a-prosaic-form"></a>Простая форма

Пример [**EntryForm**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm) определяет модель представления [`PersonalInformation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs), экземпляр которой превращается в `BindingContext` представления `TableView`. Каждый производный объект `Cell` в своем разделе `TableSection` может иметь привязки к свойствам класса `PersonalInformation`.

### <a name="custom-cells"></a>Пользовательские ячейки

В примере [**ConditionalCells**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) раскрывается **EntryForm**. Класс [`ProgrammerInformation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) включает в себя логическое свойство, которое регулирует применимость двух дополнительных свойств. Для этих двух дополнительных свойств программа использует пользовательский элемент `PickerCell` на основе [PickerCell.xamlL](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml) и [PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs) из библиотеки [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

Хотя свойства `IsEnabled` двух элементов `PickerCell` и привязаны к логическому свойству в `ProgrammerInformation`, этот прием не работает, поэтому предлагаем следующий пример.

### <a name="conditional-sections"></a>Условные разделы

В примере [**ConditionalSection**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) два элемента, которые являются условными при выборе логического элемента, помещены в отдельный раздел `TableSection`. Файл кода программной части удаляет этот раздел из `TableView` или добавляет его обратно на основе логического свойства.

### <a name="a-tableview-menu"></a>Меню TableView

Другим применением `TableView` является меню. В примере [**MenuCommands**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) показано меню, позволяющее перемещать маленькое представление `BoxView` по экрану.

## <a name="related-links"></a>Связанные ссылки

- [Глава 19, полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [Примеры для главы 19](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [Средство выбора](~/xamarin-forms/user-interface/picker/index.md)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
