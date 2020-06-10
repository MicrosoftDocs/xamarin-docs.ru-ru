---
title: Устранение неполадок приложений tvOS, созданных с помощью Xamarin
description: Эта статья содержит различные советы по устранению неполадок во время разработки приложения tvOS, созданного с помощью Xamarin. Здесь описываются известные проблемы и конкретные ошибки.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 124E4953-4DFA-42B0-BCFC-3227508FE4A6
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: c73be27ed82a643b01528ccba3887f59beeceb53
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574058"
---
# <a name="troubleshooting-tvos-apps-built-with-xamarin"></a>Устранение неполадок приложений tvOS, созданных с помощью Xamarin

_В этой статье рассматриваются проблемы, которые могут возникнуть при работе с поддержкой tvOS в Xamarin._

<a name="Known-Issues"></a>

## <a name="known-issues"></a>Известные проблемы

В текущем выпуске поддержки Xamarin tvOS имеются следующие известные проблемы.

- **Платформа Mono** — моно 4,3 Cryptography. ProtectedData не удалось расшифровать данные из Mono 4,2. В результате пакет NuGet не сможет восстановиться с ошибкой `Data unprotection failed` при настройке защищенного источника NuGet.
  - **Обходной путь** . в Visual Studio для Mac необходимо будет добавить все источники пакетов NuGet, использующие проверку подлинности с помощью пароля, перед повторной попыткой восстановления пакетов.
- **Visual Studio для Mac w/F # надстройка** — ошибка при создании шаблона F # Android в Windows. Это по-прежнему работает правильно на Mac.
- **Xamarin. Mac** — при запуске проекта унифицированного шаблона Xamarin. Mac с целевой платформой, для которой задано значение `Unsupported` , может отобразиться всплывающее окно `Could not connect to the debugger` .
  - **Возможное решение** — понизить версию платформы Mono, доступную в стабильном канале.
- **Xamarin Visual Studio & Xamarin. iOS** — при развертывании приложений WatchKit в Visual Studio может возникнуть ошибка `The file ‘bin\iPhoneSimulator\Debug\WatchKitApp1WatchKitApp.app\WatchKitApp1WatchKitApp’ does not exist` .

Сообщите об ошибках, найденных на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

## <a name="troubleshooting"></a>Диагностика

В следующих разделах перечислены некоторые известные проблемы, которые могут возникнуть при использовании tvOS 9 с Xamarin. tvOS, и решение этих проблем.

### <a name="invalid-executable---the-executable-does-not-contain-bitcode"></a>Недопустимый исполняемый файл — исполняемый объект не содержит bitcode

При попытке отправить приложение Xamarin. tvOS в магазин приложений Apple TV может появиться сообщение об ошибке в формате _"недопустимый исполняемый файл — исполняемый файл не содержит bitcode"_.

Чтобы решить эту проблему, выполните следующие действия.

