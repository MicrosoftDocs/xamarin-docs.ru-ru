---
title: Работа с параметрами watchOS в Xamarin
description: В этом документе описывается работа с параметрами watchOS в Xamarin. В нем обсуждается добавление параметров в решение для наблюдения за приложениями, использование этих параметров в приложении и Apple Watchное приложение на устройстве iPhone.
ms.prod: xamarin
ms.assetid: 4B2EB192-F0A2-4010-B141-0431520594C0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 7a3839cb7c747879f1549fb8f6715fded62d45bb
ms.sourcegitcommit: 513feb0e07558766e3de4a898e53d56b27c20559
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2021
ms.locfileid: "98697726"
---
# <a name="working-with-watchos-settings-in-xamarin"></a>Работа с параметрами watchOS в Xamarin

Apple Watch приложения могут использовать те же функции параметров, что и приложения iOS. Пользовательский интерфейс параметров отображается в **Apple Watchном** приложении iPhone, но значения доступны в приложении iPhone, а также в расширении.

![Apple Watch приложения могут использовать те же функции параметров, что и приложения iOS](settings-images/intro.png)

Параметры будут храниться в общем расположении файлов, доступном как для приложения iOS, так и для расширения приложения для наблюдения, определенного **группой приложений**. Перед добавлением параметров необходимо [настроить группу приложений](~/ios/watchos/app-fundamentals/app-groups.md) , выполнив приведенные ниже инструкции.

## <a name="add-settings-in-a-watch-solution"></a>Добавление параметров в решение "контрольные значения"

В **приложении iPhone** в решении (а *не* на контрольном приложении или расширении):

1. Щелкните правой кнопкой мыши **добавить > новый файл...** и выберите **Settings. пучок** (имя нельзя изменить в диалоговом окне " **Создание файла** "):

   [![Добавить новый пакет параметров](settings-images/settings-add-sml.png)](settings-images/settings-add.png#lightbox)

2. Измените имя на **Settings-Watch. пучок** (выберите и введите **Command + R** для переименования):

   ![Переименование пакета](settings-images/settings-rename.png)

3. Добавьте новый ключ в `ApplicationGroupContainerIdentifier` **корневую папку. plist** со значением, заданным для настроенной вами группы приложений (например, `group.com.xamarin.WatchSettings` в примере):

   [![Добавьте ключ Аппликатионграупконтаинеридентифиер в корневую папку. plist](settings-images/settings-appgroup-sml.png)](settings-images/settings-appgroup.png#lightbox)

4. Измените **Сеттингс-ватч. пучок/root. plist** , чтобы они содержали параметры, которые вы хотите использовать. файл шаблона содержит группу.
  TextField, выключатель и ползунок по умолчанию (которые можно удалить и заменить собственными параметрами):

  [![Измените Сеттингс-ватч. пучок/root. plist](settings-images/rootplist-sml.png)](settings-images/rootplist.png#lightbox)

## <a name="use-settings-in-the-watch-app"></a>Использование параметров в приложении для просмотра контрольных значений

Чтобы получить доступ к значениям, выбранным пользователем, создайте `NSUserDefaults` экземпляр с помощью группы приложений и укажите `NSUserDefaultsType.SuiteName` :

```csharp
NSUserDefaults shared = new NSUserDefaults(
    "group.com.xamarin.WatchSettings",
    NSUserDefaultsType.SuiteName);

var isEnabled = shared.BoolForKey ("enabled_preference");
var userName = shared.StringForKey ("name_preference");
```

## <a name="apple-watch-app"></a>Приложение Apple Watch

[![Новое приложение Apple Watch на устройстве iPhone](settings-images/settings-app-sml.png)](settings-images/settings-app.png#lightbox)

Пользователи будут взаимодействовать с параметрами через новое приложение **Apple Watch** на iPhone. Это приложение позволяет пользователю отображать или скрывать приложения в контрольном списке, а также изменять параметры, предоставляемые с помощью **параметров-Watch. пучок**.

![Снимок экрана показывает Ватчкитсеттингс в приложении.](settings-images/applewatch-1.png) ![Снимок экрана показывает Ватчтодо в приложении.](settings-images/applewatch-2.png)

## <a name="related-links"></a>Связанные ссылки

- [Документ параметров Apple](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Settings.html#//apple_ref/doc/uid/TP40014969-CH22-SW1)
