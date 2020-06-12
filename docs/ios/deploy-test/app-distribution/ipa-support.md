---
title: Поддержка IPA в Xamarin.iOS
description: Эта статья описывает создание файла IPA, который можно использовать для развертывания приложения с использованием прямого распространения, для тестирования или собственной разработки внутренних приложений.
ms.prod: xamarin
ms.assetid: D253C2DB-852E-6FC6-C9FD-574730B8DB19
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: b9254afdcb6286edcffc67a1a69af8b049f08b6b
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573226"
---
# <a name="ipa-support-in-xamarinios"></a>Поддержка IPA в Xamarin.iOS

_В этой статье описано, как создать файл IPA, который можно использовать для развертывания приложения при прямом распространении, для тестирования или собственной разработки внутренних приложений._

Кроме выпуска приложения для продажи через iTunes App Store, его можно развернуть для следующих целей:

- **Прямое тестирование** — приложение iOS может можно развернуть для пользователей в количестве не более 100 (определяется по UUID устройств iOS) для альфа- и бета-тестирования. См. нашу документацию по [подготовке устройства iOS для разработки](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning), где приведены подробные сведения о добавлении тестовых устройств iOS в вашу учетную запись разработчика Apple, а также наше руководство по [прямому распространению](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md).
- **Внутреннее или корпоративное развертывание** — приложение iOS можно развернуть внутри организации, для чего нужно быть участником программы для разработчиков Apple Developer Enterprise Program. Дополнительные сведения о внутреннем распространении см. в [данном руководстве](~/ios/deploy-test/app-distribution/in-house-distribution.md).

В любом случае нужно создать пакет IPA (специальный тип ZIP-файла) и подписать цифровым образом с помощью подходящего профиля подготовки к распространению. Эта статья описывает действия, необходимые для сборки пакета IPA и его установки на устройстве iOS с помощью iTunes, на Mac или компьютере с Windows.

<a name="iTunesMetadata"></a>

## <a name="the-itunesmetadataplist-file"></a>Файл iTunesMetadata.plist

При создании приложения в iTunes Connect (для продажи или для бесплатного распространения через iTunes App Store) разработчик может указать сведения, такие как жанр приложения, поджанр, уведомление об авторских правах, поддерживаемые устройства iOS и требуемые параметры устройства.

Приложения iOS, доставляемые посредством прямого или внутреннего распространения, должны каким-либо образом поддерживать эти сведения, чтобы их можно было отобразить в iTunes и на устройстве пользователя. По умолчанию при каждой сборке проекта создается небольшой файл iTunesMetadata.plist, который сохраняется в каталоге проекта.

