---
title: Вкладки оболочки Xamarin.Forms
description: Следующим уровнем навигации после всплывающего меню в приложении оболочки является нижняя панель вкладок. Или же навигация по приложению может начинаться с нижней панели вкладок без использования всплывающего меню. В обоих случаях, если нижняя вкладка содержит более одной страницы, перемещение по ним осуществляется с помощью верхней панели вкладок.
ms.prod: xamarin
ms.assetid: 318D81DB-E456-4E44-B083-36A27DBD9523
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/18/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4dcc1e30a89fc5a1e804ffa07272dd2b28b37c04
ms.sourcegitcommit: 1b542afc0f6f2f6adbced527ae47b9ac90eaa1de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101758300"
---
# <a name="xamarinforms-shell-tabs"></a>Вкладки оболочки Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Пользовательский интерфейс навигации, предоставляемый оболочкой Xamarin.Forms, основан на всплывающих окнах и вкладках. Верхний уровень навигации в приложении оболочки — это всплывающее меню или нижняя панель вкладок, выбор которых обусловлен требованиями навигации в приложении. Когда навигация в приложении начинается с нижних вкладок, дочерним объектом производного объекта [`Shell`](xref:Xamarin.Forms.Shell) должен быть объект [`TabBar`](xref:Xamarin.Forms.TabBar), представляющий нижнюю панель вкладок.

Каждый объект [`TabBar`](xref:Xamarin.Forms.TabBar) может содержать один или несколько объектов [`Tab`](xref:Xamarin.Forms.Tab), где каждый объект `Tab` представляет вкладку на нижней панели. Каждый объект `Tab` может содержать один или несколько объектов [`ShellContent`](xref:Xamarin.Forms.ShellContent), где каждый объект `ShellContent` отображает один объект [`ContentPage`](xref:Xamarin.Forms.ContentPage). Если `Tab` содержит более одного объекта `ShellContent`, перемещение по объектам `ContentPage` осуществляется с помощью верхней панели вкладок. На вкладке можно перемещаться по дополнительным объектам `ContentPage`, называемым страницами сведений.

> [!IMPORTANT]
> Тип [`TabBar`](xref:Xamarin.Forms.TabBar) отключает всплывающее меню.

## <a name="single-page"></a>Одностраничное приложение

Одностраничное приложение оболочки можно создать, добавив объект [`Tab`](xref:Xamarin.Forms.Tab) в объект [`TabBar`](xref:Xamarin.Forms.TabBar). В объекте `Tab` объекту [`ShellContent`](xref:Xamarin.Forms.ShellContent) следует присвоить значение объекта [`ContentPage`](xref:Xamarin.Forms.ContentPage):

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
       <Tab>
           <ShellContent ContentTemplate="{DataTemplate views:CatsPage}" />
       </Tab>
    </TabBar>
</Shell>
```

Этот код создаст такую структуру одностраничного приложения:

[![Снимок экрана одностраничного приложения оболочки для iOS и Android](tabs-images/single-page-app.png)](tabs-images/single-page-app-large.png#lightbox)

Оболочка содержит операторы неявного преобразования, которые позволяют упростить визуальную иерархию оболочки без добавления новых представлений в визуальное дерево. Это возможно, поскольку производный объект `Shell` может содержать только объекты [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) или объект [`TabBar`](xref:Xamarin.Forms.TabBar), которые могут содержать только объекты [`Tab`](xref:Xamarin.Forms.Tab), которые, в свою очередь, могут содержать только объекты [`ShellContent`](xref:Xamarin.Forms.ShellContent). Эти операторы неявного преобразования позволяют удалить из предыдущего примера объекты `Tab`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <Tab>
        <ShellContent ContentTemplate="{DataTemplate views:CatsPage}" />
    </Tab>
</Shell>
```

Это неявное преобразование автоматически заключает [`ShellContent`](xref:Xamarin.Forms.ShellContent) в объект [`Tab`](xref:Xamarin.Forms.Tab), а его — в объект [`TabBar`](xref:Xamarin.Forms.TabBar).