1. В Visual Studio для Mac щелкните правой кнопкой мыши файл проекта Xamarin. tvOS в **Обозреватель решений** и выберите пункт **Параметры**.
2. Выберите **Сборка tvOS** и убедитесь, что вы используете конфигурацию **выпуска** : 

    [![](troubleshooting-images/ts01.png "Select tvOS Build options")](troubleshooting-images/ts01.png#lightbox)
3. Добавьте `--bitcode=asmonly` в поле **Дополнительные аргументы mtouch** и нажмите кнопку **ОК** .
4. Перестройте приложение в конфигурации **выпуска** .

### <a name="verifying-that-your-tvos-app-contains-bitcode"></a>Проверка того, что приложение tvOS содержит Bitcode

Чтобы убедиться, что сборка приложения Xamarin. tvOS содержит Bitcode, откройте приложение терминала и введите следующее:

```csharp
otool -l /path/to/your/tv.app/tv
```

В выходных данных найдите следующее:

```csharp
Section
  sectname __bundle
   segname __LLVM
      addr 0x0000000100001000
      size 0x000000000000124f
    offset 4096
     align 2^0 (1)
    reloff 0
    nreloc 0
     flags 0x00000000
 reserved1 0
 reserved2 0
```

`addr`и `size` будут отличаться, но другие поля должны быть идентичными.

Необходимо убедиться, что любые сторонние `.a` библиотеки (), которые вы используете, были созданы для библиотек tvOS (а не библиотек iOS) и что они также содержат сведения о bitcode.

Для приложений или библиотек, которые включают допустимый bitcode, значение `size` будет больше единицы. В некоторых ситуациях, когда библиотека может иметь маркер bitcode, но не содержит допустимого bitcode. Пример.

**Недопустимый Bitcode**

```csharp
 $ otool -arch arm64 libLibrary.a | grep __bitcode -A 3
   sect name __bitcode
   segname __LLVM
      add 0x0000000000000670
      size 0x0000000000000001
```

**Допустимый Bitcode** 

```csharp
$ otool -l -arch arm64 libDownloadableAgent-tvos.a |grep __bitcode -A 3
   sectname __bitcode
   segname __LLVM
      addr 0x000000000001d2d0
      size 0x0000000000045440
```

Обратите внимание на разницу `size` между двумя библиотеками в приведенном выше примере выполнения. Библиотека должна быть создана из Xcode архивной сборки с включенным параметром bitcode (Xcode `ENABLE_BITCODE` ) в качестве решения для этой проблемы размера.

### <a name="apps-that-only-contain-the-arm64-slice-must-also-have-arm64-in-the-list-of-uirequireddevicecapabilities-in-infoplist"></a>В приложениях, содержащих только срез arm64, должно быть значение "arm64" в списке Уирекуиреддевицекапабилитиес в info. plist

При отправке приложения в магазин приложений Apple TV для публикации может появиться сообщение об ошибке в форме:

_"Приложения, которые содержат только срез arm64, также должны иметь" arm64 "в списке Уирекуиреддевицекапабилитиес в info. plist"._

В этом случае измените `Info.plist` файл и убедитесь, что он имеет следующие ключи:

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
  <string>arm64</string>
</array>
```

Перекомпилируйте приложение для выпуска и повторно отправьте его в iTunes Connect.

### <a name="task-mtouch-execution----failed"></a>Выполнение задачи "MTouch"--сбой

Если вы используете библиотеку стороннего производителя (например, в виде «долгих игр») и при компиляции выпуска произошел сбой с длинной серией сообщений об ошибках `Task "MTouch" execution -- FAILED` , которые заканчиваются, попробуйте добавить `-gcc_flags="-framework OpenAL"` **Дополнительные аргументы касания**:

[![](troubleshooting-images/mtouch01.png "Task MTouch execution")](troubleshooting-images/mtouch01.png#lightbox)

Кроме того, необходимо включить `--bitcode=asmonly` в **Дополнительные аргументы касания**, задать для параметров компоновщика значение **связать все** и выполнить чистую компиляцию.

### <a name="itms-90471-error-the-large-icon-is-missing"></a>ИТМС — ошибка 90471. Крупный значок отсутствует

При получении сообщения в формате "ИТМС-90471 ошибка". Крупный значок отсутствует "при попытке отправить приложение Xamarin. tvOS в магазин приложений Apple TV для выпуска, проверьте следующее:

1. Убедитесь, что вы включили крупные ресурсы значков в `Assets.car` файл, созданный в документации по [значкам приложений](~/ios/tvos/app-fundamentals/icons-images.md#App-Icons) .
2. Убедитесь, что файл включен `Assets.car` из документации по [работе с значками и изображениями](~/ios/tvos/app-fundamentals/icons-images.md) в конечном пакете приложений.

### <a name="invalid-bundle--an-app-that-supports-game-controllers-must-also-support-the-apple-tv-remote"></a>Недопустимый пакет — приложение, которое поддерживает игровые контроллеры, должно также поддерживать удаленное устройство Apple TV.

или 

### <a name="invalid-bundle--apple-tv-apps-with-the-gamecontroller-framework-must-include-the-gcsupportedgamecontrollers-key-in-the-apps-infoplist"></a>Недопустимый пакет — приложения Apple TV с платформой Гамеконтроллер должны содержать ключ Гксуппортедгамеконтроллерс в файле info. plist приложения.

Игровые контроллеры можно использовать для улучшения игрового процесса и предоставления смысла в игре. Они также могут использоваться для управления стандартным интерфейсом Apple TV, поэтому пользователю не нужно переключаться между удаленным и контроллером.

Если вы отправляете приложение Xamarin. tvOS с поддержкой игровых контроллеров в магазин приложений Apple TV и получаете сообщение об ошибке в виде:

_Обнаружена одна или несколько проблем с последней доставкой "имя приложения". Ваша доставка прошла успешно, но при следующей доставке может потребоваться устранить следующие проблемы:_

_Недопустимый пакет — приложение, которое поддерживает игровые контроллеры, также должно поддерживать удаленное устройство Apple TV._

или 

_Недопустимый пакет — приложения Apple TV с платформой Гамеконтроллер должны содержать ключ Гксуппортедгамеконтроллерс в файле info. plist приложения._

Решение заключается в добавлении поддержки Siri Remote ( `GCMicroGamepad` ) в `Info.plist` файл приложения. Профиль контроллера Micro Game был добавлен компанией Apple для целевого объекта Siri Remote. Например, включите следующие ключи:

```xml
<key>GCSupportedGameControllers</key>  
  <array>  
    <dict>  
      <key>ProfileName</key>  
      <string>ExtendedGamepad</string>  
    </dict>  
    <dict>  
      <key>ProfileName</key>  
      <string>MicroGamepad</string>  
    </dict>  
  </array>  
<key>GCSupportsControllerUserInteraction</key>  
<true/>
```

> [!IMPORTANT]
> Игровые контроллеры Bluetooth — это необязательная покупка, которую могут выполнять пользователи. приложение не может заставить пользователя приобретать его. Если приложение поддерживает игровые контроллеры, оно также должно поддерживать Siri Remote, чтобы играть можно было использовать для всех пользователей Apple TV.

Дополнительные сведения см. в разделе [Working with Game Controllers](~/ios/tvos/platform/remote-bluetooth.md#Working-with-Game-Controllers) статьи [Siri Remote and Bluetooth Controllers](~/ios/tvos/platform/remote-bluetooth.md) Documentation.

### <a name="incompatible-target-framework-netportable-versionv45-profileprofile78"></a>Несовместимая Целевая платформа:. Нетпортабле, версия = v 4.5, профиль = Profile78

При попытке включить в проект Xamarin. tvOS переносимую библиотеку классов (PCL) может появиться сообщение в формате:

_Несовместимая Целевая платформа:. Нетпортабле, версия = v 4.5, профиль = Profile78_

Чтобы решить эту проблему, добавьте XML-файл `Xamarin.TVOS.xml` с именем со следующим содержимым:

```xml
<Framework Identifier="Xamarin.TVOS" MinimumVersion="1.0" Profile="*" DisplayName="Xamarin.TVOS"/>
```

По следующему пути:

```csharp
/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/Profile/Profile259/SupportedFrameworks/

```

Обратите внимание, что номер профиля в пути должен совпадать с номером профиля PCL.

Заполнив этот файл, вы сможете успешно добавить файл PCL в проект Xamarin. tvOS.

## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [Руководства по tvOSму интерфейсу](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Руководством по программированию приложений для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
