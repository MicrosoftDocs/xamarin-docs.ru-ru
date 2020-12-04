---
title: Шрифты в Xamarin.Forms
description: В этой статье объясняется, как задать сведения о шрифтах для элементов управления, отображающих текст в Xamarin.Forms приложениях.
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/04/2020
ms.custom: contperfq2
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 271c6c5e510a892919b5d87c4dbc38ad8e9d657d
ms.sourcegitcommit: 342cfbd2502ad92cadada4fa9aec669b99d7830a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2020
ms.locfileid: "96604577"
---
# <a name="fonts-in-no-locxamarinforms"></a>Шрифты в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/workingwithfonts)

По умолчанию Xamarin.Forms использует системный шрифт, определенный каждой платформой. Однако элементы управления, отображающие текст, определяют свойства, которые можно использовать для изменения этого шрифта:

- `FontAttributes`Тип `FontAttributes` , который является перечислением с тремя элементами: `None` , `Bold` и `Italic` . Значение по умолчанию этого свойства равно `None`.
- `FontSize` типа `double`.
- `FontFamily` типа `string`.

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

## <a name="set-font-attributes"></a>Задание атрибутов шрифта

Элементы управления, отображающие текст, могут задавать `FontAttributes` атрибуты шрифта для свойства:

```xaml
<Label Text="Italics"
       FontAttributes="Italic" />
<Label Text="Bold and italics"
       FontAttributes="Bold, Italic" />
```

Эквивалентный код на C# выглядит так:

```csharp
Label label1 = new Label
{
    Text = "Italics",
    FontAttributes = FontAttributes.Italic
};

Label label2 = new Label
{
    Text = "Bold and italics",
    FontAttributes = FontAttributes.Bold | FontAttributes.Italic
};    
```

## <a name="set-the-font-size"></a>Задать размер шрифта

Элементы управления, отображающие текст, могут задать `FontSize` для свойства Размер шрифта. `FontSize`Для свойства можно задать `double` значение напрямую или [`NamedSize`](xref:Xamarin.Forms.NamedSize) значение перечисления:

```xaml
<Label Text="Font size 24"
       FontSize="24" />
<Label Text="Large font size"
       FontSize="Large" />
```

Эквивалентный код на C# выглядит так:

```csharp
Label label1 = new Label
{
    Text = "Font size 24",
    FontSize = 24
};

Label label2 = new Label
{
    Text = "Large font size",
    FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label))
};
```

Кроме того, `Device.GetNamedSize` метод имеет переопределение, которое указывает второй аргумент в качестве [`Element`](xref:Xamarin.Forms.Element) :

```csharp
Label myLabel = new Label
{
    Text = "Large font size",
};
myLabel.FontSize = Device.GetNamedSize(NamedSize.Large, myLabel);
```

> [!NOTE]
> `FontSize`Значение, указанное как `double` , измеряется в единицах, не зависящих от устройства. Дополнительные сведения см. в разделе [единицы измерения](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement).

