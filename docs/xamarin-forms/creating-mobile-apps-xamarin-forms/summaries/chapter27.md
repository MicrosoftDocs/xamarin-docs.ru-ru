---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 27. Custom renderers''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4b0a3e14126dc8c92a1d1b60db7dbb2afd8e8c12
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136595"
---
# <a name="summary-of-chapter-27-custom-renderers"></a>Сводная информация о Главе 27. Пользовательские отрисовщики

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)

> [!NOTE] 
> Примечания на этой странице указывают области, в которых Xamarin.Forms имеет расхождения с материалом, представленным в книге.

Элемент Xamarin.Forms, например `Button`, преобразуется для просмотра с помощью платформенной кнопки, инкапсулированной в класс с именем `ButtonRenderer`.  Ниже приведены версии для [iOS`ButtonRenderer`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/ButtonRenderer.cs), [Android`ButtonRenderer`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/Renderers/ButtonRenderer.cs) и [UWP`ButtonRenderer`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.UAP/ButtonRenderer.cs).

В этой главе рассматривается создание собственных отрисовщиков для создания пользовательских представлений, сопоставленных с платформенными объектами.

## <a name="the-complete-class-hierarchy"></a>Полная иерархия классов

Существует четыре сборки, содержащих платформенный код Xamarin.Forms.
Вы можете просмотреть исходный код на сайте GitHub этим ссылкам:

- [ **Xamarin.Forms.Platform**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform) (очень короткий)
- [ **Xamarin.Forms.Platform.iOS**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.iOS)
- [ **Xamarin.Forms.Platform.Android**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.Android)
- [ **Xamarin.Forms.Platform.UAP**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.UAP)

> [!NOTE]
> Сборки `WinRT`, упомянутые в книге, больше не являются частью этого решения. 

В примере [**PlatformClassHierarchy**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/PlatformClassHierarchy) показана иерархия классов для сборок, которые являются допустимыми для исполняющей платформы.

Обратите внимание на важный класс с именем `ViewRenderer`. Это класс, от которого производят наследование при создании платформенного отрисовщика. Он существует в трех разных версиях, так как он привязан к системе представления целевой платформы:

В iOS [`ViewRenderer<TView, TNativeView>`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs#L25) использует универсальные аргументы:

- `TView`, связанный с [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeView`, связанный с [`UIKit.UIView`](xref:UIKit.UIView)

В Android [`ViewRenderer<TView, TNativeView>`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/ViewRenderer.cs#L17) использует универсальные аргументы:

- `TView`, связанный с [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeView`, связанный с [`Android.Views.View`](xref:Android.Views.View)

В UWP [`ViewRenderer<TElement, TNativeElement>`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.UAP/ViewRenderer.cs#L6) использует универсальные аргументы:

- `TElement`, связанный с [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeElement`, связанный с [`Windows.UI.Xaml.FrameworkElement`](/uwp/api/Windows.UI.Xaml.FrameworkElement)

При написании отрисовщика вы получите производный класс от `View`, а затем запишете несколько `ViewRenderer` классов, по одному для каждой поддерживаемой платформы. Каждая платформенная реализация будет ссылаться на собственный класс, производный от типа, указанного в качестве параметра `TNativeView` или `TNativeElement`.

## <a name="hello-custom-renderers"></a>Пользовательские отрисовщики

Программа [**HelloRenderers**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/HelloRenderers) ссылается на пользовательское представление с именем `HelloView` в своем классе [`App`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/App.cs).

Класс [`HelloView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/HelloView.cs) включен в проект **HelloRenderers** и просто является производным от `View`.

Класс [`HelloViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.iOS/HelloViewRenderer.cs) в проекте **HelloRenderers.iOS** является производным от `ViewRenderer<HelloView, UILabel>`. В переопределении `OnElementChanged` создается собственная iOS `UILabel` и вызывается `SetNativeControl`.

Класс [`HelloViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.Droid/HelloViewRenderer.cs) в проекте **HelloRenderers.Droid** является производным от `ViewRenderer<HelloView, TextView>`. В переопределении `OnElementChanged` создается Android `TextView` и вызывается `SetNativeControl`.

Класс [`HelloViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.UWP/HelloViewRenderer.cs) в **HelloRenderers.UWP** и других проектах Windows является производным от `ViewRenderer<HelloView, TextBlock>`. В переопределении `OnElementChanged` создается Windows `TextBlock` и вызывается `SetNativeControl`.

Все производные от `ViewRenderer` содержат атрибут `ExportRenderer` на уровне сборки, связывающий класс `HelloView` с определенным классом `HelloViewRenderer`. Таким образом Xamarin.Forms находит отрисовщики в отдельных проектах платформы:

[![Тройной снимок экрана Hello View](images/ch27fg02-small.png "Пользовательские отрисовщики")](images/ch27fg02-large.png#lightbox "Пользовательские отрисовщики")

## <a name="renderers-and-properties"></a>Отрисовщики и свойства

Следующий набор отрисовщиков рисует эллипс и находится в различных проектах решения [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform).

Класс [`EllipseView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/EllipseView.cs) принадлежит платформе **Xamarin.FormsBook.Platform**. Класс похож на `BoxView` и определяет только одно свойство: `Color` типа `Color`.

Отрисовщики могут передавать значения свойств, заданные для `View`, в собственный объект, переопределяя метод `OnElementPropertyChanged` в отрисовщике. В этом методе (и в большей части отрисовщиков) доступны два свойства:

- `Element`, элемент Xamarin.Forms
- `Control`, собственное представление, мини-приложение или управляющий объект

Типы этих свойств определяются универсальными параметрами для `ViewRenderer`. В этом примере `Element` имеет тип `EllipseView`.

Таким образом, переопределение `OnElementPropertyChanged` может переносить значение `Color` из `Element` в собственный объект `Control`, возможно, с какого-то рода преобразованием. Три отрисовщика:

- iOS — [`EllipseViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseViewRenderer.cs), который для отрисовки эллипса использует класс [`EllipseUIView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseUIView.cs).
- Android — [`EllipseViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseViewRenderer.cs), который использует класс [`EllipseDrawableView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseDrawableView.cs) для отрисовки эллипса.
- UWP — [`EllipseViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/EllipseViewRenderer.cs), который может использовать собственный класс Windows [`Ellipse`](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse).

Класс [**EllipseDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/EllipseDemo) отображает несколько таких объектов `EllipseView`:

[![Тройной снимок экрана с демонстрацией эллипсов](images/ch27fg03-small.png "Пользовательские отрисовщики EllipseView")](images/ch27fg03-large.png#lightbox "Пользовательские отрисовщики EllipseView")

[**BouncingBall**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/BouncingBall) заставляет `EllipseView` отскакивать от краев экрана.

## <a name="renderers-and-events"></a>Отрисовщики и события

Кроме того, отрисовщики могут создавать события неявно. Класс [`StepSlider`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/StepSlider.cs) похож на стандартный класс Xamarin.Forms `Slider`, но позволяет указать ряд дискретных шагов между значениями `Minimum` и `Maximum`.

Три отрисовщика:

- iOS: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/StepSliderRenderer.cs)
- Android: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/StepSliderRenderer.cs)
- UWP: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/StepSliderRenderer.cs)

Отрисовщики обнаруживают изменения в собственном элементе управления, а затем вызывают `SetValueFromRenderer`, который ссылается на свойство с привязкой, определенное в `StepSlider`, изменение которого заставляет `StepSlider` запускать событие `ValueChanged`.

В примере [**StepSliderDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/StepSliderDemo) демонстрируется такой новый ползунок.

## <a name="related-links"></a>Связанные ссылки

- [Полный текст главы 27 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)
- [Примеры для Главы 27](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)
- [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
