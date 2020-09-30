---
title: Изображения в Xamarin.Forms
description: Образы можно совместно использовать на разных платформах Xamarin.Forms , они могут загружаться специально для каждой платформы, а также могут быть загружены для просмотра.
ms.prod: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/19/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 824d5ca711495c8a8ad663034e77506468efd397
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91556195"
---
# <a name="images-in-no-locxamarinforms"></a>Изображения в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages)

_Образы можно совместно использовать на разных платформах Xamarin.Forms , они могут загружаться специально для каждой платформы, а также могут быть загружены для просмотра._

Изображения являются важной частью навигации по приложениям, их удобства использования и фирменной символики. Xamarin.Forms приложения должны иметь возможность совместно использовать изображения на всех платформах, но на каждой платформе могут отображаться разные образы.

Для значков и экранов-заставок также требуются образы, зависящие от платформы. они должны быть настроены на уровне каждой платформы.

## <a name="display-images"></a>Отображение изображений

Xamarin.Forms использует [`Image`](xref:Xamarin.Forms.Image) представление для отображения изображений на странице. Он имеет несколько важных свойств:

- [`Source`](xref:Xamarin.Forms.Image.Source) — [`ImageSource`](xref:Xamarin.Forms.ImageSource) Экземпляр, файл, URI или ресурс, который задает отображаемое изображение.
- [`Aspect`](xref:Xamarin.Forms.Image.Aspect) — Изменение размера изображения в пределах границ, в которых он отображается (растяжение, кадрирование или леттербокс).

[`ImageSource`](xref:Xamarin.Forms.ImageSource) экземпляры можно получить с помощью статических методов для каждого типа источника изображения:

- [`FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String)) — Требуется имя файла или путь к файлу, который может быть разрешен на каждой платформе.
- [`FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) — Требуется объект URI, например.  `new Uri("http://server.com/image.jpg")` .
- [`FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) — Требуется идентификатор ресурса для файла изображения, внедренного в проект библиотеки приложения или .NET Standard, с **действием сборки: EmbeddedResource**.
- [`FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream})) — Требуется поток, поставляющий данные изображения.

[`Aspect`](xref:Xamarin.Forms.Image.Aspect)Свойство определяет, как изображение будет масштабироваться в соответствии с отображаемой областью:

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) — Растягивает изображение, чтобы полностью и точно заполнить отображаемую область. Это может привести к искажению изображения.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) — Обрезает изображение таким образом, чтобы оно заполнило область экрана при сохранении аспекта (т. е. без искажений).
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) — Леттербоксес изображение (при необходимости), чтобы весь образ поместился в область экрана, с добавлением пробела в верхнюю или нижнюю часть или в зависимости от того, является ли изображение широким или максимальным.

