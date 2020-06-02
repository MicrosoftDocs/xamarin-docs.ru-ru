---
title: Всплывающий элемент оболочки Xamarin.Forms
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
ms.openlocfilehash: b336a594fa7525000e119333b56284368a23cc03
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84134957"
---
# <a name="xamarinforms-shell-flyout"></a>Всплывающий элемент оболочки Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Всплывающее меню выполняет роль главного меню для приложения оболочки. Его можно вызвать специальным значком или жестом пальцем от края экрана. Всплывающее меню состоит из необязательного заголовка, вложенных элементов всплывающего меню и необязательных пунктов меню.

![Снимок экрана со всплывающим меню и заметками в оболочке](flyout-images/flyout-annotated.png "Всплывающий элемент с заметками")

При необходимости вы можете задать для фона всплывающего меню цвет [`Color`](xref:Xamarin.Forms.Color), используя привязываемое свойство `Shell.FlyoutBackgroundColor`. Это свойство также можно задать из каскадной таблицы стилей (CSS). Подробные сведения см. в разделе [Особые свойства оболочки Xamarin.Forms](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties).

## <a name="flyout-icon"></a>Значок всплывающего меню

По умолчанию приложения оболочки оснащаются значком "гамбургера", при нажатии которого открывается всплывающее меню. Вы можете изменить этот значок, присвоив привязываемому свойству `Shell.FlyoutIcon` с типом [`ImageSource`](xref:Xamarin.Forms.ImageSource) значение, соответствующее нужному значку:

```xaml
<Shell ...
       FlyoutIcon="flyouticon.png">
    ...       
</Shell>
```

## <a name="flyout-behavior"></a>Поведение всплывающего меню

Всплывающее меню отображается, если нажать кнопку "гамбургер" или провести пальцем от края экрана. Но вы можете изменить это поведение, задав в присоединенном свойстве `Shell.FlyoutBehavior` один из членов перечисления `FlyoutBehavior`:

- `Disabled` указывает, что пользователь не может открывать всплывающее меню;
- `Flyout` указывает, что пользователь может открывать и закрывать всплывающее меню. Это значение по умолчанию для свойства `FlyoutBehavior`;
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

Кроме того, всплывающее меню можно открывать и закрывать программным образом, задавая привязываемому свойству `Shell.FlyoutIsPresented` значение `boolean`, которое определяет видимость всплывающего меню на текущий момент:

```csharp
Shell.Current.FlyoutIsPresented = false;
```

## <a name="flyout-header"></a>Заголовок всплывающего меню

Заголовок всплывающего меню — это содержимое, которое при необходимости отображается в верхней части панели; его внешний вид, определяемый `object`, можно задать с помощью значения свойства `Shell.FlyoutHeader`:

```xaml
<Shell.FlyoutHeader>
    <controls:FlyoutHeader />
</Shell.FlyoutHeader>
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

![Снимок экрана с заголовком всплывающего меню](flyout-images/flyout-header.png "Заголовок всплывающего меню")

Кроме того, вид заголовка всплывающего меню можно определить через свойство `Shell.FlyoutHeaderTemplate`, присвоив ему значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
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
```

По умолчанию заголовок всплывающего меню будет зафиксирован во всплывающем элементе, хотя приведенное ниже его содержимое прокручивается в том случае, если элементов много. Тем не менее, это поведение можно изменить, задав в привязываемом свойстве `Shell.FlyoutHeaderBehavior` один из членов перечисления `FlyoutHeaderBehavior`:

- `Default` означает, что для полос прокрутки будет использоваться поведение, установленное для платформы по умолчанию. Это значение по умолчанию для свойства `FlyoutHeaderBehavior`.
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

## <a name="flyout-background-image"></a>Фоновое изображение всплывающего элемента

Всплывающий элемент может иметь необязательное фоновое изображение, которое отображается под заголовком всплывающего элемента, а также за всеми всплывающими элементами и пунктами меню. Фоновое изображение можно указать, задав для привязываемого свойства `FlyoutBackgroundImage` типа [`ImageSource`](xref:Xamarin.Forms.ImageSource) файл, внедренный ресурс, URI или поток.

