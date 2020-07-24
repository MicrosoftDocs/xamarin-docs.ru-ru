---
title: :::no-loc(Xamarin.Forms):::Ячеек
description: ':::no-loc(Xamarin.Forms):::ячейки можно добавлять в ListView и Таблевиевс. В этой статье перечислены ячейки, входящие в :::no-loc(Xamarin.Forms)::: .'
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
no-loc:
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: 21fca31ca9e1cf39843e04c3c381bb41ef77335f
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997284"
---
# <a name="no-locxamarinforms-cells"></a>:::no-loc(Xamarin.Forms):::Ячеек

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_:::no-loc(Xamarin.Forms):::ячейки можно добавлять в ListView и Таблевиевс._

*Ячейка* — это специализированный элемент, используемый для элементов в таблице и описывающий, как должен быть визуализирован каждый элемент в списке. [`Cell`](xref::::no-loc(Xamarin.Forms):::.Cell)Класс является производным от [`Element`](xref::::no-loc(Xamarin.Forms):::.Element) , из которого [`VisualElement`](xref::::no-loc(Xamarin.Forms):::.Element) также наследуется. Ячейка не является визуальным элементом; Вместо этого используется шаблон для создания визуального элемента.

`Cell`используется исключительно с [`ListView`](xref::::no-loc(Xamarin.Forms):::.ListView) [`TableView`](xref::::no-loc(Xamarin.Forms):::.TableView) элементами управления и. Сведения об использовании и настройке ячеек см [`ListView`](~/xamarin-forms/user-interface/listview/index.md) [`TableView`](~/xamarin-forms/user-interface/tableview.md) . в документации и.

## <a name="cells"></a>Ячейки

:::no-loc(Xamarin.Forms):::поддерживает следующие типы ячеек:

| Type | Описание | Внешний вид |
| --- | --- | --- |
| `TextCell` | [`TextCell`](xref::::no-loc(Xamarin.Forms):::.TextCell)Отображает одну или две текстовые строки. Задайте [`Text`](xref::::no-loc(Xamarin.Forms):::.TextCell.Text) свойство и, при необходимости, [`Detail`](xref::::no-loc(Xamarin.Forms):::.TextCell.Detail) свойство для этих текстовых строк.<br /><br />[Документация по API](xref::::no-loc(Xamarin.Forms):::.TextCell)  /  [Руководством](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell) | [![Пример Текстцелл](cells-images/TextCell.png "Пример Текстцелл")](cells-images/TextCell-Large.png#lightbox "Пример Текстцелл")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
| `ImageCell` | [`ImageCell`](xref::::no-loc(Xamarin.Forms):::.ImageCell)Отображает те же сведения, что и, [`TextCell`](xref::::no-loc(Xamarin.Forms):::.TextCell) но содержит точечный рисунок, заданный с помощью [`Source`](xref::::no-loc(Xamarin.Forms):::.Image.Source) Свойства.<br /><br />[Документация по API](xref::::no-loc(Xamarin.Forms):::.ImageCell)  /  [Руководством](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell) | [![Пример Имажецелл](cells-images/ImageCell.png "Пример Имажецелл")](cells-images/ImageCell-Large.png#lightbox "Пример Имажецелл")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
| `SwitchCell` | [`SwitchCell`](xref::::no-loc(Xamarin.Forms):::.SwitchCell)Содержит набор текста со [`Text`](xref::::no-loc(Xamarin.Forms):::.SwitchCell.Text) свойством и параметром включения/выключения, изначально заданным логическим [`On`](xref::::no-loc(Xamarin.Forms):::.SwitchCell.On) свойством. Обрабатывает [`OnChanged`](xref::::no-loc(Xamarin.Forms):::.SwitchCell.OnChanged) событие, чтобы получать уведомления при `On` изменении свойства.<br /><br />[Документация по API](xref::::no-loc(Xamarin.Forms):::.SwitchCell)  /  [Руководством](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![Пример Свитчцелл](cells-images/SwitchCell.png "Пример Свитчцелл")](cells-images/SwitchCell-Large.png#lightbox "Пример Свитчцелл")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
| `EntryCell` | [`EntryCell`](xref::::no-loc(Xamarin.Forms):::.EntryCell)Определяет [`Label`](xref::::no-loc(Xamarin.Forms):::.EntryCell.Label) свойство, идентифицирующее ячейку и одну строку редактируемого текста в [`Text`](xref::::no-loc(Xamarin.Forms):::.EntryCell.Text) свойстве. Обработайте [`Completed`](xref::::no-loc(Xamarin.Forms):::.EntryCell.Completed) событие, чтобы получать уведомления о том, что пользователь завершил ввод текста.<br /><br />[Документация по API](xref::::no-loc(Xamarin.Forms):::.EntryCell)  /  [Руководством](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![Пример Ентрицелл](cells-images/EntryCell.png "Пример Ентрицелл")](cells-images/EntryCell-Large.png#lightbox "Пример Ентрицелл")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
| | | |

## <a name="related-links"></a>Связанные ссылки

- [:::no-loc(Xamarin.Forms):::Пример Формсгаллери](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Примеры для :::no-loc(Xamarin.Forms):::](https://docs.microsoft.com/samples/browse/?products=xamarin&term=:::no-loc(Xamarin.Forms):::)
- [:::no-loc(Xamarin.Forms):::Документация по API](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
