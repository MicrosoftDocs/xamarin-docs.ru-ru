---
title: Множественные привязки Xamarin.Forms
description: В этой статье объясняется, как присоединить коллекцию объектов привязки к одному свойству целевого объекта привязки с помощью класса с MultiBinding.
ms.prod: xamarin
ms.assetid: E73AE622-664C-4A90-B5B2-BD47D0E7A1A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2020
ms.openlocfilehash: 0aafe01fcbde6cf1aacf3e2dd47444d4b77021e2
ms.sourcegitcommit: 79ba3deb031c8a60d0841bb3dbeaaf65daf2b224
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "85846380"
---
# <a name="xamarinforms-multi-bindings"></a>Множественные привязки Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

Множественные привязки позволяют присоединять коллекцию объектов [`Binding`](xref:Xamarin.Forms.Binding) к одному свойству целевого объекта привязки. Они создаются с помощью класса `MultiBinding`, который вычисляет все объекты `Binding` и возвращает одно значение через экземпляр `IMultiValueConverter`, предоставляемый приложением. Кроме того, `MultiBinding` повторно вычисляет все объекты `Binding` при изменении каких-либо привязанных данных.

Класс `MultiBinding` определяет следующие свойства:

- `Bindings` типа `IList<BindingBase>` — представление коллекции объектов [`Binding`](xref:Xamarin.Forms.Binding) в экземпляре `MultiBinding`.
- `Converter` типа `IMultiValueConverter` — представление преобразователя исходных значений в целевое значение или из целевого значения.
- `ConverterParameter` типа `object` — представление необязательного параметра для передачи в `Converter`.

`Bindings` — это свойство содержимого класса `MultiBinding`. Поэтому его не нужно явно задавать из XAML.

Кроме того, класс `MultiBinding` наследует следующие свойства от класса `BindingBase`:

- `FallbackValue` типа `object` — представление значения, используемого, когда множественная привязка не может вернуть значение.
- `Mode` типа [`BindingMode`](xref:Xamarin.Forms.BindingMode) — указатель направления потока данных множественной привязки.
- `StringFormat` типа `string` — определение способа форматирования результата множественной привязки, если он отображается в виде строки.
- `TargetNullValue` типа `object` — представление значения, используемого в целевом объекте, когда значение источника равно `null`.

Класс `MultiBinding` должен использовать `IMultiValueConverter`, чтобы создать значение для целевого объекта привязки на основе значений привязок в коллекции `Bindings`. Например, [`Color`](xref:Xamarin.Forms.Color) можно вычислить на основе значений красного, синего и зеленого цветов (все они могут быть значениями одних и тех же или разных объектов источника привязки). Когда значение перемещается из целевого объекта в источники, значение целевого свойства преобразуется в набор значений, которые передаются обратно в привязки.

> [!IMPORTANT]
> Отдельные привязки в коллекции `Bindings` могут иметь собственные преобразователи величин.

Значение свойства `Mode` определяет возможности `MultiBinding` и используется в качестве режима привязки для всех привязок в коллекции, если только отдельная привязка не переопределяет свойство. Например, если для свойства `Mode` объекта `MultiBinding` задано значение [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay), все привязки в коллекции считаются `TwoWay`, если только для одной из привязок явно не задано другое значение `Mode`.

## <a name="define-a-imultivalueconverter"></a>Определение IMultiValueConverter

Интерфейс `IMultiValueConverter` позволяет применять пользовательскую логику к `MultiBinding`. Чтобы связать преобразователь с `MultiBinding`, создайте класс, реализующий интерфейс `IMultiValueConverter`, а затем реализуйте методы `Convert` и `ConvertBack`.

