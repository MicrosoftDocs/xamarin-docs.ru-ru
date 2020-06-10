---
title: Ошибки Xamarin. iOS
description: В этом документе описываются различные ошибки, создаваемые mtouch, средством, используемым для объединения приложений Xamarin. iOS. Ошибки перечисляются по коду и получают полное описание.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9F76162B-D622-45DA-996B-2FBF8017E208
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/06/2018
ms.openlocfilehash: 98d2ef76974e63df7c1b0596d59c638590339fa3
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84570873"
---
# <a name="xamarinios-errors"></a>Ошибки Xamarin. iOS

## <a name="mt0xxx-mtouch-error-messages"></a>MT0xxx: сообщения об ошибках mtouch

Пример: параметры, среда, отсутствующие инструменты.

<!--
 MT0xxx mtouch itself, e.g. parameters, environment (e.g. missing tools)
 https://github.com/xamarin/xamarin-macios/blob/master/tools/mtouch/error.cs
  -->

<a name="MT0000"></a>

### <a name="mt0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcomxamarinxamarin-maciosissuesnew"></a>MT0000: непредвиденная ошибка. Заполните отчет об ошибке по адресу.https://github.com/xamarin/xamarin-macios/issues/new

Произошло непредвиденное условие ошибки. Запишите новую ошибку в [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) с максимально возможной информацией, включая:

- Полные журналы сборки с максимальным уровнем детализации (например `-v -v -v -v` , в **дополнительных аргументах mtouch**);
- Минимальный тестовый случай, воспроизводящий ошибку; перетаскивани
- Все сведения о версиях

Самый простой способ получить точную информацию о версии — использовать меню « **Visual Studio для Mac** », « **Visual Studio для Mac** элемент», « **отобразить сведения** » и скопировать/вставить сведения о версии (можно использовать кнопку « **Копировать информацию** »).

<a name="MT0001"></a>

### <a name="mt0001--devname-was-provided-without-any-device-specific-action"></a>MT0001: "-девнаме" предоставлен без каких-либо действий с устройством

Это предупреждение возникает, если девнаме передается в mtouch, когда не было запрошено никаких действий для конкретного устройства (-логдев/-инсталлдев/-киллдев/-лаунчдев/-листаппс).

<a name="MT0002"></a>

### <a name="mt0002-could-not-parse-the-environment-variable-"></a>MT0002: не удалось проанализировать переменную среды *.

Эта ошибка возникает при попытке задать недопустимую пару переменных "ключ — значение" среды. Правильный формат:`mtouch --setenv=VARIABLE=VALUE`

<a name="MT0003"></a>

### <a name="mt0003-application-name-exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MT0003: имя приложения "*. exe" конфликтует с именем пакета SDK или сборки продукта (. dll).

Имя исполняемой сборки и имя приложения не могут совпадать с именем любой библиотеки DLL в приложении. Измените имя исполняемого файла.

<a name="MT0004"></a>

### <a name="mt0004-new-refcounting-logic-requires-sgen-to-be-enabled-too"></a>MT0004. для новой логики рефкаунтинг необходимо включить SGen.

При включении расширения рефкаунтинг необходимо также включить сборщик мусора SGen в параметрах сборки iOS проекта (вкладка "Дополнительно").

Начиная с Xamarin. iOS 7.2.1 это требование было уничтожено. новую логику рефкаунтинг можно включить с помощью сборщиков мусора Boehm и SGen.

<a name="MT0005"></a>

### <a name="mt0005-the-output-directory--does-not-exist"></a>MT0005: выходной каталог * не существует.

Создайте каталог.

Эта ошибка больше не создается, mtouch автоматически создает каталог, если он не существует.

<a name="MT0006"></a>

### <a name="mt0006-there-is-no-devel-platform-at--use---platformplat-to-specify-the-sdk"></a>MT0006: отсутствует платформа которые на сайте *, используйте параметр--Platform = PLAT, чтобы указать пакет SDK.

Xamarin. iOS не удается найти каталог SDK в расположении, указанном в сообщении об ошибке. Проверьте правильность пути.

<a name="MT0007"></a>

### <a name="mt0007-the-root-assembly--does-not-exist"></a>MT0007: корневая сборка * не существует.

Xamarin. iOS не удается найти сборку в расположении, указанном в сообщении об ошибке. Проверьте правильность пути.

<a name="MT0008"></a>

### <a name="mt0008-you-should-provide-one-root-assembly-only-found--assemblies-"></a>MT0008: необходимо указать только одну корневую сборку, найдено # сборок: *.

В mtouch передано более одной корневой сборки, хотя может быть только одна корневая сборка.

<a name="MT0009"></a>

### <a name="mt0009-error-while-loading-assemblies-"></a>MT0009: ошибка при загрузке сборок: *.

Произошла ошибка при загрузке сборок из ссылок на корневую сборку. Дополнительные сведения можно указать в выходных данных сборки.

<a name="MT0010"></a>

### <a name="mt0010-could-not-parse-the-command-line-arguments-"></a>MT0010: не удалось проанализировать аргументы командной строки: *.

При синтаксическом анализе аргументов командной строки произошла ошибка. Убедитесь, что все они верны.

<a name="MT0011"></a>

### <a name="mt0011--was-built-against-a-more-recent-runtime--than-monotouch-supports"></a>MT0011: \* был создан в более поздней версии среды выполнения ( \* ), чем поддержка с поддержкой односенсорного ввода.

Это предупреждение обычно выводится, поскольку проект содержит ссылку на библиотеку классов, которая не была построена с помощью библиотеки BCL для Xamarin. iOS.

Точно так же, как приложение, использующее пакет SDK для .NET 4,0, может не работать в системе, поддерживающей .NET 2,0, Библиотека, созданная с помощью .NET 4,0, может не работать в Xamarin. iOS, она может использовать API, отсутствующий в Xamarin. iOS.

Общее решение состоит в том, чтобы создать библиотеку в виде библиотеки классов Xamarin. iOS. Это можно сделать, создав новый проект библиотеки классов Xamarin. iOS и добавив в него все исходные файлы. Если у вас нет исходного кода для библиотеки, обратитесь к поставщику и попросите предоставить совместимую с Xamarin. iOS версию своей библиотеки.

<a name="MT0012"></a>

### <a name="mt0012-incomplete-data-is-provided-to-complete-"></a>MT0012: для завершения * предоставлены неполные данные.

Эта ошибка больше не отображается в текущей версии Xamarin. iOS.

<a name="MT0013"></a>

### <a name="mt0013-profiling-support-requires-sgen-to-be-enabled-too"></a>MT0013: для поддержки профилирования необходимо включить SGen.

SGen (--Sgen) необходимо включить, если включено профилирование (--профилирование).

<a name="MT0014"></a>

### <a name="mt0014-the-ios--sdk-does-not-support-building-applications-targeting-"></a>MT0014: \* пакет SDK для iOS не поддерживает создание приложений, предназначенных для \* .

Это может произойти в следующих случаях.

- ARMv6 включен, а также установлен Xcode 4,5 или более поздней версии.
- ARMv7s включен, а также установлен Xcode 4,4 или более ранней версии.

Убедитесь, что установленная версия Xcode поддерживает выбранные архитектуры.

<a name="MT0015"></a>

### <a name="mt0015-invalid-abi--supported-abis-are-i386-x86_64--armv7-armv7llvm-armv7llvmthumb2-armv7s-armv7sllvm-armv7sllvmthumb2-arm64-and-arm64llvm"></a>MT0015: недопустимый ABI: *. Поддерживаемые ABI: i386, x86_64, ARMv7, ARMv7 + LLVM, ARMv7 + LLVM + thumb2, armv7s, armv7s + LLVM, armv7s + LLVM + thumb2, arm64 и arm64 + LLVM.

Недопустимый ABI передан в mtouch. Укажите допустимый ABI.

<a name="MT0016"></a>

### <a name="mt0016-the-option--has-been-deprecated"></a>MT0016: параметр * является устаревшим.

Упомянутый параметр mtouch является устаревшим и будет проигнорирован.

<a name="MT0017"></a>

### <a name="mt0017-you-should-provide-a-root-assembly"></a>MT0017: необходимо указать корневую сборку.

При создании приложения необходимо указать корневую сборку (обычно основной исполняемый файл).

<a name="MT0018"></a>

### <a name="mt0018-unknown-command-line-argument-"></a>MT0018: Неизвестный аргумент командной строки: *.

Mtouch не распознает аргумент командной строки, упомянутый в сообщении об ошибке.

<a name="MT0019"></a>

### <a name="mt0019-only-one---loginstallkilllaunchdev-or---launchdebugsim-option-can-be-used"></a>MT0019: можно использовать только один параметр--[log | Install | Kill | Launch] dev или--[Launch | Debug].

Существует несколько вариантов mtouch, которые нельзя использовать одновременно:

- --логдев
- --инсталлдев
- --киллдев
- --лаунчдев
- --лаунчдебуг
- --лаунчсим

<a name="MT0020"></a>

### <a name="mt0020-the-valid-options-for--are-"></a>MT0020: допустимые параметры для " \* " — "" \* .

<a name="MT0021"></a>

### <a name="mt0021-cannot-compile-using-gccg---use-gcc-when-using-the-static-registrar-this-is-the-default-when-compiling-for-device-either-remove-the---use-gcc-flag-or-use-the-dynamic-registrar---registrardynamic"></a>MT0021: не удается выполнить компиляцию с использованием gcc/g + + (--use-GCC) при использовании статического регистратора (это значение по умолчанию при компиляции для устройства). Либо удалите флаг--use-GCC, либо используйте динамический регистратор (--регистратор: Dynamic).

<a name="MT0022"></a>

### <a name="mt0022-the-options---unsupported--enable-generics-in-registrar-and---registrar-are-not-compatible"></a>MT0022: параметры "--не поддерживается--Enable-универсальные-in-регистратор" и "--Регистратор" несовместимы.

Удалите оба параметра `--unsupported--enable-generics-in-registrar` и `--registrar` . Начиная с Xamarin. iOS 7.2.1 регистратор по умолчанию поддерживает универсальные шаблоны.

Эта ошибка больше не отображается (аргумент командной строки `--unsupported--enable-generics-in-registrar` удален из mtouch).

<a name="MT0023"></a>

### <a name="mt0023-application-name-exe-conflicts-with-another-user-assembly"></a>MT0023: имя приложения "*. exe" конфликтует с другой сборкой пользователя.

Имя исполняемой сборки и имя приложения не могут совпадать с именем любой библиотеки DLL в приложении. Измените имя исполняемого файла.

<a name="MT0024"></a>

### <a name="mt0024-could-not-find-required-file-"></a>MT0024: не удалось найти требуемый файл "*".

<a name="MT0025"></a>

### <a name="mt0025-no-sdk-version-was-provided-please-add---sdkxy-to-specify-which-ios-sdk-should-be-used-to-build-your-application"></a>MT0025: не указана версия пакета SDK. Добавьте `--sdk=X.Y` , чтобы указать, какой пакет SDK для iOS следует использовать для сборки приложения.

<a name="MT0026"></a>

### <a name="mt0026-could-not-parse-the-command-line-argument--"></a>MT0026: не удалось проанализировать аргумент командной строки " \* ":\*

<a name="MT0027"></a>

### <a name="mt0027-the-options--and--are-not-compatible"></a>MT0027: параметры " \* " и " \* " несовместимы.

<a name="MT0028"></a>

### <a name="mt0028-cannot-enable-pie--pie-when-targeting-ios-41-or-earlier-please-disable-pie--piefalse-or-set-the-deployment-target-to-at-least-ios-42"></a>MT0028: невозможно включить КРУГовую диаграмму при использовании iOS 4,1 или более ранней версии. Отключите круговую диаграмму (-сектор: false) или задайте для цели развертывания значение не ниже iOS 4,2

<a name="MT0029"></a>

### <a name="mt0029-repl---enable-repl-is-only-supported-in-the-simulator---sim"></a>MT0029: REPL (--Enable-REPL) поддерживается только в симуляторе (--SIM).

REPL поддерживается только в том случае, если вы собираетесь в симулятор. Это означает, что при передаче `--enable-repl` в mtouch необходимо также передать `--sim` .

<a name="MT0030"></a>

### <a name="mt0030-the-executable-name--and-the-app-name--are-different-this-may-prevent-crash-logs-from-getting-symbolicated-properly"></a>MT0030: имя исполняемого файла ( \* ) и имя приложения ( \* ) различаются, это может препятствовать правильному получению проведения результативного журналов аварийного завершения.

Когда Xcode симболикатес (преобразует адреса памяти в имена функций и номера файлов и строк), процесс может завершиться ошибкой, если имя исполняемого файла и приложения имеют разные имена (без расширения).

Чтобы исправить это, измените "имя приложения" в параметрах сборки или приложения iOS проекта или измените "имя сборки" в параметрах сборки или вывода проекта.

<a name="MT0031"></a>

### <a name="mt0031-the-command-line-arguments---enable-background-fetch-and---launch-for-background-fetch-require---launchsim-too"></a>MT0031: аргументы командной строки "--Enable-background-FETCH" и "--Launch-for-Background-FETCH" должны также требовать "--лаунчсим".

<a name="MT0032"></a>

### <a name="mt0032-the-option---debugtrack-is-ignored-unless---debug-is-also-specified"></a>MT0032: параметр "--дебугтракк" игнорируется, если не указано также "--Debug".

<a name="MT0033"></a>

### <a name="mt0033-a-xamarinios-project-must-reference-either-monotouchdll-or-xamariniosdll"></a>MT0033: проект Xamarin. iOS должен ссылаться либо на файлы. dll, либо на Xamarin. iOS. dll

<a name="MT0034"></a>

### <a name="mt0034-cannot-include-both-monotouchdll-and-xamariniosdll-in-the-same-xamarinios-project----is-referenced-explicitly-while--is-referenced-by-"></a>MT0034: не может одновременно включать "непрямое касание. dll" и "Xamarin. iOS. dll" в один и тот же проект Xamarin. iOS-" \* ", на который \* ссылается "*".

<!-- MT0035 unused -->

<a name="MT0036"></a>

### <a name="mt0036-cannot-launch-a--simulator-for-a--app-please-enable-the-correct-architectures-in-your-projects-ios-build-options-advanced-page"></a>MT0036: не удается запустить \* симулятор для \* приложения. Включите правильные архитектуры в параметрах сборки iOS проекта (страница "Дополнительно").

<a name="MT0037"></a>

### <a name="mt0037-monotouchdll-is-not-64-bit-compatible-either-reference-xamariniosdll-or-do-not-build-for-a-64-bit-architecture-arm64-andor-x86_64"></a>MT0037: несовместимость. dll не поддерживает 64-разрядные технологии. Либо сослаться на Xamarin. iOS. dll, либо не выполнять сборку для 64-разрядной архитектуры (ARM64 и/или x86_64).

<a name="MT0038"></a>

### <a name="mt0038-the-old-registrars---registraroldstaticolddynamic-are-not-supported-when-referencing-xamariniosdll"></a>MT0038: старые регистраторы (--регистратор: олдстатик | олддинамик) не поддерживаются при обращении к Xamarin. iOS. dll.

<a name="MT0039"></a>

### <a name="mt0039-applications-targeting-armv6-cannot-reference-xamariniosdll"></a>MT0039: приложения, нацеленные на ARMv6, не могут ссылаться на Xamarin. iOS. dll.

<a name="MT0040"></a>

### <a name="mt0040-could-not-find-the-assembly--referenced-by-"></a>MT0040: не удалось найти сборку " \* ", на которую ссылается " \* ".

<a name="MT0041"></a>

### <a name="mt0041-cannot-reference-both-monotouchdll-and-xamariniosdll"></a>MT0041: не может ссылаться на "неtouch. dll" и "Xamarin. iOS. dll".

<a name="MT0042"></a>

### <a name="mt0042-no-reference-to-either-monotouchdll-or-xamariniosdll-was-found-a-reference-to-monotouchdll-will-be-added"></a>MT0042: не найдена ссылка на ни один из этих файлов. dll или Xamarin. iOS. dll. Будет добавлена ссылка на библиотеку. dll.

<a name="MT0043"></a>

### <a name="mt0043-the-boehm-garbage-collector-is-currently-not-supported-when-referencing-xamariniosdll-the-sgen-garbage-collector-has-been-selected-instead"></a>MT0043: сборщик мусора Boehm сейчас не поддерживается при ссылке на "Xamarin. iOS. dll". Вместо этого выбран сборщик мусора SGen.

В Объединенных проектах поддерживается только сборщик мусора SGen. Убедитесь, что отсутствуют дополнительные флаги mtouch, указывающие Boehm в качестве сборщика мусора.

<a name="MT0044"></a>

### <a name="mt0044---listsim-is-only-supported-with-xcode-60-or-later"></a>MT0044:--листсим поддерживается только в Xcode 6,0 и более поздних версиях.

Установите более новую версию Xcode.

<a name="MT0045"></a>

### <a name="mt0045---extension-is-only-supported-when-using-the-ios-80-or-later-sdk"></a>MT0045:--Extension поддерживается только при использовании пакета SDK для iOS 8,0 (или более поздней версии).

<!-- MT0046 is not reported anymore -->

<a name="MT0047"></a>

### <a name="mt0047-the-minimum-deployment-target-for-unified-applications-is-511-the-current-deployment-target-is--please-select-a-newer-deployment-target-in-your-projects-ios-application-options"></a>MT0047: минимальный целевой объект развертывания для Объединенных приложений — 5.1.1, текущий целевой объект развертывания — "*". Выберите более новую цель развертывания в параметрах приложения iOS проекта.

<!-- MT0048 is not reported anymore -->

<a name="MT0049"></a>

### <a name="mt0049-framework-is-supported-only-if-deployment-target-is-80-or-later--features-might-not-work-correctly"></a>MT0049: \* . Framework поддерживается, только если целью развертывания является 8,0 или более поздняя версия. \*функции могут работать неправильно.

Указанная платформа не поддерживается в версии iOS, на которую ссылается цель развертывания. Либо обновите целевой объект развертывания до более новой версии iOS, либо удалите использование указанной платформы из приложения.

<!-- MT0050 is not reported anymore -->

<a name="MT0051"></a>

### <a name="mt0051-xamarinios--requires-xcode-50-or-later-the-current-xcode-version-found-in--is-"></a>MT0051: Xamarin. iOS \* требует Xcode 5,0 или более поздней версии. Текущая версия Xcode (найдена в \* ) — \* .

Установите более новую Xcode.

<a name="MT0052"></a>

### <a name="mt0052-no-command-specified"></a>MT0052: команда не указана.

Не указано действие для mtouch.

<!-- 0053 is used by mmp -->

<a name="MT0054"></a>

### <a name="mt0054-unable-to-canonicalize-the-path--"></a>MT0054: не удалось канонизировать путь " \* ":\*

