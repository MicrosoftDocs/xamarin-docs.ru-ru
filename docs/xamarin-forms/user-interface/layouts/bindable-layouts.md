---
title: Связываемые макеты в Xamarin.Forms
description: Макеты с возможностью привязки позволяют классам макета создавать свое содержимое путем привязки к коллекции элементов с параметром для установки внешнего вида каждого элемента с помощью DataTemplate.
ms.prod: xamarin
ms.assetid: 824C3319-20A0-42D0-8632-CDECD98349C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/09/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f8bd745f2fee5e2e1b8bb03d474578127a360ac2
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93367677"
---
# <a name="bindable-layouts-in-no-locxamarinforms"></a>Связываемые макеты в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)

Макеты с возможностью привязки позволяют любому классу макета, производному от [`Layout<T>`](xref:Xamarin.Forms.Layout`1) класса, создавать его содержимое путем привязки к коллекции элементов с возможностью задать внешний вид каждого элемента с помощью [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . Связываемые макеты предоставляются `BindableLayout` классом, который предоставляет следующие вложенные свойства:

- `ItemsSource` — Указывает коллекцию элементов, `IEnumerable` отображаемых макетом.
- `ItemTemplate` — Задает объект, [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) применяемый к каждому элементу в коллекции элементов, отображаемых макетом.
- `ItemTemplateSelector` — Указывает [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) , который будет использоваться для выбора [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) элемента во время выполнения.

> [!NOTE]
> `ItemTemplate`Свойство имеет приоритет, если `ItemTemplate` `ItemTemplateSelector` заданы свойства и.

Кроме того, `BindableLayout` класс предоставляет следующие привязываемые свойства:

- `EmptyView` — Указывает `string` представление или, которое будет отображаться `ItemsSource` , если свойство имеет значение `null` , или если коллекция, указанная `ItemsSource` свойством, имеет значение `null` или пустое значение. Значение по умолчанию — `null`.
- `EmptyViewTemplate` — Указывает [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , который будет отображаться, если `ItemsSource` свойство имеет значение `null` , или если коллекция, указанная `ItemsSource` свойством, имеет значение `null` или пустое значение. Значение по умолчанию — `null`.

> [!NOTE]
> `EmptyViewTemplate`Свойство имеет приоритет, если `EmptyView` `EmptyViewTemplate` заданы свойства и.

Все эти свойства могут быть присоединены к [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) [`Grid`](xref:Xamarin.Forms.Grid) классам,,, [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) и [`StackLayout`](xref:Xamarin.Forms.StackLayout) , которые все являются производными от [`Layout<T>`](xref:Xamarin.Forms.Layout`1) класса.

`Layout<T>`Класс предоставляет [`Children`](xref:Xamarin.Forms.Layout`1.Children) коллекцию, в которую добавляются дочерние элементы макета. Если `BinableLayout.ItemsSource` свойство установлено в коллекцию элементов и присоединяется к [`Layout<T>`](xref:Xamarin.Forms.Layout`1) классу, производному от, каждый элемент в коллекции добавляется в `Layout<T>.Children` коллекцию для просмотра макетом. `Layout<T>`Класс, производный от, обновит свои дочерние представления при изменении базовой коллекции. Дополнительные сведения о Xamarin.Forms цикле макета см. в разделе [Создание пользовательского макета](~/xamarin-forms/user-interface/layouts/custom.md).

Макеты с возможностью привязки следует использовать только в том случае, если коллекция отображаемых элементов невелика, а прокрутка и выбор не требуются. Хотя возможность прокрутки может быть предоставлена путем заключения в оболочку связываемого макета [`ScrollView`](xref:Xamarin.Forms.ScrollView) , это не рекомендуется, так как в структурах, поддерживающих связывание, отсутствует виртуализация пользовательского интерфейса. Если требуется прокрутка, следует использовать прокручиваемое представление, которое включает в себя виртуализацию пользовательского интерфейса, например [`ListView`](xref:Xamarin.Forms.ListView) или [`CollectionView`](xref:Xamarin.Forms.CollectionView) . Несоблюдение этой рекомендации может привести к проблемам с производительностью.

