---
title: Xamarin.FormsРежима
description: Это средство Xamarin.Forms позволяет пользователю выбрать числовое значение из диапазона значений. Он состоит из двух кнопок с знаками "минус" и "плюс". При манипуляции с двумя кнопками выбранное значение изменяется постепенно.
ms.prod: xamarin
ms.assetid: 62571B3E-D84B-4F52-9FC7-C105D6733B16
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/17/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4f071530fb17de44d8ede786ca1b42f5e11f4f7c
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84130550"
---
# <a name="xamarinforms-stepper"></a>Xamarin.FormsРежима

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos)

_Для выбора числового значения из диапазона значений используйте средство организации пошагового режима._

Xamarin.Forms [`Stepper`](xref:Xamarin.Forms.Stepper) Состоит из двух кнопок с знаками «минус» и «плюс». Пользователь может управлять этими кнопками для последовательного выбора `double` значения из диапазона значений.

[`Stepper`](xref:Xamarin.Forms.Stepper)Определяет четыре свойства типа `double` :

- [`Increment`](xref:Xamarin.Forms.Stepper.Increment)величина, на которую нужно изменить выбранное значение по умолчанию, равное 1.
- [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum)является минимумом диапазона и значением по умолчанию 0.
- [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum)Максимальное значение диапазона со значением по умолчанию 100.
- [`Value`](xref:Xamarin.Forms.Stepper.Value)— Это значение средства Организации, которое может находиться в диапазоне от `Minimum` до до `Maximum` и имеет значение по умолчанию 0.

Все эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. [`Value`](xref:Xamarin.Forms.Stepper.Value)Свойство имеет режим привязки по умолчанию [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) , что означает, что он подходит как источник привязки в приложении, использующем архитектуру [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) .

