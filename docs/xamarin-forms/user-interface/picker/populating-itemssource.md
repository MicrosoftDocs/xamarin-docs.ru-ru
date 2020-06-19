---
title: Задание свойства ItemsSource средства выбора
description: Представление выбора — это элемент управления для выбора текстового элемента из списка данных. В этой статье объясняется, как заполнить средство выбора данными, задав свойство ItemsSource, и как реагировать на выбор элементов пользователем.
ms.prod: xamarin
ms.assetid: 8ECF390C-9DB2-4441-B9A3-101AE7E5AEC5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8c4fc732082a77a2e471465af448a487862b513c
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136296"
---
# <a name="setting-a-pickers-itemssource-property"></a>Задание свойства ItemsSource средства выбора

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-monkeyapppicker)

_Представление выбора — это элемент управления для выбора текстового элемента из списка данных. В этой статье объясняется, как заполнить средство выбора данными, задав свойство ItemsSource, и как реагировать на выбор элементов пользователем._

Xamarin.Forms2.3.4 улучшила [`Picker`](xref:Xamarin.Forms.Picker) представление, добавляя возможность заполнять его данными, задавая его [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) свойство и извлекать выбранный элемент из [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) Свойства. Кроме того, цвет текста для выбранного элемента можно изменить, задав [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) свойству значение [`Color`](xref:Xamarin.Forms.Color) .

## <a name="populating-a-picker-with-data"></a>Заполнение средства выбора данными

[`Picker`](xref:Xamarin.Forms.Picker)Может заполняться данными путем установки его [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) свойства в `IList` коллекцию. Каждый элемент в коллекции должен иметь тип или быть производным от типа `object` . Элементы можно добавлять в XAML путем инициализации `ItemsSource` свойства из массива элементов:

```xaml
<Picker x:Name="picker"
        Title="Select a monkey"
        TitleColor="Red">
  <Picker.ItemsSource>
    <x:Array Type="{x:Type x:String}">
      <x:String>Baboon</x:String>
      <x:String>Capuchin Monkey</x:String>
      <x:String>Blue Monkey</x:String>
      <x:String>Squirrel Monkey</x:String>
      <x:String>Golden Lion Tamarin</x:String>
      <x:String>Howler Monkey</x:String>
      <x:String>Japanese Macaque</x:String>
    </x:Array>
  </Picker.ItemsSource>
</Picker>
```

> [!NOTE]
> Обратите внимание на то, что для элемента `x:Array` требуется атрибут `Type`, указывающий тип элементов в массиве.

Эквивалентный код C# показан ниже:

```csharp
var monkeyList = new List<string>();
monkeyList.Add("Baboon");
monkeyList.Add("Capuchin Monkey");
monkeyList.Add("Blue Monkey");
monkeyList.Add("Squirrel Monkey");
monkeyList.Add("Golden Lion Tamarin");
monkeyList.Add("Howler Monkey");
monkeyList.Add("Japanese Macaque");

var picker = new Picker { Title = "Select a monkey", TitleColor = Color.Red };
picker.ItemsSource = monkeyList;
```

## <a name="responding-to-item-selection"></a>Реагирование на выбор элементов

[`Picker`](xref:Xamarin.Forms.Picker)Поддерживает выборку по одному элементу за раз. Когда пользователь выбирает элемент, [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) возникает событие, [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) свойство обновляется до целого числа, представляющего индекс выбранного элемента в списке, и [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) свойство обновляется до объекта, `object` представляющего выбранный элемент. Свойство — это номер, начинающийся с [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) нуля, указывающий элемент, выбранный пользователем. Если элемент не выбран, то есть при [`Picker`](xref:Xamarin.Forms.Picker) первом создании и инициализации, `SelectedIndex` будет равен-1.

> [!NOTE]
> Поведение выбора элементов в [`Picker`](xref:Xamarin.Forms.Picker) можно настроить в iOS с учетом конкретной платформы. Дополнительные сведения см. в разделе [Управление выбором элементов средства выбора](~/xamarin-forms/platform/ios/picker-selection.md).

В следующем примере кода показано, как получить [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) значение свойства из [`Picker`](xref:Xamarin.Forms.Picker) в XAML:

```xaml
<Label Text="{Binding Source={x:Reference picker}, Path=SelectedItem}" />
```

Эквивалентный код C# показан ниже:

```csharp
var monkeyNameLabel = new Label();
monkeyNameLabel.SetBinding(Label.TextProperty, new Binding("SelectedItem", source: picker));
```

