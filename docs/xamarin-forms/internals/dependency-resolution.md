---
title: Разрешение зависимостей в Xamarin.Forms
description: В этой статье объясняется, как внедрить метод разрешения зависимостей в Xamarin.Forms , чтобы контейнер внедрения зависимостей приложения получил контроль над созданием и временем существования пользовательских модулей подготовки отчетов, эффектов и реализаций DependencyService.
ms.prod: xamarin
ms.assetid: 491B87DC-14CB-4ADC-AC6C-40A7627B2524
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/27/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 58210aaaa7c017c50ba3e2aee3e8402c1b76be16
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373930"
---
# <a name="dependency-resolution-in-no-locxamarinforms"></a>Разрешение зависимостей в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-dicontainerdemo)

_В этой статье объясняется, как внедрить метод разрешения зависимостей в Xamarin.Forms , чтобы контейнер внедрения зависимостей приложения получил контроль над созданием и временем существования пользовательских модулей подготовки отчетов, эффектов и реализаций DependencyService. Примеры кода в этой статье взяты из примера [разрешения зависимостей с помощью контейнеров](/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-dicontainerdemo) ._

В контексте Xamarin.Forms приложения, использующего шаблон Model-View-ViewModel (MVVM), контейнер внедрения зависимостей можно использовать для регистрации и разрешения моделей представлений, а также для регистрации служб и их внедрения в модели представления. Во время создания модели представления контейнер внедряет все необходимые зависимости. Если эти зависимости не были созданы, контейнер сначала создает и разрешает зависимости. Дополнительные сведения о внедрении зависимостей, включая примеры внедрения зависимостей в модели представления, см. в разделе [внедрение зависимостей](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).

Управление созданием и временем существования типов в проектах платформы традиционно выполняется с помощью Xamarin.Forms , которое использует `Activator.CreateInstance` метод для создания экземпляров пользовательских модулей подготовки отчетов, эффектов и [`DependencyService`](xref:Xamarin.Forms.DependencyService) реализаций. К сожалению, это ограничивает Управление созданием и временем существования этих типов, а также возможность внедрять в них зависимости. Это поведение можно изменить, добавив в него метод разрешения зависимостей Xamarin.Forms , который управляет созданием типов — либо с помощью контейнера внедрения зависимостей приложения, либо с помощью Xamarin.Forms . Однако обратите внимание, что нет необходимости внедрять метод разрешения зависимостей в Xamarin.Forms . Xamarin.Forms будет продолжать создавать и администрировать время существования типов в проектах платформы, если не внедрен метод разрешения зависимостей.

> [!NOTE]
> Хотя эта статья посвящена внедрению метода разрешения зависимостей в Xamarin.Forms , который разрешает зарегистрированные типы с помощью контейнера внедрения зависимостей, можно также внедрить метод разрешения зависимостей, использующий заводские методы для разрешения зарегистрированных типов. Дополнительные сведения см. в примере [разрешения зависимостей с использованием фабричных методов](/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-factoriesdemo) .

## <a name="injecting-a-dependency-resolution-method"></a>Внедрение метода разрешения зависимостей

[`DependencyResolver`](xref:Xamarin.Forms.Internals.DependencyResolver)Класс предоставляет возможность внедрять метод разрешения зависимостей в Xamarin.Forms с помощью [`ResolveUsing`](xref:Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) метода. Затем, когда Xamarin.Forms требуется экземпляр определенного типа, метод разрешения зависимости получает возможность предоставить экземпляр. Если метод разрешения зависимостей возвращает `null` для запрошенного типа, то возвращается Xamarin.Forms к попытке создать экземпляр типа с помощью `Activator.CreateInstance` метода.

В следующем примере показано, как задать метод разрешения зависимостей с помощью [`ResolveUsing`](xref:Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) метода:

```csharp
using Autofac;
using Xamarin.Forms.Internals;
...

public partial class App : Application
{
    // IContainer and ContainerBuilder are provided by Autofac
    static IContainer container;
    static readonly ContainerBuilder builder = new ContainerBuilder();

    public App()
    {
        ...
        DependencyResolver.ResolveUsing(type => container.IsRegistered(type) ? container.Resolve(type) : null);
        ...
    }
    ...
}
```

