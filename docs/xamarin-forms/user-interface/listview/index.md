---
Title: Xamarin.Forms Описание "ListView" Description: "в этом руководство описывается Xamarin.Forms ListView, который можно использовать для представления данных в интерактивных списках".
MS. произв. Xamarin MS. AssetID: FEFDF7E0-720F-4BD1-863F-4477226AA695 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 09/04/2019 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-listview"></a>Xamarin.FormsЭлементе

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview)

[`ListView`](xref:Xamarin.Forms.ListView)представляет собой представление для представления списков данных, особенно длинных списков, требующих прокрутки.

> [!IMPORTANT]
> Представление [`CollectionView`](xref:Xamarin.Forms.CollectionView) служит для вывода списков данных с различными спецификациями макета. Она нацелена на предоставление более гибкой и производительной альтернативы [`ListView`](xref:Xamarin.Forms.ListView) . Дополнительные сведения см. в разделе [Xamarin.Forms CollectionView](~/xamarin-forms/user-interface/collectionview/index.md).

## <a name="use-cases"></a>Варианты использования

`ListView`Элемент управления может использоваться в любой ситуации, когда отображаются прокручиваемые списки данных. `ListView`Класс поддерживает действия контекста и привязку данных.

`ListView`Элемент управления не должен путать с [`TableView`](~/xamarin-forms/user-interface/tableview.md) элементом управления. `TableView`Элемент управления является лучшим вариантом при наличии непривязанного списка параметров или данных, так как он позволяет указывать в XAML предопределенные параметры. Например, приложение iOS Settings, которое имеет преимущественно предопределенный набор параметров, лучше подходит для использования, `TableView` чем `ListView` .

`ListView`Класс не поддерживает определение элементов списка в XAML `ItemsSource` `ItemTemplate` . для определения элементов в списке необходимо использовать свойство или привязку данных с помощью.

`ListView`Лучше всего подходит для коллекций, состоящих из одного типа данных. Это требование связано с тем, что для каждой строки в списке можно использовать только один тип ячеек. `TableView`Элемент управления может поддерживать несколько типов ячеек, поэтому это лучший вариант, если требуется отобразить несколько типов данных.

Дополнительные сведения о привязке данных к `ListView` экземпляру см. в разделе [Источники данных в ListView](~/xamarin-forms/user-interface/listview/data-and-databinding.md).

## <a name="components"></a>Компоненты

У `ListView` элемента управления есть ряд компонентов, позволяющих использовать собственные функции каждой платформы. Эти компоненты определены в следующих разделах.

### <a name="headers-and-footers"></a>[Верхние и нижние колонтитулы](customizing-list-appearance.md#headers-and-footers)

Компоненты верхнего и нижнего колонтитулов отображаются в начале и в конце списка, отделены от данных списка. Верхние и нижние колонтитулы можно привязать к отдельному источнику данных из источника данных ListView.

### <a name="groups"></a>[Группы](customizing-list-appearance.md#grouping)

Данные в `ListView` могут быть сгруппированы для упрощения навигации. Группы обычно привязаны к данным. На следующем снимке экрана показан элемент `ListView` с сгруппированными данными:

[!["Сгруппированные данные в ListView"](images/grouping-depth-cropped.png)](images/grouping-depth.png#lightbox "Сгруппированные данные в ListView")

### <a name="cells"></a>[Ячейки](customizing-cell-appearance.md)

Элементы данных в называются `ListView` ячейками. Каждая ячейка соответствует строке данных. Существуют встроенные ячейки, которые можно выбрать, или вы можете определить собственную пользовательскую ячейку. Как встроенные, так и пользовательские ячейки можно использовать и определять в XAML или коде.

- [Встроенные ячейки](customizing-cell-appearance.md#built-in-cells), такие как `TextCell` и `ImageCell` , соответствуют собственным элементам управления и являются особенно производительными.
  - [`TextCell`](customizing-cell-appearance.md#textcell)Отображает строку текста (при необходимости с текстом подробностей). Текст сведений отображается в виде второй строки с меньшим шрифтом с контрастным цветом.
  - [`ImageCell`](customizing-cell-appearance.md#imagecell)Отображает изображение с текстом. Отображается как объект `TextCell` с изображением слева.
- [Пользовательские ячейки](customizing-cell-appearance.md#custom-cells) используются для представления сложных данных. Например, пользовательскую ячейку можно использовать для представления списка композиций, включающих альбом и исполнителя.

На следующем снимке экрана показан элемент `ListView` с имажецелл элементами:

[!["Имажецелл элементы в ListView"](images/image-cell-default-cropped.png)](images/image-cell-default.png#lightbox "Имажецелл элементы в ListView")

Дополнительные сведения о настройке ячеек в `ListView` см. в разделе [Настройка внешнего вида ячеек ListView](customizing-cell-appearance.md).

## <a name="functionality"></a>Функциональность

`ListView`Класс поддерживает несколько стилей взаимодействия.

- Запрос [на обновление](interactivity.md#pull-to-refresh) позволяет пользователю извлекаться `ListView` вниз для обновления содержимого.
- [Контекстные действия](interactivity.md#context-actions) позволяют разработчику указывать настраиваемые действия для отдельных элементов списка. Например, можно реализовать операции считывания в iOS или длительное касание на Android.
- [Выбор](interactivity.md#selection-and-taps) позволяет разработчику прикреплять функции к событиям выбора и девыделения для элементов списка.

На следующем снимке экрана показана `ListView` контекстная операция.

[!["Контекстные действия в ListView"](images/context-default-cropped.png)](images/context-default.png#lightbox "Контекстные действия в ListView")

Дополнительные сведения о возможностях взаимодействия см `ListView` . в разделе [действия & интерактивном представлении с помощью ListView](interactivity.md).

## <a name="related-links"></a>Связанные ссылки

- [Работа с ListView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview)
- [Двусторонняя привязка (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
- [Встроенные ячейки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [Пользовательские ячейки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [Группирование (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)
- [Настраиваемое представление модуля подготовки отчетов (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative/)
- [Взаимодействие с ListView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)
