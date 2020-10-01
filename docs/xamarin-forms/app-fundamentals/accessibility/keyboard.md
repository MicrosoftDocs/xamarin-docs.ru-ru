---
title: Специальные возможности клавиатуры
description: Чтобы не использовать последовательность табуляции по умолчанию, нужно настроить специальные возможности пользовательского интерфейса, указав эту последовательность с помощью свойств TabIndex и IsTabStop.
ms.prod: xamarin
ms.assetid: 8be8f498-558a-4894-a01f-91a0d3ef927e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: db0591f89447a2ae083f5ac73f6e002b4d0ee6f8
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91561356"
---
# <a name="keyboard-accessibility-in-no-locxamarinforms"></a>Специальные возможности клавиатуры в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-accessibility)

Пользователям, которые работают со средствами чтения с экрана или испытывают проблемы с передвижением, может быть трудно работать с приложениями, которые не обеспечивают должный уровень доступа с клавиатуры. В приложениях Xamarin.Forms можно определить ожидаемую последовательность табуляции, чтобы сделать приложение более удобным и доступным. Определение последовательности табуляции для элементов управления позволяет выполнять навигацию с помощью клавиатуры, подготавливает страницы приложений к вводу данных в определенном порядке и разрешает средствам чтения с экрана читать имена фокусируемых элементов для пользователя.

По умолчанию последовательность табуляции для элементов управления соответствует порядку, в котором они перечислены в XAML или программно добавлены в дочернюю коллекцию. Этот порядок соответствует порядку навигации по элементам управления с клавиатуры и считывания данных средством чтения с экрана, и часто такой порядок по умолчанию является оптимальным. Однако порядок по умолчанию не всегда соответствует ожидаемому, как показано в следующем примере кода XAML:

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="0.5*" />
        <ColumnDefinition Width="0.5*" />
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Label Text="You"
           HorizontalOptions="Center" />
    <Label Grid.Column="1"
           Text="Manager"
           HorizontalOptions="Center" />
    <Entry Grid.Row="1"
           Placeholder="Enter forename" />
    <Entry Grid.Column="1"
           Grid.Row="1"
           Placeholder="Enter forename" />
    <Entry Grid.Row="2"
           Placeholder="Enter surname" />
    <Entry Grid.Column="1"
           Grid.Row="2"
           Placeholder="Enter surname" />
</Grid>
```

На следующем снимке экрана показана последовательность табуляции по умолчанию для этого примера кода:

![Последовательность табуляции по умолчанию на основе строк](keyboard-images/default-tab-order.png)

Здесь последовательность табуляции основана на строках и соответствует порядку элементов управления в XAML. Таким образом, при нажатии клавиши TAB выполняется переход по экземплярам имени [`Entry`](xref:Xamarin.Forms.Entry), а затем экземплярам фамилии `Entry`. Однако более интуитивно понятным было бы использование перехода по столбцам, чтобы при нажатии клавиши TAB выполнялся переход по парам имя-фамилия. Этого можно добиться, указав последовательность табуляции для элементов управления вводом.

> [!NOTE]
> В универсальной платформе Windows можно определить сочетания клавиш, предоставляющие пользователям интуитивно понятный способ для быстрого перехода и взаимодействия с видимым пользовательским интерфейсом приложения с клавиатуры, а не с помощью сенсорного интерфейса или мыши. Дополнительные сведения см. в разделе [Настройка клавиш доступа VisualElement](~/xamarin-forms/platform/windows/visualelement-access-keys.md).

## <a name="setting-the-tab-order"></a>Настройка последовательности табуляции

Свойство `VisualElement.TabIndex` используется для указания порядка, в котором экземпляры [`VisualElement`](xref:Xamarin.Forms.VisualElement) получают фокус, когда пользователь переходит между элементами управления с помощью клавиши TAB. По умолчанию свойство равно 0, при этом оно может принимать любое значение `int`.

Следующие правила применяются при использовании последовательности табуляции по умолчанию или задании свойства `TabIndex`:

- Экземпляры [`VisualElement`](xref:Xamarin.Forms.VisualElement) с `TabIndex`, равным 0, добавляются в последовательность табуляции с учетом порядка их объявления в XAML или дочерних коллекциях.
- Экземпляры [`VisualElement`](xref:Xamarin.Forms.VisualElement) с `TabIndex` больше 0 добавляются в последовательность табуляции с учетом их значения `TabIndex`.
- Экземпляры [`VisualElement`](xref:Xamarin.Forms.VisualElement) с `TabIndex` меньше 0 добавляются в последовательность табуляции и отображаются до любого нулевого значения.
- Конфликты для `TabIndex`, устраняемые в порядке объявления.

После определения последовательности табуляции нажатие клавиши TAB будет переключать фокус между элементами управления в порядке по возрастанию `TabIndex` с возвратом в начало после достижения конечного элемента управления.

Следующий пример XAML показывает свойство `TabIndex`, заданное в элементах управления вводом для обеспечения навигации по столбцам:

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="0.5*" />
        <ColumnDefinition Width="0.5*" />
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Label Text="You"
           HorizontalOptions="Center" />
    <Label Grid.Column="1"
           Text="Manager"
           HorizontalOptions="Center" />
    <Entry Grid.Row="1"
           Placeholder="Enter forename"
           TabIndex="1" />
    <Entry Grid.Column="1"
           Grid.Row="1"
           Placeholder="Enter forename"
           TabIndex="3" />
    <Entry Grid.Row="2"
           Placeholder="Enter surname"
           TabIndex="2" />
    <Entry Grid.Column="1"
           Grid.Row="2"
           Placeholder="Enter surname"
           TabIndex="4" />
</Grid>
```

