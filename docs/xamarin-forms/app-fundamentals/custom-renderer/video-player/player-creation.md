---
title: Создание видеопроигрывателей платформы
description: В этой статье объясняется, как внедрить пользовательский отрисовщик видеопроигрывателя на каждой платформе с помощью Xamarin.Forms.
ms.prod: xamarin
ms.assetid: EEE2FB9B-EB73-4A3F-A859-7A1D4808E149
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7de5f0ec67a4735b8157b539b80e193fa67256b1
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91563488"
---
# <a name="creating-the-platform-video-players"></a>Создание видеопроигрывателей платформы

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

Решение [**VideoPlayerDemos**](/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos) содержит весь необходимый код для внедрения видеопроигрывателя для Xamarin.Forms. Оно также включает ряд страниц, где демонстрируется использование видеопроигрывателя в приложении. Весь код `VideoPlayer` и отрисовщики платформ находятся в папках проекта с именем `FormsVideoLibrary` и используют пространство имен `FormsVideoLibrary`. Благодаря этому вы сможете легко копировать файлы в собственное приложение и ссылаться на классы.

## <a name="the-video-player"></a>Видеопроигрыватель

Класс [`VideoPlayer`](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos/FormsVideoLibrary/VideoPlayer.cs) входит в библиотеку .NET Standard **VideoPlayerDemos**, которая является общей для платформ. Он является производным от `View`.

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
    }
}
```

Члены этого класса (и интерфейс `IVideoPlayerController`) описываются в следующих статьях.

Каждая платформа содержит класс `VideoPlayerRenderer`, имеющий характерный для платформы код, который предназначен для реализации видеопроигрывателя. Основная задача этого отрисовщика заключается в создании видеопроигрывателя для данной платформы.

### <a name="the-ios-player-view-controller"></a>Контроллер представления проигрывателя в iOS

При реализации видеопроигрывателя в iOS используется несколько классов. Сначала приложение создает класс [`AVPlayerViewController`](xref:AVKit.AVPlayerViewController), а затем в качестве значения свойства [`Player`](xref:AVKit.AVPlayerViewController.Player*) задает объекта типа [`AVPlayer`](xref:AVFoundation.AVPlayer). При назначении видеопроигрывателю источника видео требуются дополнительные классы.

Как все отрисовщики, класс [`VideoPlayerRenderer`](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos.iOS/FormsVideoLibrary/VideoPlayerRenderer.csVideoPlayerRenderer.cs) в iOS содержит атрибут `ExportRenderer`, определяющий представление `VideoPlayer` с помощью отрисовщика.

```csharp
using System;
using System.ComponentModel;
using System.IO;

using AVFoundation;
using AVKit;
using CoreMedia;
using Foundation;
using UIKit;

using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.iOS.VideoPlayerRenderer))]

namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
    }
}
```

Обычно отрисовщик, который задает элемент управления платформы, является производным от класса [`ViewRenderer<View, NativeView>`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs), где `View` — это производная Xamarin.Forms `View` (в этом случае `VideoPlayer`), а `NativeView` — производная `UIView` в iOS для класса отрисовщика. В этом отрисовщике универсальному аргументу просто задано значение `UIView`. Причины вы узнаете в ближайшее время.

Если отрисовщик основан на производной `UIViewController` (такой как эта), то класс должен переопределить свойство `ViewController` и вернуть контроллер представления, в данном случае `AVPlayerViewController`. В этом заключается цель поля `_playerViewController`.

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        AVPlayerItem playerItem;
        AVPlayerViewController _playerViewController;       // solely for ViewController property

        public override UIViewController ViewController => _playerViewController;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    // Create AVPlayerViewController
                    _playerViewController = new AVPlayerViewController();

                    // Set Player property to AVPlayer
                    player = new AVPlayer();
                    _playerViewController.Player = player;

                    // Use the View from the controller as the native control
                    SetNativeControl(_playerViewController.View);
                }
                ···
            }
        }
        ···
    }
}
```

Основная обязанность переопределения метода `OnElementChanged` состоит в том, чтобы проверить, задано ли свойству `Control` значение `null`, и если это так, создать элемент управления платформы и передать его методу `SetNativeControl`. В этом случае объект доступен только из свойства `View` класса `AVPlayerViewController`. Производная `UIView` является закрытым классом с именем `AVPlayerView`, но так как класс закрытый, его невозможно явно указать в качестве второго универсального аргумента для `ViewRenderer`.

