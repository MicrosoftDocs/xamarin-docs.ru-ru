---
title: Xamarin.Forms Pages
description: Xamarin.FormsСтраницы представляют собой кросс-платформенные экраны мобильных приложений. В этой статье перечислены страницы, входящие в Xamarin.Forms .
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c576186dcfd598cb4fcfecd6d36edf04f73eee64
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84132825"
---
# <a name="xamarinforms-pages"></a>Xamarin.Forms Pages

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Страницы Xamarin. Forms представляют собой межплатформенные экраны мобильных приложений._

Все типы страниц, описанные ниже, являются производными от Xamarin.Forms [`Page`](xref:Xamarin.Forms.Page) класса. Эти визуальные элементы занимают весь экран или большую часть экрана. `Page`Объект представляет в `ViewController` iOS и `Page` в универсальная платформа Windows. На Android каждая страница занимает экран `Activity` , как, но Xamarin.Forms страницы *не* являются `Activity` объектами.

[![](pages-images/pages-sml.png "Xamarin.Forms Page Types")](pages-images/pages.png#lightbox "Xamarin.Forms Page Types")

## <a name="pages"></a>Страницы

Xamarin.Formsподдерживает следующие типы страниц:

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     |
| --- | --- |
| [`ContentPage`](xref:Xamarin.Forms.ContentPage)— Это самый простой и наиболее распространенный тип страницы. Задайте [`Content`](xref:Xamarin.Forms.ContentPage.Content) для свойства один [`View`](views.md) объект, который чаще всего [`Layout`](layouts.md) такой, как [`StackLayout`](layouts.md#stackLayout) , [`Grid`](layouts.md#grid) или [`ScrollView`](layouts.md#scrollView) .<br /><br />[Документация по API-интерфейсам](xref:Xamarin.Forms.ContentPage) | [![Пример ContentPage](pages-images/ContentPage.png "Пример ContentPage")](pages-images/ContentPage-Large.png#lightbox "Пример ContentPage")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     |
| --- | --- |
| А [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) управляет двумя областями информации. Задайте [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) для свойства страницу, обычно отображающую список или меню. Задайте [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) для свойства страницу, показывающую выбранный элемент на главной странице. [`IsPresented`](xref:Xamarin.Forms.MasterDetailPage.IsPresented)Свойство определяет, является ли страница «основной» или «подробности» видимой.<br /><br />[Документация по API](xref:Xamarin.Forms.MasterDetailPage)  /  [Руководством](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-masterdetailpage) | [![Пример Мастердетаилпаже](pages-images/MasterDetailPage.png "Пример Мастердетаилпаже")](pages-images/MasterDetailPage-Large.png#lightbox "Пример Мастердетаилпаже")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) с [кодом программной части](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     |
| --- | --- |
| [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)Управляет навигацией между другими страницами с помощью архитектуры на основе стека. При использовании навигации по страницам в приложении экземпляр домашней страницы должен быть передан конструктору `NavigationPage` объекта.<br /><br />[Документация по API](xref:Xamarin.Forms.NavigationPage)  /  [Руководством](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)  /  [Примеры 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-hierarchical), [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-passingdata)и [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-loginflow)  | [![Пример Навигатионпаже](pages-images/NavigationPage.png "Пример Навигатионпаже")](pages-images/NavigationPage-Large.png#lightbox "Пример Навигатионпаже")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) с [кодом = Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     |
| --- | --- |
| [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)является производным от абстрактного [`MultiPage`](xref:Xamarin.Forms.MultiPage`1) класса и позволяет перемещаться между дочерними страницами с помощью вкладок. Присвойте [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) свойству коллекцию страниц или задайте для [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) свойства коллекцию объектов данных, а [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) свойству — [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Описание способа представления каждого объекта визуально.<br /><br />[Документация по API](xref:Xamarin.Forms.TabbedPage)  /  [Руководством](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)  /  [Пример 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpage) и [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage) | [![Пример Таббедпаже](pages-images/TabbedPage.png "Пример TabbedPage")](pages-images/TabbedPage-Large.png#lightbox "Пример TabbedPage")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     |
| --- | --- |
| [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)является производным от абстрактного [`MultiPage`](xref:Xamarin.Forms.MultiPage`1) класса и позволяет перемещаться между дочерними страницами через прокрутку пальцами. Задайте [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) для свойства коллекцию [`ContentPage`](#contentPage) объектов или задайте для [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) свойства коллекцию объектов данных, а [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) свойству — [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Описание того, как каждый объект должен быть представлен визуально.<br /><br />[Документация по API](xref:Xamarin.Forms.CarouselPage)  /  [Руководством](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md)  /  [Пример 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpage) и [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpagetemplate) | [![Пример Карауселпаже](pages-images/CarouselPage.png "Пример Карауселпаже")](pages-images/CarouselPage-Large.png#lightbox "Пример Карауселпаже")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>темплатедпаже

|     |     |
| --- | --- |
| [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)Отображает содержимое в полноэкранном режиме с помощью шаблона элемента управления, а является базовым классом для [`ContentPage`](#contentPage) .<br /><br />[Документация по API](xref:Xamarin.Forms.TemplatedPage)  /  [Руководством](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![Пример Темплатедпаже](pages-images/TemplatedPage.png "Пример Темплатедпаже")](pages-images/TemplatedPage.png "Пример Темплатедпаже") |
|     |     |

## <a name="related-links"></a>Связанные ссылки

- [Xamarin.FormsПример Формсгаллери](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.FormsРегистрируют](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsДокументация по API](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
