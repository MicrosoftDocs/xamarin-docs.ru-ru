---
title: Xamarin.FormsМетка
description: Xamarin.FormsМакеты используются для создания элементов управления пользовательского интерфейса в визуальных структурах. В этой статье перечислены макеты, входящие в Xamarin.Forms .
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c39bf29feceaf598ac8fd38e6af3d227b6deddc0
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137310"
---
# <a name="xamarinforms-layouts"></a>Xamarin.FormsМетка

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Макеты Xamarin.Forms используются для создания элементов управления пользовательского интерфейса в visual структуры._

[`Layout`](xref:Xamarin.Forms.Layout)Классы и [`Layout<T>`](xref:Xamarin.Forms.Layout`1) в Xamarin.Forms являются специализированными подтипами представлений, которые действуют как контейнеры для представлений и других макетов. `Layout`Сам класс является производным от [`View`](views.md) . `Layout`Производная обычно содержит логику для задания расположения и размера дочерних элементов в Xamarin.Forms приложениях.

[![Xamarin.FormsТипы макетов](layouts-images/layouts-sml.png "[! Операцион. NO-LOC (Xamarin. Forms)] типы макетов")](layouts-images/layouts.png#lightbox "[! Операцион. NO-LOC (Xamarin. Forms)] типы макетов")

Классы, производные от `Layout` можно разделить на две категории:

## <a name="layouts-with-single-content"></a>Макеты с одиночное содержимое

Эти классы являются производными от класса [`Layout`](xref:Xamarin.Forms.Layout) , который определяет [`Padding`](xref:Xamarin.Forms.Layout.Padding) [`IsClippedToBounds`](xref:Xamarin.Forms.Layout.IsClippedToBounds) Свойства и.

<a name="contentView" />

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](xref:Xamarin.Forms.ContentView)содержит один дочерний элемент, заданный [`Content`](xref:Xamarin.Forms.ContentView.Content) свойством. `Content` Свойство может устанавливаться к любому `View` производных продуктов, включая другие `Layout` производные от него. `ContentView`в основном используется в качестве структурного элемента и служит базовым классом для [`Frame`](#frame) .<br /><br />[Документация по API](xref:Xamarin.Forms.ContentView)  /  [Руководством](~/xamarin-forms/user-interface/layouts/contentview.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/) | [![Пример ContentView](layouts-images/ContentView.png "Пример ContentView")](layouts-images/ContentView-Large.png#lightbox "Пример ContentView")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) |
|     |     |

<a named="frame" />

### <a name="frame"></a>Frame

|     |     |
| --- | --- |
| [`Frame`](xref:Xamarin.Forms.Frame)Класс является производным от [`ContentView`](#contentView) и отображает границу (рамку) вокруг ее дочернего элемента. `Frame`Класс имеет значение по умолчанию [`Padding`](xref:Xamarin.Forms.Layout.Padding) 20, а также [`BorderColor`](xref:Xamarin.Forms.Frame.BorderColor) [`CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius) свойства, и [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow) .<br /><br />[Документация по API](xref:Xamarin.Forms.Frame)  /  [Руководством](~/xamarin-forms/user-interface/layouts/frame.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/) | [![Пример кадра](layouts-images/Frame.png "Пример кадра")](layouts-images/Frame-Large.png#lightbox "Пример кадра")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) |
|     |     |

<a name="scrollView" />

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](xref:Xamarin.Forms.ScrollView)может выполнять прокрутку содержимого. Задайте [`Content`](xref:Xamarin.Forms.ScrollView.Content) для свойства представление или макет, размер которого не умещается на экране. (Содержимое объекта `ScrollView` очень часто представляет собой [`StackLayout`](#stackLayout) .) Задайте [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) свойство, чтобы указать, должна ли прокрутка быть вертикальной, горизонтальной или обеих.<br /><br />[Документация по API](xref:Xamarin.Forms.ScrollView)  /  [Руководством](~/xamarin-forms/user-interface/layouts/scroll-view.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![Пример Скроллвиев](layouts-images/ScrollView.png "Пример Скроллвиев")](layouts-images/ScrollView-Large.png#lightbox "Пример Скроллвиев")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) |
|     |     |

### <a name="templatedview"></a>TemplatedView

|     |     |
| --- | --- |
| [`TemplatedView`](xref:Xamarin.Forms.TemplatedView)Отображает содержимое с помощью шаблона элемента управления, а является базовым классом для [`ContentView`](#contentView) .<br /><br />[Документация по API](xref:Xamarin.Forms.TemplatedView)  /  [Руководством](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![Пример Темплатедвиев](layouts-images/TemplatedView.png "Пример Темплатедвиев")](layouts-images/TemplatedView.png#lightbox "Пример Темплатедвиев") |
|     |     |

### <a name="contentpresenter"></a>ContentPresenter

|     |     |
| --- | --- |
| [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)— Это Диспетчер макетов для шаблонных представлений, который используется в, [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) чтобы отметить, где отображается содержимое, которое должно быть представлено.<br /><br />[Документация по API](xref:Xamarin.Forms.ContentPresenter)  /  [Руководством](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![Пример ContentPresenter](layouts-images/ContentPresenter.png "Пример ContentPresenter")](layouts-images/ContentPresenter.png#lightbox "Пример ContentPresenter") |
|     |     |

## <a name="layouts-with-multiple-children"></a>Макеты с нескольких дочерних элементов

Эти классы являются производными от [`Layout<View>`](xref:Xamarin.Forms.Layout`1) .

<a name="stackLayout" />

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](xref:Xamarin.Forms.StackLayout)размещает дочерние элементы в стеке по горизонтали или вертикали в зависимости от [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) Свойства. [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing)Свойство регулирует интервал между дочерними элементами и имеет значение по умолчанию 6.<br /><br />[Документация по API](xref:Xamarin.Forms.StackLayout)  /  [Руководством](~/xamarin-forms/user-interface/layouts/stacklayout.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)| [![Пример StackLayout](layouts-images/StackLayout.png "Пример StackLayout")](layouts-images/StackLayout-Large.png#lightbox "Пример StackLayout")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) |
|     |     |

<a name="grid" />

### <a name="grid"></a>Макет Grid

|     |     |
| --- | --- |
| [`Grid`](xref:Xamarin.Forms.Grid)размещает свои дочерние элементы в сетке строк и столбцов. Расположение дочернего элемента указывается с помощью [вложенных свойств](~/xamarin-forms/xaml/attached-properties.md) ,, [`Row`](xref:Xamarin.Forms.Grid.RowProperty) [`Column`](xref:Xamarin.Forms.Grid.ColumnProperty) [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) и [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) .<br /><br />[Документация по API](xref:Xamarin.Forms.Grid)  /  [Руководством](~/xamarin-forms/user-interface/layouts/grid.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![Пример сетки](layouts-images/Grid.png "Пример сетки")](layouts-images/Grid-Large.png#lightbox "Пример сетки")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) |
|     |     |

### <a name="absolutelayout"></a>AbsoluteLayout

|     |     |
| --- | --- |
| [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)Позиционирует дочерние элементы в конкретных расположениях относительно родительского элемента. Расположение дочернего элемента указывается с помощью [вложенных свойств](~/xamarin-forms/xaml/attached-properties.md) [`LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) и [`LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) . `AbsoluteLayout` Удобно для анимации положения представления.<br /><br />[Документация по API](xref:Xamarin.Forms.AbsoluteLayout)  /  [Руководством](~/xamarin-forms/user-interface/layouts/absolute-layout.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![Пример Абсолутелайаут](layouts-images/AbsoluteLayout.png "Пример Абсолутелайаут")](layouts-images/AbsoluteLayout-Large.png#lightbox "Пример Абсолутелайаут")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml) с [кодом программной части](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)Позиционирует дочерние элементы относительно `RelativeLayout` самого себя или их элементов того же уровня. Положение дочернего элемента указывается с помощью [присоединенных свойств](~/xamarin-forms/xaml/attached-properties.md) , которые задаются для объектов типа [`Constraint`](xref:Xamarin.Forms.Constraint) и [`BoundsConstraint`](xref:Xamarin.Forms.Constraint).<br /><br />[Документация по API](xref:Xamarin.Forms.RelativeLayout)  /  [Руководством](~/xamarin-forms/user-interface/layouts/relative-layout.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![Пример RelativeLayout](layouts-images/RelativeLayout.png "Пример RelativeLayout")](layouts-images/RelativeLayout-Large.png#lightbox "Пример RelativeLayout")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) |
|     |     |

### <a name="flexlayout"></a>FlexLayout

|     |     |
| --- | --- |
| [`FlexLayout`](xref:Xamarin.Forms.FlexLayout)основывается на [модуле макета гибкой рамки](https://www.w3.org/TR/css-flexbox-1/)CSS, обычно известном как _Flex Layout_ или _Flex-Box_. `FlexLayout` Определяет шесть привязываемые свойства и пять вложенных привязываемые свойства, которые позволяют дочерним элементам с накоплением и оболочку с многие параметры выравнивания и ориентацию.<br /><br />[Документация по API](xref:Xamarin.Forms.FlexLayout)  /  [Руководством](~/xamarin-forms/user-interface/layouts/flex-layout.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos) | [![Пример Флекслайаут](layouts-images/FlexLayout.png "Пример Флекслайаут")](layouts-images/FlexLayout-Large.png#lightbox "Пример Флекслайаут")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Связанные ссылки

- [Xamarin.FormsПример Формсгаллери](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.FormsРегистрируют](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsДокументация по API](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