Это внутренняя ошибка. Если вы видите эту ошибку, создайте новую ошибку на сайте [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT0055"></a>

### <a name="mt0055-the-xcode-path--does-not-exist"></a>MT0055: путь Xcode "*" не существует.

Путь Xcode, переданный с помощью, `--sdkroot` не существует. Укажите допустимый путь.

<a name="MT0056"></a>

### <a name="mt0056-cannot-find-xcode-in-the-default-location-applicationsxcodeapp-please-install-xcode-or-pass-a-custom-path-using---sdkroot-path"></a>MT0056: не удается найти Xcode в расположении по умолчанию (/Applications/Xcode.app). Установите Xcode или передайте пользовательский путь с помощью--sdkroot добавлен \<path> .

<a name="MT0057"></a>

### <a name="mt0057-cannot-determine-the-path-to-xcodeapp-from-the-sdk-root--please-specify-the-full-path-to-the-xcodeapp-bundle"></a>MT0057: не удается определить путь к Xcode. app из корневого пакета SDK "*". Укажите полный путь к пакету Xcode. app.

Путь, переданный с помощью, `--sdkroot` не указывает допустимое приложение Xcode. Укажите путь к приложению Xcode.

<a name="MT0058"></a>

### <a name="mt0058-the-xcodeapp--is-invalid-the-file--does-not-exist"></a>MT0058: недопустимое Xcode. App " \* " (файл " \* " не существует).

Путь, переданный с помощью, `--sdkroot` не указывает допустимое приложение Xcode. Укажите путь к приложению Xcode.

<a name="MT0059"></a>

### <a name="mt0059-could-not-find-the-currently-selected-xcode-on-the-system-"></a>MT0059: не удалось найти текущий выбранный Xcode в системе: *

<a name="MT0060"></a>

### <a name="mt0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned--but-that-directory-does-not-exist"></a>MT0060: не удалось найти текущий выбранный Xcode в системе. "Xcode-SELECT--Print-Path" вернул "*", но этот каталог не существует.

<a name="MT0061"></a>

### <a name="mt0061-no-xcodeapp-specified-using---sdkroot-using-the-system-xcode-as-reported-by-xcode-select---print-path-"></a>MT0061: не указано Xcode. app (с помощью--sdkroot добавлен), используется система Xcode, сообщаемая параметром "Xcode-SELECT--Print-Path": *

Это информационное предупреждение, объясняющее, какой Xcode будет использоваться, так как не было указано ни одного.

<a name="MT0062"></a>

### <a name="mt0062-no-xcodeapp-specified-using---sdkroot-or-xcode-select---print-path-using-the-default-xcode-instead-"></a>MT0062: не указано Xcode. app (с помощью--sdkroot добавлен или "Xcode-SELECT--Print-Path"), вместо него используется значение по умолчанию Xcode: *

Это информационное предупреждение, объясняющее, какой Xcode будет использоваться, так как не было указано ни одного.

<a name="MT0063"></a>

### <a name="mt0063-cannot-find-the-executable-in-the-extension--no-cfbundleexecutable-entry-in-its-infoplist"></a>MT0063: не удается найти исполняемый файл в расширении * (нет записи Кфбундликсекутабле в его info. plist)

Каждый файл info. plist должен иметь исполняемый объект (с помощью записи Кфбундликсекутабле), однако запись во время сборки должна создаваться автоматически.

Обычно это указывает на ошибку в Xamarin. iOS; Запишите новую ошибку в [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) с тестовым случаем.

<a name="MT0064"></a>

### <a name="mt0064-xamarinios-only-supports-embedded-frameworks-with-unified-projects"></a>MT0064: Xamarin. iOS поддерживает только внедренные платформы с едиными проектами.

Xamarin. iOS поддерживает только внедренные платформы только при использовании Unified API; Обновите проект, чтобы использовать Unified API.

<a name="MT0065"></a>

### <a name="mt0065-xamarinios-only-supports-embedded-frameworks-when-deployment-target-is-at-least-80-current-deployment-target--embedded-frameworks-"></a>MT0065: Xamarin. iOS поддерживает только внедренные платформы, если целью развертывания является по крайней мере 8,0 (текущий целевой объект развертывания: \* внедренные платформы: \* )

Xamarin. iOS поддерживает только внедренные платформы, если целью развертывания является по крайней мере 8,0 (поскольку более ранние версии iOS не поддерживают внедренные платформы).

Обновите целевой объект развертывания в файле info. plist проекта на 8,0 или более поздней версии.

<a name="MT0066"></a>

### <a name="mt0066-invalid-build-registrar-assembly-"></a>MT0066: недопустимая сборка регистратора сборки: *

Обычно это указывает на ошибку в Xamarin. iOS; Запишите новую ошибку в [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) с тестовым случаем.

<a name="MT0067"></a>

### <a name="mt0067-invalid-registrar-"></a>MT0067: недопустимый регистратор: *

Обычно это указывает на ошибку в Xamarin. iOS; Запишите новую ошибку в [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) с тестовым случаем.

<a name="MT0068"></a>

### <a name="mt0068-invalid-value-for-target-framework-"></a>MT0068: недопустимое значение для целевой платформы: *.

Недопустимая целевая платформа передана с помощью аргумента--Target-Framework. Укажите допустимую целевую платформу.

<a name="MT0069"></a>

<!--### MT0069: currently unused -->

<a name="MT0070"></a>

### <a name="mt0070-invalid-target-framework--valid-target-frameworks-are-"></a>MT0070: Недопустимая целевая платформа: *. Допустимые целевые платформы: *.

Недопустимая целевая платформа передана с помощью аргумента--Target-Framework. Укажите допустимую целевую платформу.

<a name="MT0071"></a>

### <a name="mt0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinios-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a>MT0071: неизвестная платформа: *. Обычно это указывает на ошибку в Xamarin. iOS; Запишите отчет об ошибке в http://bugzilla.xamarin.com тестовом случае.

Обычно это указывает на ошибку в Xamarin. iOS; Запишите новую ошибку в [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) с тестовым случаем.

<a name="MT0072"></a>

### <a name="mt0072-extensions-are-not-supported-for-the-platform-"></a>MT0072: расширения не поддерживаются для платформы "*".

Обычно это указывает на ошибку в Xamarin. iOS; Запишите новую ошибку в [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) с тестовым случаем.

<a name="MT0073"></a>

### <a name="mt0073-xamarinios--does-not-support-a-deployment-target-of--the-minimum-is--please-select-a-newer-deployment-target-in-your-projects-infoplist"></a>MT0073: Xamarin. iOS \* не поддерживает целевой объект развертывания \* (минимальное значение — \* ). Выберите более новую цель развертывания в файле info. plist проекта.

Минимальный целевой объект развертывания — это значение, указанное в сообщении об ошибке. Выберите более новую цель развертывания в файле info. plist проекта.

Если обновление целевого объекта развертывания невозможно, используйте более старую версию Xamarin. iOS.

<a name="MT0074"></a>

### <a name="mt0074-xamarinios--does-not-support-a-minimum-deployment-target-of--the-maximum-is--please-select-an-older-deployment-target-in-your-projects-infoplist-or-upgrade-to-a-newer-version-of-xamarinios"></a>MT0074: Xamarin. iOS \* не поддерживает минимальный целевой объект развертывания \* (максимум \* ). Выберите более старый целевой объект развертывания в файле info. plist проекта или обновите его до более новой версии Xamarin. iOS.

Xamarin. iOS не поддерживает задание минимальной версии целевого объекта развертывания более высокого уровня, чем версия этой конкретной версии Xamarin. iOS.

Выберите более старый минимальный целевой объект развертывания в файле info. plist проекта или обновите версию Xamarin. iOS до более новой версии.

<a name="MT0075"></a>

### <a name="mt0075-invalid-architecture--for--projects-valid-architectures-are-"></a>MT0075: недопустимая архитектура " \* " для \* проектов. Допустимые архитектуры:\*

Указана недопустимая архитектура. Убедитесь, что архитектура допустима.

<a name="MT0076"></a>

### <a name="mt0076-no-architecture-specified-using-the---abi-argument-an-architecture-is-required-for--projects"></a>MT0076: не указана архитектура (с помощью аргумента--ABI). Для проектов * требуется архитектура.

Обычно это указывает на ошибку в Xamarin. iOS; Запишите новую ошибку в [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) с тестовым случаем.

<a name="MT0077"></a>

### <a name="mt0077-watchos-projects-must-be-extensions"></a>MT0077: проекты WatchOS должны быть расширениями.

Обычно это указывает на ошибку в Xamarin. iOS; Запишите новую ошибку в [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) с тестовым случаем.

<a name="MT0078"></a>

### <a name="mt0078-incremental-builds-are-enabled-with-a-deployment-target--80-currently--this-is-not-supported-the-resulting-application-will-not-launch-on-ios-9-so-the-deployment-target-will-be-set-to-80"></a>MT0078: добавочные сборки включены с целевым объектом развертывания < 8,0 (в настоящее время *). Это не поддерживается (полученное приложение не будет запущено в iOS 9), поэтому для целевого объекта развертывания будет установлено значение 8,0.

Это предупреждение сообщает о том, что для цели развертывания в этой сборке установлено значение 8,0, чтобы последовательные сборки работали правильно.

Добавочные сборки поддерживаются, только если целью развертывания является по крайней мере 8,0 (так как полученное приложение не запустится в iOS 9 в противном случае).

<a name="MT0079"></a>

### <a name="mt0079-the-recommended-xcode-version-for-xamarinios--is-xcode--or-later-the-current-xcode-version-found-in--is-"></a>MT0079: рекомендуемая версия Xcode для Xamarin. iOS \* — Xcode \* или более поздняя. Текущая версия Xcode (найдена в \* ) — \* .

Это предупреждение, информирующее о том, что текущая версия Xcode не является рекомендуемой версией Xcode для этой версии Xamarin. iOS.

Обновите Xcode, чтобы обеспечить оптимальное поведение.

<a name="MT0080"></a>

### <a name="mt0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MT0080: отключение Неврефкаунт,--New-refcount: false является устаревшим.

Это предупреждение, информирующее о том, что запрос на отключение нового refcount (--New-refcount: false) был пропущен.

Новая функция refcount теперь является обязательной для всех проектов, и поэтому ее больше нельзя отключить.

<a name="MT0081"></a>

### <a name="mt0081-the-command-line-argument---download-crash-report-also-requires---download-crash-report-to"></a>MT0081: аргумент командной строки--download-Crash-Report также требует наличия--download-Crash-Report-to.

<a name="MT0082"></a>

### <a name="mt0082-repl---enable-repl-is-only-supported-when-linking-is-not-used---nolink"></a>MT0082: REPL (--Enable-REPL) поддерживается, только если компоновка не используется (--unlink).

<a name="MT0083"></a>

### <a name="mt0083-asm-only-bitcode-is-not-supported-on-watchos-use-either---bitcodemarker-or---bitcodefull"></a>MT0083: только ASM-bitcode не поддерживается в watchOS. Используйте либо--bitcode: маркер, либо--bitcode: Full.

<a name="MT0084"></a>

### <a name="mt0084-bitcode-is-not-supported-in-the-simulator-do-not-pass---bitcode-when-building-for-the-simulator"></a>MT0084: Bitcode не поддерживается в симуляторе. Не передавайте параметр--bitcode при сборке для симулятора.

<a name="MT0085"></a>

### <a name="mt0085-no-reference-to--was-found-it-will-be-added-automatically"></a>MT0085: не найдена ссылка на "*". Он будет добавлен автоматически.

<a name="MT0086"></a>

### <a name="mt0086-a-target-framework---target-framework-must-be-specified-when-building-for-tvos-or-watchos"></a>MT0086: необходимо указать целевую платформу (--Target-Framework) при сборке для TVOS или WatchOS.

Это указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT0087"></a>

### <a name="mt0087-incremental-builds---fastdev-is-not-supported-with-the-boehm-gc-incremental-builds-will-be-disabled"></a>MT0087: добавочные сборки (--Фастдев) не поддерживаются для сборки мусора Boehm. Добавочные сборки будут отключены.

<a name="MT0088"></a>

### <a name="mt0088-the-gc-must-be-in-cooperative-mode-for-watchos-apps-please-remove-the---coopfalse-argument-to-mtouch"></a>MT0088: GC должен находиться в режиме совместного режима для приложений watchOS. Удалите аргумент--Coop: false в mtouch.

<a name="MT0089"></a>

### <a name="mt0089-the-option--cannot-take-the-value--when-cooperative-mode-is-enabled-for-the-gc"></a>MT0089: параметр " \* " не может принимать значение " \* ", если для GC включен режим совместного выполнения.

<a name="MT0091"></a>

### <a name="mt0091-this-version-of-xamarinios-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-set-the-managed-linker-behaviour-to-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MT0091. для этой версии Xamarin. iOS требуется \* пакет SDK (поставляется с Xcode \* ). Обновите Xcode, чтобы получить необходимые файлы заголовков, или настройте поведение управляемого компоновщика, чтобы связать только пакеты SDK инфраструктуры (чтобы попытаться избежать новых API).

Для сборки приложения в Xamarin. iOS требуются файлы заголовков из версии пакета SDK, указанной в сообщении об ошибке. Чтобы устранить эту ошибку, рекомендуется обновить Xcode, чтобы получить необходимый пакет SDK, в который будут включены все необходимые файлы заголовков. Если установлено несколько версий Xcode или вы хотите использовать Xcode в расположении, отличном от расположения по умолчанию, убедитесь, что в настройках IDE задано правильное расположение Xcode.

Потенциальным альтернативным решением является включение управляемого компоновщика. Это приведет к удалению неиспользуемого API, включая, в большинстве случаев, нового API, где файлы заголовков отсутствуют (или неполны). Однако это не будет работать, если в проекте используется API, который был представлен в более новом пакете SDK, чем тот, который предоставляет Xcode.

Последним страв решением будет использование более старой версии Xamarin. iOS, которая поддерживает пакет SDK, требуемый для вашего проекта.

<!-- MT0092 used by mlaunch -->

<a name="MT0093"></a>

### <a name="mt0093-could-not-find-mlaunch"></a>MT0093: не удалось найти "mlaunch".

<!-- MT0094 is not reported anymore -->

<a name="MT0095"></a>

### <a name="mt0095-aot-files-could-not-be-copied-to-the-destination-directory-dest-error"></a>MT0095: не удалось скопировать файлы AOT в конечный каталог {dest}: {Error}

<a name="MT0096"></a>

### <a name="mt0096-no-reference-to-xamariniosdll-was-found"></a>MT0096: не найдена ссылка на Xamarin. iOS. dll.

<!-- MT0097: used by mmp -->
<!-- MT0098: used by mmp -->

<a name="MT0099"></a>

### <a name="mt0099-internal-error--please-file-a-bug-report-with-a-test-case-httpsbugzillaxamarincom"></a>MT0099: Внутренняя ошибка *. Запишите отчет об ошибке с тестовым случаем ( https://bugzilla.xamarin.com) .

Это сообщение об ошибке появляется при сбое внутренней проверки согласованности в Xamarin. iOS.

Обычно это указывает на ошибку в Xamarin. iOS; Запишите новую ошибку в [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) с тестовым случаем.

<a name="MT0100"></a>

### <a name="mt0100-invalid-assembly-build-target--please-file-a-bug-report-with-a-test-case-httpsbugzillaxamarincom"></a>MT0100: недопустимый целевой объект сборки сборки: "*". Запишите отчет об ошибке с тестовым случаем ( https://bugzilla.xamarin.com) .

Это сообщение об ошибке появляется при сбое внутренней проверки согласованности в Xamarin. iOS.

Обычно это указывает на ошибку в Xamarin. iOS; Запишите новую ошибку в [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) с тестовым случаем.

<a name="MT0101"></a>

### <a name="mt0101-the-assembly--is-specified-multiple-times-in---assembly-build-target-arguments"></a>MT0101: сборка "*" указана несколько раз в аргументах--Assembly-Build-Target.

Сборка, упомянутая в сообщении об ошибке, указана несколько раз в аргументах--Assembly-Build-Target. Убедитесь, что каждая сборка упоминается только один раз.

<a name="MT0102"></a>

### <a name="mt0102-the-assemblies--and--have-the-same-target-name--but-different-targets--and-"></a>MT0102: сборки " \* " и " \* " имеют одно и то же целевое имя (" \* "), но разные целевые объекты (" \* " и "*").

Сборки, указанные в сообщении об ошибке, имеют конфликтующие целевые объекты сборки.

Пример.

```
  --assembly-build-target:Assembly1.dll=framework=MyBinary --assembly-build-target:Assembly2.dll=dynamiclibrary=MyBinary
```

В этом примере выполняется попытка создать динамическую библиотеку и платформу с одним и тем же параметром make ( `MyBinary` ).

<a name="MT0103"></a>

### <a name="mt0103-the-static-object--contains-more-than-one-assembly--but-each-static-object-must-correspond-with-exactly-one-assembly"></a>MT0103: статический объект " \* " содержит более одной сборки (" \* "), но каждый статический объект должен соответствовать ровно одной сборке.

Сборки, упомянутые в сообщении об ошибке, компилируются в один статический объект. Это не допускается, каждая сборка должна быть скомпилирована в другой статический объект.

Пример.

```
--assembly-build-target:Assembly1.dll=staticobject=MyBinary --assembly-build-target:Assembly2.dll=staticobject=MyBinary
```

В этом примере предпринимается попытка создать статический объект ( `MyBinary` ), состоящий из двух сборок ( `Assembly1.dll` и `Assembly2.dll` ), что недопустимо.

<a name="MT0105"></a>

### <a name="mt0105-no-assembly-build-target-was-specified-for-"></a>MT0105: для "*" не указан цель сборки сборки.

При указании целевого объекта сборки сборки с помощью `--assembly-build-target` каждая сборка в приложении должна иметь назначенный целевой объект сборки.

Эта ошибка возникает, когда сборке, указанной в сообщении об ошибке, не назначен целевой объект сборки сборки.

Дополнительные сведения см. в документации `--assembly-build-target` .

<a name="MT0106"></a>

### <a name="mt0106-the-assembly-build-target-name--is-invalid-the-character--is-not-allowed"></a>MT0106: недопустимое имя целевого объекта сборки сборки " \* ": символ " \* " не разрешен.

Имя целевого объекта сборки сборки должно быть допустимым именем файла.

Например, следующие значения активируют эту ошибку:

```
--assembly-build-target:Assembly1.dll=staticobject=my/path.o
```

так как `my/path.o` не является допустимым именем файла из-за символа разделителя каталога.

<a name="MT0107"></a>

### <a name="mt0107-the-assemblies--have-different-custom-llvm-optimizations--which-is-not-allowed-when-they-are-all-compiled-to-a-single-binary"></a>MT0107: сборки " \* " имеют разные пользовательские оптимизации LLVM ( \* ), что недопустимо, если все они компилируются в один двоичный файл.

<a name="MT0108"></a>

### <a name="mt0108-the-assembly-build-target--did-not-match-any-assemblies"></a>MT0108: целевой объект сборки сборки "*" не соответствует ни одной сборке.

<a name="MT0109"></a>

### <a name="mt0109-the-assembly-0-was-loaded-from-a-different-path-than-the-provided-path-provided-path-1-actual-path-2"></a>MT0109: сборка " {0} " была загружена из пути, отличного от указанного пути (указанный путь: {1} , фактический путь: {2} ).

Это предупреждение, указывающее, что сборка, на которую ссылается приложение, загружена из расположения, отличного от запрошенного.

Это может означать, что приложение ссылается на несколько сборок с одним и тем же именем, но из разных расположений, которые могут привести к непредвиденным результатам (будет использоваться только первая сборка).

<a name="MT0110"></a>

### <a name="mt0110-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-third-party-binding-libraries-and-that-compiles-to-bitcode"></a>MT0110: добавочные сборки были отключены, так как эта версия Xamarin. iOS не поддерживает добавочные сборки в проектах, содержащих сторонние библиотеки привязки и компилируемые в bitcode.

Добавочные сборки были отключены, так как эта версия Xamarin. iOS не поддерживает добавочные сборки в проектах, которые включают сторонние библиотеки привязки и компилируются в bitcode (проекты tvOS и watchOS).

Для вашего компонента никаких действий не требуется, это сообщение является исключительно информационным.

Дополнительные сведения см. в описании ошибки No[51710](https://bugzilla.xamarin.com/show_bug.cgi?id=51710).

Это предупреждение больше не отображается.

<a name="MT0111"></a>

### <a name="mt0111-bitcode-has-been-enabled-because-this-version-of-xamarinios-does-not-support-building-watchos-projects-using-llvm-without-enabling-bitcode"></a>MT0111: Bitcode включен, так как эта версия Xamarin. iOS не поддерживает сборку проектов watchOS с помощью LLVM без включения Bitcode.

Bitcode был включен автоматически, так как эта версия Xamarin. iOS не поддерживает сборку проектов watchOS с помощью LLVM без включения Bitcode.

Для вашего компонента никаких действий не требуется, это сообщение является исключительно информационным.

Дополнительные сведения см. в описании ошибки No[51634](https://bugzilla.xamarin.com/show_bug.cgi?id=51634).

<a name="MT0112"></a>

### <a name="mt0112-native-code-sharing-has-been-disabled-because-"></a>MT0112: общий доступ к машинному коду был отключен, так как *

Существует несколько причин, по которым можно отключить общий доступ к коду.

- так как целевое приложение-контейнер развертывания имеет более раннюю версию, чем iOS 8,0 (это *)).

Для общего доступа к машинному коду требуется iOS 8,0, так как совместное использование машинного кода реализуется с помощью пользовательских платформ, которые появились в iOS 8,0.

- так как приложение-контейнер включает сборки I18N (*).

Общий доступ к машинному коду в настоящее время не поддерживается, если приложение-контейнер включает сборки I18N.

- так как приложение-контейнер содержит пользовательские определения XML для управляемого компоновщика (*).

Для общего доступа к машинному коду требуется не поддерживается для проектов, использующих пользовательские XML-определения для управляемого компоновщика.

<a name="MT0113"></a>

### <a name="mt0113-native-code-sharing-has-been-disabled-for-the-extension--because-"></a>MT0113: общий доступ к машинному коду отключен для расширения "*", так как *.

- так как параметры bitcode отличаются между приложением-контейнером ( \* ) и расширением ( \* ).

  Для общего доступа к машинному коду необходимо, чтобы параметры bitcode совпадали между проектами, которые совместно используют код.

- так как параметры--Assembly-Build-Target различаются в приложении контейнера ( \* ) и расширении ( \* ).

  Для общего доступа к машинному коду требуется, чтобы параметры--Assembly-Build-Target совпадали между проектами, которые совместно используют код.

  Это состояние может произойти, если добавочные сборки не включены или отключены во всех проектах.

- так как сборки I18N различаются между приложением-контейнером ( \* ) и расширением ( \* ).

  Совместное использование машинного кода в настоящее время не поддерживается для расширений, включающих сборки I18N.

- так как аргументы для компилятора AOT отличаются между собой в приложении-контейнере ( \* ) и расширении ( \* ).

  Для общего доступа к машинному коду требуется, чтобы аргументы в компиляторе AOT не отличались друг от друга между проектами, которые совместно используют код.

- так как другие аргументы компилятора AOT отличаются между собой в приложении-контейнере ( \* ) и расширении ( \* ).

  Для общего доступа к машинному коду требуется, чтобы аргументы в компиляторе AOT не отличались друг от друга между проектами, которые совместно используют код.

  Это состояние возникает, если в проектах разное число операций с плавающей запятой "выполнять все 32-разрядные операции с плавающей запятой как 64-bit float".

- так как LLVM не включен и не отключен в приложении-контейнере ( \* ) и расширении ( \* ).

  Для общего доступа к машинному коду требуется, чтобы LLVM был включен или отключен для всех проектов, использующих общий код.

- так как параметры управляемого компоновщика между приложением-контейнером ( \* ) и расширением ( \* ) различаются.

  Для общего доступа к машинному коду необходимо, чтобы параметры управляемого компоновщика совпадали для всех проектов, использующих общий код.

- так как пропущенные сборки для управляемого компоновщика отличаются между приложением-контейнером ( \* ) и расширением ( \* ).

  Для общего доступа к машинному коду необходимо, чтобы параметры управляемого компоновщика совпадали для всех проектов, использующих общий код.

- так как расширение содержит пользовательские XML-определения для управляемого компоновщика (*).

  Для общего доступа к машинному коду требуется не поддерживается для проектов, использующих пользовательские XML-определения для управляемого компоновщика.

- так как приложение-контейнер не создает интерфейс ABI * (во время создания расширения для этого ABI).

  Для общего доступа к машинному коду требуется, чтобы приложение контейнера собыло построено для всех архитектур всех сборок расширения приложения.

  Например: это состояние возникает при сборке расширения для ARM64 + ARMv7, но приложение контейнера строится только для ARM64.

- так как приложение-контейнер создается для ABI \* , что несовместимо с ABI ( \* ) расширения.

  Для общего доступа к машинному коду необходимо, чтобы все проекты были построены на точно такой же API.

  Например: это состояние возникает при сборке расширения для ARMv7 + LLVM + thumb2, но приложение контейнера строится только для ARMv7 + LLVM.

- так как приложение-контейнер ссылается на сборку " \* " из " \* ", а расширение ссылается на другую версию из "*".

  Для общего доступа к машинному коду требуется, чтобы все проекты, использующие код, использовали одни и те же версии для всех сборок.

<!-- MT0114: used by mmp -->

<a name="MT0115"></a>

### <a name="mt0115-it-is-recommended-to-reference-dynamic-symbols-using-code---dynamic-symbol-modecode-when-bitcode-is-enabled"></a>MT0115: рекомендуется ссылаться на динамические символы с помощью кода (--Dynamic-Symbol-Mode = Code), если bitcode включен.

Проекты Xamarin. iOS часто ссылаются на собственные символы динамически, что означает, что собственный Компоновщик может удалить такие собственные символы во время процесса компоновки, поскольку собственный компоновщик не увидит, что эти символы используются.

Как правило, Xamarin. iOS запрашивает у собственного компоновщика возможность сохранения таких символов (с помощью `-u symbol` флага компоновщика), но при компиляции для bitcode собственный компоновщик не принимает `-u` флаг.

Xamarin. iOS реализовал альтернативное решение: мы создаем дополнительный машинный код, который ссылается на эти символы, и, таким образом, собственный компоновщик увидит, что эти символы используются. Это происходит автоматически при компиляции в bitcode.

Если `--dynamic-symbol-mode=linker` передается в mtouch, это альтернативное решение будет отключено, а Xamarin. iOS попытается передать `-u` его в собственный компоновщик. Скорее всего, это приведет к ошибкам в собственном компоновщике.

Решением является удаление `--dynamic-symbol-mode=linker` аргумента из дополнительных аргументов mtouch в параметрах сборки проекта.

<!-- 0116 - 0124: free to use -->

<a name="MT0116"></a>

### <a name="mt0116-invalid-architecture-arch-32-bit-architectures-are-not-supported-when-deployment-target-is-11-or-later-make-sure-the-project-does-not-build-for-a-32-bit-architecture"></a>MT0116: недопустимая архитектура: {Arch}. 32-разрядные архитектуры не поддерживаются, если целью развертывания является 11 или более поздняя версия. Убедитесь, что проект не построен для 32-разрядной архитектуры.

в iOS 11 не включена поддержка 32-разрядных приложений, поэтому сборка для 32-разрядного приложения не поддерживается, если целью развертывания является iOS 11 или более поздней версии.

Либо измените целевую архитектуру в параметрах сборки iOS проекта на arm64, либо измените целевой объект развертывания в файле info. plist проекта на более раннюю версию iOS.

<a name="MT0117"></a>

### <a name="mt0117-cant-launch-a-32-bit-app-on-a-simulator-that-only-supports-64-bit"></a>MT0117: не удается запустить 32-разрядное приложение в симуляторе, поддерживающем только 64-разрядную версию.

<a name="MT0118"></a>

### <a name="mt0118-aot-files-could-not-be-found-at-the-expected-directory-msymdir"></a>MT0118: не удалось найти файлы AOT в ожидаемом каталоге "{мсимдир}".

<!-- 0119 - 0123: free to use -->

<a name="MT0123"></a>

### <a name="mt0123-the-executable-assembly--does-not-reference-"></a>MT0123: исполняемая сборка \* не ссылается на \* .

Не удалось найти ссылку на сборку платформы (Xamarin. iOS. dll/Xamarin. TVOS. dll/Xamarin. WatchOS. dll) в исполняемой сборке.

Обычно это происходит, когда в исполняемом проекте нет кода, использующего что-либо из сборки платформы; Например, пустой метод Main (без другого кода) отобразит следующую ошибку:

```csharp
class Program {
    void Main (string[] args)
    {
    }
}
```

При использовании API из сборки платформы будет устранена ошибка:

```csharp
class Program {
    void Main (string[] args)
    {
        System.Console.WriteLine (typeof (UIKit.UIWindow));
    }
}
```

<a name="MT0124"></a>

### <a name="mt0124-could-not-set-the-current-language-to-lang-according-to-langlang-exception"></a>MT0124: не удалось задать для текущего языка значение "{lang}" (в соответствии со значением LANG = {LANG}): {Exception}

Это предупреждение, указывающее на то, что текущему языку не удалось задать язык в сообщении об ошибке.

Текущий язык будет по умолчанию языком системы.

<a name="MT0125"></a>

### <a name="mt0125-the---assembly-build-target-command-line-argument-is-ignored-in-the-simulator"></a>MT0125: аргумент командной строки--Assembly-Build-Target не учитывается в симуляторе.

Никаких действий не требуется, это сообщение является исключительно информационным.

<a name="MT0126"></a>

### <a name="mt0126-incremental-builds-have-been-disabled-because-incremental-builds-are-not-supported-in-the-simulator"></a>MT0126: добавочные сборки были отключены, так как в симуляторе не поддерживаются добавочные сборки.

Никаких действий не требуется, это сообщение является исключительно информационным.

<a name="MT0127"></a>

### <a name="mt0127-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-more-than-one-third-party-binding-libraries"></a>MT0127: добавочные сборки были отключены, так как эта версия Xamarin. iOS не поддерживает добавочные сборки в проектах, включающих более одной библиотеки привязки сторонних разработчиков.

Добавочные сборки были отключены автоматически, так как эта версия Xamarin. iOS не всегда правильно строится проекты с несколькими библиотеками привязки сторонних разработчиков.

Никаких действий не требуется, это сообщение является исключительно информационным.

Дополнительные сведения см. в описании ошибки No[52727](https://bugzilla.xamarin.com/show_bug.cgi?id=52727).

<a name="MT0128"></a>

### <a name="mt0128-could-not-touch-the-file--"></a>MT0128: не удалось коснуться файла " \* ":\*

Произошла ошибка при касании файла (что сделано для обеспечения правильной работы частичных сборок).

Это предупреждение может быть, скорее всего, игнорироваться. в случае возникновения каких-либо проблем в файле [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) возникла новая проблема, которая будет исследована.

<a name="MT0135"></a>

### <a name="mt0135-did-not-link-system-framework-0-referenced-by-assembly-1-because-it-was-introduced-in-2-3-and-were-using-the-2-4-sdk"></a>MT0135: не удалось связать системную платформу " {0} " (на которую ссылается сборка " {1} "), так как она была введена в {2} {3} , и мы используем {2} {4} пакет SDK.

Для сборки приложения Xamarin. iOS должен ссылаться на системные библиотеки, некоторые из которых зависят от версии пакета SDK, указанной в сообщении об ошибке. Поскольку используется более старая версия пакета SDK, вызовы этих API могут завершиться ошибкой во время выполнения.

Чтобы устранить эту ошибку, рекомендуется обновить Xcode, чтобы получить необходимый пакет SDK. Если у вас установлено несколько версий Xcode или вы хотите использовать Xcode в расположении, отличном от расположения по умолчанию, убедитесь, что в настройках IDE задано правильное расположение Xcode.

Кроме того, можно разрешить управляемому [компоновщику](https://docs.microsoft.com/xamarin/ios/deploy-test/linker) удалять неиспользуемые API, в том числе (в большинстве случаев) новые, для которых требуется Указанная библиотека. Однако это не будет работать, если для проекта требуются API, появившиеся в более новом пакете SDK, чем тот, который предоставляет Xcode.

В качестве последнего страв решения используйте более раннюю версию Xamarin. iOS, которая не требует наличия этих новых пакетов SDK в процессе сборки.

## <a name="mt1xxx-project-related-error-messages"></a>MT1xxx: сообщения об ошибках, связанные с проектом

### <a name="mt10xx-installer--mtouch"></a>MT10xx: установщик/mtouch

<!--
 MT1xxx file copy / symlinks (project related)
  MT10xx installer.cs / mtouch.cs
  -->

<a name="MT1001"></a>

### <a name="mt1001-could-not-find-an-application-at-the-specified-directory"></a>MT1001: не удалось найти приложение в указанном каталоге

<a name="MT1002"></a>

### <a name="mt1002-could-not-create-symlinks-files-were-copied"></a>MT1002: не удалось создать символических ссылок, файлы были скопированы

<a name="MT1003"></a>

### <a name="mt1003-could-not-kill-the-application--you-may-have-to-kill-the-application-manually"></a>MT1003: не удалось завершить работу приложения "*". Может потребоваться завершить работу приложения вручную.

<a name="MT1004"></a>

### <a name="mt1004-could-not-get-the-list-of-installed-applications"></a>MT1004: не удалось получить список установленных приложений.

## <a name="mt1xxx-project-related-error-messages"></a>MT1xxx: сообщения об ошибках, связанные с проектом

<a name="MT1005"></a>

### <a name="mt1005-could-not-kill-the-application--on-the-device----you-may-have-to-kill-the-application-manually"></a>MT1005: не удалось завершить работу приложения " \* " на устройстве " \* ": *-возможно, потребуется завершить работу приложения вручную.

<a name="MT1006"></a>

### <a name="mt1006-could-not-install-the-application--on-the-device--"></a>MT1006: не удалось установить приложение " \* " на устройстве " \* ": *.

<a name="MT1007"></a>

### <a name="mt1007-failed-to-launch-the-application--on-the-device---you-can-still-launch-the-application-manually-by-tapping-on-it"></a>MT1007: не удалось запустить приложение "" \* на устройстве " \* ": *. Вы по-прежнему можете запустить приложение вручную, коснувшись его.

<a name="MT1008"></a>

### <a name="mt1008-failed-to-launch-the-simulator"></a>MT1008: не удалось запустить симулятор

Эта ошибка появляется, если mtouch не удалось запустить симулятор.   Иногда это может произойти из-за того, что уже запущен устаревший или неработающий симулятор.

Следующая команда, выданная в командной строке UNIX, может использоваться для уничтожения процессов с зависанием симулятора:

```bash
$ launchctl list|grep UIKitApplication|awk '{print $3}'|xargs launchctl remove
```

<a name="MT1009"></a>

### <a name="mt1009-could-not-copy-the-assembly--to--"></a>MT1009: не удалось скопировать сборку " \* " в " \* ": *

Это известная проблема в некоторых версиях Xamarin. iOS.

В этом случае попробуйте следующее решение:

```bash
sudo chmod 0644 /Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/*/*.mdb
```

Однако поскольку эта проблема решена в последней версии Xamarin. iOS, создайте новую проблему на сайте [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) , указав полную информацию о версии и выходные данные журнала сборки.

<a name="MT1010"></a>

### <a name="mt1010-could-not-load-the-assembly--"></a>MT1010: не удалось загрузить сборку " \* ":\*

<a name="MT1011"></a>

### <a name="mt1011-could-not-add-missing-resource-file-"></a>MT1011: не удалось добавить отсутствующий файл ресурсов: "*"

<a name="MT1012"></a>

### <a name="mt1012-failed-to-list-the-apps-on-the-device--"></a>MT1012: не удалось перечислить приложения на устройстве " \* ":\*

<a name="MT1013"></a>

### <a name="mt1013-dependency-tracking-error-no-files-to-compare-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a>MT1013: ошибка отслеживания зависимостей: нет файлов для сравнения. Запишите отчет об ошибке в http://bugzilla.xamarin.com тестовом случае.

Это указывает на ошибку в Xamarin. iOS. Запишите новую ошибку в [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) с тестовым случаем.

<a name="MT1014"></a>

### <a name="mt1014-failed-to-re-use-cached-version-of--"></a>MT1014: не удалось повторно использовать кэшированную версию "*": *.

<a name="MT1015"></a>

### <a name="mt1015-failed-to-create-the-executable--"></a>MT1015: не удалось создать исполняемый файл " \* ":\*

<a name="MT1015"></a>

### <a name="mt1015-failed-to-create-the-executable--"></a>MT1015: не удалось создать исполняемый файл " \* ":\*

<a name="MT1016"></a>

### <a name="mt1016-failed-to-create-the-notice-file-because-a-directory-already-exists-with-the-same-name"></a>MT1016: не удалось создать файл уведомления, так как каталог с таким именем уже существует.

Удалите каталог `NOTICE` из проекта.

<a name="MT1017"></a>

### <a name="mt1017-failed-to-create-the-notice-file-"></a>MT1017: не удалось создать файл уведомления: *.

<a name="MT1018"></a>

### <a name="mt1018-your-application-failed-code-signing-checks-and-could-not-be-installed-on-the-device--check-your-certificates-provisioning-profiles-and-bundle-ids-probably-your-device-is-not-part-of-the-selected-provisioning-profile-error-0xe8008015"></a>MT1018: приложение не прошло проверку подписи кода и не может быть установлено на устройстве "*". Проверьте сертификаты, профили подготовки и идентификаторы пакетов. Возможно, устройство не входит в выбранный профиль подготовки (ошибка: 0xe8008015).

<a name="MT1019"></a>

### <a name="mt1019-your-application-has-entitlements-not-supported-by-your-current-provisioning-profile-and-could-not-be-installed-on-the-device--please-check-the-ios-device-log-for-more-detailed-information-error-0xe8008016"></a>MT1019: у вашего приложения есть права, которые не поддерживаются текущим профилем подготовки и не могут быть установлены на устройстве "*". Дополнительные сведения см. в журнале устройств iOS (ошибка: 0xe8008016).

Причины могут быть следующими:

- У вашего приложения есть права, которые не поддерживаются текущим профилем подготовки.
  Возможные решения
  - Укажите другой профиль подготовки, который поддерживает права, необходимые приложению.
  - Удалите права, не поддерживаемые в текущем профиле подготовки.
- Устройство, на которое вы пытаетесь выполнить развертывание, не включено в профиль подготовки, который вы используете.
  Возможные решения
  - Создание нового приложения на основе шаблона в Xcode, выбор того же профиля подготовки и развертывание на том же устройстве. Иногда Xcode может автоматически обновлять профили подготовки с помощью новых устройств (в других случаях Xcode будет спрашивать, что делать).
  — Перейдите в центр разработки iOS и обновите профиль подготовки с помощью нового устройства, а затем Скачайте обновленный профиль подготовки на компьютер.

В большинстве случаев дополнительные сведения о сбое будут выводиться в журнал устройств iOS, что поможет диагностировать проблему.

<a name="MT1020"></a>

### <a name="mt1020-failed-to-launch-the-application--on-the-device--"></a>MT1020: не удалось запустить приложение "" \* на устройстве " \* ": *

<a name="MT1021"></a>

### <a name="mt1021-could-not-copy-the-file--to--2"></a>MT1021: не удалось скопировать файл " \* " в " \* ":{2}

Не удалось скопировать файл. Сообщение об ошибке из операции копирования содержит дополнительные сведения об ошибке.

<a name="MT1022"></a>

### <a name="mt1022-could-not-copy-the-directory--to--2"></a>MT1022: не удалось скопировать каталог " \* " в " \* ":{2}

Не удалось скопировать каталог. Сообщение об ошибке из операции копирования содержит дополнительные сведения об ошибке.

<a name="MT1023"></a>

### <a name="mt1023-could-not-communicate-with-the-device-to-find-the-application---"></a>MT1023: не удалось связаться с устройством для поиска приложения " \* ":\*

Произошла ошибка при попытке найти приложение на устройстве.

Для решения этой проблемы выполните следующие действия.

- Удалите приложение с устройства и повторите попытку.
- Отключите устройство и подключите его повторно.
- Перезапустите устройство.
- Перезагрузите компьютер Mac.

<a name="MT1024"></a>

### <a name="mt1024-the-application-signature-could-not-be-verified-on-device--please-make-sure-that-the-provisioning-profile-is-installed-and-not-expired-error-0xe8008017"></a>MT1024: не удалось проверить подпись приложения на устройстве "*". Убедитесь, что профиль подготовки установлен и не просрочен (ошибка: 0xe8008017).

Устройство отклонило установку приложения, так как не удалось проверить подпись.

Убедитесь, что профиль подготовки установлен и не просрочен.

<a name="MT1025"></a>

### <a name="mt1025-could-not-list-the-crash-reports-on-the-device-"></a>MT1025: не удалось перечислить отчеты о сбоях на устройстве *.

Произошла ошибка при попытке перечислить отчеты о сбоях на устройстве.

Для решения этой проблемы выполните следующие действия.

- Удалите приложение с устройства и повторите попытку.
- Отключите устройство и подключите его повторно.
- Перезапустите устройство.
- Перезагрузите компьютер Mac.
- Синхронизируйте устройство с iTunes (при этом все отчеты о сбоях будут удалены с устройства).

<a name="MT1026"></a>

### <a name="mt1026-could-not-download-the-crash-report--from-the-device-"></a>MT1026: не удалось скачать отчет о сбоях \* с устройства \* .

Произошла ошибка при попытке загрузить отчеты о сбоях с устройства.

Для решения этой проблемы выполните следующие действия.

- Удалите приложение с устройства и повторите попытку.
- Отключите устройство и подключите его повторно.
- Перезапустите устройство.
- Перезагрузите компьютер Mac.
- Синхронизируйте устройство с iTunes (при этом все отчеты о сбоях будут удалены с устройства).

<a name="MT1027"></a>

### <a name="mt1027-cant-use-xcode-7-to-launch-applications-on-devices-with-ios--xcode-7-only-supports-ios-6"></a>MT1027: не удается использовать Xcode 7 + для запуска приложений на устройствах с iOS * (Xcode 7 поддерживает только iOS 6 +).

Xcode 7 + нельзя использовать для запуска приложений на устройствах с версией iOS ниже 6,0.

Используйте более раннюю версию Xcode или коснитесь приложения вручную, чтобы запустить его.

<a name="MT1028"></a>

### <a name="mt1028-invalid-device-specification--expected-ios-watchos-or-all"></a>MT1028: Недопустимая спецификация устройства: "*". Ожидалось "iOS", "watchos" или "ALL".

Спецификация устройства, переданная с помощью--Device, недопустима. Допустимые значения: "iOS", "watchos" или "ALL".

<a name="MT1029"></a>

### <a name="mt1029-could-not-find-an-application-at-the-specified-directory-"></a>MT1029: не удалось найти приложение в указанном каталоге: *

Путь приложения, переданный в параметре--лаунчдев, не существует. Укажите допустимый набор приложений.

<a name="MT1030"></a>

### <a name="mt1030-launching-applications-on-device-using-a-bundle-identifier-is-deprecated-please-pass-the-full-path-to-the-bundle-to-launch"></a>MT1030: запуск приложений на устройстве с помощью идентификатора пакета является устаревшим. Передайте полный путь к пакету для запуска.

Рекомендуется передать в приложение путь для запуска на устройстве вместо просто идентификатора пакета.

<a name="MT1031"></a>

### <a name="mt1031-could-not-launch-the-app--on-the-device--because-the-device-is-locked-please-unlock-the-device-and-try-again"></a>MT1031: не удалось запустить приложение " \* " на устройстве "", \* так как устройство заблокировано. Разблокируйте устройство и повторите попытку.

Разблокируйте устройство и повторите попытку.

<a name="MT1032"></a>

### <a name="mt1032-this-application-executable-might-be-too-large--mb-to-execute-on-device-if-bitcode-was-enabled-you-might-want-to-disable-it-for-development-it-is-only-required-to-submit-applications-to-apple"></a>MT1032: этот исполняемый файл приложения может быть слишком большим (* МБ) для выполнения на устройстве. Если bitcode был включен, вам может потребоваться отключить его для разработки, а только для отправки приложений в Apple.

<a name="MT1033"></a>

### <a name="mt1033-could-not-uninstall-the-application--from-the-device--"></a>MT1033: не удалось удалить приложение " \* " с устройства " \* ": *

<!-- 1034 used by mmp -->

<a name="MT1035"></a>

### <a name="mt1035-cannot-include-different-versions-of-the-framework-name"></a>MT1035: не может включать разные версии платформы "{Name}"

Невозможно связать с разными версиями одной и той же платформы.

Обычно это происходит, когда расширение ссылается на версию платформы, отличную от версии приложения-контейнера (возможно, через сборку привязки стороннего разработчика).

После этой ошибки будут отображаться несколько ошибок [MT1036](#MT1036) , в которых перечислены пути для каждой из разных платформ.

<a name="MT1036"></a>

### <a name="mt1036-framework-name-included-from-path-related-to-previous-error"></a>MT1036: платформа "{Name}" включена из: {Path} (связана с предыдущей ошибкой)

Эта ошибка сообщается только вместе с [MT1036](#MT1036). Дополнительные сведения см. в разделе [MT1036](#MT1036) .

### <a name="mt11xx-debug-service"></a>MT11xx: служба отладки

<!--
  MT11xx DebugService.cs
  -->

<a name="MT1101"></a>

### <a name="mt1101-could-not-start-app"></a>MT1101: не удалось запустить приложение

<a name="MT1102"></a>

### <a name="mt1102-could-not-attach-to-the-app-to-kill-it-"></a>MT1102: не удалось присоединиться к приложению (для его уничтожения): *

<a name="MT1103"></a>

### <a name="mt1103-could-not-detach"></a>MT1103: не удалось отсоединить

<a name="MT1104"></a>

### <a name="mt1104-failed-to-send-packet-"></a>MT1104: не удалось отправить пакет: *

<a name="MT1105"></a>

### <a name="mt1105-unexpected-response-type"></a>MT1105: непредвиденный тип ответа

<a name="MT1106"></a>

### <a name="mt1106-could-not-get-list-of-applications-on-the-device-request-timed-out"></a>MT1106: не удалось получить список приложений на устройстве: истекло время ожидания запроса.

<a name="MT1107"></a>

### <a name="mt1107-application-failed-to-launch-"></a>MT1107: не удалось запустить приложение: *

Убедитесь, что устройство заблокировано.

Если вы развертываете корпоративное приложение или используете бесплатный профиль подготовки, возможно, вы доверяете разработчику (это объясняется <a href="https://stackoverflow.com/a/30726375/183422">здесь</a>).

<a name="MT1108"></a>

### <a name="mt1108-could-not-find-developer-tools-for-this-xx-yy-device"></a>MT1108: не удалось найти средства разработчика для этого устройства XX (гг).

Для выполнения нескольких операций из mtouch требуется `DeveloperDiskImage.dmg` наличие файла.   Этот файл является частью Xcode и обычно находится относительно пакета SDK, который используется для сборки в `Xcode.app/Contents/Developer/iPhoneOS.platform/DeviceSupport/VERSION/DeveloperDiskImage.dmg` .

Эта ошибка может возникать из-за отсутствия Девелопердискимаже. DMG, соответствующего подключенному устройству.

<a name="MT1109"></a>

### <a name="mt1109-application-failed-to-launch-because-the-device-is-locked-please-unlock-the-device-and-try-again"></a>MT1109: не удалось запустить приложение, так как устройство заблокировано. Разблокируйте устройство и повторите попытку.

Убедитесь, что устройство заблокировано.

<a name="MT1110"></a>

### <a name="mt1110-application-failed-to-launch-because-of-ios-security-restrictions-please-ensure-the-developer-is-trusted"></a>MT1110: не удалось запустить приложение из-за ограничений безопасности iOS. Убедитесь, что разработчик является доверенным.

Если вы развертываете корпоративное приложение или используете бесплатный профиль подготовки, возможно, вы доверяете разработчику (это объясняется <a href="https://stackoverflow.com/a/30726375/183422">здесь</a>).

<a name="MT1111"></a>

### <a name="mt1111-application-launched-successfully-but-its-not-possible-to-wait-for-the-app-to-exit-as-requested-because-its-not-possible-to-detect-app-termination-when-launching-using-gdbserver"></a>MT1111: приложение успешно запущено, но невозможно дождаться завершения работы приложения по запросу, так как невозможно обнаружить завершение приложения при запуске с помощью gdbserver.

### <a name="mt12xx-simulator"></a>MT12xx: симулятор

<!--
  MT12xx simcontroller.cs
  -->

<a name="MT1201"></a>

### <a name="mt1201-could-not-load-the-simulator-"></a>MT1201: не удалось загрузить симулятор: *

<a name="MT1202"></a>

### <a name="mt1202-invalid-simulator-configuration-"></a>MT1202: Недопустимая конфигурация симулятора: *

<a name="MT1203"></a>

### <a name="mt1203-invalid-simulator-specification-"></a>MT1203: Недопустимая спецификация симулятора: *

<a name="MT1204"></a>

### <a name="mt1204-invalid-simulator-specification--runtime-not-specified"></a>MT1204: Недопустимая спецификация симулятора "*": среда выполнения не указана.

<a name="MT1205"></a>

### <a name="mt1205-invalid-simulator-specification--device-type-not-specified"></a>MT1205: Недопустимая спецификация симулятора "*": тип устройства не указан.

<a name="MT1206"></a>

### <a name="mt1206-could-not-find-the-simulator-runtime-"></a>MT1206: не удалось найти среду выполнения симулятора "*".

<a name="MT1207"></a>

### <a name="mt1207-could-not-find-the-simulator-device-type-"></a>MT1207: не удалось найти тип устройства симулятора "*".

<a name="MT1208"></a>

### <a name="mt1208-could-not-find-the-simulator-runtime-"></a>MT1208: не удалось найти среду выполнения симулятора "*".

<a name="MT1209"></a>

### <a name="mt1209-could-not-find-the-simulator-device-type-"></a>MT1209: не удалось найти тип устройства симулятора "*".

<a name="MT1210"></a>

### <a name="mt1210-invalid-simulator-specification--unknown-key-"></a>MT1210: Недопустимая спецификация имитатора: \* , неизвестный ключ " \* "

<a name="MT1211"></a>

### <a name="mt1211-the-simulator-version--does-not-support-the-simulator-type-"></a>MT1211: версия имитатора " \* " не поддерживает тип симулятора " \* "

<a name="MT1212"></a>

### <a name="mt1212-failed-to-create-a-simulator-version-where-type---and-runtime--"></a>MT1212: не удалось создать версию симулятора, где Type = \* и Runtime = \* .

<a name="MT1213"></a>

### <a name="mt1213-invalid-simulator-specification-for-xcode-4-"></a>MT1213: Недопустимая спецификация симулятора для Xcode 4: *

<a name="MT1214"></a>

### <a name="mt1214-invalid-simulator-specification-for-xcode-5-"></a>MT1214: Недопустимая спецификация симулятора для Xcode 5: *

<a name="MT1215"></a>

### <a name="mt1215-invalid-sdk-specified-"></a>MT1215: указан недопустимый пакет SDK: *

<a name="MT1216"></a>

### <a name="mt1216-could-not-find-the-simulator-udid-"></a>MT1216: не удалось найти симулятор UDID "*".

<a name="MT1217"></a>

### <a name="mt1217-could-not-load-the-app-bundle-at-"></a>MT1217: не удалось загрузить набор приложений в "*".

<a name="MT1218"></a>

### <a name="mt1218-no-bundle-identifier-found-in-the-app-at-"></a>MT1218: в приложении в "*" не найден идентификатор пакета.

<a name="MT1219"></a>

### <a name="mt1219-could-not-find-the-simulator-for-"></a>MT1219: не удалось найти симулятор для "*".

<a name="MT1220"></a>

### <a name="mt1220-could-not-find-the-latest-simulator-runtime-for-device-"></a>MT1220: не удалось найти последнюю версию среды выполнения имитатора для устройства "*".

Обычно это свидетельствует о проблеме с Xcode.

Действия по устранению этой ошибки:

- Используйте симулятор один раз в Xcode.
- Передайте явную версию пакета SDK с помощью--SDK \<version> .
- Переустановите Xcode.

<a name="MT1221"></a>

### <a name="mt1221-could-not-find-the-paired-iphone-simulator-for-the-watchos-simulator-"></a>MT1221: не удалось найти Парный симулятор iPhone для симулятора WatchOS "*".

При запуске приложения WatchOS в симуляторе WatchOS также должен быть парный симулятор iOS.

Контрольные симуляторы можно связать с симуляторами iOS с помощью пользовательского интерфейса устройств Xcode (окно меню — > устройств).

### <a name="mt13xx-linkwith"></a>MT13xx: [Линквис]

<!--
  MT13xx [LinkWith]
  -->

<a name="MT1301"></a>

### <a name="mt1301-native-library---was-ignored-since-it-does-not-match-the-current-build-architectures-"></a>MT1301: собственная библиотека `*` ( \* ) была пропущена, так как она не соответствует текущим архитектурам сборки ( \* )

<a name="MT1302"></a>

### <a name="mt1302-could-not-extract-the-native-library--from--please-ensure-the-native-library-was-properly-embedded-in-the-managed-assembly-if-the-assembly-was-built-using-a-binding-project-the-native-library-must-be-included-in-the-project-and-its-build-action-must-be-objcbindingnativelibrary"></a>MT1302: не удалось извлечь собственную библиотеку "*" из "+". Убедитесь, что собственная библиотека была правильно внедрена в управляемую сборку (если сборка была построена с помощью проекта привязки, собственная библиотека должна быть включена в проект, а действие сборки должно быть "Обжкбиндингнативелибрари").

<a name="MT1303"></a>

### <a name="mt1303-could-not-decompress-the-native-framework--from--please-review-the-build-log-for-more-information-from-the-native-zip-command"></a>MT1303: не удалось распаковать собственную платформу " \* " из " \* ". Дополнительные сведения о собственной команде "ZIP" см. в журнале сборки.

Не удалось распаковать указанную собственную платформу из библиотеки привязки.

Дополнительные сведения об этом сбое из собственной команды "ZIP" см. в журнале булид.

<a name="MT1304"></a>

### <a name="mt1304-the-embedded-framework--in--is-invalid-it-does-not-contain-an-infoplist"></a>MT1304: внедренная платформа " \* " в \* является недопустимой: она не содержит info. plist.

Указанная внедренная платформа не содержит info. plist и поэтому не является допустимой платформой.

Убедитесь, что платформа допустима.

<a name="MT1305"></a>

### <a name="mt1305-the-binding-library--contains-a-user-framework--but-embedded-user-frameworks-require-ios-80-the-current-deployment-target-is--please-set-the-deployment-target-in-the-infoplist-file-to-at-least-80"></a>MT1305: в библиотеке привязки " \* " содержится пользовательская платформа ( \* ), но для внедренных пользовательских платформ требуется iOS 8,0 (текущий целевой объект развертывания — *). Задайте целевой объект развертывания в файле info. plist по меньшей мере 8,0.

Указанная библиотека привязки содержит внедренную платформу, но Xamarin. iOS поддерживает только внедренные платформы в iOS 8,0 или более поздней версии.

Чтобы устранить эту ошибку (или не использовать внедренные платформы), задайте целевой объект развертывания в файле info. plist по меньшей мере 8,0.

### <a name="mt14xx-crash-reports"></a>MT14xx: отчеты о сбоях

<!--
  MT14xx    CrashReports.cs
  -->

<a name="MT1400"></a>

### <a name="mt1400-could-not-open-crash-report-service-afcconnectionopen-returned-"></a>MT1400: не удалось открыть службу отчетов о сбоях: возвращено Афкконнектионопен *

Произошла ошибка при попытке получить доступ к отчетам о сбоях с устройства.

Для решения этой проблемы выполните следующие действия.

- Удалите приложение с устройства и повторите попытку.
- Отключите устройство и подключите его повторно.
- Перезапустите устройство.
- Перезагрузите компьютер Mac.
- Синхронизируйте устройство с iTunes (при этом все отчеты о сбоях будут удалены с устройства).

<a name="MT1401"></a>

### <a name="mt1401-could-not-close-crash-report-service-afcconnectionclose-returned-"></a>MT1401: не удалось закрыть службу отчетов о сбоях: Афкконнектионклосе вернул *

Произошла ошибка при попытке получить доступ к отчетам о сбоях с устройства.

Для решения этой проблемы выполните следующие действия.

- Удалите приложение с устройства и повторите попытку.
- Отключите устройство и подключите его повторно.
- Перезапустите устройство.
- Перезагрузите компьютер Mac.
- Синхронизируйте устройство с iTunes (при этом все отчеты о сбоях будут удалены с устройства).

<a name="MT1402"></a>

### <a name="mt1402-could-not-read-file-info-for--afcfileinfoopen-returned-"></a>MT1402: не удалось прочитать сведения о файле для \* : возвращено афкфилеинфупен\*

Произошла ошибка при попытке получить доступ к отчетам о сбоях с устройства.

Для решения этой проблемы выполните следующие действия.

- Удалите приложение с устройства и повторите попытку.
- Отключите устройство и подключите его повторно.
- Перезапустите устройство.
- Перезагрузите компьютер Mac.
- Синхронизируйте устройство с iTunes (при этом все отчеты о сбоях будут удалены с устройства).

<a name="MT1403"></a>

### <a name="mt1403-could-not-read-crash-report-afcdirectoryopen--returned-"></a>MT1403: не удалось считать отчет о сбоях: Афкдиректорйопен ( \* ) вернул:\*

Произошла ошибка при попытке получить доступ к отчетам о сбоях с устройства.

Для решения этой проблемы выполните следующие действия.

- Удалите приложение с устройства и повторите попытку.
- Отключите устройство и подключите его повторно.
- Перезапустите устройство.
- Перезагрузите компьютер Mac.
- Синхронизируйте устройство с iTunes (при этом все отчеты о сбоях будут удалены с устройства).

<a name="MT1404"></a>

### <a name="mt1404-could-not-read-crash-report-afcfilerefopen--returned-"></a>MT1404: не удалось считать отчет о сбоях: Афкфилерефопен ( \* ) вернул:\*

Произошла ошибка при попытке получить доступ к отчетам о сбоях с устройства.

Для решения этой проблемы выполните следующие действия.

- Удалите приложение с устройства и повторите попытку.
- Отключите устройство и подключите его повторно.
- Перезапустите устройство.
- Перезагрузите компьютер Mac.
- Синхронизируйте устройство с iTunes (при этом все отчеты о сбоях будут удалены с устройства).

<a name="MT1405"></a>

### <a name="mt1405-could-not-read-crash-report-afcfilerefread--returned-"></a>MT1405: не удалось считать отчет о сбоях: Афкфилерефреад ( \* ) вернул:\*

Произошла ошибка при попытке получить доступ к отчетам о сбоях с устройства.

Для решения этой проблемы выполните следующие действия.

- Удалите приложение с устройства и повторите попытку.
- Отключите устройство и подключите его повторно.
- Перезапустите устройство.
- Перезагрузите компьютер Mac.
- Синхронизируйте устройство с iTunes (при этом все отчеты о сбоях будут удалены с устройства).

<a name="MT1406"></a>

### <a name="mt1406-could-not-list-crash-reports-afcdirectoryopen--returned-"></a>MT1406: не удалось перечислить отчеты о сбоях: Афкдиректорйопен ( \* ) вернул:\*

Произошла ошибка при попытке получить доступ к отчетам о сбоях с устройства.

Для решения этой проблемы выполните следующие действия.

- Удалите приложение с устройства и повторите попытку.
- Отключите устройство и подключите его повторно.
- Перезапустите устройство.
- Перезагрузите компьютер Mac.
- Синхронизируйте устройство с iTunes (при этом все отчеты о сбоях будут удалены с устройства).

<!--- 1407 used by mmp -->

### <a name="mt16xx-macho"></a>MT16xx: мачо

<!--
  MT16xx    MachO.cs
  -->

<a name="MT1600"></a>

### <a name="mt1600-not-a-mach-o-dynamic-library-unknown-header-0x-"></a>MT1600: не является динамической библиотекой машинного ввода (неизвестный заголовок "0x *"): *.

При обработке динамической библиотеки возникла ошибка.

Убедитесь, что динамическая библиотека является допустимой динамической библиотекой машинного ввода-вывода.

Формат библиотеки можно проверить с помощью `file` команды из терминала:

```
file -arch all -l /path/to/library.dylib
```

<a name="MT1601"></a>

### <a name="mt1601-not-a-static-library-unknown-header--"></a>MT1601: не является статической библиотекой (неизвестный заголовок "*"): *.

При обработке статической библиотеки возникла ошибка.

Убедитесь, что статическая библиотека является допустимой статической библиотекой машинного ввода-вывода.

Формат библиотеки можно проверить с помощью `file` команды из терминала:

```
file -arch all -l /path/to/library.a
```

<a name="MT1602"></a>

### <a name="mt1602-not-a-mach-o-dynamic-library-unknown-header-0x-"></a>MT1602: не является динамической библиотекой машинного ввода (неизвестный заголовок "0x *"): *.

При обработке динамической библиотеки возникла ошибка.

Убедитесь, что динамическая библиотека является допустимой динамической библиотекой машинного ввода-вывода.

Формат библиотеки можно проверить с помощью `file` команды из терминала:

```
file -arch all -l /path/to/library.dylib
```

<a name="MT1603"></a>

### <a name="mt1603-unknown-format-for-fat-entry-at-position--in-"></a>MT1603: неизвестный формат записи FAT в позиции \* в \* .

Произошла ошибка при обработке архива FAT.

Убедитесь, что Архив FAT допустим.

Формат архива FAT можно проверить с помощью `file` команды из терминала:

```
file -arch all -l /path/to/file
```

<a name="MT1604"></a>

### <a name="mt1604-file-of-type--is-not-a-macho-file-"></a>MT1604: файл типа \* не является файлом мачо ( \* ).

Произошла ошибка при обработке рассматриваемого файла мачо.

Убедитесь, что файл является допустимой динамической библиотекой для операций с машинным вводом.

Формат файла можно проверить с помощью `file` команды терминала:

```
file -arch all -l /path/to/file
```

## <a name="mt2xxx-linker-error-messages"></a>MT2xxx: сообщения об ошибках компоновщика

<!--
 MT2xxx Linker
  MT20xx Linker (general) errors
  -->

<a name="MT2001"></a>

### <a name="mt2001-could-not-link-assemblies"></a>MT2001: не удалось связать сборки

Эта ошибка означает, что в управляемом компоновщике произошла непредвиденная ошибка, например исключение, и не удалось завершить или сохранить обрабатываемую сборку. Дополнительные сведения об точной ошибке будут входить в журнал сборки, например

```
error MT2001: Could not link assemblies.
    Method: `System.Void Todo.TodoListPageCS/<<-ctor>b__1_0>d::SetStateMachine(System.Runtime.CompilerServices.IAsyncStateMachine)`
    Assembly: `QuickTodo, Version=1.0.6297.28241, Culture=neutral, PublicKeyToken=null`
Reason: Value cannot be null.
Parameter name: instruction
```

Очень важно отправить отчет об ошибке для таких проблем. В большинстве случаев обходной путь можно предоставить, пока не будет опубликовано правильное исправление. Приведенные выше сведения являются критически важными (вместе с тестовым случаем и (или) сборкой бинаири) для решения проблемы.

<a name="MT2002"></a>

### <a name="mt2002-can-not-resolve-reference-"></a>MT2002: не удается разрешить ссылку: *

<a name="MT2003"></a>

### <a name="mt2003-option--will-be-ignored-since-linking-is-disabled"></a>MT2003: параметр "*" будет пропущен, так как связь отключена

<a name="MT2004"></a>

### <a name="mt2004-extra-linker-definitions-file--could-not-be-located"></a>MT2004: не удалось найти дополнительный файл определений компоновщика "*".

<a name="MT2005"></a>

### <a name="mt2005-definitions-from--could-not-be-parsed"></a>MT2005: не удалось выполнить синтаксический анализ определений из "*".

<a name="MT2006"></a>

### <a name="mt2006-can-not-load-mscorlibdll-from--please-reinstall-xamarinios"></a>MT2006: не удается загрузить библиотеку mscorlib. dll из: *. Переустановите Xamarin. iOS.

Обычно это означает, что возникла проблема с установкой Xamarin. iOS. Попробуйте переустановить Xamarin. iOS.

<!--- 2007 used by mmp -->
<!--- 2009 used by mmp -->

<a name="MT2010"></a>

### <a name="mt2010-unknown-httpmessagehandler--valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MT2010: неизвестный HttpMessageHandler `*` . Допустимые значения: HttpClientHandler (по умолчанию), Кфнетворкхандлер или Нсурлсессионхандлер

<a name="MT2011"></a>

### <a name="mt2011-unknown-tlsprovider---valid-values-are-default-or-appletls"></a>MT2011: неизвестный Тлспровидер `*` .  Допустимые значения: Default или апплетлс.

Значение, заданное для `tls-provider=` , не является допустимым поставщиком TLS (безопасность транспортного уровня).

`default`И `appletls` являются единственными допустимыми значениями, и оба представляют один и тот же параметр, который обеспечивает поддержку SSL/TLS с помощью собственного API Apple TLS.

<!--- 2012 used by mmp -->
<!--- 2013 used by mmp -->
<!--- 2014 used by mmp -->

<a name="MT2015"></a>

### <a name="mt2015-invalid-httpmessagehandler--for-watchos-the-only-valid-value-is-nsurlsessionhandler"></a>MT2015: недопустимый HttpMessageHandler `*` для watchOS. Единственное допустимое значение — Нсурлсессионхандлер.

Это предупреждение возникает из-за того, что в файле проекта указан недопустимый HttpMessageHandler.

Предыдущие версии средств предварительного просмотра, созданные по умолчанию, имеют недопустимое значение в файле проекта.

Чтобы устранить это предупреждение, откройте файл проекта в текстовом редакторе и удалите из XML все узлы HttpMessageHandler.

<a name="MT2016"></a>

### <a name="mt2016-invalid-tlsprovider-legacy-option-the-only-valid-value-appletls-will-be-used"></a>MT2016: недопустимый `legacy` параметр тлспровидер. Будет использовано единственное допустимое значение `appletls` .

`legacy`Поставщик, который был полностью управляемым поставщиком SSLv3/TLSv1, не поставляется с Xamarin. iOS больше. Проекты, которые использовали этот старый поставщик, теперь используют более новый `appletls` .

Чтобы устранить это предупреждение, откройте файл проекта в текстовом редакторе и удалите из XML все узлы "Мтаучтлспровидер".

<a name="MT2017"></a>

### <a name="mt2017-could-not-process-xml-description"></a>MT2017: не удалось обработать XML-описание.

Это означает наличие ошибки в указанном [файле конфигурации компоновщика XML](~/cross-platform/deploy-test/linker.md) . Проверьте файл.

<a name="MT2018"></a>

### <a name="mt2018-the-assembly--is-referenced-from-two-different-locations--and-"></a>MT2018: ссылка на сборку " \* " содержится в двух разных расположениях: " \* " и "*".

Сборка, упомянутая в сообщении об ошибке, загружается из нескольких расположений. Обязательно используйте одну и ту же версию сборки.

<a name="MT2019"></a>

### <a name="mt2019-can-not-load-the-root-assembly-"></a>MT2019: не удается загрузить корневую сборку "*"

Не удалось загрузить корневую сборку. Убедитесь, что путь в сообщении об ошибке ссылается на существующий файл и является допустимой сборкой .NET.

<a name="MT202x"></a>

### <a name="mt202x-binding-optimizer-failed-processing-"></a>MT202x: не удалось обработать оптимизатор привязки `...` .

Возникла непредвиденная ошибка при попытке оптимизации созданного кода привязки. Элемент, вызвавший ошибку, назван в сообщении об ошибке. Чтобы устранить эту проблему, необходимо предоставить сборку с именем (или содержащую тип или метод с именем) в новой ошибке в [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) вместе с полным журналом сборки с включенной детализацией (т. е. `-v -v -v -v` в **дополнительных аргументах mtouch**).

Последняя цифра `x` будет:

- `0`для имени сборки;
- `1`для имени типа;
- `3`для имени метода;

<a name="MT2030"></a>

### <a name="mt2030-remove-user-resources-failed-processing-"></a>MT2030: ошибка при удалении пользовательских ресурсов `...` .

Произошла непредвиденная ошибка при попытке удаления пользовательских ресурсов. Сборка, вызвавшая ошибку, называется в сообщении об ошибке. Чтобы устранить эту проблему, сборка должна быть предоставлена в новой ошибке на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) вместе с полным журналом сборки с включенной детализацией (т. е. `-v -v -v -v` в **дополнительных аргументах mtouch**).

Пользовательские ресурсы — это файлы, включенные в сборки (в виде ресурсов), которые необходимо извлечь во время сборки, чтобы создать пакет приложений. В том числе:

- `__monotouch_content_*`и `__monotouch_pages_*` ресурсы; и
- Встроенные библиотеки, внедренные в сборку привязки;

<a name="MT2040"></a>

### <a name="mt2040-default-httpmessagehandler-setter-failed-processing-"></a>MT2040: не удалось обработать метод задания HttpMessageHandler по умолчанию `...` .

Возникла непредвиденная ошибка при попытке задать значение по умолчанию `HttpMessageHandler` для приложения. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) вместе с полным журналом сборки с включенной детализацией (т. е. `-v -v -v -v` в **дополнительных аргументах mtouch**).

<a name="MT2050"></a>

### <a name="mt2050-code-remover-failed-processing-"></a>MT2050: сбой обработки кода удаления `...` .

Возникла непредвиденная ошибка при попытке удаления кода из доставки BCL с приложением. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) вместе с полным журналом сборки с включенной детализацией (т. е. `-v -v -v -v` в **дополнительных аргументах mtouch**).

<a name="MT2060"></a>

### <a name="mt2060-sealer-failed-processing-"></a>MT2060: сбой при обработке запечатывания `...` .

Возникла непредвиденная ошибка при попытке запечатать типы или методы (Final) или при девиртуализации некоторых методов. Сборка, вызвавшая ошибку, называется в сообщении об ошибке. Чтобы устранить эту проблему, сборка должна быть предоставлена в новой ошибке на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) вместе с полным журналом сборки с включенной детализацией (т. е. `-v -v -v -v` в **дополнительных аргументах mtouch**).

<a name="MT2070"></a>

### <a name="mt2070-metadata-reducer-failed-processing-"></a>MT2070: сбой обработки сокращения метаданных `...` .

Возникла непредвиденная ошибка при попытке сократить метаданные приложения. Сборка, вызвавшая ошибку, называется в сообщении об ошибке. Чтобы устранить эту проблему, сборка должна быть предоставлена в новой ошибке на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) вместе с полным журналом сборки с включенной детализацией (т. е. `-v -v -v -v` в **дополнительных аргументах mtouch**).

<a name="MT2080"></a>

### <a name="mt2080-marknsobjects-failed-processing-"></a>MT2080: сбой обработки Маркнсобжектс `...` .

Возникла непредвиденная ошибка при попытке пометить `NSObject` подклассы из приложения. Сборка, вызвавшая ошибку, называется в сообщении об ошибке. Чтобы устранить эту проблему, сборка должна быть предоставлена в новой ошибке на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) вместе с полным журналом сборки с включенной детализацией (т. е. `-v -v -v -v` в **дополнительных аргументах mtouch**).