Пропорции фонового изображения можно настроить, установив для привязываемого свойства `FlyoutBackgroundImageAspect` типа [`Aspect`](xref:Xamarin.Forms.Aspect)один из членов перечисления `Aspect`:

- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) — обрезает изображение таким образом, чтобы оно заполнило область экрана, сохраняя пропорции.
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) — осуществляет леттербоксинг изображения (при необходимости), чтобы изображение поместилось в область экрана, с добавлением пустого пространства в верхнюю, нижнюю или боковую часть в зависимости от ориентации изображения.
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) — растягивает изображение, чтобы полностью заполнить отображаемую область. Это может привести к искажению изображения.

По умолчанию свойство `FlyoutBackgroundImageAspect` имеет значение `AspectFit`.

В следующем примере показано задание этих свойств:

```xaml
<Shell ...
       FlyoutBackgroundImage="photo.jpg"
       FlyoutBackgroundImageAspect="AspectFill">
    ...
</Shell>
```

Это приводит к отображению фонового изображения во всплывающем элементе:

![Снимок экрана: фоновое изображение всплывающего элемента](flyout-images/flyout-backgroundimage.png "Фоновое изображение всплывающего элемента")

## <a name="flyout-items"></a>Элементы всплывающего меню

Если шаблон навигации по приложению включает всплывающее меню, производный объект `Shell` должен содержать один или несколько объектов `FlyoutItem`, где каждый объект `FlyoutItem` представляет один такой элемент. Каждый объект `FlyoutItem` должен быть дочерним для объекта `Shell`.

Следующий пример создает всплывающее меню с заголовком и двумя элементами:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>
    <FlyoutItem Title="Cats"
                Icon="cat.png">
        <Tab>
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
    <FlyoutItem Title="Dogs"
                Icon="dog.png">
        <Tab>
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
</Shell>
```

В этом примере каждый [`ContentPage`](xref:Xamarin.Forms.ContentPage) доступен только через элементы всплывающего меню:

[![Снимок экрана двухстраничного приложения оболочки с элементами всплывающего меню для iOS и Android](flyout-images/two-page-app-flyout.png "Двухстраничное приложение оболочки с элементами всплывающего меню")](flyout-images/two-page-app-flyout-large.png#lightbox "Двухстраничное приложение оболочки с элементами всплывающего меню")

> [!NOTE]
> Если заголовок всплывающего меню отсутствует, элементы всплывающего меню отображаются от самого верха всплывающего меню. В противном случае они отображаются под заголовком всплывающего меню.

Оболочка содержит операторы неявного преобразования, которые позволяют упростить визуальную иерархию оболочки без добавления новых представлений в визуальное дерево. Это возможно, так как производный объект `Shell` может содержать только объекты `FlyoutItem` или объект `TabBar`, которые могут содержать только объекты `Tab`, которые, в свою очередь, могут содержать только объекты `ShellContent`. Эти операторы неявного преобразования позволяют удалить из предыдущего примера объекты `FlyoutItem`, `Tab` и `ShellContent`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>
    <views:CatsPage IconImageSource="cat.png" />
    <views:DogsPage IconImageSource="dog.png" />
</Shell>
```

Это неявное преобразование автоматически заключает каждый объект [`ContentPage`](xref:Xamarin.Forms.ContentPage) в объекты `ShellContent`, которые заключаются в объекты `Tab`, которые заключаются в объекты `FlyoutItem`.

