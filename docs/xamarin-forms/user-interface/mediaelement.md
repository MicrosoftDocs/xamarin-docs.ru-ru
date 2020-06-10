---
Title: " Xamarin.Forms MediaElement" Description: "в этой статье объясняется, как использовать MediaElement для воспроизведения видео и аудио в Xamarin.Forms приложении".
MS. произв. Xamarin MS. AssetID: e65f1e56-a80d-46c7-9ff4-7ae6650a3165 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 02/18/2020 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-mediaelement"></a>Xamarin.FormsMediaElement

![](~/media/shared/preview.png "This API is currently pre-release")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos/)

[`MediaElement`](xref:Xamarin.Forms.MediaElement)— Это представление для воспроизведения видео и аудио. Носители, поддерживаемые базовой платформой, можно воспроизводить из следующих источников:

- Веб-узел с использованием URI (HTTP или HTTPS).
- Ресурс, внедренный в приложение платформы, с использованием `ms-appx:///` схемы URI.
- Файлы, полученные из локальных и временных папок приложения с использованием `ms-appdata:///` схемы URI.
- Библиотека устройства.

[`MediaElement`](xref:Xamarin.Forms.MediaElement)может использовать элементы управления воспроизведением платформы, которые называются элементами управления транспортом. Однако по умолчанию они отключены и могут быть заменены собственными элементами управления транспорта. На следующих снимках экрана показано `MediaElement` Воспроизведение видео с помощью элементов управления транспортной платформы:

[![Снимок экрана элемента MediaElement: воспроизведение видео в iOS и Android](mediaelement-images/playback-controls.png "Элемент MediaElement воспроизводит видео")](mediaelement-images/playback-controls-large.png#lightbox "Элемент MediaElement воспроизводит видео")

[`MediaElement`](xref:Xamarin.Forms.MediaElement)доступна в Xamarin.Forms 4,5. Однако в настоящее время он экспериментальен и может использоваться только путем добавления следующей строки кода в файл *app.XAML.CS* :

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

> [!NOTE]
> [`MediaElement`](xref:Xamarin.Forms.MediaElement)доступен в iOS, Android, универсальная платформа Windows (UWP), macOS, Windows Presentation Foundation и Tizen.

[`MediaElement`](xref:Xamarin.Forms.MediaElement)определяет следующие свойства:

- [`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect), типа [`Aspect`](xref:Xamarin.Forms.Aspect) определяет, как будет масштабироваться носитель в соответствии с отображаемой областью. Значение по умолчанию этого свойства равно `AspectFit`.
- [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay)Тип `bool` — указывает, начнется ли воспроизведение мультимедиа автоматически при [`Source`](xref:Xamarin.Forms.MediaElement.Source) установке свойства. Значение по умолчанию этого свойства равно `true`.
- [`BufferingProgress`](xref:Xamarin.Forms.MediaElement.BufferingProgress)Тип — `double` указывает текущий ход выполнения буферизации. Значение этого свойства по умолчанию равно 0,0.
- [`CanSeek`](xref:Xamarin.Forms.MediaElement.CanSeek)Тип `bool` — указывает, можно ли переместить носитель, задав значение [`Position`](xref:Xamarin.Forms.MediaElement.Position) Свойства. Это свойство доступно только для чтения.
- [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)Тип [`MediaElementState`](xref:Xamarin.Forms.MediaElementState) — указывает текущее состояние элемента управления. Это свойство доступно только для чтения, для которого значение по умолчанию — `MediaElementState.Closed` .
- [`Duration`](xref:Xamarin.Forms.MediaElement.Duration)Тип — `TimeSpan?` указывает длительность текущего открытого носителя. Это свойство доступно только для чтения, значение по умолчанию — `null` .
- [`IsLooping`](xref:Xamarin.Forms.MediaElement.IsLooping)Тип определяет, `bool` должен ли текущий загруженный источник мультимедиа возобновить воспроизведение с начала после достижения конца. Значение по умолчанию этого свойства равно `false`.
- [`KeepScreenOn`](xref:Xamarin.Forms.MediaElement.KeepScreenOn), тип `bool` , определяет, должен ли экран устройства остаться во время воспроизведения мультимедиа. Значение по умолчанию этого свойства равно `false`.
- [`Position`](xref:Xamarin.Forms.MediaElement.Position), типа `TimeSpan` , описывает текущий ход выполнения при воспроизведении мультимедиа. Значение по умолчанию этого свойства равно `TimeSpan.Zero`.
- [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls), тип `bool` , определяет, отображаются ли элементы управления воспроизведением платформ. Значение по умолчанию этого свойства равно `false`.
- [`Source`](xref:Xamarin.Forms.MediaElement.Source)Тип [`MediaSource`](xref:Xamarin.Forms.MediaSource) — указывает источник мультимедиа, загруженного в элемент управления.
- [`VideoHeight`](xref:Xamarin.Forms.MediaElement.VideoHeight), типа `int` , указывает высоту элемента управления. Это свойство доступно только для чтения.
- [`VideoWidth`](xref:Xamarin.Forms.MediaElement.VideoWidth)Тип `int` — указывает ширину элемента управления. Это свойство доступно только для чтения.
- [`Volume`](xref:Xamarin.Forms.MediaElement.Volume), типа `double` определяет громкость носителя, которая представлена на линейном масштабировании от 0 до 1. Это свойство использует `TwoWay` привязку, а значение по умолчанию — 1.

Эти свойства, за исключением `CanSeek` свойства, поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

[`MediaElement`](xref:Xamarin.Forms.MediaElement)Класс также определяет четыре события:

- [`MediaOpened`](xref:Xamarin.Forms.MediaElement.MediaOpened)возникает при проверке и открытии потока мультимедиа.
- [`MediaEnded`](xref:Xamarin.Forms.MediaElement.MediaEnded)срабатывает, когда `MediaElement` завершает воспроизведение носителя.
- [`MediaFailed`](xref:Xamarin.Forms.MediaElement.MediaFailed)возникает при возникновении ошибки, связанной с источником мультимедиа.
- [`SeekCompleted`](xref:Xamarin.Forms.MediaElement.SeekCompleted)возникает, когда точка поиска запрошенной операции поиска готова к воспроизведению.

Кроме того, [`MediaElement`](xref:Xamarin.Forms.MediaElement) включает [`Play`](xref:Xamarin.Forms.MediaElement.Play) [`Pause`](xref:Xamarin.Forms.MediaElement.Pause) методы, и [`Stop`](xref:Xamarin.Forms.MediaElement.Stop) .

Сведения о поддерживаемых форматах мультимедиа в Android см. в статье [Поддерживаемые форматы мультимедиа](https://developer.android.com/guide/topics/media/media-formats) в Developer.Android.com. Сведения о поддерживаемых форматах мультимедиа в универсальная платформа Windows (UWP) см. в разделе [Поддерживаемые кодеки](/windows/uwp/audio-video-camera/supported-codecs).

## <a name="play-remote-media"></a>Воспроизвести Удаленный носитель

[`MediaElement`](xref:Xamarin.Forms.MediaElement)Может воспроизводить удаленные файлы мультимедиа с помощью схем URI HTTP и HTTPS. Это достигается путем присвоения [`Source`](xref:Xamarin.Forms.MediaElement.Source) свойству универсального кода ресурса (URI) файла мультимедиа:

```xaml
<MediaElement Source="https://sec.ch9.ms/ch9/5d93/a1eab4bf-3288-4faf-81c4-294402a85d93/XamarinShow_mid.mp4"
              ShowsPlaybackControls="True" />
```

По умолчанию носитель, определяемый [`Source`](xref:Xamarin.Forms.MediaElement.Source) свойством, воспроизводится сразу после открытия носителя. Чтобы отключить автоматическое воспроизведение мультимедиа, присвойте [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay) свойству значение `false` .

Элементы управления воспроизведением мультимедиа по умолчанию отключены и включаются путем присвоения [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls) свойству значения `true` . [`MediaElement`](xref:Xamarin.Forms.MediaElement)затем будет использовать элементы управления воспроизведением платформы.

## <a name="play-local-media"></a>Воспроизвести локальный носитель

Локальный носитель можно воспроизвести из следующих источников:

- Ресурс, внедренный в приложение платформы, с использованием `ms-appx:///` схемы URI.
- Файлы, полученные из локальных и временных папок приложения с использованием `ms-appdata:///` схемы URI.
- Библиотека устройства.

Дополнительные сведения об этих схемах URI см. в разделе [схемы URI](/windows/uwp/app-resources/uri-schemes).

### <a name="play-media-embedded-in-the-app-package"></a>Воспроизведение мультимедиа, внедренного в пакет приложения

[`MediaElement`](xref:Xamarin.Forms.MediaElement)Может воспроизводить файлы мультимедиа, внедренные в пакет приложения, используя `ms-appx:///` схему универсального кода ресурса (URI). Файлы мультимедиа внедряются в пакет приложения путем помещения их в проект платформы.

Хранение файла мультимедиа в проекте платформы отличается для каждой платформы:

- В iOS файлы мультимедиа должны храниться в папке **Resources** или во вложенной папке папки **Resources** . Файл мультимедиа должен иметь значение `Build Action` `BundleResource` .
- В Android файлы мультимедиа должны храниться во вложенной папке **ресурсов** с именем **RAW**. В папке **raw** не может быть вложенных папок. Файл мультимедиа должен иметь значение `Build Action` `AndroidResource` .
- В UWP файлы мультимедиа могут храниться в любой папке проекта. Файл мультимедиа должен иметь значение `BuildAction` `Content` .

Затем можно воспроизводить файлы мультимедиа, соответствующие этим критериям, с помощью `ms-appx:///` схемы URI:

```xaml
<MediaElement Source="ms-appx:///XamarinForms101UsingEmbeddedImages.mp4"
              ShowsPlaybackControls="True" />
```

При использовании привязки данных можно использовать преобразователь значений для применения этой схемы URI:

```csharp
public class VideoSourceConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value == null)
            return null;

        if (string.IsNullOrWhiteSpace(value.ToString()))
            return null;

        if (Device.RuntimePlatform == Device.UWP)
            return new Uri($"ms-appx:///Assets/{value}");
        else
            return new Uri($"ms-appx:///{value}");
    }
    // ...
}
```

`VideoSourceConverter`Затем экземпляр можно использовать для применения `ms-appx:///` схемы URI к внедренному файлу мультимедиа:

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

Дополнительные сведения о схеме URI ms-appx см. в статье [MS-Appx и ms-appx-web](/windows/uwp/app-resources/uri-schemes#ms-appx-and-ms-appx-web).

### <a name="play-media-from-the-apps-local-and-temporary-folders"></a>Воспроизведение мультимедиа из локальных и временных папок приложения

[`MediaElement`](xref:Xamarin.Forms.MediaElement)Может воспроизводить файлы мультимедиа, которые копируются в локальные или временные папки приложения с помощью `ms-appdata:///` схемы URI.

В следующем примере показано, [`Source`](xref:Xamarin.Forms.MediaElement.Source) как задать свойство для файла мультимедиа, хранящегося в папке локальных данных приложения:

```xaml
<MediaElement Source="ms-appdata:///local/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

В следующем примере показано [`Source`](xref:Xamarin.Forms.MediaElement.Source) свойство для файла мультимедиа, хранящегося в папке временных данных приложения:

```xaml
<MediaElement Source="ms-appdata:///temp/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

> [!IMPORTANT]
> Кроме воспроизведения файлов мультимедиа, хранящихся в локальных или временных папках данных приложения, UWP также может воспроизводить файлы мультимедиа, расположенные в перемещаемой папке приложения. Это можно сделать путем добавления файла мультимедиа в `ms-appdata:///roaming/` .

При использовании привязки данных можно использовать преобразователь значений для применения этой схемы URI:

```csharp
public class VideoSourceConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value == null)
            return null;

        if (string.IsNullOrWhiteSpace(value.ToString()))
            return null;

        return new Uri($"ms-appdata:///{value}");
    }
    // ...
}
```

Экземпляр `VideoSourceConverter` можно использовать для применения `ms-appdata:///` схемы универсального кода ресурса (URI) к файлу мультимедиа в папке локальных или временных данных приложения.

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

Дополнительные сведения о схеме URI MS-AppData см. в статье [MS-AppData](/windows/uwp/app-resources/uri-schemes#ms-appdata).

#### <a name="copying-a-media-file-to-the-apps-local-or-temporary-data-folder"></a>Копирование файла мультимедиа в локальную или временную папку приложения

Воспроизведение файла мультимедиа, хранящегося в папке локальных или временных данных приложения, требует, чтобы файл мультимедиа был скопирован в него приложением. Это можно сделать, например, скопировав файл мультимедиа из пакета приложения:

```csharp
// This method copies the video from the app package to the app data
// directory for your app. To copy the video to the temp directory
// for your app, comment out the first line of code, and uncomment
// the second line of code.
public static async Task CopyVideoIfNotExists(string filename)
{
    string folder = FileSystem.AppDataDirectory;
    //string folder = Path.GetTempPath();
    string videoFile = Path.Combine(folder, "XamarinVideo.mp4");

    if (!File.Exists(videoFile))
    {
        using (Stream inputStream = await FileSystem.OpenAppPackageFileAsync(filename))
        {
            using (FileStream outputStream = File.Create(videoFile))
            {
                await inputStream.CopyToAsync(outputStream);
            }
        }
    }
}
```

> [!NOTE]
> В приведенном выше примере кода используется `FileSystem` класс, входящий в Xamarin.Essentials . Дополнительные сведения см. в разделе [ Xamarin.Essentials вспомогательные функции файловой системы](~/essentials/file-system-helpers.md?context=xamarin%2Fxamarin-forms&tabs=android).

### <a name="play-media-from-the-device-library"></a>Воспроизведение мультимедиа из библиотеки устройств

Большинство современных мобильных устройств и настольных компьютеров могут записывать видео и аудио с помощью камеры и микрофона устройства. Затем созданный носитель сохраняется в виде файлов на устройстве. Эти файлы можно извлечь из библиотеки и воспроизвести с помощью [`MediaElement`](xref:Xamarin.Forms.MediaElement) .

Каждая из платформ включает в себя средство, позволяющее пользователю выбирать носитель из библиотеки устройства. В Xamarin.Forms Проекты платформы могут вызывать эту функцию, и она может вызываться [`DependencyService`](xref:Xamarin.Forms.DependencyService) классом.

Служба зависимости видео, используемая в образце приложения, очень похожа на ту, которая определена при [выборе фотографии из библиотеки рисунков](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md), за исключением того, что средство выбора возвращает имя файла, а не `Stream` объект. Проект общего кода определяет интерфейс с именем `IVideoPicker` , который определяет один метод с именем `GetVideoFileAsync` . Затем каждая платформа реализует этот интерфейс в `VideoPicker` классе.

В следующем примере кода показано, как получить файл мультимедиа из библиотеки устройств.

```csharp
string filename = await DependencyService.Get<IVideoPicker>().GetVideoFileAsync();
if (!string.IsNullOrWhiteSpace(filename))
{
    mediaElement.Source = new FileMediaSource
    {
        File = filename
    };
}
```

Служба зависимости видео вызывается путем вызова `DependencyService.Get` метода для получения реализации `IVideoPicker` интерфейса в проекте платформы. `GetVideoFileAsync`Затем метод вызывается для этого экземпляра, а возвращаемое имя файла используется для создания [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) объекта и задания его [`Source`](xref:Xamarin.Forms.MediaElement.Source) свойству [`MediaElement`](xref:Xamarin.Forms.MediaElement) .

## <a name="change-video-aspect-ratio"></a>Изменение пропорций видео

[`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect)Свойство определяет способ масштабирования видео мультимедиа в соответствии с отображаемой областью. По умолчанию этому свойству присваивается `AspectFit` элемент перечисления, но его можно задать любому из [`Aspect`](xref:Xamarin.Forms.Aspect) членов перечисления:

- `AspectFit`Указывает, что при необходимости видео будет леттербоксед, чтобы уместиться в области экрана, сохраняя пропорции.
- `AspectFill`Указывает, что видео будет обрезано, чтобы заполнить область экрана, сохраняя пропорции.
- `Fill`Указывает, что видео будет растянуто для заполнения отображаемой области.

## <a name="poll-for-position-data"></a>Опрос данных о положении

Уведомление об изменении свойства для [`Position`](xref:Xamarin.Forms.MediaElement.Position) привязываемого свойства срабатывает только в рамках ключа, такого как начало и окончание воспроизведения, и приостановка. Поэтому привязка данных к `Position` свойству не будет давать точные данные о положении. Вместо этого необходимо настроить таймер и опросить свойство.

Хорошим местом для этого является `OnAppearing` Переопределение страницы, которая требует, чтобы данные о положении были воспроизведены как носители:

```csharp
bool polling = true;

protected override void OnAppearing()
{
    base.OnAppearing();

    Device.StartTimer(TimeSpan.FromMilliseconds(1000), () =>
    {
        Device.BeginInvokeOnMainThread(() =>
        {
            positionLabel.Text = mediaElement.Position.ToString("hh\\:mm\\:ss");
        });
        return polling;
    });
}

protected override void OnDisappearing()
{
    base.OnDisappearing();
    polling = false;
}
```

В этом примере `OnAppearing` Переопределение запускает таймер, который обновляет `positionLabel` со `Position` значением каждую секунду. Обратный вызов таймера вызывается каждую секунду, пока обратный вызов не вернет значение `false` . При переходе на страницу `OnDisappearing` выполняется переопределение, которое останавливает вызов обратного вызова таймера.

## <a name="understand-mediasource-types"></a>Общие сведения о типах Медиасаурце

[`MediaElement`](xref:Xamarin.Forms.MediaElement)Можно воспроизвести носитель, присвоив его [`Source`](xref:Xamarin.Forms.MediaElement.Source) свойству удаленный или локальный файл мультимедиа. `Source`Свойство имеет тип [`MediaSource`](xref:Xamarin.Forms.MediaSource) , и этот класс определяет два статических метода:

- [`FromFile`](xref:Xamarin.Forms.MediaSource.FromFile*)Возвращает [`MediaSource`](xref:Xamarin.Forms.MediaSource) экземпляр из `string` аргумента.
- [`FromUri`](xref:Xamarin.Forms.MediaSource.FromUri*)Возвращает [`MediaSource`](xref:Xamarin.Forms.MediaSource) экземпляр из `Uri` аргумента.

Кроме того, у [`MediaSource`](xref:Xamarin.Forms.MediaSource) класса также есть неявные операторы, возвращающие `MediaSource` экземпляры из `string` `Uri` аргументов и.

> [!NOTE]
> Если [`Source`](xref:Xamarin.Forms.MediaElement.Source) свойство задано в XAML, вызывается преобразователь типов, который возвращает [`MediaSource`](xref:Xamarin.Forms.MediaSource) экземпляр из `string` или `Uri` .

[`MediaSource`](xref:Xamarin.Forms.MediaSource)Класс также имеет два производных класса:

- [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource), который используется для указания удаленного файла мультимедиа из универсального кода ресурса (URI). Этот класс имеет [`Uri`](xref:Xamarin.Forms.UriMediaSource.Uri) свойство, для которого можно задать значение `Uri` .
- [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource), который используется для указания локального файла мультимедиа из `string` . Этот класс имеет [`File`](xref:Xamarin.Forms.FileMediaSource.File) свойство, для которого можно задать значение `string` . Кроме того, этот класс содержит неявные операторы для преобразования объекта в `string` `FileMediaSource` объект и `FileMediaSource` объект в `string` .

> [!NOTE]
> При [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) создании объекта в XAML вызывается преобразователь типов, который возвращает [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) экземпляр из `string` .

## <a name="determine-mediaelement-status"></a>Определение состояния MediaElement

[`MediaElement`](xref:Xamarin.Forms.MediaElement)Класс определяет свойство с возможностью привязки только для чтения с именем [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState) и типом [`MediaElementState`](xref:Xamarin.Forms.MediaElementState) . Это свойство указывает текущее состояние элемента управления, например, воспроизведение или приостановка мультимедиа или еще не готово к воспроизведению мультимедиа.

[`MediaElementState`](xref:Xamarin.Forms.MediaElementState)Перечисление определяет следующие члены:

- `Closed`Указывает, что объект `MediaElement` не содержит носитель.
- `Opening`Указывает, что `MediaElement` проверяется и пытается загрузить указанный источник.
- `Buffering`Указывает, что `MediaElement` загружает носитель для воспроизведения. Его [`Position`](xref:Xamarin.Forms.MediaElement.Position) свойство не переходит в это состояние. Если `MediaElement` воспроизводилось видео, то будет отображаться последний отображаемый кадр.
- `Playing`Указывает, что `MediaElement` воспроизводит источник мультимедиа.
- `Paused`Указывает, что `MediaElement` свойство не выполняет продвижение его [`Position`](xref:Xamarin.Forms.MediaElement.Position) Свойства. Если `MediaElement` воспроизводилось видео, текущий кадр будет отображаться.
- `Stopped`Указывает, что объект `MediaElement` содержит носитель, но не воспроизводится или приостанавливается. Его [`Position`](xref:Xamarin.Forms.MediaElement.Position) свойство равно 0 и не выполняется. Если загруженный носитель — видео, то `MediaElement` отображается первый кадр.

Обычно не требуется проверять [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState) свойство при использовании [`MediaElement`](xref:Xamarin.Forms.MediaElement) элементов управления транспорта. Однако это свойство очень важно при реализации собственных элементов управления транспортом.

## <a name="implement-custom-transport-controls"></a>Реализация пользовательских элементов управления транспортом

Элементы управления транспорта проигрывателя мультимедиа включают кнопки, выполняющие функции **воспроизведения**, **приостановки**и **остановки**. Эти кнопки обычно определяются по знакомым значкам, а не тексту. Как правило, функции **Воспроизведение** и **Пауза** объединены в одну кнопку.

По умолчанию [`MediaElement`](xref:Xamarin.Forms.MediaElement) элементы управления воспроизведением отключены. Это позволяет управлять `MediaElement` программно или путем предоставления собственных элементов управления транспорта. В поддержку этого `MediaElement` [`Play`](xref:Xamarin.Forms.MediaElement.Play) метода входят методы, [`Pause`](xref:Xamarin.Forms.MediaElement.Pause) и [`Stop`](xref:Xamarin.Forms.MediaElement.Stop) .

В следующем примере XAML показана страница, содержащая [`MediaElement`](xref:Xamarin.Forms.MediaElement) пользовательские элементы управления транспорта и.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MediaElementDemos.CustomTransportPage"
             Title="Custom transport">
    <Grid>
        ...
        <MediaElement x:Name="mediaElement"
                      AutoPlay="False"
                      ... />
        <StackLayout BindingContext="{x:Reference mediaElement}"
                     ...>
            <Button Text="&#x25B6;&#xFE0F; Play"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnPlayPauseButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding CurrentState}"
                                 Value="{x:Static MediaElementState.Playing}">
                        <Setter Property="Text"
                                Value="&#x23F8; Pause" />
                    </DataTrigger>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding CurrentState}"
                                 Value="{x:Static MediaElementState.Buffering}">
                        <Setter Property="IsEnabled"
                                Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
            <Button Text="&#x23F9; Stop"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnStopButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding CurrentState}"
                                 Value="{x:Static MediaElementState.Stopped}">
                        <Setter Property="IsEnabled"
                                Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
        </StackLayout>
    </Grid>
</ContentPage>
```

В этом примере пользовательские элементы управления транспортом определяются как [`Button`](xref:Xamarin.Forms.Button) объекты. Однако существует только два `Button` объекта, первый из `Button` которых представляет **Воспроизведение** и **приостановку**, а второй `Button` представляет **остановку**. [`DataTrigger`](xref:Xamarin.Forms.DataTrigger)объекты используются для включения и отключения кнопок, а также для переключения первой кнопки между **воспроизведением** и **паузой**. Дополнительные сведения о триггерах данных см. в разделе [ Xamarin.Forms Triggers](~/xamarin-forms/app-fundamentals/triggers.md).

Файл кода программной части содержит обработчики для [`Clicked`](xref:Xamarin.Forms.Button.Clicked) событий:

```csharp
void OnPlayPauseButtonClicked(object sender, EventArgs args)
{
    if (mediaElement.CurrentState == MediaElementState.Stopped ||
        mediaElement.CurrentState == MediaElementState.Paused)
    {
        mediaElement.Play();
    }
    else if (mediaElement.CurrentState == MediaElementState.Playing)
    {
        mediaElement.Pause();
    }
}

void OnStopButtonClicked(object sender, EventArgs args)
{
    mediaElement.Stop();
}
```

Кнопка **воспроизведения** может быть нажата, после того как она будет включена, чтобы начать воспроизведение:

[![Снимок экрана элемента MediaElement с пользовательскими элементами управления транспортом в iOS и Android](mediaelement-images/custom-transport-playback.png "Элемент MediaElement воспроизводит видео")](mediaelement-images/custom-transport-playback-large.png#lightbox "Элемент MediaElement воспроизводит видео")

Нажатие кнопки **приостановить** приводит к паузе воспроизведения:

[![Снимок экрана элемента MediaElement с приостановленным воспроизведением в iOS и Android](mediaelement-images/custom-transport-paused.png "MediaElement с приостановленным видео")](mediaelement-images/custom-transport-paused-large.png#lightbox "MediaElement с приостановленным видео")

Нажатие кнопки **остановить** останавливает воспроизведение и возвращает расположение файла мультимедиа в начало.

## <a name="implement-a-custom-position-bar"></a>Реализация пользовательской панели позиций

На каждой платформе реализуются элементы управления транспортировкой, в том числе строка позиционирования. Эта панель напоминает ползунок и показывает текущее расположение носителя в течение его общей продолжительности. Кроме того, можно управлять панелью позиций, чтобы перемещаться вперед или назад к новой должности в видео.

Для реализации пользовательской панели должно знать длительность мультимедиа и текущую точку воспроизведения. Эти данные доступны в [`Duration`](xref:Xamarin.Forms.MediaElement.Duration) [`Position`](xref:Xamarin.Forms.MediaElement.Position) свойствах и.

> [!IMPORTANT]
> [`Position`](xref:Xamarin.Forms.MediaElement.Position)Чтобы получить точные данные о положении, необходимо опросить. Дополнительные сведения см. в разделе [опрос данных о положении](#poll-for-position-data).

Настраиваемую строку позиций можно реализовать с помощью [`Slider`](xref:Xamarin.Forms.Slider) , как показано в следующем примере:

```csharp
public class PositionSlider : Slider
{
    public static readonly BindableProperty DurationProperty =
        BindableProperty.Create(nameof(Duration), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(1),
                                propertyChanged: (bindable, oldValue, newValue) =>
                                {
                                    ((PositionSlider)bindable).SetTimeToEnd();
                                    double seconds = ((TimeSpan)newValue).TotalSeconds;
                                    ((Slider)bindable).Maximum = seconds <= 0 ? 1 : seconds;
                                });

    public TimeSpan Duration
    {
        get { return (TimeSpan)GetValue(DurationProperty); }
        set { SetValue(DurationProperty, value); }
    }

    public static readonly BindableProperty PositionProperty =
        BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(0),
                                propertyChanged: (bindable, oldValue, newValue) => ((PositionSlider)bindable).SetTimeToEnd());

    public TimeSpan Position
    {
        get { return (TimeSpan)GetValue(PositionProperty); }
        set { SetValue(PositionProperty, value); }
    }

    static readonly BindablePropertyKey TimeToEndPropertyKey =
        BindableProperty.CreateReadOnly(nameof(TimeToEnd), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan());

    public static readonly BindableProperty TimeToEndProperty = TimeToEndPropertyKey.BindableProperty;

    public TimeSpan TimeToEnd
    {
        get { return (TimeSpan)GetValue(TimeToEndProperty); }
        private set { SetValue(TimeToEndPropertyKey, value); }
    }

    public PositionSlider()
    {
        PropertyChanged += (sender, args) =>
        {
            if (args.PropertyName == "Value")
            {
                TimeSpan newPosition = TimeSpan.FromSeconds(Value);
                if (Math.Abs(newPosition.TotalSeconds - Position.TotalSeconds) / Duration.TotalSeconds > 0.01)
                {
                    Position = newPosition;
                }
            }
        };
    }

    void SetTimeToEnd()
    {
        TimeToEnd = Duration - Position;
    }
}
```

`PositionSlider`Класс определяет собственные `Duration` и `Position` связываемые свойства, а также `TimeToEnd` связываемое свойство. Все три свойства имеют тип `TimeSpan` . Обработчик изменения свойства для `Duration` свойства устанавливает свойство объекта в `Maximum` [`Slider`](xref:Xamarin.Forms.Slider) `TotalSeconds` свойство `TimeSpan` значения. `TimeToEnd`Свойство вычисляется на основе изменений `Duration` свойств и, которое `Position` начинается с длительности носителя и уменьшается до нуля по мере продолжения воспроизведения.

`PositionSlider`Обновляется с базовой [`Slider`](xref:Xamarin.Forms.Slider) при `Slider` перемещении, чтобы показать, что носитель должен быть расширен или реверсирован до новой должности. Это обнаруживается в `PropertyChanged` обработчике в `PositionSlider` конструкторе. Обработчик проверяет наличие изменений в свойстве `Value`. Если оно отличается от свойства `Position`, то свойству `Position` присваивается значение свойства `Value`. Дополнительные сведения об использовании значка [`Slider`](xref:Xamarin.Forms.Slider) просмотра, [ Xamarin.Forms ползунка](~/xamarin-forms/user-interface/slider.md)

> [!NOTE]
> В Android [`Slider`](xref:Xamarin.Forms.Slider) имеется всего 1000 дискретных действий, независимо от `Minimum` `Maximum` параметров и. Если длина носителя превышает 1000 секунд, два разных `Position` значения будут соответствовать одному и тому же значению `Value` `Slider` . Именно поэтому вышеприведенный код проверяет, что новая и существующая должности больше одной сотый доли общей длительности.

В следующем примере показан пример, используемый `PositionSlider` на странице:

```xaml
<controls:PositionSlider x:Name="positionSlider"
                         BindingContext="{x:Reference mediaElement}"
                         Duration="{Binding Duration}"
                         ValueChanged="OnPositionSliderValueChanged">
    <controls:PositionSlider.Triggers>
        <DataTrigger TargetType="controls:PositionSlider"
                     Binding="{Binding CurrentState}"
                     Value="{x:Static MediaElementState.Buffering}">
            <Setter Property="IsEnabled" Value="False" />
        </DataTrigger>
    </controls:PositionSlider.Triggers>
</controls:PositionSlider>
```

В этом примере `Duration` свойство объекта `PositionSlider` привязано к данным [`Duration`](xref:Xamarin.Forms.MediaElement.Duration) свойства объекта [`MediaElement`](xref:Xamarin.Forms.MediaElement) . При [`Value`](xref:Xamarin.Forms.Slider.Value) [`Slider`](xref:Xamarin.Forms.Slider) изменении свойства `ValueChanged` происходит событие, и `OnPositionSliderValueChanged` обработчик выполняется. Этот обработчик задает [`MediaElement.Position`](xref:Xamarin.Forms.MediaElement.Position) свойству значение свойства `PositionSlider.Position` . Таким образом, перетаскивание `Slider` результатов в положении воспроизведения мультимедиа изменяется:

[![Снимок экрана элемента MediaElement с настраиваемой строкой расположения в iOS и Android](mediaelement-images/custom-position-bar.png "MediaElement с настраиваемой строкой позиционирования")](mediaelement-images/custom-position-bar-large.png#lightbox "MediaElement с настраиваемой строкой позиционирования")

Кроме того, [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) объект используется для отключения `PositionSlider` При буферизации мультимедиа. Дополнительные сведения о триггерах данных см. в разделе [ Xamarin.Forms Triggers](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="implement-a-custom-volume-control"></a>Реализация пользовательского элемента управления "Громкость"

Элементы управления воспроизведением мультимедиа, реализованные на каждой платформе, включают панель томов. Эта панель напоминает ползунок и показывает объем носителя. Кроме того, можно управлять панелью громкости, чтобы увеличить или уменьшить объем тома.

Пользовательская панель громкости может быть реализована с помощью [`Slider`](xref:Xamarin.Forms.Slider) , как показано в следующем примере:

```xaml
<StackLayout>
    <MediaElement AutoPlay="False"
                  Source="{StaticResource AdvancedAsync}" />
    <Slider Maximum="1.0"
            Minimum="0.0"
            Value="{Binding Volume}"
            Rotation="270"
            WidthRequest="100" />
</StackLayout>
```

В этом примере [`Slider`](xref:Xamarin.Forms.Slider) данные привязывают свое `Value` свойство к [`Volume`](xref:Xamarin.Forms.MediaElement.Volume) свойству объекта [`MediaElement`](xref:Xamarin.Forms.MediaElement) . Это возможно, поскольку `Volume` свойство использует `TwoWay` привязку. Поэтому изменение свойства `Value` приведет к `Volume` изменению свойства.

> [!NOTE]
> [`Volume`](xref:Xamarin.Forms.MediaElement.Volume)Свойство имеет обратный вызов влидатион, гарантирующий, что его значение больше или равно 0,0 и меньше или равно 1,0.

Дополнительные сведения об использовании значка [`Slider`](xref:Xamarin.Forms.Slider) просмотра, [ Xamarin.Forms ползунка](~/xamarin-forms/user-interface/slider.md)

## <a name="related-links"></a>Связанные ссылки

- [Медиаелементдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos/)
- [Схемы URI](/windows/uwp/app-resources/uri-schemes)
- [Триггеры Xamarin.Forms](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.FormsГруппу](~/xamarin-forms/user-interface/slider.md)
- [Android: Поддерживаемые форматы мультимедиа](https://developer.android.com/guide/topics/media/media-formats)
- [UWP: Поддерживаемые кодеки](/windows/uwp/audio-video-camera/supported-codecs)
