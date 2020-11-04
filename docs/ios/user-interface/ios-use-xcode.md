---
title: Использование Xcode для проектирования пользовательских интерфейсов
description: Рекомендуемый способ создания пользовательских интерфейсов iOS — непосредственно с помощью Xcode на компьютере Mac.
ms.prod: xamarin
ms.assetid: af9f95db-5cd6-475d-867d-f73e1574e8fc
ms.technology: xamarin-ios
author: joshspicer
ms.author: jospicer
ms.date: 10/27/2020
ms.openlocfilehash: b6fc5c89e9221bc84b364290a275fbd6d35c12c4
ms.sourcegitcommit: d2aa3a8bf9a60b6708db55b10b0c6893c06d3256
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/04/2020
ms.locfileid: "93343322"
---
# <a name="using-xcode-to-design-user-interfaces"></a>Использование Xcode для проектирования пользовательских интерфейсов

_Рекомендуемый способ изменения файлов. Storyboard и. NIB заключается в их редактировании в Xcode Interface Builder на компьютере Mac._

## <a name="how-to-open-a-storyboard"></a>Как Открыть раскадровку 

Откройте файл пользовательского интерфейса iOS в Visual Studio для Mac, щелкнув файл раскадровки правой кнопкой мыши и выбрав пункт **Xcode Interface Builder** :

[![Выберите Interface Builder](images/select-interface-builder.png)](images/select-interface-builder.png#lightbox)

После этого появится окно Xcode Launch (запуск окна). Все сохраненные здесь изменения будут отражены в проекте Visual Studio.

[![Окно Xcode](images/xcode.png)](images/xcode.png#lightbox)

Дополнительные сведения о Interface Builder Xcode можно найти [здесь](https://developer.apple.com/xcode/interface-builder/).

## <a name="known-problems"></a>Известные проблемы

В этом разделе рассматриваются известные проблемы.

### <a name="visual-studio-could-not-communicate-with-xcode"></a>"Visual Studio не удалось связаться с Xcode"

В macOS Catalina или более поздней версии может возникнуть следующая ошибка:

[![не удается установить связь с ERR](images/could-not-communicate.png)](images/could-not-communicate.png#lightbox)

Сначала убедитесь в том, что в настройках системы компьютера Mac в разделе **безопасность & конфиденциальность > Автоматизация** , в которой указана Visual Studio и **Xcode** установлен.

[![безопасность macOS](images/macos-security.png)](images/macos-security.png#lightbox)

Если **Xcode** установлен и сообщение об ошибке по-прежнему отображается, может потребоваться сбросить Visual Studio для Mac разрешений на конфиденциальность.

Это можно сделать, запустив окно терминала и выполнив команду:

```bash
sudo tccutil reset All "com.microsoft.visual-studio"
```

Чтобы убедиться, что указанные выше изменения вступают в силу, сбросьте Прам компьютера Mac. Инструкции о том, как это сделать, можно найти [здесь](https://support.apple.com/HT204063).
