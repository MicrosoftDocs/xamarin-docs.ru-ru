---
title: Xamarin.Forms Словари ресурсов
description: Xamarin.Forms Ресурсы XAML — это объекты, которые могут использоваться совместно и повторно использоваться во всем Xamarin.Forms приложении.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/10/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.custom: video
ms.openlocfilehash: 60d16183e1a2ea162c97bbf8b30636a5a9999204
ms.sourcegitcommit: f2942b518f51317acbb263be5bc0c91e66239f50
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2020
ms.locfileid: "94590273"
---
# <a name="no-locxamarinforms-resource-dictionaries"></a>Xamarin.Forms Словари ресурсов

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)— Это репозиторий для ресурсов, используемых Xamarin.Forms приложением. Типичные ресурсы, которые хранятся в `ResourceDictionary` [стилях](~/xamarin-forms/user-interface/styles/index.md)включения, [шаблонах элементов управления](~/xamarin-forms/app-fundamentals/templates/control-template.md), [шаблонах данных](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), цветах и конвертерах.

В XAML ресурсы, хранящиеся в, [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) можно ссылаться и применять к элементам с помощью `StaticResource` `DynamicResource` расширения разметки или. В C# ресурсы также можно определять в, а `ResourceDictionary` затем ссылаться на них и применять их к элементам с помощью индексатора на основе строк. Однако в C# есть немало преимуществ `ResourceDictionary` , так как общие объекты могут храниться в виде полей или свойств, а доступ к ним осуществляется напрямую без необходимости их извлечения из словаря.

## <a name="create-resources-in-xaml"></a>Создание ресурсов в XAML

Каждый [`VisualElement`](xref:Xamarin.Forms.VisualElement) производный объект имеет [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) свойство, которое [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) может содержать ресурсы. Аналогичным образом, [`Application`](xref:Xamarin.Forms.Application) производный объект имеет [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) свойство, которое [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) может содержать ресурсы.

Xamarin.FormsПриложение содержит только класс, производный от [`Application`](xref:Xamarin.Forms.Application) , но часто использует многие классы, производные от [`VisualElement`](xref:Xamarin.Forms.VisualElement) , включая страницы, макеты и элементы управления. Для любого из этих объектов `Resources` свойству может быть присвоено значение, [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) содержащее ресурсы. Выбор места для размещения определенных `ResourceDictionary` последствий, в которых можно использовать ресурсы:

- Ресурсы в `ResourceDictionary` , которые присоединены к представлению, например [`Button`](xref:Xamarin.Forms.Button) или [`Label`](xref:Xamarin.Forms.Label) , могут применяться только к этому конкретному объекту.
- Ресурсы в `ResourceDictionary` присоединенном к макету, например [`StackLayout`](xref:Xamarin.Forms.StackLayout) или, [`Grid`](xref:Xamarin.Forms.Grid) можно применить к макету и ко всем дочерним элементам этого макета.
- Ресурсы в, `ResourceDictionary` определенные на уровне страницы, могут быть применены к странице и ко всем ее дочерним элементам.
- Ресурсы в, `ResourceDictionary` определенные на уровне приложения, можно применять ко всему приложению.

За исключением неявных стилей, каждый ресурс в словаре ресурсов должен иметь уникальный строковый ключ, определенный с помощью `x:Key` атрибута.

В следующем коде XAML показаны ресурсы, определенные на уровне приложения `ResourceDictionary` в файле **app. XAML** :

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ResourceDictionaryDemo.App">
    <Application.Resources>

        <Thickness x:Key="PageMargin">20</Thickness>

        <!-- Colors -->
        <Color x:Key="AppBackgroundColor">AliceBlue</Color>
        <Color x:Key="NavigationBarColor">#1976D2</Color>
        <Color x:Key="NavigationBarTextColor">White</Color>
        <Color x:Key="NormalTextColor">Black</Color>

        <!-- Implicit styles -->
        <Style TargetType="{x:Type NavigationPage}">
            <Setter Property="BarBackgroundColor"
                    Value="{StaticResource NavigationBarColor}" />
            <Setter Property="BarTextColor"
                    Value="{StaticResource NavigationBarTextColor}" />
        </Style>

        <Style TargetType="{x:Type ContentPage}"
               ApplyToDerivedTypes="True">
            <Setter Property="BackgroundColor"
                    Value="{StaticResource AppBackgroundColor}" />
        </Style>

    </Application.Resources>
