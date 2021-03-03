---
title: Сохранение точечных рисунков SkiaSharp в файлах
description: Изучите различные форматы файлов, поддерживаемые SkiaSharp для сохранения точечных рисунков в библиотеке фотографий пользователя.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 2D696CB6-B31B-42BC-8D3B-11D63B1E7D9C
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1d50c03fcea043c4b29db4a82ee3dc1712c288df
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93372084"
---
# <a name="saving-skiasharp-bitmaps-to-files"></a>Сохранение точечных рисунков SkiaSharp в файлах

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

После того как приложение SkiaSharp создало или изменило точечный рисунок, приложение может сохранить растровое изображение в библиотеку фотографий пользователя:

![Сохранение точечных рисунков](saving-images/SavingSample.png "Сохранение точечных рисунков")

Эта задача состоит из двух этапов:

- Преобразование точечного рисунка SkiaSharp в данные в определенном формате, например JPEG или PNG.
- Сохранение результата в библиотеку фотографий с помощью кода, зависящего от платформы.

## <a name="file-formats-and-codecs"></a>Форматы файлов и кодеки

Большинство современных форматов файлов точечных рисунков используют сжатие для уменьшения объема хранилища. Две основные категории методов сжатия называются _потерей_ и _без потерь_. Эти термины указывают, приводит ли алгоритм сжатия к утрате данных.

Самый популярный формат с потерей данных был разработан группой «Объединенная фотограф экспертов» и называется JPEG. Алгоритм сжатия JPEG анализирует изображение с помощью математического инструмента, который называется _дискретным преобразованием_, и пытается удалить данные, которые не являются критически важными для сохранения визуальной точности изображения. Степень сжатия можно контролировать с помощью параметра, который обычно называется _качеством_. Более высокие параметры качества приводят к увеличению размера файлов.

Напротив, алгоритм сжатия без потерь анализирует изображение на повторение и шаблоны пикселей, которые могут быть закодированы способом, который сокращает объем данных, но не приводит к утрате каких-либо сведений. Исходные данные точечного рисунка можно полностью восстановить из сжатого файла. Основной формат сжатых файлов без потерь, используемый сегодня, — это переносимая сетевая графика (PNG).

Как правило, JPEG используется для фотографий, а PNG используется для изображений, созданных вручную или алгоритмически. Любой алгоритм сжатия без потерь, уменьшающий размер некоторых файлов, должен обязательно увеличивать размер других. К счастью, размер такого увеличения обычно происходит только для данных, содержащих много случайных (или случайных) сведений.

Алгоритмы сжатия достаточно сложны, чтобы гарантировать два термина, описывающих процессы сжатия и распаковки:

- _декодирование_ &mdash; чтение формата файла точечного рисунка и его распаковка
- _кодирование_ &mdash; Сжатие точечного рисунка и запись в формат файла точечного рисунка

[`SKBitmap`](xref:SkiaSharp.SKBitmap)Класс содержит несколько методов с именем `Decode` , которые создают `SKBitmap` из сжатого источника. Все, что необходимо, — предоставить имя файла, поток или массив байтов. Декодер может определить формат файла и передать его в соответствующую внутреннюю функцию декодирования.