```csharp
public class AllTrueMultiConverter : IMultiValueConverter
{
    public object Convert(object[] values, Type targetType, object parameter, CultureInfo culture)
    {
        if (values == null || !targetType.IsAssignableFrom(typeof(bool)))
        {
            return false;
            // Alternatively, return BindableProperty.UnsetValue to use the binding FallbackValue
        }

        foreach (var value in values)
        {
            if (!(value is bool b))
            {
                return false;
                // Alternatively, return BindableProperty.UnsetValue to use the binding FallbackValue
            }
            else if (!b)
            {
                return false;
            }
        }
        return true;
    }

    public object[] ConvertBack(object value, Type[] targetTypes, object parameter, CultureInfo culture)
    {
        if (!(value is bool b) || targetTypes.Any(t => !t.IsAssignableFrom(typeof(bool))))
        {
            // Return null to indicate conversion back is not possible
            return null;
        }

        if (b)
        {
            return targetTypes.Select(t => (object)true).ToArray();
        }
        else
        {
            // Can't convert back from false because of ambiguity
            return null;
        }
    }
}
```

Метод `Convert` преобразует исходные значения в значение для целевого объекта привязки. Xamarin.Forms вызывает этот метод при распространении значений от исходных привязок в целевой объект привязки. Этот метод принимает четыре аргумента:

- `values` типа `object[]` — массив значений, создаваемый исходными привязками в `MultiBinding`.
- `targetType` типа `Type` — тип целевого свойства привязки.
- `parameter` типа `object` — используемый параметр преобразователя.
- `culture` типа `CultureInfo` — язык и региональные параметры, используемые в преобразователе.

Метод `Convert` возвращает `object` — представление преобразованного значения. Этот метод должен возвращать следующее:

- `BindableProperty.UnsetValue` — указывает, что преобразователь не создал значение и что привязка будет использовать `FallbackValue`.
- `Binding.DoNothing` — указывает Xamarin.Forms не выполнять никаких действий, например не передавать значение в целевой объект привязки или не использовать `FallbackValue`.
- `null` — указывает, что преобразователь не может выполнить преобразование и что привязка будет использовать `TargetNullValue`.

> [!IMPORTANT]
> Объект `MultiBinding`, принимающий `BindableProperty.UnsetValue` из метода `Convert`, должен определять его свойство [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue). Точно так же объект `MultiBinding`, принимающий `null` из метода `Convert`, должен определять его свойство [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue).

Метод `ConvertBack` преобразует целевой объект привязки в значения привязки к источнику. Этот метод принимает четыре аргумента:

- `value` типа `object` — значение, созданное целевым объектом привязки.
- `targetTypes` типа `Type[]` — массив типов для преобразования. Длина массива указывает количество и типы значений, которые, как предполагается, метод будет возвращать.
- `parameter` типа `object` — используемый параметр преобразователя.
- `culture` типа `CultureInfo` — язык и региональные параметры, используемые в преобразователе.

Метод `ConvertBack` возвращает массив значений типа `object[]`, преобразованных из целевых в исходные. Этот метод должен возвращать следующее:

- `BindableProperty.UnsetValue` в позиции `i` — указывает, что преобразователь не может предоставить значение для исходной привязки по индексу `i` и что для него не задано значение.
- `Binding.DoNothing` в позиции `i` — указывает, что для исходной привязки не должно быть задано значение в индексе `i`.
- `null` — указывает, что преобразователь не может выполнить преобразование или что он не поддерживает преобразование в этом направлении.

## <a name="consume-a-imultivalueconverter"></a>Использование IMultiValueConverter

`IMultiValueConverter` используется путем создания экземпляра в словаре ресурсов. Затем на него добавляется ссылка с помощью расширения разметки `StaticResource` для установки свойства `MultiBinding.Converter`.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.MultiBindingConverterPage"
             Title="MultiBinding Converter demo">

    <ContentPage.Resources>
        <local:AllTrueMultiConverter x:Key="AllTrueConverter" />
        <local:InverterConverter x:Key="InverterConverter" />
    </ContentPage.Resources>

    <CheckBox>
        <CheckBox.IsChecked>
            <MultiBinding Converter="{StaticResource AllTrueConverter}">
                <Binding Path="Employee.IsOver16" />
                <Binding Path="Employee.HasPassedTest" />
                <Binding Path="Employee.IsSuspended"
                         Converter="{StaticResource InverterConverter}" />
            </MultiBinding>
        </CheckBox.IsChecked>
    </CheckBox>
