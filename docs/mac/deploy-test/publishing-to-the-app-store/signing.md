---
title: Подписывание приложений Xamarin.Mac с помощью идентификатора разработчика
description: Этот документ описывает подпись приложения Xamarin.Mac с помощью идентификатора разработчика, чтобы его можно было распространять за пределами Mac App Store. В нем описаны параметры подписи кода и сборка.
ms.prod: xamarin
ms.assetid: cf7b733b-e08f-4f56-a233-264b29ee4c97
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 7e2d676b577e835bafe5cdd8d6bc48229a1a1a06
ms.sourcegitcommit: 513feb0e07558766e3de4a898e53d56b27c20559
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2021
ms.locfileid: "98697583"
---
# <a name="signing-xamarinmac-apps-with-a-developer-id"></a>Подписывание приложений Xamarin.Mac с помощью идентификатора разработчика

Если приложение планируется распространять напрямую пользователям macOS, компания Apple рекомендует подписать его код с помощью идентификатора разработчика, чтобы устанавливать в системах macOS с включенным **привратником**. Если приложение не было подписано, **привратник** запретит его устанавливать, выводя предупреждение (пользователи могут обойти это ограничение, удерживая нажатой клавишу CTRL при запуске).

Дополнительные сведения об [идентификаторе разработчика и привратнике](https://developer.apple.com/developer-id/) и [распространении за пределами Mac App Store](https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html) см. на веб-сайте Apple.

## <a name="code-signing-options"></a>Параметры подписывания кода

Чтобы создать приложение, напрямую развертываемое для пользователей (а не через Mac App Store), в **параметрах подписывания** используйте значение **Идентификатор разработчика**. Параметр "Конфигурация" должен иметь значение **Выпуск**.

 [![Параметры подписывания Mac](signing-images/config02.png)](signing-images/config02.png#lightbox)

## <a name="build"></a>Построение

Перед выполнением сборки убедитесь, что выбрана правильная конфигурация, и в окне **Сборка Mac** выберите параметр создания пакета установки.

[![Параметры сборки](signing-images/config03.png)](signing-images/config03.png#lightbox)

При сборке приложения выводится предложение об использовании обоих сертификатов:

 [![Снимок экрана: диалоговое окно разрешения доступа для codesign.](signing-images/image57.png)](signing-images/image57.png#lightbox)

 [![Снимок экрана: диалоговое окно разрешения доступа для productbuild.](signing-images/image58.png)](signing-images/image58.png#lightbox)

После сборки приложения щелкните проект правой кнопкой мыши и выберите команду **Открыть содержащую папку**, чтобы найти файл пакета (в каталоге `bin/Release`). В этом файле содержится установщик для приложения, поэтому его можно распространять любому пользователю macOS для установки.

 [![Выбор пакета приложения в Finder](signing-images/image59.png)](signing-images/image59.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [Установка](~//mac/get-started/installation.md)
- [Пример кода приложения "Привет, Mac"](~//mac/get-started/hello-mac.md)
- [Распространение приложений в Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Руководство по работе с инструментами. Подписывание кода приложения](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Идентификатор разработчика и привратник](https://developer.apple.com/developer-id/)
