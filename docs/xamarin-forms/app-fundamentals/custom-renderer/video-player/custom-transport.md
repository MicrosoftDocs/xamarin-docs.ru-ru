---
title: "Пользовательские элементы управления транспортировкой видео" description: "В этой статье объясняется, как внедрить пользовательские элементы управления транспортировкой в приложение видеопроигрывателя с помощью Xamarin.Forms".
ms.prod: xamarin ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 12.02.2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="custom-video-transport-controls"></a>Пользовательские элементы управления транспортировкой видео

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

К элементам управления транспортировкой в видеопроигрывателе относятся кнопки, которые выполняют функции **воспроизведения**, **паузы** и **остановки**. Эти кнопки обычно определяются по знакомым значкам, а не тексту. Как правило, функции **Воспроизведение** и **Пауза** объединены в одну кнопку.

По умолчанию `VideoPlayer` отображает элементы управления транспортировкой, поддерживаемые каждой платформы. Если свойству `AreTransportControlsEnabled` задается значение `false`, эти элементы управления подавляются. Затем `VideoPlayer` можно управлять программным способом или указать собственные элементы управления транспортировкой.

## <a name="the-play-pause-and-stop-methods"></a>Методы Play, Pause и Stop

Класс `VideoPlayer` определяет три метода — `Play`, `Pause` и `Stop`, — которые реализуются путем запуска событий.

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        public event EventHandler PlayRequested;

        public void Play()
        {
            PlayRequested?.Invoke(this, EventArgs.Empty);
        }

        public event EventHandler PauseRequested;

        public void Pause()
        {
            PauseRequested?.Invoke(this, EventArgs.Empty);
        }

        public event EventHandler StopRequested;

        public void Stop()
        {
            StopRequested?.Invoke(this, EventArgs.Empty);
        }
    }
}
```

Обработчики событий для этих событий задаются классом `VideoPlayerRenderer` на каждой платформе, как показано ниже.

### <a name="ios-transport-implementations"></a>Реализации транспортировки на платформе iOS

Версия класса `VideoPlayerRenderer` для iOS использует метод `OnElementChanged`, чтобы задать обработчики для этих трех событий, если свойству `NewElement` не задано значение `null`, и отсоединяет обработчики событий, если свойству `OldElement` не задано значение `null`.

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        // Event handlers to implement methods
        void OnPlayRequested(object sender, EventArgs args)
        {
            player.Play();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            player.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            player.Pause();
            player.Seek(new CMTime(0, 1));
        }
    }
}
```

Обработчики событий реализуются путем вызова методов в объекте `AVPlayer`. Поскольку метод `Stop` для `AVPlayer` отсутствует, он имитируется путем приостановки видео и перемещения позиции в начало.

### <a name="android-transport-implementations"></a>Реализации транспортировки на платформе Android

Реализация в Android аналогична реализации в iOS. Обработчики для трех функций задаются, если свойству `NewElement` не задано значение `null`, и отсоединяются, если свойству `OldElement` не задано значение `null`.

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        void OnPlayRequested(object sender, EventArgs args)
        {
            videoView.Start();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            videoView.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            videoView.StopPlayback();
        }
    }
}
```

Три функции вызывают методы, определенные классом `VideoView`.

### <a name="uwp-transport-implementations"></a>Реализации транспортировки на платформе UWP

Реализация трех функций транспортировки на платформе UWP практически аналогична реализациям в iOS и Android:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        // Event handlers to implement methods
        void OnPlayRequested(object sender, EventArgs args)
        {
            Control.Play();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            Control.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            Control.Stop();
        }
    }
}
```

## <a name="the-video-player-status"></a>Состояние видеопроигрывателя

Реализации функций **Воспроизведение**, **Пауза** и **Остановка** недостаточно для поддержки элементов управления транспортировкой. Часто команды **Воспроизведение** и **Пауза** реализуются с помощью одной кнопки, которая изменяет свой внешний вид, указывая, в каком режиме на данный момент находится видео — воспроизведение или приостановка. Кроме того, кнопки даже не следует включать, пока не загружено видео.

Эти требования означают, что видеопроигрывателю необходимо сделать доступным текущее состояние, указывающее его актуальный режим — воспроизводит видео, приостановлен или пока не готов к воспроизведению видео. (Каждая платформа также поддерживает свойства, указывающие на возможность приостановки или перемещения видео в новую позицию, но эти свойства применяются для потоковой передачи видео, а не видеофайлов, поэтому они не поддерживаются в описанном здесь `VideoPlayer`.)