Кроме того, [`SKCodec`](xref:SkiaSharp.SKCodec) класс имеет два метода с именем `Create` , которые могут создать `SKCodec` объект из сжатого источника и позволить приложению больше участвовать в процессе декодирования. ( `SKCodec` Класс показан в статье [**анимация SkiaSharp растровых изображений**](animating.md#gif-animation) в связи с декодированием анимированного GIF-файла.)

При кодировании точечного рисунка требуются дополнительные сведения. кодировщику необходимо узнать конкретный формат файла, который приложение хочет использовать (JPEG или PNG или что-то другое). Если требуется формат потери данных, кодирование должно также иметь необходимый уровень качества.

`SKBitmap`Класс определяет один [`Encode`](xref:SkiaSharp.SKBitmap.Encode(SkiaSharp.SKWStream,SkiaSharp.SKEncodedImageFormat,System.Int32)) метод со следующим синтаксисом:

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

Этот метод более подробно описан ниже. Закодированный точечный рисунок записывается в поток, доступный для записи. («W» `SKWStream` означает «для записи».) Второй и третий аргументы указывают формат файла и (для форматов с потерей данных) желаемое качество в диапазоне от 0 до 100.

Кроме [`SKImage`](xref:SkiaSharp.SKImage) того, классы и [`SKPixmap`](xref:SkiaSharp.SKPixmap) также определяют `Encode` методы, которые являются более универсальными и могут быть предпочтительными. Вы можете легко создать `SKImage` объект из `SKBitmap` объекта с помощью статического [`SKImage.FromBitmap`](xref:SkiaSharp.SKImage.FromBitmap(SkiaSharp.SKBitmap)) метода. Получить `SKPixmap` объект из `SKBitmap` объекта можно с помощью [`PeekPixels`](xref:SkiaSharp.SKBitmap.PeekPixels) метода.

Один из [`Encode`](xref:SkiaSharp.SKImage.Encode) методов, определенных, не `SKImage` имеет параметров и автоматически сохраняется в формате PNG. Метод без параметров очень прост в использовании.

## <a name="platform-specific-code-for-saving-bitmap-files"></a>Код, зависящий от платформы, для сохранения файлов точечных рисунков

При кодировании `SKBitmap` объекта в определенный формат файла, как правило, вы оставляете объект потока некоторой сортировки или массив данных. Некоторые `Encode` методы (включая метод без параметров, определенных `SKImage` ) возвращают [`SKData`](xref:SkiaSharp.SKData) объект, который можно преобразовать в массив байтов с помощью [`ToArray`](xref:SkiaSharp.SKData.ToArray) метода. Эти данные необходимо сохранить в файле.

Сохранение в файл в локальном хранилище приложения довольно просто, поскольку `System.IO` для этой задачи можно использовать стандартные классы и методы. Эта методика показана в статье [**анимация SkiaSharp растровых изображений**](animating.md#bitmap-animation) в связи с анимацией ряда растровых изображений в наборе Мандельброта.

Если файл должен совместно использоваться другими приложениями, его необходимо сохранить в библиотеке фотографий пользователя. Для этой задачи требуется код, зависящий от платформы, и использование Xamarin.Forms [`DependencyService`](xref:Xamarin.Forms.DependencyService) .

Проект **скиашарпформсдемо** в приложении [**скиашарпформсдемос**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) определяет `IPhotoLibrary` интерфейс, используемый с `DependencyService` классом. Он определяет синтаксис `SavePhotoAsync` метода:

```csharp
public interface IPhotoLibrary
{
    Task<Stream> PickPhotoAsync();

    Task<bool> SavePhotoAsync(byte[] data, string folder, string filename);
}
```

Этот интерфейс также определяет `PickPhotoAsync` метод, который используется для открытия средства выбора файлов, специфичных для платформы, для библиотеки фотографий устройства.

Для `SavePhotoAsync` первый аргумент — это массив байтов, который содержит точечный рисунок, уже закодированный в определенный формат файла, например JPEG или PNG. Возможно, что приложению нужно изолировать все создаваемые им точечные рисунки в определенной папке, которая указана в следующем параметре, за которым следует имя файла. Метод возвращает логическое значение, указывающее на успешное выполнение или нет.

В следующих разделах описывается `SavePhotoAsync` , как реализуется на каждой платформе.

### <a name="the-ios-implementation"></a>Реализация iOS

Реализация iOS `SavePhotoAsync` использует [`SaveToPhotosAlbum`](xref:UIKit.UIImage.SaveToPhotosAlbum*) метод из `UIImage` :

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        NSData nsData = NSData.FromArray(data);
        UIImage image = new UIImage(nsData);
        TaskCompletionSource<bool> taskCompletionSource = new TaskCompletionSource<bool>();

        image.SaveToPhotosAlbum((UIImage img, NSError error) =>
        {
            taskCompletionSource.SetResult(error == null);
        });

        return taskCompletionSource.Task;
    }
}
```

К сожалению, невозможно указать имя файла или папку для изображения.

Для файла **info. plist** в проекте iOS требуется ключ, указывающий, что он добавляет изображения в библиотеку фотографий:

```xml
<key>NSPhotoLibraryAddUsageDescription</key>
<string>SkiaSharp Forms Demos adds images to your photo library</string>
```

Осторожно! Ключ разрешения для простого доступа к библиотеке фотографий очень похож, но не тот же:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>SkiaSharp Forms Demos accesses your photo library</string>
```

### <a name="the-android-implementation"></a>Реализация Android

Реализация Android `SavePhotoAsync` сначала проверяет, является ли аргумент параметром, `folder` `null` или пустой строкой. Если да, то битовая карта сохраняется в корневом каталоге библиотеки фотографий. В противном случае папка будет получена, и, если она не существует, она будет создана:

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public async Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        try
        {
            File picturesDirectory = Environment.GetExternalStoragePublicDirectory(Environment.DirectoryPictures);
            File folderDirectory = picturesDirectory;

            if (!string.IsNullOrEmpty(folder))
            {
                folderDirectory = new File(picturesDirectory, folder);
                folderDirectory.Mkdirs();
            }

            using (File bitmapFile = new File(folderDirectory, filename))
            {
                bitmapFile.CreateNewFile();

                using (FileOutputStream outputStream = new FileOutputStream(bitmapFile))
                {
                    await outputStream.WriteAsync(data);
                }

                // Make sure it shows up in the Photos gallery promptly.
                MediaScannerConnection.ScanFile(MainActivity.Instance,
                                                new string[] { bitmapFile.Path },
                                                new string[] { "image/png", "image/jpeg" }, null);
            }
        }
        catch
        {
            return false;
        }

        return true;
    }
}
```

Вызов метода `MediaScannerConnection.ScanFile` не является строго необходимым, но если вы тестируете программу путем немедленной проверки библиотеки фотографий, она помогает значительно обновить представление галереи библиотеки.

Для файла **AndroidManifest.xml** требуется следующий тег разрешения:

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

### <a name="the-uwp-implementation"></a>Реализация UWP

Реализация UWP `SavePhotoAsync` очень похожа на структуру реализации Android:

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public async Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        StorageFolder picturesDirectory = KnownFolders.PicturesLibrary;
        StorageFolder folderDirectory = picturesDirectory;

        // Get the folder or create it if necessary
        if (!string.IsNullOrEmpty(folder))
        {
            try
            {
                folderDirectory = await picturesDirectory.GetFolderAsync(folder);
            }
            catch
            { }

            if (folderDirectory == null)
            {
                try
                {
                    folderDirectory = await picturesDirectory.CreateFolderAsync(folder);
                }
                catch
                {
                    return false;
                }
            }
        }

        try
        {
            // Create the file.
            StorageFile storageFile = await folderDirectory.CreateFileAsync(filename,
                                                CreationCollisionOption.GenerateUniqueName);

            // Convert byte[] to Windows buffer and write it out.
            IBuffer buffer = WindowsRuntimeBuffer.Create(data, 0, data.Length, data.Length);
            await FileIO.WriteBufferAsync(storageFile, buffer);
        }
        catch
        {
            return false;
        }

        return true;
    }
}
```

