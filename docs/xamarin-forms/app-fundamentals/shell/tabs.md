---
title: Макет оболочки Xamarin.Forms
description: Следующим уровнем навигации после всплывающего меню в приложении оболочки является нижняя панель вкладок. Или же навигация по приложению может начинаться с нижней панели вкладок без использования всплывающего меню. В обоих случаях, если нижняя вкладка содержит более одной страницы, перемещение по ним осуществляется с помощью верхней панели вкладок.
ms.prod: xamarin
ms.assetid: 318D81DB-E456-4E44-B083-36A27DBD9523
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/06/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 59c7435273d7c01f93ef0bbfb0240e3917f8b716
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91563020"
---
# <a name="no-locxamarinforms-shell-tabs"></a>Вкладки оболочки Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Когда шаблон навигации для приложения предусматривает всплывающее меню, следующим уровнем навигации в приложении будет нижняя панель вкладок. Кроме того, когда всплывающее меню закрыто, верхним уровнем навигации в приложении будет нижняя панель вкладок.

Или же навигация по приложению может начинаться с нижней панели вкладок без использования всплывающего меню. В этом случае дочерний элемент объекта `Shell` должен быть объектом `TabBar`, представляющим нижнюю панель вкладок.

> [!NOTE]
> Тип `TabBar` отключает всплывающее меню.

Каждый объект `FlyoutItem` или `TabBar` может содержать один или несколько объектов `Tab`, где каждый объект `Tab` представляет вкладку на нижней панели. Каждый объект `Tab` может содержать один или несколько объектов `ShellContent`, где каждый объект `ShellContent` отображает один объект [`ContentPage`](xref:Xamarin.Forms.ContentPage). Если `Tab` содержит более одного объекта `ShellContent`, перемещение по объектам `ContentPage` осуществляется с помощью верхней панели вкладок.

В каждом объекте [`ContentPage`](xref:Xamarin.Forms.ContentPage) можно перейти к дополнительным объектам `ContentPage`. Дополнительные сведения о навигации см. в разделе [Навигация по оболочке Xamarin.Forms](navigation.md).

## <a name="single-page-application"></a>Одностраничное приложение

Простейшее приложение оболочки содержит одну страницу, и для его создания достаточно добавить один объект `Tab` в объект `TabBar`. В объекте `Tab` следует присвоить объекту `ShellContent` значение объекта [`ContentPage`](xref:Xamarin.Forms.ContentPage):

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab>
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
    </TabBar>
</Shell>
```

Этот код создаст такую структуру одностраничного приложения:

[![Снимок экрана одностраничного приложения оболочки для iOS и Android](tabs-images/single-page-app.png "Одностраничное приложение оболочки")](tabs-images/single-page-app-large.png#lightbox "Одностраничное приложение оболочки")

> [!NOTE]
> При необходимости панель навигации можно спрятать, присвоив присоединенному свойству `Shell.NavBarIsVisible` в объекте [`ContentPage`](xref:Xamarin.Forms.ContentPage) значение `false`.

Оболочка содержит операторы неявного преобразования, которые позволяют упростить визуальную иерархию оболочки без добавления новых представлений в визуальное дерево. Это возможно, так как производный объект `Shell` может содержать только объекты `FlyoutItem` или объект `TabBar`, которые могут содержать только объекты `Tab`, которые, в свою очередь, могут содержать только объекты `ShellContent`. Эти операторы неявного преобразования позволяют удалить из предыдущего примера объекты `TabBar`, `Tab` и `ShellContent`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <views:CatsPage />
</Shell>
```

Это неявное преобразование автоматически заключает объект [`ContentPage`](xref:Xamarin.Forms.ContentPage) в объект `ShellContent`, который заключается в объект `Tab`, который заключается в объект `FlyoutItem`. В одностраничном приложении всплывающий элемент не нужен, поэтому свойство `Shell.FlyoutBehavior` получает значение `Disabled`.