Кроме того, при срабатывании события может выполняться обработчик событий [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) :

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs e)
{
  var picker = (Picker)sender;
  int selectedIndex = picker.SelectedIndex;

  if (selectedIndex != -1)
  {
    monkeyNameLabel.Text = (string)picker.ItemsSource[selectedIndex];
  }
}
```

Этот метод получает [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) значение свойства и использует значение для получения выбранного элемента из [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) коллекции. Это функционально эквивалентно извлечению выбранного элемента из [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) Свойства. Обратите внимание, что каждый элемент в `ItemsSource` коллекции имеет тип `object` и поэтому должен быть приведен к типу `string` для вывода.

> [!NOTE]
> [`Picker`](xref:Xamarin.Forms.Picker)Можно инициализировать для вывода определенного элемента, задав [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) свойства или. Однако эти свойства должны быть установлены после инициализации [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) коллекции.

## <a name="populating-a-picker-with-data-using-data-binding"></a>Заполнение средства выбора данными с помощью привязки данных

[`Picker`](xref:Xamarin.Forms.Picker)Можно также заполнить данными с помощью привязки данных, чтобы привязать его [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) свойство к `IList` коллекции. В XAML это достигается с помощью [`Binding`](xref:Xamarin.Forms.Xaml.BindingExtension) расширения разметки:

```xaml
<Picker Title="Select a monkey"
        TitleColor="Red"
        ItemsSource="{Binding Monkeys}"
        ItemDisplayBinding="{Binding Name}" />
```

Эквивалентный код C# показан ниже:

```csharp
var picker = new Picker { Title = "Select a monkey", TitleColor = Color.Red };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.ItemDisplayBinding = new Binding("Name");
```

[`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource)Данные свойства привязываются к `Monkeys` свойству подключенной модели представления, которая возвращает `IList<Monkey>` коллекцию. В следующем примере кода показан `Monkey` класс, который содержит четыре свойства:

```csharp
public class Monkey
{
  public string Name { get; set; }
  public string Location { get; set; }
  public string Details { get; set; }
  public string ImageUrl { get; set; }
}
```

При привязке к списку объектов [`Picker`](xref:Xamarin.Forms.Picker) необходимо указать, какое свойство должно отображаться из каждого объекта. Это достигается путем присвоения [`ItemDisplayBinding`](xref:Xamarin.Forms.Picker.ItemDisplayBinding) свойству требуемого свойства из каждого объекта. В приведенных выше примерах кода в `Picker` задано отображение каждого `Monkey.Name` значения свойства.

### <a name="responding-to-item-selection"></a>Реагирование на выбор элементов

Привязку данных можно использовать для присвоения объекту [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) значения свойства при его изменении:

```xaml
<Picker Title="Select a monkey"
        TitleColor="Red"
        ItemsSource="{Binding Monkeys}"
        ItemDisplayBinding="{Binding Name}"
        SelectedItem="{Binding SelectedMonkey}" />
<Label Text="{Binding SelectedMonkey.Name}" ... />
<Label Text="{Binding SelectedMonkey.Location}" ... />
<Image Source="{Binding SelectedMonkey.ImageUrl}" ... />
<Label Text="{Binding SelectedMonkey.Details}" ... />
```

Эквивалентный код C# показан ниже:

```csharp
var picker = new Picker { Title = "Select a monkey", TitleColor = Color.Red };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.SetBinding(Picker.SelectedItemProperty, "SelectedMonkey");
picker.ItemDisplayBinding = new Binding("Name");

var nameLabel = new Label { ... };
nameLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Name");

var locationLabel = new Label { ... };
locationLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Location");

var image = new Image { ... };
image.SetBinding(Image.SourceProperty, "SelectedMonkey.ImageUrl");

var detailsLabel = new Label();
detailsLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Details");
```

[`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem)Данные свойства привязываются к `SelectedMonkey` свойству подключенной модели представления, имеющей тип `Monkey` . Таким образом, когда пользователь выбирает элемент в [`Picker`](xref:Xamarin.Forms.Picker) , `SelectedMonkey` для свойства будет задан выбранный `Monkey` объект. `SelectedMonkey`Данные объекта отображаются в пользовательском интерфейсе в [`Label`](xref:Xamarin.Forms.Label) и [`Image`](xref:Xamarin.Forms.Image) представления:

![](populating-itemssource-images/monkeys.png "Picker Item Selection")

> [!NOTE]
> Обратите внимание, что [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) Свойства и поддерживают двустороннюю привязку по умолчанию.

## <a name="related-links"></a>Связанные ссылки

- [Демонстрация средства выбора (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo)
- [Приложение для обезьяны (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-monkeyapppicker)
- [Средство выбора с возможностью привязки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
- [API средства выбора](xref:Xamarin.Forms.Picker)
