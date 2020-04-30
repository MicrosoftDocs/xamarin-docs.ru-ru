---
title: Шрифты в Xamarin. Forms
description: В этой статье объясняется, как задать сведения о шрифтах для элементов управления, отображающих текст в приложениях Xamarin. Forms.
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2020
ms.openlocfilehash: 160cbbfa99114d74fa5fdaa5f92b8397ea7d3367
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82516477"
---
# <a name="fonts-in-xamarinforms"></a>Шрифты в Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)

В этой статье описывается, как Xamarin. Forms позволяет задавать атрибуты шрифта (включая вес и размер) для элементов управления, отображающих текст. Сведения о шрифтах можно [указать в коде](#Setting_Font_in_Code) или [в XAML](#Setting_Font_in_Xaml). Также можно использовать [пользовательский шрифт](#use-a-custom-font)и [Отображать значки шрифтов](#display-font-icons).

<a name="Setting_Font_in_Code" />

## <a name="set-the-font-in-code"></a>Установка шрифта в коде

Используйте три свойства, относящиеся к шрифту, для элементов управления, отображающих текст:

- **FontFamily** &ndash; имя `string` шрифта.
- **FontSize** &ndash; размер шрифта как `double`.
- **Фонтаттрибутес** &ndash; строка, указывающая сведения о стиле, такие как *курсив* и `FontAttributes` **полужирный шрифт** (с помощью перечисления в C#).

В этом коде показано, как создать метку и указать размер и вес шрифта для отображения:

```csharp
var about = new Label
{
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>Размер шрифта

Для `FontSize` свойства можно задать значение Double, например:

```csharp
label.FontSize = 24;
```

Значение размера измеряется в единицах, не зависящих от устройства. Дополнительные сведения см. в разделе [единицы измерения](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement).

Xamarin. Forms также определяет поля в [`NamedSize`](xref:Xamarin.Forms.NamedSize) перечислении, представляющие определенные размеры шрифтов. Дополнительные сведения об именованных размерах шрифтов см. в разделе [размеры именованных](#named-font-sizes)шрифтов.

<a name="FontAttributes" />

### <a name="font-attributes"></a>Атрибуты шрифта

Для `FontAttributes` свойства можно задать стили шрифта, такие как **полужирный шрифт** и *курсив* . В настоящее время поддерживаются следующие значения:

- **Нет**
- **Делять**
- **Деле**

`FontAttribute` Перечисление можно использовать следующим образом (можно указать один атрибут или `OR` их совместно):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="set-font-info-per-platform"></a>Задание сведений о шрифте для платформы

Кроме того, `Device.RuntimePlatform` свойство можно использовать для задания различных названий шрифтов на каждой платформе, как показано в следующем коде:

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "MarkerFelt-Thin" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

Хорошим источником сведений о шрифтах для iOS является [iosfonts.com](http://iosfonts.com).

<a name="Setting_Font_in_Xaml" />

## <a name="set-the-font-in-xaml"></a>Установка шрифта в XAML

Элементы управления Xamarin. Forms, отображающие текст, `FontSize` имеют свойство, которое можно задать в XAML. Самый простой способ задать шрифт в XAML — использовать значения перечисления именованного размера, как показано в следующем примере:

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

Существует встроенный преобразователь для `FontSize` свойства, который позволяет выражать все параметры шрифта как строковое значение в XAML. Кроме того `FontAttributes` , свойство можно использовать для указания атрибутов шрифта:

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#provide-platform-specific-values) Свойство также может использоваться в XAML для отображения различных шрифтов на каждой платформе. В приведенном ниже примере используются разные шрифты на каждой платформе:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

## <a name="named-font-sizes"></a>Именованные размеры шрифтов 

Xamarin. Forms определяет поля в [`NamedSize`](xref:Xamarin.Forms.NamedSize) перечислении, представляющие определенные размеры шрифтов. В следующей таблице показаны `NamedSize` элементы и их размеры по умолчанию для iOS, Android и универсальная платформа Windows (UWP).

| Участник | iOS | Android | UWP |
| --- | --- | --- | --- |
| `Default` | 16 | 14 | 14 |
| `Micro` | 11 | 10 | 15,667 |
| `Small` | 13 | 14 | 18,667 |
| `Medium` | 16 | 17 | 22,667 |
| `Large` | 20 | 22 | 32 |
| `Body` | 17 | 16 | 14 |
| `Header` | 17 | 96 | 46 |
| `Title` | 28 | 24 | 24 |
| `Subtitle` | 22 | 16 | 20 |
| `Caption` | 12 | 12 | 12 |

Значения размера измеряются в единицах, не зависящих от устройства. Дополнительные сведения см. в разделе [единицы измерения](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement).

Именованные размеры шрифтов можно задать с помощью XAML и кода. Кроме того, `Device.GetNamedSize` метод может быть вызван для возврата `double` , представляющего именованный размер шрифта:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

> [!NOTE]
> В iOS и Android именованные размеры шрифтов будут автоматически масштабироваться на основе специальных возможностей операционной системы. Это поведение можно отключить в iOS с конкретной платформой. Дополнительные сведения см. [в разделе масштабирование специальных возможностей для именованных размеров шрифтов в iOS](~/xamarin-forms/platform/ios/named-font-size-scaling.md).

## <a name="use-a-custom-font"></a>Использовать пользовательский шрифт

Пользовательские шрифты можно добавить в общий проект Xamarin. Forms и использовать в проектах платформы без дополнительной работы. Чтобы этого добиться, выполните следующие действия.

1. Добавьте шрифт в общий проект Xamarin. Forms в качестве внедренного ресурса (**действие сборки: EmbeddedResource**).
1. Зарегистрируйте файл шрифта в сборке в файле, например **AssemblyInfo.CS**, с помощью `ExportFont` атрибута. Также можно указать необязательный псевдоним.

> [!IMPORTANT]
> Внедренные шрифты требуют использования Xamarin. Forms 4.5.0.530 или более поздней версии.

В следующем примере показан Лобстер-Regular шрифт, регистрируемый в сборке вместе с псевдонимом:

```csharp
using Xamarin.Forms;

[assembly: ExportFont("Lobster-Regular.ttf", Alias = "Lobster")]
```

> [!NOTE]
> Шрифт может находиться в любой папке в общем проекте без указания имени папки при регистрации шрифта в сборке.

Затем шрифт можно использовать на каждой платформе, ссылаясь на его имя, без расширения файла:

```xaml
<!-- Use font name -->
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster-Regular" />
```

Кроме того, его можно использовать на каждой платформе, ссылаясь на его псевдоним:

```xaml
<!-- Use font alias -->
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster" />
```

Эквивалентный код на C# выглядит так:

```csharp
// Use font name
Label label1 = new Label
{
    Text = "Hello Xamarin.Forms!",
    FontFamily = "Lobster-Regular"
};

// Use font alias
Label label2 = new Label
{
    Text = "Hello Xamarin.Forms!",
    FontFamily = "Lobster"
};
```

На следующих снимках экрана показан пользовательский шрифт:

[![Пользовательский шрифт в iOS и Android](fonts-images/custom-sml.png "Пример пользовательских шрифтов")](fonts-images/custom.png#lightbox "Пример пользовательских шрифтов")

> [!IMPORTANT]
> В Windows имя файла шрифта и имя шрифта могут отличаться. Чтобы обнаружить имя шрифта в Windows, щелкните TTF-файл правой кнопкой мыши и выберите **Предварительный просмотр**. Затем имя шрифта можно определить в окне предварительного просмотра.

## <a name="display-font-icons"></a>Отображение значков шрифтов

Значки шрифтов могут отображаться приложениями Xamarin. Forms путем указания данных значка шрифта в `FontImageSource` объекте. Этот класс, производный от [`ImageSource`](xref:Xamarin.Forms.ImageSource) класса, имеет следующие свойства:

- `Glyph`— значение символа Юникода значка шрифта, указанного как `string`.
- `Size`— `double` значение, указывающее размер (в единицах, независимых от устройства) значка отображаемого шрифта. Значение по умолчанию — 30. Кроме того, для этого свойства можно задать именованный размер шрифта.
- `FontFamily`— Объект `string` , представляющий семейство шрифтов, которому принадлежит значок шрифта.
- `Color`— необязательное [`Color`](xref:Xamarin.Forms.Color) значение, используемое при отображении значка шрифта.

Эти данные используются для создания PNG-изображения, которое может отображаться в любом представлении, которое может отображать `ImageSource`. Такой подход позволяет отображать значки шрифтов, например эмодзи, в нескольких представлениях, в отличие от отображения значка шрифта в представлении одного текста, например [`Label`](xref:Xamarin.Forms.Label).

> [!IMPORTANT]
> Значки шрифтов могут быть заданы только в символьном представлении в Юникоде.

В следующем примере XAML имеется один значок шрифта, отображаемый [`Image`](xref:Xamarin.Forms.Image) представлением:

```xaml
<Image BackgroundColor="#D1D1D1">
    <Image.Source>
        <FontImageSource Glyph="&#xf30c;"
                         FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}"
                         Size="44" />
    </Image.Source>
</Image>
```

Этот код отображает значок XBox из семейства шрифтов Иониконс в [`Image`](xref:Xamarin.Forms.Image) представлении. Обратите внимание, что хотя символ Юникода для этого `\uf30c`значка имеет значение, он должен быть ЭКРАНИРОВАН в XAML `&#xf30c;`и, таким образом, стал. Эквивалентный код на C# выглядит так:

```csharp
Image image = new Image { BackgroundColor = Color.FromHex("#D1D1D1") };
image.Source = new FontImageSource
{
    Glyph = "\uf30c",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Ionicons" : "ionicons.ttf#",
    Size = 44
};
```

На следующих снимках экрана из примера " [связываемые макеты](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts) " показаны несколько значков шрифтов, отображаемых с помощью связываемого макета:

![Снимок экрана с отображаемыми значками шрифтов в iOS и Android](fonts-images/font-image-source.png "Значки шрифтов, отображаемые в представлении изображений")

## <a name="related-links"></a>Связанные ссылки

- [фонтссампле](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)
- [Текст (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Макеты с возможностью привязки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [Привязываемые макеты](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
