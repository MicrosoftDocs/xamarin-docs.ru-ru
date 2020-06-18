---
title: 'title: "Xamarin.Forms — конфигурация страницы оболочки" description: "Класс Shell определяет присоединенные свойства, с помощью которых можно настраивать внешний вид страниц в приложениях оболочки Xamarin.Forms.'
description: 'Сюда относится настройка цветов страницы, отключение панели навигации, отключение панели вкладок и отображение представлений на панели навигации". ms.prod: xamarin ms.assetid: 3FC2FBD1-C30B-4408-97B2-B04E3A2E4F03 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 01/29/2020 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: 3FC2FBD1-C30B-4408-97B2-B04E3A2E4F03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 102070fc478b42e9fbc0c7d0006197c81a49c9b8
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/10/2020
ms.locfileid: "84137505"
---
# <a name="xamarinforms-shell-page-configuration"></a>Конфигурация страницы оболочки Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Класс `Shell` определяет присоединенные свойства, с помощью которых можно настраивать внешний вид страниц в приложениях оболочки Xamarin.Forms. Сюда относится настройка цветов страниц, установка режима презентации страницы, отключение панели навигации, отключение панели вкладок и отображение представлений на панели навигации.

## <a name="set-page-colors"></a>Настройка цветов страницы

Класс `Shell` определяет следующие присоединенные свойства, с помощью которых можно настраивать цвета страницы в приложении оболочки:

- `BackgroundColor` с типом `Color` определяет цвет фона для хрома оболочки. Этот цвет не применяется под содержимым оболочки.
- `DisabledColor` с типом `Color` определяет цвет затененного текста и отключенных значков.
- `ForegroundColor` с типом `Color` определяет цвет затененного текста и значков.
- `TitleColor` с типом `Color` определяет цвет заголовка активной страницы.
- `UnselectedColor` с типом `Color` определяет цвет невыделенного текста и значков для хрома оболочки.

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязок данных, а также оформлять их, используя стили XAML. Кроме того, эти свойства можно задавать с помощью каскадных таблиц стилей (CSS). Подробные сведения см. в разделе [Особые свойства оболочки Xamarin.Forms](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties).