Образы могут загружаться из [локального файла](#local-images), [внедренного ресурса](#embedded-images), [скачанного](#download-images)или загруженного из потока. Кроме того, значки шрифтов могут отображаться в [`Image`](xref:Xamarin.Forms.Image) представлении путем указания данных значка шрифта в `FontImageSource` объекте. Дополнительные сведения см. в разделе [Отображение значков шрифтов](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons) в пошаговом окне " [шрифты](~/xamarin-forms/user-interface/text/fonts.md) ".

## <a name="local-images"></a>Локальные образы

Файлы изображений можно добавлять в каждый проект приложения и ссылаться из Xamarin.Forms общего кода. Этот метод распространения является обязательным для изображений, специфических для платформы, например при использовании разных разрешений на различных платформах или немного разных вариантах изображения.

Чтобы использовать один образ во всех приложениях, *на каждой платформе необходимо использовать*одно и то же имя файла, которое должно быть допустимым именем ресурса Android (т. е. только строчные буквы, цифры, символ подчеркивания и период).

- **iOS** — предпочтительный способ управления и поддержки образов с iOS 9 — использование **наборов образов каталога активов**, которые должны содержать все версии образа, необходимые для поддержки различных устройств и коэффициентов масштабирования для приложения. Дополнительные сведения см. [в разделе Добавление образов в набор изображений каталога активов](~/ios/app-fundamentals/images-icons/displaying-an-image.md).
- Образы на устройстве **Android** в каталоге **Resources/Draw** с **действием сборки: AndroidResource**. Также можно указать версии изображения с высоким и низким разрешением (в подкаталогах **ресурсов** с соответствующим именем, например, **Draw-лдпи**, **Draw-HDPI**и **Draw-xhdpi**).
- **Универсальная платформа Windows (UWP)** — по умолчанию образы должны размещаться в корневом каталоге приложения с **действием сборки: Content**. Кроме того, образы можно поместить в другой каталог, который затем указывается с конкретной платформой. Дополнительные сведения см. [в разделе Каталог образов по умолчанию в Windows](~/xamarin-forms/platform/windows/default-image-directory.md).

> [!IMPORTANT]
> До версии iOS 9 образы обычно помещаются в папку **Resources** с **действием сборки: BundleResource**. Однако этот метод работы с образами в приложении iOS устарел с помощью Apple. Дополнительные сведения см. в разделе [размеры и имена файлов образов](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Соблюдение этих правил для именования и размещения файлов позволяет следующему XAML загрузить и отобразить изображение на всех платформах:

```xaml
<Image Source="waterfront.jpg" />
```

Эквивалентный код C# выглядит следующим образом:

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

На следующих снимках экрана показан результат отображения локального изображения на каждой платформе:

[![Пример приложения, отображающий локальный образ](images-images/local-sml.png)](images-images/local.png#lightbox)

Для большей гибкости `Device.RuntimePlatform` свойство можно использовать для выбора другого файла изображения или пути для некоторых или всех платформ, как показано в следующем примере кода:

```csharp
image.Source = Device.RuntimePlatform == Device.Android
                ? ImageSource.FromFile("waterfront.jpg")
                : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> Чтобы использовать одно и то же имя файла образа на всех платформах, имя должно быть допустимым на всех платформах. В Android драваблес есть ограничения на именование. допускаются только строчные буквы, цифры, символы подчеркивания и точка, а для обеспечения совместимости между платформами это также необходимо выполнить на всех остальных платформах. Пример имени файла **waterfront.png** соответствует правилам, но примеры недопустимых имен файлов включают в себя "вода front.png", "WaterFront.png", "water-front.png" и "wåterfront.png".

### <a name="native-resolutions-retina-and-high-dpi"></a>Собственные разрешения (Retina и высокое разрешение)

в iOS, Android и UWP предусмотрена поддержка различных разрешений изображений, где операционная система выбирает соответствующий образ во время выполнения в зависимости от возможностей устройства. Xamarin.Forms использует API собственных платформ для загрузки локальных образов, поэтому он автоматически поддерживает альтернативные разрешения, если файлы правильно именованы и находятся в проекте.

Предпочтительным способом управления образами с iOS 9 является перетаскивание изображений для каждого разрешения, необходимого для соответствующего набора образов каталога активов. Дополнительные сведения см. [в разделе Добавление образов в набор изображений каталога активов](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

До выпуска iOS 9 Retina версии образа можно было бы поместить в папку **Resources (ресурсы** ) два и три раза в разрешение с **@2x** **@3x** суффиксом или по имени файла перед расширением файла (например, **myimage@2x.png**). Однако этот метод работы с образами в приложении iOS устарел с помощью Apple. Дополнительные сведения см. в разделе [размеры и имена файлов образов](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Образы альтернативного разрешения Android должны размещаться в [каталогах с особыми именами](https://developer.android.com/guide/practices/screens_support.html) в проекте Android, как показано на следующем снимке экрана:

[![Расположение образа Android с несколькими разрешениями](images-images/xs-highdpisolution-sml.png)](images-images/xs-highdpisolution.png#lightbox)

Имена файлов образа UWP можно дополнить [суффиксом `.scale-xxx` перед расширением файла](/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast), где `xxx` — это процент масштабирования, применяемого к ресурсу, например **myimage.scale-200.png**. На образы можно ссылаться в коде или коде XAML без модификатора Scale, например, просто **myimage.png**. Платформа выберет ближайшее соответствующее масштабирование ресурса на основе текущего DPI дисплея.

### <a name="additional-controls-that-display-images"></a>Дополнительные элементы управления, отображающие изображения

Некоторые элементы управления имеют свойства, отображающие изображение, например:

- [`Button`](xref:Xamarin.Forms.Button) имеет [`ImageSource`](xref:Xamarin.Forms.Button.ImageSource) свойство, которое может быть задано для растрового изображения, отображаемого в `Button` . Дополнительные сведения см. в разделе [Использование точечных рисунков с кнопками](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons).
- [`ImageButton`](xref:Xamarin.Forms.Button) имеет [`Source`](xref:Xamarin.Forms.ImageButton.Source) свойство, которое может быть задано для изображения, отображаемого в `ImageButton` . Дополнительные сведения см. [в разделе Установка источника изображения](~/xamarin-forms/user-interface/imagebutton.md#setting-the-image-source).
- [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) имеет [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource) свойство, которое можно задать для изображения, загружаемого из файла, внедренного ресурса, URI или потока.
- [`ImageCell`](xref:Xamarin.Forms.ImageCell) имеет [`ImageSource`](xref:Xamarin.Forms.ImageCell.ImageSource) свойство, которое можно задать для изображения, полученного из файла, внедренного ресурса, URI или потока.
- [`Page`](xref:Xamarin.Forms.Page). Любой тип страницы, производный от `Page` , [`IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) имеет [`BackgroundImageSource`](xref:Xamarin.Forms.Page.BackgroundImageSource) Свойства и, которым можно назначить файл, внедренный ресурс, URI или поток. При определенных обстоятельствах, например при [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) отображении элемента [`ContentPage`](xref:Xamarin.Forms.ContentPage) , будет отображаться значок, если он поддерживается платформой.

  > [!IMPORTANT]
  > В iOS [`Page.IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) свойство не может быть заполнено изображением в наборе изображений каталога активов. Вместо этого Загрузите изображения значков для `Page.IconImageSource` свойства из файла, внедренного ресурса, URI или потока.

## <a name="embedded-images"></a>Внедренные изображения

Внедренные изображения также поставляются вместе с приложением (например, локальными образами), но вместо копии образа в структуре файлов каждого приложения файл изображения внедряется в сборку в качестве ресурса. Этот метод распространения образов рекомендуется, когда на каждой платформе используются идентичные образы и он особенно подходит для создания компонентов, так как образ объединяется с кодом.

Чтобы внедрить изображение в проект, щелкните правой кнопкой мыши, чтобы добавить новые элементы и выбрать изображения, которые нужно добавить. По умолчанию для образа будет задано **действие сборки: нет**; необходимо задать для **действия сборки: EmbeddedResource**.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Задать для действия сборки внедренный ресурс](images-images/vs-buildaction-sml.png)](images-images/vs-buildaction.png#lightbox)

**Действие сборки** можно просмотреть и изменить в окне **свойств** файла.

В этом примере идентификатор ресурса — **WorkingWithImages.beach.jpg**.
Интегрированная среда разработки создала это значение по умолчанию путем сцепления **пространства имен по умолчанию** для этого проекта с именем файла, используя точку (.) между каждым значением.
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

![Задать действие сборки: EmbeddedResource](images-images/xs-buildaction.png)

**Действие сборки** также можно просмотреть и изменить на панели **свойств** файла.
На этой панели отображается **идентификатор ресурса** , используемый для ссылки на ресурс в коде. На следующем снимке экрана **идентификатор ресурса** **WorkingWithImages.beach.jpg**.
Интегрированная среда разработки создала это значение по умолчанию путем сцепления **пространства имен по умолчанию** для этого проекта с именем файла, используя точку (.) между каждым значением.
Этот идентификатор можно изменить на панели **свойств** , но для этих примеров будет использоваться значение **WorkingWithImages.beach.jpg** .

[![Панель свойств внедренных ресурсов](images-images/xs-embeddedproperties-sml.png)](images-images/xs-embeddedproperties.png#lightbox)

-----

Если внедренные изображения помещаются в папки в проекте, имена папок также разделяются точками (.) в ИДЕНТИФИКАТОРе ресурса. Перемещение образа **beach.jpg** в папку с именем **MyImages** приведет к появлению идентификатора ресурса **WorkingWithImages.MyImages.beach.jpg**

Код для загрузки внедренного изображения просто передает **идентификатор ресурса** в [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) метод, как показано ниже:

```csharp
Image embeddedImage = new Image
{
    Source = ImageSource.FromResource("WorkingWithImages.beach.jpg", typeof(MyClass).GetTypeInfo().Assembly)
};
```

> [!NOTE]
> Для поддержки отображения внедренных изображений в режиме выпуска на универсальная платформа Windows необходимо использовать перегрузку `ImageSource.FromResource` , указывающую исходную сборку, в которой следует искать изображение.

В настоящее время неявное преобразование идентификаторов ресурсов не предусмотрено. Вместо этого [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) `new ResourceImageSource()` для загрузки внедренных изображений необходимо использовать или.

На следующих снимках экрана показан результат отображения внедренного изображения на каждой платформе:

[![Пример приложения, отображающий внедренное изображение](images-images/resource-sml.png)](images-images/resource.png#lightbox)

### <a name="xaml"></a>XAML

Поскольку нет встроенного преобразователя типов из `string` в `ResourceImageSource` , эти типы изображений не могут быть загружены в собственном коде XAML. Вместо этого для загрузки изображений с помощью **идентификатора ресурса** , УКАЗАННОГО в XAML, можно написать простое расширение разметки XAML.

```csharp
[ContentProperty (nameof(Source))]
public class ImageResourceExtension : IMarkupExtension
{
 public string Source { get; set; }

 public object ProvideValue (IServiceProvider serviceProvider)
 {
   if (Source == null)
   {
     return null;
   }

   // Do your translation lookup here, using whatever method you require
   var imageSource = ImageSource.FromResource(Source, typeof(ImageResourceExtension).GetTypeInfo().Assembly);

   return imageSource;
 }
}
```

> [!NOTE]
> Для поддержки отображения внедренных изображений в режиме выпуска на универсальная платформа Windows необходимо использовать перегрузку `ImageSource.FromResource` , указывающую исходную сборку, в которой следует искать изображение.

Чтобы использовать это расширение, добавьте пользовательское `xmlns` в XAML, используя правильное пространство имен и значения сборки для проекта. Затем можно задать источник изображения, используя следующий синтаксис: `{local:ImageResource WorkingWithImages.beach.jpg}` . Ниже приведен полный пример XAML.

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage
   xmlns="http://xamarin.com/schemas/2014/forms"
   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
   xmlns:local="clr-namespace:WorkingWithImages;assembly=WorkingWithImages"
   x:Class="WorkingWithImages.EmbeddedImagesXaml">
 <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
   <!-- use a custom Markup Extension -->
   <Image Source="{local:ImageResource WorkingWithImages.beach.jpg}" />
 </StackLayout>
</ContentPage>
```

### <a name="troubleshoot-embedded-images"></a>Устранение неполадок внедренных образов

#### <a name="debug-code"></a>Отладка кода

Поскольку иногда трудно понять, почему не загружается определенный ресурс изображения, можно временно добавить в приложение следующий код отладки, чтобы убедиться, что ресурсы настроены правильно. Он выводит все известные ресурсы, внедренные в данную сборку, в **консоль** , чтобы помочь в отладке проблем загрузки ресурсов.

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(MyClass).GetTypeInfo().Assembly;
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

#### <a name="images-embedded-in-other-projects"></a>Изображения, внедренные в другие проекты

По умолчанию `ImageSource.FromResource` метод выполняет поиск только изображений в той же сборке, что и код, вызывающий `ImageSource.FromResource` метод. С помощью приведенного выше кода отладки можно определить, какие сборки содержат определенный ресурс, изменив `typeof()` инструкцию на `Type` известную в каждой сборке.

Однако исходная сборка, в которой выполняется поиск внедренного изображения, может быть указана в качестве аргумента для `ImageSource.FromResource` метода:

```csharp
var imageSource = ImageSource.FromResource("filename.png",
            typeof(MyClass).GetTypeInfo().Assembly);
```

## <a name="download-images"></a>Загрузка образов

Образы могут быть автоматически скачаны для отображения, как показано в следующем коде XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
       x:Class="WorkingWithImages.DownloadImagesXaml">
  <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
    <Label Text="Image UriSource Xaml" />
    <Image Source="https://aka.ms/campus.jpg" />
    <Label Text="campus.jpg gets downloaded from microsoft.com" />
  </StackLayout>
</ContentPage>
```

Эквивалентный код C# выглядит следующим образом:

```csharp
var webImage = new Image {
     Source = ImageSource.FromUri(
        new Uri("https://aka.ms/campus.jpg")
     ) };
```

[`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri))Метод требует наличия `Uri` объекта и возвращает новый объект [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) , который считывает из `Uri` .

Также существует неявное преобразование для строк URI, поэтому в следующем примере также будет работать:

```csharp
webImage.Source = "https://aka.ms/campus.jpg";
```

На следующих снимках экрана показан результат отображения удаленного образа на каждой платформе:

[![Пример приложения, отображающий скачанный образ](images-images/download-sml.png)](images-images/download.png#lightbox)

### <a name="downloaded-image-caching"></a>Кэширование скачанных образов

[`UriImageSource`](xref:Xamarin.Forms.UriImageSource)Также поддерживает кэширование скачанных образов, настроенных с помощью следующих свойств:

- [`CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled) — Включено ли кэширование (по `true` умолчанию).
- [`CacheValidity`](xref:Xamarin.Forms.UriImageSource.CacheValidity) — Значение типа `TimeSpan` , определяющее, как долго образ будет храниться локально.

Кэширование включено по умолчанию и будет хранить образ локально в течение 24 часов. Чтобы отключить кэширование для конкретного образа, создайте экземпляр источника образа следующим образом:

```csharp
image.Source = new UriImageSource { CachingEnabled = false, Uri = new Uri("https://server.com/image") };
```

Чтобы задать определенный период кэширования (например, 5 дней), создайте экземпляр источника образа следующим образом:

```csharp
webImage.Source = new UriImageSource
{
    Uri = new Uri("https://aka.ms/campus.jpg"),
    CachingEnabled = true,
    CacheValidity = new TimeSpan(5,0,0,0)
};
```

Встроенное кэширование упрощает поддержку таких сценариев, как прокручиваемые списки изображений, где можно задать (или привязать) изображение в каждой ячейке и позволить встроенному кэшу позаботиться о повторной загрузке изображения при прокрутке ячейки обратно в представление.

## <a name="animated-gifs"></a>Анимированные GIF

Xamarin.Forms включает поддержку отображения мелких анимированных GIF. Это достигается путем присвоения [`Image.Source`](xref:Xamarin.Forms.Image.Source) свойству анимированного GIF-файла:

```xaml
<Image Source="demo.gif" />
```

> [!IMPORTANT]
> Хотя поддержка анимированных рисунков GIF в Xamarin.Forms включает возможность загрузки файлов, она не поддерживает кэширование или потоковую передачу анимированных изображений GIF.

По умолчанию, когда загружается анимированный GIF-файл, он не будет воспроизводиться. Это обусловлено тем, что `IsAnimationPlaying` свойство, которое управляет воспроизведением или остановкой анимированного GIF-файла, имеет значение по умолчанию `false` . Это свойство типа `bool` поддерживается [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектом, что означает, что он может быть целевым объектом привязки данных и имеет стиль.

Таким образом, когда загружается анимированный GIF-файл, он не будет воспроизводиться до тех пор, пока `IsAnimationPlaying` свойство не будет установлено в значение `true` . Затем можно остановить воспроизведение, задав `IsAnimationPlaying` для свойства значение `false` . Обратите внимание, что это свойство не действует при отображении источника изображения, отличного от GIF.

> [!NOTE]
> В Android поддержка анимированных GIF-рисунков требует, чтобы приложение использовало быстрые модули подготовки отчетов и не работало, если вы выбрали использование модулей подготовки отчетов прежних версий.
> В UWP поддержка анимированных GIF-рисунков требует наличия минимального выпуска обновления для Windows 10 (версия 1607).

## <a name="icons-and-splash-screens"></a>Значки и экраны-заставки

Хотя [`Image`](xref:Xamarin.Forms.Image) значки приложений и экраны-заставки не связаны с представлением, они также являются важным использованием изображений в Xamarin.Forms проектах.

Установка значков и экранов-заставок для Xamarin.Forms приложений выполняется в каждом из проектов приложений. Это означает создание изображений с правильным размером для iOS, Android и UWP. Эти образы должны называться и располагаться в соответствии с требованиями каждой платформы.

## <a name="icons"></a>Значки

Дополнительные сведения о создании этих ресурсов приложений см. в руководстве по [работе с образами](~/ios/app-fundamentals/images-icons/index.md), [Google Иконографи](https://developer.android.com/design/style/iconography.html)и [UWP руководства для плиток и ресурсов значков](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/) .

Кроме того, значки шрифтов могут отображаться в [`Image`](xref:Xamarin.Forms.Image) представлении путем указания данных значка шрифта в `FontImageSource` объекте. Дополнительные сведения см. в разделе [Отображение значков шрифтов](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons) в пошаговом окне " [шрифты](~/xamarin-forms/user-interface/text/fonts.md) ".

## <a name="splash-screens"></a>Экраны-заставки

Только для приложений iOS и UWP требуется экран-заставка (также называемый экраном запуска или изображением по умолчанию).

См. документацию по [iOS, которая работает с изображениями](~/ios/app-fundamentals/images-icons/index.md) и [экранами-заставками](/windows/uwp/launch-resume/splash-screens/) в центре разработки для Windows.

## <a name="related-links"></a>Связанные ссылки

- [Воркингвисимажес (пример)](/samples/xamarin/xamarin-forms-samples/workingwithimages)
- [Работа iOS с изображениями](~/ios/app-fundamentals/images-icons/index.md)
- [Иконографи Android](https://developer.android.com/design/style/iconography.html)
- [Руководство по работе с ресурсами плиток и значков](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)