Раздел **возможностей** файла **Package. appxmanifest** требует наличия **библиотеки изображений**.

## <a name="exploring-the-image-formats"></a>Просмотр форматов изображений

Ниже приведен [`Encode`](xref:SkiaSharp.SKBitmap.Encode(SkiaSharp.SKWStream,SkiaSharp.SKEncodedImageFormat,System.Int32)) метод `SKImage` .

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

[`SKEncodedImageFormat`](xref:SkiaSharp.SKEncodedImageFormat) — Это перечисление с элементами, которые ссылаются на одиннадцать форматов файлов, некоторые из которых довольно понятны:

- `Astc`&mdash;Адаптивное масштабируемое сжатие текстур
- `Bmp`&mdash;Точечный рисунок Windows
- `Dng`&mdash;Adobe Digital негатив
- `Gif`&mdash;Формат обмена изображениями
- `Ico`&mdash;Изображения значков Windows
- `Jpeg`&mdash;Группа «совместная фотография экспертов»
- `Ktx`&mdash;Формат текстуры кхронос для OpenGL
- `Pkm`&mdash;Покéмон сохранить файл
- `Png`&mdash;Переносимая сетевая графика
- `Wbmp`&mdash;Формат точечного рисунка протокола приложения для беспроводной связи (1 бит на пиксель)
- `Webp`&mdash;Формат Google вебп

