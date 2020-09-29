---
title: 'Пошаговое руководство: привязка библиотеки Swift для iOS'
description: В этой статье представлено пошаговое руководство по созданию привязки Xamarin. iOS для существующей платформы SWIFT, Gigya. В нем рассматриваются такие темы, как компиляция платформы SWIFT, привязка ее и использование привязки в приложении Xamarin. iOS.
ms.prod: xamarin
ms.assetid: B563ADE9-D0E3-4BC3-A288-66D2296BE706
ms.technology: xamarin-ios
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: 3bc6f0a95bdd01991ccea69d7917f5fabcb9f51b
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91430966"
---
# <a name="walkthrough-bind-an-ios-swift-library"></a>Пошаговое руководство: привязка библиотеки Swift для iOS

> [!IMPORTANT]
> В настоящее время рассматривается возможность использования настраиваемых привязок на платформе Xamarin. Примите участие в [**этом опросе**](https://www.surveymonkey.com/r/KKBHNLT), чтобы помочь определить дальнейшие направления разработки.

Xamarin позволяет разработчикам мобильных приложений создавать кросс-платформенные собственные мобильные возможности с помощью Visual Studio и C#. Вы можете использовать компоненты пакета SDK для платформы iOS из этого комплекта. Но во многих случаях вы также хотите использовать сторонние пакеты SDK, разработанные для этой платформы, которые Xamarin позволяет выполнять через привязки. Чтобы внедрить сторонние платформы цели-C в приложение Xamarin. iOS, необходимо создать для него привязку Xamarin. iOS, прежде чем ее можно будет использовать в приложениях.

Платформа iOS, а также ее собственные языки и средства постоянно развиваются, а Swift является одной из самых динамических областей в мире разработки iOS прямо сейчас. Существует ряд сторонних пакетов SDK, которые уже перенесены из цели-C в SWIFT и представляют нам новые проблемы. Несмотря на то, что процесс привязки SWIFT похож на цель-C, он требует дополнительных шагов и параметров конфигурации для успешной сборки и запуска приложения Xamarin. iOS, допустимого для AppStore.

Цель этого документа — описать общий подход для подготовки такого сценария и предоставить подробные пошаговые инструкции с простым примером.

## <a name="background"></a>Фон

Программа SWIFT впервые появилась в Apple в 2014 и теперь включена в версию 5,1, а внедрение сторонних платформ быстро растет. Существует несколько вариантов привязки к платформе SWIFT, и в этом документе описывается подход, использующий заголовок интерфейса, сформированный с помощью целевого элемента. Этот заголовок автоматически создается средствами Xcode при создании платформы и используется как способ обмена данными из управляемого мира с мировым миром.

## <a name="prerequisites"></a>Предварительные требования

Чтобы выполнить это пошаговое руководство, вам потребуется:

- [Xcode](https://apps.apple.com/us/app/xcode/id497799835)
- [Visual Studio для Mac](https://visualstudio.microsoft.com/downloads)
- [Objective Sharpie](../../../cross-platform/macios/binding/objective-sharpie/get-started.md#installing-objective-sharpie)
- [АППЦЕНТЕР CLI](/appcenter/test-cloud/) (необязательно)

## <a name="build-a-native-library"></a>Сборка собственной библиотеки

Первым шагом является создание собственной платформы SWIFT с включенным заголовком цели-C. Платформа обычно предоставляется сторонним разработчиком и содержит заголовок, встроенный в пакет, в следующем каталоге: ** \<FrameworkName> . Framework/Headers/ \<FrameworkName> -SWIFT. h**.

Этот заголовок предоставляет открытые интерфейсы, которые будут использоваться для создания метаданных привязки Xamarin. iOS и создания классов C#, предоставляющих члены платформы SWIFT. Если заголовок не существует или имеет неполный открытый интерфейс (например, вы не видите классы и члены), у вас есть два варианта:

- Обновите исходный код SWIFT, чтобы создать заголовок и пометить необходимые элементы `@objc` атрибутом
- Создание прокси-платформы, в которой вы управляете открытым интерфейсом и прокси-сервером все вызовы базовой платформы.

В этом руководстве описан второй подход, так как он содержит меньше зависимостей от стороннего исходного кода, который не всегда доступен. Еще одна причина, по которой следует избегать первого подхода, — это дополнительные усилия, необходимые для поддержки будущих изменений платформы. После того как вы начнете добавлять изменения в исходный код стороннего разработчика, вы несете ответственность за поддержку этих изменений и их слияние с каждым будущим обновлением.

Например, в этом руководстве создается привязка для [пакета SDK Gigya для SWIFT](https://developers.gigya.com/display/GD/Swift+SDK) :

1. Откройте Xcode и создайте новую платформу SWIFT, которая будет прокси-сервером между кодом Xamarin. iOS и сторонней платформой SWIFT. Щелкните **файл > создать > проект** и следуйте инструкциям мастера.

    ![Xcode создать проект платформы](walkthrough-images/xcode-create-framework-project.png)

    ![проект платформы имен Xcode](walkthrough-images/xcode-name-framework-project.png)

1. Скачайте [Gigya Framework](https://developers.gigya.com/display/GD/Swift+SDK) с веб-сайта разработчика и распакуйте его. На момент написания статьи последняя версия — [Gigyaный пакет SDK для Swift 1.0.9](https://downloads.gigya.com/predownload?fileName=Swift-Core-framework-1.0.9.zip)

1. Выберите **свифтфрамеворкпрокси** в обозревателе файлов проекта и перейдите на вкладку Общие.

1. Перетащите пакет **Gigya. Framework** в список платформы Xcode Frameworks и библиотеки, расположенный на вкладке Общие. при добавлении платформы проверьте параметр **Копировать элементы при необходимости** .

    ![Xcode Copy Framework](walkthrough-images/xcode-copy-framework.png)

    Убедитесь, что в проект добавлена платформа SWIFT, в противном случае следующие параметры будут недоступны.

1. Убедитесь, что выбран параметр **не внедрять** , который впоследствии будет управляться вручную:

    [![Xcode донотембед, параметр](walkthrough-images/xcode-donotembed-option.png)](walkthrough-images/xcode-donotembed-option.png#lightbox)

1. Убедитесь, что параметр сборки **всегда внедряет стандартные библиотеки Swift**, которые включают библиотеки SWIFT с платформой, имеет значение нет. Впоследствии он будет управляться вручную, который включает SWIFT дилибс в окончательный пакет:

    [![параметр Xcode всегда внедрять значение false](walkthrough-images/xcode-alwaysembedfalse-option.png)](walkthrough-images/xcode-alwaysembedfalse-option.png#lightbox)

1. Убедитесь, что для параметра **включить Bitcode** задано значение **нет**. Как уже сейчас Xamarin. iOS не включает Bitcode, хотя Apple требует, чтобы все библиотеки поддерживали одну и ту же архитектуру:

    [![Xcode Enable bitcode false, параметр](walkthrough-images/xcode-enablebitcodefalse-option.png)](walkthrough-images/xcode-enablebitcodefalse-option.png#lightbox)

    Чтобы убедиться в том, что в результирующей платформе отключен параметр Bitcode, выполните следующую команду терминала для платформы:

    ```bash
    otool -l SwiftFrameworkProxy.framework/SwiftFrameworkProxy | grep __LLVM
    ```

    Результат должен быть пустым, в противном случае необходимо проверить параметры проекта для конкретной конфигурации.

1. Убедитесь, что параметр **имя заголовка интерфейса, сформированный на языке C** , включен и указывает имя заголовка. По умолчанию используется имя ** \<FrameworkName> -SWIFT. h**:

    [![параметр включения заголовка Xcode обжектице-c](walkthrough-images/xcode-objcheaderenabled-option.png)](walkthrough-images/xcode-objcheaderenabled-option.png#lightbox)

1. Предоставьте нужные методы и пометьте их `@objc` атрибутом и примените дополнительные правила, определенные ниже. Если платформа строится без этого шага, созданный заголовок "цель-C" будет пустым, а Xamarin. iOS не сможет получить доступ к членам платформы SWIFT. Предоставьте логику инициализации для базового пакета SDK Gigya SWIFT, создав новый SWIFT-файл **свифтфрамеворкпрокси. SWIFT** и определив следующий код:

    ```swift
    import Foundation
    import UIKit
    import Gigya

    @objc(SwiftFrameworkProxy)
    public class SwiftFrameworkProxy : NSObject {

        @objc
        public func initFor(apiKey: String) -> String {
            Gigya.sharedInstance().initFor(apiKey: apiKey)
            let gigyaDomain = Gigya.sharedInstance().config.apiDomain
            let result = "Gigya initialized with domain: \(gigyaDomain)"
            return result
        }
    }
    ```

    Несколько важных замечаний по коду, приведенному выше:

    - Импортируйте модуль Gigya отсюда из исходного пакета SDK Gigya стороннего разработчика и теперь можете получить доступ к любому члену платформы.
    - Пометьте класс Свифтфрамеворкпрокси `@objc` атрибутом, задающим имя, в противном случае будет создано уникальное нечитаемое имя, например `_TtC19SwiftFrameworkProxy19SwiftFrameworkProxy` . Имя типа должно быть четко определено, так как оно будет использоваться позже по имени.
    - Наследовать класс-посредник от `NSObject` , в противном случае он не будет создан в файле заголовка цели-C.
    - Пометьте все элементы как `public` .

1. Измените конфигурацию сборки схемы с **Отладка** на **выпуск**. Для этого откройте диалоговое окно **Xcode > Target > изменить схему** , а затем установите для параметра **конфигурации сборки** значение **Release**:

    ![Xcode изменить схему](walkthrough-images/xcode-edit-scheme.png)

    ![Xcode изменить схему](walkthrough-images/xcode-edit-scheme-release.png)

1. На этом этапе платформа готова к созданию. Создайте платформу как для симуляторов, так и для архитектуры устройств, а затем объедините выходные данные как единый пакет платформы. Выявление установленных версий пакета SDK для создания исходного кода с помощью средства **xcodebuild** :

    ```bash
    xcodebuild -showsdks
    ```

    Результат будет выглядеть примерно так:

    ```bash
    iOS SDKs:
            iOS 13.2                        -sdk iphoneos13.2
    iOS Simulator SDKs:
            Simulator - iOS 13.2            -sdk iphonesimulator13.2
    macOS SDKs:
            DriverKit 19.0                  -sdk driverkit.macosx19.0
            macOS 10.15                     -sdk macosx10.15
    tvOS SDKs:
            tvOS 13.2                       -sdk appletvos13.2
    tvOS Simulator SDKs:
            Simulator - tvOS 13.2           -sdk appletvsimulator13.2
    watchOS SDKs:
            watchOS 6.1                     -sdk watchos6.1
    watchOS Simulator SDKs:
            Simulator - watchOS 6.1         -sdk watchsimulator6.1
    ```

    Выберите нужный пакет SDK для iOS и версию пакета SDK для симулятора iOS, в данном случае версия 13,2 и выполните сборку с помощью следующей команды.

    ```bash
    xcodebuild -sdk iphonesimulator13.2 -project "Swift/SwiftFrameworkProxy/SwiftFrameworkProxy.xcodeproj" -configuration Release
    xcodebuild -sdk iphoneos13.2 -project "Swift/SwiftFrameworkProxy/SwiftFrameworkProxy.xcodeproj" -configuration Release
    ```

    > [!TIP]
    > Если вы используете рабочую область вместо Project, создайте рабочую область и укажите целевой объект в качестве обязательного параметра. Также необходимо указать выходной каталог, так как для рабочих областей этот каталог будет отличаться от для сборок проекта.

    > [!TIP]
    > Можно также использовать [вспомогательный скрипт](https://github.com/alexeystrakh/xamarin-binding-swift-framework/blob/master/Swift/Scripts/build.fat.sh#L3-L14) для создания платформы для всех применимых архитектур или просто создать ее из симулятора переключения Xcode и устройства в селекторе цели.

1. Существует две платформы SWIFT, по одной для каждой платформы, объединять их как один пакет для последующего внедрения в проект привязки Xamarin. iOS. Чтобы создать платформу FAT, объединяющую обе архитектуры, необходимо выполнить следующие действия. Пакет платформы — это просто папка, в которой можно выполнять все типы операций, например добавлять, удалять и заменять файлы:

    - Перейдите в папку выходных данных сборки с вложенными папками **Release-ифонеос** и **Release-iphonesimulator** и скопируйте одну из платформ в качестве начальной версии окончательных выходных данных (платформа FAT).

        ```bash
        cp -R "Release-iphoneos" "Release-fat"
        ```

    - Объединение модулей из другой сборки с помощью модулей FAT Framework

        ```bash
        cp -R "Release-iphonesimulator/SwiftFrameworkProxy.framework/Modules/SwiftFrameworkProxy.swiftmodule/" "Release-fat/SwiftFrameworkProxy.framework/Modules/SwiftFrameworkProxy.swiftmodule/"
        ```

    - Объединение конфигурации ифонеос + iphonesimulator в качестве платформы FAT

        ```bash
        lipo -create -output "Release-fat/SwiftFrameworkProxy.framework/SwiftFrameworkProxy" "Release-iphoneos/SwiftFrameworkProxy.framework/SwiftFrameworkProxy" "Release-iphonesimulator/SwiftFrameworkProxy.framework/SwiftFrameworkProxy"
        ```

    - проверите результаты;

        ```bash
        lipo -info "Release-fat/SwiftFrameworkProxy.framework/SwiftFrameworkProxy"
        ```

        Выходные данные должны визуализироваться следующим образом, отражающее имя платформы и реализованные архитектуры:

        ```bash
        Architectures in the fat file: Release-fat/SwiftFrameworkProxy.framework/SwiftFrameworkProxy are: x86_64 arm64
        ```

    > [!TIP]
    > Если требуется поддержка только одной платформы (например, если вы создаете приложение, которое может выполняться только на устройстве), можно пропустить этот шаг, чтобы создать библиотеку FAT и использовать выходную среду из сборки устройства, созданной ранее.

    > [!TIP]
    > Также можно использовать [вспомогательный скрипт](https://github.com/alexeystrakh/xamarin-binding-swift-framework/blob/master/Swift/Scripts/build.fat.sh#L16-L24) для создания платформы FAT, которая автоматизирует все описанные выше действия.

## <a name="prepare-metadata"></a>Подготовка метаданных

В настоящее время у вас должна быть платформа с автоматически созданным заголовком интерфейса "цель-C" для использования в привязке Xamarin. iOS.  Следующим шагом является подготовка интерфейсов определения API, которые используются в проекте привязки для создания классов C#. Эти определения можно создать вручную или автоматически с помощью инструмента [задания Шарпие](../../../cross-platform/macios/binding/objective-sharpie/index.md) и созданного файла заголовка. Используйте Шарпие для создания метаданных:

1. Скачайте последнюю версию средства [Шарпие](../../../cross-platform/macios/binding/objective-sharpie/index.md) из официального веб-сайта скачивания и установите его, следуя мастеру. После завершения установки можно проверить ее, выполнив команду Шарпие:

    ```bash
    sharpie -v
    ```

1. Создайте метаданные с помощью Шарпие и файла заголовков с автоматически созданной задачей-C:

    ```bash
    sharpie bind --sdk=iphoneos13.2 --output="XamarinApiDef" --namespace="Binding" --scope="Release-fat/SwiftFrameworkProxy.framework/Headers/" "Release-fat/SwiftFrameworkProxy.framework/Headers/SwiftFrameworkProxy-Swift.h"
    ```

    Выходные данные отражают создаваемые файлы метаданных: **ApiDefinitions.CS** и **StructsAndEnums.CS**. Сохраните эти файлы для следующего шага, чтобы включить их в проект привязки Xamarin. iOS вместе со встроенными ссылками:

    ```bash
    Parsing 1 header files...
    Binding...
        [write] ApiDefinitions.cs
        [write] StructsAndEnums.cs
    ```

    Средство создаст метаданные C# для каждого предоставленного члена цели-C, который будет выглядеть примерно так, как показано в следующем коде. Как видите, его можно определить вручную, так как он имеет понятный для человека формат и понятные элементы.

    ```csharp
    [Export ("initForApiKey:")]
    string InitForApiKey (string apiKey);
    ```

    > [!TIP]
    > Имя файла заголовка может отличаться, если вы изменили параметры Xcode по умолчанию для имени заголовка. По умолчанию он имеет имя проекта с суффиксом **-SWIFT** . Вы всегда можете проверить файл и его имя, перейдя в папку заголовков пакета платформы.

    > [!TIP]
    > В рамках процесса автоматизации можно использовать [вспомогательный скрипт](https://github.com/alexeystrakh/xamarin-binding-swift-framework/blob/master/Swift/Scripts/build.fat.sh#L35) для автоматического создания метаданных после создания платформы FAT.

## <a name="build-a-binding-library"></a>Сборка библиотеки привязки

Следующим шагом является создание проекта привязки Xamarin. iOS с помощью шаблона привязки Visual Studio, добавление необходимых метаданных, встроенных ссылок и последующая сборка проекта для создания используемой библиотеки:

1. Откройте Visual Studio для Mac и создайте новый проект библиотеки привязки Xamarin. iOS, присвойте ему имя, в данном случае Свифтфрамеворкпрокси. Binding, и завершите работу мастера. Шаблон привязки Xamarin. iOS расположен по следующему пути: Библиотека **> библиотеки iOS > Библиотека привязки**:

    ![Создание библиотеки привязки Visual Studio](walkthrough-images/visualstudio-create-binding-library.png)

1. Удалите существующие файлы метаданных **ApiDefinition.CS** и **structs.CS** , так как они будут полностью заменены метаданными, созданными средством задания Шарпие.
1. Скопируйте метаданные, созданные Шарпие, на одном из предыдущих шагов, выберите следующее действие сборки в окне Свойства: **обжбиндингапидефинитион** для файла **ApiDefinitions.CS** и **обжбиндингкоресаурце** для файла **StructsAndEnums.CS** :

    ![метаданные структуры проекта Visual Studio](walkthrough-images/visualstudio-project-structure-metadata.png)

    Метаданные сами описывают каждый предоставленный класс цели-C и член с помощью языка C#. Вы можете увидеть исходное определение заголовка цели-C вместе с объявлением C#:

    ```csharp
    // @interface SwiftFrameworkProxy : NSObject
    [BaseType (typeof(NSObject))]
    interface SwiftFrameworkProxy
    {
        // -(NSString * _Nonnull)initForApiKey:(NSString * _Nonnull)apiKey __attribute__((objc_method_family("none"))) __attribute__((warn_unused_result));
        [Export ("initForApiKey:")]
        string InitForApiKey (string apiKey);
    }
    ```

    Несмотря на то, что это допустимый код C#, он не используется, как есть, вместо этого используется средствами Xamarin. iOS для создания классов C# на основе этого определения метаданных. В результате вместо интерфейса Свифтфрамеворкпрокси вы получаете класс C# с тем же именем, который можно создать с помощью кода Xamarin. iOS. Этот класс получает методы, свойства и другие члены, определенные метаданными, которые будут вызываться способом C#.

1. Добавьте собственную ссылку на созданную ранее платформу FAT, а также зависимость от этой платформы. В этом случае добавьте в проект привязки собственные ссылки Свифтфрамеворкпрокси и Gigya Framework:

    - Чтобы добавить ссылки на собственные платформы, откройте модуль поиска и перейдите в папку с платформами. Перетащите платформы в расположение машинных ссылок в обозреватель решений. Кроме того, можно использовать пункт контекстного меню в папке машинные ссылки и выбрать пункт **Добавить встроенную ссылку** , чтобы найти платформы и добавить их:

    ![собственные ссылки в структуре проекта Visual Studio](walkthrough-images/visualstudio-project-structure-nativerefs.png)

    - Обновите свойства всех машинных ссылок и проверьте три важных параметра:

        - Задать интеллектуальную связь = true
        - Задать принудительную загрузку = false
        - Задайте список платформ, используемых для создания исходных платформ. В этом случае каждая платформа имеет только две зависимости: Foundation и UIKit. Задайте для него поле Frameworks (платформы):

        ![параметры прокси-сервера Visual Studio нативереф](walkthrough-images/visualstudio-nativeref-proxy-options.png)

        При наличии дополнительных флагов компоновщика задайте их в поле флаги компоновщика. В этом случае не заключайте его в пустое место.

    - При необходимости укажите дополнительные флаги компоновщика. Если библиотека, которую вы привязываете, предоставляет только интерфейсы API цели-C, но внутреннее использует SWIFT, то могут возникнуть такие проблемы:

        ```Console
        error MT5209 : Native linking error : warning: Auto-Linking library not found for -lswiftCore
        error MT5209 : Native linking error : warning: Auto-Linking library not found for -lswiftQuartzCore
        error MT5209 : Native linking error : warning: Auto-Linking library not found for -lswiftCoreImage
        ```

        В свойствах проекта привязки для собственной библиотеки в флаги компоновщика должны быть добавлены следующие значения:

        ```Linker
        L/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/swift/iphonesimulator/ -L/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/swift/iphoneos -Wl,-rpath -Wl,@executable_path/Frameworks
        ```

        Первые два параметра (  `-L ...`   ) сообщают компилятору Native, где найти библиотеки Swift. Компилятор в машинном код будет игнорировать библиотеки, у которых нет правильной архитектуры. Это означает, что можно одновременно передать расположение для библиотек симуляторов и библиотек устройств, чтобы оно работала как для симуляторов, так и для сборок устройств (эти пути верны только для iOS; для tvOS и watchOS их необходимо обновить). Недостатком является то, что этот подход требует, чтобы верный Xcode в/Аппликатион/кскоде.АПП, если потребитель библиотеки привязки имеет Xcode в другом расположении, он не будет работать. Альтернативным решением является добавление этих параметров в дополнительные аргументы mtouch в параметрах сборки iOS исполняемого проекта ( `--gcc_flags -L... -L...` ). Третий вариант заставляет собственный компоновщик сохранить расположение библиотек SWIFT в исполняемом файле, чтобы операционная система могла их найти.

1. Последним действием является создание библиотеки и проверка отсутствия ошибок компиляции. Часто вы обнаружите, что метаданные привязок, созданные целевым Шарпие, будут помечены  `[Verify]`   атрибутом. Эти атрибуты указывают на то, что необходимо убедиться, что цель Шарпиеа правильно, сравнив привязку с исходным объявлением цели-C (которое будет предоставлено в примечании над привязанным объявлением). Дополнительные сведения о членах, помеченных атрибутом, можно получить по [следующей ссылке](../../../cross-platform/macios/binding/objective-sharpie/platform/verify.md). После сборки проекта его можно использовать в приложении Xamarin. iOS.

## <a name="consume-the-binding-library"></a>Использование библиотеки привязки

Последним шагом является использование библиотеки привязки Xamarin. iOS в приложении Xamarin. iOS. Создайте новый проект Xamarin. iOS, добавьте ссылку на библиотеку привязки и активируйте пакет SDK Gigya для Swift:

1. Создайте проект Xamarin. iOS. В качестве отправной точки можно использовать **приложение > iOS > приложении с одним представлением** :

    ![новое приложение Visual Studio](walkthrough-images/visualstudio-app-new.png)

1. Добавьте ссылку на проект привязки к целевому проекту или DLL, созданному ранее. Рассматривать библиотеку привязки как обычную библиотеку Xamarin. iOS:

    ![ссылки на приложения Visual Studio](walkthrough-images/visualstudio-app-refs.png)

1. Обновите исходный код приложения и добавьте логику инициализации в первичную ViewController, которая активирует пакет SDK Gigya.

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
        var proxy = new SwiftFrameworkProxy();
        var result = proxy.InitForApiKey("APIKey");
        System.Diagnostics.Debug.WriteLine(result);
    }
    ```

1. Создайте кнопку с именем **бтнлогин** и добавьте следующий обработчик нажатия кнопки, чтобы активировать поток проверки подлинности:

    ```csharp
    private void btnLogin_Tap(object sender, EventArgs e)
    {
        _proxy.LoginWithProvider(GigyaSocialProvidersProxy.Instagram, this, (result, error) =>
        {
            // process your login result here
        });
    }
    ```

1. Запустите приложение. в выходных данных отладки отобразится следующая строка: `Gigya initialized with domain: us1.gigya.com` . Нажмите кнопку, чтобы активировать поток проверки подлинности:

    [![результат прокси-сервера SWIFT](walkthrough-images/swiftproxy-result.png)](walkthrough-images/swiftproxy-result.png#lightbox)

Поздравляем! Вы успешно создали приложение Xamarin. iOS и библиотеку привязок, использующую платформу SWIFT. Приложение, описанное выше, будет успешно запущено в iOS 12.2 +, так как в версии iOS [Apple появилась стабильность ABI](https://swift.org/blog/swift-5-1-released/) , и каждый iOS, начиная с 12.2 +, включает библиотеки среды выполнения SWIFT, которые могут использоваться для запуска приложения, скомпилированного с помощью SWIFT 5.1 +. Если вам нужно добавить поддержку более ранних версий iOS, необходимо выполнить еще несколько действий:

1. Чтобы добавить поддержку iOS 12,1 и более ранних версий, необходимо отправить специальный SWIFT дилибс, используемый для компиляции платформы. Используйте пакет NuGet [Xamarin. iOS. свифтрунтимесуппорт](https://www.nuget.org/packages/Xamarin.iOS.SwiftRuntimeSupport/) для обработки и копирования необходимых библиотек с помощью IPA. Добавьте ссылку NuGet в целевой проект и перестройте приложение. Дальнейшие действия не требуются, пакет NuGet устанавливает определенные задачи, которые выполняются вместе с процессом сборки, определяют необходимые SWIFT дилибс и упаковывают их в окончательный IPA.

1. Чтобы отправить приложение в магазин приложений, в котором вы хотите использовать Xcode и распространение, что приведет к обновлению файла IPA и папки **свифтсуппорт** дилибс, поэтому он будет принят AppStore:

    ○ Заархивируйте приложение. В меню Visual Studio для Mac выберите **сборка > Архив для публикации**:

    ![Архив Visual Studio для публикации](walkthrough-images/visualstudio-archiveforpublishing.png)

    Это действие выполняет сборку проекта и его достижение организатору, доступ к которому Xcode для распространения.

    ○ Распространение через Xcode. Откройте Xcode и перейдите в **окно >** меню "Организатор".

    ![Архивы Visual Studio](walkthrough-images/visualstudio-archives.png)

    Выберите архив, созданный на предыдущем шаге, и нажмите кнопку распространить приложение. Следуйте указаниям мастера, чтобы отправить приложение в AppStore.

1. Этот шаг является необязательным, но важно убедиться, что приложение может работать в iOS 12,1 и более ранних версиях, а также 12,2. Это можно сделать с помощью инфраструктуры Test Cloud и UITest. Создайте проект UITest и базовый пользовательский интерфейс, который запускает приложение:

    - Создайте проект UITest и настройте его для приложения Xamarin. iOS:

        ![новые Visual Studio UITest](walkthrough-images/visualstudio-uitest-new.png)

        > [!TIP]
        > Дополнительные сведения о том, как создать проект UITest и настроить его для своего приложения, можно найти по [следующей ссылке](/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest).

    - Создайте базовый тест для запуска приложения и используйте некоторые функции пакета SDK SWIFT. Этот тест активирует приложение, пытается войти в систему, а затем нажимает кнопку "Отмена":

        ```csharp
        [Test]
        public void HappyPath()
        {
            app.WaitForElement(StatusLabel);
            app.WaitForElement(LoginButton);
            app.Screenshot("App loaded.");
            Assert.AreEqual(app.Query(StatusLabel).FirstOrDefault().Text, "Gigya initialized with domain: us1.gigya.com");

            app.Tap(LoginButton);
            app.WaitForElement(GigyaWebView);
            app.Screenshot("Login activated.");

            app.Tap(CancelButton);
            app.WaitForElement(LoginButton);
            app.Screenshot("Login cancelled.");
        }
        ```

        > [!TIP]
        > Дополнительные сведения о тестов пользовательского интерфейса Framework и автоматизации пользовательского интерфейса см. по [следующей ссылке](/appcenter/test-cloud/uitest/).

    - Создайте приложение iOS в центре приложений, создайте новый тестовый запуск с новым набором устройств для запуска теста:

        ![Новый центр приложений Visual Studio](walkthrough-images/visualstudio-appcenter-new.png)

        > [!TIP]
        > Дополнительные сведения о Аппцентер Test Cloud см. по [следующей ссылке](/appcenter/test-cloud/).

    - Установка интерфейса командной строки аппцентер

        ```bash
        npm install -g appcenter-cli
        ```

        > [!IMPORTANT]
        > Убедитесь, что установлена версия node версии 6.3 или более поздняя.

    - Выполните тест с помощью следующей команды. Убедитесь также в том, что Командная строка аппцентер в данный момент находится в системе.

        ```bash
        appcenter test run uitest --app "Mobile-Customer-Advisory-Team/SwiftBinding.iOS" --devices a7e7cb50 --app-path "Xamarin.SingleView.ipa" --test-series "master" --locale "en_US" --build-dir "Xamarin/Xamarin.SingleView.UITests/bin/Debug/"
        ```

    - Проверьте результат. На портале Аппцентер перейдите к **приложению > тест > тестовые запуски**:

        ![результат Visual Studio аппцентер UITest](walkthrough-images/visualstudio-appcenter-uitest-result.png)

        И выберите нужный тестовый запуск и проверьте результат:

        ![запуски аппцентер uitest в Visual Studio](walkthrough-images/visualstudio-appcenter-uitest-runs.png)

Вы разработали базовое приложение Xamarin. iOS, которое использует собственную платформу Swift через библиотеку привязки Xamarin. iOS. Пример предоставляет упрощенный способ использования выбранной платформы и в реальных приложениях. вам потребуется предоставить больше интерфейсов API и подготовить метаданные для этих API. Скрипт для создания метаданных упростит будущие изменения в API-интерфейсах платформы.

## <a name="related-links"></a>Связанные ссылки

- [Xcode](https://apps.apple.com/us/app/xcode/id497799835)
- [Visual Studio для Mac](https://visualstudio.microsoft.com/downloads)
- [Objective Sharpie](../../../cross-platform/macios/binding/objective-sharpie/index.md)
- [Проверка метаданных Шарпие](../../../cross-platform/macios/binding/objective-sharpie/platform/verify.md)
- [Цель привязки — платформа C](../binding-objective-c/walkthrough.md)
- [Пакет SDK для Gigya iOS (платформа SWIFT)](https://developers.gigya.com/display/GD/Swift+SDK)
- [Нестабильность SWIFT 5,1 ABI](https://swift.org/blog/swift-5-1-released/)
- [Свифтрунтимесуппорт NuGet](https://www.nuget.org/packages/Xamarin.iOS.SwiftRuntimeSupport/)
- [Автоматизация UITest в Xamarin](/appcenter/test-cloud/uitest/)
- [Конфигурация Xamarin. iOS UITest](/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)
- [Аппцентер Test Cloud](/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)
- [Репозиторий примеров проектов](https://github.com/alexeystrakh/xamarin-binding-swift-framework)