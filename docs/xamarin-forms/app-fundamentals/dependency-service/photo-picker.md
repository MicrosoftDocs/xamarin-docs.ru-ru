---
title: Выбор фотографии в библиотеке рисунков
description: В этой статье объясняется, как использовать класс DependencyService в Xamarin.Forms для выбора фотографии в библиотеке рисунков на телефоне.
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9f999d56fbf178be160e91756643c127d574b090
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84197558"
---
# <a name="picking-a-photo-from-the-picture-library"></a>Выбор фотографии в библиотеке рисунков

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice/)

Эта статья описывает создание приложения, позволяющего пользователю выбрать фотографию в библиотеке рисунков на телефоне. Так как Xamarin.Forms не содержит эту функцию, требуется использовать [`DependencyService`](xref:Xamarin.Forms.DependencyService) для доступа к собственным API на каждой платформе.

## <a name="creating-the-interface"></a>Создание интерфейса

Сначала создайте в общем коде интерфейс для реализации нужной функциональности. В случае с приложением для выбора фотографий требуется всего один метод. В примере кода он определяется в интерфейсе [`IPhotoPickerService`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos/Services/IPhotoPickerService.cs) в библиотеке .NET Standard:

```csharp
namespace DependencyServiceDemos
{
    public interface IPhotoPickerService
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

Метод `GetImageStreamAsync` определен как асинхронный, так как должен быстро возвращать данные, но он не может возвратить объект `Stream` для выбранной фотографии, пока пользователь не просмотрит библиотеку рисунков и не выберет один из них.

Этот интерфейс реализуется на всех платформах с помощью кода, учитывающего особенности конкретной платформы.

## <a name="ios-implementation"></a>Реализация в iOS

Реализация интерфейса `IPhotoPickerService` в iOS использует [`UIImagePickerController`](xref:UIKit.UIImagePickerController), как описано в разделе [**Выбор фотографии из коллекции**](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery) и [примере кода](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery).

Реализация для iOS содержится в классе [`PhotoPickerService`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos.iOS/Services/PhotoPickerService.cs) в проекте iOS примера кода. Чтобы сделать этот класс видимым для диспетчера`DependencyService`, нужно определить класс с атрибутом [`assembly`] типа `Dependency`, при этом он должен быть открытым и явно реализовывать интерфейс `IPhotoPickerService`:

```csharp
[assembly: Dependency (typeof (PhotoPickerService))]
namespace DependencyServiceDemos.iOS
{
    public class PhotoPickerService : IPhotoPickerService
    {
        TaskCompletionSource<Stream> taskCompletionSource;
        UIImagePickerController imagePicker;

        public Task<Stream> GetImageStreamAsync()
        {
            // Create and define UIImagePickerController
            imagePicker = new UIImagePickerController
            {
                SourceType = UIImagePickerControllerSourceType.PhotoLibrary,
                MediaTypes = UIImagePickerController.AvailableMediaTypes(UIImagePickerControllerSourceType.PhotoLibrary)
            };

            // Set event handlers
            imagePicker.FinishedPickingMedia += OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled += OnImagePickerCancelled;

            // Present UIImagePickerController;
            UIWindow window = UIApplication.SharedApplication.KeyWindow;
            var viewController = window.RootViewController;
            viewController.PresentViewController(imagePicker, true, null);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<Stream>();
            return taskCompletionSource.Task;
        }
        ...
    }
}

