---
title: Сводка по главе 25. Виды страниц
description: Создание мобильных приложений с помощью Xamarin.Forms. Сводка по главе 25. Виды страниц
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D1D348F2-6A44-4781-ADCE-A0B7BB9AEF89
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e66fb50b8d537ee0267457d5b0ab0f417813e676
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136621"
---
# <a name="summary-of-chapter-25-page-varieties"></a>Сводка по главе 25. Виды страниц

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)

До сих пор вы видели два класса, производных от `Page`: `ContentPage` и `NavigationPage`. В этой главе представлены два других:

- [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) управляет двумя страницами — Главная и Детали
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) управляет несколькими дочерними страницами, доступными через вкладки

Эти типы страниц предоставляют более сложные параметры навигации, чем `NavagationPage`, обсуждаемые в [Главе 24. Переход по страницам](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter24.md).

## <a name="master-and-detail"></a>Главная и Детали

[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) определяет два свойства типа `Page`: [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) и [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail). Обычно каждому из этих свойств присваивается `ContentPage`. `MasterDetailPage` отображает эти две страницы и переключается между ними.

Существует два фундаментальных способа переключения между этими двумя страницами:

- *разбиение*, где главные и детальные данные находятся рядом друг с другом
- *всплывающее окно* — страница сведений частично перекрывает главную страницу.

Существует несколько разновидностей подхода для *всплывающих окон* (*слайд*, *перекрывающее* и *переключающее*), но обычно они зависят от платформы. Свойству [`MasterDetailBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) объекта `MasterDetailPage` можно присвоить значение одного из элементов перечисления [`MasterBehavior`](xref:Xamarin.Forms.MasterBehavior):

- [`Default`](xref:Xamarin.Forms.MasterBehavior.Default)
- [`Split`](xref:Xamarin.Forms.MasterBehavior.Split)
- [`SplitOnLandscape`](xref:Xamarin.Forms.MasterBehavior.SplitOnLandscape)
- [`SplitOnPortrait`](xref:Xamarin.Forms.MasterBehavior.SplitOnPortrait)
- [`Popover`](xref:Xamarin.Forms.MasterBehavior.Popover)

Однако это свойство не влияет на телефоны. Телефоны всегда имеют режимы работы с всплывающим окном. Режим работы с разделением имеют только планшеты и компьютеры (Windows).

### <a name="exploring-the-behaviors"></a>Изучение этих режимов работы

Образец [**MasterDetailBehaviors**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailBehaviors) позволяет экспериментировать с режимом по умолчанию на различных устройствах. Программа содержит две отдельные производные `ContentPage`, которые являются производными для основных и детальных данных (со свойством `Title`, установленным в обоих), и другим классом, который является производной от `MasterDetailPage`, объединяющего их. Страница деталей заключена в `NavigationPage`, так как программа UWP не будет работать без нее.

Для платформ Windows 8.1 и Windows Phone 8.1 требуется, чтобы растровое изображение было установлено в свойство `Icon` главной страницы.

### <a name="back-to-school"></a>Снова за учебу

Пример [**SchoolAndDetail**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/SchoolAndDetail) использует несколько другой подход к созданию программы для показа учащихся из библиотеки [**SchoolOfFineArt**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt).

Свойства `Master` и `Detail` определяются с помощью визуальных деревьев в файле [SchoolAndDetailPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml), который является производным от `MasterDetailPage`. Это расположение позволяет устанавливать привязки данных между главной страницей и страницами деталей.

Этот файл XAML также задает для свойства [`IsPresented`](xref:Xamarin.Forms.MasterDetailPage.IsPresented) из `MasterDetailPage` значение `True`. В результате главная страница будет отображаться при запуске; по умолчанию отображается страница деталей. Файл [SchoolAndDetailPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml.cs) задает значение `IsPresented` для `false` при выборе элемента из `ListView` на главной странице. Затем отобразится страница деталей:

[![Тройной снимок экрана школы и деталей](images/ch25fg09-small.png "Страница сведений из MasterDetailPage")](images/ch25fg09-large.png#lightbox "Страница сведений из MasterDetailPage")

### <a name="your-own-user-interface"></a>Собственный пользовательский интерфейс

Несмотря на то, что Xamarin.Forms предоставляет пользовательский интерфейс для переключения между представлениями "Главная" и "Детали", вы можете указать собственное представление. Для этого сделайте следующее:

- Задайте для свойства [`IsGestureEnabled`](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabled) значение `false`, чтобы отключить прокрутку.
- Переопределите метод [`ShouldShowToolbarButton`](xref:Xamarin.Forms.MasterDetailPage.ShouldShowToolbarButton) и возвратите `false`, чтобы скрыть кнопки на панели инструментов в Windows 8.1 и Windows Phone 8.1.

Затем необходимо предоставить средства для переключения между главной страницей и страницами деталей, например с помощью примера [**ColorsDetail**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/ColorsDetails).

В примере [**MasterDetailTaps**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailTaps) демонстрируется другой подход, использующий `TapGestureRecognizer` на главной странице и на страницах деталей.

## <a name="tabbedpage"></a>TabbedPage

[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) — это коллекция страниц, которые можно переключать с помощью вкладок. Она является производной от `MultiPage<Page>` и не определяет открытые свойства или методы самостоятельно. [`MultiPage<T>`](xref:Xamarin.Forms.MultiPage`1), однако, определяет свойство:

