---
title: Xamarin.Forms Ячеек
description: Xamarin.Forms ячейки можно добавлять в ListView и Таблевиевс. В этой статье перечислены ячейки, входящие в Xamarin.Forms .
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 01f625d9ecfb91bc36013b7f6d45fb3d275e8bee
ms.sourcegitcommit: 10c7dd16fe78226053d1d036492b6c9102fc421b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2021
ms.locfileid: "93370836"
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms Ячеек

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/formsgallery)

_Xamarin.Forms ячейки можно добавлять в ListView и Таблевиевс._

*Ячейка* — это специализированный элемент, используемый для элементов в таблице и описывающий, как должен быть визуализирован каждый элемент в списке. [`Cell`](xref:Xamarin.Forms.Cell)Класс является производным от [`Element`](xref:Xamarin.Forms.Element) , из которого [`VisualElement`](xref:Xamarin.Forms.Element) также наследуется. Ячейка не является визуальным элементом; Вместо этого используется шаблон для создания визуального элемента.

`Cell` используется исключительно с [`ListView`](xref:Xamarin.Forms.ListView) [`TableView`](xref:Xamarin.Forms.TableView) элементами управления и. Сведения об использовании и настройке ячеек см [`ListView`](~/xamarin-forms/user-interface/listview/index.md) [`TableView`](~/xamarin-forms/user-interface/tableview.md) . в документации и.

## <a name="cells"></a>Ячейки

Xamarin.Forms поддерживает следующие типы ячеек:

| Тип | Описание | Внешний вид |
| --- | --- | --- |
| `TextCell` | [`TextCell`](xref:Xamarin.Forms.TextCell)Отображает одну или две текстовые строки. Задайте [`Text`](xref:Xamarin.Forms.TextCell.Text) свойство и, при необходимости, [`Detail`](xref:Xamarin.Forms.TextCell.Detail) свойство для этих текстовых строк.<br /><br />[Документация по API](xref:Xamarin.Forms.TextCell)  /  [Руководством](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell) | [![Пример Текстцелл](cells-images/TextCell.png "Пример Текстцелл")](cells-images/TextCell-Large.png#lightbox "Пример Текстцелл")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
| `ImageCell` | [`ImageCell`](xref:Xamarin.Forms.ImageCell)Отображает те же сведения, что и, [`TextCell`](xref:Xamarin.Forms.TextCell) но содержит точечный рисунок, заданный с помощью [`Source`](xref:Xamarin.Forms.Image.Source) Свойства.<br /><br />[Документация по API](xref:Xamarin.Forms.ImageCell)  /  [Руководством](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell) | [![Пример Имажецелл](cells-images/ImageCell.png "Пример Имажецелл")](cells-images/ImageCell-Large.png#lightbox "Пример Имажецелл")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
| `SwitchCell` | [`SwitchCell`](xref:Xamarin.Forms.SwitchCell)Содержит набор текста со [`Text`](xref:Xamarin.Forms.SwitchCell.Text) свойством и параметром включения/выключения, изначально заданным логическим [`On`](xref:Xamarin.Forms.SwitchCell.On) свойством. Обрабатывает [`OnChanged`](xref:Xamarin.Forms.SwitchCell.OnChanged) событие, чтобы получать уведомления при `On` изменении свойства.<br /><br />[Документация по API](xref:Xamarin.Forms.SwitchCell)  /  [Руководством](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![Пример Свитчцелл](cells-images/SwitchCell.png "Пример Свитчцелл")](cells-images/SwitchCell-Large.png#lightbox "Пример Свитчцелл")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
| `EntryCell` | [`EntryCell`](xref:Xamarin.Forms.EntryCell)Определяет [`Label`](xref:Xamarin.Forms.EntryCell.Label) свойство, идентифицирующее ячейку и одну строку редактируемого текста в [`Text`](xref:Xamarin.Forms.EntryCell.Text) свойстве. Обработайте [`Completed`](xref:Xamarin.Forms.EntryCell.Completed) событие, чтобы получать уведомления о том, что пользователь завершил ввод текста.<br /><br />[Документация по API](xref:Xamarin.Forms.EntryCell)  /  [Руководством](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![Пример Ентрицелл](cells-images/EntryCell.png "Пример Ентрицелл")](cells-images/EntryCell-Large.png#lightbox "Пример Ентрицелл")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
| | | |

## <a name="related-links"></a>Связанные ссылки

- [Xamarin.Forms Пример Формсгаллери](/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Примеры для Xamarin.Forms](/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms Документация по API](/dotnet/api/xamarin.forms?view=xamarin-forms)