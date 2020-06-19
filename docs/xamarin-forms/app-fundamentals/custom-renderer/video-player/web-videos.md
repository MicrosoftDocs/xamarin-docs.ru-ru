---
title: Воспроизведение видео через Интернет
description: В этой статье объясняется, как воспроизводить видео через Интернет в приложении видеопроигрывателя с помощью Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 75781A10-865D-4BA8-8D6B-E3DA012922BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b4269b768d6dd9a0eadbbe557f701f8a43b74c99
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84133683"
---
# <a name="playing-a-web-video"></a>Воспроизведение видео через Интернет

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

Класс `VideoPlayer` определяет свойство `Source`, используемое для указания источника видеофайла, а также свойство `AutoPlay`. Свойству `AutoPlay` задано значение по умолчанию `true`. Это значит, что видеовоспроизведение должно начинаться автоматически после установки свойства `Source`.

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Source property
        public static readonly BindableProperty SourceProperty =
            BindableProperty.Create(nameof(Source), typeof(VideoSource), typeof(VideoPlayer), null);

        [TypeConverter(typeof(VideoSourceConverter))]
        public VideoSource Source
        {
            set { SetValue(SourceProperty, value); }
            get { return (VideoSource)GetValue(SourceProperty); }
        }

        // AutoPlay property
        public static readonly BindableProperty AutoPlayProperty =
            BindableProperty.Create(nameof(AutoPlay), typeof(bool), typeof(VideoPlayer), true);

        public bool AutoPlay
        {
            set { SetValue(AutoPlayProperty, value); }
            get { return (bool)GetValue(AutoPlayProperty); }
        }
        ···
    }
}
```

Свойство `Source` имеет тип `VideoSource`, который создан по шаблону абстрактного класса Xamarin.Forms [`ImageSource`](xref:Xamarin.Forms.ImageSource) и его трех производных — [`UriImageSource`](xref:Xamarin.Forms.UriImageSource), [`FileImageSource`](xref:Xamarin.Forms.FileImageSource) и [`StreamImageSource`](xref:Xamarin.Forms.StreamImageSource) Однако для класса `VideoPlayer` отсутствует возможность потоковой передачи, так как iOS и Android не поддерживают воспроизведение видео из потока.

## <a name="video-sources"></a>Источники видео

Абстрактный класс `VideoSource` состоит только из трех статических методов, которые создают три класса, производные от `VideoSource`.

```csharp
namespace FormsVideoLibrary
{
    [TypeConverter(typeof(VideoSourceConverter))]
    public abstract class VideoSource : Element
    {
        public static VideoSource FromUri(string uri)
        {
            return new UriVideoSource() { Uri = uri };
        }

        public static VideoSource FromFile(string file)
        {
            return new FileVideoSource() { File = file };
        }

        public static VideoSource FromResource(string path)
        {
            return new ResourceVideoSource() { Path = path };
        }
    }
}
```

Класс `UriVideoSource` используется для указания скачиваемого видеофайла с URI. Он определяет единственное свойство типа `string`.

```csharp
namespace FormsVideoLibrary
{
    public class UriVideoSource : VideoSource
    {
        public static readonly BindableProperty UriProperty =
            BindableProperty.Create(nameof(Uri), typeof(string), typeof(UriVideoSource));

        public string Uri
        {
            set { SetValue(UriProperty, value); }
            get { return (string)GetValue(UriProperty); }
        }
    }
}
```

Обработка объектов типа `UriVideoSource` описывается ниже.

Класс `ResourceVideoSource` используется для доступа к видеофайлам, хранящимся в виде внедренных ресурсов в приложении платформы, для которого также задано свойство `string`.

```csharp
namespace FormsVideoLibrary
{
    public class ResourceVideoSource : VideoSource
    {
        public static readonly BindableProperty PathProperty =
            BindableProperty.Create(nameof(Path), typeof(string), typeof(ResourceVideoSource));

        public string Path
        {
            set { SetValue(PathProperty, value); }
            get { return (string)GetValue(PathProperty); }
        }
    }
}
```

Обработка объектов типа `ResourceVideoSource` описывается в статье [Загрузка видеоресурсов приложения](loading-resources.md). Класс `VideoPlayer` не поддерживает загрузку видеофайлов, хранящихся в виде ресурсов в библиотеке .NET Standard.

Класс `FileVideoSource` используется для доступа к видеофайлам из видеотеки устройства. Единственное свойство также имеет тип `string`.

```csharp
namespace FormsVideoLibrary
{
    public class FileVideoSource : VideoSource
    {
        public static readonly BindableProperty FileProperty =
                  BindableProperty.Create(nameof(File), typeof(string), typeof(FileVideoSource));