</Application>
```

В этом примере словарь ресурсов определяет [`Thickness`](xref:Xamarin.Forms.Thickness) ресурс, несколько [`Color`](xref:Xamarin.Forms.Color) ресурсов и два неявных [`Style`](xref:Xamarin.Forms.Style) ресурса. Дополнительные сведения о `App` классе см. в разделе [ Xamarin.Forms класс App](~/xamarin-forms/app-fundamentals/application-class.md).

> [!NOTE]
> Также допускается размещение всех ресурсов между явными `ResourceDictionary` тегами. Однако, поскольку Xamarin.Forms 3,0 `ResourceDictionary` теги не требуются. Вместо этого `ResourceDictionary` объект создается автоматически, и можно вставить ресурсы непосредственно между `Resources` тегами элемента свойства.

## <a name="consume-resources-in-xaml"></a>Использование ресурсов в XAML

Каждый ресурс имеет ключ, который задается с помощью `x:Key` атрибута, который становится ключом словаря в `ResourceDictionary` . Ключ используется для ссылки на ресурс из [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) с [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) [`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension) расширением разметки или.

`StaticResource`Расширение разметки аналогично `DynamicResource` расширению разметки в том, что для ссылки на значение из словаря ресурсов используется ключ словаря. Однако, хотя `StaticResource` расширение разметки выполняет поиск по одному словарю, `DynamicResource` расширение разметки сохраняет ссылку на ключ словаря. Таким образом, если запись словаря, связанная с ключом, заменена, это изменение применяется к визуальному элементу. Это позволяет вносить изменения в ресурсы среды выполнения в приложении. Дополнительные сведения о расширениях разметки см. в разделе [расширения разметки XAML](~/xamarin-forms/xaml/markup-extensions/index.md).

В следующем примере XAML показано, как использовать ресурсы, а также определять дополнительные ресурсы в [`StackLayout`](xref:Xamarin.Forms.StackLayout) :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ResourceDictionaryDemo.HomePage"
             Title="Home Page">
    <StackLayout Margin="{StaticResource PageMargin}">
        <StackLayout.Resources>
            <!-- Implicit style -->
            <Style TargetType="Button">
                <Setter Property="FontSize" Value="Medium" />
                <Setter Property="BackgroundColor" Value="#1976D2" />
                <Setter Property="TextColor" Value="White" />
                <Setter Property="CornerRadius" Value="5" />
            </Style>
        </StackLayout.Resources>

        <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries." />
        <Button Text="Navigate"
                Clicked="OnNavigateButtonClicked" />
    </StackLayout>
</ContentPage>
```

В этом примере [`ContentPage`](xref:Xamarin.Forms.ContentPage) объект использует неявный стиль, определенный в словаре ресурсов уровня приложения. [`StackLayout`](xref:Xamarin.Forms.StackLayout)Объект потребляет `PageMargin` ресурс, определенный в словаре ресурсов уровня приложения, а [`Button`](xref:Xamarin.Forms.Button) объект использует неявный стиль, определенный в [`StackLayout`](xref:Xamarin.Forms.StackLayout) словаре ресурсов. Результат показан на следующих снимках экрана.

[![Использование ресурсов ResourceDictionary](resource-dictionaries-images/consuming.png "Использование ресурсов ResourceDictionary")](resource-dictionaries-images/consuming-large.png#lightbox "Использование ресурсов ResourceDictionary")

> [!IMPORTANT]
> Ресурсы, относящиеся к одной странице, не должны включаться в словарь ресурсов на уровне приложения, так как такие ресурсы будут анализироваться при запуске приложения, а не при необходимости на странице. Дополнительные сведения см. [в разделе сокращение размера словаря ресурсов приложения](~/xamarin-forms/deploy-test/performance.md).

## <a name="resource-lookup-behavior"></a>Поведение при поиске ресурсов

Следующий процесс поиска происходит при ссылке на ресурс с [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) [`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension) расширением разметки или:

- Запрошенный ключ проверяется в словаре ресурсов, если он существует, для элемента, который задает свойство. Если запрошенный ключ найден, возвращается его значение и процесс поиска завершается.
- Если совпадение не найдено, процесс поиска выполняет поиск по визуальному дереву назад, проверяя словарь ресурсов каждого родительского элемента. Если запрошенный ключ найден, возвращается его значение и процесс поиска завершается. В противном случае процесс будет продолжаться до тех пор, пока не будет достигнут корневой элемент.
- Если совпадение не найдено в корневом элементе, проверяется словарь ресурсов уровня приложения.
- Если совпадение по-прежнему не найдено, `XamlParseException` создается исключение.

Таким образом, когда средство синтаксического анализа XAML встречает `StaticResource` `DynamicResource` расширение разметки или, оно ищет соответствующий ключ, переключаясь вверх по визуальному дереву, используя первое найденное совпадение. Если этот поиск заканчивается на странице, а ключ по-прежнему не найден, средство синтаксического анализа XAML выполняет поиск в `ResourceDictionary` объекте, присоединенном к `App` объекту. Если ключ по-прежнему не найден, возникает исключение.

## <a name="override-resources"></a>Переопределение ресурсов

Когда ресурсы совместно используют ключи, ресурсы, определенные ниже в визуальном дереве, имеют приоритет над теми, которые определены выше. Например, задание `AppBackgroundColor` ресурса на `AliceBlue` уровне приложения будет переопределено ресурсом уровня страницы, `AppBackgroundColor` имеющим значение `Teal` . Аналогичным образом, ресурс уровня страницы `AppBackgroundColor` будет переопределен ресурсом уровня элемента управления `AppBackgroundColor` .

## <a name="stand-alone-resource-dictionaries"></a>Независимые словари ресурсов

Класс, производный от, [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) также может находиться в автономном файле XAML. Затем файл XAML может совместно использоваться приложениями.

Чтобы создать такой файл, добавьте в проект новое **представление содержимого** или элемент **страницы содержимого** (но не **представление содержимого** или **страницу содержимого** с файлом C#). Удалите файл кода программной части и в XAML-файле измените имя базового класса с [`ContentView`](xref:Xamarin.Forms.ContentView) или [`ContentPage`](xref:Xamarin.Forms.ContentPage) на [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . Кроме того, удалите `x:Class` атрибут из корневого тега файла.

В следующем примере XAML показан [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) именованный **миресаурцедиктионари. XAML** :

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml">
    <DataTemplate x:Key="PersonDataTemplate">
        <ViewCell>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.5*" />
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.3*" />
                </Grid.ColumnDefinitions>
                <Label Text="{Binding Name}"
                       TextColor="{StaticResource NormalTextColor}"
                       FontAttributes="Bold" />
                <Label Grid.Column="1"
                       Text="{Binding Age}"
                       TextColor="{StaticResource NormalTextColor}" />
                <Label Grid.Column="2"
                       Text="{Binding Location}"
                       TextColor="{StaticResource NormalTextColor}"
                       HorizontalTextAlignment="End" />
            </Grid>
        </ViewCell>
    </DataTemplate>
</ResourceDictionary>
```

В этом примере [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) компонент содержит один ресурс, который является объектом типа [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . **Миресаурцедиктионари. XAML** можно использовать, объединив его с другим словарем ресурсов.

По умолчанию компоновщик удаляет автономные файлы XAML из сборок выпуска, если поведение компоновщика настроено для связывания всех сборок. Чтобы гарантировать, что автономные файлы XAML остаются в сборке выпуска, выполните следующие действия.

1. Добавьте настраиваемый `Preserve` атрибут в сборку, содержащую автономные файлы XAML. Дополнительные сведения см. в разделе [Сохранение кода](~/ios/deploy-test/linker.md).
1. Задайте `Preserve` атрибут на уровне сборки:

    ```csharp
    [assembly:Preserve(AllMembers = true)]
    ```

Дополнительные сведения о связывании см. в статье [связывание приложений Xamarin. iOS](~/ios/deploy-test/linker.md) и [связывание в Android](~/android/deploy-test/linker.md).

## <a name="merged-resource-dictionaries"></a>Объединенные словари ресурсов

Объединенные словари ресурсов объединяют один или несколько [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) объектов в другой `ResourceDictionary` .

### <a name="merge-local-resource-dictionaries"></a>Объединить словари локальных ресурсов

Локальный [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) файл можно объединить в другой `ResourceDictionary` , создав `ResourceDictionary` объект, [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) свойство которого имеет значение filename файла XAML с ресурсами:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <!-- Add more resources here -->
        <ResourceDictionary Source="MyResourceDictionary.xaml" />
        <!-- Add more resources here -->
    </ContentPage.Resources>
    ...
</ContentPage>
```

Этот синтаксис не создает экземпляр `MyResourceDictionary` класса. Вместо этого он ссылается на файл XAML. По этой причине при задании [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) свойства файл кода программной части не требуется, и `x:Class` атрибут можно удалить из корневого тега файла **миресаурцедиктионари. XAML** .

> [!IMPORTANT]
> [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source)Свойство может быть задано только из XAML.

### <a name="merge-resource-dictionaries-from-other-assemblies"></a>Объединить словари ресурсов из других сборок

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)Можно также объединить в другой `ResourceDictionary` , добавив его в [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) свойство объекта `ResourceDictionary` . Этот метод позволяет объединять словари ресурсов независимо от сборки, в которой они находятся. Для объединения словарей ресурсов из внешних сборок требуется, [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) чтобы для свойства для действия сборки было задано значение **EmbeddedResource** , чтобы имелся файл кода программной части, а также для определения `x:Class` атрибута в корневом теге файла.