</ContentPage>    
```

В этом примере объект `MultiBinding` использует экземпляр `AllTrueMultiConverter`, чтобы задать для свойства [`CheckBox.IsChecked`](xref:Xamarin.Forms.CheckBox.IsChecked) значение `true`, если трем объектам [`Binding`](xref:Xamarin.Forms.Binding) присваивается значение `true`. В противном случае свойству `CheckBox.IsChecked` присваивается значение `false`.

По умолчанию свойство [`CheckBox.IsChecked`](xref:Xamarin.Forms.CheckBox.IsChecked) использует привязку [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay). Таким образом, метод `ConvertBack` экземпляра `AllTrueMultiConverter` выполняется, когда пользователь не использует [`CheckBox`](xref:Xamarin.Forms.CheckBox), задавая для значений привязки к источнику значение свойства `CheckBox.IsChecked`.

## <a name="format-strings"></a>Строки формата

`MultiBinding` может форматировать результат множественной привязки, отображаемый в виде строки, со свойством `StringFormat`. Для этого свойства можно задать стандартную строку форматирования .NET с заполнителями, которая определяет способ форматирования результата множественной привязки.

```xaml
<Label>
    <Label.Text>
        <MultiBinding StringFormat="{}{0} {1} {2}">
            <Binding Path="Employee1.Forename" />
            <Binding Path="Employee1.MiddleName" />
            <Binding Path="Employee1.Surname" />
        </MultiBinding>
    </Label.Text>
</Label>
```

В этом примере свойство `StringFormat` объединяет три привязанных значения в одну строку, отображаемую [`Label`](xref:Xamarin.Forms.Label).

> [!IMPORTANT]
> Число параметров в формате составной строки не должно превышать число дочерних `Binding` объектов в `MultiBinding`.

При установке свойств `Converter` и `StringFormat` сначала к значению данных применяется преобразователь, а затем применяется `StringFormat`.

См. сведения в статье [Форматирование строк Xamarin.Forms](string-formatting.md).

## <a name="provide-fallback-values"></a>Предоставление резервных значений

Повысить надежность привязок данных можно, определив резервные значения, которые будут использоваться в случае сбоя привязки. Для этого при необходимости можно определить свойства [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) и [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) в объекте `MultiBinding`.

`MultiBinding` будет использовать [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue), когда метод `Convert` экземпляра `IMultiValueConverter` возвращает `BindableProperty.UnsetValue`. Это означает, что преобразователь не создал значение. `MultiBinding` будет использовать [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue), когда метод `Convert` экземпляра `IMultiValueConverter` возвращает `null`. Это означает, что преобразователь не может выполнить преобразование.

См. сведения в статье [Резервные значения привязок Xamarin.Forms](binding-fallbacks.md).

## <a name="nest-multibinding-objects"></a>Вложение объектов Nest MultiBinding

`MultiBinding` объекты могут быть вложенными, чтобы при вычислении нескольких объектов `MultiBinding` значение возвращалось через экземпляр `IMultiValueConverter`.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.NestedMultiBindingPage"
             Title="Nested MultiBinding demo">

    <ContentPage.Resources>
        <local:AllTrueMultiConverter x:Key="AllTrueConverter" />
        <local:AnyTrueMultiConverter x:Key="AnyTrueConverter" />
        <local:InverterConverter x:Key="InverterConverter" />
    </ContentPage.Resources>

    <CheckBox>
        <CheckBox.IsChecked>
            <MultiBinding Converter="{StaticResource AnyTrueConverter}">
                <MultiBinding Converter="{StaticResource AllTrueConverter}">
                    <Binding Path="Employee.IsOver16" />
                    <Binding Path="Employee.HasPassedTest" />
                    <Binding Path="Employee.IsSuspended" Converter="{StaticResource InverterConverter}" />                        
                </MultiBinding>
                <Binding Path="Employee.IsMonarch" />
            </MultiBinding>
        </CheckBox.IsChecked>
    </CheckBox>
</ContentPage>
```

