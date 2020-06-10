---
title: Развертывание приложений watchOS в App Store
description: В этом документе описывается развертывание приложений watchOS, созданных с помощью Xamarin, в App Store. В нем рассматриваются профили подготовки распространителя и iTunes Connect, а также приведены некоторые советы по устранению неполадок.
ms.prod: xamarin
ms.assetid: DBE16040-70D2-4F61-B5F3-C8D213DBC754
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: f42c4a6dd2f4408d7b0ccf65e60851ebd29b76f6
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574550"
---
# <a name="deploying-watchos-apps-to-the-app-store"></a>Развертывание приложений watchOS в App Store

> [!IMPORTANT]
> Обязательно ознакомьтесь с [руководством по отправке контрольного набора для Apple Watch](https://developer.apple.com/app-store/watch/)и ознакомьтесь с разделом [Устранение](#troubleshooting) проблем.

- Убедитесь, что у вас есть:
  - [**Профили подготовки распространения**](#provisioning) , созданные для проектов.
  - **Цель развертывания** ( `MinimumOSVersion` ) для родительского приложения iOS со значением **8,2** или более ранней (8,3 не поддерживается).

- В [**iTunes Connect**](#iTunes_Connect):

  - Создайте запись приложения iOS (или добавьте **новую версию** в существующее приложение).
  - Добавьте значок контрольных снимков и снимки экрана.

- Затем в [Visual Studio для Mac](#xamarin_studio) (Visual Studio в настоящее время не поддерживается):

  - Щелкните правой кнопкой мыши приложение iOS и выберите **Назначить запускаемым проектом**.
  - Перейдите в конфигурацию **App Store** .
  - С помощью функции **архивации** создайте архив приложения.

- Наконец, переключитесь на [Xcode 6.2 +](#xcode)

  - Перейдите в **окно > организатора** и выберите **архивы**.
  - Выберите приложение и архив из списка.
  - При необходимости **Проверить...** Архив.
  - **Отправить...** Архив и выполните действия по отправке в iTunes Connect для проверки и утверждения.

Ознакомьтесь с конкретными советами, связанными с этими элементами ниже. Если у вас возникли проблемы, см. раздел [Устранение неполадок](#troubleshooting) .

<a name="provisioning"></a>

## <a name="distribution-provisioning-profiles"></a>Профили подготовки распространения

Чтобы выполнить сборку для развертывания App Store, необходимо создать **профиль подготовки распространения** для каждого идентификатора приложения в решении.

Если у вас есть идентификатор приложения с подстановочными знаками, *потребуется только один профиль подготовки*. но если у вас есть отдельный идентификатор приложения для каждого проекта, вам потребуется профиль подготовки для каждого идентификатора приложения:

![](appstore-images/provisioningprofile-distribution-sml.png "The App Store Distribution profile")

После создания всех трех профилей они будут отображаться в списке. Не забудьте скачать и установить каждый из них (дважды щелкните его):

![](appstore-images/provisioningprofiles-sml.png "The list of available profiles")

Проверить профиль подготовки можно в **параметрах проекта** . для этого выберите пункт **Сборка > пакет iOS** и выберите Конфигурация **AppStore | iPhone** .

В списке **профиль подготовки** будут показаны все профили сопоставления. Вы должны увидеть профили сопоставления, созданные в этом раскрывающемся списке.

![](appstore-images/options-selectprofile-sml.png "The iOS Bundle Signing dialog")

<a name="iTunes_Connect"></a>

## <a name="itunes-connect"></a>iTunes Connect

Выполните [Общие сведения о распространении приложений](~/ios/deploy-test/app-distribution/index.md), в частности:

- [Настройка приложения в iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Публикация в Магазине приложений](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)

При настройке приложения в iTunes Connect не забудьте добавить значок контрольного снимка и снимков экрана:

![](appstore-images/itunesconnect-watch-sml.png "The Watch icon and screenshots in iTunes Connect")

Файл значка должен быть 1024x1024 пикселями, и к нему будет применена циклическая маска при его отображении. Значок не должен иметь альфа-канал.

Требуется по крайней мере один снимок экрана, но можно отправить до пяти.
Они должны быть 312x390 пикселями и демонстрировать приложение Watch в действии.
Для получения снимков экрана с таким размером можно использовать симулятор часы Watch.

<a name="xamarin_studio"></a>

## <a name="visual-studio-for-mac"></a>Visual Studio для Mac

1. Убедитесь, что приложение iOS является запускаемым проектом. Если это не так, щелкните правой кнопкой мыши, чтобы задать его.

   ![](appstore-images/xs-startup.png "Setting the startup project")

2. Выберите конфигурацию сборки **AppStore** :

   ![](appstore-images/xs-appstore.png "The AppStore build configuration")

3. Выберите пункт меню " **сборка > Архив** ", чтобы запустить процесс архивации:

   ![](appstore-images/xs-archive.png "The Build menu")

Можно также выбрать пункт меню **просмотреть > архивы...** , чтобы просмотреть ранее созданные архивы.

  ![](appstore-images/xs-archives-sml.png "The Archives view")

<a name="xcode"></a>

## <a name="xcode"></a>Xcode

Xcode автоматически отображает архивы, созданные в Visual Studio для Mac.

1. Запустите Xcode и выберите **окно организатор >**:

   ![](appstore-images/xc-organizer.png "The Window menu")

2. Перейдите на вкладку **архивы** и выберите архив, созданный с помощью Visual Studio для Mac:

   ![](appstore-images/xc-archives.png "The Archives tab")

3. При необходимости **Проверьте...** Архив, а затем нажмите кнопку **Отправить...** , чтобы отправить приложение в iTunes Connect.

4. Выберите команду разработчиков (если вы принадлежите к нескольким) и Подтвердите отправку:

   ![](appstore-images/xc-submit1.png "The development team section")

5. Перейдите в iTunes Connect еще раз, чтобы просмотреть отправленный двоичный файл. Перейдите на страницу настройки приложения и выберите в верхнем меню пункт **Предварительная версия** , чтобы просмотреть список **сборок** :

   [![](appstore-images/itc-prerelease-sml.png "The apps configuration page in iTunes Connect")](appstore-images/itc-prerelease.png#lightbox)

Затем вы можете отправить приложение на утверждение на странице **версии** . Дополнительные сведения см. в статье [Обзор распространения приложений для iOS](~/ios/deploy-test/app-distribution/index.md) .

## <a name="troubleshooting"></a>Диагностика

Ниже приведены некоторые ошибки, которые могут возникнуть при отправке в магазин приложений, и действия, которые можно предпринять для их устранения.

### <a name="archive-menu-option-is-not-visible-in-visual-studio-for-mac"></a>Пункт меню "Архив" не отображается в Visual Studio для Mac

Выполните описанные [выше действия](#xamarin_studio) , чтобы настроить решение для архивирования. Если не удается правильно задать запускаемый проект, перед изменением запускаемого проекта убедитесь, что для конфигурации сборки сначала задано значение Отладка или выпуск. Затем снова задайте для конфигурации сборки значение **AppStore**.

### <a name="invalid-icon"></a>Значок «Недопустимо»

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/AppIcon27.5x27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Следуйте [инструкциям по удалению альфа-канала](~/ios/watchos/troubleshooting.md) из значков.

### <a name="cfbundleversion-mismatch"></a>Несоответствие CFBundleVersion

```csharp
CFBundleVersion Mismatch. The CFBundleVersion value '1' of watch application
'...watchkitextension.appex/WatchApp.app' does not match the CFBundleVersion
value '1.0' of its containing iOS application `YouriOS.app`.
```

Все проекты в решении — приложение iOS, расширение Watch и приложение Watch — должны использовать один и тот же номер версии. Измените каждый файл **info. plist** , чтобы номер версии совпадал точно с ним.

### <a name="missing-icons"></a>Отсутствующие значки

```csharp
Missing Icons. No icons found for watch application '...watchkitextension.appex/WatchApp.app'.
Please make sure that its Info.plist file includes entries for CFBundleIconFiles.
```

Следуйте инструкциям в разделе [Работа со значками](~/ios/watchos/app-fundamentals/icons.md) , чтобы добавить все необходимые изображения в проект Watch App.

### <a name="missing-icon"></a>Отсутствует значок

```csharp
Missing Icon. The watch application '...watchkitextension.appex/WatchApp.app'
is missing icon with name pattern '*44x44@2x.png' (Home Screen 42mm).
```

Убедитесь, что у вас установлена последняя версия Visual Studio для Mac и что **AppIcon. appiconset** содержит полный набор образов. Если эта ошибка по-прежнему возникает, просмотрите исходный код файла **Content. JSON** , чтобы убедиться, что он содержит запись для всех необходимых изображений. Кроме того, если вы уверены, что используете последнюю версию Xamarin, удалите и повторно создайте **AppIcon. appiconset**.

> [!IMPORTANT]
> Обнаружена известная ошибка в Visual Studio для Mac поддержка значка просмотра: она ожидает изображение в виде 88x88 пикселя для **29x29@3x** изображения (которое должно быть 87x87 пикселей).

Вы не можете исправить это в Visual Studio для Mac. Измените ресурс изображения в Xcode или вручную измените файл **Content. JSON** .

### <a name="invalid-watchkit-support"></a>Недопустимая поддержка WatchKit

```csharp
Invalid WatchKit Support - The bundle contains an invalid implementation of WatchKit.
The app may have been built or signed with non-compliant or pre-release tools.
```

Это сообщение может появиться во время проверки и отправки, а также в автоматическом сообщении электронной почты из iTunes Connect после вероятной успешной отправки.
<!--
Ensure you are using the latest version of Xcode and Xamarin's tools.
-->
> [!IMPORTANT]
> Необходимо **заархивировать** приложение в Visual Studio для Mac, а затем переключиться на Xcode 6.2 + для проверки и отправки в iTunes Connect.

Используйте стабильный канал Xamarin и Xcode 6.2 +.

### <a name="invalid-provisioning-profile"></a>Недопустимый профиль подготовки

```csharp
Invalid Provisioning Profile. The provisioning profile included in the bundle
...iOSWatchApp.watchkitapp [iOSWatchApp.app/PlugIns/...iOSWatchApp.watchkitextension.appex/WatchApp.app]
is invalid. [Missing code-signing certificate.]
```

**Профили подготовки распространения** должны быть предоставлены для всех трех проектов в решении "Контрольное Приложение": приложение iOS, расширение Watch и приложение Watch — либо явно (три профиля), либо с помощью одного профиля с подстановочными знаками. Убедитесь, что профили подготовки существуют в центре разработки iOS и были скачаны и добавлены на компьютер Mac.

### <a name="invalid-code-signing-entitlements"></a>Недопустимые права на подписывание кода

```csharp
ITMS-90046: Invalid Code Signing Entitlements. Your application bundle's signature contains
code signing entitlements that are not supported on iOS. Specifically, value
'...watchkitextension' for key 'application-identifier' in '...watchkitextension'
is not supported. The value should be a string startign with your TEAMID, followed
by a dot '.' followed by the bundle identifier.
```

Убедитесь, что профили подготовки правильно настроены в центре разработки Apple и что вы скачали и установили их. Также убедитесь, что они заданы в окне свойств Visual Studio для Mac для каждого проекта.

### <a name="invalid-architecture"></a>Недопустимая архитектура

```csharp
Invalid architecture: Apps that include an app extension
and framework must support arm64.
```

Вы можете добавлять только приложения [-часы Unified API (64-разрядные)](~/cross-platform/macios/unified/index.md) приложения Xamarin. iOS.
Щелкните правой кнопкой мыши проект приложения iOS, а затем последовательно выберите **пункты параметры > сборка > iOS > вкладка "Дополнительно"** и убедитесь, что **Поддерживаемые архитектуры** для конфигурации AppStore-iPhone включают **ARM64** (например, **ARMv7 + ARM64**).

### <a name="this-bundle-is-invalid"></a>Этот пакет является недопустимым.

```csharp
ITMS-90068: This bundle is invalid. The value provided for the key
MinimumOSVersion '8.3' is not acceptable.
```

Родительское приложение iOS должно иметь значение Минимумосверсион, равное "8,2" или более раннюю.

### <a name="non-public-api-usage"></a>Неоткрытое использование API

```csharp
Your app contains non-public API usage.
Please review the errors, and resubmit your application.
```

Убедитесь, что вы используете последнюю версию средств Xcode и Xamarin.
Ваш код не должен обращаться к каким-либо не открытым API.

### <a name="build-error-mt5309"></a>Ошибка сборки MT5309

```csharp
Error MT5309: Native linking error: clang: error: no such file or directory:
```

Эта ошибка, скорее всего, является результатом переименования установки Xcode из **Xcode. app**. Например, эта ошибка возникает при переименовании установки в **Xcode 6.2. app**.

## <a name="related-links"></a>Связанные ссылки

- [Рекомендации по отправке Apple WatchKit](https://developer.apple.com/app-store/watch/)
