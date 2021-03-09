---
title: Всплывающий элемент оболочки Xamarin.Forms
description: Всплывающий элемент выполняет роль необязательного главного меню для приложения оболочки. Его можно вызвать специальным значком или жестом пальцем от края экрана. Всплывающий элемент состоит из входящих в него пунктов, а также (необязательно) заголовка, пунктов меню и нижнего колонтитула.
ms.prod: xamarin
ms.assetid: FEDE51EB-577E-4B3E-9890-B7C1A5E52516
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 82a8297d393e82119156509f20a7306d2ea34540
ms.sourcegitcommit: 1b542afc0f6f2f6adbced527ae47b9ac90eaa1de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101757882"
---
# <a name="xamarinforms-shell-flyout"></a>Всплывающий элемент оболочки Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Пользовательский интерфейс навигации, предоставляемый оболочкой Xamarin.Forms, основан на всплывающих окнах и вкладках. Всплывающее окно является необязательным корневым меню для приложения оболочки и является полностью настраиваемым. Чтобы открыть всплывающее окно, нажмите соответствующий значок или проведите пальцем от края экрана. Всплывающий элемент состоит из входящих в него пунктов, а также (необязательно) заголовка, пунктов меню и нижнего колонтитула:

![Снимок экрана со всплывающим меню и заметками в оболочке](flyout-images/flyout-annotated.png)

## <a name="flyout-items"></a>Элементы всплывающего меню

Во всплывающее меню можно добавлять пункты, каждый из которых представлен объектом [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem). Каждый объект `FlyoutItem` должен быть дочерним для объекта [`Shell`](xref:Xamarin.Forms.Shell). Если заголовок всплывающего меню отсутствует, элементы всплывающего меню отображаются от самого верха всплывающего меню.

Следующий пример создает всплывающее меню с двумя элементами:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <FlyoutItem Title="Cats"
                Icon="cat.png">
       <Tab>
           <ShellContent ContentTemplate="{DataTemplate views:CatsPage}" />
       </Tab>
    </FlyoutItem>
    <FlyoutItem Title="Dogs"
                Icon="dog.png">
       <Tab>
           <ShellContent ContentTemplate="{DataTemplate views:DogsPage}" />
       </Tab>
    </FlyoutItem>
</Shell>
```

Свойство [`FlyoutItem.Title`](xref:Xamarin.Forms.BaseShellItem.Title) типа `string` определяет заголовок всплывающего элемента. Свойство [`FlyoutItem.Icon`](xref:Xamarin.Forms.BaseShellItem.Icon) типа [`ImageSource`](xref:Xamarin.Forms.ImageSource) определяет значок всплывающего элемента:

[![Снимок экрана двухстраничного приложения оболочки с элементами всплывающего меню для iOS и Android](flyout-images/two-page-app-flyout.png)](flyout-images/two-page-app-flyout-large.png#lightbox)

В этом примере каждый объект [`ShellContent`](xref:Xamarin.Forms.ShellContent) доступен только через элементы всплывающего меню, а не через вкладки: Это связано с тем, что по умолчанию вкладки отображаются только в случае, если всплывающий элемент содержит более одной вкладки.

> [!IMPORTANT]
> В приложении оболочки страницы создаются по запросу в ответ на навигацию. Это достигается с помощью расширения разметки [`DataTemplate`](xref:Xamarin.Forms.Xaml.DataTemplateExtension) для задания свойства [`ContentTemplate`](xref:Xamarin.Forms.ShellContent.ContentTemplate) каждого объекта [`ShellContent`](xref:Xamarin.Forms.ShellContent) в соответствии с объектом [`ContentPage`](xref:Xamarin.Forms.ContentPage).

Оболочка содержит операторы неявного преобразования, которые позволяют упростить визуальную иерархию оболочки без добавления новых представлений в визуальное дерево. Это возможно, поскольку производный объект [`Shell`](xref:Xamarin.Forms.Shell) может содержать только объекты [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) или объект [`TabBar`](xref:Xamarin.Forms.TabBar), которые могут содержать только объекты [`Tab`](xref:Xamarin.Forms.Tab), которые, в свою очередь, могут содержать только объекты [`ShellContent`](xref:Xamarin.Forms.ShellContent). Эти операторы неявного преобразования позволяют удалить из предыдущего примера объекты `FlyoutItem` и `Tab`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
   <ShellContent Title="Cats"
                 Icon="cat.png"
                 ContentTemplate="{DataTemplate views:CatsPage}" />
   <ShellContent Title="Dogs"
                 Icon="dog.png"
                 ContentTemplate="{DataTemplate views:DogsPage}" />
</Shell>
```

Это неявное преобразование автоматически заключает каждый объект [`ShellContent`](xref:Xamarin.Forms.ShellContent) в объекты [`Tab`](xref:Xamarin.Forms.Tab), которые заключаются в объекты [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem).

> [!NOTE]
> Все объекты [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) в подклассах объекта [`Shell`](xref:Xamarin.Forms.Shell) добавляются в коллекцию `Shell.FlyoutItems`, которая определяет список элементов, отображаемых во всплывающем меню.

### <a name="flyout-display-options"></a>Параметры отображения всплывающего меню

Свойство [`FlyoutItem.FlyoutDisplayOptions`](xref:Xamarin.Forms.ShellGroupItem.FlyoutDisplayOptions) определяет, как всплывающий элемент и его дочерние элементы отображаются во всплывающем окне. Этому свойству должен быть присвоен член перечисления [`FlyoutDisplayOptions`](xref:Xamarin.Forms.FlyoutDisplayOptions):