Как вы увидите вскоре, SkiaSharp поддерживает только три из этих форматов файлов ( `Jpeg` , `Png` и `Webp` ).

Чтобы сохранить `SKBitmap` объект с именем `bitmap` в библиотеку фотографий пользователя, также требуется член `SKEncodedImageFormat` перечисления с именем `imageFormat` и (для форматов с потерей данных) целочисленной `quality` переменной. Для сохранения этого растрового изображения в файл с именем в папке можно использовать следующий код `filename` `folder` :

```csharp
using (MemoryStream memStream = new MemoryStream())
using (SKManagedWStream wstream = new SKManagedWStream(memStream))
{
    bitmap.Encode(wstream, imageFormat, quality);
    byte[] data = memStream.ToArray();

    // Check the data array for content!

    bool success = await DependencyService.Get<IPhotoLibrary>().SavePhotoAsync(data, folder, filename);

    // Check return value for success!
}
```

`SKManagedWStream`Класс является производным от `SKWStream` (который означает "записываемый поток"). `Encode`Метод записывает закодированный файл точечного рисунка в поток. Комментарии в этом коде относятся к проверке ошибок, которые может потребоваться выполнить.

На странице **Сохранение форматов файлов** в приложении [**скиашарпформсдемос**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) используется аналогичный код, позволяющий экспериментировать с сохранением растрового изображения в различных форматах.

XAML-файл содержит объект `SKCanvasView` , который отображает точечный рисунок, а остальная часть страницы содержит все, что требуется приложению для вызова `Encode` метода `SKBitmap` . Он имеет `Picker` для члена `SKEncodedImageFormat` перечисления, `Slider` для аргумента Quality в форматах с потерей растровых изображений, двух `Entry` представлений для имени файла и папки, а также `Button` для сохранения файла.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.SaveFileFormatsPage"
             Title="Save Bitmap Formats">

    <StackLayout Margin="10">
        <skiaforms:SKCanvasView PaintSurface="OnCanvasViewPaintSurface"
                                VerticalOptions="FillAndExpand" />

        <Picker x:Name="formatPicker"
                Title="image format"
                SelectedIndexChanged="OnFormatPickerChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKEncodedImageFormat}">
                    <x:Static Member="skia:SKEncodedImageFormat.Astc" />
                    <x:Static Member="skia:SKEncodedImageFormat.Bmp" />
                    <x:Static Member="skia:SKEncodedImageFormat.Dng" />
                    <x:Static Member="skia:SKEncodedImageFormat.Gif" />
                    <x:Static Member="skia:SKEncodedImageFormat.Ico" />
                    <x:Static Member="skia:SKEncodedImageFormat.Jpeg" />
                    <x:Static Member="skia:SKEncodedImageFormat.Ktx" />
                    <x:Static Member="skia:SKEncodedImageFormat.Pkm" />
                    <x:Static Member="skia:SKEncodedImageFormat.Png" />
                    <x:Static Member="skia:SKEncodedImageFormat.Wbmp" />
                    <x:Static Member="skia:SKEncodedImageFormat.Webp" />
                </x:Array>
            </Picker.ItemsSource>
        </Picker>

        <Slider x:Name="qualitySlider"
                Maximum="100"
                Value="50" />

        <Label Text="{Binding Source={x:Reference qualitySlider},
                              Path=Value,
                              StringFormat='Quality = {0:F0}'}"
               HorizontalTextAlignment="Center" />

        <StackLayout Orientation="Horizontal">
            <Label Text="Folder Name: "
                   VerticalOptions="Center" />

            <Entry x:Name="folderNameEntry"
                   Text="SaveFileFormats"
                   HorizontalOptions="FillAndExpand" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="File Name: "
                   VerticalOptions="Center" />

            <Entry x:Name="fileNameEntry"
                   Text="Sample.xxx"
                   HorizontalOptions="FillAndExpand" />
        </StackLayout>

        <Button Text="Save"
                Clicked="OnButtonClicked">
            <Button.Triggers>
                <DataTrigger TargetType="Button"
                             Binding="{Binding Source={x:Reference formatPicker},
                                               Path=SelectedIndex}"
                             Value="-1">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>

                <DataTrigger TargetType="Button"
                             Binding="{Binding Source={x:Reference fileNameEntry},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Button.Triggers>
        </Button>

        <Label x:Name="statusLabel"
               Text="OK"
               Margin="10, 0" />
    </StackLayout>