Проект **VideoPlayerDemos** содержит перечисление `VideoStatus` с тремя элементами.

```csharp
namespace FormsVideoLibrary
{
    public enum VideoStatus
    {
        NotReady,
        Playing,
        Paused
    }
}
```

Класс `VideoPlayer` определяет доступное только для чтения привязываемое свойство `Status` типа `VideoStatus`. Это свойство определено как доступное только для чтения, так как его следует задавать только из отрисовщика платформы.

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Status read-only property
        private static readonly BindablePropertyKey StatusPropertyKey =
            BindableProperty.CreateReadOnly(nameof(Status), typeof(VideoStatus), typeof(VideoPlayer), VideoStatus.NotReady);

        public static readonly BindableProperty StatusProperty = StatusPropertyKey.BindableProperty;

        public VideoStatus Status
        {
            get { return (VideoStatus)GetValue(StatusProperty); }
        }

        VideoStatus IVideoPlayerController.Status
        {
            set { SetValue(StatusPropertyKey, value); }
            get { return Status; }
        }
        ···
    }
}
```

Как правило, чтобы доступное только для чтения привязываемое свойство можно было задать в классе, оно должно иметь закрытый метод доступа `set` для свойства `Status`. Однако для производной `View`, поддерживаемой отрисовщиком, свойство должно быть задано вне класса и только отрисовщиком платформы.

Поэтому другое свойство определяется с именем `IVideoPlayerController.Status`. Это явная реализация интерфейса, которая стала возможной благодаря интерфейсу `IVideoPlayerController`, реализуемому классом `VideoPlayer`.

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPlayerController
    {
        VideoStatus Status { set; get; }

        TimeSpan Duration { set; get; }
    }
}
```

Эта процедура аналогична тому, как элемент управления [`WebView`](xref:Xamarin.Forms.WebView) использует интерфейс [`IWebViewController`](xref:Xamarin.Forms.IWebViewController) для реализации свойств `CanGoBack` и `CanGoForward`. (Чтобы узнать все более подробно, см. исходный код для [`WebView`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/WebView.cs) и его отрисовщиков.)

Это позволяет классу, внешнему по отношению к `VideoPlayer`, задавать свойство `Status` путем ссылки на интерфейс `IVideoPlayerController`. (Скоро вы увидите код.) Свойство можно также задать из других классов, но вряд ли оно будет указано случайным образом. Что еще более важно, свойство `Status` невозможно задать через привязку данных.

Чтобы помочь отрисовщикам поддерживать актуальность этого свойства `Status`, класс `VideoPlayer` определяет событие `UpdateStatus`, которое запускается каждую десятую долю секунды.

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        public event EventHandler UpdateStatus;

        public VideoPlayer()
        {
            Device.StartTimer(TimeSpan.FromMilliseconds(100), () =>
            {
                UpdateStatus?.Invoke(this, EventArgs.Empty);
                return true;
            });
        }
        ···
    }
}
```

### <a name="the-ios-status-setting"></a>Параметр состояния на платформе iOS

Класс iOS `VideoPlayerRenderer` задает обработчик для события `UpdateStatus` (и отсоединяет этот обработчик при отсутствии базового элемента `VideoPlayer`) и использует его, чтобы задать свойство `Status`.

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.UpdateStatus += OnUpdateStatus;
                ···
            }

            if (args.OldElement != null)
            {
                args.OldElement.UpdateStatus -= OnUpdateStatus;
                ···
            }
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            VideoStatus videoStatus = VideoStatus.NotReady;

            switch (player.Status)
            {
                case AVPlayerStatus.ReadyToPlay:
                    switch (player.TimeControlStatus)
                    {
                        case AVPlayerTimeControlStatus.Playing:
                            videoStatus = VideoStatus.Playing;
                            break;

                        case AVPlayerTimeControlStatus.Paused:
                            videoStatus = VideoStatus.Paused;
                            break;
                    }
                    break;
                }
            }

            ((IVideoPlayerController)Element).Status = videoStatus;
            ···
        }
        ···
    }
}
```

Необходимо обратиться к двум свойствам `AVPlayer`: свойству [`Status`](xref:AVFoundation.AVPlayer.Status*) типа `AVPlayerStatus` и свойству [`TimeControlStatus`](xref:AVFoundation.AVPlayer.TimeControlStatus*) типа `AVPlayerTimeControlStatus`. Обратите внимание, что для задания свойства `Status` свойство `Element` (которое является `VideoPlayer`) должно быть приведено к типу `IVideoPlayerController`.

### <a name="the-android-status-setting"></a>Параметр состояния на платформе Android

