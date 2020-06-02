---
title: ''
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
ms.openlocfilehash: 50e28291d72550264e3806c0911f59a57c6d8bf0
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136335"
---
# <a name="customizing-a-contentpage"></a>Настройка ContentPage

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-contentpage)

_ContentPage — это визуальный элемент, который отображает одно представление и занимает большую часть экрана. В этой статье показано, как создать пользовательский отрисовщик для страницы ContentPage, чтобы переопределять собственный способ отрисовки по умолчанию с помощью настройки в зависимости от платформы._

Каждый элемент управления Xamarin.Forms имеет сопутствующий отрисовщик для каждой платформы, который создает экземпляр собственного элемента управления. Когда [`ContentPage`](xref:Xamarin.Forms.ContentPage) отображается в приложении Xamarin.Forms, на платформе iOS создается класс `PageRenderer`, который, в свою очередь, создает собственный элемент управления `UIViewController`. На платформе Android класс `PageRenderer` создает элемент управления `ViewGroup`. На универсальной платформе Windows (UWP) класс `PageRenderer` создает элемент управления `FrameworkElement`. Дополнительные сведения об отрисовщике и классах собственных элементов управления, с которыми сопоставляются элементы управления Xamarin.Forms, см. в статье [Базовые классы и собственные элементы управления отрисовщика](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

На следующей схеме показана связь между классом [`ContentPage`](xref:Xamarin.Forms.ContentPage) и соответствующими собственными элементами управления, которые его реализуют:

![](contentpage-images/contentpage-classes.png "Relationship Between ContentPage Class and Implementing Native Controls")

Процесс отрисовки можно использовать для реализации настроек конкретных платформ путем создания пользовательского отрисовщика для [`ContentPage`](xref:Xamarin.Forms.ContentPage) на каждой платформе. Этот процесс выглядит следующим образом:

1. [Создание](#Creating_the_Xamarin.Forms_Page) страницы Xamarin.Forms.
1. [Использование](#Consuming_the_Xamarin.Forms_Page) страницы из Xamarin.Forms.
1. [Создание](#Creating_the_Page_Renderer_on_each_Platform) пользовательского отрисовщика для страницы на каждой платформе.

Мы рассмотрим каждый пункт по порядку, чтобы реализовать класс `CameraPage` с возможностями прямой видеотрансляции и фотографирования.

<a name="Creating_the_Xamarin.Forms_Page" />

## <a name="creating-the-xamarinforms-page"></a>Создание страницы Xamarin.Forms

Неизмененный класс [`ContentPage`](xref:Xamarin.Forms.ContentPage) можно добавить в общий проект Xamarin.Forms, как показано в следующем примере кода XAML.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CustomRenderer.CameraPage">
    <ContentPage.Content>
    </ContentPage.Content>
</ContentPage>
```

Аналогичным образом файл программного кода для класса [`ContentPage`](xref:Xamarin.Forms.ContentPage) также должен остаться неизменным, как показано в следующем примере кода.

```csharp
public partial class CameraPage : ContentPage
{
    public CameraPage ()
    {
        // A custom renderer is used to display the camera UI
        InitializeComponent ();
    }
}
```

В следующем примере кода показано создание страницы в C#.

```csharp
public class CameraPageCS : ContentPage
{
    public CameraPageCS ()
    {
    }
}
```

Для прямой трансляции видео на каждой платформе будет использоваться экземпляр класса `CameraPage`. Настройка элемента управления будет осуществляться в пользовательском отрисовщике, поэтому дополнительная реализация в классе `CameraPage` не требуется.

<a name="Consuming_the_Xamarin.Forms_Page" />

## <a name="consuming-the-xamarinforms-page"></a>Использование страницы Xamarin.Forms

В приложении Xamarin.Forms должен отображаться пустой класс `CameraPage`. Это происходит при нажатии кнопки в экземпляре `MainPage`, что, в свою очередь, приводит к выполнению метода `OnTakePhotoButtonClicked`, как показано в следующем примере кода.

```csharp
async void OnTakePhotoButtonClicked (object sender, EventArgs e)
{
    await Navigation.PushAsync (new CameraPage ());
}
```

Код просто переходит к классу `CameraPage`, в котором пользовательские отрисовщики будут настраивать внешний вид страницы на каждой платформе.

<a name="Creating_the_Page_Renderer_on_each_Platform" />

## <a name="creating-the-page-renderer-on-each-platform"></a>Создание отрисовщика страницы на каждой платформе

Процесс создания класса пользовательского отрисовщика выглядит следующим образом:

1. Создайте подкласс класса `PageRenderer`.
1. Переопределение метода `OnElementChanged`, который отрисовывает собственную страницу, и написание логики для настройки страницы. Метод `OnElementChanged` вызывается при создании соответствующего элемента управления Xamarin.Forms.
1. Добавление атрибута `ExportRenderer` в класс отрисовщика страницы, чтобы указать, что он будет использоваться для отрисовки страницы Xamarin.Forms. Этот атрибут используется для регистрации пользовательского отрисовщика в Xamarin.Forms.

> [!NOTE]
> Предоставлять отрисовщик страницы в проекте каждой платформы необязательно. Если отрисовщик страницы не зарегистрирован, будет использоваться отрисовщик по умолчанию.

На следующей схеме показаны обязанности каждого проекта в примере приложения, а также связи между ними:

![](contentpage-images/solution-structure.png "CameraPage Custom Renderer Project Responsibilities")

Экземпляр `CameraPage` отрисовывается с помощью зависящих от платформы классов `CameraPageRenderer`, которые являются производными от класса `PageRenderer` данной платформы. В результате каждый экземпляр `CameraPage` отрисовывается в ходе прямой видеотрансляции, как показано на следующих снимках экрана.

![](contentpage-images/screenshots.png "CameraPage on each Platform")

Класс `PageRenderer` предоставляет метод `OnElementChanged`, который вызывается при создании страницы Xamarin.Forms для отрисовки соответствующего собственного элемента управления. Этот метод принимает параметр `ElementChangedEventArgs`, содержащий свойства `OldElement` и `NewElement`. Эти свойства представляют элемент Xamarin.Forms, к которому *был* присоединен отрисовщик, и элемент Xamarin.Forms, к которому *присоединен* отрисовщик, соответственно. В примере приложения свойство `OldElement` будет иметь значение `null`, а свойство `NewElement` будет содержать ссылку на экземпляр `CameraPage`.

Настройка собственной страницы выполняется в переопределенной версии метода `OnElementChanged` в классе `CameraPageRenderer`. Ссылку на отрисовываемый экземпляр страницы Xamarin.Forms можно получить с помощью свойства `Element`.

Каждый класс пользовательского отрисовщика дополняется атрибутом `ExportRenderer`, который регистрирует отрисовщик в Xamarin.Forms. Атрибут принимает два параметра — имя типа отрисовываемой страницы Xamarin.Forms и имя типа пользовательского отрисовщика. Префикс `assembly` в атрибуте указывает, что атрибут применяется ко всей сборке.

В следующих разделах рассматривается реализация пользовательского отрисовщика `CameraPageRenderer` для каждой платформы.

### <a name="creating-the-page-renderer-on-ios"></a>Создание отрисовщика страницы на платформе iOS

В следующем примере кода показан отрисовщик страницы для платформы iOS.

```csharp
[assembly:ExportRenderer (typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPageRenderer : PageRenderer
    {
        ...

        protected override void OnElementChanged (VisualElementChangedEventArgs e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null || Element == null) {
                return;
            }

            try {
                SetupUserInterface ();
                SetupEventHandlers ();
                SetupLiveCameraStream ();
                AuthorizeCameraUse ();
            } catch (Exception ex) {
                System.Diagnostics.Debug.WriteLine (@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

В результате вызова метода `OnElementChanged` базового класса создается элемент управления iOS `UIViewController`. Отрисовка трансляции видеопотока происходит только при условии, что отрисовщик еще не присоединен к существующему элементу Xamarin.Forms и существует экземпляр страницы, который отрисовывается пользовательским отрисовщиком.

Затем страница настраивается с помощью ряда методов, использующих API `AVCapture` для предоставления возможностей трансляции видеопотока и фотографирования.

### <a name="creating-the-page-renderer-on-android"></a>Создание отрисовщика страницы на платформе Android

В следующем примере кода показан отрисовщик страницы для платформы Android.

```csharp
[assembly: ExportRenderer(typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPageRenderer : PageRenderer, TextureView.ISurfaceTextureListener
    {
        ...
        public CameraPageRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Page> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                SetupUserInterface();
                SetupEventHandlers();
                AddView(view);
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

В результате вызова метода `OnElementChanged` базового класса создается элемент управления `ViewGroup` Android, который является группой представлений. Отрисовка трансляции видеопотока происходит только при условии, что отрисовщик еще не присоединен к существующему элементу Xamarin.Forms и существует экземпляр страницы, который отрисовывается пользовательским отрисовщиком.

Затем страница настраивается путем вызова ряда методов, использующих API `Camera` для предоставления возможностей трансляции видеопотока и фотографирования. После этого вызывается метод `AddView` для добавления пользовательского интерфейса трансляции видеопотока в элемент управления `ViewGroup`. Обратите внимание, что для выполнения операций по измерению и действий с макетом на платформе Android также требуется переопределить метод `OnLayout`. Дополнительные сведения см. на странице с [примером отрисовщика ContentPage](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-contentpage).

### <a name="creating-the-page-renderer-on-uwp"></a>Создание отрисовщика страницы на платформе UWP

В следующем примере кода показан отрисовщик страницы для платформы UWP.

```csharp
[assembly: ExportRenderer(typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.UWP
{
    public class CameraPageRenderer : PageRenderer
    {
        ...
        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.Page> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                ...
                SetupUserInterface();
                SetupBasedOnStateAsync();

                this.Children.Add(page);
            }
            ...
        }

        protected override Size ArrangeOverride(Size finalSize)
        {
            page.Arrange(new Windows.Foundation.Rect(0, 0, finalSize.Width, finalSize.Height));
            return finalSize;
        }
        ...
    }
}

```

В результате вызова метода `OnElementChanged` базового класса создается элемент управления `FrameworkElement`, в котором отрисовывается страница. Отрисовка трансляции видеопотока происходит только при условии, что отрисовщик еще не присоединен к существующему элементу Xamarin.Forms и существует экземпляр страницы, который отрисовывается пользовательским отрисовщиком. Затем страница настраивается путем вызова ряда методов, использующих API `MediaCapture` для предоставления возможностей трансляции видеопотока и фотографирования. После этого настроенная страница добавляется в коллекцию `Children` для отображения.

При реализации пользовательского отрисовщика, который является производным от класса `PageRenderer` на платформе UWP, необходимо также реализовать метод `ArrangeOverride` для упорядочения элементов управления страницы, так как базовый отрисовщик не знает, что с ними делать. В противном случае будет отображена пустая страница. Таким образом, в этом примере метод `ArrangeOverride` вызывает метод `Arrange` в экземпляре `Page`.

> [!NOTE]
> Важно остановить и удалить объекты, которые предоставляют доступ к камере в приложении UWP. Невыполнение этого требования может помешать работе других приложений, которые пытаются получить доступ к камере устройства. Дополнительные сведения см. в статье об [отображении предварительного изображения с камеры](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access).

## <a name="summary"></a>Сводка

В этой статье было показано, как создать пользовательский отрисовщик для страницы [`ContentPage`](xref:Xamarin.Forms.ContentPage), чтобы переопределять собственный способ отрисовки по умолчанию с помощью настройки в зависимости от платформы. `ContentPage` — это визуальный элемент, который отображает одно представление и занимает большую часть экрана.

## <a name="related-links"></a>Связанные ссылки

- [CustomRendererContentPage (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-contentpage)
