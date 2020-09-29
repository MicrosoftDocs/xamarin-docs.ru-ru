---
title: Часто задаваемые вопросы о Xamarin. iOS
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 65E04188-185D-493D-BA3C-A89711CB6CAF
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 5e258f350256945c7794ee67f814d30d09894c9a
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91432170"
---
# <a name="ios-frequently-asked-questions"></a>Часто задаваемые вопросы о iOS

## <a name="general-questions"></a>Общие вопросы

### <a name="can-i-use-a-mac-vm-with-xamarin"></a>[Можно ли использовать виртуальную машину Mac с Xamarin?](mac-vm.md)
Да, но только на оборудовании Mac.

### <a name="how-can-i-downgrade-xcode"></a>[Как перейти на использование более ранней версии Xcode?](./previous-xcode.md)
В этом руководством содержатся ссылки на доступ к предыдущим версиям Xcode, а также к последней версии.

### <a name="where-can-i-set-my-ios-sdk-locations"></a>[Где можно задать свои расположения пакета SDK для iOS?](ios-sdk.md)
Для большинства пользователей они автоматически устанавливаются в соответствующие расположения. В этом руководство перечислены расположения пакетов SDK по умолчанию и способы их изменения при необходимости.

### <a name="how-can-i-reenable-developer-options-after-updating-ios"></a>[Как снова включить параметры разработчика после обновления iOS?](update-developer-options.md)
Ошибка iOS может привести к исчезновению параметров разработчика после обновления версий iOS. это наблюдается при переключении на iOS 8. x. В этом руководство описано, как можно повторно включить параметры.

### <a name="user-location-not-working-in-ios-8"></a>[Расположение пользователя не работает в iOS 8](ios8-user-location.md)
В этом руководство объясняется, как изменить info. plist, чтобы включить расположение пользователя в iOS 8.

### <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>[Где найти dSYM-файл, чтобы выразить символами журналы с данными об аварийном завершении iOS?](symbolicate-ios-crash.md)
В этом руководстве описываются основные шаги для журналов аварийного завершения симболикатинг iOS, помогающие диагностировать сбои. Он также содержит ссылки на дополнительные ресурсы для более сложных приемов символов & сведения о интерпретации журналов сбоев iOS.

### <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>[Как задать переменные среды выполнения Mono для проектов iOS в Xamarin Studio?](xs-mono-runtime.md)
Если необходимо задать переменные среды выполнения для Mono, их можно задать в **параметрах проекта > запуска > страница Общие** .

## <a name="publishing-questions"></a>Вопросы публикации

### <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>[Ошибка при отправке в App Store: в отправке обнаружены недопустимые параметры пакета, которые должны быть внедрены в bitcode.](invalid-bundle-bitcode.md)

Отправка приложений, _требующих_ bitcode, таких как приложения WatchOS и tvOS, необходимо выполнить с помощью Xcode 9.

### <a name="can-i-change-the-output-path-of-the-ipa-file"></a>[Можно ли изменить выходной путь к файлу IPA?](ipa-output-path.md)
Начиная с Xamarin Cycle 7 для достижения этого результата можно использовать настраиваемые целевые объекты MSBuild.

### <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>[Как скопировать выходные файлы IPA в папку сброса TFS?](ipa-tfs.md)
Да, это руководство описывает, как это делать.

### <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>[Можно ли добавлять или удалять файлы из файла IPA после его создания в Visual Studio?](modify-ipa.md)
Да, это возможно, но обычно потребуется повторно подписать `.app` пакет после внесения изменений. Обратите внимание, что изменение `.ipa` файла не является обязательным при нормальном использовании. Эта статья предоставляется исключительно в информационных целях.

### <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>[Можно ли создать xcarchive Архив из Visual Studio?](create-xcarchive.md)
Начиная с Xamarin 4, теперь можно создать `.xcarchive` из Windows, задав `ArchiveOnBuild` свойству значение `true` .

### <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--"></a>[Почему отправка приложения завершается сбоем с: "запрещенные пути (" файла itunesmetadata. plist ") найдены в..."?](itunesmetadata-disallowed-paths.md)
Эта ошибка является результатом изменения в процессе проверки в магазине приложений Apple. Эта конкретная ошибка _не_ связана с конкретной версией Xamarin, которую вы установили, поэтому переход на использование более ранней версии _не_ поможет. Это руководство содержит ссылки на дополнительные сведения о том, как устранить проблему.

