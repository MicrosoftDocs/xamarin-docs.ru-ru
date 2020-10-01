---
title: Доступ к видеотеке устройства
description: В этой статье объясняется, как получить доступ к библиотеке видео устройства в приложении видеопроигрывателя с помощью Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 364C1D43-EAAE-45B9-BE24-0DA5AE74C4D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ff9f44ad790f3acda1e855f3308942bb702e66ee
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91563904"
---
# <a name="accessing-the-devices-video-library"></a>Доступ к видеотеке устройства

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

Большинство современных мобильных устройств и настольных компьютеров позволяют записывать видео с помощью камеры. Создаваемые пользователем видеоролики сохраняются в виде файлов на устройстве. Эти файлы можно извлечь из библиотеки изображений и воспроизвести с помощью класса `VideoPlayer`, как и любые другие видеоролики.

## <a name="the-photo-picker-dependency-service"></a>Служба зависимостей для средства выбора фотографий

Каждая из платформ позволяет разрешить пользователю выбрать фото или видео из библиотеки изображений устройства. Первым шагом при воспроизведении видео из библиотеки изображений устройства является создание службы зависимостей, которая вызывает средство выбора изображений в каждой платформе. Описанная ниже служба зависимостей очень похожа на описанную в статье [**Выбор фотографии из библиотеки рисунков**](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md), за исключением того, что средство выбора видео возвращает имя файла, а не объект `Stream`.

Проект библиотеки .NET Standard определяет интерфейс `IVideoPicker` для службы зависимостей:

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPicker
    {
        Task<string> GetVideoFileAsync();
    }
}
```

Каждая из платформ содержит класс `VideoPicker`, реализующий этот интерфейс.

### <a name="the-ios-video-picker"></a>Средство выбора видео на платформе iOS

В iOS `VideoPicker` для доступа к библиотеке изображений используется iOS [`UIImagePickerController`](xref:UIKit.UIImagePickerController), при этом он должен быть ограничен видеороликами (для этого используется термин "movies") в свойстве iOS `MediaType`. Обратите внимание, что `VideoPicker` явным образом реализует интерфейс `IVideoPicker`. Следует отметить и атрибут`Dependency`, определяющий этот класс как службу зависимостей. Ниже приведены два требования, которые позволяют Xamarin.Forms найти службу зависимостей в проекте платформы:

```csharp
using System;
using System.Threading.Tasks;
using UIKit;
using Xamarin.Forms;

[assembly: Dependency(typeof(FormsVideoLibrary.iOS.VideoPicker))]

namespace FormsVideoLibrary.iOS
{
    public class VideoPicker : IVideoPicker
    {
        TaskCompletionSource<string> taskCompletionSource;
        UIImagePickerController imagePicker;

        public Task<string> GetVideoFileAsync()
        {
            // Create and define UIImagePickerController
            imagePicker = new UIImagePickerController
            {
                SourceType = UIImagePickerControllerSourceType.SavedPhotosAlbum,
                MediaTypes = new string[] { "public.movie" }
            };

            // Set event handlers
            imagePicker.FinishedPickingMedia += OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled += OnImagePickerCancelled;

            // Present UIImagePickerController;
            UIWindow window = UIApplication.SharedApplication.KeyWindow;
            var viewController = window.RootViewController;
            viewController.PresentViewController(imagePicker, true, null);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<string>();
            return taskCompletionSource.Task;
        }

        void OnImagePickerFinishedPickingMedia(object sender, UIImagePickerMediaPickedEventArgs args)
        {
            if (args.MediaType == "public.movie")
            {
                taskCompletionSource.SetResult(args.MediaUrl.AbsoluteString);
            }
            else
            {
                taskCompletionSource.SetResult(null);
            }
            imagePicker.DismissModalViewController(true);
        }

        void OnImagePickerCancelled(object sender, EventArgs args)
        {
            taskCompletionSource.SetResult(null);
            imagePicker.DismissModalViewController(true);
        }
    }
}
```

### <a name="the-android-video-picker"></a>Средство выбора видео на платформе Android

Реализация `IVideoPicker` на платформе Android требует метод обратного вызова, который является частью действий приложения. Поэтому класс `MainActivity` определяет два свойства, поле и метод обратного вызова:

```csharp
namespace VideoPlayerDemos.Droid
{
    ···
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            Current = this;
            ···
        }

        // Field, properties, and method for Video Picker
        public static MainActivity Current { private set; get; }

        public static readonly int PickImageId = 1000;

        public TaskCompletionSource<string> PickImageTaskCompletionSource { set; get; }

        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);

            if (requestCode == PickImageId)
            {
                if ((resultCode == Result.Ok) && (data != null))
                {
                    // Set the filename as the completion of the Task
                    PickImageTaskCompletionSource.SetResult(data.DataString);
                }
                else
                {
                    PickImageTaskCompletionSource.SetResult(null);
                }
            }
        }
    }
}
```

Метод `OnCreate` в `MainActivity` сохраняет свой собственный экземпляр в статическом свойстве `Current`. Это позволяет реализации `IVideoPicker` получить экземпляр `MainActivity` для запуска средства выбора **, Выбор видео**:

```csharp
using System;
using System.Threading.Tasks;
using Android.Content;
using Xamarin.Forms;

// Need application's MainActivity
using VideoPlayerDemos.Droid;

[assembly: Dependency(typeof(FormsVideoLibrary.Droid.VideoPicker))]

