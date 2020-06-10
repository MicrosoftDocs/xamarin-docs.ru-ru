---
title: Fonts
ms.prod: xamarin
ms.assetid: 3F543FC5-FDED-47F8-8D2C-481FCC98BFDA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/09/2018
ms.openlocfilehash: daeebc4d1531e340b305b810096b72094ab9230d
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84566374"
---
# <a name="fonts"></a>Fonts

## <a name="overview"></a>Обзор

Начиная с API уровня 26, пакет SDK для Android позволяет обрабатывать шрифты как ресурсы, как и макеты или драваблес. [Библиотека поддержки Android 26](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/26.1.0.1) отправит новый API шрифта в приложения, предназначенные для API уровня 14 или выше.

После нацеливания API 26 или установки V26 библиотеки поддержки Android существует два способа использования шрифтов в приложении Android:

1. **Упаковка шрифта как ресурса Android** &ndash; Это гарантирует, что шрифт всегда будет доступен для приложения, но увеличит размер APK.
2. **Загрузка шрифтов** &ndash; Android также поддерживает загрузку шрифта из _поставщика шрифтов_. Поставщик шрифтов проверяет, установлен ли на устройстве этот шрифт. При необходимости шрифт будет скачан и кэширован на устройстве. Этот шрифт может совместно использоваться несколькими приложениями.

Аналогичные шрифты (или шрифт, которые могут иметь несколько различных стилей) могут быть сгруппированы в _семейства шрифтов_. Это позволяет разработчикам указывать определенные атрибуты шрифта, например вес, а Android автоматически выбирает соответствующий шрифт из семейства шрифтов.