</ContentPage>
```

Файл кода программной части загружает ресурс точечного рисунка и использует `SKCanvasView` для его вывода. Это изображение никогда не изменяется. `SelectedIndexChanged`Обработчик для `Picker` изменяет имя файла с расширением, которое совпадает с элементом перечисления:

```csharp
public partial class SaveFileFormatsPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(typeof(SaveFileFormatsPage),
        "SkiaSharpFormsDemos.Media.MonkeyFace.png");

    public SaveFileFormatsPage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        args.Surface.Canvas.DrawBitmap(bitmap, args.Info.Rect, BitmapStretch.Uniform);
    }

    void OnFormatPickerChanged(object sender, EventArgs args)
    {
        if (formatPicker.SelectedIndex != -1)
        {
            SKEncodedImageFormat imageFormat = (SKEncodedImageFormat)formatPicker.SelectedItem;
            fileNameEntry.Text = Path.ChangeExtension(fileNameEntry.Text, imageFormat.ToString());
            statusLabel.Text = "OK";
        }
    }

    async void OnButtonClicked(object sender, EventArgs args)
    {
        SKEncodedImageFormat imageFormat = (SKEncodedImageFormat)formatPicker.SelectedItem;
        int quality = (int)qualitySlider.Value;

        using (MemoryStream memStream = new MemoryStream())
        using (SKManagedWStream wstream = new SKManagedWStream(memStream))
        {
            bitmap.Encode(wstream, imageFormat, quality);
            byte[] data = memStream.ToArray();

            if (data == null)
            {
                statusLabel.Text = "Encode returned null";
            }
            else if (data.Length == 0)
            {
                statusLabel.Text = "Encode returned empty array";
            }
            else
            {
                bool success = await DependencyService.Get<IPhotoLibrary>().
                    SavePhotoAsync(data, folderNameEntry.Text, fileNameEntry.Text);

                if (!success)
                {
                    statusLabel.Text = "SavePhotoAsync return false";
                }
                else
                {
                    statusLabel.Text = "Success!";
                }
            }
        }
    }
}
```

`Clicked`Обработчик для `Button` выполняет всю реальную работу. Он получает два аргумента для `Encode` из `Picker` и `Slider` , а затем использует приведенный выше код для создания `SKManagedWStream` для `Encode` метода. Два `Entry` представления имеют имена папок и файлов для `SavePhotoAsync` метода.

Большая часть этого метода предназначена для обработки проблем или ошибок. Если `Encode` создает пустой массив, это означает, что конкретный формат файла не поддерживается. Если `SavePhotoAsync` возвращает `false` , то файл не был успешно сохранен.

Программа работает следующим образом:

[![Сохранение форматов файлов](saving-images/SaveFileFormats.png "Сохранение форматов файлов")](saving-images/SaveFileFormats-Large.png#lightbox)

На этом снимке экрана показаны только три формата, которые поддерживаются на следующих платформах:

- JPEG
- PNG
- вебп

Для всех остальных форматов `Encode` метод ничего не записывает в поток, а результирующий массив байтов пуст.

Точечный рисунок, сохраняемый в **форматах сохраняемых файлов** , составляет 600 пикселей в квадрате. С 4 байтами на пиксель это всего 1 440 000 байт в памяти. В следующей таблице показан размер файла для различных сочетаний формата и качества файла:

|Формат|Качество|Размер|
|------|------:|---:|
| PNG | Недоступно | 492K |
| JPEG | 0 | 2.95 р |
|      | 50 | 22.1 р |
|      | 100 | 206K |
| вебп | 0 | 2.71 р |
|      | 50 | 11.9 р |
|      | 100 | 101K |

Вы можете поэкспериментировать с различными параметрами качества и изучить результаты.

## <a name="saving-finger-paint-art"></a>Сохранение изображения с пальцами

Чаще всего точечный рисунок используется в программах рисования, где он работает как объект, называемый _теневым точечным рисунком_. Все рисунки сохранены на точечном рисунке, который затем отображается программой. Точечный рисунок также удобен для сохранения рисунка.

В статье о [**прорисовке пальцев в SkiaSharp**](../paths/finger-paint.md) показано, как использовать сенсорное отслеживание для реализации простейшей программы рисования пальцами. Программа поддерживала только один цвет и только одну толщину обводки, но все рисование в коллекции объектов сохранилась `SKPath` .

На странице с изображением **пальца с сохранением** в образце [**скиашарпформсдемос**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) также сохраняется вся прорисовка в коллекции `SKPath` объектов, но рисунок также визуализируется на точечном рисунке, который может быть сохранен в библиотеке фотографий.

Большая часть этой программы похожа на первоначальную программу **рисования пальца** . Одним из улучшений является то, что XAML-файл теперь создает экземпляры кнопок с надписью **clear** и **Save**:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Bitmaps.FingerPaintSavePage"
             Title="Finger Paint Save">

    <StackLayout>
        <Grid BackgroundColor="White"
              VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="canvasView"
                               PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>

        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
        </Grid>

        <Button Text="Clear"
                Grid.Row="0"
                Margin="50, 5"
                Clicked="OnClearButtonClicked" />

        <Button Text="Save"
                Grid.Row="1"
                Margin="50, 5"
                Clicked="OnSaveButtonClicked" />

    </StackLayout>
</ContentPage>
```