> [!WARNING]
> На внутреннем уровне [`Stepper`](xref:Xamarin.Forms.Stepper) обеспечивает [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) меньшее значение [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) . Если `Minimum` или `Maximum` когда-либо задаются так, что `Minimum` не меньше `Maximum` , возникает исключение. Дополнительные сведения о задании `Minimum` свойств и `Maximum` см. в разделе [меры предосторожности](#precautions) .

Объект [`Stepper`](xref:Xamarin.Forms.Stepper) приводит [`Value`](xref:Xamarin.Forms.Stepper.Value) свойство таким образом, чтобы оно было между [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) и [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) включительно. Если `Minimum` свойству присвоено значение, большее, чем `Value` свойство, `Stepper` `Value` свойство устанавливает для значения `Minimum` . Аналогично, если для параметра задано `Maximum` значение меньше `Value` , то `Stepper` для свойства задается `Value` `Maximum` .

[`Stepper`](xref:Xamarin.Forms.Stepper)Определяет [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) событие, возникающее при изменении с [`Value`](xref:Xamarin.Forms.Stepper.Value) помощью пользовательской манипуляции `Stepper` или, когда приложение задает `Value` свойство напрямую. `ValueChanged`Событие также срабатывает при `Value` приведении свойства, как описано в предыдущем абзаце.

[`ValueChangedEventArgs`](xref:Xamarin.Forms.ValueChangedEventArgs)Объект, сопровождающий [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) событие, имеет два свойства типа `double` : [`OldValue`](xref:Xamarin.Forms.ValueChangedEventArgs.OldValue) и [`NewValue`](xref:Xamarin.Forms.ValueChangedEventArgs.NewValue) . Во время срабатывания события значение совпадает `NewValue` со [`Value`](xref:Xamarin.Forms.Stepper.Value) свойством [`Stepper`](xref:Xamarin.Forms.Stepper) объекта.

## <a name="basic-stepper-code-and-markup"></a>Базовый код и разметка для организации пошагового разметки

Пример [**степпердемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) содержит три страницы, которые функционально идентичны, но реализуются различными способами. На первой странице используется только код C#, второй использует XAML с обработчиком событий в коде, а третья позволяет избежать обработчика событий с помощью привязки данных в файле XAML.

### <a name="creating-a-stepper-in-code"></a>Создание средства организации пошагового режима в коде

На странице " **базовый код для многошагового** руководства" в примере [**степпердемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) показано, как создать [`Stepper`](xref:Xamarin.Forms.Stepper) и два [`Label`](xref:Xamarin.Forms.Label) объекта в коде:

```csharp
public class BasicStepperCodePage : ContentPage
{
    public BasicStepperCodePage()
    {
        Label rotationLabel = new Label
        {
            Text = "ROTATING TEXT",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Label displayLabel = new Label
        {
            Text = "(uninitialized)",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Stepper stepper = new Stepper
        {
            Maximum = 360,
            Increment = 30,
            HorizontalOptions = LayoutOptions.Center
        };
        stepper.ValueChanged += (sender, e) =>
        {
            rotationLabel.Rotation = stepper.Value;
            displayLabel.Text = string.Format("The Stepper value is {0}", e.NewValue);
        };

        Title = "Basic Stepper Code";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = { rotationLabel, stepper, displayLabel }
        };
    }
}
```

[`Stepper`](xref:Xamarin.Forms.Stepper)Инициализируется [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) свойством, равным 360, и [`Increment`](xref:Xamarin.Forms.Stepper.Increment) свойством, равным 30. `Stepper`При изменении выбранного значения выбранное значение увеличивается в [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) зависимости от `Maximum` значения `Increment` Свойства. [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged)Обработчик `Stepper` использует [`Value`](xref:Xamarin.Forms.Stepper.Value) свойство `stepper` объекта, чтобы установить [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) свойство первого элемента [`Label`](xref:Xamarin.Forms.Label) , и использует `string.Format` метод со `NewValue` свойством аргументов события, чтобы установить [`Text`](xref:Xamarin.Forms.Label.Text) свойство второго `Label` . Эти два подхода к получению текущего значения `Stepper` являются взаимозаменяемыми.

На следующих снимках экрана показана **Базовая кодовая** страница для многоадресной Организации:

[![Базовый код средства Организации](stepper-images/basic-stepper-code.png "Базовый код средства Организации")](stepper-images/basic-stepper-code-large.png#lightbox)

Во втором [`Label`](xref:Xamarin.Forms.Label) выводится текст "(не инициализировано)" до тех пор [`Stepper`](xref:Xamarin.Forms.Stepper) , пока не будет выполнен манипуляций, что приводит к [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) срабатыванию первого события.

### <a name="creating-a-stepper-in-xaml"></a>Создание средства организации пошагового режима в XAML

**Базовая страница XAML для многошаговой** работы функционально аналогична **базовому коду многошагового** выполнения, но реализована в основном в XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StepperDemo.BasicStepperXAMLPage"
             Title="Basic Stepper XAML">
    <StackLayout Margin="20">
        <Label x:Name="_rotatingLabel"
               Text="ROTATING TEXT"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
        <Stepper Maximum="360"
                 Increment="30"
                 HorizontalOptions="Center"
                 ValueChanged="OnStepperValueChanged" />
        <Label x:Name="_displayLabel"
               Text="(uninitialized)"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />        
    </StackLayout>
</ContentPage>
```

Файл кода программной части содержит обработчик [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) события:

```csharp
public partial class BasicStepperXAMLPage : ContentPage
{
    public BasicStepperXAMLPage()
    {
        InitializeComponent();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs e)
    {
        double value = e.NewValue;
        _rotatingLabel.Rotation = value;
        _displayLabel.Text = string.Format("The Stepper value is {0}", value);
    }
}
```

Кроме того, обработчик событий может получить объект [`Stepper`](xref:Xamarin.Forms.Stepper) , который заактивирует событие с помощью `sender` аргумента. [`Value`](xref:Xamarin.Forms.Stepper.Value)Свойство содержит текущее значение:

```csharp
double value = ((Stepper)sender).Value;
```

Если [`Stepper`](xref:Xamarin.Forms.Stepper) объекту было присвоено имя в XAML-файле с `x:Name` атрибутом (например, "средство организации"), обработчик событий может ссылаться на этот объект напрямую:

```csharp
double value = stepper.Value;
```

### <a name="data-binding-the-stepper"></a>Связывание данных с помощью средства Организации

На странице " **базовые привязки для многошагового** руководства" показано, как написать почти эквивалентное приложение, которое устраняет [`Value`](xref:Xamarin.Forms.Stepper.Value) обработчик событий с помощью [привязки данных](~/xamarin-forms/app-fundamentals/data-binding/index.md):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StepperDemo.BasicStepperBindingsPage"
             Title="Basic Stepper Bindings">
    <StackLayout Margin="20">
        <Label Text="ROTATING TEXT"
               Rotation="{Binding Source={x:Reference _stepper}, Path=Value}"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
        <Stepper x:Name="_stepper"
                 Maximum="360"
                 Increment="30"
                 HorizontalOptions="Center" />
        <Label Text="{Binding Source={x:Reference _stepper}, Path=Value, StringFormat='The Stepper value is {0:F0}'}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

[`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)Свойство первого объекта [`Label`](xref:Xamarin.Forms.Label) привязано к [`Value`](xref:Xamarin.Forms.Stepper.Value) свойству [`Stepper`](xref:Xamarin.Forms.Stepper) , а — к свойству [`Text`](xref:Xamarin.Forms.Label.Text) секунды `Label` со `StringFormat` спецификацией. **Основная страница привязок** с учетом основных действий немного отличается от двух предыдущих страниц: при первом отображении страницы вторая `Label` отображает текстовую строку со значением. Это преимущество использования привязки данных. Чтобы отобразить текст без привязки данных, необходимо специально инициализировать `Text` свойство объекта `Label` или имитировать срабатывание [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) события, вызвав обработчик события из конструктора класса.

## <a name="precautions"></a>Меры предосторожности

Значение [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) свойства должно всегда быть меньше значения [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) Свойства. В следующем фрагменте кода вызывается [`Stepper`](xref:Xamarin.Forms.Stepper) исключение:

```csharp
// Throws an exception!
Stepper stepper = new Stepper
{
    Minimum = 180,
    Maximum = 360
};
```

Компилятор C# создает код, который устанавливает эти два свойства последовательно, а если [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) свойство имеет значение 180, оно больше, чем значение по умолчанию [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) 100. Исключение в этом случае можно избежать, задав `Maximum` свойство в первую очередь:

```csharp
Stepper stepper = new Stepper
{
    Maximum = 360,
    Minimum = 180
};
```

Значение [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) 360 не является проблемой, так как оно больше значения по умолчанию [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) 0. Если `Minimum` задано, значение меньше `Maximum` значения 360.

Та же проблема существует в XAML. Задайте свойства в порядке, который гарантирует, что [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) всегда будет больше `Minimum` :

```xaml
<Stepper Maximum="360"
         Minimum="180" ... />
```

Можно задать [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) значения и для отрицательных чисел, но только в том порядке, где `Minimum` всегда меньше `Maximum` :

```xaml
<Stepper Minimum="-360"
         Maximum="-180" ... />
```

[`Value`](xref:Xamarin.Forms.Stepper.Value)Свойство всегда больше или равно [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) значению и меньше или равно [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) . Если для параметра задано `Value` значение за пределами этого диапазона, значение будет преобразовано в диапазон, но исключение не будет создано. Например, этот код *не* вызовет исключение:

```csharp
Stepper stepper = new Stepper
{
    Value = 180
};
```

Вместо этого [`Value`](xref:Xamarin.Forms.Stepper.Value) свойство приводится к [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) значению 100.

Ниже приведен фрагмент кода, показанный выше.

```csharp
Stepper stepper = new Stepper
{
    Maximum = 360,
    Minimum = 180
};
```

Если [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) для задано значение 180, то [`Value`](xref:Xamarin.Forms.Stepper.Value) также устанавливается значение 180.

Если [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) обработчик событий был присоединен к моменту, когда [`Value`](xref:Xamarin.Forms.Stepper.Value) свойство приводится к значению, отличному от значения по умолчанию 0, то `ValueChanged` возникает событие. Вот фрагмент кода XAML:

```xaml
<Stepper ValueChanged="OnStepperValueChanged"
         Maximum="360"
         Minimum="180" />
```

Если [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) для задано значение 180, [`Value`](xref:Xamarin.Forms.Stepper.Value) то также присваивается значение 180, и [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) возникает событие. Это может произойти до того, как будет создана оставшаяся часть страницы, и обработчик может попытаться сослаться на другие элементы на странице, которые еще не были созданы. Может потребоваться добавить в обработчик некоторый код `ValueChanged` , который проверяет `null` значения других элементов на странице. Или можно задать `ValueChanged` обработчик событий после [`Stepper`](xref:Xamarin.Forms.Stepper) инициализации значений.

## <a name="related-links"></a>Связанные ссылки

- [Пример с демонстрацией пошагового использования](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos)
- [API организации пошагового режима](xref:Xamarin.Forms.Stepper)