- `AsSingleItem` указывает, будет ли элемент отображаться как один элемент; Это значение по умолчанию для свойства [`FlyoutDisplayOptions`](xref:Xamarin.Forms.ShellGroupItem.FlyoutDisplayOptions).
- `AsMultipleItems` указывает, что сам элемент и его дочерние элементы будут отображаться во всплывающем меню как группа элементов.

Всплывающий элемент для каждого объекта [`Tab`](xref:Xamarin.Forms.Tab) в [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) можно отобразить путем присвоения свойству [`FlyoutItem.FlyoutDisplayOptions`](xref:Xamarin.Forms.ShellGroupItem.FlyoutDisplayOptions) значения `AsMultipleItems`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       FlyoutHeaderBehavior="CollapseOnScroll"
       x:Class="Xaminals.AppShell">

    <FlyoutItem FlyoutDisplayOptions="AsMultipleItems">
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
        <ShellContent Title="Elephants"
                      Icon="elephant.png"
                      ContentTemplate="{DataTemplate views:ElephantsPage}" />  
        <ShellContent Title="Bears"
                      Icon="bear.png"
                      ContentTemplate="{DataTemplate views:BearsPage}" />
    </FlyoutItem>

    <ShellContent Title="About"
                  Icon="info.png"
                  ContentTemplate="{DataTemplate views:AboutPage}" />    
</Shell>
```

В этом примере элементы всплывающего меню создаются для объекта [`Tab`](xref:Xamarin.Forms.Tab), производного от объекта [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem), и объектов [`ShellContent`](xref:Xamarin.Forms.ShellContent), производных от объекта `FlyoutItem`. Это связано с тем, что каждый объект `ShellContent`, производный от объекта `FlyoutItem`, автоматически упаковывается в объект `Tab`. Кроме того, элемент всплывающего меню создается для последнего объекта `ShellContent`, который автоматически упаковывается в объект `Tab`, а затем в объект `FlyoutItem`.

> [!NOTE]
> Если элемент [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) содержит более одного объекта [`ShellContent`](xref:Xamarin.Forms.ShellContent), отображаются вкладки.

Все это создает такие элементы всплывающего меню:

[![Снимок экрана всплывающего меню с объектами FlyoutItem для iOS и Android](flyout-images/flyout-reduced.png "Всплывающее меню оболочки с объектами FlyoutItem")](flyout-images/flyout-reduced-large.png#lightbox "Всплывающее меню оболочки с объектами FlyoutItem")

### <a name="define-flyoutitem-appearance"></a>Определение внешнего вида FlyoutItem

Внешний вид каждого объекта [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) можно настроить, присвоив присоединенному свойству [`Shell.ItemTemplate`](xref:Xamarin.Forms.Shell.ItemTemplate) значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<Shell ...>
    ...
    <Shell.ItemTemplate>
        <DataTemplate>
            <Grid ColumnDefinitions="0.2*,0.8*">
                <Image Source="{Binding FlyoutIcon}"
                       Margin="5"
                       HeightRequest="45" />
                <Label Grid.Column="1"
                       Text="{Binding Title}"
                       FontAttributes="Italic"
                       VerticalTextAlignment="Center" />
            </Grid>
        </DataTemplate>
    </Shell.ItemTemplate>
</Shell>
```

Этот пример отображает заголовок каждого объекта [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) курсивом:

[![Снимок экрана шаблонных объектов FlyoutItem для iOS и Android](flyout-images/flyoutitem-templated.png)](flyout-images/flyoutitem-templated-large.png#lightbox)

Поскольку [`Shell.ItemTemplate`](xref:Xamarin.Forms.Shell.ItemTemplate) является присоединенным свойством, для отдельных объектов [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) можно задавать разные шаблоны.

> [!NOTE]
> Оболочка предоставляет свойства [`Title`](xref:Xamarin.Forms.BaseShellItem.Title) и [`FlyoutIcon`](xref:Xamarin.Forms.BaseShellItem.FlyoutIcon) для [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) в `ItemTemplate`.

Кроме того, оболочка содержит три класса стилей, которые автоматически применяются к объектам [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem). Дополнительные сведения см. в разделе [Объекты FlyoutItem и MenuItem](#style-flyoutitem-and-menuitem-objects).

### <a name="default-template-for-flyoutitems"></a>Шаблон по умолчанию для элементов FlyoutItem

Ниже показан [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) по умолчанию, используемый для каждого элемента [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem).

```xaml
<DataTemplate x:Key="FlyoutTemplate">
    <Grid x:Name="FlyoutItemLayout"
          HeightRequest="{x:OnPlatform Android=50}"
          ColumnSpacing="{x:OnPlatform UWP=0}"
          RowSpacing="{x:OnPlatform UWP=0}">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroupList>
                <VisualStateGroup x:Name="CommonStates">
                    <VisualState x:Name="Normal" />
                    <VisualState x:Name="Selected">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor"
                                    Value="{x:OnPlatform Android=#F2F2F2, iOS=#F2F2F2}" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateGroupList>
        </VisualStateManager.VisualStateGroups>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="{x:OnPlatform Android=54, iOS=50, UWP=Auto}" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Image x:Name="FlyoutItemImage"
               Source="{Binding FlyoutIcon}"
               VerticalOptions="Center"
               HorizontalOptions="{x:OnPlatform Default=Center, UWP=Start}"
               HeightRequest="{x:OnPlatform Android=24, iOS=22, UWP=16}"
               WidthRequest="{x:OnPlatform Android=24, iOS=22, UWP=16}">
            <Image.Margin>
                <OnPlatform x:TypeArguments="Thickness">
                    <OnPlatform.Platforms>
                        <On Platform="UWP"
                            Value="12,0,12,0" />
                    </OnPlatform.Platforms>
                </OnPlatform>
            </Image.Margin>
        </Image>
        <Label x:Name="FlyoutItemLabel"
               Grid.Column="1"
               Text="{Binding Title}"
               FontSize="{x:OnPlatform Android=14, iOS=Small}"
               HorizontalOptions="{x:OnPlatform UWP=Start}"
               HorizontalTextAlignment="{x:OnPlatform UWP=Start}"
               FontAttributes="{x:OnPlatform iOS=Bold}"
               VerticalTextAlignment="Center">
            <Label.TextColor>
                <OnPlatform x:TypeArguments="Color">
                    <OnPlatform.Platforms>
                        <On Platform="Android"
                            Value="#D2000000" />
                    </OnPlatform.Platforms>
                </OnPlatform>
            </Label.TextColor>
            <Label.Margin>
                <OnPlatform x:TypeArguments="Thickness">
                    <OnPlatform.Platforms>
                        <On Platform="Android"
                            Value="20, 0, 0, 0" />
                    </OnPlatform.Platforms>
                </OnPlatform>
            </Label.Margin>
            <Label.FontFamily>
                <OnPlatform x:TypeArguments="x:String">
                    <OnPlatform.Platforms>
                        <On Platform="Android"
                            Value="sans-serif-medium" />
                    </OnPlatform.Platforms>
                </OnPlatform>
            </Label.FontFamily>
        </Label>
    </Grid>
</DataTemplate>
```

Этот шаблон можно использовать в качестве основы для внесения изменений в существующий макет раскрывающегося меню, а также для отображения визуальных состояний, реализованных для элементов всплывающего меню.

Кроме того, все элементы [`Grid`](xref:Xamarin.Forms.Grid), [`Image`](xref:Xamarin.Forms.Image) и [`Label`](xref:Xamarin.Forms.Label) имеют значения `x:Name` и поэтому могут использоваться с диспетчером визуальных состояний. Дополнительные сведения см. в разделе [Задание состояния для нескольких элементов](~/xamarin-forms/user-interface/visual-state-manager.md#set-state-on-multiple-elements).

> [!NOTE]
> Этот же шаблон можно использовать для объектов [`MenuItem`](xref:Xamarin.Forms.MenuItem).

### <a name="replace-flyout-content"></a>Замена содержимого всплывающего меню

Элементы всплывающего меню, представляющие содержимое всплывающего меню, при необходимости можно заменить собственным содержимым, задав значение `object` для привязываемого свойства `Shell.FlyoutContent`:

```xaml
<Shell ...
       x:Name="shell">
    ...
    <Shell.FlyoutContent>
        <CollectionView BindingContext="{x:Reference shell}"
                        IsGrouped="True"
                        ItemsSource="{Binding FlyoutItems}">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <Label Text="{Binding Title}"
                           TextColor="White"
                           FontSize="Large" />
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </Shell.FlyoutContent>
</Shell>
```

В этом примере содержимое всплывающего меню заменяется содержимым [`CollectionView`](xref:Xamarin.Forms.CollectionView), в котором отображается заголовок каждого элемента в коллекции `FlyoutItems`.

> [!NOTE]
> Свойство `FlyoutItems` в классе [`Shell`](xref:Xamarin.Forms.Shell) — это доступная только для чтения коллекция элементов всплывающего меню.

Содержимое всплывающего меню также можно определить, задав значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) для привязываемого свойства `Shell.FlyoutContentTemplate`:

```xaml
<Shell ...
       x:Name="shell">
    ...
    <Shell.FlyoutContentTemplate>
        <DataTemplate>
            <CollectionView BindingContext="{x:Reference shell}"
                            IsGrouped="True"
                            ItemsSource="{Binding FlyoutItems}">
                <CollectionView.ItemTemplate>
                    <DataTemplate>
                        <Label Text="{Binding Title}"
                               TextColor="White"
                               FontSize="Large" />
                    </DataTemplate>
                </CollectionView.ItemTemplate>
            </CollectionView>
        </DataTemplate>
    </Shell.FlyoutContentTemplate>
</Shell>
```

> [!IMPORTANT]
> При желании заголовок всплывающего меню можно отобразить над содержимым всплывающего меню, а нижний колонтитул всплывающего меню — под содержимым всплывающего меню. Если содержимое всплывающего меню поддерживает прокрутку, то оболочка попытается обработать поведение прокрутки для заголовка всплывающего меню.

## <a name="menu-items"></a>Пункты меню

Во всплывающее меню можно при необходимости добавлять пункты меню, каждый из которых представлен объектом [`MenuItem`](xref:Xamarin.Forms.MenuItem). Положение объектов `MenuItem` во всплывающем меню определяется порядком их объявления в визуальной иерархии оболочки. Таким образом, любые объекты `MenuItem`, объявленные перед объектами [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem), отображаются перед объектами `FlyoutItem` во всплывающем меню, а любые объекты `MenuItem`, объявленные после объектов `FlyoutItem`, — после объектов `FlyoutItem` во всплывающем меню.

Класс [`MenuItem`](xref:Xamarin.Forms.MenuItem) имеет событие [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked) и свойство [`Command`](xref:Xamarin.Forms.MenuItem.Command). Это означает, что объекты `MenuItem` позволяют выполнять действия в ответ на касание `MenuItem`.

Объекты [`MenuItem`](xref:Xamarin.Forms.MenuItem) можно добавить в всплывающее окно, как показано в следующем примере:

```xaml
<Shell ...>
    ...            
    <MenuItem Text="Help"
              IconImageSource="help.png"
              Command="{Binding HelpCommand}"
              CommandParameter="https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell" />    
</Shell>
```

Этот пример добавляет объект [`MenuItem`](xref:Xamarin.Forms.MenuItem) во всплывающее меню под всеми всплывающими элементами:

[![Снимок экрана всплывающего меню с объектами MenuItem для iOS и Android](flyout-images/flyout.png)](flyout-images/flyout-large.png#lightbox)

Объект [`MenuItem`](xref:Xamarin.Forms.MenuItem) выполняет `ICommand` с именем `HelpCommand`, который открывает в веб-браузере URL-адрес, заданный свойством `CommandParameter`.

> [!NOTE]
> [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) для каждого элемента [`MenuItem`](xref:Xamarin.Forms.MenuItem) наследуется от производного объекта [`Shell`](xref:Xamarin.Forms.Shell).

### <a name="define-menuitem-appearance"></a>Определение внешнего вида MenuItem

Внешний вид каждого объекта [`MenuItem`](xref:Xamarin.Forms.MenuItem) можно настроить, присвоив присоединенному свойству [`Shell.MenuItemTemplate`](xref:Xamarin.Forms.Shell.MenuItemTemplate) значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<Shell ...>
    <Shell.MenuItemTemplate>
        <DataTemplate>
            <Grid ColumnDefinitions="0.2*,0.8*">
                <Image Source="{Binding Icon}"
                       Margin="5"
                       HeightRequest="45" />
                <Label Grid.Column="1"
                       Text="{Binding Text}"
                       FontAttributes="Italic"
                       VerticalTextAlignment="Center" />
            </Grid>
        </DataTemplate>
    </Shell.MenuItemTemplate>
    ...
    <MenuItem Text="Help"
              IconImageSource="help.png"
              Command="{Binding HelpCommand}"
              CommandParameter="https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell" />  
</Shell>
```

В этом примере [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) задается для каждого объекта [`MenuItem`](xref:Xamarin.Forms.MenuItem), чтобы заголовок каждого объекта `MenuItem` отображался курсивом:

[![Снимок экрана шаблонных объектов MenuItem для iOS и Android](flyout-images/menuitem-templated.png)](flyout-images/menuitem-templated-large.png#lightbox)

Поскольку [`Shell.MenuItemTemplate`](xref:Xamarin.Forms.Shell.ItemTemplate) является присоединенным свойством, для отдельных объектов [`MenuItem`](xref:Xamarin.Forms.MenuItem) можно задавать разные шаблоны.

> [!NOTE]
> Оболочка предоставляет свойства [`Text`](xref:Xamarin.Forms.MenuItem.Text) и [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource) для [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) в [`MenuItemTemplate`](xref:Xamarin.Forms.Shell.MenuItemTemplate). Можно также использовать `Title` вместо `Text` и `Icon` вместо `IconImageSource`, что позволяет повторно использовать один и тот же шаблон для пунктов меню и пунктов всплывающего элемента.

Шаблон по умолчанию для объектов [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) можно также использовать для объектов [`MenuItem`](xref:Xamarin.Forms.MenuItem). Дополнительные сведения см. в разделе [Шаблон по умолчанию для элементов FlyoutItem](#default-template-for-flyoutitems).

## <a name="style-flyoutitem-and-menuitem-objects"></a>Объекты Style FlyoutItem и MenuItem

Оболочка включает три класса стилей, которые автоматически применяются к объектам [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) и [`MenuItem`](xref:Xamarin.Forms.MenuItem). Классам стилей заданы следующие имена: `FlyoutItemLabelStyle`, `FlyoutItemImageStyle` и `FlyoutItemLayoutStyle`.

В следующем коде XAML показан пример определения стилей для этих классов стилей:

```xaml
<Style TargetType="Label"
       Class="FlyoutItemLabelStyle">
    <Setter Property="TextColor"
            Value="Black" />
    <Setter Property="HeightRequest"
            Value="100" />
</Style>

<Style TargetType="Image"
       Class="FlyoutItemImageStyle">
    <Setter Property="Aspect"
            Value="Fill" />
</Style>

<Style TargetType="Layout"
       Class="FlyoutItemLayoutStyle"
       ApplyToDerivedTypes="True">
    <Setter Property="BackgroundColor"
            Value="Teal" />
</Style>
```

Эти стили будут автоматически применены к объектам [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) и [`MenuItem`](xref:Xamarin.Forms.MenuItem), причем задавать их свойствам [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass) имена классов стилей не требуется.

Кроме того, для объектов [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) и [`MenuItem`](xref:Xamarin.Forms.MenuItem) можно определить и применить пользовательские классы стилей. Дополнительные сведения о классах стилей см. в статье [Классы стилей Xamarin.Forms](~/xamarin-forms/user-interface/styles/xaml/style-class.md).

## <a name="flyout-header"></a>Заголовок всплывающего меню

Заголовок всплывающего меню — это содержимое, которое при необходимости отображается в верхней части панели; его внешний вид, определяемый `object`, можно задать с помощью привязываемого свойства [`Shell.FlyoutHeader`](xref:Xamarin.Forms.Shell.FlyoutHeader):

```xaml
<Shell ...>
    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>
</Shell>
```

Тип `FlyoutHeader` показан в примере ниже.

```xaml
<ContentView xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Xaminals.Controls.FlyoutHeader"
             HeightRequest="200">
    <Grid BackgroundColor="Black">
        <Image Aspect="AspectFill"
               Source="xamarinstore.jpg"
               Opacity="0.6" />
        <Label Text="Animals"
               TextColor="White"
               FontAttributes="Bold"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />
    </Grid>
</ContentView>
```

Вот результат его применения к заголовку всплывающего меню:

![Снимок экрана с заголовком всплывающего меню](flyout-images/flyout-header.png)

Кроме того, вид заголовка всплывающего меню можно определить через привязываемое свойство [`Shell.FlyoutHeaderTemplate`](xref:Xamarin.Forms.Shell.FlyoutHeaderTemplate), присвоив ему значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<Shell ...>
    <Shell.FlyoutHeaderTemplate>
        <DataTemplate>
            <Grid BackgroundColor="Black"
                  HeightRequest="200">
                <Image Aspect="AspectFill"
                       Source="xamarinstore.jpg"
                       Opacity="0.6" />
                <Label Text="Animals"
                       TextColor="White"
                       FontAttributes="Bold"
                       HorizontalTextAlignment="Center"
                       VerticalTextAlignment="Center" />
            </Grid>            
        </DataTemplate>
    </Shell.FlyoutHeaderTemplate>
</Shell>
```

По умолчанию заголовок всплывающего меню будет зафиксирован во всплывающем элементе, хотя приведенное ниже его содержимое прокручивается в том случае, если элементов много. Тем не менее это поведение можно изменить, задав в привязываемом свойстве [`Shell.FlyoutHeaderBehavior`](xref:Xamarin.Forms.Shell.FlyoutHeaderBehavior) один из членов перечисления [`FlyoutHeaderBehavior`](xref:Xamarin.Forms.FlyoutHeaderBehavior):

- `Default` означает, что для полос прокрутки будет использоваться поведение, установленное для платформы по умолчанию. Это значение по умолчанию для свойства [`FlyoutHeaderBehavior`](xref:Xamarin.Forms.Shell.FlyoutHeaderBehavior).
- `Fixed` означает, что заголовок всплывающего меню все время остается видимым и не изменяется.
- `Scroll` указывает, что заголовок всплывающего меню пропадает с экрана, прокручиваясь вместе с другими элементами.
- `CollapseOnScroll` указывает, что заголовок всплывающего меню сворачивается до заглавия во время прокрутки элементов.

В следующем примере показано, как свернуть заголовок всплывающего меню при прокрутке пользователем:

```xaml
<Shell ...
       FlyoutHeaderBehavior="CollapseOnScroll">
    ...
</Shell>
```

## <a name="flyout-footer"></a>Нижний колонтитул всплывающего меню

Нижний колонтитул всплывающего меню — это содержимое, которое при необходимости отображается в нижней части элемента. Его внешний вид, определяемый в `object`, можно задать с помощью привязываемого свойства `Shell.FlyoutFooter`:

```xaml
<Shell ...>
    <Shell.FlyoutFooter>
        <controls:FlyoutFooter />
    </Shell.FlyoutFooter>
</Shell>
```

Тип `FlyoutFooter` показан в примере ниже.

```xaml
<ContentView xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=netstandard"
             x:Class="Xaminals.Controls.FlyoutFooter">
    <StackLayout>
        <Label Text="Xaminals"
               TextColor="GhostWhite"
               FontAttributes="Bold"
               HorizontalOptions="Center" />
        <Label Text="{Binding Source={x:Static sys:DateTime.Now}, StringFormat='{0:MMMM dd, yyyy}'}"
               TextColor="GhostWhite"
               HorizontalOptions="Center" />
    </StackLayout>
</ContentView>
```

В результате получается нижний колонтитул следующего вида:

![Снимок экрана нижнего колонтитула всплывающего меню](flyout-images/flyout-footer.png "Нижний колонтитул всплывающего меню")

Кроме того, вид нижнего колонтитула всплывающего меню можно определить через свойство `Shell.FlyoutFooterTemplate` для [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<Shell ...>
    <Shell.FlyoutFooterTemplate>
        <DataTemplate>
            <StackLayout>
                <Label Text="Xaminals"
                       TextColor="GhostWhite"
                       FontAttributes="Bold"
                       HorizontalOptions="Center" />
                <Label Text="{Binding Source={x:Static sys:DateTime.Now}, StringFormat='{0:MMMM dd, yyyy}'}"
                       TextColor="GhostWhite"
                       HorizontalOptions="Center" />
            </StackLayout>
        </DataTemplate>
    </Shell.FlyoutFooterTemplate>
</Shell>
```

Нижний колонтитул фиксируется в нижней части всплывающего окна и может иметь любую высоту. Кроме того, нижний колонтитул меню никогда не скрывает никаких его пунктов.

## <a name="flyout-width-and-height"></a>Ширина и высота всплывающего меню

Ширину и высоту всплывающего меню можно настроить, установив для присоединенных свойств `Shell.FlyoutWidth` и `Shell.FlyoutHeight` значение `double`:

```xaml
<Shell ...
       FlyoutWidth="400"
       FlyoutHeight="200">
    ...
</Shell>
```

Это позволяет выполнять такие сценарии как расширение всплывающего меню по всему экрану или уменьшение высоты всплывающего меню, чтобы оно не закрывало панель вкладок.

## <a name="flyout-icon"></a>Значок всплывающего меню

По умолчанию приложения оболочки оснащаются значком "гамбургера", при нажатии которого открывается всплывающее меню. Вы можете изменить этот значок, присвоив привязываемому свойству [`Shell.FlyoutIcon`](xref:Xamarin.Forms.Shell.FlyoutIcon) с типом [`ImageSource`](xref:Xamarin.Forms.ImageSource) значение, соответствующее нужному значку:

```xaml
<Shell ...
       FlyoutIcon="flyouticon.png">
    ...       
</Shell>
```

## <a name="flyout-background"></a>Фон всплывающего элемента

Задать цвет фона всплывающего элемента можно с помощью привязываемого свойства [`Shell.FlyoutBackgroundColor`](xref:Xamarin.Forms.Shell.FlyoutBackgroundColor).

```xaml
<Shell ...
       FlyoutBackgroundColor="AliceBlue">
    ...
</Shell>
```

> [!NOTE]
> Свойство [`Shell.FlyoutBackgroundColor`](xref:Xamarin.Forms.Shell.FlyoutBackgroundColor) также можно задать из каскадной таблицы стилей (CSS). Подробные сведения см. в разделе [Особые свойства оболочки Xamarin.Forms](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties).

Кроме того, можно указать фон всплывающего элемента, задав привязываемому свойству [`Shell.FlyoutBackground`](xref:Xamarin.Forms.Shell.FlyoutBackground) значение [`Brush`](xref:Xamarin.Forms.Brush).

```xaml
<Shell ...
       FlyoutBackground="LightGray">
    ...
</Shell>
```

В этом примере для фона всплывающего элемента выбран светло-серый цвет ([`SolidColorBrush`](xref:Xamarin.Forms.SolidColorBrush)).

В следующем примере показано, что для фона всплывающего элемента задано [`LinearGradientBrush`](xref:Xamarin.Forms.LinearGradientBrush):

```xaml
<Shell ...>
    <Shell.FlyoutBackground>
        <LinearGradientBrush StartPoint="0,0"
                             EndPoint="1,1">
            <GradientStop Color="#8A2387"
                          Offset="0.1" />
            <GradientStop Color="#E94057"
                          Offset="0.6" />
            <GradientStop Color="#F27121"
                          Offset="1.0" />
        </LinearGradientBrush>
    </Shell.FlyoutBackground>
    ...
</Shell>
```

Дополнительные сведения о кистях см. в статье [Кисти Xamarin.Forms](~/xamarin-forms/user-interface/brushes/index.md).

## <a name="flyout-background-image"></a>Фоновое изображение всплывающего элемента

Всплывающий элемент может иметь необязательное фоновое изображение, которое отображается под заголовком всплывающего элемента, а также за всеми всплывающими элементами, пунктами меню и нижним колонтитулом. Фоновое изображение можно указать, задав для привязываемого свойства [`FlyoutBackgroundImage`](xref:Xamarin.Forms.Shell.FlyoutBackgroundImage) типа [`ImageSource`](xref:Xamarin.Forms.ImageSource) файл, внедренный ресурс, URI или поток.

Пропорции фонового изображения можно настроить, установив для привязываемого свойства [`FlyoutBackgroundImageAspect`](xref:Xamarin.Forms.Shell.FlyoutBackgroundImageAspect) типа [`Aspect`](xref:Xamarin.Forms.Aspect) один из членов перечисления `Aspect`:

- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) — обрезает изображение таким образом, чтобы оно заполнило область экрана, сохраняя пропорции.
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) — осуществляет леттербоксинг изображения (при необходимости), чтобы изображение поместилось в область экрана, с добавлением пустого пространства в верхнюю, нижнюю или боковую часть в зависимости от ориентации изображения. Это значение по умолчанию для свойства [`FlyoutBackgroundImageAspect`](xref:Xamarin.Forms.Shell.FlyoutBackgroundImageAspect).
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) — растягивает изображение, чтобы полностью заполнить отображаемую область. Это может привести к искажению изображения.

В следующем примере показано задание этих свойств:

```xaml
<Shell ...
       FlyoutBackgroundImage="photo.jpg"
       FlyoutBackgroundImageAspect="AspectFill">
    ...
</Shell>
```

Это приводит к отображению фонового изображения во всплывающем элементе под заголовком:

![Снимок экрана: фоновое изображение всплывающего элемента](flyout-images/flyout-backgroundimage.png)

## <a name="flyout-backdrop"></a>Фон всплывающего элемента

Фон всплывающего элемента (внешний вид наложения всплывающего окна) можно указать, задав для [`Shell.FlyoutBackdrop`](xref:Xamarin.Forms.Shell.FlyoutBackdrop) присоединенное свойство [`Brush`](xref:Xamarin.Forms.Brush):

```xaml
<Shell ...
       FlyoutBackdrop="Silver">
    ...
</Shell>
```

В этом примере для фона всплывающего элемента выбран серебряный цвет ([`SolidColorBrush`](xref:Xamarin.Forms.SolidColorBrush)).

> [!IMPORTANT]
> Присоединенное свойство [`FlyoutBackdrop`](xref:Xamarin.Forms.Shell.FlyoutBackdrop) можно задать для любого элемента оболочки, но оно будет применяться только в том случае, если задано для объектов `Shell`, `FlyoutItem` или `TabBar`.

В следующем примере показано, что для фона всплывающего элемента указана кисть [`LinearGradientBrush`](xref:Xamarin.Forms.LinearGradientBrush):

```xaml
<Shell ...>
    <Shell.FlyoutBackdrop>
        <LinearGradientBrush StartPoint="0,0"
                             EndPoint="1,1">
            <GradientStop Color="#8A2387"
                          Offset="0.1" />
            <GradientStop Color="#E94057"
                          Offset="0.6" />
            <GradientStop Color="#F27121"
                          Offset="1.0" />
        </LinearGradientBrush>
    </Shell.FlyoutBackdrop>
    ...
</Shell>
```

Дополнительные сведения о кистях см. в статье [Кисти Xamarin.Forms](~/xamarin-forms/user-interface/brushes/index.md).

## <a name="flyout-behavior"></a>Поведение всплывающего меню

Всплывающее меню отображается, если нажать кнопку "гамбургер" или провести пальцем от края экрана. Но вы можете изменить это поведение, задав в присоединенном свойстве [`Shell.FlyoutBehavior`](xref:Xamarin.Forms.Shell.FlyoutBehavior) один из членов перечисления [`FlyoutBehavior`](xref:Xamarin.Forms.FlyoutBehavior):

- `Disabled` указывает, что пользователь не может открывать всплывающее меню;
- `Flyout` указывает, что пользователь может открывать и закрывать всплывающее меню. Это значение по умолчанию для свойства [`FlyoutBehavior`](xref:Xamarin.Forms.Shell.FlyoutBehavior).
- `Locked` указывает, что пользователь не может закрыть всплывающее меню, и это меню не перекрывает содержимое.

В следующем примере показано, как отключить всплывающее меню.

```xaml
<Shell ...
       FlyoutBehavior="Disabled">
    ...
</Shell>
```

> [!NOTE]
> Присоединенному свойству `FlyoutBehavior` можно присвоить значение `Shell`, `FlyoutItem`, `ShellContent` или объект страницы, чтобы переопределить поведение всплывающего меню по умолчанию.

## <a name="flyout-vertical-scroll"></a>Вертикальная прокрутка всплывающего меню

По умолчанию всплывающее меню можно прокручивать по вертикали, если элементы не помещаются в нем. Это поведение можно изменить, задав в привязываемом свойстве [`Shell.FlyoutVerticalScrollMode`](xref:Xamarin.Forms.Shell.FlyoutVerticalScrollMode) один из элементов перечисления [`ScrollMode`](xref:Xamarin.Forms.ScrollMode):

- `Disabled` — указывает, что вертикальная прокрутка будет отключена.
- `Enabled` — указывает, что вертикальная прокрутка будет включена.
- `Auto` — указывает, что вертикальная прокрутка будет включена, если элементы не помещаются во всплывающем меню. Это значение по умолчанию для свойства [`FlyoutVerticalScrollMode`](xref:Xamarin.Forms.Shell.FlyoutVerticalScrollMode).

В следующем примере показано, как отключить вертикальную прокрутку.

```xaml
<Shell ...
       FlyoutVerticalScrollMode="Disabled">
    ...
</Shell>
```

## <a name="flyoutitem-tab-order"></a>Последовательность табуляции для FlyoutItem

По умолчанию последовательность табуляции для объектов [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) соответствует порядку, в котором они перечислены в XAML или программно добавлены в дочернюю коллекцию. Этот порядок определяет порядок навигации по элементам `FlyoutItem` с клавиатуры, и часто порядок по умолчанию является оптимальным.

Вы можете изменить установленную по умолчанию последовательность табуляции, задав свойство [`FlyoutItem.TabIndex`](xref:Xamarin.Forms.BaseShellItem.TabIndex). Оно обозначает порядок, в котором объекты [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) будут получать фокус при переходе пользователя между элементами путем нажатия клавиши TAB. По умолчанию свойство равно 0, при этом оно может принимать любое значение `int`.

Следующие правила применяются при использовании последовательности табуляции по умолчанию или задании свойства [`TabIndex`](xref:Xamarin.Forms.BaseShellItem.TabIndex):

- Объекты [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) со значением 0 для свойства [`TabIndex`](xref:Xamarin.Forms.BaseShellItem.TabIndex) добавляются в последовательность табуляции в том порядке, в котором они объявлены в XAML или дочерних коллекциях.
- Объекты [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) со значением [`TabIndex`](xref:Xamarin.Forms.BaseShellItem.TabIndex) больше 0 добавляются в последовательность табуляции с учетом значений `TabIndex`.
- Объекты [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) со значением [`TabIndex`](xref:Xamarin.Forms.BaseShellItem.TabIndex) меньше 0 добавляются в последовательность табуляции и отображаются раньше, чем любой объект с нулевым значением.
- Конфликты для [`TabIndex`](xref:Xamarin.Forms.BaseShellItem.TabIndex) устраняются в порядке объявления.

Когда последовательность табуляции определена, нажатие клавиши TAB переключает фокус между объектами [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) в порядке возрастания значений [`TabIndex`](xref:Xamarin.Forms.BaseShellItem.TabIndex), а после достижения конечного элемента управления возвращает фокус в начало.

Кроме настройки последовательности табуляции для объектов [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem), может потребоваться исключить из этой последовательности некоторые объекты. Это можно сделать с помощью свойства [`FlyoutItem.IsTabStop`](xref:Xamarin.Forms.BaseShellItem.IsTabStop), которое указывает, включается ли `FlyoutItem` в навигацию по клавише TAB. Значение по умолчанию — `true`, а если оно равно `false`, элемент `FlyoutItem` игнорируется инфраструктурой навигации по клавише TAB, независимо от значения [`TabIndex`](xref:Xamarin.Forms.BaseShellItem.TabIndex).

## <a name="flyoutitem-selection"></a>Выбор FlyoutItem

При первом запуске приложения оболочки, использующего всплывающий элемент, свойству [`Shell.CurrentItem`](xref:Xamarin.Forms.Shell.CurrentItem) будет присвоено значение первого объекта `FlyoutItem` в производном объекте [`Shell`](xref:Xamarin.Forms.Shell). Но этому свойству можно присвоить значение другого `FlyoutItem`, как показано в следующем примере:

```xaml
<Shell ...
       CurrentItem="{x:Reference aboutItem}">
    <FlyoutItem FlyoutDisplayOptions="AsMultipleItems">
        ...
    </FlyoutItem>
    <ShellContent x:Name="aboutItem"
                  Title="About"
                  Icon="info.png"
                  ContentTemplate="{DataTemplate views:AboutPage}" />
</Shell>
```

В этом примере объекту [`ShellContent`](xref:Xamarin.Forms.ShellContent) с именем `aboutItem` задается свойство [`CurrentItem`](xref:Xamarin.Forms.Shell.CurrentItem), что приводит к его выделению и отображению. В нашем примере используется неявное преобразование для помещения объекта `ShellContent` в объект [`Tab`](xref:Xamarin.Forms.Tab), который упаковывается в объект [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem).

С учетом объекта [`ShellContent`](xref:Xamarin.Forms.ShellContent) с именем `aboutItem` эквивалентный код на C# выглядит так:

```csharp
CurrentItem = aboutItem;
```

В этом примере свойство [`CurrentItem`](xref:Xamarin.Forms.Shell.CurrentItem) задается в подклассе класса [`Shell`](xref:Xamarin.Forms.Shell). Кроме того, свойство `CurrentItem` может быть задано в любом классе с помощью статического свойства `Shell.Current`:

```csharp
Shell.Current.CurrentItem = aboutItem;
```

> [!NOTE]
> Приложение может ввести состояние, при котором выбор всплывающего элемента не является допустимой операцией. В таких случаях [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) можно отключить, задав свойству `IsEnabled` значение `false`. В результате пользователи не смогут выбрать всплывающий элемент.

## <a name="flyoutitem-visibility"></a>Видимость FlyoutItem

По умолчанию элементы всплывающего меню являются видимыми. Однако элемент может быть скрыт во всплывающем меню со свойством `FlyoutItemIsVisible` и удален из всплывающего окна со свойством [`IsVisible`](xref:Xamarin.Forms.BaseShellItem.IsVisible):

- `FlyoutItemIsVisible` типа `bool` указывает, скрыт ли элемент во всплывающем меню, но по-прежнему доступен с помощью метода навигации [`GoToAsync`](xref:Xamarin.Forms.Shell.GoToAsync*). Значение по умолчанию этого свойства равно `true`.
- [`IsVisible`](xref:Xamarin.Forms.BaseShellItem.IsVisible) типа `bool` указывает, следует ли удалять элемент из визуального дерева и, следовательно, не отображать во всплывающем окне. Значение по умолчанию — `true`.

В следующем примере показано скрытие элемента во всплывающем меню:

```xaml
<Shell ...>
    <FlyoutItem ...
                FlyoutItemIsVisible="False">
        ...
    </FlyoutItem>
</Shell>
```

> [!NOTE]
> Также имеется присоединенное свойство `Shell.FlyoutItemIsVisible`, которое можно задать для объектов [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem), [`MenuItem`](xref:Xamarin.Forms.MenuItem), [`Tab`](xref:Xamarin.Forms.Tab) и [`ShellContent`](xref:Xamarin.Forms.ShellContent).

## <a name="open-and-close-the-flyout-programmatically"></a>Открытие и закрытие всплывающего меню программным способом

Всплывающее меню можно открывать и закрывать программным образом, задавая привязываемому свойству [`Shell.FlyoutIsPresented`](xref:Xamarin.Forms.Shell.FlyoutIsPresented) значение `boolean`, которое определяет открытие всплывающего меню на текущий момент:

```xaml
<Shell ...
       FlyoutIsPresented="{Binding IsFlyoutOpen}">
</Shell>
```

Кроме того, это может быть выполнено в коде:

```csharp
Shell.Current.FlyoutIsPresented = false;
```

## <a name="related-links"></a>Связанные ссылки

- [Xaminals (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Классы стилей Xamarin.Forms](~/xamarin-forms/user-interface/styles/xaml/style-class.md)
- [Диспетчер визуального представления состояний Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md)
- [Кисти Xamarin.Forms](~/xamarin-forms/user-interface/brushes/index.md)
- [Особые свойства оболочки Xamarin.Forms](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)
