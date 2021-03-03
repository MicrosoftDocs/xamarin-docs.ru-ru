---
title: Относительные привязки Xamarin.Forms
description: Эта статья описывает, как создать относительные привязки с помощью расширения разметки RelativeSource, чтобы задать источник привязки относительно положения целевого объекта привязки.
ms.prod: xamarin
ms.assetid: CC64BB1D-8303-46B1-94B6-4EF2F20317A8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 859f5d3c196a572aa22a5f5a29801ad4ee44441a
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374593"
---
# <a name="xamarinforms-relative-bindings"></a>Относительные привязки Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/databindingdemos)

Относительные привязки позволяют задать источник привязки относительно положения целевого объекта привязки. Они создаются с помощью расширения разметки `RelativeSource` и задаются в качестве свойства `Source` выражения привязки.

Расширение разметки `RelativeSource` поддерживается классом `RelativeSourceExtension`, определяющим следующие свойства:

- `Mode` с типом `RelativeBindingSourceMode` описывает расположение источника привязки относительно положения целевого объекта привязки.
- `AncestorLevel` с типом `int` является необязательным уровнем предка для поиска, когда свойство `Mode` имеет значение `FindAncestor`. `AncestorLevel` `n` пропускает `n-1` экземпляры `AncestorType`.
- `AncestorType` с типом `Type` является типом предка для поиска, когда свойство `Mode` имеет значение `FindAncestor`.

> [!NOTE]
> Средство синтаксического анализа XAML позволяет сократить класс `RelativeSourceExtension` как `RelativeSource`.

Для свойства `Mode` должен быть задан один из членов перечисления `RelativeBindingSourceMode`.