В библиотеке поддержки Android V26 будет поддерживать поддержку портов для шрифтов на уровень API 26. При нацеливании на более старые уровни API необходимо объявить `app` пространство имен XML и присвоить имена различным атрибутам шрифта, используя `android:` пространство имен и `app:` пространство имен. Если используется только `android:` пространство имен, то шрифты не будут отображаться на устройствах с уровнем API 25 или менее. Например, этот фрагмент XML-кода объявляет новый ресурс [_семейства шрифтов_](#font_families) , который будет работать на уровне API 14 и выше:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

     <font  android:font="@font/sourcesanspro_regular"
            android:fontStyle="normal"
            android:fontWeight="400"
            app:font="@font/sourcesanspro_regular"
            app:fontStyle="normal"
            app:fontWeight="400" />

</font-family>
```

При условии, что шрифты предоставлены для приложения Android надлежащим образом, их можно применить к мини-приложению пользовательского интерфейса, задав [ `fontFamily` атрибут](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily). Например, в следующем фрагменте кода показано, как отобразить шрифт в TextView:

```xml
<TextView
    android:text="The quick brown fox jumped over the lazy dog."
    android:fontFamily="@font/sourcesanspro_regular"
    app:fontFamily="@font/sourcesanspro_regular"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

В этом руководство сначала обсуждается использование шрифтов в качестве ресурса Android, а затем переходите к обсуждению загрузки шрифтов во время выполнения.

## <a name="fonts-as-a-resource"></a>Шрифты как ресурсы

Упаковка шрифта в APK Android гарантирует, что он всегда будет доступен для приложения. Файл шрифта (или. TTF или a. Файл ОТФ) добавляется в приложение Xamarin. Android так же, как и любой другой ресурс, путем копирования файлов в подкаталог в папке **Resources** проекта Xamarin. Android. Ресурсы шрифтов хранятся во вложенном каталоге **Font** папки **Resources** проекта.

> [!NOTE]
> Шрифты должны иметь **действие сборки** **AndroidResource** , или они не будут упакованы в окончательный apk. Действие сборки должно быть автоматически задано интегрированной средой разработки.

Если существует много похожих файлов шрифтов (например, один и тот же шрифт с разными весовыми коэффициентами или стилями), их можно сгруппировать в семейство шрифтов.

<a name="font_families"></a>

### <a name="font-families"></a>Семейства шрифтов

Семейство шрифтов — это набор шрифтов, имеющих различные весовые коэффициенты и стили. Например, можно использовать отдельные файлы шрифтов для полужирного шрифта или курсива. Семейство шрифтов определяется `font` элементами в XML-файле, который хранится в каталоге **Resources/Font** . Каждое семейство шрифтов должно иметь собственный XML-файл.

Чтобы создать семейство шрифтов, сначала добавьте все шрифты в папку **Resources/Font** . Затем создайте новый XML-файл в папке Font для семейства шрифтов. Имя XML-файла не имеет сходства или связи со шрифтами, на которые указывают ссылки; файл ресурсов может быть любым юридическим именем файла ресурсов Android. Этот XML-файл будет иметь корневой `font-family` элемент, содержащий один или несколько `font` элементов. Каждый `font` элемент объявляет атрибуты шрифта.

Следующий код XML является примером семейства шрифтов для шрифта _Sources Sans Pro_ , определяющего множество различных весов шрифтов. Он сохраняется в виде файла в папке **Resources/Font** с именем **саурцесанспро. XML**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto">
    <font android:font="@font/sourcesanspro_regular"
          android:fontStyle="normal"
          android:fontWeight="400"
          app:font="@font/sourcesanspro_regular"
          app:fontStyle="normal"
          app:fontWeight="400" />
    <font android:font="@font/sourcesanspro_bold"
          android:fontStyle="normal"
          android:fontWeight="800"
          app:font="@font/sourcesanspro_bold"
          app:fontStyle="normal"
          app:fontWeight="800" />
    <font android:font="@font/sourcesanspro_italic"
          android:fontStyle="italic"
          android:fontWeight="400"
          app:font="@font/sourcesanspro_italic"
          app:fontStyle="italic"
          app:fontWeight="400" />
</font-family>
```

`fontStyle`Атрибут имеет два возможных значения:

- **Обычная задача** &ndash; обычный шрифт
- **курсив** &ndash; курсив

`fontWeight`Атрибут соответствует `font-weight` атрибуту CSS и ссылается на толщину шрифта. Это значение в диапазоне от 100 до 900. В следующем списке приведены общие значения веса шрифта и их имена:

- **Тонкий** &ndash; 100
- **Очень тонкий** &ndash; 200
- **Освещение** &ndash; 300
- **Обычная задача** &ndash; 400
- **Средний уровень** &ndash; 500
- **Плотный** &ndash; 600
- **Полужирный шрифт** &ndash; 700
- **Жирный полужирный** &ndash; 800
- **Черный цвет** &ndash; 900

После определения семейства шрифтов его можно использовать декларативно, задав `fontFamily` `textStyle` атрибуты, и `fontWeight` в файле макета.  Например, следующий фрагмент XML-кода задает шрифт 600 (обычный) и курсивное начертание:

```xml
<TextView
    android:text="Sans Source Pro semi-bold italic, 600 weight, italic"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:fontFamily="@font/sourcesanspro"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:gravity="center_horizontal"
    android:fontWeight="600"
    android:textStyle="italic"
    />
```

### <a name="programmatically-assigning-fonts"></a>Программное назначение шрифтов

Шрифты могут быть заданы программно с помощью [`Resources.GetFont`](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int)) метода для получения [`Typeface`](https://developer.android.com/reference/android/graphics/Typeface.html) объекта. Многие представления имеют `TypeFace` свойство, которое можно использовать для назначения шрифта мини-приложению. В этом фрагменте кода показано, как программно задать шрифт для TextView:

```csharp
Android.Graphics.Typeface typeface = this.Resources.GetFont(Resource.Font.caveat_regular);
textView1.Typeface = typeface;
textView1.Text = "Changed the font";
```

`GetFont`Метод будет автоматически загружать первый шрифт в семействе шрифтов.  Чтобы загрузить шрифт, соответствующий определенному стилю, используйте `Typeface.Create` метод. Этот метод попытается загрузить шрифт, соответствующий указанному стилю. Например, этот фрагмент кода попытается загрузить полужирный `Typeface` объект из семейства шрифтов, определенного в **Resources/Fonts**:

```csharp
var typeface = Typeface.Create("<FONT FAMILY NAME>", Android.Graphics.TypefaceStyle.Bold);
textView1.Typeface = typeface;
```

## <a name="downloading-fonts"></a>Загрузка шрифтов

Вместо упаковки шрифтов в качестве ресурса приложения Android может загружать шрифты из удаленного источника. Это обеспечит желаемый результат уменьшения размера APK.

Шрифты загружаются с помощью _поставщика шрифтов_. Это специализированный поставщик содержимого, который управляет скачиванием и кэшированием шрифтов для всех приложений на устройстве. Android 8,0 включает поставщик шрифтов для загрузки шрифтов из [репозитория Google Font](https://fonts.google.com). Этот поставщик шрифтов по умолчанию переносится на уровень API 14 с помощью библиотеки поддержки Android V26.

Когда приложение выполняет запрос шрифта, поставщик шрифтов сначала проверяет, уже есть ли на устройстве этот шрифт. В противном случае будет предпринята попытка загрузить шрифт. Если не удается скачать шрифт, Android будет использовать системный шрифт по умолчанию. После скачивания шрифт доступен для всех приложений на устройстве, а не только для приложения, которое выполнило первоначальный запрос.

Когда выполняется запрос на загрузку шрифта, приложение не запрашивает поставщик шрифтов напрямую. Вместо этого приложения будут использовать экземпляр [`FontsContract`](https://developer.android.com/reference/android/provider/FontsContract.html) API (или, [`FontsContractCompat`](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html) Если используется библиотека поддержки 26).  

Android 8,0 поддерживает загрузку шрифтов двумя разными способами:

1. **Объявление загружаемых шрифтов как ресурса** &ndash; Приложение может объявлять загружаемые шрифты в Android с помощью файлов ресурсов XML. Эти файлы будут содержать все метаданные, необходимые Android для асинхронного скачивания шрифтов при запуске приложения и их кэширования на устройстве.
2. **Программным способом** &ndash; API-интерфейсы на уровне API Android 26 позволяют приложению загружать шрифты программно, во время работы приложения. Приложения будут создавать `FontRequest` объект для заданного шрифта и передавать этот объект в `FontsContract` класс. Объект `FontsContract` принимает `FontRequest` и получает шрифт от _поставщика шрифтов_. Android будет синхронно скачивать шрифт. Пример создания `FontRequest` будет показан далее в этом пошаговом окне.

Независимо от используемого подхода, файлы ресурсов, которые необходимо добавить в приложение Xamarin. Android перед скачиванием шрифтов. Сначала необходимо объявить шрифты в XML-файле в каталоге **Resources/Font** как часть семейства шрифтов. В этом фрагменте кода приведен пример загрузки шрифтов из коллекции с [открытым исходным кодом Google](https://fonts.google.com) с помощью поставщика шрифтов по умолчанию, поставляемого с Android 8,0 (или поддержки библиотеки V26):

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto"
             android:fontProviderAuthority="com.google.android.gms.fonts"
             android:fontProviderPackage="com.google.android.gms"
             android:fontProviderQuery="VT323"
             android:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
             app:fontProviderAuthority="com.google.android.gms.fonts"
             app:fontProviderPackage="com.google.android.gms"
             app:fontProviderQuery="VT323"
             app:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
>
</font-family>
```

`font-family`Элемент содержит следующие атрибуты, объявляющие сведения, необходимые Android для загрузки шрифтов:

1. **фонтпровидераусорити** &ndash; Центр поставщика шрифтов, используемый для запроса.
2. **фонтпаккаже** &ndash; Пакет для поставщика шрифтов, который будет использоваться для запроса. Используется для проверки удостоверения поставщика.
3. **фонткуери** &ndash; Это строка, которая поможет поставщику шрифтов определить требуемый шрифт. Сведения о запросе шрифта относятся к поставщику шрифтов. [`QueryBuilder`](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs)Класс в примере приложения для [загрузки шрифтов](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) предоставляет некоторую информацию о формате запросов для шрифтов из коллекции Open Source для Google Fonts.
4. **фонтпровидерцертс** &ndash;  Массив ресурсов со списком наборов хэшей для сертификатов, с которыми должен быть подписан поставщик.

После определения шрифтов может потребоваться предоставить сведения о _сертификатах шрифтов_ , участвующих в скачивании.

### <a name="font-certificates"></a>Сертификаты шрифтов

Если поставщик шрифтов не был предварительно установлен на устройстве или приложение использует `Xamarin.Android.Support.Compat` библиотеку, для Android требуются сертификаты безопасности поставщика шрифтов. Эти сертификаты будут перечислены в файле ресурсов массива, который хранится в каталоге **Resources/Values** .

Например, следующий XML-код называется **Resources/Values/fonts_cert. XML** и хранит сертификаты для поставщика шрифтов Google:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="com_google_android_gms_fonts_certs">
        <item>@array/com_google_android_gms_fonts_certs_dev</item>
        <item>@array/com_google_android_gms_fonts_certs_prod</item>
    </array>
    <string-array name="com_google_android_gms_fonts_certs_dev">
        <item>
            MIIEqDCCA5CgAwIBAgIJANWFuGx90071MA0GCSqGSIb3DQEBBAUAMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTAeFw0wODA0MTUyMzM2NTZaFw0zNTA5MDEyMzM2NTZaMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBANbOLggKv+IxTdGNs8/TGFy0PTP6DHThvbbR24kT9ixcOd9W+EaBPWW+wPPKQmsHxajtWjmQwWfna8mZuSeJS48LIgAZlKkpFeVyxW0qMBujb8X8ETrWy550NaFtI6t9+u7hZeTfHwqNvacKhp1RbE6dBRGWynwMVX8XW8N1+UjFaq6GCJukT4qmpN2afb8sCjUigq0GuMwYXrFVee74bQgLHWGJwPmvmLHC69EH6kWr22ijx4OKXlSIx2xT1AsSHee70w5iDBiK4aph27yH3TxkXy9V89TDdexAcKk/cVHYNnDBapcavl7y0RiQ4biu8ymM8Ga/nmzhRKya6G0cGw8CAQOjgfwwgfkwHQYDVR0OBBYEFI0cxb6VTEM8YYY6FbBMvAPyT+CyMIHJBgNVHSMEgcEwgb6AFI0cxb6VTEM8YYY6FbBMvAPyT+CyoYGapIGXMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbYIJANWFuGx90071MAwGA1UdEwQFMAMBAf8wDQYJKoZIhvcNAQEEBQADggEBABnTDPEF+3iSP0wNfdIjIz1AlnrPzgAIHVvXxunW7SBrDhEglQZBbKJEk5kT0mtKoOD1JMrSu1xuTKEBahWRbqHsXclaXjoBADb0kkjVEJu/Lh5hgYZnOjvlba8Ld7HCKePCVePoTJBdI4fvugnL8TsgK05aIskyY0hKI9L8KfqfGTl1lzOv2KoWD0KWwtAWPoGChZxmQ+nBli+gwYMzM1vAkP+aayLe0a1EQimlOalO762r0GXO0ks+UeXde2Z4e+8S/pf7pITEI/tP+MxJTALw9QUWEv9lKTk+jkbqxbsh8nfBUapfKqYn0eidpwq2AzVp3juYl7//fKnaPhJD9gs=
        </item>
    </string-array>
    <string-array name="com_google_android_gms_fonts_certs_prod">
        <item>
            MIIEQzCCAyugAwIBAgIJAMLgh0ZkSjCNMA0GCSqGSIb3DQEBBAUAMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDAeFw0wODA4MjEyMzEzMzRaFw0zNjAxMDcyMzEzMzRaMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBAKtWLgDYO6IIrgqWbxJOKdoR8qtW0I9Y4sypEwPpt1TTcvZApxsdyxMJZ2JORland2qSGT2y5b+3JKkedxiLDmpHpDsz2WCbdxgxRczfey5YZnTJ4VZbH0xqWVW/8lGmPav5xVwnIiJS6HXk+BVKZF+JcWjAsb/GEuq/eFdpuzSqeYTcfi6idkyugwfYwXFU1+5fZKUaRKYCwkkFQVfcAs1fXA5V+++FGfvjJ/CxURaSxaBvGdGDhfXE28LWuT9ozCl5xw4Yq5OGazvV24mZVSoOO0yZ31j7kYvtwYK6NeADwbSxDdJEqO4k//0zOHKrUiGYXtqw/A0LFFtqoZKFjnkCAQOjgdkwgdYwHQYDVR0OBBYEFMd9jMIhF1Ylmn/Tgt9r45jk14alMIGmBgNVHSMEgZ4wgZuAFMd9jMIhF1Ylmn/Tgt9r45jk14aloXikdjB0MQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEUMBIGA1UEChMLR29vZ2xlIEluYy4xEDAOBgNVBAsTB0FuZHJvaWQxEDAOBgNVBAMTB0FuZHJvaWSCCQDC4IdGZEowjTAMBgNVHRMEBTADAQH/MA0GCSqGSIb3DQEBBAUAA4IBAQBt0lLO74UwLDYKqs6Tm8/yzKkEu116FmH4rkaymUIE0P9KaMftGlMexFlaYjzmB2OxZyl6euNXEsQH8gjwyxCUKRJNexBiGcCEyj6z+a1fuHHvkiaai+KL8W1EyNmgjmyy8AW7P+LLlkR+ho5zEHatRbM/YAnqGcFh5iZBqpknHf1SKMXFh4dd239FJ1jWYfbMDMy3NS5CTMQ2XFI1MvcyUTdZPErjQfTbQe3aDQsQcafEQPD+nqActifKZ0Np0IS9L9kR/wbNvyz6ENwPiTrjV2KRkEjH78ZMcUQXg0L3BYHJ3lc69Vs5Ddf9uUGGMYldX3WfMBEmh/9iFBDAaTCK
        </item>
    </string-array>
</resources>
```

При наличии этих файлов ресурсов приложение может скачивать шрифты.

### <a name="declaring-downloadable-fonts-as-resources"></a>Объявление загружаемых шрифтов как ресурсов

Перечисляя загружаемые шрифты в **AndroidManifest. XML**, Android будет асинхронно скачивать шрифты при первом запуске приложения. Сам шрифт указывается в файле ресурсов массива аналогично следующему:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="downloadable_fonts" translatable="false">
        <item>@font/vt323</item>
    </array>
</resources>
```

Чтобы скачать эти шрифты, их необходимо объявить в **AndroidManifest. XML** , добавив в `meta-data` качестве дочернего `application` элемента. Например, если загружаемые шрифты объявляются в файле ресурсов в **Resources/Values/downloadable_fonts. XML**, этот фрагмент необходимо добавить в манифест:

```xml
<meta-data android:name="downloadable_fonts" android:resource="@array/downloadable_fonts" />
```

### <a name="downloading-a-font-with-the-font-apis"></a>Загрузка шрифта с помощью API шрифтов

Можно программно загрузить шрифт, создав экземпляр [`FontRequest`](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html) объекта и передав его `FontContractCompat.RequestFont` методу. `FontContractCompat.RequestFont`Метод сначала проверяет, существует ли шрифт на устройстве, а затем, если необходимо, асинхронно запрашивает поставщик шрифтов и пытается скачать шрифт для приложения. Если `FontRequest` не удается скачать шрифт, Android будет использовать системный шрифт по умолчанию.

`FontRequest`Объект содержит сведения, которые будут использоваться поставщиком шрифтов для нахождение и скачивания шрифта. Для `FontRequest` требуется четыре фрагмента информации:

1. **Центр** &ndash; поставщика шрифтов Центр поставщика шрифтов, используемый для запроса.
2. **Пакет шрифтов** &ndash; Пакет для поставщика шрифтов, который будет использоваться для запроса. Используется для проверки удостоверения поставщика.
3. **Запрос шрифта** &ndash; Это строка, которая поможет поставщику шрифтов определить требуемый шрифт. Сведения о запросе шрифта относятся к поставщику шрифтов. Подробные сведения о строке относятся к поставщику шрифтов. [`QueryBuilder`](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs)Класс в примере приложения для [загрузки шрифтов](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) предоставляет некоторую информацию о формате запросов для шрифтов из коллекции Open Source для Google Fonts.
4. **Сертификаты** &ndash; поставщика шрифтов  Массив ресурсов со списком наборов хэшей для сертификатов, с которыми должен быть подписан поставщик.

Этот фрагмент кода является примером создания нового `FontRequest` объекта:

```csharp
FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms", <FontToDownload>, Resource.Array.com_google_android_gms_fonts_certs);
```

В предыдущем фрагменте кода `FontToDownload` приведен запрос, который поможет шрифту из коллекции Google Fonts с открытым исходным кодом.

Перед передачей в `FontRequest` `FontContractCompat.RequestFont` метод необходимо создать два объекта:

- **`FontsContractCompat.FontRequestCallback`**&ndash;Это абстрактный класс, который должен быть расширен. Это обратный вызов, который будет вызываться по `RequestFont` завершении. Приложение Xamarin. Android должно подделать подкласс `FontsContractCompat.FontRequestCallback` и переопределять `OnTypefaceRequestFailed` и `OnTypefaceRetrieved` , предоставляя действия, выполняемые при неудачном или неуспешном завершении загрузки.
- **`Handler`**&ndash;Это значение, `Handler` которое будет использоваться `RequestFont` для скачивания шрифта в потоке при необходимости. **Не** следует скачивать шрифты в ПОТОКЕ пользовательского интерфейса.

Этот фрагмент кода является примером класса C#, который будет асинхронно скачивать шрифт из коллекции с открытым исходным кодом Google. Он реализует `FontRequestCallback` интерфейс и создает событие C# по `FontRequest` завершении.

```csharp
public class FontDownloadHelper : FontsContractCompat.FontRequestCallback
{
    // A very simple font query; replace as necessary
    public static readonly String FontToDownload = "Courgette";

    Android.OS.Handler Handler = null;

    public event EventHandler<FontDownloadEventArg> FontDownloaded = delegate
    {
        // just an empty delegate to avoid null reference exceptions.  
    };

    public void DownloadFonts(Context context)
    {
        FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms",FontToDownload , Resource.Array.com_google_android_gms_fonts_certs);
        FontsContractCompat.RequestFont(context, request, this, GetHandlerThreadHandler());
    }

    public override void OnTypefaceRequestFailed(int reason)
    {
        base.OnTypefaceRequestFailed(reason);
        FontDownloaded(this, new FontDownloadEventArg(null));
    }

    public override void OnTypefaceRetrieved(Android.Graphics.Typeface typeface)
    {
        base.OnTypefaceRetrieved(typeface);
        FontDownloaded(this, new FontDownloadEventArg(typeface));
    }

    Handler GetHandlerThreadHandler()
    {
        if (Handler == null)
        {
            HandlerThread handlerThread = new HandlerThread("fonts");
            handlerThread.Start();
            Handler = new Handler(handlerThread.Looper);
        }
        return Handler;
    }
}

public class FontDownloadEventArg : EventArgs
{
    public FontDownloadEventArg(Android.Graphics.Typeface typeface)
    {
        Typeface = typeface;
    }
    public Android.Graphics.Typeface Typeface { get; private set; }
    public bool RequestFailed
    {
        get
        {
            return Typeface != null;
        }
    }
}
```

Чтобы использовать этот вспомогательный метод, `FontDownloadHelper` создается новый объект, и ему назначается обработчик событий:  

```csharp
var fontHelper = new FontDownloadHelper();

fontHelper.FontDownloaded += (object sender, FontDownloadEventArg e) =>
{
    //React to the request
};
fontHelper.DownloadFonts(this); // this is an Android Context instance.
```

## <a name="summary"></a>Сводка

В этом руководством обсуждаются новые интерфейсы API в Android 8,0 для поддержки загружаемых шрифтов и шрифтов в качестве ресурсов. В нем обсуждалось внедрение существующих шрифтов в APK и их использование в макете. В нем также обсуждалось, как Android 8,0 поддерживает загрузку шрифтов из поставщика шрифтов либо программно, либо путем объявления метаданных шрифта в файлах ресурсов.

## <a name="related-links"></a>Связанные ссылки

- [Семейство](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)
- [фонтконфиг](https://developer.android.com/reference/android/text/FontConfig.html)
- [фонтрекуест](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html)
- [фонтсконтракткомпат](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html)
- [Шрифты Resources.](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int))
- [Шрифт](https://developer.android.com/reference/android/graphics/Typeface.html)
- [Библиотека поддержки Android 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/)
- [Использование шрифтов в Android](https://www.youtube.com/watch?v=TfB-TsLFJdM)
- [Спецификация веса шрифта CSS](https://www.w3.org/TR/css-fonts-3/#font-weight-numeric-values)
- [Коллекция Google Fonts с открытым исходным кодом](https://fonts.google.com/)
- [Источник Sans Pro](https://fonts.google.com/specimen/Source+Sans+Pro)