> [!IMPORTANT]
> В приложении оболочки страницы создаются по запросу в ответ на навигацию. Это достигается с помощью расширения разметки [`DataTemplate`](xref:Xamarin.Forms.Xaml.DataTemplateExtension) для задания свойства [`ContentTemplate`](xref:Xamarin.Forms.ShellContent.ContentTemplate) каждого объекта [`ShellContent`](xref:Xamarin.Forms.ShellContent) в соответствии с объектом [`ContentPage`](xref:Xamarin.Forms.ContentPage).

## <a name="bottom-tabs"></a>Нижние вкладки

Объекты [`Tab`](xref:Xamarin.Forms.Tab) отображаются в виде нижних вкладок, если существует несколько объектов `Tab` в одном объекте [`TabBar`](xref:Xamarin.Forms.TabBar):

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
       <Tab Title="Cats"
            Icon="cat.png">
           <ShellContent ContentTemplate="{DataTemplate views:CatsPage}" />
       </Tab>
       <Tab Title="Dogs"
            Icon="dog.png">
           <ShellContent ContentTemplate="{DataTemplate views:DogsPage}" />
       </Tab>
    </TabBar>
</Shell>
```

Свойство [`Title`](xref:Xamarin.Forms.BaseShellItem.Title) типа `string` определяет заголовок вкладки. Свойство [`Icon`](xref:Xamarin.Forms.BaseShellItem.Icon) типа [`ImageSource`](xref:Xamarin.Forms.ImageSource) определяет значок вкладки:

[![Снимок экрана двухстраничного приложения оболочки с нижними вкладками для iOS и Android](tabs-images/two-page-app-bottom-tabs.png)](tabs-images/two-page-app-bottom-tabs-large.png#lightbox)

При наличии более пяти вкладок в [`TabBar`](xref:Xamarin.Forms.TabBar) появляется вкладка **Дополнительно**, используемая для доступа к дополнительным вкладкам.

[![Снимок экрана: приложение оболочки со вкладкой "Дополнительно" для iOS и Android](tabs-images/more-tabs.png)](tabs-images/more-tabs-large.png#lightbox)

Вы также можете использовать операторы неявного преобразования оболочки для удаления объектов [`ShellContent`](xref:Xamarin.Forms.ShellContent) и [`Tab`](xref:Xamarin.Forms.Tab) из предыдущего примера:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
       <ShellContent Title="Cats"
                     Icon="cat.png"
                     ContentTemplate="{DataTemplate views:CatsPage}" />
       <ShellContent Title="Dogs"
                     Icon="dog.png"
                     ContentTemplate="{DataTemplate views:DogsPage}" />
    </TabBar>
</Shell>
```

Это неявное преобразование автоматически заключает объект [`ShellContent`](xref:Xamarin.Forms.ShellContent) в объект [`Tab`](xref:Xamarin.Forms.Tab).

> [!IMPORTANT]
> В приложении оболочки страницы создаются по запросу в ответ на навигацию. Это достигается с помощью расширения разметки [`DataTemplate`](xref:Xamarin.Forms.Xaml.DataTemplateExtension) для задания свойства [`ContentTemplate`](xref:Xamarin.Forms.ShellContent.ContentTemplate) каждого объекта [`ShellContent`](xref:Xamarin.Forms.ShellContent) в соответствии с объектом [`ContentPage`](xref:Xamarin.Forms.ContentPage).

## <a name="bottom-and-top-tabs"></a>Нижние и верхние вкладки

Если [`ShellContent`](xref:Xamarin.Forms.ShellContent) содержит более одного объекта [`Tab`](xref:Xamarin.Forms.Tab), вверху добавляется еще одна панель вкладок, с помощью которой осуществляется перемещение по объектам [`ContentPage`](xref:Xamarin.Forms.ContentPage):

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
       <Tab Title="Domestic"
            Icon="paw.png">
           <ShellContent Title="Cats"
                         ContentTemplate="{DataTemplate views:CatsPage}" />
           <ShellContent Title="Dogs"
                         ContentTemplate="{DataTemplate views:DogsPage}" />
       </Tab>
       <Tab Title="Monkeys"
            Icon="monkey.png">
           <ShellContent ContentTemplate="{DataTemplate views:MonkeysPage}" />
       </Tab>
    </TabBar>