```

Метод `GetImageStreamAsync` создает `UIImagePickerController` и инициализирует его, чтобы выбрать изображения из библиотеки фотографий. Требуется два обработчика событий: один, когда пользователь выбирает фотографию, и другой — когда пользователь отменяет отображение библиотеки фотографий. Затем метод `PresentViewController` отображает библиотеку фотографий для пользователя.

На этом этапе метод `GetImageStreamAsync` должен возвращать объект `Task<Stream>` вызывающему его коду. Эта задача выполняется только после того, как пользователь закончил взаимодействие с библиотекой фотографий и вызван один из обработчиков событий. Для таких ситуаций крайне важен класс [`TaskCompletionSource`](https://msdn.microsoft.com/library/dd449174(v=vs.110).aspx). Он предоставляет объект `Task` соответствующего универсального типа для возврата из метода `GetImageStreamAsync`, а позднее этому классу можно подать сигнал о выполнении задачи.

Обработчик событий `FinishedPickingMedia` вызывается, когда пользователь выбирает изображение. Однако обработчик предоставляет объект `UIImage` и `Task` должен возвратить объект .NET `Stream`. Это осуществляется в два этапа: сначала объект `UIImage` преобразуется в PNG- или JPEG-файл в памяти (хранится в объекте `NSData`), а затем объект `NSData` преобразуется в объект .NET `Stream`. Вызов метода `SetResult` объекта `TaskCompletionSource` завершает задачу, предоставляя объект `Stream`:

```csharp
namespace DependencyServiceDemos.iOS
{
    public class PhotoPickerService : IPhotoPickerService
    {
        TaskCompletionSource<Stream> taskCompletionSource;
        UIImagePickerController imagePicker;
        ...
        void OnImagePickerFinishedPickingMedia(object sender, UIImagePickerMediaPickedEventArgs args)
        {
            UIImage image = args.EditedImage ?? args.OriginalImage;

            if (image != null)
            {
                // Convert UIImage to .NET Stream object
                NSData data;
                if (args.ReferenceUrl.PathExtension.Equals("PNG") || args.ReferenceUrl.PathExtension.Equals("png"))
                {
                    data = image.AsPNG();
                }
                else
                {
                    data = image.AsJPEG(1);
                }
                Stream stream = data.AsStream();

                UnregisterEventHandlers();

                // Set the Stream as the completion of the Task
                taskCompletionSource.SetResult(stream);
            }
            else
            {
                UnregisterEventHandlers();
                taskCompletionSource.SetResult(null);
            }
            imagePicker.DismissModalViewController(true);
        }

        void OnImagePickerCancelled(object sender, EventArgs args)
        {
            UnregisterEventHandlers();
            taskCompletionSource.SetResult(null);
            imagePicker.DismissModalViewController(true);
        }

        void UnregisterEventHandlers()
        {
            imagePicker.FinishedPickingMedia -= OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled -= OnImagePickerCancelled;
        }
    }
}
```

Приложению iOS требуется разрешение пользователя для обращения к библиотеке фотографий на телефоне. Добавьте следующий код в раздел `dict` файла Info.plist:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Picture Picker uses photo library</string>
```

## <a name="android-implementation"></a>Реализация в Android

Реализация для Android использует методики, описанные в разделе [**Выбор изображения**](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image) и [примере кода](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image). Однако метод, вызываемый при выборе пользователем изображения из библиотеки рисунков, представляет собой переопределение `OnActivityResult` в классе, производном от `Activity`. Поэтому нормальный класс [`MainActivity`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos.Android/MainActivity.cs) в проекте Android был дополнен полем, свойством и переопределением метода `OnActivityResult`:

```csharp
public class MainActivity : FormsAppCompatActivity
{
    internal static MainActivity Instance { get; private set; }  

    protected override void OnCreate(Bundle savedInstanceState)
    {
        // ...
        Instance = this;
    }
    // ...
    // Field, property, and method for Picture Picker
    public static readonly int PickImageId = 1000;

    public TaskCompletionSource<Stream> PickImageTaskCompletionSource { set; get; }

    protected override void OnActivityResult(int requestCode, Result resultCode, Intent intent)
    {
        base.OnActivityResult(requestCode, resultCode, intent);

        if (requestCode == PickImageId)
        {
            if ((resultCode == Result.Ok) && (intent != null))
            {
                Android.Net.Uri uri = intent.Data;
                Stream stream = ContentResolver.OpenInputStream(uri);

                // Set the Stream as the completion of the Task
                PickImageTaskCompletionSource.SetResult(stream);
            }
            else
            {
                PickImageTaskCompletionSource.SetResult(null);
            }
        }
    }
}
```

Переопределение `OnActivityResult` указывает выбранный файл изображения с помощью объекта Android `Uri`, но его можно преобразовать в объект .NET `Stream`, вызвав метод `OpenInputStream` объекта `ContentResolver`, который был получен из свойства `ContentResolver` действия.