В этом примере метод разрешения зависимостей устанавливается в лямбда-выражение, которое использует контейнер внедрения зависимостей Autofac для разрешения всех типов, зарегистрированных в контейнере. В противном случае `null` будет возвращено значение, которое приведет Xamarin.Forms к попытке разрешить тип.

> [!NOTE]
> API-интерфейс, используемый контейнером внедрения зависимостей, относится только к контейнеру. В примерах кода в этой статье используется Autofac в качестве контейнера внедрения зависимостей, который предоставляет `IContainer` `ContainerBuilder` типы и. Также можно использовать альтернативные контейнеры внедрения зависимостей, но использовать другие интерфейсы API, отличные от представленных здесь.

Обратите внимание, что при запуске приложения не требуется задавать метод разрешения зависимостей. Его можно задать в любое время. Единственное ограничение заключается в том, что Xamarin.Forms необходимо узнать о методе разрешения зависимостей по времени, которое приложение пытается использовать типы, хранящиеся в контейнере внедрения зависимостей. Таким образом, если в контейнере внедрения зависимостей есть службы, которые будут необходимы приложению во время запуска, то метод разрешения зависимостей должен быть установлен на раннем этапе жизненного цикла приложения. Аналогично, если контейнер внедрения зависимостей управляет созданием и временем существования конкретного объекта [`Effect`](xref:Xamarin.Forms.Effect) , Xamarin.Forms необходимо знать о методе разрешения зависимостей, прежде чем пытаться создать представление, использующее это `Effect` .

> [!WARNING]
> Регистрация и разрешение типов с помощью контейнера внедрения зависимостей имеет затраты на производительность из-за использования отражения в контейнере для создания каждого типа, особенно если зависимости перестраиваются для каждой навигации по страницам в приложении. При наличии большого числа зависимостей или глубоких зависимостей стоимость создания может значительно возрасти.

## <a name="registering-types"></a>Регистрация типов

Типы должны быть зарегистрированы в контейнере внедрения зависимостей, прежде чем они смогут разрешить их с помощью метода разрешения зависимостей. В следующем примере кода показаны методы регистрации, которые образец приложения предоставляет в `App` классе для контейнера Autofac:

```csharp
using Autofac;
using Autofac.Core;
...

public partial class App : Application
{
    static IContainer container;
    static readonly ContainerBuilder builder = new ContainerBuilder();
    ...

    public static void RegisterType<T>() where T : class
    {
        builder.RegisterType<T>();
    }

    public static void RegisterType<TInterface, T>() where TInterface : class where T : class, TInterface
    {
        builder.RegisterType<T>().As<TInterface>();
    }

    public static void RegisterTypeWithParameters<T>(Type param1Type, object param1Value, Type param2Type, string param2Name) where T : class
    {
        builder.RegisterType<T>()
               .WithParameters(new List<Parameter>()
        {
            new TypedParameter(param1Type, param1Value),
            new ResolvedParameter(
                (pi, ctx) => pi.ParameterType == param2Type && pi.Name == param2Name,
                (pi, ctx) => ctx.Resolve(param2Type))
        });
    }

    public static void RegisterTypeWithParameters<TInterface, T>(Type param1Type, object param1Value, Type param2Type, string param2Name) where TInterface : class where T : class, TInterface
    {
        builder.RegisterType<T>()
               .WithParameters(new List<Parameter>()
        {
            new TypedParameter(param1Type, param1Value),
            new ResolvedParameter(
                (pi, ctx) => pi.ParameterType == param2Type && pi.Name == param2Name,
                (pi, ctx) => ctx.Resolve(param2Type))
        }).As<TInterface>();
    }

    public static void BuildContainer()
    {
        container = builder.Build();
    }
    ...
}
```

Если приложение использует метод разрешения зависимостей для разрешения типов из контейнера, регистрации типов обычно выполняются из проектов платформы. Это позволяет проектам платформы регистрировать типы для пользовательских модулей подготовки отчетов, эффектов и [`DependencyService`](xref:Xamarin.Forms.DependencyService) реализаций.

После регистрации типа в проекте платформы `IContainer` объект должен быть построен, что выполняется путем вызова `BuildContainer` метода. Этот метод вызывает `Build` метод Autofac для `ContainerBuilder` экземпляра, который создает новый контейнер внедрения зависимостей, который содержит выполненные регистрации.