> [!IMPORTANT]
> В приложении оболочки каждый [`ContentPage`](xref:Xamarin.Forms.ContentPage), который является дочерним элементом объекта `ShellContent`, создается во время запуска приложения. Такой метод добавления объектов `ShellContent` приводит к созданию дополнительных страниц во время запуска приложения, что может замедлять запуск. Но оболочка может создавать страницы и по требованию, реагируя на переходы. Подробные сведения см. в разделе [об эффективной загрузке страниц](tabs.md#efficient-page-loading).

## <a name="bottom-tabs"></a>Нижние вкладки

Объекты `Tab` отображаются в виде нижних вкладок, если существует несколько объектов `Tab` в одном объекте `TabBar`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab Title="Cats"
             Icon="cat.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Dogs"
             Icon="dog.png">
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </TabBar>
</Shell>
```

Заголовки и значки для вкладок задаются отдельно для каждого объекта `Tab` и отображаются на нижних вкладках:

[![Снимок экрана двухстраничного приложения оболочки с нижними вкладками для iOS и Android](tabs-images/two-page-app-bottom-tabs.png "Двухстраничное приложение оболочки с нижними вкладками")](tabs-images/two-page-app-bottom-tabs-large.png#lightbox "Двухстраничное приложение оболочки с нижними вкладками")

При наличии более пяти вкладок появляется вкладка **Дополнительно**, используемая для доступа к дополнительным вкладкам.

[![Снимок экрана: приложение оболочки со вкладкой Дополнительно для iOS и Android](tabs-images/more-tabs.png "Приложение оболочки со вкладкой Дополнительно")](tabs-images/more-tabs-large.png#lightbox "Приложение оболочки со вкладками Дополнительно")

Вы также можете использовать операторы неявного преобразования оболочки для удаления объектов `ShellContent` и `Tab` из предыдущего примера:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <views:CatsPage IconImageSource="cat.png" />
        <views:DogsPage IconImageSource="dog.png" />
    </TabBar>
</Shell>
```

Это неявное преобразование автоматически заключает объект [`ContentPage`](xref:Xamarin.Forms.ContentPage) в объект `ShellContent`, а затем оба этих объекта — в объект `Tab`.

> [!IMPORTANT]
> В приложении оболочки каждый [`ContentPage`](xref:Xamarin.Forms.ContentPage), который является дочерним элементом объекта `ShellContent`, создается во время запуска приложения. Такой метод добавления объектов `ShellContent` приводит к созданию дополнительных страниц во время запуска приложения, что может замедлять запуск. Но оболочка может создавать страницы и по требованию, реагируя на переходы. Подробные сведения см. в разделе [об эффективной загрузке страниц](tabs.md#efficient-page-loading).

### <a name="tab-class"></a>Класс Tab

Класс `Tab` включает следующие свойства, которые определяют внешний вид и поведение вкладки:

- `CurrentItem` с типом `ShellContent` обозначает выбранный элемент.
- `FlyoutDisplayOptions` с типом `FlyoutDisplayOptions` определяет, как этот элемент и его дочерние элементы отображаются во всплывающем меню. Значение по умолчанию — `AsSingleItem`.
- `FlyoutIcon` с типом `ImageSource` определяет значок, который отображается во всплывающем элементе.
- `Icon` с типом `ImageSource` определяет значок, который отображается в частях хрома, не являющихся всплывающими элементами.
- `IsChecked` с типом `boolean` определяет, выделен ли этот элемент во всплывающем элементе в настоящий момент.
- `IsEnabled` с типом `boolean` определяет, можно ли выбрать элемент в хроме.
- `IsTabStop` с типом `bool` указывает, включается ли `Tab` в навигацию по клавише TAB. Значение по умолчанию — `true`, а если оно равно `false`, элемент `Tab` игнорируется инфраструктурой навигации по клавише TAB, независимо от значения `TabIndex`.
- `Items` с типом `IList<ShellContent>` определяет все содержимое в `Tab`.
- `TabIndex` с типом `int` обозначает порядок, в котором объекты `Tab` получают фокус при переходе пользователя между элементами посредством нажатия клавиши TAB. Это свойство по умолчанию имеет значение 0.
- `Title` с типом `string` определяет заголовок, отображаемый на вкладке в пользовательском интерфейсе.

## <a name="shell-content"></a>Содержимое оболочки

Дочерним элементом каждого объекта `Tab` является объект `ShellContent`, для свойства `Content` которого задается значение [`ContentPage`](xref:Xamarin.Forms.ContentPage):

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab Title="Cats"
             Icon="cat.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Dogs"
             Icon="dog.png">
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </TabBar>
</Shell>
```

В каждом объекте [`ContentPage`](xref:Xamarin.Forms.ContentPage) можно перейти к дополнительным объектам `ContentPage`. Дополнительные сведения о навигации см. в разделе [Навигация по оболочке Xamarin.Forms](navigation.md).

> [!NOTE]
> [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) каждого объекта `ShellContent` наследуется от родительского объекта `Tab`.

### <a name="shellcontent-class"></a>Класс ShellContent

Класс `ShellContent` включает следующие свойства, которые определяют внешний вид и поведение содержимого вкладки:

- `Content` с типом `object` определяет содержимое из `ShellContent`.
- `ContentTemplate` с типом `DataTemplate` определяет шаблон, на основе которого динамически оформляется содержимое `ShellContent`.
- `FlyoutIcon` с типом `ImageSource` определяет значок, который отображается во всплывающем элементе.
- `Icon` с типом `ImageSource` определяет значок, который отображается в частях хрома, не являющихся всплывающими элементами.
- `IsChecked` с типом `boolean` определяет, выделен ли этот элемент во всплывающем элементе в настоящий момент.
- `IsEnabled` с типом `boolean` определяет, можно ли выбрать элемент в хроме.
- `IsVisible` с типом `bool` указывает, скрыт ли объект `ShellContent` во всех структурах пользовательского интерфейса. Значение по умолчанию — `true`.
- `MenuItems` с типом `MenuItemCollection` обозначает элементы меню, которые отображаются во всплывающем элементе, если этот `ShellContent` является отображаемой страницей.
- `Title` с типом `string` определяет заголовок, отображаемый на вкладке в пользовательском интерфейсе.

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязки данных.

## <a name="bottom-and-top-tabs"></a>Нижние и верхние вкладки

Если `Tab` содержит более одного объекта `ShellContent`, вверху добавляется еще одна панель вкладок, с помощью которой осуществляется перемещение по объектам [`ContentPage`](xref:Xamarin.Forms.ContentPage):

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab Title="Domestic"
             Icon="domestic.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Monkeys"
             Icon="monkey.png">
            <ShellContent>
                <views:MonkeysPage />
            </ShellContent>
        </Tab>
    </TabBar>
</Shell>
```

В результате создается такой макет, как показано на следующих снимках экрана:

[![Снимок экрана двухстраничного приложения оболочки с верхними и нижними вкладками для iOS и Android](tabs-images/two-page-app-top-tabs.png "Двухстраничное приложение оболочки с верхними и нижними вкладками")](tabs-images/two-page-app-top-tabs-large.png#lightbox "Двухстраничное приложение оболочки с верхними и нижними вкладками")

Также вы можете использовать операторы неявного преобразования оболочки для удаления объектов `ShellContent` и второго объекта `Tab` из предыдущего примера:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab Title="Domestic"
             Icon="domestic.png">
            <views:CatsPage />
            <views:DogsPage />
        </Tab>
        <views:MonkeysPage IconImageSource="monkey.png" />
    </TabBar>
</Shell>
```

Это неявное преобразование автоматически заключает `MonkeysPage` в объект `ShellContent`, а его — в объект `Tab`. Кроме того, `CatsPage` и `DogsPage` неявным образом заключаются в объекты `ShellContent`.

## <a name="efficient-page-loading"></a>Эффективная загрузка страниц

В приложении оболочки каждый объект [`ContentPage`](xref:Xamarin.Forms.ContentPage) в объекте `ShellContent` создается во время запуска приложения, что может замедлять запуск. Но оболочка позволяет создавать страницы по требованию, реагируя на переходы. Для этого следует применить расширение разметки `DataTemplate`, чтобы преобразовать каждый `ContentPage` в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) и сохранить полученный результат в свойстве `ShellContent.ContentTemplate`:

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

Этот XAML создает и отображает `CatsPage`, то есть первый элемент содержимого, объявленный в производном объекте `Shell`. Переход между `DogsPage` и `MonkeysPage` осуществляется через нижние вкладки, и эти страницы создаются только тогда, когда пользователь переходит к ним. Преимуществом этого подхода является ускорение запуска приложения, ведь теперь страницы создаются только по запросу в ответ на действия навигации, а не сразу при запуске приложения.

## <a name="tab-appearance"></a>Внешний вид вкладок

Класс `Shell` предоставляет следующие присоединенные свойства, которые определяют внешний вид вкладок:

- `TabBarBackgroundColor` с типом `Color` определяет цвет фона для панели вкладок. Если это свойство не задано, используется значение свойства `BackgroundColor`.
- `TabBarDisabledColor` с типом `Color` определяет цвет отключенных вкладок на панели. Если это свойство не задано, используется значение свойства `DisabledColor`.
- `TabBarForegroundColor` с типом `Color` определяет цвет переднего плана для панели вкладок. Если это свойство не задано, используется значение свойства `ForegroundColor`.
- `TabBarTitleColor` с типом `Color` определяет цвет заголовков для панели вкладок. Если это свойство не задано, используется значение свойства `TitleColor`.
- `TabBarUnselectedColor` с типом `Color` определяет цвет невыбранных вкладок на панели. Если это свойство не задано, используется значение свойства `UnselectedColor`.

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязки данных, а также задавать им стиль.

В следующем примере показан стиль XAML, который задает разные свойства для цветов вкладки:

```xaml
<Style x:Key="BaseStyle"
       TargetType="Element">
    <Setter Property="Shell.TabBarBackgroundColor"
            Value="#3498DB" />
    <Setter Property="Shell.TabBarTitleColor"
            Value="White" />
    <Setter Property="Shell.TabBarUnselectedColor"
            Value="#B4FFFFFF" />
</Style>
```

Кроме того, стиль вкладок можно задать с помощью каскадных таблиц стилей (CSS). Подробные сведения см. в разделе [Особые свойства оболочки Xamarin.Forms](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties).

## <a name="related-links"></a>Связанные ссылки

- [Xaminals (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Навигация по оболочке Xamarin.Forms](navigation.md)
- [Особые свойства каскадных таблиц стилей оболочки Xamarin.Forms](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)