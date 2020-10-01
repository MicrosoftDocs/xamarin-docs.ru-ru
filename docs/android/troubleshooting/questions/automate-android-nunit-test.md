---
title: Как автоматизировать тестовый проект Android NUnit?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/29/2018
ms.openlocfilehash: c8c9e721bc46d9071bb2af479a5e1d37b93fce27
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91458203"
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Как автоматизировать тестовый проект Android NUnit?

> [!NOTE]
> В этом руководстве объясняется, как автоматизировать тестовый проект NUnit на Android, а не проект Xamarin.UITest. Руководства по Xamarin.UITest можно найти [здесь](/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest).

При создании проекта **Приложение модульного тестирования (Android)** в Visual Studio (или проект **Модульный тест Android** в Visual Studio для Mac), этот проект не будет автоматически выполнять тестовые проекты по умолчанию.
Для выполнения тестовых проектов NUnit на целевом устройстве можно создать подкласс [Android.App.Instrumentation](xref:Android.App.Instrumentation), запущенный с помощью следующей команды: 

```shell
adb shell am instrument 
```

Приведенные ниже шаги показывают, как это сделать.

1. Создайте новый файл с именем **TestInstrumentation.cs**: 

    ```cs 
    using System;
    using System.Reflection;
    using Android.App;
    using Android.Content;
    using Android.Runtime;
    using Xamarin.Android.NUnitLite;

    namespace App.Tests {

        [Instrumentation(Name="app.tests.TestInstrumentation")]
        public class TestInstrumentation : TestSuiteInstrumentation {

            public TestInstrumentation (IntPtr handle, JniHandleOwnership transfer) : base (handle, transfer)
            {
            }

            protected override void AddTests ()
            {
                AddTest (Assembly.GetExecutingAssembly ());
            }
        }
    }
    ```

    В этом файле `Xamarin.Android.NUnitLite.TestSuiteInstrumentation` (из **Xamarin.Android.NUnitLite.dll**) содержит подклассы для создания `TestInstrumentation`.

2. Реализуйте конструктор `TestInstrumentation` и метод `AddTests`. Метод `AddTests` определяет, какие тестовые проекты выполняются.

3. Измените файл `.csproj`, добавив **TestInstrumentation.cs**. Пример:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        ...
        <ItemGroup>
            <Compile Include="TestInstrumentation.cs" />
        </ItemGroup>
        <Target Name="RunTests" DependsOnTargets="_ValidateAndroidPackageProperties">
            <Exec Command="&quot;$(_AndroidPlatformToolsDirectory)adb&quot; $(AdbTarget) $(AdbOptions) shell am instrument -w $(_AndroidPackage)/app.tests.TestInstrumentation" />
        </Target>
        ...
    </Project>
    ```

4. Используйте следующую команду для запуска модульных тестов. Замените `PACKAGE_NAME` именем пакета приложения (имя пакета можно найти в атрибуте приложения `/manifest/@package`, расположенном в папке **AndroidManifest.xml**):

    ```shell
    adb shell am instrument -w PACKAGE_NAME/app.tests.TestInstrumentation
    ```

5. При необходимости файл `.csproj` можно изменить, добавив целевой объект MSBuild `RunTests`. Это дает возможность вызывать модульные тесты с помощью команды, как в приведенном ниже примере.

    ```shell
    msbuild /t:RunTests Project.csproj
    ```

    (Обратите внимание, что использование этого нового целевого объекта не является обязательным; вместо `msbuild`можно использовать предыдущую команду `adb`.)

Дополнительные сведения об использовании команды `adb shell am instrument` для выполнения модульных тестов см. в статье о Android Developer [Запуск тестов с помощью ADB](https://developer.android.com/studio/test/command-line.html#RunTestsDevice).

> [!NOTE]
> При использовании [Xamarin.Android выпуска 5.0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#Android_Callable_Wrapper_Naming) имена пакетов по умолчанию для вызываемых программ-оболочек Android будут основываться на MD5SUM имени экспортируемого типа с указанием сборки. Это позволяет предоставить одно и то же полное имя из двух разных сборок и не получить ошибку упаковки. Поэтому убедитесь, что вы используете свойство `Name` атрибута `Instrumentation`, чтобы создать имя класса для чтения или ACW.

_Имя ACW должно использоваться в команде `adb`, приведенной выше_.
Для переименования или рефакторинга класса C# потребуется изменить команду `RunTests`, чтобы она использовала правильное имя ACW.