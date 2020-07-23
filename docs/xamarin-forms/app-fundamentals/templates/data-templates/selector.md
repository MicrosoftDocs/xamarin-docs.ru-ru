---
title: Создание DataTemplateSelector в Xamarin.Forms
description: В этой статье показано, как создавать и использовать объект DataTemplateSelector для выбора шаблона DataTemplate во время выполнения в соответствии со значением привязанного к данным свойства.
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7cbeeb9a0eed37ec109b2e71c46e3f04cd08822d
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936452"
---
# <a name="creating-a-xamarinforms-datatemplateselector"></a>Создание DataTemplateSelector в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplateselector)

_С помощью объекта DataTemplateSelector можно выбирать шаблон DataTemplate во время выполнения в соответствии со значением привязанного к данным свойства. Это позволяет применять несколько шаблонов DataTemplate к одному типу объектов, чтобы настроить внешний вид определенных объектов. В этой статье содержатся сведения о создании и использовании объекта DataTemplateSelector._

Селектор шаблона данных обеспечивает такие сценарии, как привязка [`ListView`](xref:Xamarin.Forms.ListView) к коллекции объектов для выбора внешнего вида каждого объекта в `ListView` во время выполнения с помощью селектора, возвращающего определенный шаблон [`DataTemplate`](xref:Xamarin.Forms.DataTemplate).

## <a name="creating-a-datatemplateselector"></a>Создание DataTemplateSelector

Селектор шаблона данных реализуется путем создания класса, наследуемого от [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector). Метод `OnSelectTemplate` затем переопределяется для возврата определенного шаблона [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), как показано в следующем примере кода.

```csharp
public class PersonDataTemplateSelector : DataTemplateSelector
{
  public DataTemplate ValidTemplate { get; set; }
  public DataTemplate InvalidTemplate { get; set; }

  protected override DataTemplate OnSelectTemplate (object item, BindableObject container)
  {
    return ((Person)item).DateOfBirth.Year >= 1980 ? ValidTemplate : InvalidTemplate;
  }
}
```

Метод `OnSelectTemplate` возвращает шаблон в соответствии со значением свойства `DateOfBirth`. Возвращаемый шаблон — это значение свойства `ValidTemplate` или `InvalidTemplate`, которые задаются при использовании `PersonDataTemplateSelector`.

После этого экземпляр класса, представляющего селектор шаблона данных, можно присваивать свойствам элементов управления Xamarin.Forms, таких как [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1). Список допустимых свойств см. в статье [Создание DataTemplate](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md).

### <a name="limitations"></a>Ограничения

На экземпляры [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) налагаются указанные ниже ограничения.

- При повторных запросах подкласс `DataTemplateSelector` должен возвращать один и тот же шаблон для определенных данных.
- Подкласс `DataTemplateSelector` не должен возвращать другой подкласс `DataTemplateSelector`.
- Подкласс `DataTemplateSelector` не должен возвращать новые экземпляры `DataTemplate` при каждом вызове. Должен возвращаться один и тот же экземпляр. В противном случае будет происходить утечка памяти и виртуализация будет отключена.
- В Android для каждого элемента `ListView` может быть не более 20 разных шаблонов данных.

## <a name="consuming-a-datatemplateselector-in-xaml"></a>Использование DataTemplateSelector в XAML

В XAML экземпляр `PersonDataTemplateSelector` можно создать, объявив его как ресурс, как показано в следующем примере кода.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Selector;assembly=Selector" x:Class="Selector.HomePage">
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="validPersonTemplate">
                <ViewCell>
                   ...
                </ViewCell>
            </DataTemplate>
            <DataTemplate x:Key="invalidPersonTemplate">
                <ViewCell>
                   ...
                </ViewCell>
            </DataTemplate>
            <local:PersonDataTemplateSelector x:Key="personDataTemplateSelector"
                ValidTemplate="{StaticResource validPersonTemplate}"
                InvalidTemplate="{StaticResource invalidPersonTemplate}" />
        </ResourceDictionary>
    </ContentPage.Resources>
  ...
</ContentPage>
```

В этом словаре [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) на уровне страницы определены два экземпляра [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) и экземпляр `PersonDataTemplateSelector`. Свойствам `ValidTemplate` и `InvalidTemplate` экземпляра `PersonDataTemplateSelector` присвоены соответствующие экземпляры `DataTemplate` с помощью расширения разметки `StaticResource`. Обратите внимание на то, что ресурсы могут определяться не только в объекте [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) страницы, но и на уровне элемента управления или приложения.

Экземпляр `PersonDataTemplateSelector` используется путем присвоения свойству [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1), как показано в следующем примере кода.

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

Во время выполнения [`ListView`](xref:Xamarin.Forms.ListView) вызывает метод `PersonDataTemplateSelector.OnSelectTemplate` для каждого элемента в базовой коллекции. В вызове объект данных передается в качестве параметра `item`. Затем шаблон [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), возвращенный методом, применяется к этому объекту.

На следующих снимках экрана показан результат применения `PersonDataTemplateSelector` к каждому объекту базовой коллекции в [`ListView`](xref:Xamarin.Forms.ListView).

![ListView с селектором шаблона данных](selector-images/data-template-selector.png)

Все объекты `Person`, свойство `DateOfBirth` которых имеет значение не меньше 1980, отображаются зеленым цветом. Остальные объекты отображаются красным цветом.

## <a name="consuming-a-datatemplateselector-in-cnum"></a>Использование DataTemplateSelector на C&num;

На C# экземпляр `PersonDataTemplateSelector` можно создать и присвоить свойству [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1), как показано в следующем примере кода.

```csharp
public class HomePageCS : ContentPage
{
  DataTemplate validTemplate;
  DataTemplate invalidTemplate;

  public HomePageCS ()
  {
    ...
    SetupDataTemplates ();
    var listView = new ListView {
      ItemsSource = people,
      ItemTemplate = new PersonDataTemplateSelector {
        ValidTemplate = validTemplate,
        InvalidTemplate = invalidTemplate }
    };

    Content = new StackLayout {
      Margin = new Thickness (20),
      Children = {
        ...
        listView
      }
    };
  }
  ...  
}
```

Свойствам `ValidTemplate` и `InvalidTemplate` экземпляра `PersonDataTemplateSelector` присвоены соответствующие экземпляры [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), созданные методом `SetupDataTemplates`. Во время выполнения [`ListView`](xref:Xamarin.Forms.ListView) вызывает метод `PersonDataTemplateSelector.OnSelectTemplate` для каждого элемента в базовой коллекции. В вызове объект данных передается в качестве параметра `item`. Затем шаблон `DataTemplate`, возвращенный методом, применяется к этому объекту.

## <a name="summary"></a>Сводка

В этой статье было показано, как создать и использовать [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector). Объект `DataTemplateSelector` можно использовать для выбора [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) в среде выполнения на основе значения свойства, привязанного к данным. Это позволяет применять несколько экземпляров `DataTemplate` к одному типу объектов, чтобы настроить внешний вид определенных объектов.

## <a name="related-links"></a>Связанные ссылки

- [Селектор шаблона данных (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplateselector)
- [DataTemplateSelector](xref:Xamarin.Forms.DataTemplateSelector)