> [!IMPORTANT]
> Хотя технически можно присоединить связываемый макет к любому классу макета, производному от [`Layout<T>`](xref:Xamarin.Forms.Layout`1) класса, это не всегда целесообразно, особенно для [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) [`Grid`](xref:Xamarin.Forms.Grid) классов, и [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) . Например, рассмотрим ситуацию, когда нужно отобразить коллекцию данных в с [`Grid`](xref:Xamarin.Forms.Grid) использованием связываемого макета, где каждый элемент в коллекции является объектом, содержащим несколько свойств. Каждая строка в `Grid` должна отображать объект из коллекции, каждый столбец в котором `Grid` отображает одно из свойств объекта. Поскольку [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) для связываемого макета может содержаться только один объект, необходимо, чтобы этот объект был классом макета, содержащим несколько представлений, каждый из которых отображает одно из свойств объекта в определенном `Grid` столбце. Хотя этот сценарий может быть реализован с помощью связываемых макетов, он приводит к использованию родителя, `Grid` содержащего дочерний `Grid` элемент для каждого элемента в привязанной коллекции, что является очень неэффективным и проблематичным использованием `Grid` макета.

## <a name="populate-a-bindable-layout-with-data"></a>Заполнение связываемого макета данными

Связываемый макет заполняется данными путем установки его `ItemsSource` свойства в любую коллекцию, реализующую `IEnumerable` , и присоединение ее к [`Layout<T>`](xref:Xamarin.Forms.Layout`1) производному классу:

```xaml
<Grid BindableLayout.ItemsSource="{Binding Items}" />
```

Эквивалентный код на C# выглядит так:

```csharp
IEnumerable<string> items = ...;
var grid = new Grid();
BindableLayout.SetItemsSource(grid, items);
```

Если `BindableLayout.ItemsSource` вложенное свойство задано для макета, но `BindableLayout.ItemTemplate` присоединенное свойство не задано, каждый элемент в `IEnumerable` коллекции будет отображаться объектом [`Label`](xref:Xamarin.Forms.Label) , созданным `BindableLayout` классом.

## <a name="define-item-appearance"></a>Определение внешнего вида элемента

Внешний вид каждого элемента в макете с возможностью привязки можно определить, установив `BindableLayout.ItemTemplate` для присоединенного свойства значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) .

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding User.TopFollowers}"
             Orientation="Horizontal"
             ...>
    <BindableLayout.ItemTemplate>
        <DataTemplate>
            <controls:CircleImage Source="{Binding}"
                                  Aspect="AspectFill"
                                  WidthRequest="44"
                                  HeightRequest="44"
                                  ... />
        </DataTemplate>
    </BindableLayout.ItemTemplate>
</StackLayout>
```

Эквивалентный код на C# выглядит так:

```csharp
DataTemplate circleImageTemplate = ...;
var stackLayout = new StackLayout();
BindableLayout.SetItemsSource(stackLayout, viewModel.User.TopFollowers);
BindableLayout.SetItemTemplate(stackLayout, circleImageTemplate);
```

В этом примере каждый элемент в `TopFollowers` коллекции будет отображаться в `CircleImage` представлении, определенном в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) :

![Связываемый макет с DataTemplate](bindable-layouts-images/top-followers.png "Связываемый макет с шаблоном данных")

Дополнительные сведения о шаблонах данных см. в разделе [Общие сведения о шаблонах данныхXamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choose-item-appearance-at-runtime"></a>Выбор внешнего вида элемента во время выполнения

Внешний вид каждого элемента в макете с возможностью привязки можно выбрать во время выполнения на основе значения элемента, установив `BindableLayout.ItemTemplateSelector` для присоединенного свойства значение [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) :

```xaml
<FlexLayout BindableLayout.ItemsSource="{Binding User.FavoriteTech}"
            BindableLayout.ItemTemplateSelector="{StaticResource TechItemTemplateSelector}"
            ... />
