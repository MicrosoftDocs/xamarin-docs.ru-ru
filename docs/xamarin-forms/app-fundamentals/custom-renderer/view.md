---
title: Описание реализации представления: В этой статье описывается, как создать настраиваемый отрисовщик для пользовательского элемента управления Xamarin.Forms, который используется для отображения видеопотока для предварительного просмотра с камеры устройства.
ms.prod: xamarin ms.assetid: 915E25E7-4A6B-4F34-B7B4-07D5F4B240F2 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 10/30/2020 no-loc:
- "Xamarin.Forms"
- "Xamarin.Essentials"

---
# <a name="implementing-a-view"></a>Реализация представления

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-view)

Пользовательские элементы управления пользовательского интерфейса _Xamarin.Forms должны быть производными от класса View, который используется для размещения макетов и элементов управления на экране. Эта статья описывает, как создать настраиваемый отрисовщик для пользовательского элемента управления Xamarin.Forms, который используется для отображения видеопотока для предварительного просмотра с камеры устройства._

Каждое представление Xamarin.Forms имеет сопутствующий отрисовщик для каждой платформы, который создает экземпляр собственного элемента управления. Когда [`View`](xref:Xamarin.Forms.View) отображается в приложении Xamarin.Forms, на платформе iOS создается класс `ViewRenderer`, который, в свою очередь, создает собственный элемент управления `UIView`. На платформе Android класс `ViewRenderer` создает собственный элемент управления `View`. На универсальной платформе Windows (UWP) класс `ViewRenderer` создает экземпляр собственного элемента управления `FrameworkElement`. Дополнительные сведения об отрисовщике и классах собственных элементов управления, с которыми сопоставляются элементы управления Xamarin.Forms, см. в статье [Базовые классы и собственные элементы управления отрисовщика](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

> [!NOTE]
> Некоторые элементы управления в Android используют быстрые отрисовщики, которые не используют класс `ViewRenderer`. Дополнительные сведения о быстрых отрисовщиках см. в разделе [Xamarin.Forms Быстрые отрисовщики](~/xamarin-forms/internals/fast-renderers.md).

На следующей схеме показана связь между классом [`View`](xref:Xamarin.Forms.View) и соответствующими собственными элементами управления, которые его реализуют:

![Связь между классом View и его реализующими нативными классами](view-images/view-classes.png)

Процесс отрисовки можно использовать для реализации настроек конкретных платформ путем создания настраиваемого отрисовщика для [`View`](xref:Xamarin.Forms.View) на каждой платформе. Этот процесс выглядит следующим образом:

1. [Создание](#creating-the-custom-control) пользовательского элемента управления Xamarin.Forms.
1. [Использование](#consuming-the-custom-control) пользовательского элемента управления в Xamarin.Forms.
1. [Создание](#creating-the-custom-renderer-on-each-platform) пользовательского отрисовщика для элемента управления на каждой платформе.

Далее будет поочередно рассмотрен каждый элемент для реализации отрисовщика `CameraPreview`, который отображает видеопоток для предварительного просмотра с камеры устройства. Коснувшись видеопотока, можно запустить или остановить его.

## <a name="creating-the-custom-control"></a>Создание пользовательского элемента управления

Пользовательский элемент управления можно создать путем создания подклассов класса [`View`](xref:Xamarin.Forms.View), как показано в следующем примере кода:

```csharp
public class CameraPreview : View
{
  public static readonly BindableProperty CameraProperty = BindableProperty.Create (
    propertyName: "Camera",
    returnType: typeof(CameraOptions),
    declaringType: typeof(CameraPreview),
    defaultValue: CameraOptions.Rear);

  public CameraOptions Camera
  {
    get { return (CameraOptions)GetValue (CameraProperty); }
    set { SetValue (CameraProperty, value); }
  }
}
```

Пользовательский элемент управления `CameraPreview` создается в проекте библиотеки .NET Standard и определяет API для элемента управления. Пользовательский элемент управления предоставляет свойство `Camera`, используемое для управления отображением видеопотока с передней или задней камеры на устройстве. Если при создании элемента управления значение свойства `Camera` не указано, по умолчанию используется задняя камера.

## <a name="consuming-the-custom-control"></a>Использование пользовательского элемента управления

На пользовательский элемент управления `CameraPreview` можно ссылаться в XAML в проекте библиотеки .NET Standard, объявив пространство имен для его расположения и используя префикс пространства имен в пользовательском элементе управления. В следующем примере кода показано, как пользовательский элемент управления `CameraPreview` может использоваться страницей XAML.

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             ...>
    <StackLayout>
        <Label Text="Camera Preview:" />
        <local:CameraPreview Camera="Rear"
                             HorizontalOptions="FillAndExpand"
                             VerticalOptions="FillAndExpand" />
    </StackLayout>
</ContentPage>
```

Префикс пространства имен `local` может иметь любое имя. Тем не менее значения `clr-namespace` и `assembly` должны соответствовать данным пользовательского элемента управления. После объявления пространства имен префикс используется для ссылки на пользовательский элемент управления.

В следующем примере кода показано, как пользовательский элемент управления `CameraPreview` может использоваться страницей C#:

```csharp
public class MainPageCS : ContentPage
{
  public MainPageCS ()
  {
    ...
    Content = new StackLayout
    {
      Children =
      {
        new Label { Text = "Camera Preview:" },
        new CameraPreview
        {
          Camera = CameraOptions.Rear,
          HorizontalOptions = LayoutOptions.FillAndExpand,
          VerticalOptions = LayoutOptions.FillAndExpand
        }
      }
    };
  }
}
```

Экземпляр `CameraPreview` пользовательского элемента управления будет использоваться для отображения видеопотока для предварительного просмотра с камеры устройства. Кроме необязательного указания значения для свойства `Camera`, настройка элемента управления будет осуществляться в пользовательском отрисовщике.

Пользовательский отрисовщик теперь можно добавлять в каждый проект приложения для создания зависящих от платформы элементов управления для предварительного просмотра с камеры.

## <a name="creating-the-custom-renderer-on-each-platform"></a>Создание пользовательского отрисовщика на каждой платформе

Процесс создания класса пользовательского отрисовщика в iOS и UWP выглядит следующим образом:

1. Создайте подкласс класса `ViewRenderer<T1,T2>`, который отрисовывает пользовательский элемент управления. Первый аргумент типа должен быть пользовательским элементом управления, для которого предназначен отрисовщик, в данном случае — `CameraPreview`. Второй аргумент типа должен быть собственным элементом управления, который будет реализовывать пользовательский элемент управления.
1. Переопределите метод `OnElementChanged`, который отрисовывает пользовательский элемент управления, и напишите логику для его настройки. Этот метод вызывается при создании соответствующего элемента управления Xamarin.Forms.
1. Добавьте атрибут `ExportRenderer` в класс настраиваемого отрисовщика, чтобы указать, что он будет использоваться для отрисовки пользовательского элемента управления Xamarin.Forms. Этот атрибут используется для регистрации пользовательского отрисовщика в Xamarin.Forms.

Процесс создания класса пользовательского отрисовщика в Android в качестве быстрого отрисовщика выглядит следующим образом:

1. Создайте подкласс элемента управления Android, который отрисовывает пользовательский элемент управления. Кроме того, укажите, что подкласс будет реализовывать интерфейсы `IVisualElementRenderer` и `IViewRenderer`.
1. Реализуйте интерфейсы `IVisualElementRenderer` и `IViewRenderer` в классе быстрого отрисовщика.
1. Добавьте атрибут `ExportRenderer` в класс настраиваемого отрисовщика, чтобы указать, что он будет использоваться для отрисовки пользовательского элемента управления Xamarin.Forms. Этот атрибут используется для регистрации пользовательского отрисовщика в Xamarin.Forms.

> [!NOTE]
> Для большинства элементов Xamarin.Forms предоставлять пользовательский отрисовщик в проекте для каждой платформы необязательно. Если пользовательский отрисовщик не зарегистрирован, будет использоваться отрисовщик по умолчанию для базового класса элемента управления. Однако настраиваемые отрисовщики необходимы в каждом зависящем от платформы проекте при отрисовке элемента [View](xref:Xamarin.Forms.View).

На следующей схеме показаны обязанности каждого проекта в примере приложения, а также связи между ними:

![Задачи проекта настраиваемого отрисовщика CameraPreview](view-images/solution-structure.png)

Пользовательский элемент управления `CameraPreview` отрисовывается с помощью зависящих от платформы классов отрисовщика, которые являются производными от класса `ViewRenderer` в iOS и UWP, а также класса `FrameLayout` в Android. Это приводит к тому, что каждый пользовательский элемент управления `CameraPreview` отрисовывается с помощью зависящих от платформы элементов управления, как показано на следующих снимках экрана:

![CameraPreview на каждой платформе](view-images/screenshots.png)

Класс `ViewRenderer` предоставляет метод `OnElementChanged`, который вызывается при создании пользовательского элемента управления Xamarin.Forms для отрисовки соответствующего собственного элемента управления. Этот метод принимает параметр `ElementChangedEventArgs`, содержащий свойства `OldElement` и `NewElement`. Эти свойства представляют элемент Xamarin.Forms, к которому *был* присоединен отрисовщик, и элемент Xamarin.Forms, к которому *присоединен* отрисовщик, соответственно. В примере приложения свойство `OldElement` будет иметь значение `null`, а свойство `NewElement` будет содержать ссылку на экземпляр `CameraPreview`.

Настройка и создание экземпляра собственного элемента управления выполняется в переопределенной версии метода `OnElementChanged` в классе отрисовщика для каждой платформы. Метод `SetNativeControl` должен использоваться для создания экземпляра собственного элемента управления и будет присваивать свойству `Control` ссылку на элемент управления. Кроме того, ссылку на отрисовываемый элемент управления Xamarin.Forms можно получить с помощью свойства `Element`.

В некоторых случаях метод `OnElementChanged` может вызываться несколько раз. Поэтому в целях предотвращения утечек памяти необходимо с осторожностью создавать собственный элемент управления. В следующем примере кода показан подход, используемый при создании собственного элемента управления в пользовательском отрисовщике:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null)
  {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null)
  {    
    if (Control == null)
    {
      // Instantiate the native control and assign it to the Control property with
      // the SetNativeControl method
    }
    // Configure the control and subscribe to event handlers
  }
}
```

Собственный элемент управления должен создаваться только один раз, когда свойству `Control` задано значение `null`. Кроме того, элемент управления следует создавать, настраивать и подписывать на обработчики событий только при присоединении пользовательского отрисовщика к новому элементу Xamarin.Forms. Аналогично, от всех обработчиков событий, на которые были созданы подписки, следует отписываться только при изменении элемента с подключенным отрисовщиком. Реализовав этот подход, вы сможете создать пользовательский отрисовщик, на который не влияют утечки памяти.

> [!IMPORTANT]
> Метод `SetNativeControl` должен вызываться, только если `e.NewElement` не равен `null`.

Каждый класс пользовательского отрисовщика дополняется атрибутом `ExportRenderer`, который регистрирует отрисовщик в Xamarin.Forms. Атрибут принимает два параметра — имя типа отрисовываемого пользовательского элемента управления Xamarin.Forms и имя типа пользовательского отрисовщика. Префикс `assembly` в атрибуте указывает, что атрибут применяется ко всей сборке.

В следующих разделах рассматривается реализация класса пользовательского отрисовщика для каждой платформы.

### <a name="creating-the-custom-renderer-on-ios"></a>Создание пользовательского отрисовщика в iOS

В следующем примере кода показан пользовательский отрисовщик для платформы iOS:

```csharp
[assembly: ExportRenderer (typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, UICameraPreview>
    {
        UICameraPreview uiCameraPreview;

        protected override void OnElementChanged (ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null) {
                // Unsubscribe
                uiCameraPreview.Tapped -= OnCameraPreviewTapped;
            }
            if (e.NewElement != null) {
                if (Control == null) {
                  uiCameraPreview = new UICameraPreview (e.NewElement.Camera);
                  SetNativeControl (uiCameraPreview);
                }
                // Subscribe
                uiCameraPreview.Tapped += OnCameraPreviewTapped;
            }
        }

        void OnCameraPreviewTapped (object sender, EventArgs e)
        {
            if (uiCameraPreview.IsPreviewing) {
                uiCameraPreview.CaptureSession.StopRunning ();
                uiCameraPreview.IsPreviewing = false;
            } else {
                uiCameraPreview.CaptureSession.StartRunning ();
                uiCameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

При условии, что свойство `Control` имеет значение `null`, вызывается метод `SetNativeControl`, чтобы создать экземпляр нового элемента управления `UICameraPreview` и назначить ссылку на него свойству `Control`. Элемент управления `UICameraPreview` является пользовательским элементом управления, относящимся к конкретной платформе, который использует API`AVCapture` для предоставления потока предварительного просмотра с камеры. Он предоставляет событие `Tapped`, которое обрабатывается методом `OnCameraPreviewTapped`, для остановки и запуска предварительного просмотра видео при касании. Подписка на событие `Tapped` добавляется только при присоединении настраиваемого отрисовщика к новому элементу Xamarin.Forms, а удаляется только при изменении элемента, к которому присоединен отрисовщик.

### <a name="creating-the-custom-renderer-on-android"></a>Создание пользовательского отрисовщика в Android

В следующем примере кода показан быстрый отрисовщик для платформы Android:

```csharp
[assembly: ExportRenderer(typeof(CustomRenderer.CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPreviewRenderer : FrameLayout, IVisualElementRenderer, IViewRenderer
    {
        // ...
        CameraPreview element;
        VisualElementTracker visualElementTracker;
        VisualElementRenderer visualElementRenderer;
        FragmentManager fragmentManager;
        CameraFragment cameraFragment;

        FragmentManager FragmentManager => fragmentManager ??= Context.GetFragmentManager();

        public event EventHandler<VisualElementChangedEventArgs> ElementChanged;
        public event EventHandler<PropertyChangedEventArgs> ElementPropertyChanged;

        CameraPreview Element
        {
            get => element;
            set
            {
                if (element == value)
                {
                    return;
                }

                var oldElement = element;
                element = value;
                OnElementChanged(new ElementChangedEventArgs<CameraPreview>(oldElement, element));
            }
        }

        public CameraPreviewRenderer(Context context) : base(context)
        {
            visualElementRenderer = new VisualElementRenderer(this);
        }

        void OnElementChanged(ElementChangedEventArgs<CameraPreview> e)
        {
            CameraFragment newFragment = null;

            if (e.OldElement != null)
            {
                e.OldElement.PropertyChanged -= OnElementPropertyChanged;
                cameraFragment.Dispose();
            }
            if (e.NewElement != null)
            {
                this.EnsureId();

                e.NewElement.PropertyChanged += OnElementPropertyChanged;

                ElevationHelper.SetElevation(this, e.NewElement);
                newFragment = new CameraFragment { Element = element };
            }

            FragmentManager.BeginTransaction()
                .Replace(Id, cameraFragment = newFragment, "camera")
                .Commit();
            ElementChanged?.Invoke(this, new VisualElementChangedEventArgs(e.OldElement, e.NewElement));
        }

        async void OnElementPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            ElementPropertyChanged?.Invoke(this, e);

            switch (e.PropertyName)
            {
                case "Width":
                    await cameraFragment.RetrieveCameraDevice();
                    break;
            }
        }       
        // ...
    }
}
```

В этом примере метод `OnElementChanged` создает объект `CameraFragment`, если настраиваемый отрисовщик подключен к новому элементу Xamarin.Forms. Тип `CameraFragment` — это пользовательский класс, который использует API `Camera2` для предоставления потока предварительного просмотра с камеры. Объект `CameraFragment` удаляется, когда элемент Xamarin.Forms, к которому присоединен отрисовщик, меняется.

### <a name="creating-the-custom-renderer-on-uwp"></a>Создание пользовательского отрисовщика на универсальной платформе Windows

В следующем примере кода показан пользовательский отрисовщик для платформы UWP:

```csharp
[assembly: ExportRenderer(typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.UWP
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, Windows.UI.Xaml.Controls.CaptureElement>
    {
        ...
        CaptureElement _captureElement;
        bool _isPreviewing;

        protected override void OnElementChanged(ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
                Tapped -= OnCameraPreviewTapped;
                ...
            }
            if (e.NewElement != null)
            {
                if (Control == null)
                {
                  ...
                  _captureElement = new CaptureElement();
                  _captureElement.Stretch = Stretch.UniformToFill;

                  SetupCamera();
                  SetNativeControl(_captureElement);
                }
                // Subscribe
                Tapped += OnCameraPreviewTapped;
            }
        }

        async void OnCameraPreviewTapped(object sender, TappedRoutedEventArgs e)
        {
            if (_isPreviewing)
            {
                await StopPreviewAsync();
            }
            else
            {
                await StartPreviewAsync();
            }
        }
        ...
    }
}
```

При условии, что свойство `Control` имеет значение `null`, создается экземпляр `CaptureElement` и вызывается метод `SetupCamera`, использующий API `MediaCapture` для предоставления видеопотока предварительного просмотра с камеры. Вызывается метод `SetNativeControl`, чтобы назначить ссылку на экземпляр `CaptureElement` свойству `Control`. Элемент управления `CaptureElement` предоставляет событие `Tapped`, которое обрабатывается методом `OnCameraPreviewTapped`, для остановки и запуска предварительного просмотра видео при касании. Подписка на событие `Tapped` добавляется только при присоединении настраиваемого отрисовщика к новому элементу Xamarin.Forms, а удаляется только при изменении элемента, к которому присоединен отрисовщик.

> [!NOTE]
> Важно остановить и удалить объекты, которые предоставляют доступ к камере в приложении UWP. Невыполнение этого требования может помешать работе других приложений, которые пытаются получить доступ к камере устройства. Дополнительные сведения см. в статье об [отображении предварительного изображения с камеры](/windows/uwp/audio-video-camera/simple-camera-preview-access/).

## <a name="summary"></a>Сводка

В этой статье показано, как создать настраиваемый отрисовщик для пользовательского элемента управления Xamarin.Forms, который используется для отображения видеопотока для предварительного просмотра с камеры устройства. Пользовательские элементы управления пользовательского интерфейса Xamarin.Forms должны быть производными от класса [`View`](xref:Xamarin.Forms.View), который используется для размещения макетов и элементов управления на экране.

## <a name="related-links"></a>Связанные ссылки

- [CustomRendererView (пример)](/samples/xamarin/xamarin-forms-samples/customrenderers-view)