<a name="MT2090"></a>

### <a name="mt2090-inliner-failed-processing-"></a>MT2090: сбой обработки во встроенном коде `...` .

Возникла непредвиденная ошибка при попытке встроенного кода из приложения. Сборка, вызвавшая ошибку, называется в сообщении об ошибке. Чтобы устранить эту проблему, сборка должна быть предоставлена в новой ошибке на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) вместе с полным журналом сборки с включенной детализацией (т. е. `-v -v -v -v` в **дополнительных аргументах mtouch**).

<!-- MT21xx: more linker errors -->

<!--- 2100 used by mmp -->

<a name="MT2100"></a>

### <a name="mt2100-smart-enum-conversion-preserver-failed-processing-"></a>MT2100: не удалось обработать промежуточный сервер преобразования Smart Enum `...` .

Возникла непредвиденная ошибка при попытке пометить методы преобразования для интеллектуальных перечислений из приложения. Сборка, вызвавшая ошибку, называется в сообщении об ошибке. Чтобы устранить эту проблему, сборка должна быть предоставлена в новой ошибке на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) вместе с полным журналом сборки с включенной детализацией (т. е. `-v -v -v -v` в **дополнительных аргументах mtouch**).

<a name="MT2101"></a>

### <a name="mt2101-cant-resolve-the-reference--referenced-from-the-method--in-"></a>MT2101: не удается разрешить ссылку " \* ", на которую ссылается метод " \* " в "*".

