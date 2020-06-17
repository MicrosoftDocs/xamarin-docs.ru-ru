---
title: "Локализация справа налево" description: "Локализация справа налево поддерживает направление потока справа налево в приложениях Xamarin.Forms".
ms.prod: xamarin ms.assetid: 90E0CB16-C42A-4CC8-A70E-0C2CFB64A429 ms.technology: xamarin-forms ms.custom: xamu-video author: davidbritch ms.author: dabritch ms.date: 05/07/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="right-to-left-localization"></a>Локализация справа налево

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todolocalizedrtl)

_Локализация справа налево поддерживает направление потока справа налево в приложениях Xamarin.Forms._

> [!NOTE]
> Локализация справа налево требует использования iOS 9 или более поздней версии и API 17 или более поздней версии в Android.

Направление потока — это направление, в котором глаз человека перемещается по элементам пользовательского интерфейса на странице. Некоторые языки, такие как арабский и иврит, требуют, чтобы элементы пользовательского интерфейса располагались в направлении справа налево. Чтобы реализовать это, можно задать свойство [`VisualElement.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection). Оно возвращает или задает направление, в котором элементы пользовательского интерфейса следуют в родительском элементе, определяющем их размещение. Для этого свойства следует задать одно из значений перечисления [`FlowDirection`](xref:Xamarin.Forms.FlowDirection):

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

Установка для свойства [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) значения [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft) в элементе в общем случае указывает выравнивание по правому краю, порядок чтения справа налево и поток элементов управления справа налево:

[![TodoItemPage на арабском языке с написанием справа налево](rtl-images/TodoItemPage-Arabic.png "TodoItemPage на арабском языке с написанием справа налево")](rtl-images/TodoItemPage-Arabic-Large.png#lightbox "TodoItemPage на арабском языке с написанием справа налево")

> [!TIP]
> Сначала свойство [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) следует задать только в начальном макете. Изменение этого значения во время выполнения приводит к ресурсоемкому процессу, который негативно влияет на производительность.

Значением свойства [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) по умолчанию для элемента без родительского объекта является [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight), а `FlowDirection` по умолчанию для элемента с родительским объектом — [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent). Таким образом, элемент наследует значение свойства `FlowDirection` от своего родительского элемента в визуальном дереве, и любой элемент может переопределить значение, которое он получает от своего родительского объекта.

> [!TIP]
> При локализации приложения на языки с направлением потока справа налево задайте свойство [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) на странице или в корневом макете. В результате все элементы, содержащиеся на странице или в корневом макете, должным образом реагируют на направление потока.

## <a name="respecting-device-flow-direction"></a>Соблюдение направления потока устройства

Соблюдение направления потока устройства в зависимости от выбранного языка и региона является явным выбором разработчика и не происходит автоматически. Это можно сделать, задав для свойства [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) на странице или в корневом макете значение `static` [`Device.FlowDirection`](xref:Xamarin.Forms.Device.FlowDirection):

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

```csharp
this.FlowDirection = Device.FlowDirection;
```

После этого все дочерние элементы страницы или корневого макета по умолчанию унаследуют значение [`Device.FlowDirection`](xref:Xamarin.Forms.Device.FlowDirection).

## <a name="platform-setup"></a>Настройка платформы

Для включения языковых стандартов с направлением потока справа налево требуется настройка конкретной платформы.

### <a name="ios"></a>iOS

Требуемый языковой стандарт с направлением потока справа налево необходимо добавить в качестве поддерживаемого языка в элементы массива для ключа `CFBundleLocalizations` в файле **Info.plist**. В следующем примере показано добавление арабского языка в массив для ключа `CFBundleLocalizations`:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>ar</string>
</array>
```

![Поддерживаемые языки Info.plist](rtl-images/ios-locales.png "Поддерживаемые языки Info.plist")

Подробнее об этом см. в разделе [Основы локализации в iOS](https://docs.microsoft.com/xamarin/ios/app-fundamentals/localization/#localization-basics-in-ios).

После этого локализацию справа налево можно протестировать, изменив язык и регион на устройстве или симуляторе на языковой стандарт с направлением потока справа налево, который был указан в **Info.plist**.

> [!WARNING]
> Следует учитывать, что при изменении языка и региона на языковой стандарт с направлением потока справа налево в iOS любые представления [`DatePicker`](xref:Xamarin.Forms.DatePicker) вызовут исключение, если не включить ресурсы, необходимые для этого языкового стандарта. Например, при тестировании приложения на арабском языке, имеющего `DatePicker`, убедитесь, что в разделе **Интернационализация** области **Сборка iOS** выбрано значение **Ближний Восток**.

### <a name="android"></a>Android

Файл **AndroidManifest.xml** приложения нужно обновить, чтобы узел `<uses-sdk>` задавал значение 17 для атрибута `android:minSdkVersion`, а узел `<application>` задавал значение `true` для атрибута `android:supportsRtl`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <uses-sdk android:minSdkVersion="17" ... />
    <application ... android:supportsRtl="true">
    </application>
</manifest>
```

После этого локализацию справа налево можно проверить, настроив устройство или эмулятор для использования языка с направлением потока справа налево либо включив **Force RTL layout direction** (Принудительное направление макета справа налево) в разделе **Параметры > Параметры разработчика**.

### <a name="universal-windows-platform-uwp"></a>Универсальная платформа Windows (UWP)

Необходимые языковые ресурсы следует указать в узле `<Resources>` файла **Package.appxmanifest**. В следующем примере показано добавление арабского языка в узел `<Resources>`:

```xml
<Resources>
    <Resource Language="x-generate"/>
    <Resource Language="en" />
    <Resource Language="ar" />
</Resources>
```

Кроме того, универсальная платформа Windows требует, чтобы язык и региональные параметры приложения по умолчанию были явно определены в библиотеке .NET Standard. Это можно сделать, задав язык и региональные параметры по умолчанию для атрибута `NeutralResourcesLanguage` в `AssemblyInfo.cs` или в другом классе:

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en")]
```

После этого локализацию справа налево протестировать, изменив язык и регион на устройстве на соответствующий языковой стандарт с направлением потока справа налево.

## <a name="limitations"></a>Ограничения

Сейчас локализация справа налево Xamarin.Forms имеет ряд ограничений:

- Расположение кнопок [`NavigationPage`](xref:Xamarin.Forms.NavigationPage), расположение элементов панели инструментов и анимация переходов определяются языковым стандартом устройства, а не свойством [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection).
- Направление прокрутки [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) не перевертывается.
- Визуальное содержимое [`Image`](xref:Xamarin.Forms.Image) не перевертывается.
- Ориентация [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) и [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])) определяется языковым стандартом устройства, а не свойством [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection).
- Содержимое [`WebView`](xref:Xamarin.Forms.WebView) не учитывает свойство [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection).
- Требуется добавить свойство `TextDirection` для управления выравниванием текста.