        public string File
        {
            set { SetValue(FileProperty, value); }
            get { return (string)GetValue(FileProperty); }
        }
    }
}
```

Обработка объектов типа `FileVideoSource` описывается в статье [Доступ к видеотеке устройства](accessing-library.md).

Класс `VideoSource` содержит атрибут `TypeConverter`, который ссылается на класс `VideoSourceConverter`.

```csharp
namespace FormsVideoLibrary
{
    [TypeConverter(typeof(VideoSourceConverter))]
    public abstract class VideoSource : Element
    {
        ···
    }
}
```

Этот преобразователь типов вызывается, если свойству `Source` в XAML задано строковое значение. Класс `VideoSourceConverter` выглядит так:

```csharp
namespace FormsVideoLibrary
{
    public class VideoSourceConverter : TypeConverter
    {
        public override object ConvertFromInvariantString(string value)
        {
            if (!String.IsNullOrWhiteSpace(value))
            {
                Uri uri;
                return Uri.TryCreate(value, UriKind.Absolute, out uri) && uri.Scheme != "file" ?
                                VideoSource.FromUri(value) : VideoSource.FromResource(value);
            }

            throw new InvalidOperationException("Cannot convert null or whitespace to ImageSource");
        }
    }
}
```

Метод `ConvertFromInvariantString` пытается преобразовать строку в объект `Uri`. Если операция выполняется успешно и схема имеет значение `file:`, то метод возвращает `UriVideoSource`. В противном случае он возвращает `ResourceVideoSource`.

## <a name="setting-the-video-source"></a>Задание источника видео

Вся остальная логика, связанная с источниками видео, реализуется в отрисовщиках для отдельных платформ. В следующих разделах показано, каким образом отрисовщики для платформ воспроизводят видео, если в качестве значения свойства `Source` задан объект `UriVideoSource`.

### <a name="the-ios-video-source"></a>Источник видео на платформе iOS

В установке источника видео для видеопроигрывателя участвуют два раздела класса `VideoPlayerRenderer`. Когда Xamarin.Forms сначала создает объект типа `VideoPlayer`, вызывается метод `OnElementChanged`, свойству `NewElement` объекта аргументов которого задано значение этого `VideoPlayer`. Метод `OnElementChanged` вызывает метод `SetSource`.

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
                SetSource();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            ···
        }
        ···
    }
}
```

Позже при изменении свойства `Source` вызывается метод `OnElementPropertyChanged` со свойством `PropertyName`, имеющим значение Source, и снова вызывается метод `SetSource`.

Чтобы воспроизвести видеофайл в iOS, сначала создается объект типа [`AVAsset`](xref:AVFoundation.AVAsset) для инкапсуляции видеофайла. Этот объект используется для создания класса [`AVPlayerItem`](xref:AVFoundation.AVPlayerItem), который затем передается объекту `AVPlayer`. Вот как метод `SetSource` обрабатывает свойство `Source`, если оно имеет тип `UriVideoSource`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        AVPlayerItem playerItem;
        ···
        void SetSource()
        {
            AVAsset asset = null;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    asset = AVAsset.FromUrl(new NSUrl(uri));
                }
            }
            ···
            if (asset != null)
            {
                playerItem = new AVPlayerItem(asset);
            }
            else
            {
                playerItem = null;
            }

            player.ReplaceCurrentItemWithPlayerItem(playerItem);

            if (playerItem != null && Element.AutoPlay)
            {
                player.Play();
            }
        }
        ···
    }
}
```

У свойства `AutoPlay` нет аналога в классах видео на платформе iOS, поэтому оно проверяется в конце метода `SetSource` для вызова метода `Play` для `AVPlayer` объекта.

В некоторых случаях видео продолжает воспроизводиться после возврата со страницы с классом `VideoPlayer` на домашнюю страницу. Для остановки видео в переопределении метода `Dispose` также задается метод `ReplaceCurrentItemWithPlayerItem`.

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void Dispose(bool disposing)
        {
            base.Dispose(disposing);

            if (player != null)
            {
                player.ReplaceCurrentItemWithPlayerItem(null);
            }
        }
        ···
    }
}
```

### <a name="the-android-video-source"></a>Источник видео на платформе Android

Класс `VideoPlayerRenderer` в Android должен задать источник видео для проигрывателя сначала при создании класса `VideoPlayer` и позже при изменении свойства `Source`.

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetSource();
                ···
            }
        }
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            ···
        }
        ···
    }
}
```

Метод `SetSource` обрабатывает объекты типа `UriVideoSource` путем вызова метода `SetVideoUri` в классе `VideoView` с объектом Android `Uri`, созданным из строкового URI. Здесь класс `Uri` имеет полное имя, позволяющее отличать его от класса `Uri` .NET.

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void SetSource()
        {
            isPrepared = false;
            bool hasSetSource = false;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    videoView.SetVideoURI(Android.Net.Uri.Parse(uri));
                    hasSetSource = true;
                }
            }
            ···

            if (hasSetSource && Element.AutoPlay)
            {
                videoView.Start();
            }
        }
        ···
    }
}
```