namespace FormsVideoLibrary.Droid
{
    public class VideoPicker : IVideoPicker
    {
        public Task<string> GetVideoFileAsync()
        {
            // Define the Intent for getting images
            Intent intent = new Intent();
            intent.SetType("video/*");
            intent.SetAction(Intent.ActionGetContent);

            // Get the MainActivity instance
            MainActivity activity = MainActivity.Current;

            // Start the picture-picker activity (resumes in MainActivity.cs)
            activity.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Video"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            activity.PickImageTaskCompletionSource = new TaskCompletionSource<string>();

            // Return Task object
            return activity.PickImageTaskCompletionSource.Task;
        }
    }
}
```

Дополнением к объекту `MainActivity` является только код в решении [**VideoPlayerDemos**](/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos), где требуется изменить обычный код приложения для поддержки классов `FormsVideoLibrary`.

### <a name="the-uwp-video-picker"></a>Средство выбора видео на платформе UWP

Реализация интерфейса `IVideoPicker` на платформе UWP использует UWP [`FileOpenPicker`](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/). Она начинает поиск файла с библиотеки рисунков и ограничивает типы файлов до MP4 и WMV (Windows Media Video):

```csharp
using System;
using System.Threading.Tasks;
using Windows.Storage;
using Windows.Storage.Pickers;
using Xamarin.Forms;

[assembly: Dependency(typeof(FormsVideoLibrary.UWP.VideoPicker))]

namespace FormsVideoLibrary.UWP
{
    public class VideoPicker : IVideoPicker
    {
        public async Task<string> GetVideoFileAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary
            };

            openPicker.FileTypeFilter.Add(".wmv");
            openPicker.FileTypeFilter.Add(".mp4");

            // Get a file and return the path
            StorageFile storageFile = await openPicker.PickSingleFileAsync();
            return storageFile?.Path;
        }
    }
}
```

## <a name="invoking-the-dependency-service"></a>Вызов службы зависимостей

На странице **Play Library Video** (Воспроизведение видео из библиотеки) программы [**VideoPlayerDemos**](/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos) показано, как использовать службу зависимостей средства выбора видео. Файл XAML содержит экземпляр `VideoPlayer` и `Button` с меткой **Show Video Library** (Показать видеотеку):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayLibraryVideoPage"
             Title="Play Library Video">
    <StackLayout>
        <video:VideoPlayer x:Name="videoPlayer"
                           VerticalOptions="FillAndExpand" />

        <Button Text="Show Video Library"
                Margin="10"
                HorizontalOptions="Center"
                Clicked="OnShowVideoLibraryClicked" />
    </StackLayout>
</ContentPage>
```

Файл кода программной части содержит обработчик `Clicked` для `Button`. Для вызова службы зависимостей требуется вызов `DependencyService.Get`, чтобы получить реализацию интерфейса `IVideoPicker` в проекте платформы. Затем для этого экземпляра вызывается метод `GetVideoFileAsync`:

```csharp
namespace VideoPlayerDemos
{
    public partial class PlayLibraryVideoPage : ContentPage
    {
        public PlayLibraryVideoPage()
        {
            InitializeComponent();
        }

       async void OnShowVideoLibraryClicked(object sender, EventArgs args)
        {
            Button btn = (Button)sender;
            btn.IsEnabled = false;

            string filename = await DependencyService.Get<IVideoPicker>().GetVideoFileAsync();

            if (!String.IsNullOrWhiteSpace(filename))
            {
                videoPlayer.Source = new FileVideoSource
                {
                    File = filename
                };
            }

            btn.IsEnabled = true;
        }
    }
}
```

Обработчик `Clicked` использует это имя файла, чтобы создать объект `FileVideoSource` и присвоить ему значение свойства `Source` класса `VideoPlayer`.

Каждый из классов `VideoPlayerRenderer` содержит в своем методе `SetSource` код для объектов типа `FileVideoSource`. Они показаны ниже:

### <a name="handling-ios-files"></a>Обработка файлов на платформе iOS

Версия `VideoPlayerRenderer` для iOS обрабатывает объекты `FileVideoSource` с помощью статического метода `Asset.FromUrl` с использованием заданного имени файла. При этом создается объект `AVAsset`, представляющий файл в библиотеке изображений устройства:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void SetSource()
        {
            AVAsset asset = null;
            ···
            else if (Element.Source is FileVideoSource)
            {
                string uri = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    asset = AVAsset.FromUrl(new NSUrl(uri));
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="handling-android-files"></a>Обработка файлов на платформе Android

При обработке объектов типа `FileVideoSource` реализация `VideoPlayerRenderer` для Android использует метод `SetVideoPath` объекта `VideoView` для указания файла в библиотеке изображений устройства:

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
            ···
            else if (Element.Source is FileVideoSource)
            {
                string filename = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(filename))
                {
                    videoView.SetVideoPath(filename);
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="handling-uwp-files"></a>Обработка файлов на платформе UWP

При обработке объектов типа `FileVideoSource` реализация метода `SetSource` для UWP должна создать объект `StorageFile`, открыть этот файл для чтения и передать объект потока в метод `SetSource` объекта `MediaElement`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
        async void SetSource()
        {
            bool hasSetSource = false;
            ···
            else if (Element.Source is FileVideoSource)
            {
                // Code requires Pictures Library in Package.appxmanifest Capabilities to be enabled
                string filename = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(filename))
                {
                    StorageFile storageFile = await StorageFile.GetFileFromPathAsync(filename);
                    IRandomAccessStreamWithContentType stream = await storageFile.OpenReadAsync();
                    Control.SetSource(stream, storageFile.ContentType);
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

Для каждой платформы воспроизведение видео начинается почти сразу после задания источника видео, так как файл находится на устройстве и скачивать его не требуется.

## <a name="related-links"></a>Связанные ссылки

- [Демоверсии видеопроигрывателя (пример)](/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
- [Выбор фотографии в библиотеке рисунков](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)