На следующем снимке экрана показана последовательность табуляции для этого примера кода:

![Последовательность табуляции на основе столбцов](keyboard-images/correct-tab-order.png)

Здесь последовательность табуляции основана на столбцах. Таким образом, при нажатии клавиши TAB выполняется переход по парам имя-фамилия [`Entry`](xref:Xamarin.Forms.Entry).

> [!IMPORTANT]
> Средства чтения с экрана в iOS и Android будут учитывать `TabIndex` элемента [`VisualElement`](xref:Xamarin.Forms.VisualElement) при чтении имен доступных элементов на экране.

## <a name="excluding-controls-from-the-tab-order"></a>Исключение элементов управления из последовательности табуляции

Кроме настройки последовательности табуляции для элементов управления, может потребоваться исключить элементы управления из этой последовательности. Один из способов сделать это заключается в задании значения `false` для свойства [`IsEnabled`](xref:Xamarin.Forms.VisualElement) элементов управления, так как отключенные элементы управления исключаются из последовательности табуляции.

Однако может потребоваться исключить неотключенные элементы управления из последовательности табуляции. Это можно сделать с помощью свойства `VisualElement.IsTabStop`, которое указывает, включается ли [`VisualElement`](xref:Xamarin.Forms.VisualElement) в навигацию. Значение по умолчанию — `true`, и если оно равно `false`, элемент управления игнорируется инфраструктурой навигации по клавише TAB независимо от того, задано ли `TabIndex`.

## <a name="supported-controls"></a>Поддерживаемые элементы управления

Свойства `TabIndex` и `IsTabStop` поддерживаются для следующих элементов управления, которые принимает ввод с клавиатуры в одной или нескольких платформах:

- [`Button`](xref:Xamarin.Forms.Button)
- [`DatePicker`](xref:Xamarin.Forms.DatePicker)
- [`Editor`](xref:Xamarin.Forms.Editor)
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)
- [`Picker`](xref:Xamarin.Forms.Picker)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)
- [`SearchBar`](xref:Xamarin.Forms.SearchBar)
- [`Slider`](xref:Xamarin.Forms.Slider)
- [`Stepper`](xref:Xamarin.Forms.Stepper)
- [`Switch`](xref:Xamarin.Forms.Switch)
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)
- [`TimePicker`](xref:Xamarin.Forms.TimePicker)

> [!NOTE]
> Все эти элементы управления способны получать фокус по нажатию клавиши TAB не на всех платформах.

## <a name="related-links"></a>Связанные ссылки

- [Специальные возможности (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-accessibility)