При обработке метода, указанного в сообщении об ошибке, обнаружена недопустимая ссылка на сборку.

Сборка, вызвавшая ошибку, называется в сообщении об ошибке. Чтобы устранить эту проблему, сборка должна быть предоставлена в новой ошибке на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) вместе с полным журналом сборки с включенной детализацией (т. е. `-v -v -v -v` в **дополнительных аргументах mtouch**).

<a name="MT2102"></a>

### <a name="mt2102-error-processing-the-method--in-the-assembly--"></a>MT2102: ошибка при обработке метода " \* " в сборке " \* ": *

Возникла непредвиденная ошибка при попытке пометить метод, упомянутый в сообщении об ошибке.

Сборка, вызвавшая ошибку, называется в сообщении об ошибке. Чтобы устранить эту проблему, сборка должна быть предоставлена в новой ошибке на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) вместе с полным журналом сборки с включенной детализацией (т. е. `-v -v -v -v` в **дополнительных аргументах mtouch**).

<a name="MT2103"></a>

### <a name="mt2103-error-processing-assembly--"></a>MT2103: ошибка при обработке сборки " \* ": *

При обработке сборки произошла непредвиденная ошибка.

Сборка, вызвавшая ошибку, называется в сообщении об ошибке. Чтобы устранить эту проблему, сборка должна быть предоставлена в новой ошибке на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) вместе с полным журналом сборки с включенной детализацией (т. е. `-v -v -v -v` в **дополнительных аргументах mtouch**).