Файл кода программной части поддерживает поле типа `SKBitmap` с именем `saveBitmap` . Это растровое изображение создается или воссоздается в `PaintSurface` обработчике при каждом изменении размера области отображения. Если необходимо создать точечный рисунок, содержимое существующего растрового изображения копируется в новое растровое изображение, чтобы сохранять все, независимо от того, как изменяется размер поверхности отображения.

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    ···
    SKBitmap saveBitmap;

    public FingerPaintSavePage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        // Create bitmap the size of the display surface
        if (saveBitmap == null)
        {
            saveBitmap = new SKBitmap(info.Width, info.Height);
        }
        // Or create new bitmap for a new size of display surface
        else if (saveBitmap.Width < info.Width || saveBitmap.Height < info.Height)
        {
            SKBitmap newBitmap = new SKBitmap(Math.Max(saveBitmap.Width, info.Width),
                                              Math.Max(saveBitmap.Height, info.Height));

            using (SKCanvas newCanvas = new SKCanvas(newBitmap))
            {
                newCanvas.Clear();
                newCanvas.DrawBitmap(saveBitmap, 0, 0);
            }

            saveBitmap = newBitmap;
        }

        // Render the bitmap
        canvas.Clear();
        canvas.DrawBitmap(saveBitmap, 0, 0);
    }
    ···
}
```

Рисование, выполняемое `PaintSurface` обработчиком, происходит в самом конце и состоит только из визуализации растрового изображения.

Сенсорная обработка аналогична предыдущей программе. Программа поддерживает две коллекции, `inProgressPaths` и `completedPaths` , которые содержат все, что пользователь нарисован с момента последнего очистки экрана. Для каждого события касания `OnTouchEffectAction` обработчик вызывает `UpdateBitmap` :

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    Dictionary<long, SKPath> inProgressPaths = new Dictionary<long, SKPath>();
    List<SKPath> completedPaths = new List<SKPath>();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };
    ···
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = new SKPath();
                    path.MoveTo(ConvertToPixel(args.Location));
                    inProgressPaths.Add(args.Id, path);
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Moved:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = inProgressPaths[args.Id];
                    path.LineTo(ConvertToPixel(args.Location));
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Released:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    completedPaths.Add(inProgressPaths[args.Id]);
                    inProgressPaths.Remove(args.Id);
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Cancelled:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    inProgressPaths.Remove(args.Id);
                    UpdateBitmap();
                }
                break;
        }
    }

    SKPoint ConvertToPixel(Point pt)
    {
        return new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                            (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));
    }

    void UpdateBitmap()
    {
        using (SKCanvas saveBitmapCanvas = new SKCanvas(saveBitmap))
        {
            saveBitmapCanvas.Clear();

            foreach (SKPath path in completedPaths)
            {
                saveBitmapCanvas.DrawPath(path, paint);
            }

            foreach (SKPath path in inProgressPaths.Values)
            {
                saveBitmapCanvas.DrawPath(path, paint);
            }
        }

        canvasView.InvalidateSurface();
    }
    ···
}
```