В этом примере объект `MultiBinding` использует свой экземпляр `AnyTrueMultiConverter`, чтобы задать для свойства [`CheckBox.IsChecked`](xref:Xamarin.Forms.CheckBox.IsChecked) значение `true`, если всем объектам [`Binding`](xref:Xamarin.Forms.Binding) во внутреннем объекте `MultiBinding` присваивается значение `true` или если объекту `Binding` во внешнем объекте `MultiBinding` присваивается значение `true`. В противном случае свойству `CheckBox.IsChecked` присваивается значение `false`.

## <a name="use-a-relativesource-binding-in-a-multibinding"></a>Использование привязки RelativeSource в MultiBinding

Объект `MultiBinding` поддерживает относительные привязки, которые позволяют задать источник привязки относительно положения целевого объекта привязки.

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:DataBindingDemos">
    <ContentPage.Resources>
        <local:AllTrueMultiConverter x:Key="AllTrueConverter" />

        <ControlTemplate x:Key="CardViewExpanderControlTemplate">
            <Expander BindingContext="{Binding Source={RelativeSource TemplatedParent}}"
                      IsExpanded="{Binding IsExpanded, Source={RelativeSource TemplatedParent}}"
                      BackgroundColor="{Binding CardColor}">
                <Expander.IsVisible>
                    <MultiBinding Converter="{StaticResource AllTrueConverter}">
                        <Binding Path="IsExpanded" />
                        <Binding Path="IsEnabled" />
                    </MultiBinding>
                </Expander.IsVisible>
                <Expander.Header>
                    <Grid>
                        <!-- XAML that defines Expander header goes here -->
                    </Grid>
                </Expander.Header>
                <Grid>
                    <!-- XAML that defines Expander content goes here -->
                </Grid>
            </Expander>
        </ControlTemplate>
    </ContentPage.Resources>

    <StackLayout>
        <controls:CardViewExpander BorderColor="DarkGray"
                                   CardTitle="John Doe"
                                   CardDescription="Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla elit dolor, convallis non interdum."
                                   IconBackgroundColor="SlateGray"
                                   IconImageSource="user.png"
                                   ControlTemplate="{StaticResource CardViewExpanderControlTemplate}"
                                   IsEnabled="True"
                                   IsExpanded="True" />
    </StackLayout>
</ContentPage>
```

В этом примере режим относительной привязки `TemplatedParent` используется для привязки из шаблона элемента управления к экземпляру объекта среды выполнения, к которому применяется шаблон. `Expander` — это корневой элемент [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate), у которого есть `BindingContext` с заданным экземпляром объекта среды выполнения, к которому применяется шаблон. Следовательно, `Expander` с дочерними элементами разрешают свои выражения привязки и объекты [`Binding`](xref:Xamarin.Forms.Binding) для свойств объекта `CardViewExpander`. `MultiBinding` использует экземпляр `AllTrueMultiConverter`, чтобы задать для свойства `Expander.IsVisible` значение `true`, если двум объектам [`Binding`](xref:Xamarin.Forms.Binding) присваивается значение `true`. В противном случае свойству `Expander.IsVisible` присваивается значение `false`.

Дополнительные сведения об относительных привязках Xamarin.Forms см. в [этой статье](relative-bindings.md). Дополнительные сведения о шаблонах элементов управления см. в разделе [Шаблоны элементов управления Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/control-template.md).

## <a name="related-links"></a>Связанные ссылки

- [Демоверсии привязок данных (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Форматирование строк Xamarin.Forms](string-formatting.md)
- [Резервные значения привязок в Xamarin.Forms](binding-fallbacks.md)
- [Относительные привязки Xamarin.Forms](relative-bindings.md)
- [Шаблоны элементов управления Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/control-template.md)
