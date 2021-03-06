---
title: Страницы оболочки Xamarin.Forms
description: Класс Shell определяет присоединенные свойства, с помощью которых можно настраивать внешний вид страниц в приложениях оболочки Xamarin.Forms. Сюда относится настройка цветов страницы, отключение панели навигации, отключение панели вкладок и отображение представлений на панели навигации.
ms.prod: xamarin
ms.assetid: 3FC2FBD1-C30B-4408-97B2-B04E3A2E4F03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c008ffb6d58a86301f41fdaa54b23eb2d9869618
ms.sourcegitcommit: 1b542afc0f6f2f6adbced527ae47b9ac90eaa1de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101760233"
---
# <a name="xamarinforms-shell-pages"></a>Страницы оболочки Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Объект [`ShellContent`](xref:Xamarin.Forms.ShellContent) представляет объект [`ContentPage`](xref:Xamarin.Forms.ContentPage) для каждого [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) или [`Tab`](xref:Xamarin.Forms.Tab). Если `Tab` содержит более одного объекта `ShellContent`, перемещение по объектам `ContentPage` осуществляется с помощью верхней панели вкладок. На странице можно перемещаться по дополнительным объектам `ContentPage`, называемым страницами сведений.

Кроме того, класс [`Shell`](xref:Xamarin.Forms.Shell) определяет присоединенные свойства, с помощью которых можно настраивать внешний вид страниц в приложениях оболочки Xamarin.Forms. Сюда относится настройка цветов страниц, установка режима презентации страницы, отключение панели навигации, отключение панели вкладок и отображение представлений на панели навигации.

## <a name="display-pages"></a>Отображение страниц

В приложениях оболочки Xamarin.Forms страницы обычно создаются по запросу в ответ на навигацию. Это достигается с помощью расширения разметки [`DataTemplate`](xref:Xamarin.Forms.Xaml.DataTemplateExtension) для задания свойства [`ContentTemplate`](xref:Xamarin.Forms.ShellContent.ContentTemplate) каждого объекта [`ShellContent`](xref:Xamarin.Forms.ShellContent) в соответствии с объектом [`ContentPage`](xref:Xamarin.Forms.ContentPage):

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
       <ShellContent Title="Monkeys"
                     Icon="monkey.png"
                     ContentTemplate="{DataTemplate views:MonkeysPage}" />
    </TabBar>
</Shell>
```

В этом примере для удаления объектов [`Tab`](xref:Xamarin.Forms.Tab) из визуальной иерархии используются неявные операторы преобразования оболочки. Однако каждый объект [`ShellContent`](xref:Xamarin.Forms.ShellContent) отображается на вкладке:

[![Снимок экрана: приложение оболочки с тремя вкладками в iOS и Android](pages-images/three-pages.png)](pages-images/three-pages-large.png#lightbox)

> [!NOTE]
> [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) каждого объекта [`ShellContent`](xref:Xamarin.Forms.ShellContent) наследуется от родительского объекта [`Tab`](xref:Xamarin.Forms.Tab).

В каждом объекте [`ContentPage`](xref:Xamarin.Forms.ContentPage) можно перейти к дополнительным объектам `ContentPage`. Дополнительные сведения о навигации см. в разделе [Навигация по оболочке Xamarin.Forms](navigation.md).

## <a name="load-pages-at-application-startup"></a>Загрузка страниц при запуске приложения

В приложении оболочки каждый объект [`ContentPage`](xref:Xamarin.Forms.ContentPage) обычно создается по запросу в ответ на навигацию. Однако можно также создавать объекты `ContentPage` при запуске приложения.

> [!WARNING]
> Объекты [`ContentPage`](xref:Xamarin.Forms.ContentPage), создаваемые при запуске приложения, могут привести к неудачному запуску.

Объекты [`ContentPage`](xref:Xamarin.Forms.ContentPage) можно создавать при запуске приложения, присвоив свойства [`ShellContent.Content`](xref:Xamarin.Forms.ShellContent.Content) объектам `ContentPage`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
     <ShellContent Title="Cats"
                   Icon="cat.png">
         <views:CatsPage />
     </ShellContent>
     <ShellContent Title="Dogs"
                   Icon="dog.png">
         <views:DogsPage />
     </ShellContent>
     <ShellContent Title="Monkeys"
                   Icon="monkey.png">
         <views:MonkeysPage />
     </ShellContent>
    </TabBar>
</Shell>
```

В этом примере `CatsPage`, `DogsPage` и `MonkeysPage` создаются при запуске приложения, а не по запросу в ответ на навигацию.

> [!NOTE]
> [`Content`](xref:Xamarin.Forms.ShellContent.Content) — это свойство содержимого класса [`ShellContent`](xref:Xamarin.Forms.ShellContent). Поэтому его не нужно задавать явно.

## <a name="set-page-colors"></a>Настройка цветов страницы

Класс [`Shell`](xref:Xamarin.Forms.Shell) определяет следующие присоединенные свойства, с помощью которых можно настраивать цвета страницы в приложении оболочки:

- [`BackgroundColor`](xref:Xamarin.Forms.Shell.BackgroundColorProperty) с типом [`Color`](xref:Xamarin.Forms.Color) определяет цвет фона для хрома оболочки. Этот цвет не применяется под содержимым оболочки.
- [`DisabledColor`](xref:Xamarin.Forms.Shell.DisabledColorProperty) с типом [`Color`](xref:Xamarin.Forms.Color) определяет цвет затененного текста и отключенных значков.
- [`ForegroundColor`](xref:Xamarin.Forms.Shell.ForegroundColorProperty) с типом [`Color`](xref:Xamarin.Forms.Color) определяет цвет затененного текста и значков.
- [`TitleColor`](xref:Xamarin.Forms.Shell.TitleColorProperty) с типом [`Color`](xref:Xamarin.Forms.Color) определяет цвет заголовка активной страницы.
- [`UnselectedColor`](xref:Xamarin.Forms.Shell.UnselectedColorProperty) с типом [`Color`](xref:Xamarin.Forms.Color) определяет цвет невыделенного текста и значков для хрома оболочки.

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязок данных, а также оформлять их, используя стили XAML. Кроме того, эти свойства можно задавать с помощью каскадных таблиц стилей (CSS). Подробные сведения см. в разделе [Особые свойства оболочки Xamarin.Forms](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties).