> [!NOTE]
> Есть также свойства, которые позволяют определить цвета вкладки. Дополнительные сведения см. в разделе [Внешний вид вкладок](tabs.md#tab-appearance).

В следующем примере XAML показано, как задать свойства цвета в производном классе `Shell`:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
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

По умолчанию происходит анимация навигации при переходе на страницу с помощью метода `GoToAsync`. Но вы можете изменить это поведение, задав в присоединенном свойстве `Shell.PresentationMode` для [`ContentPage`](xref:Xamarin.Forms.ContentPage) один из элементов перечисления `PresentationMode`:

- `NotAnimated` указывает, что страница будет отображаться без анимации навигации.
- `Animated` указывает, что страница будет отображаться с анимацией навигации. Это значение по умолчанию для присоединенного свойства `Shell.PresentationMode`.
- `Modal` указывает, что страница будет отображаться в виде модальной страницы.
- `ModalAnimated` указывает, что страница будет отображаться в виде модальной страницы с анимацией навигации.
- `ModalNotAnimated` указывает, что страница будет отображаться в виде модальной страницы без анимации навигации.

> [!IMPORTANT]
> Тип `PresentationMode` является перечислением флагов. Это означает, что сочетание элементов перечисления может быть применено в коде. Но для простоты использования в XAML элемент `ModalAnimated` является сочетанием элементов `Animated` и `Modal`, а элемент `ModalNotAnimated` — сочетанием `NotAnimated` и `Modal`. Дополнительные сведения о перечислении флагов см. в [этом разделе](/dotnet/csharp/language-reference/builtin-types/enum#enumeration-types-as-bit-flags).

Следующий пример XAML задает присоединенное свойство `Shell.PresentationMode` для [`ContentPage`](xref:Xamarin.Forms.ContentPage):

```xaml
<ContentPage ...
             Shell.PresentationMode="Modal">
    ...             
</ContentPage>
```

В этом примере [`ContentPage`](xref:Xamarin.Forms.ContentPage) задается как модальная страница при переходе на нее с помощью метода `GoToAsync`.

## <a name="enable-navigation-bar-shadow"></a>Включение тени панели навигации

Класс `Shell` определяет присоединенное свойство `NavBarHasShadow` с типом `bool`, которое определяет, будет ли панель навигации иметь тень. По умолчанию для этого свойства задано значение `false` в iOS и значение `true` в Android.

Хотя это свойство можно задать в производном объекте `Shell`, оно обычно устанавливается на любой странице, на которой нужно включить тень панели навигации. Так, в следующем примере XAML показано, как включить тень панели навигации из [`ContentPage`](xref:Xamarin.Forms.ContentPage):

```xaml
<ContentPage ...
             Shell.NavBarHasShadow="true">
    ...
</ContentPage>
```

Это приведет к включению тени панели навигации.

## <a name="disable-the-navigation-bar"></a>Отключение панели навигации

Класс `Shell` определяет присоединенное свойство `NavBarIsVisible` с типом `bool`, которое определяет, будет ли видна панель навигации при отображении страницы. По умолчанию этому свойству задано значение `true`.

Хотя это свойство можно задать в производном объекте `Shell`, оно обычно устанавливается на любой странице, на которой нужно скрыть панель навигации. Так, в следующем примере XAML показано, как отключить панель навигации из [`ContentPage`](xref:Xamarin.Forms.ContentPage):

```xaml
<ContentPage ...
             Shell.NavBarIsVisible="false">
    ...
</ContentPage>
```

В результате панель навигации становится скрытой, когда отображается страница:

![Снимок экрана со страницей приложения оболочки для iOS и Android, на которой скрыта панель навигации](configuration-images/navigationbar-invisible.png "Страница приложения оболочки со скрытой панелью навигации")

## <a name="disable-the-tab-bar"></a>Отключение панели вкладок

Класс `Shell` определяет присоединенное свойство `TabBarIsVisible` с типом `bool`, которое определяет, будет ли видна панель вкладок при отображении страницы. По умолчанию этому свойству задано значение `true`.

Хотя это свойство можно задать в производном объекте `Shell`, оно обычно устанавливается на любой странице, на которой нужно скрыть панель вкладок. Так, в следующем примере XAML показано, как отключить панель вкладок из [`ContentPage`](xref:Xamarin.Forms.ContentPage):

```xaml
<ContentPage ...
             Shell.TabBarIsVisible="false">
    ...
</ContentPage>
```

В результате панель вкладок становится скрытой, когда отображается страница:

![Снимок экрана со страницей приложения оболочки для iOS и Android, на которой скрыта панель вкладок](configuration-images/tabbar-invisible.png "Страница приложения оболочки со скрытой панелью вкладок")

## <a name="display-views-in-the-navigation-bar"></a>Отображение представлений на панели навигации

Класс `Shell` определяет присоединенное свойство `TitleView` с типом `View` для отображения на панели навигации всех [`View`](xref:Xamarin.Forms.View) Xamarin.Forms.

Хотя это свойство можно задать в производном объекте `Shell`, оно обычно устанавливается на любой странице, на которой нужно отобразить представление на панели навигации. Так, в следующем примере XAML показано, как отобразить [`Image`](xref:Xamarin.Forms.Image) на панели навигации для [`ContentPage`](xref:Xamarin.Forms.ContentPage):

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

![Снимок экрана со страницей приложения оболочки для iOS и Android с представлением названия](configuration-images/titleview.png "Страница приложения оболочки с представлением названия")

> [!IMPORTANT]
> Если панель навигации скрыта с помощью присоединенного свойства `NavBarIsVisible`, представление названия не будет отображаться.

Представления не будут отображаться на панели навигации, если не указан размер представления с помощью свойств [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) и [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) или не указано расположение представления с помощью свойств [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) и [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions).

Так как класс [`Layout`](xref:Xamarin.Forms.Layout) является производным от класса [`View`](xref:Xamarin.Forms.View), присоединенное свойство `TitleView` можно настроить для отображения класса макета, содержащего несколько представлений. Аналогичным образом, так как класс [`ContentView`](xref:Xamarin.Forms.ContentView) является итоговым производным от класса [`View`](xref:Xamarin.Forms.View), присоединенное свойство `TitleView` можно настроить для отображения `ContentView` с единым представлением.

## <a name="page-visibility"></a>Видимость страницы

Оболочка учитывает видимость страницы, которая задается с помощью свойства [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible). Поэтому, если у свойства `IsVisible` страницы значение `false`, она не будет отображаться в приложении оболочки и на нее невозможно будет перейти.

## <a name="related-links"></a>Связанные ссылки

- [Xaminals (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Задание стиля приложений Xamarin.Forms с помощью стилей XAML](~/xamarin-forms/user-interface/styles/xaml/index.md)
- [Особые свойства каскадных таблиц стилей оболочки Xamarin.Forms](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)