<a name="MT2104"></a>

### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104: не удалось связать сборку " {0} ", так как она имеет смешанный режим.

Сборки в смешанном режиме не могут быть обработаны компоновщиком.

https://docs.microsoft.com/cpp/dotnet/mixed-native-and-managed-assembliesДополнительные сведения о сборках в смешанном режиме см. в разделе.

## <a name="mt3xxx-aot-error-messages"></a>MT3xxx: сообщения об ошибках AOT

<!--
 MT3xxx AOT
  MT30xx AOT (general) errors
  -->

<a name="MT3001"></a>

### <a name="mt3001-could-not-aot-the-assembly-"></a>MT3001: не удалось AOT сборку "*"

Обычно это указывает на ошибку в компиляторе AOT. Запишите новую ошибку в [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) , указав проект, который можно использовать для воспроизведения ошибки.

Иногда это можно обойти, отключив добавочные сборки в параметре сборки iOS проекта (но по-прежнему это ошибка, поэтому сообщите о нем).

<a name="MT3002"></a>

### <a name="mt3002-aot-restriction-method--must-be-static-since-it-is-decorated-with-monopinvokecallback-see-developerxamarincomguidesiosadvanced_topicslimitationsreverse_callbacks"></a>MT3002: ограничение AOT: метод "*" должен быть статическим, так как он дополнен атрибутом [Монопинвокекаллбакк]. См. раздел [developer.xamarin.com/guides/ios/advanced_topics/limitations/#Reverse_Callbacks](~/ios/internals/limitations.md#reverse-callbacks)

Это сообщение об ошибке поступает из компилятора AOT.

<a name="MT3003"></a>

### <a name="mt3003-conflicting---debug-and---llvm-options-soft-debugging-is-disabled"></a>MT3003: конфликт параметров--Debug и--LLVM. Обратимая отладка отключена.

Отладка не поддерживается, если включен LLVM. Если необходимо выполнить отладку приложения, сначала отключите LLVM.

<a name="MT3004"></a>

### <a name="mt3004-could-not-aot-the-assembly--because-it-doesnt-exist"></a>MT3004: не удалось AOT сборку "*", так как она не существует.

<a name="MT3005"></a>

### <a name="mt3005-the-dependency--of-the-assembly--was-not-found-please-review-the-projects-references"></a>MT3005: \* не найдена зависимость "" сборки " \* ". Проверьте ссылки проекта.

Обычно это происходит, когда сборка ссылается на другую версию сборки платформы (обычно это версия .NET 4 библиотеки mscorlib. dll).

Эта функция не поддерживается и может не быть правильно построена или выполнена (сборка может использовать API из версии библиотеки mscorlib. dll для .NET 4, которой нет в версии Xamarin. iOS).

<a name="MT3006"></a>

### <a name="mt3006-could-not-compute-a-complete-dependency-map-for-the-project-this-will-result-in-slower-build-times-because-xamarinios-cant-properly-detect-what-needs-to-be-rebuilt-and-what-does-not-need-to-be-rebuilt-please-review-previous-warnings-for-more-details"></a>MT3006: не удалось вычислить полную карту зависимостей для проекта. Это приведет к снижению времени сборки, поскольку Xamarin. iOS не может правильно определить, что необходимо перестроить (и что не требуется перестраивать). Дополнительные сведения см. в предыдущих предупреждениях.

 сборка или выполнение должным образом (сборка может использовать API из версии библиотеки mscorlib. dll, которая не имеет версия Xamarin. iOS).

<a name="MT3007"></a>

### <a name="mt3007-debug-info-files-mdb-will-not-be-loaded-when-llvm-is-enabled"></a>MT3007: файлы сведений отладки (*. mdb) не будут загружены при включенном LLVM.

<a name="MT3008"></a>

### <a name="mt3008-bitcode-support-requires-the-use-of-the-llvm-aot-backend---llvm"></a>MT3008: поддержка Bitcode требует использования серверной части LLVM AOT (--LLVM)

Для поддержки Bitcode требуется использование серверной части LLVM AOT (--LLVM).

Отключите поддержку Bitcode или включите LLVM.

<!--- 3009 used by mmp -->
<!--- 3010 used by mmp -->

## <a name="mt4xxx-code-generation-error-messages"></a>MT4xxx: сообщения об ошибках создания кода

### <a name="mt40xx-main"></a>MT40xx: основной

<!--
 MT4xxx code generation
  MT40xx main.m
  -->

<a name="MT4001"></a>

### <a name="mt4001-the-main-template-could-not-be-expanded-to-"></a>MT4001: не удалось расширить основной шаблон до `*` .

Произошла ошибка при создании `main.m` . Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4002"></a>

### <a name="mt4002-failed-to-compile-the-generated-code-for-pinvoke-methods-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4002: не удалось скомпилировать созданный код для методов P/Invoke. Отправляйте отчет об ошибках по адресуhttp://bugzilla.xamarin.com

Не удалось скомпилировать созданный код для методов P/Invoke. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

### <a name="mt41xx-registrar"></a>MT41xx: регистратор

<!--
  MT41xx registrar.m
  -->

<a name="MT4101"></a>

### <a name="mt4101-the-registrar-cannot-build-a-signature-for-type-"></a>MT4101: регистратор не может создать сигнатуру для типа `*` .

В экспортированном API обнаружен тип, который среда выполнения не знает, как выполнить упаковку в или из цели-C.

Если вы считаете, что Xamarin. iOS должен поддерживать рассматриваемый тип, отправьте запрос на расширение на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4102"></a>

### <a name="mt4102-the-registrar-found-an-invalid-type--in-signature-for-method--use--instead"></a>MT4102: регистратор обнаружил недопустимый тип `*` в сигнатуре для метода `*` . Взамен рекомендуется использовать `*`.

В настоящее время это происходит только с одним типом: System. DateTime. Вместо этого используйте эквивалент цели-C (Нсдате).

<a name="MT4103"></a>

### <a name="mt4103-the-registrar-found-an-invalid-type--in-signature-for-method--the-type-implements-inativeobject-but-does-not-have-a-constructor-that-takes-two-intptr-bool-arguments"></a>MT4103: регистратор обнаружил недопустимый тип `*` в сигнатуре для метода `*` : тип реализует инативеобжект, но не имеет конструктора, принимающего два аргумента (IntPtr, bool)

Это происходит, когда регистратор встречает тип в сигнатуре с упомянутыми характеристиками. Регистратору может потребоваться создать новые экземпляры типа, и в этом случае требуется конструктор с сигнатурой (IntPtr, bool). Первый аргумент (IntPtr) Указывает управляемый дескриптор, а второй — если вызывающий объект перемещается за владение собственным дескриптором (если это значение равно false, будет вызываться для объекта).

<a name="MT4104"></a>

### <a name="mt4104-the-registrar-cannot-marshal-the-return-value-for-type--in-signature-for-method-"></a>MT4104: регистратор не может маршалировать возвращаемое значение для типа `*` в сигнатуре для метода `*` .

В экспортированном API обнаружен тип, который среда выполнения не знает, как выполнить упаковку в или из цели-C.

Если вы считаете, что Xamarin. iOS должен поддерживать рассматриваемый тип, отправьте запрос на расширение на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4105"></a>

### <a name="mt4105-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a>MT4105: регистратор не может маршалировать параметр типа `*` в сигнатуре для метода `*` .

Если вы считаете, что Xamarin. iOS должен поддерживать рассматриваемый тип, отправьте запрос на расширение на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4106"></a>

### <a name="mt4106-the-registrar-cannot-marshal-the-return-value-for-structure--in-signature-for-method-"></a>MT4106: регистратор не может маршалировать возвращаемое значение для структуры `*` в сигнатуре метода `*` .

В экспортированном API обнаружен тип, который среда выполнения не знает, как выполнить упаковку в или из цели-C.

Если вы считаете, что Xamarin. iOS должен поддерживать рассматриваемый тип, отправьте запрос на расширение на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4107"></a>

### <a name="mt4107-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a>MT4107: регистратор не может маршалировать параметр типа `*` в сигнатуре для метода `+` .

В экспортированном API обнаружен тип, который среда выполнения не знает, как выполнить упаковку в или из цели-C.

Если вы считаете, что Xamarin. iOS должен поддерживать рассматриваемый тип, отправьте запрос на расширение на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4108"></a>

### <a name="mt4108-the-registrar-cannot-get-the-objectivec-type-for-managed-type-"></a>MT4108: регистратор не может получить тип ObjectiveC для управляемого типа `*` .

В экспортированном API обнаружен тип, который среда выполнения не знает, как выполнить упаковку в или из цели-C.

Если вы считаете, что Xamarin. iOS должен поддерживать рассматриваемый тип, отправьте запрос на расширение на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4109"></a>

### <a name="mt4109-failed-to-compile-the-generated-registrar-code-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4109: не удалось скомпилировать созданный код регистратора. Отправляйте отчет об ошибках по адресуhttp://bugzilla.xamarin.com

Не удалось скомпилировать созданный код для регистратора. Журнал сборки будет содержать выходные данные от собственного компилятора, объясняющие, почему код не компилируется.

Это всегда является ошибкой в Xamarin. iOS; Запишите новую ошибку в [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) с помощью своего проекта или тестового случая.

<a name="MT4110"></a>

### <a name="mt4110-the-registrar-cannot-marshal-the-out-parameter-of-type--in-signature-for-method-"></a>MT4110: регистратор не может маршалировать выходной параметр типа `*` в сигнатуре для метода `*` .

<a name="MT4111"></a>

### <a name="mt4111-the-registrar-cannot-build-a-signature-for-type--in-method-"></a>MT4111: регистратор не может создать сигнатуру для типа `*` в методе `*` .

<a name="MT4112"></a>

