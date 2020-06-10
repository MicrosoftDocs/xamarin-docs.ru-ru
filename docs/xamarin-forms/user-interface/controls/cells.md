---
Title: " Xamarin.Forms ячейки" Description: " Xamarin.Forms ячейки можно добавлять в ListView и таблевиевс. В этой статье перечислены ячейки, входящие в Xamarin.Forms ".
MS. произв. Xamarin MS. AssetID: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 01/12/2016 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-cells"></a>Xamarin.FormsЯчеек

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Ячейки Xamarin. Forms можно добавлять в ListView и Таблевиевс._

*Ячейка* — это специализированный элемент, используемый для элементов в таблице и описывающий, как должен быть визуализирован каждый элемент в списке. [`Cell`](xref:Xamarin.Forms.Cell)Класс является производным от [`Element`](xref:Xamarin.Forms.Element) , из которого [`VisualElement`](xref:Xamarin.Forms.Element) также наследуется. Ячейка не является визуальным элементом; Вместо этого используется шаблон для создания визуального элемента.

`Cell`используется исключительно с [`ListView`](views.md#listview) [`TableView`](views.md#tableview) элементами управления и. Сведения об использовании и настройке ячеек см [`ListView`](~/xamarin-forms/user-interface/listview/index.md) [`TableView`](~/xamarin-forms/user-interface/tableview.md) . в документации и.

## <a name="cells"></a>Ячейки

Xamarin.Formsподдерживает следующие типы ячеек:

### <a name="textcell"></a>текстцелл

|     |     |
| --- | --- |
| [`TextCell`](xref:Xamarin.Forms.TextCell)Отображает одну или две текстовые строки. Задайте [`Text`](xref:Xamarin.Forms.TextCell.Text) свойство и, при необходимости, [`Detail`](xref:Xamarin.Forms.TextCell.Detail) свойство для этих текстовых строк.<br /><br />[Документация по API](xref:Xamarin.Forms.TextCell)  /  [Руководством](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell) | [![Пример Текстцелл](cells-images/TextCell.png "Пример Текстцелл")](cells-images/TextCell-Large.png#lightbox "Пример Текстцелл")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
|     |     |

### <a name="imagecell"></a>имажецелл

|     |     |
| --- | --- |
| [`ImageCell`](xref:Xamarin.Forms.ImageCell)Отображает те же сведения, что и, [`TextCell`](#textcell) но содержит точечный рисунок, заданный с помощью [`Source`](xref:Xamarin.Forms.Image.Source) Свойства.<br /><br />[Документация по API](xref:Xamarin.Forms.ImageCell)  /  [Руководством](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell) | [![Пример Имажецелл](cells-images/ImageCell.png "Пример Имажецелл")](cells-images/ImageCell-Large.png#lightbox "Пример Имажецелл")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
|     |     |

### <a name="switchcell"></a>свитчцелл

|     |     |
| --- | --- |
| [`SwitchCell`](xref:Xamarin.Forms.SwitchCell)Содержит набор текста со [`Text`](xref:Xamarin.Forms.SwitchCell.Text) свойством и параметром включения/выключения, изначально заданным логическим [`On`](xref:Xamarin.Forms.SwitchCell.On) свойством. Обрабатывает [`OnChanged`](xref:Xamarin.Forms.SwitchCell.OnChanged) событие, чтобы получать уведомления при `On` изменении свойства.<br /><br />[Документация по API](xref:Xamarin.Forms.SwitchCell)  /  [Руководством](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![Пример Свитчцелл](cells-images/SwitchCell.png "Пример Свитчцелл")](cells-images/SwitchCell-Large.png#lightbox "Пример Свитчцелл")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
|     |     |

### <a name="entrycell"></a>ентрицелл

|     |     |
| --- | --- |
| [`EntryCell`](xref:Xamarin.Forms.EntryCell)Определяет [`Label`](xref:Xamarin.Forms.EntryCell.Label) свойство, идентифицирующее ячейку и одну строку редактируемого текста в [`Text`](xref:Xamarin.Forms.EntryCell.Text) свойстве. Обработайте [`Completed`](xref:Xamarin.Forms.EntryCell.Completed) событие, чтобы получать уведомления о том, что пользователь завершил ввод текста.<br /><br />[Документация по API](xref:Xamarin.Forms.EntryCell)  /  [Руководством](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![Пример Ентрицелл](cells-images/EntryCell.png "Пример Ентрицелл")](cells-images/EntryCell-Large.png#lightbox "Пример Ентрицелл")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Связанные ссылки

- [Xamarin.FormsПример Формсгаллери](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Примеры для Xamarin.Forms](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsДокументация по API](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