`UpdateBitmap`Метод перерисовывается `saveBitmap` путем создания нового `SKCanvas` , очистки и последующего отображения всех путей на точечном рисунке. Он завершается недействительным, `canvasView` чтобы точечный рисунок мог быть нарисован на экране.

Ниже приведены обработчики для двух кнопок. Кнопка **clear** очищает как коллекции путей, так и обновления `saveBitmap` (что приводит к очистке точечного рисунка) и делает недействительным `SKCanvasView` следующее:

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    ···
    void OnClearButtonClicked(object sender, EventArgs args)
    {
        completedPaths.Clear();
        inProgressPaths.Clear();
        UpdateBitmap();
        canvasView.InvalidateSurface();
    }

    async void OnSaveButtonClicked(object sender, EventArgs args)
    {
        using (SKImage image = SKImage.FromBitmap(saveBitmap))
        {
            SKData data = image.Encode();
            DateTime dt = DateTime.Now;
            string filename = String.Format("FingerPaint-{0:D4}{1:D2}{2:D2}-{3:D2}{4:D2}{5:D2}{6:D3}.png",
                                            dt.Year, dt.Month, dt.Day, dt.Hour, dt.Minute, dt.Second, dt.Millisecond);

            IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();
            bool result = await photoLibrary.SavePhotoAsync(data.ToArray(), "FingerPaint", filename);

            if (!result)
            {
                await DisplayAlert("FingerPaint", "Artwork could not be saved. Sorry!", "OK");
            }
        }
    }
}
```

Обработчик кнопки **сохранения** использует упрощенный [`Encode`](xref:SkiaSharp.SKImage.Encode) метод из `SKImage` . Этот метод кодирует в формате PNG. `SKImage`Объект создается на основе `saveBitmap` , а `SKData` объект содержит закодированный PNG-файл.

`ToArray`Метод `SKData` получает массив байтов. Это то, что передается в `SavePhotoAsync` метод вместе с фиксированным именем папки и уникальное имя файла, созданное на основе текущих даты и времени.

Вот программа в действии:

[![Экономия при сохранении пальца](saving-images/FingerPaintSave.png "Экономия при сохранении пальца")](saving-images/FingerPaintSave-Large.png#lightbox)

В примере [**спинпаинт**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-spinpaint) используется очень похожий метод. Это также программа рисования пальцами, за исключением того, что пользователь рисует на цикличном диске, который затем воссоздает макет по другим четырем квадрантам. Цвет заливки пальца изменяется по мере вращения диска:

[![Покраска](saving-images/SpinPaint.png "Покраска")](saving-images/SpinPaint-Large.png#lightbox)

Кнопка **сохранить** `SpinPaint` в классе аналогична **прорисовке пальца** в том, что он сохраняет изображение в фиксированное имя папки (**спаинпаинт**) и имя файла, созданного на основе даты и времени.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [Спинпаинт (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-spinpaint)