</Shell>
```

В результате создается такой макет, как показано на следующих снимках экрана:

[![Снимок экрана двухстраничного приложения оболочки с верхними и нижними вкладками для iOS и Android](tabs-images/two-page-app-top-tabs.png "Двухстраничное приложение оболочки с верхними и нижними вкладками")](tabs-images/two-page-app-top-tabs-large.png#lightbox "Двухстраничное приложение оболочки с верхними и нижними вкладками")

Также вы можете использовать операторы неявного преобразования оболочки для удаления второго объекта [`Tab`](xref:Xamarin.Forms.Tab) из предыдущего примера:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
       <Tab Title="Domestic"
            Icon="paw.png">
           <ShellContent Title="Cats"
                         Icon="cat.png"
                         ContentTemplate="{DataTemplate views:CatsPage}" />
           <ShellContent Title="Dogs"
                         Icon="dog.png"
                         ContentTemplate="{DataTemplate views:DogsPage}" />
       </Tab>
       <ShellContent Title="Monkeys"
                     Icon="monkey.png"
                     ContentTemplate="{DataTemplate views:MonkeysPage}" />
    </TabBar>
</Shell>
```

Это неявное преобразование автоматически заключает в оболочку третий объект [`ShellContent`](xref:Xamarin.Forms.ShellContent) в объекте [`Tab`](xref:Xamarin.Forms.Tab).

## <a name="tab-appearance"></a>Внешний вид вкладок

Класс [`Shell`](xref:Xamarin.Forms.Shell) предоставляет следующие присоединенные свойства, которые определяют внешний вид вкладок:

- [`TabBarBackgroundColor`](xref:Xamarin.Forms.Shell.TabBarBackgroundColorProperty) с типом [`Color`](xref:Xamarin.Forms.Color) определяет цвет фона для панели вкладок. Если это свойство не задано, используется значение свойства `BackgroundColor`.
- [`TabBarDisabledColor`](xref:Xamarin.Forms.Shell.TabBarDisabledColorProperty) с типом [`Color`](xref:Xamarin.Forms.Color) определяет цвет отключенных вкладок на панели. Если это свойство не задано, используется значение свойства `DisabledColor`.
- [`TabBarForegroundColor`](xref:Xamarin.Forms.Shell.TabBarForegroundColorProperty) с типом [`Color`](xref:Xamarin.Forms.Color) определяет цвет переднего плана для панели вкладок. Если это свойство не задано, используется значение свойства `ForegroundColor`.
- [`TabBarTitleColor`](xref:Xamarin.Forms.Shell.TabBarTitleColorProperty) с типом [`Color`](xref:Xamarin.Forms.Color) определяет цвет заголовков для панели вкладок. Если это свойство не задано, используется значение свойства `TitleColor`.
- [`TabBarUnselectedColor`](xref:Xamarin.Forms.Shell.TabBarUnselectedColorProperty) с типом [`Color`](xref:Xamarin.Forms.Color) определяет цвет невыбранных вкладок на панели. Если это свойство не задано, используется значение свойства `UnselectedColor`.

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязки данных, а также задавать им стиль.

В следующем примере показан стиль XAML, который задает разные свойства для цветов панели вкладок:

```xaml
<Style TargetType="TabBar">
    <Setter Property="Shell.TabBarBackgroundColor"
            Value="CornflowerBlue" />
    <Setter Property="Shell.TabBarTitleColor"
            Value="Black" />
    <Setter Property="Shell.TabBarUnselectedColor"
            Value="AntiqueWhite" />
</Style>
```

Кроме того, стиль вкладок можно задать с помощью каскадных таблиц стилей (CSS). Подробные сведения см. в разделе [Особые свойства оболочки Xamarin.Forms](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties).

## <a name="tab-selection"></a>Выбор вкладки