```

Эквивалентный код на C# выглядит так:

```csharp
DataTemplateSelector dataTemplateSelector = new TechItemTemplateSelector { ... };
var flexLayout = new FlexLayout();
BindableLayout.SetItemsSource(flexLayout, viewModel.User.FavoriteTech);
BindableLayout.SetItemTemplateSelector(flexLayout, dataTemplateSelector);
```

Объект, [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) используемый в примере приложения, показан в следующем примере:

```csharp
public class TechItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate XamarinFormsTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return (string)item == "Xamarin.Forms" ? XamarinFormsTemplate : DefaultTemplate;
    }
}
```

`TechItemTemplateSelector`Класс определяет `DefaultTemplate` Свойства и `XamarinFormsTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , для которых установлены разные шаблоны данных. `OnSelectTemplate`Метод возвращает `XamarinFormsTemplate` , который отображает элемент темно-красного цвета с сердце рядом с ним, когда элемент равен " Xamarin.Forms ". Если элемент не равен " Xamarin.Forms ", `OnSelectTemplate` метод возвращает объект `DefaultTemplate` , который отображает элемент, используя цвет по умолчанию [`Label`](xref:Xamarin.Forms.Label) :

![Связываемый макет с DataTemplateSelector](bindable-layouts-images/favorite-tech.png "Связываемый макет с помощью селектора шаблона данных")

Дополнительные сведения о селекторах шаблонов данных см. [в разделе Создание Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="display-a-string-when-data-is-unavailable"></a>Отображать строку, если данные недоступны

`EmptyView`Свойству может быть присвоена строка, которая будет отображаться объектом, [`Label`](xref:Xamarin.Forms.Label) Если `ItemsSource` свойство имеет значение `null` , или если коллекция, указанная `ItemsSource` свойством, имеет значение `null` или является пустым. В следующем коде XAML показан пример этого сценария:

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}"
             BindableLayout.EmptyView="No achievements">
    ...
</StackLayout>
```

Результат заключается в том, что если коллекция с привязкой к данным имеет `null` значение, то строка, заданная в качестве `EmptyView` значения свойства, отображается следующим образом:

[![Снимок экрана: пустое представление строки макета с возможностью привязки в iOS и Android](bindable-layouts-images/emptyview-string.png "Пустое представление строки макета с возможностью привязки")](bindable-layouts-images/emptyview-string-large.png#lightbox "Пустое представление строки макета с возможностью привязки")

## <a name="display-views-when-data-is-unavailable"></a>Отображать представления, если данные недоступны

`EmptyView`Для свойства можно задать представление, которое будет отображаться, если `ItemsSource` свойство имеет значение `null` , или если коллекция, указанная `ItemsSource` свойством, имеет значение `null` или является пустым. Это может быть одно представление или представление, содержащее несколько дочерних представлений. В следующем примере XAML показано `EmptyView` свойство, для которого задано представление, содержащее несколько дочерних представлений:

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}">
    <BindableLayout.EmptyView>
        <StackLayout>
            <Label Text="None."
                   FontAttributes="Italic"
                   FontSize="{StaticResource smallTextSize}" />
            <Label Text="Try harder and return later?"
                   FontAttributes="Italic"
                   FontSize="{StaticResource smallTextSize}" />
        </StackLayout>
    </BindableLayout.EmptyView>
    ...
</StackLayout>
```

В результате, если коллекция привязана к данным `null` , то [`StackLayout`](xref:Xamarin.Forms.StackLayout) отображаются и ее дочерние представления.