### <a name="ios"></a>iOS

- Ориентация [`Stepper`](xref:Xamarin.Forms.Stepper) определяется языковым стандартом устройства, а не свойством [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection).
- Выравнивание текста [`EntryCell`](xref:Xamarin.Forms.EntryCell) определяется языковым стандартом устройства, а не свойством [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection).
- Жесты и выравнивание [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) невозможно обратить.

### <a name="android"></a>Android

- Ориентация [`SearchBar`](xref:Xamarin.Forms.SearchBar) определяется языковым стандартом устройства, а не свойством [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection).
- Расположение [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) определяется языковым стандартом устройства, а не свойством [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection).

### <a name="uwp"></a>UWP

- Выравнивание текста [`Editor`](xref:Xamarin.Forms.Editor) определяется языковым стандартом устройства, а не свойством [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection).
- Свойство [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) не наследуется дочерними объектами [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage).
- Выравнивание текста [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) определяется языковым стандартом устройства, а не свойством [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection).

## <a name="force-right-to-left-layout"></a>Принудительное использование макета с написанием справа налево

В приложениях Xamarin.iOS и Xamarin.Android можно на постоянной основе принудительно использовать макет с написанием справа налево независимо от параметров устройства, изменив соответствующие проекты платформы.

### <a name="ios"></a>iOS

В приложениях Xamarin.iOS можно на постоянной основе принудительно использовать макет с написанием справа налево, изменив класс **AppDelegate** следующим образом:

1. Объявите функцию `IntPtr_objc_msgSend` как первую строку в классе `AppDelegate`:

   ```csharp
   [System.Runtime.InteropServices.DllImport(ObjCRuntime.Constants.ObjectiveCLibrary, EntryPoint = "objc_msgSend")]
   internal extern static IntPtr IntPtr_objc_msgSend(IntPtr receiver, IntPtr selector, UISemanticContentAttribute arg1);
   ```

1. Перед возвратом из метода `FinshedLaunching` вызовите функцию `IntPtr_objc_msgSend` из метода `FinishedLaunching`:

   ```csharp
   bool result = base.FinishedLaunching(app, options);

   ObjCRuntime.Selector selector = new ObjCRuntime.Selector("setSemanticContentAttribute:");
   IntPtr_objc_msgSend(UIView.Appearance.Handle, selector.Handle, UISemanticContentAttribute.ForceRightToLeft);

   return result;
   ```

Такой подход удобен для приложений, которым постоянно требуется макет с написанием справа налево, и устраняет необходимость задавать свойство [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection).

Дополнительные сведения о методе `IntrPtr_objc_msgSend` см. в разделе [Селекторы Objective-C в Xamarin.iOS](~/ios/internals/objective-c-selectors.md).

### <a name="android"></a>Android

В приложениях Xamarin.Android можно на постоянной основе принудительно использовать макет с написанием справа налево, включив в класс **MainActivity** следующую строку:

```csharp
Window.DecorView.LayoutDirection = LayoutDirection.Rtl;
```

Такой подход удобен для приложений, которым постоянно требуется макет с написанием справа налево, и устраняет необходимость задавать свойство [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection).

## <a name="right-to-left-language-support-with-xamarinuniversity"></a>Поддержка языков с направлением потока справа налево с использованием Xamarin.University

> [!VIDEO https://youtube.com/embed/f2lQ5yw3iiU]

**Видеоролик о поддержке написания справа налево в Xamarin.Forms 3.0**

## <a name="related-links"></a>Связанные ссылки

- [Пример приложения TodoLocalizedRTL](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todolocalizedrtl)
