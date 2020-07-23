---
title: Xamarin.FormsПрисутствует
description: В Xamarin.Forms средстве выбора отображается короткий список элементов, из которого пользователь может выбрать элемент. В этой статье объясняется, как использовать класс средства выбора для выбора текстового элемента из списка данных.
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5e3153e796f26ef150dccc79d8ea6f90127c6a26
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938688"
---
# <a name="xamarinforms-picker"></a>Xamarin.FormsПрисутствует

_Представление выбора — это элемент управления для выбора текстового элемента из списка данных._

Xamarin.Forms [`Picker`](xref:Xamarin.Forms.Picker) Отображает короткий список элементов, из которого пользователь может выбрать элемент. `Picker` определяет следующие свойства:

- [`Title`](xref:Xamarin.Forms.Picker.Title)типа `string` , значение по умолчанию — `null` .
- `TitleColor`типа [`Color`](xref:Xamarin.Forms.Color) — цвет, используемый для вывода `Title` текста.
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource)типа `IList` — исходный список элементов для вывода, который по умолчанию имеет значение `null` .
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex)типа `int` — индекс выбранного элемента, который по умолчанию имеет значение-1.
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem)типа `object` , выбранного элемента, который по умолчанию имеет значение `null` .
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor)для типа [`Color`](xref:Xamarin.Forms.Color) — цвет, используемый для вывода текста, который по умолчанию имеет значение [`Color.Default`](xref:Xamarin.Forms.Color.Default) .
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes)типа [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) , значение по умолчанию — [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None) .
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily)типа `string` , значение по умолчанию — `null` .
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize)типа `double` , значение по умолчанию — 1,0.
- `CharacterSpacing`, тип `double` — интервал между символами элемента, отображаемого `Picker` .

Все свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, то есть они могут быть стилями, а свойства могут быть целями привязок данных. [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex)Свойства и [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) имеют режим привязки по умолчанию [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) , то есть могут быть целевыми объектами привязок данных в приложении, использующем архитектуру [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) . Дополнительные сведения о настройке свойств шрифта см. в разделе [шрифты](~/xamarin-forms/user-interface/text/fonts.md).

[`Picker`](xref:Xamarin.Forms.Picker)При первом отображении объект не отображает никаких данных. Вместо этого значение его [`Title`](xref:Xamarin.Forms.Picker.Title) Свойства отображается в виде заполнителя на платформах iOS и Android:

[![Отображение исходного окна выбора](images/picker-initial.png)](images/picker-initial-large.png#lightbox "Отображение исходного окна выбора")

Когда [`Picker`](xref:Xamarin.Forms.Picker) фокус на прибыль, его данные отображаются и пользователь может выбрать элемент:

[![Выбор элемента в средстве выбора](images/picker-selection.png)](images/picker-selection-large.png#lightbox "Выбор элемента в средстве выбора")

[`Picker`](xref:Xamarin.Forms.Picker) [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) Компонент запускает событие, когда пользователь выбирает элемент. После выбора выбранный элемент отображается в `Picker` :

![Средство выбора после выбора](images/picker-after-selection.png)

Существует два способа заполнения [`Picker`](xref:Xamarin.Forms.Picker) данными с помощью:

- Присвоение [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) свойству отображаемых данных. Это рекомендуемая методика. Дополнительные сведения см. [в разделе Задание свойства ItemsSource элемента управления](populating-itemssource.md).
- Добавление данных, которые должны отображаться в [`Items`](xref:Xamarin.Forms.Picker.Items) коллекции. Этот прием был исходным процессом заполнения [`Picker`](xref:Xamarin.Forms.Picker) данными. Дополнительные сведения см. в разделе [Добавление данных в коллекцию элементов средства выбора](populating-items.md).

## <a name="related-links"></a>Связанные ссылки

- [Средство выбора](xref:Xamarin.Forms.Picker)