Свойство [`IsPlaying`](xref:Android.Widget.VideoView.IsPlaying) класса Android `VideoView` является логическим, которое указывает только состояние видео — воспроизведение или приостановка. Чтобы определить, что `VideoView` пока не может ни воспроизводить, ни приостанавливать видео, необходимо обработать событие `Prepared` класса `VideoView`. Эти два обработчика задаются в методе `OnElementChanged` и отсоединяются во время переопределения метода `Dispose`.

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        bool isPrepared;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    ···
                    videoView.Prepared += OnVideoViewPrepared;
                    ···
                }
                ···
                args.NewElement.UpdateStatus += OnUpdateStatus;
                ···
            }

            if (args.OldElement != null)
            {
                args.OldElement.UpdateStatus -= OnUpdateStatus;
                ···
            }

        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null && videoView != null)
            {
                videoView.Prepared -= OnVideoViewPrepared;
            }
            if (Element != null)
            {
                Element.UpdateStatus -= OnUpdateStatus;
            }

            base.Dispose(disposing);
        }
        ···
    }
}
```

Чтобы задать свойство `Status`, обработчик `UpdateStatus` использует поле `isPrepared` (заданное в обработчике `Prepared`) и свойство `IsPlaying`.

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        bool isPrepared;
        ···
        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            isPrepared = true;
            ···
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            VideoStatus status = VideoStatus.NotReady;

            if (isPrepared)
            {
                status = videoView.IsPlaying ? VideoStatus.Playing : VideoStatus.Paused;
            }
            ···
        }
        ···
    }
}
```

### <a name="the-uwp-status-setting"></a>Параметр состояния на платформе UWP

Класс UWP `VideoPlayerRenderer` использует событие `UpdateStatus`, но оно не требуется для задания значения свойству `Status`. Класс `MediaElement` определяет событие [`CurrentStateChanged`](xref:Windows.UI.Xaml.Controls.MediaElement.CurrentStateChanged), которое позволяет уведомлять отрисовщик об изменении свойства [`CurrentState`](xref:Windows.UI.Xaml.Controls.MediaElement.CurrentState*). Свойство отсоединено в переопределении метода `Dispose`.

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    ···
                    mediaElement.CurrentStateChanged += OnMediaElementCurrentStateChanged;
                };
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null)
            {
                ···
                Control.CurrentStateChanged -= OnMediaElementCurrentStateChanged;
            }

            base.Dispose(disposing);
        }
        ···
    }
}
```

Свойство `CurrentState` имеет тип [`MediaElementState`](/uwp/api/windows.ui.xaml.media.mediaelementstate) и легко вводится в `VideoStatus`.

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        void OnMediaElementCurrentStateChanged(object sender, RoutedEventArgs args)
        {
            VideoStatus videoStatus = VideoStatus.NotReady;

            switch (Control.CurrentState)
            {
                case MediaElementState.Playing:
                    videoStatus = VideoStatus.Playing;
                    break;

                case MediaElementState.Paused:
                case MediaElementState.Stopped:
                    videoStatus = VideoStatus.Paused;
                    break;
            }

            ((IVideoPlayerController)Element).Status = videoStatus;
        }
        ···
    }
}
```

## <a name="play-pause-and-stop-buttons"></a>Кнопки "Воспроизведение", "Пауза" и "Остановка"

