---
title: Работа с родительским приложением watchOS в Xamarin
description: В этом документе описывается работа с родительским приложением watchOS в Xamarin. В нем обсуждаются расширения приложений watchOS, приложения iOS, общее хранилище и многое другое.
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 002c57a1549201018cb2068f000a038f686eb2c0
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436737"
---
# <a name="working-with-the-watchos-parent-application-in-xamarin"></a>Работа с родительским приложением watchOS в Xamarin

Существует несколько способов обмена данными между приложением Watch и приложением iOS, с которым оно объединяется.

- Смотреть приложения могут [запускать код](#run-code) в родительском приложении на iPhone.

- Расширения Watch могут [совместно использовать расположение хранилища](#shared-storage) с родительским приложением iPhone.

- Используйте пересылку, чтобы передать данные из уведомления в приложение наблюдения, отправив пользователю конкретный контроллер интерфейса в приложении.

Кроме того, родительское приложение иногда называют приложением-контейнером.

## <a name="run-code"></a>Выполнение кода

Эти два примера демонстрируют, как использовать `WCSession` для запуска кода и отправки сообщений между приложением Watch и парным iPhone.

- [Просмотр подключения](/samples/xamarin/ios-samples/watchos-watchconnectivity/)
- [симплеватчконнективити](/samples/xamarin/ios-samples/watchos-simplewatchconnectivity/) 

## <a name="shared-storage"></a>Общее хранилище

Если вы настраиваете [группу приложений](~/ios/watchos/app-fundamentals/app-groups.md) , расширения iOS 8 (включая расширения контрольных значений) могут обмениваться данными с родительским приложением.

### <a name="nsuserdefaults"></a>нсусердефаултс

Следующий код можно записать как в расширении приложения Watch, так и в родительском приложении iPhone, чтобы они могли ссылаться на общий набор `NSUserDefaults` :

```csharp
NSUserDefaults shared = new NSUserDefaults(
        "group.com.your-company.watchstuff",
        NSUserDefaultsType.SuiteName);

// set values
shared.SetInt (2, "count");
shared.Synchronize ();

// get values
shared.Synchronize ();
var count = shared.IntForKey ("count");
```

<a name="files"></a>

### <a name="files"></a>Файлы

Приложение iOS и расширение Watch могут также использовать общие пути к файлам.

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

Примечание. Если путь, `null` Проверьте [конфигурацию группы приложений](~/ios/watchos/app-fundamentals/app-groups.md) , чтобы убедиться, что профили подготовки настроены правильно и были загружены и установлены на компьютере разработчика.

Дополнительные сведения см. в документации по [возможностям группы приложений](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) .

## <a name="related-links"></a>Связанные ссылки

- [Справочник по Вкинтерфацеконтроллер Apple](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Совместное использование данных Apple с содержащим приложением](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)