- `TemplatedParent` указывает элемент, к которому применяется шаблон, где существует связанный элемент. Дополнительные сведения см. в разделе [Привязка к шаблонному родительскому элементу](#bind-to-a-templated-parent).
- `Self` указывает элемент, в котором настраивается привязка, позволяя привязать одно свойство этого элемента к другому свойству того же элемента. Дополнительные сведения см. в разделе [Привязка к самому себе](#bind-to-self).
- `FindAncestor` указывает предка в визуальном дереве связанного элемента. Этот режим следует использовать для привязки к элементу управления предка, представленному свойством `AncestorType`. Дополнительные сведения см. в разделе [Привязка к предку](#bind-to-an-ancestor).
- `FindAncestorBindingContext` указывает `BindingContext` предка в визуальном дереве связанного элемента. Этот режим следует использовать для привязки к `BindingContext` предка, представленному свойством `AncestorType`. Дополнительные сведения см. в разделе [Привязка к предку](#bind-to-an-ancestor).

Свойство `Mode` — это свойство содержимого класса `RelativeSourceExtension`. Таким образом, для выражений разметки XAML с фигурными скобками можно исключить часть `Mode=` выражения.

Дополнительные сведения о расширениях разметки Xamarin.Forms см. в статье [Расширения разметки XAML](~/xamarin-forms/xaml/markup-extensions/index.md).

## <a name="bind-to-self"></a>Привязка к самому себе

Режим относительной привязки `Self` используется для привязки свойства элемента к другому свойству того же элемента:

```xaml
<BoxView Color="Red"
         WidthRequest="200"
         HeightRequest="{Binding Source={RelativeSource Self}, Path=WidthRequest}"
         HorizontalOptions="Center" />
```

В этом примере [`BoxView`](xref:Xamarin.Forms.BoxView) задает для своего свойства `WidthRequest` фиксированный размер, а свойство `HeightRequest` привязывается к свойству `WidthRequest`. Таким образом, оба свойства равны, поэтому рисуется квадрат:

[![Снимок экрана: режим относительной привязки к самому себе в iOS и Android](relative-bindings-images/self-relative-binding.png "Режим относительной привязки к самому себе")](relative-bindings-images/self-relative-binding-large.png#lightbox "Режим относительной привязки к самому себе")

> [!IMPORTANT]
> При привязке свойства элемента к другому свойству того же элемента эти свойства должны иметь одинаковый тип. Кроме того, в привязке можно указать преобразователь для преобразования значения.

Обычно этот режим привязки используется для того, чтобы задать для `BindingContext` объекта его же свойство. В приведенном ниже коде показан соответствующий пример:

```xaml
<ContentPage ...
             BindingContext="{Binding Source={RelativeSource Self}, Path=DefaultViewModel}">
    <StackLayout>
        <ListView ItemsSource="{Binding Employees}">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

В этом примере для `BindingContext` страницы задается его собственное свойство `DefaultViewModel`. Это свойство определено в файле кода программной части для страницы и предоставляет экземпляр модели представления. [`ListView`](xref:Xamarin.Forms.ListView) привязывается к свойству `Employees` модели представления.

## <a name="bind-to-an-ancestor"></a>Привязка к предку

Режимы относительной привязки `FindAncestor` и `FindAncestorBindingContext` используются для привязки к родительским элементам определенного типа в визуальном дереве. Режим `FindAncestor` используется для привязки к родительскому элементу, производному от типа [`Element`](xref:Xamarin.Forms.Element). Режим `FindAncestorBindingContext` используется для привязки к `BindingContext` родительского элемента.

> [!WARNING]
> При использовании режимов относительной привязки `FindAncestor` и `FindAncestorBindingContext` необходимо задать `Type` для свойства `AncestorType`, в противном случае возникает исключение `XamlParseException`.

Если свойство `Mode` не задано явным образом, задание для свойства `AncestorType` типа, производного от [`Element`](xref:Xamarin.Forms.Element), неявно назначит свойству `Mode` значение `FindAncestor`. Аналогичным образом, задание для свойства `AncestorType` типа, не являющегося производным от `Element`, неявно назначит свойству `Mode` значение `FindAncestorBindingContext`.

> [!NOTE]
> Относительные привязки, использующие режим `FindAncestorBindingContext`, будут повторно применены при изменении `BindingContext` любых предков.

В следующем коде XAML показан пример, где для свойства `Mode` будет неявно задано значение `FindAncestorBindingContext`:

```xaml
<ContentPage ...
             BindingContext="{Binding Source={RelativeSource Self}, Path=DefaultViewModel}">
    <StackLayout>
        <ListView ItemsSource="{Binding Employees}">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <Label Text="{Binding Fullname}"
                                   VerticalOptions="Center" />
                            <Button Text="Delete"
                                    Command="{Binding Source={RelativeSource AncestorType={x:Type local:PeopleViewModel}}, Path=DeleteEmployeeCommand}"
                                    CommandParameter="{Binding}"
                                    HorizontalOptions="EndAndExpand" />
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

В этом примере для `BindingContext` страницы задается его собственное свойство `DefaultViewModel`. Это свойство определено в файле кода программной части для страницы и предоставляет экземпляр модели представления. [`ListView`](xref:Xamarin.Forms.ListView) привязывается к свойству `Employees` модели представления. [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), определяющий внешний вид каждого элемента в `ListView`, содержит [`Button`](xref:Xamarin.Forms.Button). Свойство `Command` кнопки привязано к `DeleteEmployeeCommand` в модели представления его родительского элемента. При нажатии `Button` удаляется сотрудник:

[![Снимок экрана: режим относительной привязки FindAncestor в iOS и Android](relative-bindings-images/findancestor-relative-binding.png "Режим относительной привязки FindAncestor")](relative-bindings-images/findancestor-relative-binding-large.png#lightbox "Режим относительной привязки FindAncestor")

Кроме того, необязательное свойство `AncestorLevel` может помочь устранить неоднозначность поиска предка в случаях, где в визуальном дереве может существовать более одного предка данного типа:

```xaml
<Label Text="{Binding Source={RelativeSource AncestorType={x:Type Entry}, AncestorLevel=2}, Path=Text}" />
```

В этом примере свойство `Label.Text` привязывается к свойству `Text` второго [`Entry`](xref:Xamarin.Forms.Entry), обнаруженному на пути вверх, начиная с целевого объекта привязки.

> [!NOTE]
> Свойству `AncestorLevel` должно быть назначено значение 1, чтобы найти предка, ближайшего к целевому объекту привязки.

## <a name="bind-to-a-templated-parent"></a>Привязка к шаблонному родительскому элементу

Режим относительной привязки `TemplatedParent` используется для привязки из шаблона элемента управления к экземпляру объекта среды выполнения, к которому применяется этот шаблон (также называется шаблонным родительским элементом). Этот режим применим только в том случае, если относительная привязка находится внутри шаблона элемента управления и аналогична заданию `TemplateBinding`.

Ниже представлен пример XAML для относительной привязки `TemplatedParent`:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ControlTemplate x:Key="CardViewControlTemplate">
            <Frame BindingContext="{Binding Source={RelativeSource TemplatedParent}}"
                   BackgroundColor="{Binding CardColor}"
                   BorderColor="{Binding BorderColor}"
                   ...>
                <Grid>
                    ...
                    <Label Text="{Binding CardTitle}"
                           ... />
                    <BoxView BackgroundColor="{Binding BorderColor}"
                             ... />
                    <Label Text="{Binding CardDescription}"
                           ... />
                </Grid>
            </Frame>
        </ControlTemplate>
    </ContentPage.Resources>
    <StackLayout>        
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="John Doe"
                           CardDescription="Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla elit dolor, convallis non interdum."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"
                           ControlTemplate="{StaticResource CardViewControlTemplate}" />
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="Jane Doe"
                           CardDescription="Phasellus eu convallis mi. In tempus augue eu dignissim fermentum. Morbi ut lacus vitae eros lacinia."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"
                           ControlTemplate="{StaticResource CardViewControlTemplate}" />
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="Xamarin Monkey"
                           CardDescription="Aliquam sagittis, odio lacinia fermentum dictum, mi erat scelerisque erat, quis aliquet arcu."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"
                           ControlTemplate="{StaticResource CardViewControlTemplate}" />
    </StackLayout>
</ContentPage>
```

В этом примере [`Frame`](xref:Xamarin.Forms.Frame), являющийся корневым элементом `ControlTemplate`, имеет `BindingContext`, для которого установлен экземпляр объекта среды выполнения, к которому применяется шаблон. Таким образом, `Frame` и его дочерние элементы разрешают свои выражения привязки для свойств каждого объекта `CardView`:

[![Снимок экрана: режим относительной привязки TemplatedParent в iOS и Android](relative-bindings-images/templatedparent-relative-binding.png "Режим относительной привязки TemplatedParent")](relative-bindings-images/templatedparent-relative-binding-large.png#lightbox "Режим относительной привязки TemplatedParent")

Дополнительные сведения о шаблонах элементов управления см. в разделе [Шаблоны элементов управления Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/control-template.md).

## <a name="related-links"></a>Связанные ссылки

- [Демоверсии привязок данных (пример)](/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Расширения разметки XAML](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Шаблоны элементов управления Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/control-template.md)