> [!IMPORTANT]
> В приложении оболочки каждый [`ContentPage`](xref:Xamarin.Forms.ContentPage), который является дочерним элементом объекта `ShellContent`, создается во время запуска приложения. Такой метод добавления объектов `ShellContent` приводит к созданию дополнительных страниц во время запуска приложения, что может замедлять запуск. Но оболочка может создавать страницы и по требованию, реагируя на переходы. Дополнительные сведения см. в разделе об [эффективности загрузки страниц](tabs.md#efficient-page-loading) в руководстве по [вкладкам оболочки Xamarin.Forms](tabs.md).

### <a name="flyoutitem-class"></a>Класс FlyoutItem

Класс `FlyoutItem` включает следующие свойства, которые определяют внешний вид и поведение всплывающего меню:

- `FlyoutDisplayOptions` с типом `FlyoutDisplayOptions` определяет, как этот элемент и его дочерние элементы отображаются во всплывающем меню. Значение по умолчанию — `AsSingleItem`.
- `CurrentItem` с типом `Tab` обозначает выбранный элемент.
- `Items` с типом `IList<Tab>` определяет все вкладки в `FlyoutItem`.
- `FlyoutIcon` с типом `ImageSource` — значок, который используется для элемента. Если это свойство не установлено, по умолчанию ему присваивается значение свойства `Icon`.
- `Icon` с типом `ImageSource` определяет значок, который отображается в частях хрома, не относящихся к всплывающему меню.
- `IsChecked` с типом `boolean` определяет, выделен ли этот элемент во всплывающем элементе в настоящий момент.
- `IsEnabled` с типом `boolean` определяет, можно ли выбрать элемент в хроме.
- `IsTabStop` с типом `bool` указывает, включается ли `FlyoutItem` в навигацию по клавише TAB. Значение по умолчанию — `true`, а если оно равно `false`, элемент `FlyoutItem` игнорируется инфраструктурой навигации по клавише TAB, независимо от значения `TabIndex`.
- `TabIndex` с типом `int` обозначает порядок, в котором объекты `FlyoutItem` получают фокус при переходе пользователя между элементами посредством нажатия клавиши TAB. Это свойство по умолчанию имеет значение 0.
- `Title` с типом `string` определяет заголовок, отображаемый на вкладке в пользовательском интерфейсе.
- `Route` с типом `string` — строка, используемая для адресации элемента.

Все эти свойства, кроме свойства `Route`, поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязки данных.

> [!NOTE]
> Все объекты `FlyoutItem` в подклассах объекта Shell добавляются в коллекцию `Shell.Items`, которая определяет список элементов, отображаемых во всплывающем меню.

Кроме того, класс `FlyoutItem` предоставляет следующие переопределяемые методы:

- `OnTabIndexPropertyChanged` вызывается при изменении свойства `TabIndex`;
- `OnTabStopPropertyChanged` вызывается при изменении свойства `IsTabStop`;
- `TabIndexDefaultValueCreator` возвращает `int` и позволяет задать значение по умолчанию для свойства `TabIndex`;
- `TabStopDefaultValueCreator` возвращает `bool` и позволяет задать значение по умолчанию для свойства `TabStop`.

## <a name="flyout-vertical-scroll"></a>Вертикальная прокрутка всплывающего меню

По умолчанию всплывающее меню можно прокручивать по вертикали, если элементы не помещаются в нем. Это поведение можно изменить, задав в привязываемом свойстве `Shell.FlyoutVerticalScrollMode` один из элементов перечисления `ScrollMode`:

- `Disabled` — указывает, что вертикальная прокрутка будет отключена.
- `Enabled` — указывает, что вертикальная прокрутка будет включена.
- `Auto` — указывает, что вертикальная прокрутка будет включена, если элементы не помещаются во всплывающем меню. Это значение по умолчанию для свойства `Shell.FlyoutVerticalScrollMode`.

В следующем примере показано, как отключить вертикальную прокрутку.

```xaml
<Shell ...
       FlyoutVerticalScrollMode="Disabled"
    ...
</Shell>
```

## <a name="flyout-display-options"></a>Параметры отображения всплывающего меню

Перечисление `FlyoutDisplayOptions` определяет следующие члены:

- `AsSingleItem` указывает, будет ли элемент отображаться как один элемент;
- `AsMultipleItems` указывает, что сам элемент и его дочерние элементы будут отображаться во всплывающем меню как группа элементов.

Если вы установите для свойства `FlyoutItem.FlyoutDisplayOptions` значение `AsMultipleItems`, будет создан элемент всплывающего меню для каждого `Tab` в `FlyoutItem`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       FlyoutHeaderBehavior="CollapseOnScroll"
       x:Class="Xaminals.AppShell">

    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>

    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
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

В этом примере элементы всплывающего меню создаются для объекта `Tab`, производного от объекта `FlyoutItem`, и объектов `ShellContent`, производных от объекта `FlyoutItem`. Это связано с тем, что каждый объект `ShellContent`, производный от объекта `FlyoutItem`, автоматически упаковывается в объект `Tab`. Кроме того, элемент всплывающего меню создается для последнего объекта `ShellContent`, который упаковывается в объект `Tab`, а затем в объект `FlyoutItem`.

Все это создает такие элементы всплывающего меню:

[![Снимок экрана всплывающего меню с объектами FlyoutItem для iOS и Android](flyout-images/flyout-reduced.png "Всплывающее меню оболочки с объектами FlyoutItem")](flyout-images/flyout-reduced-large.png#lightbox "Всплывающее меню оболочки с объектами FlyoutItem")

## <a name="define-flyoutitem-appearance"></a>Определение внешнего вида FlyoutItem

Внешний вид каждого объекта `FlyoutItem` можно настроить, присвоив присоединенному свойству `Shell.ItemTemplate` значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<Shell ...>
    ...
    <Shell.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.8*" />
                </Grid.ColumnDefinitions>
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

Этот пример отображает заголовок каждого объекта `FlyoutItem` курсивом:

[![Снимок экрана шаблонных объектов FlyoutItem для iOS и Android](flyout-images/flyoutitem-templated.png "Шаблонные объекты FlyoutItem оболочки")](flyout-images/flyoutitem-templated-large.png#lightbox "Шаблонные объекты FlyoutItem оболочки")

Так как `Shell.ItemTemplate` является присоединенным свойством, для отдельных объектов `FlyoutItem` можно задавать разные шаблоны.

> [!NOTE]
> Оболочка предоставляет свойства `Title` и `FlyoutIcon` для [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) в `ItemTemplate`.

Кроме того, оболочка содержит три класса стилей, которые автоматически применяются к объектам `FlyoutItem`. Дополнительные сведения см. в разделе [Классы стилей FlyoutItem и MenuItem](#flyoutitem-and-menuitem-style-classes).

### <a name="default-template-for-flyoutitems"></a>Шаблон по умолчанию для элементов FlyoutItem

Ниже показан [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) по умолчанию, используемый для каждого элемента `FlyoutItem`.

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
> Этот же шаблон можно использовать для объектов `MenuItem`.

## <a name="flyoutitem-tab-order"></a>Последовательность табуляции для FlyoutItem

По умолчанию последовательность табуляции для объектов `FlyoutItem` соответствует порядку, в котором они перечислены в XAML или программно добавлены в дочернюю коллекцию. Этот порядок определяет порядок навигации по элементам `FlyoutItem` с клавиатуры, и часто порядок по умолчанию является оптимальным.

Вы можете изменить установленную по умолчанию последовательность табуляции, установив свойство `FlyoutItem.TabIndex`. Оно обозначает порядок, в котором объекты `FlyoutItem` будут получать фокус при переходе пользователя между элементами путем нажатия клавиши TAB. По умолчанию свойство равно 0, при этом оно может принимать любое значение `int`.

Следующие правила применяются при использовании последовательности табуляции по умолчанию или задании свойства `TabIndex`:

- Объекты `FlyoutItem` со значением 0 для свойства `TabIndex` добавляются в последовательность табуляции в том порядке, в котором они объявлены в XAML или дочерних коллекциях.
- Объекты `FlyoutItem` со значением `TabIndex` больше 0 добавляются в последовательность табуляции с учетом значений `TabIndex`.
- Объекты `FlyoutItem` со значением `TabIndex` меньше 0 добавляются в последовательность табуляции и отображаются раньше, чем любой объект с нулевым значением.
- Конфликты для `TabIndex`, устраняемые в порядке объявления.

Когда последовательность табуляции определена, нажатие клавиши TAB переключает фокус между объектами `FlyoutItem` в порядке возрастания значений `TabIndex`, а после достижения конечного элемента управления возвращает фокус в начало.

Кроме настройки последовательности табуляции для объектов `FlyoutItem`, может потребоваться исключить из этой последовательности некоторые объекты. Это можно сделать с помощью свойства `FlyoutItem.IsTabStop`, которое указывает, включается ли `FlyoutItem` в навигацию по клавише TAB. Значение по умолчанию — `true`, а если оно равно `false`, элемент `FlyoutItem` игнорируется инфраструктурой навигации по клавише TAB, независимо от значения `TabIndex`.

## <a name="set-the-current-flyoutitem"></a>Настройка текущего FlyoutItem

Класс `Shell` имеет привязываемое свойство с именем `CurrentItem` и типом `FlyoutItem`, которое представляет выбранный в данный момент `FlyoutItem`. При первом запуске приложения оболочки это свойство принимает значение первого `FlyoutItem` в производном объекте `Shell`. Но этому свойству можно присвоить значение другого `FlyoutItem`, как показано в следующем примере:

```xaml
<Shell ...
       CurrentItem="{x:Reference aboutItem}">
    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        ...
    </FlyoutItem>
    <ShellContent x:Name="aboutItem"
                  Title="About"
                  Icon="info.png"
                  ContentTemplate="{DataTemplate views:AboutPage}" />
</Shell>
```

Этот код задает объект `ShellContent` с именем `aboutItem` в качестве значения свойства `CurrentItem`, что приводит к его отображению. В нашем примере используется неявное преобразование для помещения объекта `ShellContent` в объект `Tab`, который упаковывается в объект `FlyoutItem`.

С учетом объекта `ShellContent` с именем `aboutItem` эквивалентный код на C# выглядит так:

```csharp
CurrentItem = aboutItem;
```

В этом примере свойство `CurrentItem` задается в подклассе класса `Shell`. Кроме того, свойство `CurrentItem` может быть задано в любом классе с помощью статического свойства `Shell.Current`:

```csharp
Shell.Current.CurrentItem = aboutItem;
```

## <a name="menu-items"></a>Пункты меню

Во всплывающее меню можно при необходимости добавлять пункты меню, каждый из которых представлен объектом [`MenuItem`](xref:Xamarin.Forms.MenuItem). Положение объектов `MenuItem` во всплывающем меню определяется порядком их объявления в визуальной иерархии оболочки. Таким образом, любые объекты `MenuItem`, объявленные перед объектами `FlyoutItem`, отображаются в верхней части всплывающего меню, а любые объекты `MenuItem`, объявленные после объектов `FlyoutItem`, — в его нижней части.

> [!NOTE]
> Класс `MenuItem` имеет событие [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked) и свойство [`Command`](xref:Xamarin.Forms.MenuItem.Command). Это означает, что объекты `MenuItem` позволяют выполнять действия в ответ на касание `MenuItem`. Сюда относятся такие сценарии, как навигация и открытие веб-браузера на определенной веб-странице.

Объекты [`MenuItem`](xref:Xamarin.Forms.MenuItem) можно добавить в всплывающее окно, как показано в следующем примере:

```xaml
<Shell ...>
    ...            
    <MenuItem Text="Random"
              IconImageSource="random.png"
              Command="{Binding RandomPageCommand}" />
    <MenuItem Text="Help"
              IconImageSource="help.png"
              Command="{Binding HelpCommand}"
              CommandParameter="https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell" />    
</Shell>
```

Этот код добавляет во всплывающее меню два объекта [`MenuItem`](xref:Xamarin.Forms.MenuItem) под всеми его пунктами:

[![Снимок экрана всплывающего меню с объектами MenuItem для iOS и Android](flyout-images/flyout.png "Всплывающее меню оболочки с объектами MenuItem")](flyout-images/flyout-large.png#lightbox "Всплывающее меню оболочки с объектами MenuItem")

Первый объект [`MenuItem`](xref:Xamarin.Forms.MenuItem) выполняет `ICommand` с именем `RandomPageCommand`, который ведет на случайную страницу в приложении. Второй объект `MenuItem` выполняет `ICommand` с именем `HelpCommand`, который открывает в веб-браузере URL-адрес, заданный свойством `CommandParameter`.

> [!NOTE]
> [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) для каждого элемента `MenuItem` наследуется от производного объекта `Shell`.

## <a name="define-menuitem-appearance"></a>Определение внешнего вида MenuItem

Внешний вид каждого объекта `MenuItem` можно настроить, присвоив присоединенному свойству `Shell.MenuItemTemplate` значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<Shell ...>
    <Shell.MenuItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.8*" />
                </Grid.ColumnDefinitions>
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
    <MenuItem Text="Random"
              IconImageSource="random.png"
              Command="{Binding RandomPageCommand}" />
    <MenuItem Text="Help"
              IconImageSource="help.png"
              Command="{Binding HelpCommand}"
              CommandParameter="https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell" />  
</Shell>
```

В этом примере `MenuItemTemplate` на уровне оболочки задается для каждого объекта `MenuItem`, чтобы заголовок каждого объекта `MenuItem` отображался курсивом:

[![Снимок экрана шаблонных объектов MenuItem для iOS и Android](flyout-images/menuitem-templated.png "Шаблонные объекты MenuItem оболочки")](flyout-images/menuitem-templated-large.png#lightbox "Шаблонные объекты MenuItem оболочки")

> [!NOTE]
> Оболочка предоставляет свойства [`Text`](xref:Xamarin.Forms.MenuItem.Text) и [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource) для [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) в `MenuItemTemplate`. Можно также использовать `Title` вместо `Text` и `Icon` вместо `IconImageSource`, что позволяет повторно использовать один и тот же шаблон для пунктов меню и пунктов всплывающего элемента.

Так как `Shell.MenuItemTemplate` является присоединенным свойством, для отдельных объектов `MenuItem` можно задавать разные шаблоны:

```xaml
<Shell ...>
    <Shell.MenuItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.8*" />
                </Grid.ColumnDefinitions>
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
    <MenuItem Text="Random"
              IconImageSource="random.png"
              Command="{Binding RandomPageCommand}" />
    <MenuItem Text="Help"
              Icon="help.png"
              Command="{Binding HelpCommand}"
              CommandParameter="https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell">
        <Shell.MenuItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </Shell.MenuItemTemplate>
    </MenuItem>
</Shell>
```

В этом примере `MenuItemTemplate` на уровне оболочки задается для первого объекта `MenuItem`, а для второго объекта `MenuItem` задается встроенный шаблон `MenuItemTemplate`.

> [!NOTE]
> Шаблон по умолчанию для объектов `FlyoutItem` можно также использовать для объектов `MenuItem`. Дополнительные сведения см. в разделе [Шаблон по умолчанию для элементов FlyoutItem](#default-template-for-flyoutitems).

## <a name="flyoutitem-and-menuitem-style-classes"></a>Классы стилей FlyoutItem и MenuItem

Оболочка включает три класса стилей, которые автоматически применяются к объектам `FlyoutItem` и `MenuItem`. Классам стилей заданы следующие имена:

- `FlyoutItemLabelStyle`
- `FlyoutItemImageStyle`
- `FlyoutItemLayoutStyle`

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

Эти стили будут автоматически применены к объектам `FlyoutItem` и `MenuItem`, причем задавать их свойствам [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass) имена классов стилей не требуется.

Кроме того, для объектов `FlyoutItem` и `MenuItem` можно определить и применить пользовательские классы стилей. Дополнительные сведения о классах стилей см. в статье [Классы стилей Xamarin.Forms](~/xamarin-forms/user-interface/styles/xaml/style-class.md).

## <a name="related-links"></a>Связанные ссылки

- [Xaminals (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Классы стилей Xamarin.Forms](~/xamarin-forms/user-interface/styles/xaml/style-class.md)
- [Диспетчер визуального представления состояний Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md)
