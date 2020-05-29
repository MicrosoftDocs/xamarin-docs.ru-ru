---
title: Xamarin.FormsГруппирование CollectionView
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 51661d6573d78386f4ca8bfea0063b752295c2d8
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136465"
---
# <a name="xamarinforms-collectionview-grouping"></a>Xamarin.FormsГруппирование CollectionView

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

Большие наборы данных часто становятся неудобными, если они представлены в постоянном списке прокрутки. В этом сценарии Организация данных в группы может улучшить взаимодействие с пользователем, упрощая навигацию по данным.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)поддерживает отображение сгруппированных данных и определяет следующие свойства, которые определяют, как они будут представлены:

- `IsGrouped`Тип `bool` — указывает, должны ли базовые данные отображаться в группах. Значение по умолчанию этого свойства равно `false`.
- `GroupHeaderTemplate`Тип [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) — шаблон, используемый для заголовка каждой группы.
- `GroupFooterTemplate`Тип [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) — шаблон, используемый для нижнего колонтитула каждой группы.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что свойства могут быть целевыми объектами привязок данных.

На следующих снимках экрана показано [`CollectionView`](xref:Xamarin.Forms.CollectionView) Отображение сгруппированных данных:

[![Снимок экрана сгруппированных данных в CollectionView на iOS и Android](grouping-images/grouped-data.png "CollectionView с сгруппированными данными")](grouping-images/grouped-data-large.png#lightbox "CollectionView с сгруппированными данными")

Дополнительные сведения о шаблонах данных см. в разделе [ Xamarin.Forms шаблоны данных](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="group-data"></a>Данные группы

Данные должны быть сгруппированы, прежде чем их можно будет отобразить. Это можно сделать, создав список групп, где каждая группа является списком элементов. Список групп должен быть `IEnumerable<T>` коллекцией, где `T` определяет два фрагмента данных:

- Имя группы.
- `IEnumerable`Коллекция, определяющая элементы, принадлежащие группе.

Таким образом, процесс группирования данных состоит в следующих целях:

- Создание типа, моделирующего один элемент.
- Создайте тип, моделирующий одну группу элементов.
- Создайте `IEnumerable<T>` коллекцию, где `T` — это тип, моделирующий одну группу элементов. Таким образом, эта коллекция представляет собой коллекцию групп, в которой хранятся сгруппированные данные.
- Добавление данных в `IEnumerable<T>` коллекцию.

### <a name="example"></a>Пример

При группировании данных первым шагом является создание типа, моделирующего один элемент. В следующем примере показан `Animal` класс из примера приложения:

```csharp
public class Animal
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

`Animal`Класс моделирует один элемент. Затем можно создать тип, моделирующий группу элементов. В следующем примере показан `AnimalGroup` класс из примера приложения:

```csharp
public class AnimalGroup : List<Animal>
{
    public string Name { get; private set; }

    public AnimalGroup(string name, List<Animal> animals) : base(animals)
    {
        Name = name;
    }
}
```

`AnimalGroup`Класс наследует от `List<T>` класса и добавляет `Name` свойство, представляющее имя группы.

`IEnumerable<T>`Затем можно создать коллекцию групп:

```csharp
public List<AnimalGroup> Animals { get; private set; } = new List<AnimalGroup>();
```

Этот код определяет коллекцию с именем `Animals` , где каждый элемент в коллекции является `AnimalGroup` объектом. Каждый `AnimalGroup` объект состоит из имени и `List<Animal>` коллекции, определяющей `Animal` объекты в группе.

После этого сгруппированные данные можно добавить в `Animals` коллекцию:

```csharp
Animals.Add(new AnimalGroup("Bears", new List<Animal>
{
    new Animal
    {
        Name = "American Black Bear",
        Location = "North America",
        Details = "Details about the bear go here.",
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/0/08/01_Schwarzbär.jpg"
    },
    new Animal
    {
        Name = "Asian Black Bear",
        Location = "Asia",
        Details = "Details about the bear go here.",
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/b/b7/Ursus_thibetanus_3_%28Wroclaw_zoo%29.JPG/180px-Ursus_thibetanus_3_%28Wroclaw_zoo%29.JPG"
    },
    // ...
}));

Animals.Add(new AnimalGroup("Monkeys", new List<Animal>
{
    new Animal
    {
        Name = "Baboon",
        Location = "Africa & Asia",
        Details = "Details about the monkey go here.",
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
    },
    new Animal
    {
        Name = "Capuchin Monkey",
        Location = "Central & South America",
        Details = "Details about the monkey go here.",
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg"
    },
    new Animal
    {
        Name = "Blue Monkey",
        Location = "Central and East Africa",
        Details = "Details about the monkey go here.",
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg"
    },
    // ...
}));
```

Этот код создает две группы в `Animals` коллекции. Первый `AnimalGroup` называется `Bears` и содержит `List<Animal>` коллекцию сведений о себе. Второй объект называется `AnimalGroup` `Monkeys` и содержит `List<Animal>` коллекцию сведений о обезьяне.

## <a name="display-grouped-data"></a>Отображение сгруппированных данных

[`CollectionView`](xref:Xamarin.Forms.CollectionView)Отображает сгруппированные данные при условии, что данные были сгруппированы правильно, путем присвоения `IsGrouped` свойству значения `true` :

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                IsGrouped="true">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                ...
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    IsGrouped = true
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
// ...
```

Внешний вид каждого элемента в определяется [`CollectionView`](xref:Xamarin.Forms.CollectionView) путем присвоения [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) свойству значения [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . Дополнительные сведения см. в разделе [Определение внешнего вида элемента](~/xamarin-forms/user-interface/collectionview/populate-data.md#define-item-appearance).

> [!NOTE]
> По умолчанию [`CollectionView`](xref:Xamarin.Forms.CollectionView) будет отображать имя группы в верхнем и нижнем колонтитулах группы. Это поведение можно изменить, настроив заголовок группы и нижний колонтитул группы.

## <a name="customize-the-group-header"></a>Настройка заголовка группы

Внешний вид каждого заголовка группы можно настроить, задав `CollectionView.GroupHeaderTemplate` для свойства значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) .

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                IsGrouped="true">
    ...
    <CollectionView.GroupHeaderTemplate>
        <DataTemplate>
            <Label Text="{Binding Name}"
                   BackgroundColor="LightGray"
                   FontSize="Large"
                   FontAttributes="Bold" />
        </DataTemplate>
    </CollectionView.GroupHeaderTemplate>
</CollectionView>
```

В этом примере каждому заголовку группы присваивается значение [`Label`](xref:Xamarin.Forms.Label) , в котором отображается имя группы, а также заданы другие свойства внешнего вида. На следующих снимках экрана показан заголовок настроенной группы:

[![Снимок экрана с настроенным заголовком группы в CollectionView на iOS и Android](grouping-images/customized-header.png "CollectionView с настроенным заголовком группы")](grouping-images/customized-header-large.png#lightbox "CollectionView с настроенным заголовком группы")

## <a name="customize-the-group-footer"></a>Настройка нижнего колонтитула группы

Внешний вид каждого нижнего колонтитула группы можно настроить, задав `CollectionView.GroupFooterTemplate` для свойства значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) .

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                IsGrouped="true">
    ...
    <CollectionView.GroupFooterTemplate>
        <DataTemplate>
            <Label Text="{Binding Count, StringFormat='Total animals: {0:D}'}"
                   Margin="0,0,0,10" />
        </DataTemplate>
    </CollectionView.GroupFooterTemplate>
</CollectionView>
```

В этом примере каждый нижний колонтитул группы имеет значение [`Label`](xref:Xamarin.Forms.Label) , которое отображает количество элементов в группе. На следующих снимках экрана показан настраиваемый нижний колонтитул группы:

[![Снимок экрана с настроенным нижним колонтитулом группы в CollectionView на iOS и Android](grouping-images/customized-footer.png "CollectionView с настроенным нижним колонтитулом группы")](grouping-images/customized-footer-large.png#lightbox "CollectionView с настроенным нижним колонтитулом группы")

## <a name="empty-groups"></a>Пустые группы

Когда [`CollectionView`](xref:Xamarin.Forms.CollectionView) отображает сгруппированные данные, будут отображены все непустые группы. Такие группы будут отображаться с верхним и нижним колонтитулами группы, что означает, что группа пуста. На следующих снимках экрана показана пустая группа:

[![Снимок экрана пустой группы в CollectionView на iOS и Android](grouping-images/empty-group.png "CollectionView с пустой группой")](grouping-images/empty-group-large.png#lightbox "CollectionView с пустой группой")

> [!NOTE]
> В iOS 10 и более низких колонтитулы группы и нижние колонтитулы для пустых групп могут отображаться в верхней части `CollectionView` .

## <a name="group-without-templates"></a>Группа без шаблонов

[`CollectionView`](xref:Xamarin.Forms.CollectionView)может отображать правильно сгруппированные данные, не присвоив [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) свойству значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) .

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                IsGrouped="true" />
```

В этом сценарии значимые данные могут отображаться путем переопределения `ToString` метода в типе, моделирующем один элемент, и типа, моделирующего одну группу элементов.

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin.FormsШаблоны данных](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