Использование символов Юникода для символических изображений **воспроизведения**, **паузы** и **остановки** является проблематичным. Три специальных символа, которые могут подойти для этой цели, можно найти в разделе [Разнообразные технические символы](https://unicode-table.com/en/blocks/miscellaneous-technical/) стандарта Юникод. Эти особые значения приведены ниже.

- 0x23F5 (черный треугольник, указывающий вправо) или &#x23F5; для кнопки **Воспроизведение**
- 0x23F8 (двойная вертикальная черта) или &#x23F8; для кнопки **Пауза**
- 0x23F9 (черный квадрат) или &#x23F9; для кнопки **Остановка**

Независимо от того, как эти символы отображаются в браузере (разные браузеры обрабатывают их по-разному), они по-разному отображаются на платформах, поддерживаемых Xamarin.Forms. На устройствах iOS и UWP символы **Пауза** и **Остановка** имеют графический вид с синим объемным фоном и белым передним планом. В Android символ выглядит по-другому — он просто синий. На платформе UWP у кодовой точки 0x23F5 для символа **воспроизведения** другой вид, и она даже не поддерживается в iOS и Android.

По этой причине кодовую точку 0x23F5 невозможно использовать для кнопки **Воспроизведение**. Рекомендуется воспользоваться следующей заменой:

- 0x25B6 (черный треугольник, указывающий вправо) или &#x25B6; для кнопки **Воспроизведение**

Она поддерживается на каждой платформе, за исключением того, что этот обычный черный треугольник не похож на объемные кнопки **Пауза** и **Остановка**. Одна из возможностей заключается в том, чтобы после кодовой точки 0x25B6 указать код варианта.

- 0x25B6, за которой следует 0xFE0F (вариант 16) или &#x25B6; &#xFE0F; для кнопки **Воспроизведение**

Именно этот способ используется в приведенной ниже разметке. В результате символ **воспроизведения** в iOS получает тот же объемный вид, что и кнопки **Пауза** и **Остановка**, но такой вариант не работает на платформах Android и UWP.

На странице **Пользовательская транспортировка** свойству **AreTransportControlsEnabled** задается значение **false**. Здесь также содержится компонент `ActivityIndicator`, отображаемый при загрузке видео, и две кнопки. Объекты `DataTrigger` используются для включения и отключения `ActivityIndicator` и кнопок, а также для выбора первой кнопки — **Воспроизведение** и **Пауза**.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.CustomTransportPage"
             Title="Custom Transport">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           AutoPlay="False"
                           AreTransportControlsEnabled="False"
                           Source="{StaticResource BigBuckBunny}" />

        <ActivityIndicator Grid.Row="0"
                           Color="Gray"
                           IsVisible="False">
            <ActivityIndicator.Triggers>
                <DataTrigger TargetType="ActivityIndicator"
                             Binding="{Binding Source={x:Reference videoPlayer},
                                               Path=Status}"
                             Value="{x:Static video:VideoStatus.NotReady}">
                    <Setter Property="IsVisible" Value="True" />
                    <Setter Property="IsRunning" Value="True" />
                </DataTrigger>
            </ActivityIndicator.Triggers>
        </ActivityIndicator>

        <StackLayout Grid.Row="1"
                     Orientation="Horizontal"
                     Margin="0, 10"
                     BindingContext="{x:Reference videoPlayer}">

            <Button Text="&#x25B6;&#xFE0F; Play"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnPlayPauseButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.Playing}">
                        <Setter Property="Text" Value="&#x23F8; Pause" />
                    </DataTrigger>

                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.NotReady}">
                        <Setter Property="IsEnabled" Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>

            <Button Text="&#x23F9; Stop"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnStopButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.NotReady}">
                        <Setter Property="IsEnabled" Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
        </StackLayout>
    </Grid>
</ContentPage>
```

Подробное описание триггеров данных см. в статье [Триггеры данных](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers).

Файл кода программной части содержит обработчики для событий кнопки `Clicked`.

```csharp
namespace VideoPlayerDemos
{
    public partial class CustomTransportPage : ContentPage
    {
        public CustomTransportPage()
        {
            InitializeComponent();
        }

        void OnPlayPauseButtonClicked(object sender, EventArgs args)
        {
            if (videoPlayer.Status == VideoStatus.Playing)
            {
                videoPlayer.Pause();
            }
            else if (videoPlayer.Status == VideoStatus.Paused)
            {
                videoPlayer.Play();
            }
        }

        void OnStopButtonClicked(object sender, EventArgs args)
        {
            videoPlayer.Stop();
        }
    }
}
```

Так как в файле **CustomTransport.xaml** атрибут `AutoPlay` имеет значение `false`, потребуется нажать активную кнопку **Воспроизведение**, чтобы начать воспроизведение. Кнопки определяются таким образом, чтобы символы Юникода, описанные выше, сопровождались своими текстовыми эквивалентами. Во время воспроизведения видео кнопки имеют согласованный внешний вид на каждой платформе.

[![Воспроизведение пользовательского транспорта](custom-transport-images/customtransportplaying-small.png "Воспроизведение пользовательского транспорта")](custom-transport-images/customtransportplaying-large.png#lightbox "Воспроизведение пользовательского транспорта")

Однако при приостановке видео кнопка **Воспроизведение** на платформах Android и UWP выглядит совершенно по-разному.

[![Приостановка пользовательского транспорта](custom-transport-images/customtransportpaused-small.png "Приостановка пользовательского транспорта")](custom-transport-images/customtransportpaused-large.png#lightbox "Приостановка пользовательского транспорта")

Чтобы обеспечить визуальное единообразие в рабочем приложении, вы, возможно, захотите использовать собственные растровые изображения для кнопок.

## <a name="related-links"></a>Связанные ссылки

- [Демоверсии видеопроигрывателя (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
