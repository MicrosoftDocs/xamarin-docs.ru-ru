---
title: Xamarin.Forms Pages
description: Xamarin.Forms Страницы представляют собой кросс-платформенные экраны мобильных приложений. В этой статье перечислены страницы, входящие в Xamarin.Forms .
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 659b30b7eb09e06e6f87f3e6e507554f877fda85
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2021
ms.locfileid: "97940166"
---
# <a name="no-locxamarinforms-pages"></a>Xamarin.Forms Pages

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Xamarin.Forms Страницы представляют собой кросс-платформенные экраны мобильных приложений._

Все типы страниц, описанные ниже, являются производными от Xamarin.Forms [`Page`](xref:Xamarin.Forms.Page) класса. Эти визуальные элементы занимают весь экран или большую часть экрана. `Page`Объект представляет в `ViewController` iOS и `Page` в универсальная платформа Windows. На Android каждая страница занимает экран `Activity` , как, но Xamarin.Forms страницы *не* являются `Activity` объектами.

[![::: No-Loc (Xamarin. Forms)::: типы страниц](pages-images/pages-sml.png)](pages-images/pages.png#lightbox "::: No-Loc (Xamarin. Forms)::: типы страниц")

## <a name="pages"></a>Страницы

Xamarin.Forms поддерживает следующие типы страниц:

| Тип | Описание | Внешний вид |
| --- | --- | --- |
| `ContentPage` | [`ContentPage`](xref:Xamarin.Forms.ContentPage) — Это самый простой и наиболее распространенный тип страницы. Задайте [`Content`](xref:Xamarin.Forms.ContentPage.Content) для свойства один [`View`](views.md) объект, который чаще всего [`Layout`](layouts.md) такой, как [`StackLayout`](xref:Xamarin.Forms.StackLayout) , [`Grid`](xref:Xamarin.Forms.Grid) или [`ScrollView`](xref:Xamarin.Forms.ScrollView) .<br /><br />[Документация по API](xref:Xamarin.Forms.ContentPage) | [![Пример ContentPage](pages-images/ContentPage.png "Пример ContentPage")](pages-images/ContentPage-Large.png#lightbox "Пример ContentPage")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
| `FlyoutPage` | А [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) управляет двумя областями информации. Задайте [`Flyout`](xref:Xamarin.Forms.FlyoutPage.Flyout) для свойства страницу, обычно отображающую список или меню. Задайте [`Detail`](xref:Xamarin.Forms.FlyoutPage.Detail) для свойства страницу, отображающую выбранный элемент на всплывающей странице. [`IsPresented`](xref:Xamarin.Forms.FlyoutPage.IsPresented)Свойство определяет видимость всплывающего окна или страницы сведений.<br /><br />[Документация по API](xref:Xamarin.Forms.FlyoutPage)  /  [Руководством](~/xamarin-forms/app-fundamentals/navigation/flyoutpage.md)  /  [Пример](/samples/xamarin/xamarin-forms-samples/navigation-flyoutpage) | [![Пример Флйоутпаже](pages-images/FlyoutPage.png "Пример Флйоутпаже")](pages-images/FlyoutPage-Large.png#lightbox "Пример Флйоутпаже")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlyoutPageDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlyoutPageDemoPage.xaml) с [кодом программной части](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlyoutPageDemoPage.xaml.cs) |
| `NavigationPage` | [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)Управляет навигацией между другими страницами с помощью архитектуры на основе стека. При использовании навигации по страницам в приложении экземпляр домашней страницы должен быть передан конструктору `NavigationPage` объекта.<br /><br />[Документация по API](xref:Xamarin.Forms.NavigationPage)  /  [Руководством](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)  /  [Примеры 1](/samples/xamarin/xamarin-forms-samples/navigation-hierarchical), [2](/samples/xamarin/xamarin-forms-samples/navigation-passingdata)и [3](/samples/xamarin/xamarin-forms-samples/navigation-loginflow)  | [![Пример Навигатионпаже](pages-images/NavigationPage.png "Пример Навигатионпаже")](pages-images/NavigationPage-Large.png#lightbox "Пример Навигатионпаже")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) с [кодом = Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) |
| `TabbedPage` | [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) является производным от абстрактного [`MultiPage`](xref:Xamarin.Forms.MultiPage`1) класса и позволяет перемещаться между дочерними страницами с помощью вкладок. Присвойте [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) свойству коллекцию страниц или задайте для [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) свойства коллекцию объектов данных, а [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) свойству — [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Описание способа представления каждого объекта визуально.<br /><br />[Документация по API](xref:Xamarin.Forms.TabbedPage)  /  [Руководством](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)  /  [Пример 1](/samples/xamarin/xamarin-forms-samples/navigation-tabbedpage) и [2](/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage) | [![Пример Таббедпаже](pages-images/TabbedPage.png "Пример TabbedPage")](pages-images/TabbedPage-Large.png#lightbox "Пример TabbedPage")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
| `CarouselPage` | [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) является производным от абстрактного [`MultiPage`](xref:Xamarin.Forms.MultiPage`1) класса и позволяет перемещаться между дочерними страницами через прокрутку пальцами. Задайте [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) для свойства коллекцию [`ContentPage`](xref:Xamarin.Forms.ContentPage) объектов или задайте для [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) свойства коллекцию объектов данных, а [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) свойству — [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Описание того, как каждый объект должен быть представлен визуально.<br /><br />[Документация по API](xref:Xamarin.Forms.CarouselPage)  /  [Руководством](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md)  /  [Пример 1](/samples/xamarin/xamarin-forms-samples/navigation-carouselpage) и [2](/samples/xamarin/xamarin-forms-samples/navigation-carouselpagetemplate) | [![Пример Карауселпаже](pages-images/CarouselPage.png "Пример Карауселпаже")](pages-images/CarouselPage-Large.png#lightbox "Пример Карауселпаже")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
| `TemplatedPage` | [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage) Отображает содержимое в полноэкранном режиме с помощью шаблона элемента управления, а является базовым классом для [`ContentPage`](xref:Xamarin.Forms.ContentPage) .<br /><br />[Документация по API](xref:Xamarin.Forms.TemplatedPage)  /  [Руководством](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![Пример Темплатедпаже](pages-images/TemplatedPage.png "Пример Темплатедпаже")](pages-images/TemplatedPage.png "Пример Темплатедпаже") |
|     |     |     |

## <a name="related-links"></a>Связанные ссылки

- [Xamarin.Forms Пример Формсгаллери](/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Примеры для Xamarin.Forms](/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms Документация по API](/dotnet/api/xamarin.forms?view=xamarin-forms)