> [!NOTE]
> Есть также свойства, которые позволяют определить цвета вкладки. Дополнительные сведения см. в разделе [Внешний вид вкладок](tabs.md#tab-appearance).

В следующем примере XAML показано, как задать свойства цвета в производном классе [`Shell`](xref:Xamarin.Forms.Shell):

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       x:Class="Xaminals.AppShell"
       BackgroundColor="#455A64"
       ForegroundColor="White"
       TitleColor="White"
       DisabledColor="#B4FFFFFF"
       UnselectedColor="#95FFFFFF">

</Shell>
```

В этом примере значения цвета применяются ко всем страницам в приложении оболочки, если это не переопределено на уровне страницы.

Так как свойства цвета являются присоединенными свойствами, их можно также задавать для отдельных страниц, чтобы соответствующим образом определять цвета:

```xaml
<ContentPage ...
             Shell.BackgroundColor="Gray"
             Shell.ForegroundColor="White"
             Shell.TitleColor="Blue"
             Shell.DisabledColor="#95FFFFFF"
             Shell.UnselectedColor="#B4FFFFFF">

</ContentPage>
```

Свойства цвета можно также задавать с помощью стиля XAML:

```xaml
<Style x:Key="DomesticShell"
       TargetType="Element" >
    <Setter Property="Shell.BackgroundColor"
            Value="#039BE6" />
    <Setter Property="Shell.ForegroundColor"
            Value="White" />
    <Setter Property="Shell.TitleColor"
            Value="White" />
    <Setter Property="Shell.DisabledColor"
            Value="#B4FFFFFF" />
    <Setter Property="Shell.UnselectedColor"
            Value="#95FFFFFF" />
</Style>
```

Дополнительные сведения о стилях XAML см. в руководстве по [оформлению приложений Xamarin.Forms с использованием стилей XAML](~/xamarin-forms/user-interface/styles/xaml/index.md).

## <a name="set-page-presentation-mode"></a>Установка режима презентации страницы

По умолчанию происходит анимация навигации при переходе на страницу с помощью метода [`GoToAsync`](xref:Xamarin.Forms.Shell.GoToAsync*). Но вы можете изменить это поведение, задав в присоединенном свойстве [`Shell.PresentationMode`](xref:Xamarin.Forms.Shell.PresentationModeProperty) для [`ContentPage`](xref:Xamarin.Forms.ContentPage) один из элементов перечисления [`PresentationMode`](xref:Xamarin.Forms.PresentationMode):

- `NotAnimated` указывает, что страница будет отображаться без анимации навигации.
- `Animated` указывает, что страница будет отображаться с анимацией навигации. Это значение по умолчанию для присоединенного свойства `Shell.PresentationMode`.
- `Modal` указывает, что страница будет отображаться в виде модальной страницы.
- `ModalAnimated` указывает, что страница будет отображаться в виде модальной страницы с анимацией навигации.
- `ModalNotAnimated` указывает, что страница будет отображаться в виде модальной страницы без анимации навигации.

> [!IMPORTANT]
> Тип `PresentationMode` является перечислением флагов. Это означает, что сочетание элементов перечисления может быть применено в коде. Но для простоты использования в XAML элемент `ModalAnimated` является сочетанием элементов `Animated` и `Modal`, а элемент `ModalNotAnimated` — сочетанием `NotAnimated` и `Modal`. Дополнительные сведения о перечислении флагов см. в [этом разделе](/dotnet/csharp/language-reference/builtin-types/enum#enumeration-types-as-bit-flags).

Следующий пример XAML задает присоединенное свойство [`Shell.PresentationMode`](xref:Xamarin.Forms.Shell.PresentationModeProperty) для [`ContentPage`](xref:Xamarin.Forms.ContentPage):

```xaml
<ContentPage ...
             Shell.PresentationMode="Modal">
    ...             
</ContentPage>
```

В этом примере [`ContentPage`](xref:Xamarin.Forms.ContentPage) задается как модальная страница при переходе на нее с помощью метода [`GoToAsync`](xref:Xamarin.Forms.Shell.GoToAsync*).

## <a name="enable-navigation-bar-shadow"></a>Включение тени панели навигации

Присоединенное свойство [`Shell.NavBarHasShadow`](xref:Xamarin.Forms.Shell.NavBarHasShadowProperty) с типом `bool` определяет, будет ли панель навигации иметь тень. По умолчанию для этого свойства задано значение `false` в iOS и значение `true` в Android.

Хотя это свойство можно задать в производном объекте [`Shell`](xref:Xamarin.Forms.Shell), оно обычно устанавливается на любой странице, на которой нужно включить тень панели навигации. Так, в следующем примере XAML показано, как включить тень панели навигации из [`ContentPage`](xref:Xamarin.Forms.ContentPage):

```xaml
<ContentPage ...
             Shell.NavBarHasShadow="true">
    ...
</ContentPage>
```

Это приведет к включению тени панели навигации.

## <a name="disable-the-navigation-bar"></a>Отключение панели навигации

Присоединенное свойство [`Shell.NavBarIsVisible`](xref:Xamarin.Forms.Shell.NavBarIsVisibleProperty) с типом `bool` определяет, будет ли видна панель навигации при отображении страницы. По умолчанию этому свойству задано значение `true`.

Хотя это свойство можно задать в производном объекте [`Shell`](xref:Xamarin.Forms.Shell), оно обычно устанавливается на любой странице, на которой нужно скрыть панель навигации. Так, в следующем примере XAML показано, как отключить панель навигации из [`ContentPage`](xref:Xamarin.Forms.ContentPage):

```xaml
<ContentPage ...
             Shell.NavBarIsVisible="false">
    ...
</ContentPage>
```

В результате панель навигации становится скрытой, когда отображается страница:

![Снимок экрана со страницей приложения оболочки для iOS и Android, на которой скрыта панель навигации](pages-images/navigationbar-invisible.png)

## <a name="display-views-in-the-navigation-bar"></a>Отображение представлений на панели навигации

Присоединенное свойство [`Shell.TitleView`](xref:Xamarin.Forms.Shell.TitleViewProperty) с типом [`View`](xref:Xamarin.Forms.View) служит для отображения на панели навигации всех `View`.

Хотя это свойство можно задать в производном объекте [`Shell`](xref:Xamarin.Forms.Shell), оно обычно устанавливается на любой странице, на которой нужно отобразить представление на панели навигации. Так, в следующем примере XAML показано, как отобразить [`Image`](xref:Xamarin.Forms.Image) на панели навигации для [`ContentPage`](xref:Xamarin.Forms.ContentPage):

```xaml
<ContentPage ...>
    <Shell.TitleView>
        <Image Source="xamarin_logo.png"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
    </Shell.TitleView>
    ...
</ContentPage>
```

В результате изображение отображается на панели навигации на странице:

![Снимок экрана со страницей приложения оболочки для iOS и Android с представлением названия](pages-images/titleview.png "Страница приложения оболочки с представлением названия")

> [!IMPORTANT]
> Если панель навигации скрыта с помощью присоединенного свойства [`NavBarIsVisible`](xref:Xamarin.Forms.Shell.NavBarIsVisibleProperty), представление названия не будет отображаться.

Представления не будут отображаться на панели навигации, если не указан размер представления с помощью свойств [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) и [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) или не указано расположение представления с помощью свойств [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) и [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions).

Так как класс [`Layout`](xref:Xamarin.Forms.Layout) является производным от класса [`View`](xref:Xamarin.Forms.View), присоединенное свойство [`TitleView`](xref:Xamarin.Forms.Shell.TitleViewProperty) можно настроить для отображения класса макета, содержащего несколько представлений. Аналогичным образом, так как класс [`ContentView`](xref:Xamarin.Forms.ContentView) является итоговым производным от класса [`View`](xref:Xamarin.Forms.View), присоединенное свойство `TitleView` можно настроить для отображения `ContentView` с единым представлением.

## <a name="page-visibility"></a>Видимость страницы

Оболочка учитывает видимость страницы, которая задается с помощью свойства [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible). Поэтому, если у свойства `IsVisible` страницы значение `false`, она не будет отображаться в приложении оболочки и на нее невозможно будет перейти.

## <a name="related-links"></a>Связанные ссылки

- [Xaminals (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Навигация по оболочке Xamarin.Forms](navigation.md)
- [Задание стиля приложений Xamarin.Forms с помощью стилей XAML](~/xamarin-forms/user-interface/styles/xaml/index.md)
- [Особые свойства каскадных таблиц стилей оболочки Xamarin.Forms](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)
