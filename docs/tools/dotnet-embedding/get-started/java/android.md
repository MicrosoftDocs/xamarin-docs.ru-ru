---
title: Начало работы с Android
description: В этом документе описывается, как приступить к использованию внедрения .NET с Android. В нем обсуждается установка внедрения .NET, создание проекта библиотеки Android, использование созданных выходных данных в проекте Android Studio и другие соображения.
ms.prod: xamarin
ms.assetid: 870F0C18-A794-4C5D-881B-64CC78759E30
author: davidortinau
ms.author: daortin
ms.date: 03/28/2018
ms.openlocfilehash: 813fb08835f30d0fc24f0c04e1f26b1f4e7f83e0
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91456553"
---
# <a name="getting-started-with-android"></a>Начало работы с Android

В дополнение к требованиям, приведенным в учебнике [Приступая к работе с Java](~/tools/dotnet-embedding/get-started/java/index.md) , вам также потребуется:

- [Xamarin. Android 7,5](https://visualstudio.microsoft.com/xamarin/) или более поздней версии
- [Android Studio 3. x](https://developer.android.com/studio/index.html) с Java 1,8

В качестве обзора мы будем делать следующее:

- Создание проекта библиотеки C# для Android
- Установка внедрения .NET с помощью NuGet
- Запуск внедрения .NET в сборку библиотеки Android
- Используйте созданный файл AAR в проекте Java в Android Studio

## <a name="create-an-android-library-project"></a>Создание проекта библиотеки Android

Откройте Visual Studio для Windows или Mac, создайте новый проект библиотеки классов Android, присвойте ему имя **Hello-from-CSharp**и сохраните его в **~/прожектс/Хелло-Фром-кшарп** или **%усерпрофиле%\прожектс\хелло-Фром-кшарп**.

Добавьте новое действие Android с именем **HelloActivity.CS**, за которым следует макет Android в **ресурсе, макете или Hello. axml**.

Добавьте новый `TextView` в макет и измените текст на что-нибудь приятно.

Ваш источник макета должен выглядеть примерно так:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <TextView
        android:text="Hello from C#!"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center" />
</LinearLayout>
```

В действии убедитесь, что вы вызываете `SetContentView` с новым макетом:

```csharp
[Activity(Label = "HelloActivity"),
    Register("hello_from_csharp.HelloActivity")]
public class HelloActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        SetContentView(Resource.Layout.hello);
    }
}
```

> [!NOTE]
> Не забудьте `[Register]` атрибут. Дополнительные сведения см. в разделе [ограничения](#current-limitations-on-android).

Выполните построение проекта. Результирующая сборка будет сохранена в `bin/Debug/hello-from-csharp.dll` .

## <a name="installing-net-embedding-from-nuget"></a>Установка внедрения .NET из NuGet

Выполните эти [инструкции](~/tools/dotnet-embedding/get-started/install/install.md) , чтобы установить и настроить внедрение .NET для проекта.

Вызов команды, который следует настроить, будет выглядеть следующим образом:

### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

```shell
mono '${SolutionDir}/packages/Embeddinator-4000.0.4.0.0/tools/Embeddinator-4000.exe' '${TargetPath}' --gen=Java --platform=Android --outdir='${SolutionDir}/output' -c
```

#### <a name="visual-studio-2017"></a>Visual Studio 2017

```shell
set E4K_OUTPUT="$(SolutionDir)output"
if exist %E4K_OUTPUT% rmdir /S /Q %E4K_OUTPUT%
"$(SolutionDir)packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe" "$(TargetPath)" --gen=Java --platform=Android --outdir=%E4K_OUTPUT% -c
```

## <a name="use-the-generated-output-in-an-android-studio-project"></a>Использование созданных выходных данных в проекте Android Studio

1. Откройте Android Studio и создайте новый проект с **пустым действием**.
2. Щелкните правой кнопкой мыши модуль **приложения** и выберите пункт **создать > модуль**.
3. Выберите **Импорт. JAR/. Пакет AAR**.
4. С помощью обозревателя каталогов перейдите к папке **~/прожектс/хелло-фром-кшарп/аутпут/hello_from_csharp. AAR** и нажмите кнопку **Готово**.

![Импорт AAR в Android Studio](android-images/androidstudioimport.png)

Файл AAR будет скопирован в новый модуль с именем **hello_from_csharp**.

![Android Studio проект](android-images/androidstudioproject.png)

Чтобы использовать новый модуль из **приложения**, щелкните правой кнопкой мыши и выберите **открыть параметры модуля**. На вкладке **зависимости** добавьте новую **зависимость модуля** и выберите **: hello_from_csharp**.

![Зависимости Android Studio](android-images/androidstudiodependencies.png)

В действии добавьте новый `onResume` метод и используйте следующий код для запуска действия C#:

```java
import hello_from_csharp.*;