### <a name="mt4112-the-registrar-found-an-invalid-type--registering-generic-types-with-objective-c-is-not-supported-and-may-lead-to-random-behavior-andor-crashes-for-backwards-compatibility-with-older-versions-of-xamarinios-it-is-possible-to-ignore-this-error-by-passing---unsupported--enable-generics-in-registrar-as-an-additional-mtouch-argument-in-the-projects-ios-build-options-page-see-developerxamarincomguidesiosadvanced_topicsregistrar-for-more-information"></a>MT4112: регистратор обнаружил недопустимый тип `*` . Регистрация универсальных типов с помощью цели-C не поддерживается и может привести к случайному поведению и (или) сбоям (для обратной совместимости с более старыми версиями Xamarin. iOS эту ошибку можно пропустить, передав в `--unsupported--enable-generics-in-registrar` качестве дополнительного аргумента mtouch на странице параметров сборки iOS проекта. Дополнительные сведения см. в разделе [Developer.Xamarin.com/Guides/iOS/advanced_topics/Registrar](~/ios/internals/registrar.md) .

<a name="MT4113"></a>

### <a name="mt4113-the-registrar-found-a-generic-method--exporting-generic-methods-is-not-supported-and-will-lead-to-random-behavior-andor-crashes"></a>MT4113: регистратор обнаружил универсальный метод: " \* . \* ". Экспорт универсальных методов не поддерживается и приводит к случайному поведению или сбоям.

<a name="MT4114"></a>

### <a name="mt4114-unexpected-error-in-the-registrar-for-the-method----please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4114: непредвиденная ошибка в регистраторе для метода " \* . \* "-Создайте отчет об ошибке по адресуhttp://bugzilla.xamarin.com

<a name="MT4116"></a>

### <a name="mt4116-could-not-register-the-assembly--"></a>MT4116: не удалось зарегистрировать сборку " \* ":\*

<a name="MT4117"></a>

### <a name="mt4117-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-method-takes--parameters-while-the-managed-method-has--parameters"></a>MT4117: регистратор обнаружил несовпадение сигнатур в методе " \* . \* ". селектор указывает, что метод принимает \* Параметры, а управляемый метод имеет \* Параметры.

<a name="MT4118"></a>

### <a name="mt4118-cannot-register-two-managed-types--and--with-the-same-native-name-"></a>MT4118: невозможно зарегистрировать два управляемых типа (" \* " и " \* ") с одним и тем же собственным именем ("*").

<a name="MT4119"></a>

### <a name="mt4119-could-not-register-the-selector--of-the-member--because-the-selector-is-already-registered-on-a-different-member"></a>MT4119: не удалось зарегистрировать селектор " \* " элемента " \* . *", так как селектор уже зарегистрирован для другого члена.

<a name="MT4120"></a>

### <a name="mt4120-the-registrar-found-an-unknown-field-type--in-field--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4120: регистратор обнаружил неизвестный тип поля " \* " в поле " \* . *". Отправляйте отчет об ошибках по адресуhttp://bugzilla.xamarin.com

Эта ошибка указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4121"></a>

### <a name="mt4121-cannot-use-gccg-to-compile-the-generated-code-from-the-static-registrar-when-using-the-accounts-framework-the-header-files-provided-by-apple-used-during-the-compilation-require-clang-either-use-clang---compilerclang-or-the-dynamic-registrar---registrardynamic"></a>MT4121: невозможно использовать GCC/G + + для компиляции созданного кода из статического регистратора при использовании платформы Accounts (для файлов заголовков, предоставленных Apple, используемых в процессе компиляции требуется Clang). Либо используйте Clang (--Compiler: Clang), либо динамический регистратор (--регистратор: Dynamic).

<a name="MT4122"></a>

### <a name="mt4122-cannot-use-the-clang-compiler-provided-in-the--sdk-to-compile-the-generated-code-from-the-static-registrar-when-non-ascii-type-names--are-present-in-the-application-either-use-gccg---compilergccg-the-dynamic-registrar---registrardynamic-or-a-newer-sdk"></a>MT4122: невозможно использовать компилятор Clang, указанный в *.* Пакет SDK для компиляции созданного кода из статического регистратора, если в приложении отсутствуют имена типов, отличные от ASCII ("*"). Либо используйте GCC/G + + (--Compiler: GCC | G + +), динамический регистратор (--регистратор: Dynamic) или более новый пакет SDK.

<a name="MT4123"></a>

### <a name="mt4123-the-type-of-the-variadic-parameter-in-the-variadic-function--must-be-systemintptr"></a>MT4123: тип параметра Variadic в функции Variadic "*" должен быть System. IntPtr.

<a name="MT4124"></a>

### <a name="mt4124-invalid--found-on--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4124: \* обнаружено недопустимое для " \* ". Отправляйте отчет об ошибках по адресуhttp://bugzilla.xamarin.com

Эта ошибка указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4125"></a>

### <a name="mt4125-the-registrar-found-an-invalid-type--in-signature-for-method--the-interface-must-have-a-protocol-attribute-specifying-its-wrapper-type"></a>MT4125: регистратор обнаружил недопустимый тип " \* " в сигнатуре для метода " \* ": интерфейс должен иметь атрибут протокола, указывающий тип оболочки.

<a name="MT4126"></a>

### <a name="mt4126-cannot-register-two-managed-protocols--and--with-the-same-native-name-"></a>MT4126: не удается зарегистрировать два управляемых протокола (" \* " и " \* ") с одним и тем же собственным именем ("*").

<a name="MT4127"></a>

### <a name="mt4127-cannot-register-more-than-one-interface-method-for-the-method--which-is-implementing-"></a>MT4127: невозможно зарегистрировать более одного метода интерфейса для метода " \* " (который реализует " \* ").

<a name="MT4128"></a>

### <a name="mt4128-the-registrar-found-an-invalid-generic-parameter-type--in-the-method--the-generic-parameter-must-have-an-nsobject-constraint"></a>MT4128: регистратор обнаружил недопустимый универсальный тип параметра " \* " в методе " \* ". Универсальный параметр должен иметь ограничение "Нсобжект".

<a name="MT4129"></a>

### <a name="mt4129-the-registrar-found-an-invalid-generic-return-type--in-the-method--the-generic-return-type-must-have-an-nsobject-constraint"></a>MT4129: регистратор обнаружил недопустимый универсальный тип возвращаемого значения " \* " в методе " \* ". Универсальный тип возвращаемого значения должен иметь ограничение "Нсобжект".

<a name="MT4130"></a>

### <a name="mt4130-the-registrar-cannot-export-static-methods-in-generic-classes-"></a>MT4130: регистратор не может экспортировать статические методы в универсальных классах ("*").

<a name="MT4131"></a>

### <a name="mt4131-the-registrar-cannot-export-static-properties-in-generic-classes-"></a>MT4131: регистратор не может экспортировать статические свойства в универсальных классах (" \* . \* ").

<a name="MT4132"></a>

### <a name="mt4132-the-registrar-found-an-invalid-generic-return-type--in-the-property--the-return-type-must-have-an-nsobject-constraint"></a>MT4132: регистратор обнаружил недопустимый универсальный тип возвращаемого \* значения "" в свойстве " \* ". Тип возвращаемого значения должен иметь ограничение "Нсобжект".

<a name="MT4133"></a>

### <a name="mt4133-cannot-construct-an-instance-of-the-type--from-objective-c-because-the-type-is-generic-runtime-exception"></a>MT4133: не удается создать экземпляр типа "*" из цели-C, так как тип является универсальным. [Исключение времени выполнения]

<a name="MT4134"></a>

### <a name="mt4134-your-application-is-using-the--framework-which-isnt-included-in-the-ios-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-ios--while-youre-building-with-the-ios--sdk-please-select-a-newer-sdk-in-your-apps-ios-build-options"></a>MT4134: ваше приложение использует \* платформу "", которая не входит в состав пакета SDK для iOS, который вы используете для сборки приложения (Эта платформа была введена в iOS \* при сборке с помощью \* пакета SDK для iOS). Выберите более новый пакет SDK в параметрах сборки iOS приложения.

<a name="MT4135"></a>

### <a name="mt4135-the-member--has-an-export-attribute-that-doesnt-specify-a-selector-a-selector-is-required"></a>MT4135: элемент " \* . \* " имеет атрибут экспорта, который не указывает селектор. Требуется селектор.

<a name="MT4136"></a>

### <a name="mt4136-the-registrar-cannot-marshal-the-parameter-type--of-the-parameter--in-the-method-"></a>MT4136: регистратор не может выполнить упаковку типа параметра "" \* параметра "" \* в методе " \* . *"

<!-- MT4137 is unused -->

<a name="MT4138"></a>

### <a name="mt4138-the-registrar-cannot-marshal-the-property-type--of-the-property-"></a>MT4138: регистратор не может выполнить упаковку типа свойства " \* " Свойства " \* . *".

<a name="MT4139"></a>

### <a name="mt4139-the-registrar-cannot-marshal-the-property-type--of-the-property--properties-with-the-connect-attribute-must-have-a-property-type-of-nsobject-or-a-subclass-of-nsobject"></a>MT4139: регистратор не может выполнить упаковку типа свойства " \* " Свойства " \* . *". Свойства с атрибутом [Connect] должны иметь тип свойства Нсобжект (или подкласс Нсобжект).

<a name="MT4140"></a>

### <a name="mt4140-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-variadic-method-takes--parameters-while-the-managed-method-has--parameters"></a>MT4140: регистратор обнаружил несовпадение сигнатур в методе " \* . \* " — селектор указывает, что метод Variadic принимает \* Параметры, а управляемый метод имеет \* Параметры.

<a name="MT4141"></a>

### <a name="mt4141-cannot-register-the-selector--on-the-member--because-xamarinios-implicitly-registers-this-selector"></a>MT4141: не удается зарегистрировать селектор " \* " для члена " \* ", так как Xamarin. iOS неявно регистрирует этот селектор.

Это происходит при создании подкласса типа платформы и попытке реализовать метод "удержать", "Release" или "unalloc":

```csharp
class MyNSObject : NSObject
{
  [Export ("retain")]
  new void Retain () {}

  [Export ("release")]
  new void Release () {}

  [Export ("dealloc")]
  new void Dealloc () {}
}
```

Однако можно переопределить эти методы, если класс не является первым подклассом типа платформы:

```csharp
class MyNSObject : NSObject
{
}

class MyCustomNSObject : MyNSObject
{
  [Export ("retain")]
  new void Retain () {}

  [Export ("release")]
  new void Release () {}

  [Export ("dealloc")]
  new void Dealloc () {}
}
```

В этом случае Xamarin. iOS будет переопределять `retain` , `release` а `dealloc` в `MyNSObject` классе и нет конфликтов.

<a name="MT4142"></a>

### <a name="mt4142-failed-to-register-the-type-"></a>MT4142: не удалось зарегистрировать тип "*".

<a name="MT4143"></a>

### <a name="mt4143-the-objectivec-class--could-not-be-registered-it-does-not-seem-to-derive-from-any-known-objectivec-class-including-nsobject"></a>MT4143: не удалось зарегистрировать класс ObjectiveC "*", он не является производным от любого известного класса ObjectiveC (включая Нсобжект).

<a name="MT4144"></a>

### <a name="mt4144-cannot-register-the-method--since-it-does-not-have-an-associated-trampoline-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4144: не удается зарегистрировать метод "*", так как у него нет связанного трамполине. Отправляйте отчет об ошибках по адресу http://bugzilla.xamarin.com .

Это указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4145"></a>

### <a name="mt4145-invalid-enum--enums-with-the-native-attribute-must-have-a-underlying-enum-type-of-either-long-or-ulong"></a>MT4145: недопустимое перечисление "*": перечисления с атрибутом [Native] должны иметь базовый перечисляемый тип либо "Long", либо "ulong".

<a name="MT4146"></a>

### <a name="mt4146-the-name-parameter-of-the-registrar-attribute-on-the-class---contains-an-invalid-character--"></a>MT4146: параметр Name атрибута регистратор в классе " \* " (" \* ") содержит недопустимый символ: " \* " ( \* ).

Имя класса Обжектице-C не может содержать пробелы. Это означает, что `Register` атрибут в соответствующем управляемом классе не может `Name` содержать пробелы в качестве параметра.

Убедитесь, что `Register` атрибут в управляемом классе, упоминаемом в сообщении об ошибке, не содержит пробелов.

<a name="MT4147"></a>

### <a name="mt4147-detected-a-protocol-inheriting-from-the-jsexport-protocol-while-using-the-dynamic-registrar-it-is-not-possible-to-export-protocols-to-javascriptcore-dynamically-the-static-registrar-must-be-used-add---registrarstatic-to-the-additional-mtouch-arguments-in-the-projects-ios-build-options-to-select-the-static-registrar"></a>MT4147: обнаружен протокол, наследующий от протокола Жсекспорт при использовании динамического регистратора. Невозможно экспортировать протоколы в Жаваскрипткоре динамически. необходимо использовать статический регистратор (Add--регистратор: Static для дополнительных аргументов mtouch в параметрах сборки iOS проекта, чтобы выбрать статический регистратор).

<a name="MT4148"></a>

### <a name="mt4148-the-registrar-found-a-generic-protocol--exporting-generic-protocols-is-not-supported"></a>MT4148: регистратор обнаружил универсальный протокол: "*". Экспорт универсальных протоколов не поддерживается.

<a name="MT4149"></a>

### <a name="mt4149-cannot-register-the-method--because-the-type-of-the-first-parameter--does-not-match-the-category-type-"></a>MT4149: не удается зарегистрировать метод " \* . \* ", так как тип первого параметра (" \* ") не соответствует типу категории (" \* ").

<a name="MT4150"></a>

### <a name="mt4150-cannot-register-the-type--because-the-type-property--in-its-category-attribute-does-not-inherit-from-nsobject"></a>MT4150: не удается зарегистрировать тип " \* ", поскольку свойство типа (" \* ") в его атрибуте category не наследует от нсобжект.

<a name="MT4151"></a>

### <a name="mt4151-cannot-register-the-type--because-the-type-property-in-its-category-attribute-isnt-set"></a>MT4151: не удается зарегистрировать тип "*", так как свойство Type в его атрибуте category не задано.

<a name="MT4152"></a>

### <a name="mt4152-cannot-register-the-type--as-a-category-because-it-implements-inativeobject-or-subclasses-nsobject"></a>MT4152: не удается зарегистрировать тип "*" в качестве категории, так как он реализует Инативеобжект или подклассы Нсобжект.

<a name="MT4153"></a>

### <a name="mt4153-cannot-register-the-type--as-a-category-because-its-generic"></a>MT4153: невозможно зарегистрировать тип " \* " в качестве категории, так как он является универсальным.

<a name="MT4154"></a>

### <a name="mt4154-cannot-register-the-method--as-a-category-method-because-its-generic"></a>MT4154: не удается зарегистрировать метод " \* " в качестве метода категории, так как он является универсальным.

<a name="MT4155"></a>

### <a name="mt4155-cannot-register-the-method--with-the-selector--as-a-category-method-on--because-the-objective-c-already-has-an-implementation-for-this-selector"></a>MT4155: не удается зарегистрировать метод " \* " с селектором "" в \* качестве метода категории для "*", так как цель-C уже имеет реализацию для этого селектора.

<a name="MT4156"></a>

### <a name="mt4156-cannot-register-two-categories--and--with-the-same-native-name-"></a>MT4156: невозможно зарегистрировать две категории (" \* " и " \* ") с одним и тем же собственным именем ("*").

<a name="MT4157"></a>

### <a name="mt4157-cannot-register-the-category-method--because-at-least-one-parameter-is-required-and-its-type-must-match-the-category-type-"></a>MT4157: не удается зарегистрировать метод категории " \* ", так как по крайней мере один параметр является обязательным (и его тип должен соответствовать типу категории " \* ")

<a name="MT4158"></a>

### <a name="mt4158-cannot-register-the-constructor--in-the-category--because-constructors-in-categories-are-not-supported"></a>MT4158: не удается зарегистрировать конструктор \* в категории, \* так как конструкторы в категориях не поддерживаются.

<a name="MT4159"></a>

### <a name="mt4159-cannot-register-the-method--as-a-category-method-because-category-methods-must-be-static"></a>MT4159: не удается зарегистрировать метод "*" в качестве метода категории, так как методы категорий должны быть статическими.

<a name="MT4160"></a>

### <a name="mt4160-invalid-character---found-in-selector--for-"></a>MT4160: в селекторе "" для "" обнаружен недопустимый символ " \* " ( \* ) \* \* .

<a name="MT4161"></a>

### <a name="mt4161-the-registrar-found-an-unsupported-structure--all-fields-in-a-structure-must-also-be-structures-field--with-type-2-is-not-a-structure"></a>MT4161: регистратор обнаружил неподдерживаемую структуру " \* ": все поля в структуре также должны быть структурами (поле " \* " с типом " {2} " не является структурой).

Регистратор обнаружил структуру с неподдерживаемыми полями.

Все поля в структуре, предоставляемой цели-C, также должны быть структурами (не классами).

<a name="MT4162"></a>

### <a name="mt4162-the-type--used-as--2-is-not-available-in---it-was-introduced-in---please-build-with-a-newer--sdk-usually-done-by-using-the-most-recent-version-of-xcode"></a>MT4162: тип " \* " (используется как * {2} ) недоступен в * * (он появился в * *). \* выполните сборку с более новым пакетом SDK * (обычно это делается с помощью последней версии Xcode.

Регистратор обнаружил тип, который не включен в текущий пакет SDK.

Обновите Xcode.

<a name="MT4163"></a>

### <a name="mt4163-internal-error-in-the-registrar--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4163: Внутренняя ошибка в регистраторе (*). Отправляйте отчет об ошибках по адресуhttp://bugzilla.xamarin.com

Эта ошибка указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4164"></a>

### <a name="mt4164-cannot-export-the-property--because-its-selector--is-an-objective-c-keyword-please-use-a-different-name"></a>MT4164: не удается экспортировать свойство " \* ", так как его селектор " \* " является ключевым словом "цель-C". Укажите другое имя.

Селектор для рассматриваемого свойства не является допустимым идентификатором цели-C.

Используйте допустимый идентификатор цели-C в качестве селекторов.

<a name="MT4165"></a>

### <a name="mt4165-the-registrar-couldnt-find-the-type-systemvoid-in-any-of-the-referenced-assemblies"></a>MT4165: регистратору не удалось найти тип "System. void" в любой из сборок, на которые имеются ссылки.

Эта ошибка, скорее всего, указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4166"></a>

### <a name="mt4166-cannot-register-the-method--because-the-signature-contains-a-type--that-isnt-a-reference-type"></a>MT4166: не удается зарегистрировать метод " \* ", поскольку сигнатура содержит тип ( \* ), который не является ссылочным типом.

Обычно это указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4167"></a>

### <a name="mt4167-cannot-register-the-method--because-the-signature-contains-a-generic-type--with-a-generic-argument-type-that-isnt-an-nsobject-subclass-"></a>MT4167: не удается зарегистрировать метод " \* ", поскольку сигнатура содержит универсальный тип ( \* ) с универсальным типом аргумента, который не является подклассом нсобжект (*).

Обычно это указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4168"></a>

### <a name="mt4168-cannot-register-the-type-managed_name-because-its-objective-c-name-exported_name-is-an-objective-c-keyword-please-use-a-different-name"></a>MT4168: не удается зарегистрировать тип "{Managed \_ Name}", так как его цель-c "{Exported \_ Name}" является ключевым словом цели-c. Укажите другое имя.

Имя цели-C для рассматриваемого типа не является допустимым идентификатором цели-C.

Используйте допустимый идентификатор цели-C.

<a name="MT4169"></a>

### <a name="mt4169-failed-to-generate-a-pinvoke-wrapper-for-method-message"></a>MT4169: не удалось создать оболочку P/Invoke для {Method}: {Message}

Xamarin. iOS не удалось создать функцию-оболочку P/Invoke для упоминаемого.
Ознакомьтесь с выданным сообщением об ошибке для получения базовой причины.

<a name="MT4170"></a>

### <a name="mt4170-the-registrar-cant-convert-from-managed-type-to-native-type-for-the-return-value-in-the-method-method"></a>MT4170: регистратор не может выполнить преобразование из "{Managed Type}" в "{native Type}" для возвращаемого значения в методе {Method}.

См. описание ошибки <a href="#MT4172">MT4172</a>.

<a name="MT4171"></a>

### <a name="mt4171-the-bindas-attribute-on-the-member-member-is-invalid-the-bindas-type-type-is-different-from-the-property-type-type"></a>MT4171: атрибут Биндас элемента {Member} является недопустимым: тип Биндас {Type} отличается от типа свойства {Type}.

Убедитесь, что тип в атрибуте Биндас совпадает с типом члена, к которому он присоединен.

<a name="MT4172"></a>

