---
title: Локализация строк и изображений в Xamarin.Forms
description: Приложения Xamarin.Forms можно локализовать с помощью файлов ресурсов .NET.
zone_pivot_groups: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: af15dc5a23404a11be6207bef7b4fc3e4bf9fad7
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137609"
---
# <a name="xamarinforms-string-and-image-localization"></a>Локализация строк и изображений в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/usingresxlocalization)

Локализация — это процедура адаптации приложения к конкретному языку и культурным особенностям определенного рынка. Для локализации может потребоваться перевести текст и изображения в приложении на несколько языков. Локализованное приложение автоматически отображает переведенный текст в соответствии с языком и региональными параметрами, выбранными на мобильном устройстве.

![Снимки экрана: локализованное приложения в iOS и Android](text-images/localizationdemo-screenshots.png)

Платформа .NET Framework включает встроенный механизм для локализации приложений с помощью [файлов ресурсов RESX](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps). В файле ресурсов текст и другое содержимое хранится в виде пар "имя-значение", что позволяет приложению извлекать содержимое по указанному ключу. Файлы ресурсов позволяют отделить локализованное содержимое от кода приложения.

Для локализации приложений Xamarin.Forms с помощью файлов ресурсов необходимо выполнить следующие действия:

1. [создать файлы RESX](#create-resx-files), содержащие переведенный текст;
1. [указать язык и региональные параметры по умолчанию](#specify-the-default-culture) в общем проекте;
1. [локализовать текст в Xamarin.Forms](#localize-text-in-xamarinforms);
1. [локализовать изображения](#localize-images) в соответствии с языком и региональными параметрами для каждой платформы;
1. [локализовать имя приложения](#localize-the-application-name) на каждой платформе;
1. [протестировать локализацию](#test-localization) на каждой платформе.

## <a name="create-resx-files"></a>Создание RESX-файлов

Файлы ресурсов — это XML-файлы с расширением **RESX**, которые компилируются в двоичные файлы ресурсов (RESOURCES) в процессе сборки. Visual Studio 2019 создает класс, который предоставляет интерфейс API для извлечения ресурсов. Локализованное приложение обычно содержит файл ресурсов по умолчанию со всеми строками, используемыми в приложении, а также файлы ресурсов для каждого поддерживаемого языка. В примере приложения в общем проекте имеется папка **RESX**, которая содержит файлы ресурсов. Файл ресурсов по умолчанию называется **AppResources.resx**.

Файлы ресурсов содержат следующие сведения для каждого элемента:

- **Name** (Имя) — ключ, используемый для доступа к тексту в коде;
- **Value** (Значение) — переведенный текст;
- **Comment** (Примечание) — необязательное поле, содержащее дополнительные сведения.

::: zone pivot="windows"

Файл ресурсов добавляется в диалоговом окне **Добавление нового элемента** в Visual Studio 2019.

![Добавление нового ресурса в Visual Studio 2019](text-images/pc-add-resource-file.png)

После добавления файла можно добавить строки для каждого текстового ресурса.

![Указание текстовых ресурсов по умолчанию в RESX-файле](text-images/pc-default-strings.png)

Значение, выбранное в раскрывающемся списке **Модификатор доступа**, определяет то, как Visual Studio создает класс, используемый для доступа к ресурсам. Если выбрать **общедоступный** или **внутренний** модификатор доступа, будет создан класс с соответствующим уровнем доступности. Если выбрать модификатор доступа **Без создания кода**, файл класса не создается. Для создания файла класса необходимо настроить файл ресурсов по умолчанию. В результате в проект будет добавлен файл с расширением **.designer.cs**.

После создания файла ресурсов по умолчанию можно создать дополнительные файлы для каждого языка и региональных параметров, поддерживаемых приложением. В имени каждого дополнительного файла ресурсов должен быть указан целевой язык и региональные параметры, а в качестве **модификатора доступа** должно быть выбрано значение **Без создания кода**. 

Во время выполнения приложение пытается разрешить запрос ресурса в порядке от более конкретного к более общему. Например, если на устройстве выбраны язык и региональные параметры **ru-RU**, приложение ищет файлы ресурсов в следующем порядке:

1. AppResources.ru-RU.resx
1. AppResources.ru.resx
1. AppResources.resx (по умолчанию)

На следующем снимке экрана показан файл с переводом на испанский язык, который называется **AppResources.es.cs**:

![Указание текстовых ресурсов по умолчанию в RESX-файле](text-images/pc-spanish-strings.png)

В файле с переводом поля **Name** содержат те же значения, что и в файле по умолчанию, но в столбце **Value** содержатся строки, переведенные на испанский язык. Кроме того, выбран **модификатор доступа** **Без создания кода**.

::: zone-end
::: zone pivot="macos"

В Visual Studio 2019 для Mac файл ресурсов добавляется в диалоговом окне **Добавление нового файла**.

![Добавление нового ресурса в Visual Studio 2019 для Mac](text-images/mac-add-resource-file.png)

После создания файла ресурсов по умолчанию можно добавить текст, создав элементы `data` в элементе `root` в файле ресурсов:

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
    ...
    <data name="AddButton" xml:space="preserve">
        <value>Add Note</value>
    </data>
    <data name="NotesLabel" xml:space="preserve">
        <value>Notes:</value>
    </data>
    <data name="NotesPlaceholder" xml:space="preserve">
        <value>e.g. Get Milk</value>
    </data>
</root>
```

Файл класса **.designer.cs** можно создать, задав свойство **Специальный инструмент** в параметрах файла ресурсов:

![Указание специального инструмента в свойствах файла ресурсов](text-images/mac-resx-properties.png)

Если в качестве **специального инструмента** выбрать **PublicResXFileCodeGenerator**, будет создан класс с уровнем доступа `public`. Если в качестве **специального инструмента** выбрать **InternalResXFileCodeGenerator**, будет создан класс с уровнем доступа `internal`. Если оставить значение **Специальный инструмент** пустым, класс создан не будет. Имя созданного класса будет совпадать с именем файла ресурсов. Например, если файл ресурсов называется **AppResources.resx**, класс `AppResources` будет создан в файле с именем **AppResources.designer.cs**.

Для каждого поддерживаемого языка и региональных параметров можно создать дополнительные файлы ресурсов. В имени файла для каждого языка должен быть указан целевой язык и региональные параметры. Так, файл для **es-MX** должен называться **AppResources.es-MX.resx**.

Во время выполнения приложение пытается разрешить запрос ресурса в порядке от более конкретного к более общему. Например, если на устройстве выбраны язык и региональные параметры **ru-RU**, приложение ищет файлы ресурсов в следующем порядке:

1. AppResources.ru-RU.resx
1. AppResources.ru.resx
1. AppResources.resx (по умолчанию)

В файлах с переводом поля **Name** должны содержаться те же значения, что и в файле по умолчанию. Ниже представлен код XML в файле **AppResources.es.resx** с переводом на испанский язык.

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
    ...
    <data name="NotesLabel" xml:space="preserve">
        <value>Notas:</value>
    </data>
    <data name="NotesPlaceholder" xml:space="preserve">
        <value>por ejemplo . comprar leche</value>
    </data>
    <data name="AddButton" xml:space="preserve">
        <value>Agregar nuevo elemento</value>
    </data>
</root>
```

::: zone-end

## <a name="specify-the-default-culture"></a>Указание языка и региональных параметров по умолчанию

Для правильной работы файлов ресурсов в приложении необходимо указать `NeutralResourcesLanguage`. Язык и региональные параметры по умолчанию должны быть указаны в общем проекте в файле **AssemblyInfo.cs**. В следующем примере кода показано, как присвоить `NeutralResourcesLanguage` значение **en-US** в файле **AssemblyInfo.cs**:

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en-US")]
```

> [!WARNING]
> Если атрибут `NeutralResourcesLanguage` не указан, класс `ResourceManager` возвращает значения `null` для всех языков и региональных параметров, для которых файлы ресурсов отсутствуют. Если язык и региональные параметры по умолчанию указаны, `ResourceManager` возвращает результаты из файла RESX по умолчанию для неподдерживаемых языков и региональных параметров. Поэтому рекомендуется всегда указывать `NeutralResourcesLanguage`, чтобы для неподдерживаемых языков и региональных параметров текст отображался.

После создания файла ресурсов по умолчанию и указания языка и региональных параметров по умолчанию в файле **AssemblyInfo.cs** приложение может извлекать локализованные строки во время выполнения.

Дополнительные сведения о файлах ресурсов см. в статье [Создание файлов ресурсов для приложений .NET](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps).

## <a name="localize-text-in-xamarinforms"></a>Локализация текста в Xamarin.Forms

Текст в Xamarin.Forms локализуется с помощью создаваемого класса `AppResources`. Его имя совпадает с именем файла ресурсов по умолчанию. Так как файл ресурсов в примере проекта называется **AppResources.cs**, Visual Studio создает соответствующий класс с именем `AppResources`. Для каждой строки в файле ресурсов в классе `AppResources` создаются статические свойства. В классе `AppResources` для примера приложения создаются следующие статические свойства:

- AddButton
- NotesLabel
- NotesPlaceholder

Доступ к этим значениям как к свойствам [x:Static](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md#the-xstatic-markup-extension) позволяет отображать локализованный текст в XAML:

```xaml
<ContentPage ...
             xmlns:resources="clr-namespace:LocalizationDemo.Resx">
    <Label Text="{x:Static resources:AppResources.NotesLabel}" />
    <Entry Placeholder="{x:Static resources:AppResources.NotesPlaceholder}" />
    <Button Text="{x:Static resources:AppResources.AddButton}" />
</ContentPage>
```

Локализованный текст также можно извлекать в коде:

```csharp
public LocalizedCodePage()
{
    Label notesLabel = new Label
    {
        Text = AppResources.NotesLabel,
        // ...
    };
    
    Entry notesEntry = new Entry
    {
        Placeholder = AppResources.NotesPlaceholder,
        //...
    };
    
    Button addButton = new Button
    {
        Text = AppResources.AddButton,
        // ...
    };
    
    Content = new StackLayout
    {
        Children = {
            notesLabel,
            notesEntry,
            addButton
        }
    };
}
```

Чтобы определить, из какого файла ресурсов следует извлечь значения, свойства класса `AppResources` используют текущее значение `System.Globalization.CultureInfo.CurrentUICulture`.

## <a name="localize-images"></a>Локализация изображений

Помимо текста, в файлах RESX можно хранить изображения и двоичные данные. Однако мобильные устройства имеют экраны разного размера и плотности, а каждая мобильная платформа имеет функции для показа изображений с учетом плотности. Поэтому вместо хранения изображений в файлах ресурсов следует использовать функции локализации изображений, предоставляемые платформой.

### <a name="localize-images-on-android"></a>Локализация изображений на Android

На Android локализованные прорисовываемые ресурсы (изображения) хранятся в соответствии с соглашением об именовании папок в каталоге **Resources**. Имена папок состоят из слова **drawable** и целевого языка в виде суффикса. Например, папка для испанского языка называется **drawable-es**.

Когда нужен четырехбуквенный код языкового стандарта, Android требует дополнительную букву **r** после дефиса. Например, папка для мексиканского языкового стандарта (es-MX) должна называться **drawable-es-rMX**. Имена файлов изображений в папке каждого языкового стандарта должны быть одинаковыми:

![Локализованные изображения в проекте для Android](text-images/pc-android-images.png)

Дополнительные сведения см. в статье [Локализация в Android](~/android/app-fundamentals/localization.md).

### <a name="localize-images-on-ios"></a>Локализация изображений в iOS

В iOS локализованные изображения хранятся в соответствии с соглашением об именовании папок в каталоге **Resources**. Папка по умолчанию имеет имя **Base.lproj**. Имена папок для конкретных языков состоят из названия языка или языкового стандарта, за которым следует **.lproj**. Например, папка для испанского языка называется **es.lproj**.

Коды языковых стандартов из четырех символов используются так же, как и двухбуквенные коды языков. Например, папка для мексиканского языкового стандарта (es-MX) должна называться **es-MX.lproj**. Имена файлов изображений в папке каждого языкового стандарта должны быть одинаковыми:

![Локализованные изображения в проекте для iOS](text-images/pc-ios-images.png)

> [!NOTE]
> iOS поддерживает создание локализованного каталога ресурсов вместо использования структуры папок .lproj. Однако их следует создавать и управлять ими в Xcode.

Дополнительные сведения см. в статье [Локализация в iOS](~/ios/app-fundamentals/localization/index.md).

### <a name="localize-images-on-uwp"></a>Локализация изображений в UWP

В UWP локализованные изображения хранятся в соответствии с соглашением об именовании папок в каталоге **Assets/Images**. Имена папок совпадают с названиями языков или языковых стандартов. Например, папка для испанского языка называется **es**, а для мексиканского языкового стандарта — **es-MX**. Имена файлов изображений в папке каждого языкового стандарта должны быть одинаковыми:

![Локализованные изображения в проекте для UWP](text-images/pc-uwp-images.png)

Подробнее об этом см. в разделе [Локализация на универсальной платформе Windows](/windows/uwp/design/globalizing/globalizing-portal/).

### <a name="consume-localized-images"></a>Использование локализованных изображений

Так как на каждой платформе для хранения изображений используется уникальная структура файлов, XAML использует класс `OnPlatform` для задания свойства `ImageSource` в зависимости от текущей платформы:

```xaml
<Image>
    <Image.Source>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="flag.png" />
            <On Platform="UWP" Value="Assets/Images/flag.png" />
        </OnPlatform>
    </Image.Source>
</Image>
```

> [!NOTE]
> Расширение разметки `OnPlatform` позволяет более лаконично указывать значения для конкретных платформ. Дополнительные сведения см. в разделе [Расширение разметки OnPlatform](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension).

Источник изображения можно указать в коде на основе свойства `Device.RuntimePlatform`:

```csharp
string imgSrc = Device.RuntimePlatform == Device.UWP ? "Assets/Images/flag.png" : "flag.png";
Image flag = new Image
{
    Source = ImageSource.FromFile(imgSrc),
    WidthRequest = 100
};
```

## <a name="localize-the-application-name"></a>Локализация имени приложения

Имя приложения указывается для каждой платформы без использования файлов ресурсов RESX. Сведения о локализации имени приложения на Android см. в статье [Локализация имени приложения на Android](~/android/app-fundamentals/localization.md#stringsxml-file-format). Сведения о локализации имени приложения в iOS см. в статье [Локализация имени приложения в iOS](~/ios/app-fundamentals/localization/index.md#app-name). Сведения о локализации имени приложения в UWP см. в статье [Локализация строк в манифесте пакета UWP](https://docs.microsoft.com/windows/uwp/app-resources/localize-strings-ui-manifest).

## <a name="test-localization"></a>Тестирование локализации

Чтобы протестировать локализацию, лучше всего сменить язык на устройстве. Можно также задать значение `System.Globalization.CultureInfo.CurrentUICulture` в коде, но результат будет разным на разных платформах, поэтому в целях тестирования делать это не рекомендуется.

В iOS в приложении "Параметры" можно задать язык для каждого отдельного приложения, не изменяя язык устройства.

На Android настройки языка определяются и кэшируются при запуске приложения. Чтобы после смены языка изменения вступили в силу, может потребоваться перезапустить приложение.

## <a name="related-links"></a>Связанные ссылки

- [Пример проекта локализации](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/usingresxlocalization)
- [Создание файлов ресурсов для приложений .NET](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps)
- [Кроссплатформенная локализация](~/cross-platform/app-fundamentals/localization.md)
- [Использование класса CultureInfo (MSDN)](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo)
- [Локализация в Android](~/android/app-fundamentals/localization.md)
- [Локализация в iOS](~/ios/app-fundamentals/localization/index.md)
- [Локализация на универсальной платформе Windows](/windows/uwp/design/globalizing/globalizing-portal/)
- [Обнаружение и использование ресурсов для определенного языка и региональных параметров (MSDN)](https://msdn.microsoft.com/library/s9ckwb4b%28v=vs.90%29.aspx)