Обычно свойство `Control` класса отрисовщика затем ссылается на производную `UIView`, используемую для реализации отрисовщика, но в этом случае свойство `Control` нигде не применяется.

### <a name="the-android-video-view"></a>Представление видео в Android

Отрисовщик Android для `VideoPlayer` основан на классе [`VideoView`](xref:Android.Widget.VideoView) в Android. Однако при отдельном использовании `VideoView` для воспроизведения видео в приложении Xamarin.Forms видео заполняет область, отведенную для `VideoPlayer`, без сохранения правильных пропорций. Поэтому (как вы вскоре увидите) класс `VideoView` стал дочерним по отношению к классу `RelativeLayout` в Android. Производная `using` определяет `ARelativeLayout`, чтобы отличать его от Xamarin.Forms `RelativeLayout`, и он представляет собой второй универсальный аргумент в `ViewRenderer`.

```csharp
using System;
using System.ComponentModel;
using System.IO;

using Android.Content;
using Android.Media;
using Android.Widget;
using ARelativeLayout = Android.Widget.RelativeLayout;

using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.Droid.VideoPlayerRenderer))]

namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        public VideoPlayerRenderer(Context context) : base(context)
        {
        }
        ···
    }
}
```

Начиная с Xamarin.Forms 2.5 отрисовщики Android должны содержать конструктор с аргументом `Context`.

Переопределение метода `OnElementChanged` создает классы `VideoView` и `RelativeLayout` и задает параметры макета для класса `VideoView`, чтобы расположить его в центре класса `RelativeLayout`.

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        MediaController mediaController;    // Used to display transport controls
        bool isPrepared;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    // Save the VideoView for future reference
                    videoView = new VideoView(Context);

                    // Put the VideoView in a RelativeLayout
                    ARelativeLayout relativeLayout = new ARelativeLayout(Context);
                    relativeLayout.AddView(videoView);

                    // Center the VideoView in the RelativeLayout
                    ARelativeLayout.LayoutParams layoutParams =
                        new ARelativeLayout.LayoutParams(LayoutParams.MatchParent, LayoutParams.MatchParent);
                    layoutParams.AddRule(LayoutRules.CenterInParent);
                    videoView.LayoutParameters = layoutParams;

                    // Handle a VideoView event
                    videoView.Prepared += OnVideoViewPrepared;

                    // Use the RelativeLayout as the native control
                    SetNativeControl(relativeLayout);
                }
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null && videoView != null)
            {
                videoView.Prepared -= OnVideoViewPrepared;
            }
            base.Dispose(disposing);
        }

        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            isPrepared = true;
            ···
        }
        ···
    }
}
```

Обработчик для события `Prepared` присоединяется в этом методе и отсоединяется в методе `Dispose`. Это событие возникает, когда класс `VideoView` имеет достаточно данных для начала воспроизведения видеофайла.

### <a name="the-uwp-media-element"></a>Элемент мультимедиа на универсальной платформе Windows

[`MediaElement`](xref:Windows.UI.Xaml.Controls.MediaElement) является самым распространенным видеопроигрывателем на универсальной платформе Windows (UWP). В документации по классу `MediaElement` определено, что вместо него следует использовать класс [`MediaPlayerElement`](xref:Windows.UI.Xaml.Controls.MediaPlayerElement), если только необходимо поддерживать версии Windows 10, начиная со сборки 1607.

Переопределению метода `OnElementChanged` необходимо создать класс `MediaElement`, задать несколько обработчиков событий и передать объект `MediaElement` методу `SetNativeControl`.

```csharp
using System;
using System.ComponentModel;