## <a name="diagnosing-specific-error-messages"></a>Диагностика конкретных сообщений об ошибках

### <a name="ios-designer-error-with-registerserviceport"></a>[Ошибка конструктора iOS RegisterServicePort](error-registerserviceport.md)
Ошибки `RegisterServicePort` и подобные сообщения об ошибках, подобные описанным выше, обычно являются проблемой с программами-шпионами и вредоносными программами на компьютере. В этом руководством содержатся сведения о диагностике и сведения об удалении шпионских и вредоносных программ.

### <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychain"></a>[Почему сборка iOS завершается с ошибкой: не найдены допустимые ключи подписывания кода iPhone в цепочке ключей?](no-codesigning-keys.md)
Это сообщение об ошибке возникает, если рассматриваемый проект ищет действительные учетные данные подписывания кода, но их не удается найти. Для тестирования и развертывания на физических устройствах iOS требуется подписывание кода. а также ad-hoc & сборки магазина приложений.

### <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>[Почему приложение iOS 9 завершается с ошибкой: System. Exception: не удалось маршалировать объект цели-C?](exception-marshal-obj-c.md)
Изменения API в iOS 9 потребовали использования конструктора обратного вызова при вызове неуправляемого кода, так как этот интерфейс API теперь предполагает его.

### <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loaded"></a>[Ошибка выполнения: сборка mscorlib.dll не найдена или не может быть загружена](error-mscorlib-not-found.md)
Эта проблема возникает, когда *скрытые* `.monotouch-32` `.monotouch-64` папки и отсутствуют в `.xcarchive` для создания подписывания или IPA и запускают ошибку во время выполнения.

### <a name="compile-error-can-not-encode-offset-x-in-resulting-scattered-relocation"></a>[Ошибка компиляции: не удается закодировать смещение X в результирующем расположении с рассеиванием](error-encode-offset-scattered-relocation.md)
Эта проблема возникает при сборке для 32-разрядных архитектур, например ARMv7, когда конечный двоичный файл слишком велик для собственного цепочки инструментов.

## <a name="deprecated"></a>Не рекомендуется

> [!IMPORTANT]
> Приведенные ниже статьи относятся к проблемам, которые были решены в последних версиях Xamarin. Однако если проблема возникает в последней версии программного обеспечения, запишите [новую ошибку](~/cross-platform/troubleshooting/questions/howto-file-bug.md) с полными сведениями о версии и полным выходным файлом журнала сборки.

### <a name="ipa-file-is-0-bytes"></a>[Размер IPA-файла составляет 0 байт](ipa-zero-bytes.md)
В предыдущих версиях Xamarin существовали некоторые известные проблемы, которые могут привести к тому, что файл IPA в Windows будет иметь значение 0 байт.

### <a name="ibtool-error-the-operation-couldnt-be-completed"></a>[Ошибка Ибтул: не удалось завершить операцию.](error-ibtool.md)
Компания Apple устранила эту `ibtool` ошибку в Xcode 6.1.1, поэтому обновление до Xcode 6.1.1 или более поздней версии является самым простым исправлением.

### <a name="error-mt1009-could-not-copy-the-assembly"></a>[Ошибка MT1009: не удалось скопировать сборку](error-mt1009.md)
Это влияет на пользователей, использующих Xamarin. iOS 7.2.6. Эта проблема вызвана тем, что при установке Xamarin. iOS с другой учетной записью пользователя, которая является основной учетной записью разработчика, требуются более высокие права доступа к файлам.

### <a name="systemexception-amdevicenotificationsubscribe-returned-"></a>[Возвращено исключение System.Exception AMDeviceNotificationSubscribe…](exception-amddevicenotificationsubscribe.md)
Это сообщение может появиться в диалоговом окне ошибки при первом запуске Visual Studio для Mac или в `mtbserver.log` файле. Обратите внимание, что это нетипичная проблема. Если Visual Studio не удается подключиться к узлу сборки Mac, существуют другие ошибки, которые скорее всего появятся в `mtbserver.log` файле.

### <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequest"></a>[MDocArchiveToMsxDocConverter.exe не найден, rver.BaseCommand.OnRequest](mdocarchivetomsxdocconverter-not-found.md)
Эта ошибка может появиться в в `Mac Server Log` Visual Studio.