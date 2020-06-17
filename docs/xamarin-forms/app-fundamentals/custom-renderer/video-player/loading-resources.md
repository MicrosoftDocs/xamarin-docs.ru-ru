---
title: "Загрузка видеоресурсов приложения" description: "В этой статье объясняется, как загружать видео, хранящееся в виде ресурсов приложения, в приложении видеопроигрывателя с помощью Xamarin.Forms".
ms.prod: xamarin ms.assetid: F75BD540-9354-4C17-A119-57F3DEC66D54 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 12.02.2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="loading-application-resource-videos"></a>Загрузка видеоресурсов приложения

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

Пользовательские отрисовщики для представления `VideoPlayer` могут воспроизводить видеофайлы, внедренные в проекты для отдельных платформ в виде ресурсов приложения. Однако текущая версия `VideoPlayer` не может обращаться к ресурсам, внедренным в библиотеку .NET Standard.

Для загрузки таких ресурсов нужно создать экземпляр `ResourceVideoSource`, указав в качестве значения свойства `Path` имя файла ресурса (или папку и имя файла). Кроме того, для обращения к ресурсу можно вызвать статический метод `VideoSource.FromResource`. Затем присвойте объект `ResourceVideoSource` свойству `Source` представления `VideoPlayer`.

## <a name="storing-the-video-files"></a>Хранение видеофайлов

Видеофайлы хранятся в проектах для разных платформ по-разному.

### <a name="ios-video-resources"></a>Видеоресурсы в iOS

В проекте iOS видео можно хранить в папке **Resources** или во вложенной в нее папке. Видеофайл должен иметь `Build Action` типа `BundleResource`. Присвойте свойству `Path` объекта `ResourceVideoSource` имя файла, например **MyFile.mp4** для файла в папке **Resources** или **MyFolder/MyFile.mp4**, где **MyFolder** — это папка, вложенная в папку **Resources**.

В решении **VideoPlayerDemos** проект **VideoPlayerDemos.iOS** содержит в папке **Resources** вложенную папку **Videos**, в которой есть файл с именем **iOSApiVideo.mp4**. Это короткое видео, в котором показано, как найти документацию по классу iOS `AVPlayerViewController` на веб-сайте Xamarin.

### <a name="android-video-resources"></a>Видеоресурсы в Android

В проекте Android видео должно храниться в папке **raw**, вложенной в папку **Resources**. В папке **raw** не может быть вложенных папок. Видеофайл должен иметь `Build Action` типа `AndroidResource`. Присвойте свойству `Path` объекта `ResourceVideoSource` имя файла, например **MyFile.mp4**.

Проект **VideoPlayerDemos.Android** содержит папку **raw**, вложенную в папку **Resources** и содержащую файл с именем **AndroidApiVideo.mp4**.

### <a name="uwp-video-resources"></a>Видеоресурсы в UWP

В проекте универсальной платформы Windows видео можно хранить в любой папке. Файл должен иметь `Build Action` типа `Content`. Присвойте свойству `Path` объекта `ResourceVideoSource` имя папки и файла, например **MyFolder/MyVideo.mp4**.

Проект **VideoPlayerDemos.UWP** содержит папку **Videos** с файлом **UWPApiVideo.mp4**.

## <a name="loading-the-video-files"></a>Загрузка видеофайлов

В методе `SetSource` класса отрисовщика каждой платформы есть код для загрузки видеофайлов, хранящихся в виде ресурсов.

### <a name="ios-resource-loading"></a>Загрузка ресурсов в iOS

Версия `VideoPlayerRenderer` для iOS загружает ресурсы с помощью метода `GetUrlForResource` класса `NSBundle`. Полный путь необходимо разделить на имя файла, расширение и каталог. Для разделения пути к файлу на эти части в коде используется класс `Path` из пространства имен .NET `System.IO`:

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
            else if (Element.Source is ResourceVideoSource)
            {
                string path = (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    string directory = Path.GetDirectoryName(path);
                    string filename = Path.GetFileNameWithoutExtension(path);
                    string extension = Path.GetExtension(path).Substring(1);
                    NSUrl url = NSBundle.MainBundle.GetUrlForResource(filename, extension, directory);
                    asset = AVAsset.FromUrl(url);
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="android-resource-loading"></a>Загрузка ресурсов в Android

В Android представление `VideoPlayerRenderer` использует имя файла и имя пакета для формирования объекта `Uri`. Имя пакета — это имя приложения (в данном случае **VideoPlayerDemos.Android**), которое можно получить из статического свойства `Context.PackageName`. Полученный объект `Uri` затем передается в метод `SetVideoURI` класса `VideoView`:

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
            else if (Element.Source is ResourceVideoSource)
            {
                string package = Context.PackageName;
                string path = (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    string filename = Path.GetFileNameWithoutExtension(path).ToLowerInvariant();
                    string uri = "android.resource://" + package + "/raw/" + filename;
                    videoView.SetVideoURI(Android.Net.Uri.Parse(uri));
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="uwp-resource-loading"></a>Загрузка ресурсов в UWP

`VideoPlayerRenderer` в UWP формирует объект `Uri` для пути и присваивает ему значение свойства `Source` объекта `MediaElement`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        async void SetSource()
        {
            bool hasSetSource = false;
            ···
            else if (Element.Source is ResourceVideoSource)
            {
                string path = "ms-appx:///" + (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    Control.Source = new Uri(path);
                    hasSetSource = true;
                }
            }
        }
        ···
    }
}
```

## <a name="playing-the-resource-file"></a>Воспроизведение файла ресурса

На странице **Play Video Resource** (Воспроизведение видеоресурса) в решении **VideoPlayerDemos** видеофайл для каждой платформы указывается с помощью класса `OnPlatform`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayVideoResourcePage"
             Title="Play Video Resource">
    <video:VideoPlayer>
        <video:VideoPlayer.Source>
            <video:ResourceVideoSource>
                <video:ResourceVideoSource.Path>
                    <OnPlatform x:TypeArguments="x:String">
                        <On Platform="iOS" Value="Videos/iOSApiVideo.mp4" />
                        <On Platform="Android" Value="AndroidApiVideo.mp4" />
                        <On Platform="UWP" Value="Videos/UWPApiVideo.mp4" />
                    </OnPlatform>
                </video:ResourceVideoSource.Path>
            </video:ResourceVideoSource>
        </video:VideoPlayer.Source>
    </video:VideoPlayer>
</ContentPage>
```

Если ресурс iOS хранится в папке **Resources**, а ресурс UWP — в корневой папке проекта, для всех платформ можно использовать одно и то же имя файла. В таком случае это имя можно напрямую присвоить свойству `Source` представления `VideoPlayer`.

Так выглядит работающая страница:

[![Воспроизведение видеоматериала](loading-resources-images/playvideoresource-small.png "Воспроизведение видеоматериала")](loading-resources-images/playvideoresource-large.png#lightbox "Воспроизведение видеоматериала")

Теперь вы знаете, как [загружать видео по универсальному коду ресурса (URI) в Интернете](web-videos.md) и как воспроизводить внедренные ресурсы. Вы также можете [загружать видео из видеотеки устройства](accessing-library.md).

## <a name="related-links"></a>Связанные ссылки

- [Демоверсии видеопроигрывателя (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
