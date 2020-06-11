---
Title: "ListView Data Sources" Description: "в этой статье объясняется, как заполнить Xamarin.Forms ListView данными и как использовать привязку данных с ListView".
MS. произв. Xamarin MS. AssetID: B5571660-1E82-4379-95C3-0725288CF5D9 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 03/23/2020 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="listview-data-sources"></a>Источники данных ListView

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)

Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) Используется для отображения списков данных. В этой статье объясняется, как заполнить `ListView` с помощью данных и как привязать данные к выбранному элементу.

## <a name="itemssource"></a>ItemsSource

[`ListView`](xref:Xamarin.Forms.ListView)Заполняется данными с помощью [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) свойства, которое может принимать любые реализации, реализующие коллекцию `IEnumerable` . Самый простой способ заполнения `ListView` включает использование массива строк:

```xaml
<ListView>
      <ListView.ItemsSource>
          <x:Array Type="{x:Type x:String}">
            <x:String>mono</x:String>
            <x:String>monodroid</x:String>
            <x:String>monotouch</x:String>
            <x:String>monorail</x:String>
            <x:String>monodevelop</x:String>
            <x:String>monotone</x:String>
            <x:String>monopoly</x:String>
            <x:String>monomodal</x:String>
            <x:String>mononucleosis</x:String>
          </x:Array>
      </ListView.ItemsSource>
</ListView>
```

Эквивалентный код на C# выглядит так:

```csharp
var listView = new ListView();
listView.ItemsSource = new string[]
{
  "mono",
  "monodroid",
  "monotouch",
  "monorail",
  "monodevelop",
  "monotone",
  "monopoly",
  "monomodal",
  "mononucleosis"
};
```

![](data-and-databinding-images/itemssource-simple.png "ListView Displaying List of Strings")

Этот подход будет заполняться `ListView` списком строк. По умолчанию `ListView` будет вызывать `ToString` и отображать результат в `TextCell` каждой строке. Сведения о настройке отображения данных см. в разделе [внешний вид ячеек](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

Поскольку объект был `ItemsSource` отправлен в массив, содержимое не будет обновляться при изменении базового списка или массива. Если необходимо, чтобы ListView автоматически подменялся при добавлении, удалении и изменении элементов в базовом списке, необходимо использовать `ObservableCollection` . [`ObservableCollection`](xref:System.Collections.ObjectModel.ObservableCollection`1)определен в `System.Collections.ObjectModel` и, как и `List` , за исключением того, что он может уведомлять о `ListView` любых изменениях:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
listView.ItemsSource = employees;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employees.Add(new Employee(){ DisplayName="Mr. Mono"});
```

## <a name="data-binding"></a>Привязка данных

Привязка данных — это «привязывание», связывающая свойства объекта пользовательского интерфейса со свойствами некоторого объекта CLR, например класса в ViewModel. Привязка данных полезна, поскольку она упрощает разработку пользовательских интерфейсов, заменяя множество скучных стандартных кодов.

Привязка данных работает, сохраняя синхронизацию объектов при изменении их привязанных значений. Вместо того, чтобы создавать обработчики событий для каждого изменения значения элемента управления, необходимо установить привязку и включить привязку в ViewModel.

Дополнительные сведения о привязке данных см. в статье [основы привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) , которая является частью четвертой серии статей, посвященных [ Xamarin.Forms основам XAML](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="binding-cells"></a>Привязка ячеек

Свойства ячеек (и потомков ячеек) можно привязать к свойствам объектов в `ItemsSource` . Например, `ListView` можно использовать для представления списка сотрудников.

Класс Employee:

```csharp
public class Employee
{
    public string DisplayName {get; set;}
}
```

`ObservableCollection<Employee>`Создается, задается как `ListView` `ItemsSource` , а список заполняется данными:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
public ObservableCollection<Employee> Employees { get { return employees; }}

public EmployeeListPage()
{
    EmployeeView.ItemsSource = employees;

    // ObservableCollection allows items to be added after ItemsSource
    // is set and the UI will react to changes
    employees.Add(new Employee{ DisplayName="Rob Finnerty"});
    employees.Add(new Employee{ DisplayName="Bill Wrestler"});
    employees.Add(new Employee{ DisplayName="Dr. Geri-Beth Hooper"});
    employees.Add(new Employee{ DisplayName="Dr. Keith Joyce-Purdy"});
    employees.Add(new Employee{ DisplayName="Sheri Spruce"});
    employees.Add(new Employee{ DisplayName="Burt Indybrick"});
}
```

> [!WARNING]
> Хотя `ListView` будет обновляться в ответ на изменения в его базовом виде `ObservableCollection` , `ListView` не будет обновляться, если `ObservableCollection` исходной ссылке назначен другой экземпляр `ObservableCollection` (например, `employees = otherObservableCollection;` ).

В следующем фрагменте кода показана `ListView` Привязка к списку сотрудников:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
             x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
             Title="Employee List">
  <ListView x:Name="EmployeeView"
            ItemsSource="{Binding Employees}">
    <ListView.ItemTemplate>
      <DataTemplate>
        <TextCell Text="{Binding DisplayName}" />
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

В этом примере XAML определяется объект `ContentPage` , содержащий `ListView` . Источник данных для `ListView` задается с помощью атрибута `ItemsSource`. Макет каждой строки в `ItemsSource` определяется в элементе `ListView.ItemTemplate`. Это приводит к следующим снимкам экрана:

![](data-and-databinding-images/bound-data.png "ListView using Data Binding")

> [!WARNING]
> `ObservableCollection`Интерфейс  не является потокобезопасным. Изменение `ObservableCollection` приводит к тому, что обновления пользовательского интерфейса происходят в том же потоке, в котором были выполнены изменения. Если поток не является основным потоком пользовательского интерфейса, это вызовет исключение.

### <a name="binding-selecteditem"></a>Привязка SelectedItem

Часто требуется выполнить привязку к выбранному элементу `ListView` , а не использовать обработчик событий для реагирования на изменения. Чтобы сделать это в XAML, привяжите `SelectedItem` свойство:

```xaml
<ListView x:Name="listView"
          SelectedItem="{Binding Source={x:Reference SomeLabel},
          Path=Text}">
 …
</ListView>
```

Если `listView` `ItemsSource` является списком строк, `SomeLabel` `Text` свойство будет привязано к `SelectedItem` .

## <a name="related-links"></a>Связанные ссылки

- [Двусторонняя привязка (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