Настраиваемый **iTunesMetadata.plist** также можно создать, чтобы предоставить дополнительные сведения для распространения. Для получения дополнительных сведений о содержимом этого файла и способе его создания см. нашу документацию по [содержимому файла iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md#iTunesMetadata_contents) и [созданию файла iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md#iTunesMetadata_creating).

<a name="iTunesArtwork"></a>

## <a name="itunes-artwork"></a>Иллюстрации iTunes

При доставке приложения средствами, отличными от App Store, нужно также включить изображение 512 x 512 и 1024 x 1024, которое будет использоваться для представления приложения в iTunes.

Чтобы указать иллюстрации iTunes, сделайте следующее:

1. В **обозревателе решений** дважды щелкните файл **Info.plist**, чтобы открыть его для редактирования.
2. Прокрутите до пункта **Иллюстрации iTunes** в редакторе.
3. При отсутствии изображения щелкните эскиз в редакторе, выберите файл изображения с требуемой иллюстрацией iTunes в диалоговом окне **Открыть файл**, а затем нажмите кнопку **ОК** или кнопку **Открыть**.
4. Повторите этот шаг, пока не укажете все нужные образы для приложения.

Дополнительные сведения см. в документации по [иллюстрациям iTunes](~/ios/app-fundamentals/images-icons/app-icons.md).

<a name="createipa"></a>

## <a name="creating-an-ipa"></a>Создание IPA

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Создание IPA теперь встроено в новый рабочий процесс публикации. Для этого следуйте инструкциям ниже, чтобы заархивировать приложение, подписать его и сохранить ваш IPA.

Прежде чем создавать IPA для кроссплатформенного решения, выберите проект iOS в качестве запускаемого:

![](ipa-support-images/setasstartup.png "Selected the iOS project as the startup project")

### <a name="build-your-archive"></a>Построение архива

Чтобы выполнить сборку IPA, нужно создать _архив_ для сборки выпуска приложения. Этот архив содержит наше приложение и идентифицирующие сведения о нем.

1. Выберите конфигурацию **Выпуск | Устройство** в Visual Studio для Mac: !

    ![](ipa-support-images/buildxs01new.png "Select the Release | Device configuration")

1. В меню **Сборка** выберите **Архивировать для публикации**:

    ![](ipa-support-images/buildxs02new.png "Select Archive for Publishing")

1. После создания архива появится представление **Архивы**:

    ![](ipa-support-images/buildxs03new.png "The Archives view will be displayed")

### <a name="sign-and-distribute-your-app"></a>Подписывание и распространение приложения

Каждый раз при сборке приложения для архивирования автоматически открывается представление **Архивы**, где отображаются все заархивированные проекты, сгруппированные по решениям. По умолчанию в этом представлении отображается только открытое сейчас решение. Чтобы просмотреть все решения с архивами, выберите параметр **Показать все архивы**.

Рекомендуется хранить архивы, которые были развернуты для клиентов (прямые или внутренние развертывания), чтобы при необходимости впоследствии отображать любую получаемую отладочную информацию.

Обратите внимание, что для сборок, не относящихся к App Store, файл **iTunesMetadata.plist** и набор иллюстраций iTunes будут автоматически включены в IPA при обнаружении их в архиве.

Чтобы подписать приложение и подготовить его к распространению, выполните следующие действия:

1. Нажмите кнопку **Подписать и распространить...** , как показано ниже:

    ![](ipa-support-images/buildxs04new.png "Select Sign and Distribute...")

1. Откроется мастер публикации. Выберите канал распространения **Специальный** или **Корпоративный** (Внутренний), чтобы создать пакет:

    ![](ipa-support-images/distribute01.png "Select the Ad-Hoc or Enterprise In-House distribution")

1. На экране профиля подготовки выберите удостоверение для подписывания и соответствующий профиль подготовки или подпишите повторно другим удостоверением:

    ![](ipa-support-images/distribute02.png "Select the signing identity and corresponding provisioning profile")

1. Проверьте сведения о пакете и затем нажмите кнопку **Опубликовать**:

    ![](ipa-support-images/distribute03.png "Verify the package details")

1. Наконец, сохраните свой IPA на компьютере:

    ![](ipa-support-images/distribute04.png "Save the IPA to the computer")

### <a name="building-via-the-command-line-on-mac"></a>Сборка из командной строки (на Mac)

В некоторых случаях, например в среде непрерывной интеграции, может потребоваться создать IPA из командной строки. Для этого выполните следующие действия:

1. Убедитесь, что выбраны параметры **Параметры проекта > Параметры IPA iOS > Включить изображения iTunesArtwork** и **Выполнить сборку произвольного или корпоративного пакета (IPA)** :

    ![](ipa-support-images/imagexs04.png "Include iTunesArtwork images and Build ad-hoc/enterprise package IPA is checked")

    При необходимости вместо этого можно изменить файл **CSPROJ** в текстовом редакторе и вручную добавить два соответствующих свойства в `PropertyGroup` для конфигурации, которая будет использоваться для сборки приложения:

    ```xml
    <BuildIpa>true</BuildIpa>
    <IpaIncludeArtwork>false</IpaIncludeArtwork>
    ```

1. Если вы включаете необязательный файл **iTunesMetadata.plist**, нажмите кнопку **...** , выберите его в списке и нажмите кнопку **ОК**:

     ![](ipa-support-images/imagexs03.png "Select iTunesMetadata.plist from the list")

1. Вызовите **msbuild** напрямую и передайте это свойство в командной строке.

    ```bash
    /Library/Frameworks/Mono.framework/Commands/msbuild YourSolution.sln /p:Configuration=Ad-Hoc /p:Platform=iPhone /p:BuildIpa=true
    ```

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Когда профиль подготовки создан и выбран, создан необязательный файл **iTunesMetadata.plist**, а также заданы иллюстрации iTunes в Visual Studio, вы можете выполнить сборку IPA для распространения. После этого нужно настроить проект. Выполните следующие действия:

1. В **обозревателе решений** щелкните имя проекта Xamarin.iOS правой кнопкой мыши и выберите **Свойства**, чтобы открыть его для редактирования:

    ![](ipa-support-images/imagevs01.png "Select Properties")

2. Выберите **Параметры IPA iOS** и затем пункт **Специальный** в раскрывающемся списке **Конфигурация**:

    ![](ipa-support-images/imagevs02.png "Select Ad-Hoc from the Configuration dropdown list")

    > [!NOTE]
    > Конфигурация "Специальный" может быть недоступна для более новых проектов Xamarin.iOS. Если она недоступна, выберите конфигурацию **Выпуск**.

3. Если вы включаете необязательный файл **iTunesMetadata.plist**, нажмите кнопку **...** , выберите его в списке и нажмите кнопку **Открыть**:

    ![](ipa-support-images/imagevs03.png "Select iTunesMetadata.plist from the list")

4. При необходимости заполните поле **Имя пакета** для IPA-файла, иначе оно будет совпадать с именем проекта Xamarin.iOS.
5. Сохраните изменения в свойствах проекта.
6. Выберите пункт **Специальный** в раскрывающемся списке **Конфигурация сборки**, если он доступен. В противном случае выберите **Выпуск**:

    ![](ipa-support-images/imagevs05.png "Select Ad Hoc from the Build Configuration dropdown")

7. Выполните сборку проекта для создания пакета IPA.
8. Пакет IPA будет создан в папке **Bin > iOS Device > Ad Hoc (или Release)** :

    ![](ipa-support-images/imagevs06.png "The IPA in the file explorer")

-----

<a name="Customizing-the-IPA-Location"></a>

## <a name="customizing-the-ipa-location"></a>Настройка расположения файла IPA

Новое свойство **MSBuild**`IpaPackageDir` позволяет упростить настройку расположения для вывода файла **IPA**. Если в параметре `IpaPackageDir` задано настраиваемое расположение, файл **IPA** будет помещен в указанную папку, а не в установленный по умолчанию подкаталог с меткой времени. Это может быть полезно при создании автоматизированных сборок, для работы которых требуется конкретный путь к каталогу (например, это могут быть сборки непрерывной интеграции).

Использовать новое свойство можно несколькими способами:

Например, чтобы выводить файл **IPA** в старый каталог по умолчанию (как в Xamarin.iOS 9.6 и более ранних версиях), задайте для свойства `IpaPackageDir` значение `$(OutputPath)` одним из следующих способов. Оба подхода совместимы со всеми сборками Unified API Xamarin.iOS, включая сборки интегрированной среды разработки и сборки командной строки, которые используют **msbuild**, **xbuild** или **mdtool**.

- Первый способ подразумевает установку свойства `IpaPackageDir` в элементе `<PropertyGroup>` в файле **MSBuild**. Например, можно добавить следующий элемент `<PropertyGroup>` в конец файла **CSPROJ** проекта приложения iOS (непосредственно перед закрывающим тегом `</Project>`):

    ```xml
    <PropertyGroup>
        <IpaPackageDir>$(OutputPath)</IpaPackageDir>
    </PropertyGroup>
    ```

- Более эффективный подход заключается в добавлении элемента `<IpaPackageDir>` в конец существующего элемента `<PropertyGroup>`, который соответствует конфигурации, используемой при сборке файла **IPA**. Это способ является рекомендуемым, поскольку он позволяет обеспечить дальнейшую совместимость с запланированными настройками на странице параметров IPA-файла для iOS в свойствах проекта. Если вы в данный момент используете конфигурацию `Release|iPhone` для сборки файла **IPA**, полностью обновленная группа свойств может иметь следующий вид:

    ```xml
    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone' ">
        <Optimize>true</Optimize>
        <OutputPath>bin\iPhone\Release</OutputPath>
        <ErrorReport>prompt</ErrorReport>
        <WarningLevel>4</WarningLevel>
        <ConsolePause>false</ConsolePause>
        <CodesignKey>iPhone Developer</CodesignKey>
        <MtouchUseSGen>true</MtouchUseSGen>
        <MtouchUseRefCounting>true</MtouchUseRefCounting>
        <MtouchFloat32>true</MtouchFloat32>
        <CodesignEntitlements>Entitlements.plist</CodesignEntitlements>
        <MtouchLink>SdkOnly</MtouchLink>
        <MtouchArch>;ARMv7, ARM64</MtouchArch>
        <MtouchHttpClientHandler>HttpClientHandler</MtouchHttpClientHandler>
        <MtouchTlsProvider>Default</MtouchTlsProvider>
        <PlatformTarget>x86&</PlatformTarget>
        <BuildIpa>true</BuildIpa>
        <IpaPackageDir>$(OutputPath</IpaPackageDir>
    </PropertyGroup>
    ```

Альтернативный способ для сборки командной строки **msbuild** или **xbuild** заключается в добавлении аргумента `/p:` для установки свойства `IpaPackageDir`. В этом случае обратите внимание, что **msbuild** не развертывает выражения `$()`, передаваемые в командную строку, поэтому использовать синтаксис `$(OutputPath)` невозможно. Вместо этого необходимо предоставить полный путь. Команда Mono **xbuild** раскрывает выражения `$()`, однако по-прежнему рекомендуется использовать полный путь к файлу, так как вместо **xbuild** рекомендуется использовать [кроссплатформенную версию **msbuild**](https://www.mono-project.com/docs/about-mono/releases/5.0.0/#msbuild).

Полный пример, в котором используется этот подход, в ОС Windows может выглядеть следующим образом:

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

В Mac он будет иметь следующий вид:

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

<a name="installipa"></a>

## <a name="installing-an-ipa-using-itunes"></a>Установка IPA с помощью iTunes

Полученный пакет IPA можно доставить вашим тестовым пользователям для установки на устройствах iOS или передать для корпоративного развертывания. Независимо от выбранного метода конечный пользователь установит пакет в своем приложении iTunes на Mac или компьютере с Windows, дважды щелкнув файл IPA (или перетащив его в открытое окно iTunes).

Новое приложение iOS будет отображаться в разделе **Мои приложения**, где его можно щелкнуть правой кнопкой мыши и получить сведения о приложении:

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

 ![](ipa-support-images/installxs01.png "The new iOS application in the My Apps section")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

 ![](ipa-support-images/installvs01.png "The new iOS application in the My Apps section")

-----

Пользователь теперь может синхронизировать iTunes со своим устройством, чтобы установить новое приложение iOS.

<a name="Summary"></a>

## <a name="summary"></a>Сводка

Эта статья описывает настройку, необходимую для подготовки приложения Xamarin.iOS к сборке, отличной от App Store. В ней показано, как создать пакет IPA и установить полученное приложение iOS на устройстве iOS конечного пользователя для тестирования или внутреннего распространения.

## <a name="related-links"></a>Связанные ссылки

- [Распространение через Магазин приложений](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Настройка приложения в iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Публикация в App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Внутреннее распространение](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Прямое распространение](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Файл iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Устранение неполадок](~/ios/deploy-test/troubleshooting.md)
- [Иллюстрации iTunes](~/ios/app-fundamentals/images-icons/app-icons.md#itunes)
- [Разработка и распространение корпоративных приложений (Apple)](https://help.apple.com/xcode/mac/current/#/devba5e7054d)
- [Распространение корпоративных приложений (видео с WWDC)](https://developer.apple.com/videos/play/wwdc2014/705/)
