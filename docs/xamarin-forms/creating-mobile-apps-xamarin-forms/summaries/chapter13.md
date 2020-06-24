---
title: Аннотация к главе 13. Растровые изображения
description: Создание мобильных приложений с помощью Xamarin.Forms. Аннотация к главе 13. Растровые изображения
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 43caf088ad6cb816f049e7862a287c17839c2170
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136777"
---
# <a name="summary-of-chapter-13-bitmaps"></a>Аннотация к главе 13. Растровые изображения

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)

> [!NOTE]
> Примечания на этой странице указывают области, в которых Xamarin.Forms имеет расхождения с материалом, представленным в книге.

Элемент Xamarin.Forms [`Image`](xref:Xamarin.Forms.Image) показывает растровое изображение. Все платформы Xamarin.Forms поддерживают форматы файлов JPEG, PNG, GIF и BMP.

Есть четыре группы источников растровых изображений в Xamarin.Forms:

- Интернет (по указанному URL-адресу);
- изображения, внедренные как ресурсы в общей библиотеке;
- изображения, внедренные как ресурсы в проектах приложения платформы;
- любые источники, на которые может ссылаться объект `Stream` .NET, в том числе `MemoryStream`.

Ресурсы растровых изображений в общей библиотеке не зависят от платформы, а аналогичные ресурсы в проектах платформы зависят от нее.

> [!NOTE]
> В тексте книги содержатся ссылки на переносимые библиотеки классов, замененные библиотеками .NET Standard. Все примеры кода в этой книге преобразованы для использования библиотек .NET Standard.

Растровое изображение задается путем установки для свойства [`Source`](xref:Xamarin.Forms.Image.Source) объекта `Image` типа [`ImageSource`](xref:Xamarin.Forms.ImageSource), абстрактного класса с тремя производными:

- [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) — для доступа к растровому изображению через Интернет на основе объекта `Uri`, для которого установлено свойство [`Uri`](xref:Xamarin.Forms.UriImageSource.Uri);
- [`FileImageSource`](xref:Xamarin.Forms.FileImageSource) — для доступа к растровому изображению, хранящемуся в проекте приложения платформы, на основе пути к папке и файлу, где для пути должно быть задано свойство [`File`](xref:Xamarin.Forms.FileImageSource.File);
- [`StreamImageSource`](xref:Xamarin.Forms.StreamImageSource) — для загрузки растрового изображения с помощью объекта `Stream` .NET, указанного путем возвращения `Stream` от `Func`, для которого установлено свойство [`Stream`](xref:Xamarin.Forms.StreamImageSource.Stream).

Кроме того, можно использовать следующие статические методы класса `ImageSource`, каждый из которых возвращает объекты `ImageSource` (распространенный способ):

- [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) — для доступа к растровому изображению через Интернет на основе объекта `Uri`;
- [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) — для доступа к растровому изображению, хранимому в качестве внедренного ресурса в PCL приложения; [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Type)) или [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Reflection.Assembly)) — для доступа к растровому изображению в другой исходной сборке;
- [`ImageSource.FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String)) — для доступа к растровому изображению из проекта приложения платформы;
- [`ImageSource.FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream})) — для загрузки растрового изображения на основе объекта `Stream`.

Класс, эквивалентный методам `Image.FromResource`, отсутствует. Класс `UriImageSource` полезен, если требуется управлять кэшированием. Класс `FileImageSource` полезен в XAML. Класс `StreamImageSource` полезен для асинхронной загрузки объектов `Stream`, а `ImageSource.FromStream` — для синхронной.

## <a name="platform-independent-bitmaps"></a>Растровые изображения, не зависящие от платформы

Проект [**WebBitmapCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) загружает растровое изображение через Интернет с использованием `ImageSource.FromUri`. Элементу `Image` задается свойство `Content` `ContentPage`, поэтому он ограничен размером страницы. Независимо от размера растрового изображения ограниченный элемент `Image` растягивается до размера своего контейнера, а растровое изображение отображается в своем максимальном размере в элементе `Image` при сохранении пропорций изображения. Цвет областей `Image` за пределами растрового изображения можно указать с помощью [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor).

Пример [**WebBitmapXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) аналогичен описанному выше, в нем просто задается для свойства `Source` URL-адрес. Преобразование выполняет класс [`ImageSourceConverter`](xref:Xamarin.Forms.ImageSourceConverter).

### <a name="fit-and-fill"></a>Подбор размера и заливка