public class MainActivity extends AppCompatActivity {
    //... Other stuff here ...
    @Override
    protected void onResume() {
        super.onResume();

        Intent intent = new Intent(this, HelloActivity.class);
        startActivity(intent);
    }
}
```

### <a name="assembly-compression-important"></a>Сжатие сборок (*важно*)

Для внедрения .NET в проекте Android Studio требуется еще одно изменение.

Откройте файл **Build. gradle** приложения и добавьте следующее изменение:

```groovy
android {
    // ...
    aaptOptions {
        noCompress 'dll'
    }
}
```

Xamarin. Android в настоящее время загружает сборки .NET непосредственно из APK, но требует, чтобы сборки не были сжаты.

Если эта программа не установлена, приложение аварийно завершит работу при запуске и выведет на консоль примерно следующее:

```shell
com.xamarin.hellocsharp A/monodroid: No assemblies found in '(null)' or '<unavailable>'. Assuming this is part of Fast Deployment. Exiting...
```

## <a name="run-the-app"></a>Запуск приложения

При запуске приложения:

![Пример "Hello from C#", выполняемый в эмуляторе](android-images/hello-from-csharp-android.png)

Обратите внимание на то, что произошло здесь:

- У нас есть класс C#, `HelloActivity` который подклассировать Java
- У нас есть файлы ресурсов Android
- Мы использовали их из Java в Android Studio

Чтобы этот пример работал, в окончательной APK настраиваются все следующие значения:

- Xamarin. Android настраивается при запуске приложения
- Сборки .NET, включаемые в **активы или сборки**
- **AndroidManifest.xml** изменения для действий C# и т. д.
- Ресурсы и активы Android из библиотек .NET
- [Вызываемые оболочки Android](~/android/platform/java-integration/android-callable-wrappers.md) для любого `Java.Lang.Object` подкласса

Если вы ищете дополнительное пошаговое руководство, ознакомьтесь с приведенным ниже видео, в котором демонстрируется внедрение [демонстрационного финжерпаинта](/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint) Чарльз Петцольд в проект Android Studio.

[![Ембеддинатор-4000 для Android](https://img.youtube.com/vi/ZVcrXUpCNpI/0.jpg)](https://www.youtube.com/watch?v=ZVcrXUpCNpI)

## <a name="using-java-18"></a>Использование Java 1,8

Начиная с этого, лучшим вариантом является использование Android Studio 3,0 ([скачать здесь](https://developer.android.com/studio/index.html)).

Чтобы включить Java 1,8 в файле **Build. gradle** модуля приложения, выполните следующие действия.

```groovy
android {
    // ...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

Дополнительные сведения см. в [Android Studio тестовом проекте](https://github.com/mono/Embeddinator-4000/blob/master/tests/android/app/build.gradle) . 

Если вы хотите использовать Android Studio 2.3. x стабильны, необходимо включить устаревшие цепочки инструментовы разъемов:

```groovy
android {
    // ..
    defaultConfig {
        // ...
        jackOptions.enabled true
    }
}
```

## <a name="current-limitations-on-android"></a>Текущие ограничения в Android

Прямо сейчас, если вы `Java.Lang.Object` подпишете подкласс, Xamarin. Android создаст заглушку Java (вызываемую оболочку Android) вместо внедрения .NET. Поэтому необходимо следовать тем же правилам для экспорта C# в Java в качестве Xamarin. Android. Пример:

```csharp
[Register("mono.embeddinator.android.ViewSubclass")]
public class ViewSubclass : TextView
{
    public ViewSubclass(Context context) : base(context) { }

    [Export("apply")]
    public void Apply(string text)
    {
        Text = text;
    }
}
```

- `[Register]` требуется для соответствия требуемому имени пакета Java
- `[Export]` требуется для того, чтобы метод был виден Java

Мы можем использовать `ViewSubclass` в Java следующим образом:

```java
import mono.embeddinator.android.ViewSubclass;
//...
ViewSubclass v = new ViewSubclass(this);
v.apply("Hello");
```

Узнайте больше об [интеграции Java с Xamarin. Android](~/android/platform/java-integration/index.md).

## <a name="multiple-assemblies"></a>Несколько сборок

Внедрение одной сборки является простой задачей. Однако гораздо более вероятно, что у вас будет более одной сборки C#. Многие случаи зависят от пакетов NuGet, таких как библиотеки поддержки Android или Сервисы Google Play, которые еще более усложняют.

Это вызывает дилеммой, так как внедрение .NET должно включать в окончательный AAR множество типов файлов, например:

- Ресурсы Android
- Ресурсы для Android
- Собственные библиотеки Android
- Источник Android Java

Скорее всего, вы не хотите включать эти файлы из библиотеки поддержки Android или Сервисы Google Play в AAR, но вместо этого использовать официальную версию из Google в Android Studio.

Ниже приведен рекомендуемый подход.

- Передайте .NET внедрение любой сборки, которой вы владеете (имеет источник) и хотите вызвать из Java
- Передайте .NET внедрять любую сборку, в которую потребуются ресурсы Android, собственные библиотеки или ресурсы из
- Добавьте зависимости Java, такие как библиотека поддержки Android или Сервисы Google Play, в Android Studio

Поэтому команда может выглядеть следующим образом:

```shell
mono Embeddinator-4000.exe --gen=Java --platform=Android -c -o output YourMainAssembly.dll YourDependencyA.dll YourDependencyB.dll
```

Вы должны исключить что-либо из NuGet, если оно содержит ресурсы Android, ресурсы и т. д., которые понадобятся вам в проекте Android Studio. Можно также опустить зависимости, которые не нужно вызывать из Java, а компоновщик _должен_ включать необходимые части библиотеки.

Чтобы добавить любые зависимости Java, необходимые в Android Studio, файл **Build. gradle** может выглядеть следующим образом:

```groovy
dependencies {
    // ...
    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.google.android.gms:play-services-games:11.0.4'
    // ...
}
```

## <a name="further-reading"></a>Дополнительные сведения

- [Обратные вызовы на Android](~/tools/dotnet-embedding/android/callbacks.md)
- [Предварительные исследования Android](~/tools/dotnet-embedding/android/index.md)
- [Ограничения внедрения .NET](~/tools/dotnet-embedding/limitations.md)
- [Вклад в проект с открытым исходным кодом](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
- [Коды ошибок и описания](~/tools/dotnet-embedding/errors.md)

## <a name="related-links"></a>Связанные ссылки

- [Пример погоды (Android)](https://github.com/jamesmontemagno/embeddinator-weather)