При первом запуске приложения оболочки, использующего панель вкладок, свойству [`Shell.CurrentItem`](xref:Xamarin.Forms.Shell.CurrentItem) будет присвоено значение первого объекта [`Tab`](xref:Xamarin.Forms.Tab) в производном объекте [`Shell`](xref:Xamarin.Forms.Shell). Но этому свойству можно присвоить значение другого `Tab`, как показано в следующем примере:

```xaml
<Shell ...
       CurrentItem="{x:Reference dogsItem}">
    <TabBar>
        <ShellContent Title="Cats"
                      Icon="cat.png"
                      ContentTemplate="{DataTemplate views:CatsPage}" />
        <ShellContent x:Name="dogsItem"
                      Title="Dogs"
                      Icon="dog.png"
                      ContentTemplate="{DataTemplate views:DogsPage}" />
    </TabBar>
</Shell>
```

В этом примере объекту [`ShellContent`](xref:Xamarin.Forms.ShellContent) с именем `dogsItem` задается свойство [`CurrentItem`](xref:Xamarin.Forms.Shell.CurrentItem), что приводит к его выделению и отображению. В нашем примере используется неявное преобразование для помещения каждого объекта `ShellContent` в объект [`Tab`](xref:Xamarin.Forms.Tab).

С учетом объекта [`ShellContent`](xref:Xamarin.Forms.ShellContent) с именем `dogsItem` эквивалентный код на C# выглядит так:

```csharp
CurrentItem = dogsItem;
```

В этом примере свойство [`CurrentItem`](xref:Xamarin.Forms.Shell.CurrentItem) задается в подклассе класса [`Shell`](xref:Xamarin.Forms.Shell). Кроме того, свойство `CurrentItem` может быть задано в любом классе с помощью статического свойства `Shell.Current`:

```csharp
Shell.Current.CurrentItem = dogsItem;
```

## <a name="tabbar-and-tab-visibility"></a>Видимость TabBar и Tab

Панель вкладок и вкладки отображаются в приложениях оболочки по умолчанию. Однако панель вкладок можно скрыть, задав для присоединенного свойства [`Shell.TabBarIsVisible`](xref:Xamarin.Forms.Shell.TabBarIsVisibleProperty) значение `false`.

Хотя это свойство можно задать в производном объекте [`Shell`](xref:Xamarin.Forms.Shell), оно обычно задается для любого объекта [`ShellContent`](xref:Xamarin.Forms.ShellContent) или [`ContentPage`](xref:Xamarin.Forms.ContentPage), где нужно скрыть панель вкладок:

```xaml
<TabBar>
   <Tab Title="Domestic"
        Icon="paw.png">
       <ShellContent Title="Cats"
                     ContentTemplate="{DataTemplate views:CatsPage}" />
       <ShellContent Shell.TabBarIsVisible="false"
                     Title="Dogs"
                     ContentTemplate="{DataTemplate views:DogsPage}" />
   </Tab>
   <Tab Title="Monkeys"
        Icon="monkey.png">
       <ShellContent ContentTemplate="{DataTemplate views:MonkeysPage}" />
   </Tab>
</TabBar>
```

В этом примере панель вкладок скрывается при выборе вкладки **Dogs** вверху.

Кроме того, объекты [`Tab`](xref:Xamarin.Forms.Tab) можно скрыть, задав для привязываемого свойства [`IsVisible`](xref:Xamarin.Forms.BaseShellItem.IsVisible) значение `false`:

```xaml
<TabBar>
    <ShellContent Title="Cats"
                  Icon="cat.png"
                  ContentTemplate="{DataTemplate views:CatsPage}" />
    <ShellContent Title="Dogs"
                  Icon="dog.png"
                  ContentTemplate="{DataTemplate views:DogsPage}"
                  IsVisible="False" />
    <ShellContent Title="Monkeys"
                  Icon="monkey.png"
                  ContentTemplate="{DataTemplate views:MonkeysPage}" />
</TabBar>
```

В этом примере вторая вкладка скрыта.

## <a name="related-links"></a>Связанные ссылки

- [Xaminals (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Навигация по оболочке Xamarin.Forms](navigation.md)
- [Особые свойства каскадных таблиц стилей оболочки Xamarin.Forms](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)