У класса `VideoView` в Android нет соответствующего свойства `AutoPlay`, поэтому метод `Start` вызывается в том случае, если задано новое видео.

Отрисовщики в iOS and Android ведут себя по-разному, если свойству `Source` класса `VideoPlayer` задано значение `null` или если свойству `Uri` объекта `UriVideoSource` задано значение `null` либо оно содержит пустую строку. Если в данный момент видеопроигрыватель iOS воспроизводит видео и свойству `Source` задано значение `null` (или строка имеет значение `null` либо является пустой), вызывается метод `ReplaceCurrentItemWithPlayerItem` со значением `null`. Происходит замена текущего видео, и воспроизведение останавливается.

В Android подобный механизм не поддерживается. Если свойству `Source` задано значение `null`, метод `SetSource` просто игнорирует его, а воспроизведение текущего видео будет продолжаться.

### <a name="the-uwp-video-source"></a>Источник видео на платформе UWP

Класс `MediaElement` на платформе UWP определяет свойство `AutoPlay`, которое обрабатывается в отрисовщике как любое другое.

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
                SetSource();
                SetAutoPlay();
                ···    
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            else if (args.PropertyName == VideoPlayer.AutoPlayProperty.PropertyName)
            {
                SetAutoPlay();
            }
            ···
        }
        ···
    }
}
```

Свойство `SetSource` обрабатывает объект `UriVideoSource` путем установки для свойства `Source` класса `MediaElement` значения .NET `Uri`. Или задает значение `null`, если свойству `Source` класса `VideoPlayer` задано значение `null`.

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        async void SetSource()
        {
            bool hasSetSource = false;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    Control.Source = new Uri(uri);
                    hasSetSource = true;
                }
            }
            ···
            if (!hasSetSource)
            {
                Control.Source = null;
            }
        }

        void SetAutoPlay()
        {
            Control.AutoPlay = Element.AutoPlay;
        }
        ···
    }
}
```

## <a name="setting-a-url-source"></a>Задание URL-адреса источника

Благодаря реализации этих свойств в трех отрисовщиках, можно воспроизводить видео из URL-адреса источника. Страница **Воспроизведение видео в Интернете** в программе [**VideoPlayDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos) определяется в следующем XAML-файле:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayWebVideoPage"
             Title="Play Web Video">

    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

</ContentPage>
```

Класс `VideoSourceConverter` преобразует строку в `UriVideoSource`. При переходе на страницу **Воспроизведение видео через Интернет** начинается загрузка видео, а после скачивания и буферизации достаточного количества данных запускается его воспроизведение. Продолжительность видео около 10 минут.

[![Воспроизведение видео через Интернет](web-videos-images/playwebvideo-small.png "Воспроизведение видео через Интернет")](web-videos-images/playwebvideo-large.png#lightbox "Воспроизведение видео через Интернет")

Если элементы управления транспортировкой не используются, они исчезают с экрана устройства на каждой платформе. Для их восстановления нужно коснуться видео.

Чтобы видео не запускалось автоматически, свойству `AutoPlay` следует задать значение `false`.

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AutoPlay="false" />
```

Для запуска видео потребуется нажать кнопку **Воспроизведение**.

Аналогичным образом, чтобы отключить отображение элементов управления транспортировкой, задайте свойству `AreTransportControlsEnabled` значение `false`.

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AreTransportControlsEnabled="False" />
```

Если оба свойства имеют значение `false`, воспроизведение видео не начнется и его запуск будет невозможен. Необходимо вызвать `Play` из файла кода программной части или создать собственные элементы управления транспортировкой, как описано в статье [Пользовательские элементы управления транспортировкой видео](custom-transport.md).

Файл **App.xaml** содержит ресурсы для двух дополнительных видео.

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.App">
    <Application.Resources>
        <ResourceDictionary>

            <video:UriVideoSource x:Key="ElephantsDream"
                                  Uri="https://archive.org/download/ElephantsDream/ed_hd_512kb.mp4" />

            <video:UriVideoSource x:Key="BigBuckBunny"
                                  Uri="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

            <video:UriVideoSource x:Key="Sintel"
                                  Uri="https://archive.org/download/Sintel/sintel-2048-stereo_512kb.mp4" />

        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Чтобы ссылаться на один из этих фильмов, можно заменить явно заданный URL-адрес в файле **PlayWebVideo.xaml** расширением разметки `StaticResource`. В этом случае создания объекта `UriVideoSource` класс `VideoSourceConverter` не требуется.

```xaml
<video:VideoPlayer Source="{StaticResource ElephantsDream}" />
```

Кроме того, можно задать свойство `Source` из видеофайла в элементе управления `ListView`, как описано в статье [Привязка источников видео к проигрывателю](source-bindings.md).

## <a name="related-links"></a>Связанные ссылки

- [Демоверсии видеопроигрывателя (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