Как и реализация для iOS, реализация для Android использует `TaskCompletionSource`, чтобы сигнализировать о завершении задачи. Этот объект `TaskCompletionSource` определен в виде открытого свойства в классе `MainActivity`. Это позволяет ссылаться на свойство в классе [`PhotoPickerService`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos.Android/Services/PhotoPickerService.cs) проекта Android. Это класс с методом `GetImageStreamAsync`:

```csharp
[assembly: Dependency(typeof(PhotoPickerService))]
namespace DependencyServiceDemos.Droid
{
    public class PhotoPickerService : IPhotoPickerService
    {
        public Task<Stream> GetImageStreamAsync()
        {
            // Define the Intent for getting images
            Intent intent = new Intent();
            intent.SetType("image/*");
            intent.SetAction(Intent.ActionGetContent);

            // Start the picture-picker activity (resumes in MainActivity.cs)
            MainActivity.Instance.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Picture"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            MainActivity.Instance.PickImageTaskCompletionSource = new TaskCompletionSource<Stream>();

            // Return Task object
            return MainActivity.Instance.PickImageTaskCompletionSource.Task;
        }
    }
}
```

Этот метод обращается к классу `MainActivity` по нескольким причинам: за свойством `Instance`, за полем `PickImageId`, за свойством `TaskCompletionSource` и для вызова `StartActivityForResult`. Этот метод определен классом `FormsAppCompatActivity`, который является базовым классом для `MainActivity`.

## <a name="uwp-implementation"></a>Реализация на универсальной платформе Windows

В отличие от реализаций для iOS и Android, реализации средства выбора фотографий для универсальной платформы Windows не требуется класс `TaskCompletionSource`. Класс [`PhotoPickerService`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos.UWP/Services/PhotoPickerService.cs) использует класс [`FileOpenPicker`](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/) для получения доступа к библиотеке фотографий. Так как метод `PickSingleFileAsync` объекта `FileOpenPicker` сам является асинхронным, метод `GetImageStreamAsync` может просто использовать `await` с этим методом (и другими асинхронными методами) и возвращать объект `Stream`:

```csharp
[assembly: Dependency(typeof(PhotoPickerService))]
namespace DependencyServiceDemos.UWP
{
    public class PhotoPickerService : IPhotoPickerService
    {
        public async Task<Stream> GetImageStreamAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary,
            };

            openPicker.FileTypeFilter.Add(".jpg");
            openPicker.FileTypeFilter.Add(".jpeg");
            openPicker.FileTypeFilter.Add(".png");

            // Get a file and return a Stream
            StorageFile storageFile = await openPicker.PickSingleFileAsync();

            if (storageFile == null)
            {
                return null;
            }

            IRandomAccessStreamWithContentType raStream = await storageFile.OpenReadAsync();
            return raStream.AsStreamForRead();
        }
    }
}
```

## <a name="implementing-in-shared-code"></a>Реализация в общем коде

Теперь, когда интерфейс реализован для каждой платформы, общий код в библиотеке .NET Standard может им воспользоваться.

Пользовательский интерфейс содержит элемент [`Button`](xref:Xamarin.Forms.Button), который можно щелкнуть, чтобы выбрать фотографию:

```xaml
<Button Text="Pick Photo"
        Clicked="OnPickPhotoButtonClicked" />
```

Обработчик событий `Clicked` использует класс `DependencyService` для вызова `GetImageStreamAsync`. После этого выполняется вызов к проекту платформы. Если метод возвращает объект `Stream`, обработчик задает свойство `Source` объекта `image` данным `Stream`:

```csharp
async void OnPickPhotoButtonClicked(object sender, EventArgs e)
{
    (sender as Button).IsEnabled = false;

    Stream stream = await DependencyService.Get<IPhotoPickerService>().GetImageStreamAsync();
    if (stream != null)
    {
        image.Source = ImageSource.FromStream(() => stream);
    }

    (sender as Button).IsEnabled = true;
}
```

## <a name="related-links"></a>Связанные ссылки

- [DependencyService (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice/)
- [Выбор фотографии из коллекции (iOS)](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)
- [Выбор изображения (Android)](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)