В следующих разделах `Logger` класс, реализующий `ILogger` интерфейс, внедряется в конструкторы классов. `Logger`Класс реализует простую функциональность ведения журнала с помощью `Debug.WriteLine` метода и используется для демонстрации того, как службы можно внедрять в пользовательские модули подготовки отчетов, эффекты и [`DependencyService`](xref:Xamarin.Forms.DependencyService) реализации.

### <a name="registering-custom-renderers"></a>Регистрация пользовательских модулей подготовки отчетов

Пример приложения включает страницу для воспроизведения веб-видео, источник XAML которого показан в следующем примере:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             ...>
    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />
</ContentPage>
```

`VideoPlayer`Представление реализуется на каждой платформе `VideoPlayerRenderer` классом, который предоставляет функциональные возможности для воспроизведения видео. Дополнительные сведения об этих пользовательских классах модуля подготовки отчетов см. [в разделе Реализация видеопроигрывателя](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md).

В iOS и универсальная платформа Windows (UWP) `VideoPlayerRenderer` классы имеют следующий конструктор, для которого требуется `ILogger` аргумент:

```csharp
public VideoPlayerRenderer(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

На всех платформах регистрация типа с помощью контейнера внедрения зависимостей выполняется `RegisterTypes` методом, который вызывается до того, как платформа загружает приложение с помощью `LoadApplication(new App())` метода. В следующем примере показан `RegisterTypes` метод на платформе iOS:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<FormsVideoLibrary.iOS.VideoPlayerRenderer>();
    App.BuildContainer();
}
```

В этом примере `Logger` конкретный тип регистрируется с помощью сопоставления с типом интерфейса, а `VideoPlayerRenderer` Тип регистрируется напрямую без сопоставления интерфейса. Когда пользователь переходит на страницу `VideoPlayer` , содержащую представление, метод разрешения зависимостей будет вызван для разрешения `VideoPlayerRenderer` типа из контейнера внедрения зависимостей, который также будет разрешать и внедрять `Logger` тип в `VideoPlayerRenderer` конструктор.

`VideoPlayerRenderer`Конструктор на платформе Android немного сложнее, так как `Context` в дополнение к `ILogger` аргументу требуется аргумент:

```csharp
public VideoPlayerRenderer(Context context, ILogger logger) : base(context)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