Дополнительные сведения об именованных размерах шрифтов см. в разделе [сведения о размерах шрифтов](#understand-named-font-sizes).

## <a name="set-the-font-family"></a>Задание семейства шрифтов

Элементы управления, отображающие текст, могут задать `FontFamily` для свойства имя семейства шрифтов, например Times Roman. Однако это будет работать только в том случае, если семейство шрифтов поддерживается на конкретной платформе.

Существует ряд методов, с помощью которых можно попытаться получить шрифты, доступные на платформе. Однако наличие файла шрифта TTF (формат типа true) не обязательно подразумевает семейство шрифтов, и часто включаются Ттфс, которые не предназначены для использования в приложениях. Кроме того, шрифты, установленные на платформе, могут измениться в версии платформы. Таким образом, самым надежным подходом для указания семейства шрифтов является использование пользовательского шрифта.

Пользовательские шрифты можно добавить в Xamarin.Forms общий проект и использовать в проектах платформы без дополнительной работы. Чтобы этого добиться, выполните следующие действия.

1. Добавьте шрифт в Xamarin.Forms общий проект в качестве внедренного ресурса (**действие сборки: EmbeddedResource**).
1. Зарегистрируйте файл шрифта в сборке в файле, например **AssemblyInfo.CS**, с помощью `ExportFont` атрибута. Также можно указать необязательный псевдоним.

В следующем примере показан Lobster-Regularный шрифт, регистрируемый в сборке вместе с псевдонимом:

```csharp
using Xamarin.Forms;

[assembly: ExportFont("Lobster-Regular.ttf", Alias = "Lobster")]
```

> [!NOTE]
> Шрифт может находиться в любой папке в общем проекте без указания имени папки при регистрации шрифта в сборке.
>
> В Windows имя файла шрифта и имя шрифта могут отличаться. Чтобы обнаружить имя шрифта в Windows, щелкните TTF-файл правой кнопкой мыши и выберите **Предварительный просмотр**. Затем имя шрифта можно определить в окне предварительного просмотра.

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
> Для сборок выпуска в Windows убедитесь, что сборка, содержащая пользовательский шрифт, передается в качестве аргумента в `Forms.Init` вызове метода. Дополнительные сведения см. в [руководстве по устранению неполадок](~/xamarin-forms/platform/windows/installation/index.md#troubleshooting).

## <a name="set-font-properties-per-platform"></a>Установка свойств шрифта для каждой платформы

[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1)Классы и [`On`](xref:Xamarin.Forms.On) можно использовать в XAML для установки свойств шрифта для каждой платформы. В приведенном ниже примере для каждой платформы устанавливаются разные гарнитуры и размеры шрифтов:

```xaml
<Label Text="Different font properties on different platforms"
       FontSize="{OnPlatform iOS=20, Android=Medium, UWP=24}">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
            <On Platform="iOS" Value="MarkerFelt-Thin" />
            <On Platform="Android" Value="Lobster-Regular" />
            <On Platform="UWP" Value="ArimaMadurai-Black" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#provide-platform-specific-values)Свойство можно использовать в коде для задания свойств шрифта для каждой платформы

```csharp
Label label = new Label
{
    Text = "Different font properties on different platforms"
};

label.FontSize = Device.RuntimePlatform == Device.iOS ? 20 :
    Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : 24;
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "MarkerFelt-Thin" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular" : "ArimaMadurai-Black";
```

Дополнительные сведения о предоставлении значений для конкретной платформы см. в разделе [Указание значений для конкретной платформы](~/xamarin-forms/platform/device.md#provide-platform-specific-values). Дополнительные сведения о `OnPlatform` расширении разметки см. в разделе [расширение разметки для платформы](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension).

## <a name="understand-named-font-sizes"></a>Общие сведения об именованных размерах шрифтов

Xamarin.Forms Определяет поля в [`NamedSize`](xref:Xamarin.Forms.NamedSize) перечислении, представляющие определенные размеры шрифтов. В следующей таблице показаны `NamedSize` элементы и их размеры по умолчанию для iOS, Android и универсальная платформа Windows (UWP).

| Член | iOS | Android | UWP |
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

> [!NOTE]
> В iOS и Android именованные размеры шрифтов будут автоматически масштабироваться на основе специальных возможностей операционной системы. Это поведение можно отключить в iOS с конкретной платформой. Дополнительные сведения см. [в разделе масштабирование специальных возможностей для именованных размеров шрифтов в iOS](~/xamarin-forms/platform/ios/named-font-size-scaling.md).

## <a name="display-font-icons"></a>Отображение значков шрифтов

Значки шрифтов могут отображаться Xamarin.Forms приложениями путем указания данных значка шрифта в `FontImageSource` объекте. Этот класс, производный от [`ImageSource`](xref:Xamarin.Forms.ImageSource) класса, имеет следующие свойства:

- `Glyph` — значение символа Юникода значка шрифта, указанного как `string` .
- `Size` — `double` значение, указывающее размер (в единицах, независимых от устройства) значка отображаемого шрифта. Значение по умолчанию — 30. Кроме того, для этого свойства можно задать именованный размер шрифта.
- `FontFamily` — Объект, `string` представляющий семейство шрифтов, которому принадлежит значок шрифта.
- `Color` — необязательное [`Color`](xref:Xamarin.Forms.Color) значение, используемое при отображении значка шрифта.

Эти данные используются для создания PNG-изображения, которое может отображаться в любом представлении, которое может отображать `ImageSource` . Такой подход позволяет отображать значки шрифтов, например эмодзи, в нескольких представлениях, в отличие от отображения значка шрифта в представлении одного текста, например [`Label`](xref:Xamarin.Forms.Label) .

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

Этот код отображает значок XBox из семейства шрифтов Иониконс в [`Image`](xref:Xamarin.Forms.Image) представлении. Обратите внимание, что хотя символ Юникода для этого значка имеет значение `\uf30c` , он должен быть экранирован в XAML и, таким образом, стал `&#xf30c;` . Эквивалентный код на C# выглядит так:

```csharp
Image image = new Image { BackgroundColor = Color.FromHex("#D1D1D1") };
image.Source = new FontImageSource
{
    Glyph = "\uf30c",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Ionicons" : "ionicons.ttf#",
    Size = 44
};
```

На следующих снимках экрана из примера " [связываемые макеты](/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts) " показаны несколько значков шрифтов, отображаемых с помощью связываемого макета:

![Снимок экрана с отображаемыми значками шрифтов в iOS и Android](fonts-images/font-image-source.png "Значки шрифтов, отображаемые в представлении изображений")

## <a name="related-links"></a>Связанные ссылки

- [фонтссампле](/samples/xamarin/xamarin-forms-samples/workingwithfonts)
- [Текст (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Макеты с возможностью привязки (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [Укажите значения, зависящие от платформы](~/xamarin-forms/platform/device.md#provide-platform-specific-values)
- [Расширение разметки для платформы](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension)
- [Привязываемые макеты](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