Вы можете управлять способом растягивания растрового изображения, задав для свойства [`Aspect`](xref:Xamarin.Forms.Image.Aspect) объекта `Image` один из следующих членов перечисления [`Aspect`](xref:Xamarin.Forms.Aspect):

- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) — учитывает пропорции (по умолчанию);
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) — выполняет заливку области, пропорции не учитываются;
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) — выполняет заливку области и учитывает пропорции, обрезает часть растрового изображения.

### <a name="embedded-resources"></a>Внедренные ресурсы

Файл растрового изображения можно добавить в PCL или в папку в PCL. Укажите **Build Action** **EmbeddedResource**. В примере [**ResourceBitmapCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) показано, как использовать `ImageSource.FromResource` для загрузки файла. Имя ресурса, передаваемое в метод, состоит из имени сборки, за которым следует точка, дополнительное имя папки и точка, а затем имя файла.

Программа задает `LayoutOptions.Center` для свойств `VerticalOptions` и `HorizontalOptions` объекта `Image`, что делает элемент `Image` неограниченным. `Image` и растровое изображение имеют одинаковый размер:

- В iOS и Android `Image` представляет собой размер растрового изображения в пикселях. Растровые пиксели и пиксели экрана сопоставляются в соотношении "один к одному".
- На универсальной платформе Windows `Image` — это размер растрового изображения в аппаратно-независимых единицах. На большинстве устройств каждый пиксель растрового изображения занимает несколько экранных пикселей.

В примере [**StackedBitmap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) `Image` помещается в вертикальную ориентацию `StackLayout` в XAML. Расширение разметки с именем [`ImageResourceExtension`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) помогает ссылаться на внедренный ресурс в XAML. Этот класс загружает ресурсы только из сборки, в которой он расположен, и поэтому не может быть помещен в библиотеку.

### <a name="more-on-sizing"></a>Дополнительные сведения об изменении размера

Желательно всегда согласованно изменять размер растровых изображений для всех платформ.
В эксперименте с примером **StackedBitmap** вы можете задать вертикальную ориентацию `StackLayout` для `WidthRequest` элемента `Image`, чтобы обеспечить единообразие размеров на платформах, но с помощью этого способа можно только уменьшить размер.

Вы можете также задать `HeightRequest`, чтобы обеспечить единообразие размеров изображения на платформах, но ограничение ширины растрового изображения сделает этот способ менее удобным. Для изображения в вертикальной ориентации `StackLayout` `HeightRequest` следует избегать.

Лучший подход — начать с растрового изображения, ширина которого больше ширины дисплея телефона в аппаратно-независимых единицах, и присвоить `WidthRequest` желаемую ширину в этих единицах. Это показано в примере [**DeviceIndBitmapSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize).

В примере [**MadTeaParty**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) показана глава 7 книги "Алиса в стране чудес" *Льюиса Кэролла* с исходными иллюстрациями Джона Тенниела:

[![Снимок экрана с изображением "Безумного чаепития" на двух устройствах](images/ch13fg16-small.png "Текст книги о сумасшедшей чайной вечеринке Шляпника")](images/ch13fg16-large.png#lightbox "Текст книги о сумасшедшей чайной вечеринке Шляпника")

### <a name="browsing-and-waiting"></a>Просмотр и ожидание

В примере [**ImageBrowser**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) пользователю позволяется просматривать стандартные изображения, хранящиеся на веб-сайте Xamarin. Для загрузки файла JSON со списком растровых изображений используется класс .NET [`WebRequest`](xref:System.Net.WebRequest).

> [!NOTE]
> Программы Xamarin.Forms для обращения к файлам через Интернет должны использовать [`HttpClient`](xref:System.Net.Http.HttpClient), а не [`WebRequest`](xref:System.Net.WebRequest).

Программа использует [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator), чтобы определить, что происходят действия. При загрузке каждого растрового изображения свойство [`IsLoading`](xref:Xamarin.Forms.Image.IsLoading) только для чтения для `Image` имеет значение `true`. Свойство `IsLoading` поддерживается связываемым свойством, поэтому при его изменении происходит событие `PropertyChanged`. Программа присоединяет обработчик к этому событию и использует текущее значение `IsLoaded`, чтобы задать свойство [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning) для `ActivityIndicator`.

## <a name="streaming-bitmaps"></a>Растровые изображения потоковой передачи

Метод `ImageSource.FromStream` создает `ImageSource` на основе `Stream` .NET. Методу должен быть передан объект `Func`, возвращающий объект `Stream`.

### <a name="accessing-the-streams"></a>Получение доступа к потокам

В примере [**BitmapStreams**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) показано использование метода `ImaageSource.FromStream` для загрузки растрового изображения, хранящегося в качестве внедренного ресурса, и загрузки растрового изображения через Интернет.

### <a name="generating-bitmaps-at-run-time"></a>Создание растровых изображений во время выполнения

Все платформы Xamarin.Forms поддерживают формат файлов BMP без сжатия, который легко использовать в качестве конструкции в коде. Затем эти файлы хранятся в `MemoryStream`. Этот подход позволяет алгоритмически создавать растровые изображения во время выполнения, как реализовано в классе [`BmpMaker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) в библиотеке **Xamrin.FormsBook Toolkit**.

В примере самостоятельного выполнения [**DiyGradientBitmap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) показано использование `BmpMaker` для создания растрового изображения с градиентом.

## <a name="platform-specific-bitmaps"></a>Растровые изображения для платформ

Все платформы Xamarin.Forms позволяют сохранять растровые изображения в сборках приложений платформы. Когда приложение Xamarin.Forms получает эти растровые изображения для платформ, они приобретают тип [`FileImageSource`](xref:Xamarin.Forms.FileImageSource). Эти растровые изображения используются для:

- свойства [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) для [`MenuItem`](xref:Xamarin.Forms.MenuItem);
- свойства [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) для [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem);
- свойства [`Image`](xref:Xamarin.Forms.Button) для `Button`.

Сборки платформы уже содержат растровые изображения для значков и экранов-заставок:

- в проекте iOS в папке **Resources**;
- в проекте Android во вложенных папках в папке **Resources**;
- в проектах Windows в папке **Assets** (несмотря на то, что платформы Windows не ограничивают растровые изображения для этой папки).

В примере [**PlatformBitmaps**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) используется код для отображения значка из проекта приложения платформы.

### <a name="bitmap-resolutions"></a>Разрешения для растровых изображений

Все платформы позволяют хранить несколько версий растровых изображений для разрешений различных устройств. Во время выполнения в зависимости от разрешения устройства на экране загружается правильная версия.

В iOS эти растровые изображения отличаются по суффиксу имени файла:

- нет суффикса для устройств со 160 точками на дюйм (1 пиксель на аппаратно-независимую единицу);
- суффикс @2x для устройств с 320 точками на дюйм (2 пикселя на аппаратно-независимую единицу);
- суффикс @3x для устройств с 480 точками на дюйм (3 пикселя на аппаратно-независимую единицу).

Растровое изображение, предназначенное для отображения в виде однодюймового квадрата, будет существовать в трех версиях:

- MyImage.jpg с квадратом 160 пикселей;
- MyImage@2x.jpg с квадратом 320 пикселей;
- MyImage@3x.jpg с квадратом 480 пикселей.

Программа будет ссылаться на это растровое изображение как на MyImage.jpg, но соответствующая версия будет получена во время выполнения в зависимости от разрешения экрана. При отсутствии ограничений растровое изображение всегда будет отображаться при 160 аппаратно-независимых единицах.

Для Android точечные рисунки хранятся в различных папках, вложенных вложенных в папку **Resources**:

- drawable-ldpi (низкое количество точек на дюйм) для устройств со 120 точками на дюйм (0,75 пикселя на аппаратно-независимую единицу);
- drawable-mdpi (среднее количество) для устройств со 160 точками на дюйм (1 пиксель на аппаратно-независимую единицу);
- drawable-hdpi (большое количество) для устройств с 240 точками на дюйм (1,5 пикселя на аппаратно-независимую единицу);
- drawable-xhdpi (очень большое количество) для устройств с 320 точками на дюйм (2 пикселя на аппаратно-независимую единицу);
- drawable-xxhdpi (сверхбольшое количество) для устройств с 480 точками на дюйм (3 пикселя на аппаратно-независимую единицу);
- drawable-xxxhdpi (супербольшое количество) для устройств с 640 точками на дюйм (4 пикселя на аппаратно-независимую единицу).

У растрового изображения, предназначенного для отображения в одном квадратном дюйме, у различных его версий будет одно и то же имя, но разный размер. Эти версии будут храниться в следующих папках:

- drawable-ldpi/MyImage.jpg с квадратом 120 пикселей;
- drawable-mdpi/MyImage.jpg с квадратом 160 пикселей;
- drawable-hdpi/MyImage.jpg с квадратом 240 пикселей;
- drawable-xhdpi/MyImage.jpg с квадратом 320 пикселей;
- drawable-xxhdpi/MyImage.jpg с квадратом 480 пикселей;
- drawable-xxxhdpi/MyImage.jpg с квадратом 640 пикселей.

Растровое изображение всегда будет отображаться при 160 аппаратно-независимых единицах. (Стандартный шаблон решения Xamarin.Forms содержит только папки hdpi, xhdpi и xxhdpi.)

Проект UWP поддерживает схему именования растровых изображений по коэффициенту масштабирования пикселей на аппаратно-независимую единицу (в процентах), например:

- MyImage.scale-200.jpg при квадрате 320 пикселей.

Допустимы только некоторые процентные значения. Среди примеров программ в этой книге только примеры с изображениями с суффиксами **scale-200**, но текущие шаблоны решений Xamarin.Forms также предусматривают **scale-100**, **scale-125**, **scale-150** и **scale-400**.

При добавлении растровых изображений в проекты платформы у **Build Action** должно быть следующее значение:

- iOS: **BundleResource**
- Android: **AndroidResource**
- UWP: **Содержимое**

В примере [**ImageTap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) создается два объекта, похожие на кнопки и состоящие из объектов `Image` с установленным `TapGestureRecognizer`. Предполагается, что объекты являются однодюймовыми квадратами. Свойство `Source` для `Image` задается с использованием объектов `OnPlatform` и `On` для ссылки на потенциально отличающиеся имена файлов на платформах. Растровые изображения содержат числа, указывающие размер пикселя, поэтому можно увидеть, растровое изображение какого размера получено и готовится для отображения.

### <a name="toolbars-and-their-icons"></a>Панели инструментов и их значки

Одним из основных мест использования растровых изображений, зависящих от платформы, является панель инструментов Xamarin.Forms, которая создается путем добавления объектов [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) в коллекцию [`ToolbarItems`](xref:Xamarin.Forms.Page.ToolbarItems), определенную `Page`. `ToobarItem` происходит от [`MenuItem`](xref:Xamarin.Forms.MenuItem), от которого она наследует некоторые свойства.

Среди всех свойств `ToolbarItem` есть самые важные:

- [`Text`](xref:Xamarin.Forms.MenuItem.Text) — для текста, который может отображаться в зависимости от платформы и `Order`;
- [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) типа `FileImageSource` — для изображения, которое может отображаться в зависимости от платформы и `Order`;
- [`Order`](xref:Xamarin.Forms.ToolbarItem.Order) типа [`ToolbarItemOrder`](xref:Xamarin.Forms.ToolbarItemOrder) — перечисление с тремя членами: [`Default`](xref:Xamarin.Forms.ToolbarItemOrder.Default), [`Primary`](xref:Xamarin.Forms.ToolbarItemOrder.Primary) и [`Secondary`](xref:Xamarin.Forms.ToolbarItemOrder.Secondary).

Количество элементов `Primary` должно быть ограничено тремя или четырьмя. Необходимо включить параметр `Text` для всех элементов. Для большинства платформ только элементам `Primary` требуется `Icon`, но для Windows 8.1 требуется `Icon` для всех элементов. У значков должен быть размер 32 аппаратно-независимые единицы. Тип `FileImageSource` указывает, что они относятся к конкретной платформе.

`ToolbarItem` запускает событие [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked) при касании, подобно `Button`. `ToolbarItem` также поддерживает свойства [`Command`](xref:Xamarin.Forms.MenuItem.Command) и [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter), часто используемые для MVVM. (См. статью [Сводка главы 18. MVVM](chapter18.md)).

Для iOS и Android требуется, чтобы страница, на которой отображается панель инструментов, была [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) или страницей, к которой можно было бы перейти с помощью `NavigationPage`. Программа [**ToolbarDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) устанавливает для свойства `MainPage` класса `App` [конструктор `NavigationPage` constructor](xref:Xamarin.Forms.NavigationPage.%23ctor(Xamarin.Forms.Page)) с аргументом `ContentPage`, а также демонстрирует конструкцию и обработчик событий панели инструментов.

### <a name="button-images"></a>Изображения кнопок

Вы также можете использовать растровые изображения для определенной платформы, чтобы задать свойство [`Image`](xref:Xamarin.Forms.Button.Image) для `Button` растрового изображения с квадратом размером 32 аппаратно-независимые единицы, как показано в примере [**ButtonImage**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage).

> [!NOTE]
> Усовершенствовано использование изображений в кнопках. См. раздел [Использование растровых изображений в кнопках](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons).

## <a name="related-links"></a>Связанные ссылки

- [Полный текст главы 13 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [Примеры из главы 13](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [Работа с образами](~/xamarin-forms/user-interface/images.md)
- [Использование растровых изображений в кнопках](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)
