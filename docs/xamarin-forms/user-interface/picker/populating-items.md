---
title: Добавление данных в коллекцию элементов средства выбора
description: Представление выбора — это элемент управления для выбора текстового элемента из списка данных. В этой статье объясняется, как заполнить средство выбора данными, добавив их в коллекцию Items, и как реагировать на выбор элементов пользователем.
ms.prod: xamarin
ms.assetid: 3C840F64-A430-457D-A4B2-3D7AF46F9DBE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8904f3392e935b3d8aa08d87d208bdb46ac37b30
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93368076"
---
# <a name="adding-data-to-a-pickers-items-collection"></a>Добавление данных в коллекцию элементов средства выбора

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo)

_Представление выбора — это элемент управления для выбора текстового элемента из списка данных. В этой статье объясняется, как заполнить средство выбора данными, добавив их в коллекцию Items, и как реагировать на выбор элементов пользователем._

## <a name="populating-a-picker-with-data"></a>Заполнение средства выбора данными

До Xamarin.Forms 2.3.4 процесс заполнения [`Picker`](xref:Xamarin.Forms.Picker) данными был добавлен в [`Items`](xref:Xamarin.Forms.Picker.Items) коллекцию, доступную только для чтения, которая имеет тип `IList<string>` . Каждый элемент в коллекции должен иметь тип `string` . Элементы можно добавить в XAML путем инициализации `Items` свойства со списком `x:String` элементов:

```xaml
<Picker Title="Select a monkey"
        TitleColor="Red">
  <Picker.Items>
    <x:String>Baboon</x:String>
    <x:String>Capuchin Monkey</x:String>
    <x:String>Blue Monkey</x:String>
    <x:String>Squirrel Monkey</x:String>
    <x:String>Golden Lion Tamarin</x:String>
    <x:String>Howler Monkey</x:String>
    <x:String>Japanese Macaque</x:String>
  </Picker.Items>
</Picker>
```

Ниже приведен эквивалентный код на C#:

```csharp
var picker = new Picker { Title = "Select a monkey", TitleColor = Color.Red };
picker.Items.Add("Baboon");
picker.Items.Add("Capuchin Monkey");
picker.Items.Add("Blue Monkey");
picker.Items.Add("Squirrel Monkey");
picker.Items.Add("Golden Lion Tamarin");
picker.Items.Add("Howler Monkey");
picker.Items.Add("Japanese Macaque");
```

Помимо добавления данных с помощью `Items.Add` метода, данные также могут быть вставлены в коллекцию с помощью `Items.Insert` метода.

## <a name="responding-to-item-selection"></a>Реагирование на выбор элементов

[`Picker`](xref:Xamarin.Forms.Picker)Поддерживает выборку по одному элементу за раз. Когда пользователь выбирает элемент, [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) возникает событие, и [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) свойство обновляется до целого числа, представляющего индекс выбранного элемента в списке. Свойство — это номер, начинающийся `SelectedIndex` с нуля, указывающий элемент, выбранный пользователем. Если элемент не выбран, то есть при `Picker` первом создании и инициализации, `SelectedIndex` будет равен-1.

> [!NOTE]
> Поведение выбора элементов в [`Picker`](xref:Xamarin.Forms.Picker) можно настроить в iOS с учетом конкретной платформы. Дополнительные сведения см. в разделе [Управление выбором элементов средства выбора](~/xamarin-forms/platform/ios/picker-selection.md).

В следующем примере кода показан `OnPickerSelectedIndexChanged` метод обработчика событий, который выполняется при [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) срабатывании события:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs e)
{
  var picker = (Picker)sender;
  int selectedIndex = picker.SelectedIndex;

  if (selectedIndex != -1)
  {
    monkeyNameLabel.Text = picker.Items[selectedIndex];
  }
}
```

Этот метод получает [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) значение свойства и использует значение для получения выбранного элемента из [`Items`](xref:Xamarin.Forms.Picker.Items) коллекции. Так как каждый элемент в `Items` коллекции имеет значение `string` , они могут отображаться с помощью, [`Label`](xref:Xamarin.Forms.Label) не требуя приведения.

> [!NOTE]
> [`Picker`](xref:Xamarin.Forms.Picker)Можно инициализировать для вывода определенного элемента, задав [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) свойство. Однако это `SelectedIndex` свойство должно быть задано после инициализации [`Items`](xref:Xamarin.Forms.Picker.Items) коллекции.

## <a name="related-links"></a>Связанные ссылки

- [Демонстрация средства выбора (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo)
- [Средство выбора](xref:Xamarin.Forms.Picker)