[![Снимок экрана: пустое представление макета с возможностью привязки с несколькими представлениями в iOS и Android](bindable-layouts-images/emptyview-views.png "Пустое представление связываемого макета")](bindable-layouts-images/emptyview-views-large.png#lightbox "Пустое представление связываемого макета")

Аналогичным образом `EmptyViewTemplate` можно задать значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , которое будет отображаться, если `ItemsSource` свойство имеет значение `null` , или если коллекция, указанная `ItemsSource` свойством, является `null` или пустой. `DataTemplate`Может содержать одно представление или представление, содержащее несколько дочерних представлений. Кроме того, объект `BindingContext` `EmptyViewTemplate` будет унаследован от `BindingContext` класса `BindableLayout` . В следующем примере XAML показано `EmptyViewTemplate` свойство, для которого задано значение `DataTemplate` , содержащее одно представление:

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}">
    <BindableLayout.EmptyViewTemplate>
        <DataTemplate>
            <Label Text="{Binding Source={x:Reference usernameLabel}, Path=Text, StringFormat='{0} has no achievements.'}" />
        </DataTemplate>
    </BindableLayout.EmptyViewTemplate>
    ...
</StackLayout>
```

В результате, если коллекция привязана к данным `null` , [`Label`](xref:Xamarin.Forms.Label) в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) отображается:

[![Снимок экрана шаблона пустого представления с возможностью привязки в iOS и Android](bindable-layouts-images/emptyviewtemplate.png "Шаблон пустого представления связываемого макета")](bindable-layouts-images/emptyviewtemplate-large.png#lightbox "Шаблон пустого представления связываемого макета")

> [!NOTE]
> `EmptyViewTemplate`Свойство не может быть задано через [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) .

## <a name="choose-an-emptyview-at-runtime"></a>Выбор Емптивиев во время выполнения

Представления, которые будут отображаться `EmptyView` при недоступности данных, могут быть определены как [`ContentView`](xref:Xamarin.Forms.ContentView) объекты в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . `EmptyView`Во время выполнения для свойства можно задать конкретное значение `ContentView` , основанное на определенной бизнес-логике. В следующем коде XAML показан пример этого сценария:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        ...    
        <ContentView x:Key="BasicEmptyView">
            <StackLayout>
                <Label Text="No achievements."
                       FontSize="14" />
            </StackLayout>
        </ContentView>
        <ContentView x:Key="AdvancedEmptyView">
            <StackLayout>
                <Label Text="None."
                       FontAttributes="Italic"
                       FontSize="14" />
                <Label Text="Try harder and return later?"
                       FontAttributes="Italic"
                       FontSize="14" />
            </StackLayout>
        </ContentView>
    </ContentPage.Resources>

    <StackLayout>
        ...
        <Switch Toggled="OnEmptyViewSwitchToggled" />

        <StackLayout x:Name="stackLayout"
                     BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}">
            ...
        </StackLayout>
    </StackLayout>
</ContentPage>
```

XAML определяет два [`ContentView`](xref:Xamarin.Forms.ContentView) объекта на уровне страницы [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) и [`Switch`](xref:Xamarin.Forms.Switch) объект, управляющий тем, какой `ContentView` объект будет задан как `EmptyView` значение свойства. При `Switch` переключении `OnEmptyViewSwitchToggled` обработчик событий выполняет `ToggleEmptyView` метод:

```csharp
void ToggleEmptyView(bool isToggled)
{
    object view = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
    BindableLayout.SetEmptyView(stackLayout, view);
}
```

`ToggleEmptyView`Метод задает `EmptyView` `stackLayout` для свойства объекта один из двух [`ContentView`](xref:Xamarin.Forms.ContentView) объектов, хранящихся в, в зависимости от [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) значения [`Switch.IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) Свойства. Затем, когда коллекция, привязанная к данным `null` , имеет `ContentView` значение, набор объектов будет `EmptyView` отображаться как свойство:

[![Снимок экрана: пустое представление выбора во время выполнения в iOS и Android](bindable-layouts-images/emptyview-runtime.png "Вариант незаполненного представления среды выполнения для связываемого макета")](bindable-layouts-images/emptyview-runtime-large.png#lightbox "Вариант незаполненного представления среды выполнения для связываемого макета")

## <a name="related-links"></a>Связанные ссылки

- [Демонстрация компоновки с возможностью привязки (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [Создание пользовательского макета](~/xamarin-forms/user-interface/layouts/custom.md)
- [Xamarin.Forms Шаблоны данных](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Создание Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)