using Windows.Storage;
using Windows.Storage.Streams;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.UWP.VideoPlayerRenderer))]

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
                    MediaElement mediaElement = new MediaElement();
                    SetNativeControl(mediaElement);

                    mediaElement.MediaOpened += OnMediaElementMediaOpened;
                    mediaElement.CurrentStateChanged += OnMediaElementCurrentStateChanged;
                }
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null)
            {
                Control.MediaOpened -= OnMediaElementMediaOpened;
                Control.CurrentStateChanged -= OnMediaElementCurrentStateChanged;
            }

            base.Dispose(disposing);
        }        
        ···
    }
}
```

Два обработчика событий отсоединяются в событии `Dispose` для отрисовщика.

## <a name="showing-the-transport-controls"></a>Отображение элементов управления транспортировкой

Все видеопроигрыватели в составе платформ поддерживают заданный по умолчанию набор элементов управления транспортировкой, в который входят кнопки для воспроизведения и приостановки, строка для указания текущей позиции в видео и для перемещения в новую позицию.

Класс `VideoPlayer` определяет свойство с именем `AreTransportControlsEnabled` и задает значение по умолчанию `true`.

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // AreTransportControlsEnabled property
        public static readonly BindableProperty AreTransportControlsEnabledProperty =
            BindableProperty.Create(nameof(AreTransportControlsEnabled), typeof(bool), typeof(VideoPlayer), true);

        public bool AreTransportControlsEnabled
        {
            set { SetValue(AreTransportControlsEnabledProperty, value); }
            get { return (bool)GetValue(AreTransportControlsEnabledProperty); }
        }
        ···
    }
}
```

Несмотря на то, что у этого свойства есть оба метода доступа `set` и `get`, отрисовщик должен обрабатывать только те случаи, когда свойству задано значение. Метод доступа `get` просто возвращает текущее значение свойства.

Свойства, такие как `AreTransportControlsEnabled`, обрабатываются в отрисовщиках платформы двумя способами.

- Первый — при создании элемента `VideoPlayer` в Xamarin.Forms. Это указывается в переопределении метода `OnElementChanged` отрисовщика, если свойству `NewElement` не задано значение `null`. В настоящее время отрисовщик может устанавливать собственный видеопроигрыватель на основе начального значения свойства, как определено в классе `VideoPlayer`.

- Если позже свойство в классе `VideoPlayer` меняется, то в отрисовщике вызывается метод `OnElementPropertyChanged`. Благодаря этому отрисовщик может обновить видеопроигрыватель платформы с учетом нового параметра свойства.

В следующих разделах описывается обработка свойства `AreTransportControlsEnabled` на каждой платформе.

### <a name="ios-playback-controls"></a>Элементы управления воспроизведением в iOS

Для управления отображением элементов управления транспортировкой служит свойство класса `AVPlayerViewController` в iOS — [`ShowsPlaybackControls`](xref:AVKit.AVPlayerViewController.ShowsPlaybackControls*). Это свойство задается в классе `VideoViewRenderer` iOS следующим образом:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        AVPlayerViewController _playerViewController;       // solely for ViewController property

        public override UIViewController ViewController => _playerViewController;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            ((AVPlayerViewController)ViewController).ShowsPlaybackControls = Element.AreTransportControlsEnabled;
        }
        ···
    }
}
```

Свойство `Element` отрисовщика ссылается на класс `VideoPlayer`.

### <a name="the-android-media-controller"></a>Контроллер мультимедиа Android

Для отображения элементов управления транспортировкой в Android необходимо создать объект [`MediaController`](xref:Android.Widget.MediaController) и связать его с объектом `VideoView`. Механизмы показаны в методе `SetAreTransportControlsEnabled`:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        MediaController mediaController;    // Used to display transport controls
        bool isPrepared;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            if (Element.AreTransportControlsEnabled)
            {
                mediaController = new MediaController(Context);
                mediaController.SetMediaPlayer(videoView);
                videoView.SetMediaController(mediaController);
            }
            else
            {
                videoView.SetMediaController(null);

                if (mediaController != null)
                {
                    mediaController.SetMediaPlayer(null);
                    mediaController = null;
                }
            }
        }
        ···
    }
}
```

### <a name="the-uwp-transport-controls-property"></a>Свойство элементов управления транспортировкой на универсальной платформе Windows

Класс `MediaElement` на платформе UWP определяет свойство с именем [`AreTransportControlsEnabled`](xref:Windows.UI.Xaml.Controls.MediaElement.AreTransportControlsEnabled*) таким образом, что свойство задается из одноименного свойства `VideoPlayer`.

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
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            Control.AreTransportControlsEnabled = Element.AreTransportControlsEnabled;
        }
        ···
    }
}
```

Чтобы начать воспроизведение видео, требуется еще одно свойство. Это важное свойство `Source`, которое ссылается на видеофайл. Реализация свойства `Source` описывается в следующей статье — [Воспроизведение видео через Интернет](web-videos.md).

## <a name="related-links"></a>Связанные ссылки

- [Демоверсии видеопроигрывателя (пример)](/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)