В следующем примере показан `RegisterTypes` метод на платформе Android:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterTypeWithParameters<FormsVideoLibrary.Droid.VideoPlayerRenderer>(typeof(Android.Content.Context), this, typeof(ILogger), "logger");
    App.BuildContainer();
}
```

В этом примере `App.RegisterTypeWithParameters` метод регистрирует объект `VideoPlayerRenderer` с помощью контейнера внедрения зависимостей. Метод регистрации гарантирует, что `MainActivity` экземпляр будет вставлен в качестве `Context` аргумента и что `Logger` тип будет внедрен в качестве `ILogger` аргумента.

### <a name="registering-effects"></a>Регистрация эффектов

Пример приложения включает страницу, которая использует эффекты отслеживания касания для перетаскивания [`BoxView`](xref:Xamarin.Forms.BoxView) экземпляров вокруг страницы. [`Effect`](xref:Xamarin.Forms.Effect)Добавляется в в, `BoxView` используя следующий код:

```csharp
var boxView = new BoxView { ... };
var touchEffect = new TouchEffect();
boxView.Effects.Add(touchEffect);
```

`TouchEffect`Класс — это объект [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) , реализуемый на каждой платформе `TouchEffect` классом, который имеет значение `PlatformEffect` . Класс Platform `TouchEffect` предоставляет функциональные возможности для перетаскивания `BoxView` вокруг страницы. Дополнительные сведения об этих классах эффектов см. [в разделе вызов событий из эффектов](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

На всех платформах `TouchEffect` класс имеет следующий конструктор, для которого требуется `ILogger` аргумент:

```csharp
public TouchEffect(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

На всех платформах регистрация типа с помощью контейнера внедрения зависимостей выполняется `RegisterTypes` методом, который вызывается до того, как платформа загружает приложение с помощью `LoadApplication(new App())` метода. В следующем примере показан `RegisterTypes` метод на платформе Android:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<TouchTracking.Droid.TouchEffect>();
    App.BuildContainer();
}
```

В этом примере `Logger` конкретный тип регистрируется с помощью сопоставления с типом интерфейса, а `TouchEffect` Тип регистрируется напрямую без сопоставления интерфейса. Когда пользователь переходит на страницу, содержащую [`BoxView`](xref:Xamarin.Forms.BoxView) экземпляр, к которому `TouchEffect` присоединен объект, метод разрешения зависимостей будет вызван для разрешения типа платформы `TouchEffect` из контейнера внедрения зависимостей, который также будет разрешать и внедрять `Logger` тип в `TouchEffect` конструктор.

### <a name="registering-dependencyservice-implementations"></a>Регистрация реализаций DependencyService

Пример приложения включает страницу, использующую [`DependencyService`](xref:Xamarin.Forms.DependencyService) реализации на каждой платформе, чтобы позволить пользователю выбрать фотографию из библиотеки изображений устройства. `IPhotoPicker`Интерфейс определяет функциональность, реализуемую `DependencyService` реализациями, и показана в следующем примере:

```csharp
public interface IPhotoPicker
{
    Task<Stream> GetImageStreamAsync();
}
```

В каждом проекте платформы `PhotoPicker` класс реализует `IPhotoPicker` интерфейс с помощью API-интерфейсов платформы. Дополнительные сведения об этих службах зависимостей см. в разделе Выбор [фотографии из библиотеки рисунков](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

В iOS и UWP `PhotoPicker` классы имеют следующий конструктор, для которого требуется `ILogger` аргумент:

```csharp
public PhotoPicker(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

На всех платформах регистрация типа с помощью контейнера внедрения зависимостей выполняется `RegisterTypes` методом, который вызывается до того, как платформа загружает приложение с помощью `LoadApplication(new App())` метода. В следующем примере показан `RegisterTypes` метод в UWP:

```csharp
void RegisterTypes()
{
    DIContainerDemo.App.RegisterType<ILogger, Logger>();
    DIContainerDemo.App.RegisterType<IPhotoPicker, Services.UWP.PhotoPicker>();
    DIContainerDemo.App.BuildContainer();
}
```

В этом примере `Logger` конкретный тип регистрируется с помощью сопоставления с типом интерфейса, а `PhotoPicker` тип также регистрируется с помощью сопоставления интерфейса.

`PhotoPicker`Конструктор на платформе Android немного сложнее, так как `Context` в дополнение к `ILogger` аргументу требуется аргумент:

```csharp
public PhotoPicker(Context context, ILogger logger)
{
    _context = context ?? throw new ArgumentNullException(nameof(context));
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

В следующем примере показан `RegisterTypes` метод на платформе Android:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterTypeWithParameters<IPhotoPicker, Services.Droid.PhotoPicker>(typeof(Android.Content.Context), this, typeof(ILogger), "logger");
    App.BuildContainer();
}
```

В этом примере `App.RegisterTypeWithParameters` метод регистрирует объект `PhotoPicker` с помощью контейнера внедрения зависимостей. Метод регистрации гарантирует, что `MainActivity` экземпляр будет вставлен в качестве `Context` аргумента и что `Logger` тип будет внедрен в качестве `ILogger` аргумента.

Когда пользователь переходит на страницу выбора фотографий и выбирает фотографию, `OnSelectPhotoButtonClicked` выполняется обработчик:

```csharp
async void OnSelectPhotoButtonClicked(object sender, EventArgs e)
{
    ...
    var photoPickerService = DependencyService.Resolve<IPhotoPicker>();
    var stream = await photoPickerService.GetImageStreamAsync();
    if (stream != null)
    {
        image.Source = ImageSource.FromStream(() => stream);
    }
    ...
}
```

При [`DependencyService.Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) вызове метода вызывается метод разрешения зависимостей для разрешения `PhotoPicker` типа из контейнера внедрения зависимостей, который также будет разрешать и внедрять `Logger` тип в `PhotoPicker` конструктор.

> [!NOTE]
> [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*)Метод должен использоваться при разрешении типа из контейнера внедрения зависимостей приложения через [`DependencyService`](xref:Xamarin.Forms.DependencyService) .

## <a name="related-links"></a>Связанные ссылки

- [Разрешение зависимостей с помощью контейнеров (пример)](/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-dicontainerdemo)
- [Внедрение зависимостей](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)
- [Реализация видеопроигрывателя](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md)
- [Вызов событий из эффектов](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
- [Выбор фотографии из библиотеки рисунков](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)