> [!WARNING]
> [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)Класс также определяет [`MergedWith`](xref:Xamarin.Forms.ResourceDictionary.MergedWith) свойство. Однако это свойство является устаревшим и больше не должно использоваться.

В следующем примере кода показаны два словаря ресурсов, добавляемых в [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) коллекцию уровня страницы [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) :

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:ResourceDictionaryDemo"
             xmlns:theme="clr-namespace:MyThemes;assembly=MyThemes">
    <ContentPage.Resources>
        <ResourceDictionary>
            <!-- Add more resources here -->
            <ResourceDictionary.MergedDictionaries>
                <!-- Add more resource dictionaries here -->
                <local:MyResourceDictionary />
                <theme:LightTheme />
                <!-- Add more resource dictionaries here -->
            </ResourceDictionary.MergedDictionaries>
            <!-- Add more resources here -->
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

В этом примере словарь ресурсов из этой же сборки и словарь ресурсов из внешней сборки объединяются в словарь ресурсов на уровне страницы. Кроме того, можно добавлять другие `ResourceDictionary` объекты в [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) теги элементов свойств и другие ресурсы за пределами этих тегов.

> [!IMPORTANT]
> В может быть только один `MergedDictionaries` тег элемента Property [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) , но в нем можно разместить столько объектов, сколько `ResourceDictionary` требуется.

Если объединенные [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ресурсы имеют одинаковые `x:Key` значения атрибутов, Xamarin.Forms использует следующий приоритет ресурсов.

1. Ресурсы, локальные для словаря ресурсов.
1. Ресурсы, содержащиеся в словарях ресурсов, Объединенных через `MergedDictionaries` коллекцию в порядке, в котором они перечислены в `MergedDictionaries` свойстве.

> [!NOTE]
> Поиск словарей ресурсов может быть трудоемкой задачей, если приложение содержит несколько больших словарей ресурсов. Поэтому, чтобы избежать ненужных результатов поиска, необходимо убедиться, что каждая страница в приложении использует только словари ресурсов, подходящие для страницы.

## <a name="related-links"></a>Связанные ссылки

- [Словари ресурсов (пример)](/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)
- [Расширения разметки XAML](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Стили Xamarin.Forms](~/xamarin-forms/user-interface/styles/index.md)
- [Компоновка приложений Xamarin.iOS](~/ios/deploy-test/linker.md)
- [Linking on Android](~/android/deploy-test/linker.md) (Компоновка на Android)
- [API ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Application-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