### <a name="mt4172-the-registrar-cant-convert-from-native-type-to-managed-type-for-the-parameter-parameter-name-in-the-method-method"></a>MT4172: регистратор не может выполнить преобразование из "{native Type}" в "{Managed Type}" для параметра "{имя параметра}" в методе {Method}.

Регистратор не поддерживает преобразование указанных типов.

Это ошибка в Xamarin. iOS, если рассматриваемый API предоставляется Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

При выполнении этого при разработке проекта привязки для собственной библиотеки мы относимся к добавлению поддержки новых сочетаний типов. Если это так, отправьте запрос на расширение на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) с тестовым случаем, чтобы оценить его.

## <a name="mt5xxx-gcc-and-toolchain-error-messages"></a>MT5xxx: сообщения об ошибках GCC и цепочки инструментов

### <a name="mt51xx-compilation"></a>MT51xx: компиляция

<!--
 MT5xxx GCC and toolchain
  MT51xx compilation
  -->

<a name="MT5101"></a>

### <a name="mt5101-missing--compiler-please-install-xcode-command-line-tools-component"></a>MT5101: отсутствует компилятор "*". Установите компонент Xcode "средства командной строки".

<a name="MT5102"></a>

### <a name="mt5102-failed-to-assemble-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5102: не удалось собрать файл "*". Отправляйте отчет об ошибках по адресуhttp://bugzilla.xamarin.com

<a name="MT5103"></a>

### <a name="mt5103-failed-to-compile-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5103: не удалось скомпилировать файл "*". Отправляйте отчет об ошибках по адресуhttp://bugzilla.xamarin.com

<a name="MT5104"></a>

### <a name="mt5104-could-not-find-neither-the--nor-the--compiler-please-install-xcode-command-line-tools-component"></a>MT5104: не удалось найти ни " \* ", ни компилятор "" \* . Установите компонент Xcode "средства командной строки".

<!-- 5105 is used by mmp -->

<a name="MT5106"></a>

### <a name="mt5106-could-not-compile-the-files--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5106: не удалось скомпилировать файлы "*". Отправляйте отчет об ошибках по адресуhttp://bugzilla.xamarin.com

Обычно это указывает на ошибку в Xamarin. iOS; Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

### <a name="mt52xx-linking"></a>MT52xx: связывание

<!--
  MT52xx linking
  -->

<a name="MT5201"></a>

### <a name="mt5201-native-linking-failed-please-review-the-build-log-and-the-user-flags-provided-to-gcc-"></a>MT5201: не удалось выполнить собственную компоновку. Ознакомьтесь с журналом сборки и пользовательскими флагами, предоставленными в GCC: *

<a name="MT5202"></a>

### <a name="mt5202-native-linking-failed-please-review-the-build-log"></a>MT5202: не удалось выполнить собственную компоновку. Проверьте журнал сборки.

<a name="MT5203"></a>

### <a name="mt5203-native-linking-warning-"></a>MT5203: предупреждение собственной компоновки: *

<!--- 5204-5208 are not used -->

<a name="MT5209"></a>

### <a name="mt5209-native-linking-error-"></a>MT5209: ошибка собственной привязки: *

<a name="MT5210"></a>

### <a name="mt5210-native-linking-failed-undefined-symbol--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-are-properly-linked-in"></a>MT5210: не удалось выполнить собственную компоновку, неопределенный символ: *. Убедитесь, что имеются ссылки на все необходимые платформы, а собственные библиотеки правильно связаны в.

Это происходит, когда машинному компоновщику не удается найти символ, на который имеется ссылка. Это может произойти по нескольким причинам.