- свойство [`Children`](xref:Xamarin.Forms.MultiPage`1.Children)с типом `IList<T>`.

Эту коллекцию нужно заполнять `Children` объектами страницы.

Другой подход позволяет определять дочерние элементы `TabbedPage` так же, как и `ListView`, используя эти два свойства, которые автоматически создают страницы с вкладками:

- [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) типа `IEnumerable`
- [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) типа `DataTemplate`

Однако этот подход плохо работает в iOS, когда коллекция содержит более нескольких элементов.

`MultiPage<T>` определяет два дополнительные свойства, которые позволяют отслеживать текущую просматриваемую страницу:

- [`CurrentPage`](xref:Xamarin.Forms.MultiPage`1.CurrentPage) типа `T`, который ссылается на страницу
- [`SelectedItem`](xref:Xamarin.Forms.MultiPage`1.SelectedItem) типа `Object`, который ссылается на объект в коллекции `ItemsSource`

`MultiPage<T>` также определяет два события:

- [`PagesChanged`](xref:Xamarin.Forms.MultiPage`1.PagesChanged) при изменении коллекции `ItemsSource`
- [`CurrentPageChanged`](xref:Xamarin.Forms.MultiPage`1.CurrentPageChanged) при изменении просматриваемой страницы

### <a name="discrete-tab-pages"></a>Дискретные страницы вкладок

Пример [**DiscreteTabbedColors**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/DiscreteTabbedColors) состоит из трех страниц с вкладками, отображающие цвета тремя разными способами. Каждая вкладка является производной `ContentPage`, а затем производная `TabbedPage` [DiscreteTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColorsPage.xaml) объединяет три страницы.

Для каждой страницы, отображаемой в `TabbedPage`, свойство `Title` является обязательным для указания текста на вкладке, а в Apple Store требуется также использовать значок, поэтому для iOS установлено свойство `Icon`:

[![Тройной снимок экрана дискретных вкладок с разными цветами](images/ch25fg13-small.png "TabbedPage")](images/ch25fg13-large.png#lightbox "TabbedPage")

В примере [**StudentNotes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/StudentNotes) есть домашняя страница со списком всех учащихся. При касании учащийся переходит на производную `TabbedPage`, [`StudentNotesDataPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/StudentNotes/StudentNotes/StudentNotes/StudentNotesDataPage.xaml), которая включает три объекта `ContentPage` в своем визуальном дереве, один из которых позволяет вводить некоторые примечания для этого учащегося.

### <a name="using-an-itemtemplate"></a>Использование ItemTemplate

В примере [**MultiTabbedColor**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MultiTabbedColors) в библиотеке [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) используется класс [`NamedColor`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs). Файл [MultiTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/MultiTabbedColors/MultiTabbedColors/MultiTabbedColors/MultiTabbedColorsPage.xaml) устанавливает свойство `DataTemplate` от `TabbedPage` в визуальное дерево, начинающееся с `ContentPage`, которое содержит привязки к свойствам `NamedColor` (включая привязку к свойству `Title`).

Однако это проблематично для iOS. С помощью этого метода можно отобразить только несколько элементов, к тому же в нем отсутствует правильный способ для присвоения значков.

## <a name="related-links"></a>Связанные ссылки

- [Глава 25, полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch25-Apr2016.pdf)
- [Примеры для Главы 25](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)
- [Страница "Основной вид и подробности" Xamarin.Forms](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)
- [Страница с вкладками](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