- Для привязки к сторонним разработчикам требуется платформа, но привязка не указывает это в `[LinkWith]` атрибуте. Решения:
  - Если вы являетесь автором привязки стороннего разработчика или имеете доступ к ее источнику, измените атрибут привязки, `[LinkWith]` включив в него требуемую платформу:

    ```csharp
    [LinkWith ("mylib.a", Frameworks = "SystemConfiguration")]
    ```

  - Если вы не можете изменить привязку стороннего разработчика, вы можете вручную связать ее с требуемой платформой, передав `-gcc_flags '-framework SystemFramework'` `mtouch` (это можно сделать, изменив дополнительные аргументы mtouch на странице параметров сборки iOS проекта. Помните, что это необходимо сделать для каждой конфигурации проекта.
- В некоторых случаях управляемая Привязка состоит из нескольких собственных библиотек, и все они должны быть добавлены в привязки. В каждом проекте привязки можно использовать несколько собственных библиотек, поэтому решение заключается в добавлении всех необходимых собственных библиотек в проект привязки.</li>
- Управляемая привязка относится к собственным символам, которые не существуют в собственной библиотеке.
    Обычно это происходит, когда привязка существовала некоторое время, а машинный код был изменен в течение этого времени, чтобы определенный собственный класс был либо удален, либо переименован, пока привязка не была обновлена.
- P/Invoke ссылается на несуществующий машинный символ. Начиная с Xamarin. iOS 7,4 в этом случае будет выводиться сообщение об ошибке <a href="#MT5214">MT5214</a> (Дополнительные сведения см. в разделе MT5214).
- Привязка или библиотека стороннего разработчика была построена с помощью C++, но привязка не указывает это в `[LinkWith]` атрибуте. Обычно это довольно легко понять, поскольку символы имеют искаженные символы C++ (один распространенный пример — `__ZNKSt9exception4whatEv` ).
  - Если вы являетесь автором привязки стороннего разработчика или имеете доступ к ее источнику, измените атрибут привязки, `[LinkWith]` чтобы установить `IsCxx` флаг:

    ```csharp
    [LinkWith ("mylib.a", IsCxx = true)]
    ```

  - Если не удается изменить привязку стороннего разработчика или связывание с библиотекой стороннего разработчика вручную, можно установить эквивалентный флаг, передав ему значение `-cxx` mtouch (это делается путем изменения дополнительных аргументов mtouch на странице параметров сборки iOS проекта. Помните, что это необходимо сделать для каждой конфигурации проекта.

<a name="MT5211"></a>

### <a name="mt5211-native-linking-failed-undefined-objective-c-class--the-symbol--could-not-be-found-in-any-of-the-libraries-or-frameworks-linked-with-your-application"></a>MT5211: не удалось выполнить собственную компоновку, неопределенный класс цели-C: \* . \*Не удалось найти символ "" ни в одной из библиотек или платформ, связанных с вашим приложением.

Это происходит, когда машинному компоновщику не удается найти класс цели-C, на который есть ссылка в другом месте. Это может произойти по нескольким причинам: для [MT5210](#MT5210) и в дополнение:

- Привязка третьей стороны привязана к протоколу цели-C, но не помечена `[Protocol]` атрибутом в его определении API. Решения:
  - Добавьте атрибут Missing `[Protocol]` :

    ```csharp
    [BaseType (typeof (NSObject))]
    [Protocol] // Add this
    public interface MyProtocol
    {
    }
    ```

<a name="MT5212"></a>

### <a name="mt5212-native-linking-failed-duplicate-symbol-"></a>MT5212: не удалось выполнить собственную компоновку, дублирующийся символ: *.

Это происходит, когда машинный Компоновщик обнаруживает дубликаты символов между всеми собственными библиотеками. После этой ошибки может быть одна или несколько ошибок [MT5213](#MT5213) с расположением для каждого вхождения символа. Возможные причины этой ошибки:

- Одна и та же собственная библиотека включена дважды.
- Для определения одних и тех же символов выполняются две различные собственные библиотеки.
- Собственная библиотека неверно построена и содержит один и тот же символ более одного раза.
  Это можно проверить с помощью следующего набора команд в терминале (замените i386 на x86_64/ARMv7/armv7s/arm64 в соответствии с архитектурой, для которой вы собираетесь создать):

  ```
  # Native libraries are usually fat libraries, containing binary code for
  # several architectures in the same file. First we extract the binary
  # code for the architecture we're interested in.
  lipo libNative.a -thin i386 -output libNative.i386.a

  # Now query the native library for the duplicated symbol.
  nm libNative.i386.a | fgrep 'SYMBOL'

  # You can also list the object files inside the native library.
  # In most cases this will reveal duplicated object files.
  ar -t libNative.i386.a
  ```

  Устранить эту проблему можно несколькими способами:

  - Попросите поставщика собственной библиотеки исправить ее и предоставить обновленную версию.
  - Устраните проблему самостоятельно, удалив лишние объектные файлы (это работает только в том случае, если проблема связана с повторяющимися объектами объектов).

  ```
  # Find out if the library is a fat library, and which
  # architectures it contains.
  lipo -info libNative.a

  # Extract each architecture (i386/x86_64/armv7/armv7s/arm64) to a separate file
  lipo libNative.a -thin ARCH -output libNative.ARCH.a

  # Extract the object files for the offending architecture
  # This will remove the duplicates by overwriting them
  # (since they have the same filename)
  mkdir -p ARCH
  cd ARCH
  ar -x ../libNative.ARCH.a

  # Reassemble the object files in an .a
  ar -r ../libNative.ARCH.a *.o
  cd ..

  # Reassemble the fat library
  lipo *.a -create -output libNative.a
  ```

  - Попросите компоновщика удалить неиспользуемый код. Xamarin. iOS сделает это автоматически при соблюдении всех следующих условий:
    - Все атрибуты привязок сторонних разработчиков `[LinkWith]` включили смартлинк:

      ```csharp
      [assembly: LinkWith ("libNative.a", SmartLink = true)]
      ```

    - Нет `-gcc_flags` , передается в mtouch (в поле дополнительных аргументов mtouch в параметрах сборки iOS проекта).
    - Также можно попросить компоновщика удалить неиспользуемый код, добавив `-gcc_flags -dead_strip` Дополнительные аргументы mtouch в параметры сборки iOS проекта.

<a name="MT5213"></a>

### <a name="mt5213-duplicate-symbol-in--location-related-to-previous-error"></a>MT5213: Дублирование символа в: * (расположение, связанное с предыдущей ошибкой)

Эта ошибка сообщается только вместе с [MT5212](#MT5212). Дополнительные сведения см. в разделе [MT5212](#MT5212) .

<a name="MT5214"></a>

### <a name="mt5214-native-linking-failed-undefined-symbol--this-symbol-was-referenced-the-managed-member--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-linked"></a>MT5214: не удалось выполнить собственную компоновку, неопределенный символ: *. На этот символ был ссылка на управляемый член *. Убедитесь, что имеются ссылки на все необходимые платформы и что они связаны с собственными библиотеками.

Эта ошибка возникает, когда управляемый код содержит P/Invoke для собственного метода, который не существует. Пример.

```csharp
using System.Runtime.InteropServices;
class MyImports {
   [DllImport ("__Internal")]
   public static extern void MyInexistentFunc ();
}
```

Существует несколько возможных решений.

- Удалите из исходного кода заданный вопрос P/Invoke.
- Включите управляемый компоновщик для всех сборок (это можно сделать в параметрах сборки iOS проекта, задав для параметра "поведение компоновщика" значение "все сборки"). Это приведет к удалению всех вызовов P/Invoke, которые не используются в приложении (автоматически, а не вручную, как в предыдущей точке). Недостаток состоит в том, что это сделает симулятор более медленным и может нарушить работу приложения, если оно использует отражение. Дополнительные сведения о компоновщике можно найти [здесь](~/ios/deploy-test/linker.md) .)
- Создайте вторую собственную библиотеку, которая содержит отсутствующие собственные символы. Обратите внимание, что это просто решение (если вы попытаетесь вызвать эти функции, произойдет сбой приложения).

<a name="MT5215"></a>

### <a name="mt5215-references-to--might-require-additional--frameworkxxx-or--lxxx-instructions-to-the-native-linker"></a>MT5215: для ссылок на "*" могут потребоваться дополнительные инструкции-Framework = XXX или-Лкскскс для собственного компоновщика

Это предупреждение означает, что был обнаружен вызов P/Invoke для ссылки на рассматриваемую библиотеку, но приложение не связывается с ним.

<a name="MT5216"></a>

### <a name="mt5216-native-linking-failed-for--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5216: не удалось выполнить собственную компоновку для *. Отправляйте отчет об ошибках по адресуhttp://bugzilla.xamarin.com

Эта ошибка сообщается при связывании выходных данных компилятора AOT.

Эта ошибка, скорее всего, указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT5217"></a>

### <a name="mt5217-native-linking-possibly-failed-because-the-linker-command-line-was-too-long--characters"></a>MT5217: возможно, не удалось выполнить собственную компоновку, так как Командная строка компоновщика слишком длинная (* символов).

Не удалось выполнить собственную компоновку, так как команда компоновщика слишком длинна.

Проекты Xamarin. iOS часто ссылаются на собственные символы динамически, что означает, что собственный Компоновщик может удалить такие собственные символы во время процесса компоновки, поскольку собственный компоновщик не увидит, что эти символы используются.

Как правило, Xamarin. iOS запрашивает у собственного компоновщика возможность сохранения таких символов с помощью `-u symbol` флага компоновщика, но если имеется много таких символов, вся Командная строка может превысить максимальную длину командной строки, указанную операционной системой.

Существует несколько возможных источников для таких динамических символов:

- P/вызывает методы в статически связанных библиотеках (где имя библиотеки DLL находится `__Internal` в атрибуте dllimport `[DllImport ("__Internal")]` ).
- Поле ссылается на места в памяти в статически связанных библиотеках из привязок проектов ( `[Field]` атрибутов).
- Классы цели-C, упоминаемые в статически связанных библиотеках, из привязки проектов (при использовании добавочных сборок или при отсутствии статического регистратора).

Возможные решения

- Включите управляемый компоновщик (если это возможно для всех сборок, а не только сборок пакета SDK). Это может удалить достаточное количество источников для динамических символов, чтобы длина командной строки компоновщика не превышала максимальное значение.
- Сократите число вызовов P/Invoke, ссылки на поля и/или целевые классы C.
- Перепишите динамические символы, указав более короткие имена.
- Передайте в `-dlsym:false` качестве дополнительного аргумента mtouch в параметрах сборки iOS проекта. В этом случае Xamarin. iOS создает собственную ссылку в скомпилированном AOT коде и не требует от компоновщика сохранения этого символа. Однако это работает только для сборок устройств, и это приведет к ошибкам компоновщика, если есть P/Invoke для функций, которые не существуют в статической библиотеке.
- Передайте в `--dynamic-symbol-mode=code` качестве дополнительных аргументов mtouch в параметрах сборки iOS проекта. С помощью этого параметра Xamarin. iOS создаст дополнительный машинный код, который ссылается на эти символы, вместо того чтобы запрашивать собственный компоновщик для сохранения этих символов с помощью аргументов командной строки. Недостаток этого подхода заключается в том, что он увеличит размер исполняемого файла немного.
- Включите статический регистратор, передав `--registrar:static` в качестве дополнительного аргумента mtouch в параметрах сборки iOS проекта (для сборок имитатора, так как статический регистратор уже используется по умолчанию для сборок устройств). Статический регистратор создаст код, который ссылается на классы цели-C статически, поэтому нет необходимости запрашивать собственный компоновщик для сохранения таких классов.
- Отключить добавочные сборки (для сборок устройств). Если добавочные сборки включены, собственный компоновщик не будет рассматривать код, созданный статическим регистратором. Это означает, что Xamarin. iOS по-прежнему должен попросить компоновщика сохранить упоминаемые классы цели-C. Поэтому отключение добавочных сборок не помешает этой необходимости.

<a name="MT5218"></a>

### <a name="mt5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MT5218: не удается проигнорировать динамический символ {symbol} (--Ignore-Dynamic-Symbol = {Symbol}), так как он не был определен как динамический символ.

Аргумент командной строки `--ignore-dynamic-symbol=symbol` передан, но этот символ не является символом, который был распознан как динамический символ, который необходимо сохранить вручную.

Для этого существуют две основные причины.

- Неверное имя символа.
  - Не ставьте символ подчеркивания в начале имени символа.
  - Символом для классов цели-C является `OBJC_CLASS_$_<classname>` .
- Символ является правильным, но является символом, который уже сохранен обычным способом (некоторые параметры сборки приводят к тому, что точный список динамических символов изменяется).

### <a name="mt53xx-other-tools"></a>MT53xx: другие средства

<!--
  MT53xx other tools
  -->

<a name="MT5301"></a>

### <a name="mt5301-missing-strip-tool-please-install-xcode-command-line-tools-component"></a>MT5301: отсутствует инструмент "полоск". Установите компонент Xcode "средства командной строки".

<a name="MT5302"></a>

### <a name="mt5302-missing-dsymutil-tool-please-install-xcode-command-line-tools-component"></a>MT5302: отсутствует инструмент "дсимутил". Установите компонент Xcode "средства командной строки".

<a name="MT5303"></a>

### <a name="mt5303-failed-to-generate-the-debug-symbols-dsym-directory-please-review-the-build-log"></a>MT5303: не удалось создать отладочные символы (каталог dSYM). Проверьте журнал сборки.

Произошла ошибка при выполнении дсимутил в конечном каталоге. app для создания отладочных символов. Просмотрите журнал сборки, чтобы увидеть выходные данные дсимутил и посмотреть, как это можно исправить.

<a name="MT5304"></a>

### <a name="mt5304-failed-to-strip-the-final-binary-please-review-the-build-log"></a>MT5304: не удалось удалить конечный двоичный файл. Проверьте журнал сборки.

При запуске инструмента "полоса" для удаления отладочных данных из приложения произошла ошибка.

<a name="MT5305"></a>

### <a name="mt5305-missing-lipo-tool-please-install-xcode-command-line-tools-component"></a>MT5305: отсутствует инструмент "Липо". Установите компонент Xcode "средства командной строки".

<a name="MT5306"></a>

### <a name="mt5306-failed-to-create-the-a-fat-library-please-review-the-build-log"></a>MT5306: не удалось создать библиотеку FAT. Проверьте журнал сборки.

При запуске средства "Липо" произошла ошибка. Просмотрите журнал сборки, чтобы увидеть ошибку, сообщаемую "Липо".

<a name="MT5307"></a>

### <a name="mt5307-failed-to-sign-the-executable-please-review-the-build-log"></a>MT5307: не удалось подписать исполняемый файл. Проверьте журнал сборки.

При подписывании приложения произошла ошибка. Просмотрите журнал сборки, чтобы увидеть ошибку, о которой сообщило "соконструировать".

<!-- 5308 is used by mmp -->
<!-- 5309 is used by mmp -->
<!-- 5310 is used by mmp -->

## <a name="mt6xxx-mtouch-internal-tools-error-messages"></a>MT6xxx: сообщения об ошибках внутренних средств mtouch

### <a name="mt600x-stripper"></a>MT600x: формата

<!--
 MT6xxx mtouch internal tools
  MT600x Stripper
  -->

<a name="MT6001"></a>

### <a name="mt6001-running-version-of-cecil-doesnt-support-assembly-stripping"></a>MT6001: работающая версия Cecil не поддерживает удаление сборок

<a name="MT6002"></a>

### <a name="mt6002-could-not-strip-assembly-"></a>MT6002: не удалось удалить сборку `*` .

Произошла ошибка при удалении управляемого кода (удаление IL-кода) из сборок в приложении.

<a name="MT6003"></a>

### <a name="mt6003-unauthorizedaccessexception-message"></a>MT6003: [сообщение UnauthorizedAccessException]

Произошла ошибка безопасности при отбрасывания отладочных символов из приложения.

## <a name="mt7xxx-msbuild-error-messages"></a>MT7xxx: сообщения об ошибках MSBuild

<!--
 MT7xxx msbuild errors
  -->

<a name="MT7001"></a>

### <a name="mt7001-could-not-resolve-host-ips-for-wifi-debugger-settings"></a>MT7001: не удалось разрешить IP-адреса узла для параметров отладчика Wi-Fi.

*Задача MSBuild: Детектдебугнетворкконфигуратионтаскбасе*

Действия по устранению проблемы

- Попробуйте выполнить команду `csharp -e 'System.Net.Dns.GetHostEntry (System.Net.Dns.GetHostName ()).AddressList'` (которая выдаст вам IP-адрес, а не ошибку очевидно).
- Попробуйте выполнить команду "ping \` HostName \` ", которая может предоставить дополнительные сведения, например:`cannot resolve MyHost.local: Unknown host`

В некоторых случаях это "локальная сеть", и ее можно устранить, добавив неизвестный узел `127.0.0.1    MyHost.local` в `/etc/hosts` .

<a name="MT7002"></a>

### <a name="mt7002-this-machine-does-not-have-any-network-adapters-this-is-required-when-debugging-or-profiling-on-device-over-wifi"></a>MT7002: на этом компьютере нет сетевых адаптеров. Это необходимо при отладке или профилировании на устройстве через Wi-Fi.

*Задача MSBuild: Детектдебугнетворкконфигуратионтаскбасе*

<a name="MT7003"></a>

### <a name="mt7003-the-app-extension--does-not-contain-an-infoplist"></a>MT7003: расширение приложения "*" не содержит сведения. plist.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7004"></a>

### <a name="mt7004-the-app-extension--does-not-specify-a-cfbundleidentifier"></a>MT7004: расширение приложения "*" не указывает CFBundleIdentifier.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7005"></a>

### <a name="mt7005-the-app-extension--does-not-specify-a-cfbundleexecutable"></a>MT7005: расширение приложения "*" не указывает Кфбундликсекутабле.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7006"></a>

### <a name="mt7006-the-app-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7006: расширение приложения " \* " имеет недопустимое CFBundleIdentifier ( \* ), оно не начинается с главного CFBundleIdentifier пакета приложений (*).

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7007"></a>

### <a name="mt7007-the-app-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a>MT7007: у расширения приложения " \* " есть CFBundleIdentifier ( \* ), заканчивающиеся недопустимым суффиксом ". key".

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7008"></a>

### <a name="mt7008-the-app-extension--does-not-specify-a-cfbundleshortversionstring"></a>MT7008: расширение приложения "*" не указывает Кфбундлешортверсионстринг.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7009"></a>

### <a name="mt7009-the-app-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a>MT7009: расширение приложения "*" содержит недопустимые сведения. plist: оно не содержит словарь Нсекстенсион.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7010"></a>

### <a name="mt7010-the-app-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionpointidentifier-value"></a>MT7010: расширение приложения "*" содержит недопустимые сведения. plist: Словарь Нсекстенсион не содержит значение Нсекстенсионпоинтидентифиер.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7011"></a>

### <a name="mt7011-the-watchkit-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionattributes-dictionary"></a>MT7011: расширение WatchKit "*" содержит недопустимые сведения. plist: Словарь Нсекстенсион не содержит словарь Нсекстенсионаттрибутес.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7012"></a>

### <a name="mt7012-the-watchkit-extension--does-not-have-exactly-one-watch-app"></a>MT7012: у расширения WatchKit "*" нет ровно одного приложения наблюдения.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7013"></a>

### <a name="mt7013-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-must-contain-the-watch-companion-capability"></a>MT7013: расширение WatchKit "*" содержит недопустимые сведения. plist: Уирекуиреддевицекапабилитиес должен содержать возможность "Watch-компаньон".

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7014"></a>

### <a name="mt7014-the-watch-app--does-not-contain-an-infoplist"></a>MT7014: приложение Watch "*" не содержит сведения. plist.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7015"></a>

### <a name="mt7015-the-watch-app--does-not-specify-a-cfbundleidentifier"></a>MT7015: в приложении Watch "*" не указан CFBundleIdentifier.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7016"></a>

### <a name="mt7016-the-watch-app--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7016: приложение Watch " \* " имеет недопустимое CFBundleIdentifier ( \* ), оно не начинается с главного CFBundleIdentifier пакета приложений (*).

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7017"></a>

### <a name="mt7017-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected-watch-4-but-found--"></a>MT7017: приложение Watch " \* " не имеет допустимого значения уидевицефамили. Ожидалось "Watch (4)", но найдено " \* (*)".

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7018"></a>

### <a name="mt7018-the-watch-app--does-not-specify-a-cfbundleexecutable"></a>MT7018: в приложении Watch "*" не указан Кфбундликсекутабле

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7019"></a>

### <a name="mt7019-the-watch-app--has-an-invalid-wkcompanionappbundleidentifier-value--it-does-not-match-the-main-app-bundles-cfbundleidentifier-"></a>MT7019: приложение Watch " \* " имеет недопустимое значение вккомпанионаппбундлеидентифиер (" \* "), оно не соответствует CFBundleIdentifierу ("*") пакета главного приложения.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7020"></a>

### <a name="mt7020-the-watch-app--has-an-invalid-infoplist-the-wkwatchkitapp-key-must-be-present-and-have-a-value-of-true"></a>MT7020: приложение Watch "*" имеет недопустимые сведения. plist: ключ Вкватчкитапп должен быть указан и иметь значение "true".

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7021"></a>

### <a name="mt7021-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a>MT7021: приложение Watch "*" имеет недопустимое значение info. plist: ключ Лсрекуиресифонеос не должен присутствовать.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7022"></a>

### <a name="mt7022-the-watch-app--does-not-contain-a-watch-extension"></a>MT7022: приложение Watch "*" не содержит расширение Watch.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7023"></a>

### <a name="mt7023-the-watch-extension--does-not-contain-an-infoplist"></a>MT7023: расширение Watch "*" не содержит сведения. plist.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7024"></a>

### <a name="mt7024-the-watch-extension--does-not-specify-a-cfbundleidentifier"></a>MT7024: расширение Watch "*" не указывает CFBundleIdentifier.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7025"></a>

### <a name="mt7025-the-watch-extension--does-not-specify-a-cfbundleexecutable"></a>MT7025: расширение Watch "*" не указывает Кфбундликсекутабле.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7026"></a>

### <a name="mt7026-the-watch-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7026: расширение Watch " \* " имеет недопустимое CFBundleIdentifier ( \* ), оно не начинается с главного CFBundleIdentifier пакета приложений (*).

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7027"></a>

### <a name="mt7027-the-watch-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a>MT7027: у расширения Watch " \* " есть CFBundleIdentifier ( \* ), заканчивающиеся недопустимым суффиксом ". key".

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7028"></a>

### <a name="mt7028-the-watch-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a>MT7028: расширение Watch "*" содержит недопустимые сведения. plist: оно не содержит словарь Нсекстенсион.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7029"></a>

### <a name="mt7029-the-watch-extension--has-an-invalid-infoplist-the-nsextensionpointidentifier-must-be-comapplewatchkit"></a>MT7029: расширение Watch "*" содержит недопустимые сведения. plist: Нсекстенсионпоинтидентифиер должен иметь значение "com. Apple. watchkit".

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7030"></a>

### <a name="mt7030-the-watch-extension--has-an-invalid-infoplist-the-nsextension-dictionary-must-contain-nsextensionattributes"></a>MT7030: расширение Watch "*" содержит недопустимые сведения. plist: Словарь Нсекстенсион должен содержать Нсекстенсионаттрибутес.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7031"></a>

### <a name="mt7031-the-watch-extension--has-an-invalid-infoplist-the-nsextensionattributes-dictionary-must-contain-a-wkappbundleidentifier"></a>MT7031: расширение Watch "*" содержит недопустимые сведения. plist: Словарь Нсекстенсионаттрибутес должен содержать Вкаппбундлеидентифиер.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7032"></a>

### <a name="mt7032-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-should-not-contain-the-watch-companion-capability"></a>MT7032: расширение WatchKit "*" содержит недопустимые сведения. plist: Уирекуиреддевицекапабилитиес не должно содержать возможность "Просмотр-помощник".

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7033"></a>

### <a name="mt7033-the-watch-app--does-not-contain-an-infoplist"></a>MT7033: приложение Watch "*" не содержит сведения. plist.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7034"></a>

### <a name="mt7034-the-watch-app--does-not-specify-a-cfbundleidentifier"></a>MT7034: в приложении Watch "*" не указан CFBundleIdentifier.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7035"></a>

### <a name="mt7035-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected--but-found--"></a>MT7035: приложение Watch " \* " не имеет допустимого значения уидевицефамили. Ожидалось " \* ", но обнаружено " \* ( \* )".

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7036"></a>

### <a name="mt7036-the-watch-app--does-not-specify-a-cfbundleexecutable"></a>MT7036: в приложении Watch "*" не указан Кфбундликсекутабле.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7037"></a>

### <a name="mt7037-the-watchkit-extension-extensionname-has-an-invalid-wkappbundleidentifier-value--it-does-not-match-the-watch-apps-cfbundleidentifier-"></a>MT7037: расширение WatchKit "{extensionName}" имеет недопустимое значение Вкаппбундлеидентифиер (" \* "), оно не соответствует CFBundleIdentifier (" \* ") приложения Watch.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7038"></a>

### <a name="mt7038-the-watch-app--has-an-invalid-infoplist-the-wkcompanionappbundleidentifier-must-exist-and-must-match-the-main-app-bundles-cfbundleidentifier"></a>MT7038: приложение Watch "*" имеет недопустимое значение info. plist: Вккомпанионаппбундлеидентифиер должно существовать и соответствовать основному CFBundleIdentifier пакета приложений.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7039"></a>

### <a name="mt7039-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a>MT7039: приложение Watch "*" имеет недопустимое значение info. plist: ключ Лсрекуиресифонеос не должен присутствовать.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7040"></a>

### <a name="mt7040-the-app-bundle-appbundlepath-does-not-contain-an-infoplist"></a>MT7040: набор приложений {Аппбундлепас} не содержит info. plist.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7041"></a>

### <a name="mt7041-main-infoplist-path-does-not-specify-a-cfbundleidentifier"></a>MT7041: Основная информация. plist путь не указывает CFBundleIdentifier.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7042"></a>

### <a name="mt7042-main-infoplist-path-does-not-specify-a-cfbundleexecutable"></a>MT7042: Основная информация. plist путь не указывает Кфбундликсекутабле.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7043"></a>

### <a name="mt7043-main-infoplist-path-does-not-specify-a-cfbundlesupportedplatforms"></a>MT7043: Основная информация. plist путь не указывает Кфбундлесуппортедплатформс.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7044"></a>

### <a name="mt7044-main-infoplist-path-does-not-specify-a-uidevicefamily"></a>MT7044: Основная информация. plist путь не указывает Уидевицефамили.

*Задача MSBuild: Валидатеаппбундлетаскбасе*

<a name="MT7045"></a>

### <a name="mt7045-unrecognized-format-"></a>MT7045: нераспознанный формат: *.

*Задача MSBuild: Пропертилистедитортаскбасе*

Где * может быть:

- строка
- массиве
- dict
- bool
- real
- Целое число
- Дата
- .

<a name="MT7046"></a>

### <a name="mt7046-add-entry--incorrectly-specified"></a>MT7046: Add: Entry, *, указано неправильно.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7047"></a>

### <a name="mt7047-add-entry--contains-invalid-array-index"></a>MT7047: Add: Entry, *, содержит недопустимый индекс массива.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7048"></a>

### <a name="mt7048-add--entry-already-exists"></a>MT7048: Add: * запись уже существует.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7049"></a>

### <a name="mt7049-add-cant-add-entry--to-parent"></a>MT7049: Add: не удается добавить запись, *, в родительский элемент.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7050"></a>

### <a name="mt7050-delete-cant-delete-entry--from-parent"></a>MT7050: Delete: не удается удалить запись, *, из родительского элемента.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7051"></a>

### <a name="mt7051-delete-entry--contains-invalid-array-index"></a>MT7051: Delete: Entry, *, содержит недопустимый индекс массива.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7052"></a>

### <a name="mt7052-delete-entry--does-not-exist"></a>MT7052: Delete: запись, *, не существует.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7053"></a>

### <a name="mt7053-import-entry--incorrectly-specified"></a>MT7053: импорт: запись, *, неправильно указана.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7054"></a>

### <a name="mt7054-import-entry--contains-invalid-array-index"></a>MT7054: операция импорта: Entry, *, содержит недопустимый индекс массива.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7055"></a>

### <a name="mt7055-import-error-reading-file-"></a>MT7055: ошибка при чтении файла: *.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7056"></a>

### <a name="mt7056-import-cant-add-entry--to-parent"></a>MT7056: импорт: не удается добавить запись, *, в родительский элемент.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7057"></a>

### <a name="mt7057-merge-cant-add-array-entries-to-dict"></a>MT7057: Merge: не удается добавить элементы массива в словарь.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7058"></a>

### <a name="mt7058-merge-specified-entry-must-be-a-container"></a>MT7058: Merge: указанная запись должна быть контейнером.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7059"></a>

### <a name="mt7059-merge-entry--contains-invalid-array-index"></a>MT7059: Merge: Entry, *, содержит недопустимый индекс массива.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7060"></a>

### <a name="mt7060-merge-entry--does-not-exist"></a>MT7060: Merge: Entry, *, не существует.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7061"></a>

### <a name="mt7061-merge-error-reading-file-"></a>MT7061: Merge: ошибка при чтении файла: *.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7062"></a>

### <a name="mt7062-set-entry--incorrectly-specified"></a>MT7062: Set: Entry, *, указано неправильно.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7063"></a>

### <a name="mt7063-set-entry--contains-invalid-array-index"></a>MT7063: Set: Entry, *, содержит недопустимый индекс массива.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7064"></a>

### <a name="mt7064-set-entry--does-not-exist"></a>MT7064: Set: Entry, *, не существует.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7065"></a>

### <a name="mt7065-unknown-propertylist-editor-action-"></a>MT7065: неизвестное действие редактора PropertyList: *.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7066"></a>

### <a name="mt7066-error-loading--"></a>MT7066: ошибка при загрузке "*": *.

*Задача MSBuild: Пропертилистедитортаскбасе*

<a name="MT7067"></a>

### <a name="mt7067-error-saving--"></a>MT7067: ошибка при сохранении "*": *.

*Задача MSBuild: Пропертилистедитортаскбасе*

## <a name="mt8xxx-runtime-error-messages"></a>MT8xxx: сообщения об ошибках времени выполнения

<!--
 MT8xxx runtime
  MT800x misc
  -->

<a name="MT8001"></a>

### <a name="mt8001-version-mismatch-between-the-native-xamarinios-runtime-and-monotouchdll-please-reinstall-xamarinios"></a>MT8001: несоответствие версий собственной среды выполнения Xamarin. iOS и файла. dll. Переустановите Xamarin. iOS.

<a name="MT8002"></a>

### <a name="mt8002-could-not-find-the-method--in-the-type-"></a>MT8002: не удалось найти метод " \* " в типе " \* ".

<a name="MT8003"></a>

### <a name="mt8003-failed-to-find-the-closed-generic-method--on-the-type-"></a>MT8003: не удалось найти закрытый универсальный метод " \* " для типа " \* ".

<a name="MT8004"></a>

### <a name="mt8004-cannot-create-an-instance-of--for-the-native-object-0x-of-type--because-another-instance-already-exists-for-this-native-object-of-type-"></a>MT8004: невозможно создать экземпляр \* для собственного объекта 0x * (типа " \* "), так как для этого собственного объекта (типа) уже существует другой экземпляр \* .

<a name="MT8005"></a>

### <a name="mt8005-wrapper-type--is-missing-its-native-objectivec-class-"></a>MT8005: в типе оболочки " \* " отсутствует собственный класс ObjectiveC " \* ".

<a name="MT8006"></a>

### <a name="mt8006-failed-to-find-the-selector--on-the-type-"></a>MT8006: не удалось найти селектор " \* " для типа " \* "

<a name="MT8007"></a>

### <a name="mt8007-cannot-get-the-method-descriptor-for-the-selector--on-the-type--because-the-selector-does-not-correspond-to-a-method"></a>MT8007: не удается получить дескриптор метода для селектора " \* " для типа " \* ", так как селектор не соответствует методу

<a name="MT8008"></a>

### <a name="mt8008-the-loaded-version-of-xamariniosdll-was-compiled-for--bits-while-the-process-is--bits-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8008: загруженная версия Xamarin. iOS. dll была скомпилирована для \* BITS, а процесс — в \* битах. Отправляйте ошибку по адресу http://bugzilla.xamarin.com .

Это указывает на то, что в процессе сборки возникли проблемы. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8009"></a>

### <a name="mt8009-unable-to-locate-the-block-to-delegate-conversion-method-for-the-method-s-parameter--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8009: не удалось разместить блок для делегирования метода преобразования для метода *.*" s параметр # *. Отправляйте ошибку по адресу http://bugzilla.xamarin.com .

Это означает, что API не был правильно привязан. Если это API, предоставляемый Xamarin, напишите новую ошибку в [GitHub](https://github.com/xamarin/xamarin-macios/issues/new). Если это привязка третьей стороны, обратитесь к поставщику.

<a name="MT8010"></a>

### <a name="mt8010-native-type-size-mismatch-between-xamariniosmacdll-and-the-executing-architecture-xamariniosmacdll-was-built-for--bit-while-the-current-process-is--bit"></a>MT8010: несоответствие размера собственного типа Xamarin. [iOS | Mac]. dll и исполняющая архитектура. Xamarin. [iOS | Mac]. dll была создана для *-bit, а текущий процесс — *-bit.

Это указывает на то, что в процессе сборки возникли проблемы. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8011"></a>

### <a name="mt8011-unable-to-locate-the-delegate-to-block-conversion-attribute-delegateproxy-for-the-return-value-for-the-method--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8011: не удалось нахождение делегата для блокировки атрибута преобразования ([Делегатепрокси]) для возвращаемого значения метода *.* Отправляйте ошибку по адресу http://bugzilla.xamarin.com .

Xamarin. iOS не удалось разместить необходимый метод во время выполнения (для преобразования делегата в блок).

Обычно это указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8012"></a>

### <a name="mt8012-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8012: недопустимый Делегатепроксяттрибуте для возвращаемого значения метода *.*: делегатетипе имеет значение null. Отправляйте ошибку по адресу http://bugzilla.xamarin.com .

Недопустимый атрибут Делегатепрокси для рассматриваемого метода.

Обычно это указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8013"></a>

### <a name="mt8013-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-2-specifies-a-type-without-a-handler-field-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8013: недопустимый Делегатепроксяттрибуте для возвращаемого значения метода *.*: делегатетипе ( {2} ) указывает тип без поля "handler". Отправляйте ошибку по адресу http://bugzilla.xamarin.com .

`[DelegateProxy]`Недопустимый атрибут для рассматриваемого метода.

Обычно это указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8014"></a>

### <a name="mt8014-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8014: недопустимый Делегатепроксяттрибуте для возвращаемого значения метода *.*: {2} поле "handler" () "обработчика делегатетипе" имеет значение null. Отправляйте ошибку по адресу http://bugzilla.xamarin.com .

`[DelegateProxy]`Недопустимый атрибут для рассматриваемого метода.

Обычно это указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8015"></a>

### <a name="mt8015-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-not-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8015: недопустимый Делегатепроксяттрибуте для возвращаемого значения метода *.*: {2} поле "handler" () "обработчик" делегатетипе не является делегатом, а является *. Отправляйте ошибку по адресу http://bugzilla.xamarin.com .

Недопустимый атрибут Делегатепрокси для рассматриваемого метода.

Обычно это указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8016"></a>

### <a name="mt8016-unable-to-convert-delegate-to-block-for-the-return-value-for-the-method--because-the-input-isnt-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8016: не удалось преобразовать делегата в Block для возвращаемого значения для метода *.*, поскольку входные данные не являются делегатом, это *. Отправляйте ошибку по адресу http://bugzilla.xamarin.com .

`[DelegateProxy]`Недопустимый атрибут для рассматриваемого метода.

Обычно это указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<!-- 8017 is used by mmp -->

<a name="MT8018"></a>

### <a name="mt8018-internal-consistency-error-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8018: Внутренняя ошибка согласованности. Отправляйте отчет об ошибках по адресу http://bugzilla.xamarin.com .

Это указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8019"></a>

### <a name="mt8019-could-not-find-the-assembly--in-the-loaded-assemblies"></a>MT8019: не удалось найти сборку * в загруженных сборках.

Это указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8020"></a>

### <a name="mt8020-could-not-find-the-module-with-metadatatoken--in-the-assembly-"></a>MT8020: не удалось найти модуль с Метадататокен \* в сборке \* .

Это указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8021"></a>

### <a name="mt8021-unknown-implicit-token-type-"></a>MT8021: неизвестный тип неявного токена: *.

Это указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8022"></a>

### <a name="mt8022-expected-the-token-reference--to-be-a--but-its-a--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8022: ожидалась ссылка на маркер \* \* , а не значение \* . Отправляйте отчет об ошибках по адресу http://bugzilla.xamarin.com .

Это указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8023"></a>

### <a name="mt8023-an-instance-object-is-required-to-construct-a-closed-generic-method-for-the-open-generic-method--token-reference--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8023: объект экземпляра требуется для создания закрытого универсального метода для открытого универсального метода: \* (ссылка на маркер: \* ). Отправляйте отчет об ошибках по адресу http://bugzilla.xamarin.com .

Это указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8024"></a>

### <a name="mt8024-could-not-find-a-valid-extension-type-for-the-smart-enum-smart_type-please-file-a-bug-at-httpsbugzillaxamarincom"></a>MT8024: не удалось найти допустимый тип расширения для интеллектуального перечисления "{smart_type}". Отправляйте ошибку по адресу https://bugzilla.xamarin.com .

Это указывает на ошибку в Xamarin. iOS. Запишите новую ошибку на [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).
