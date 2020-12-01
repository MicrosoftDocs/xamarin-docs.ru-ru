---
title: Проектирование пользовательских интерфейсов с помощью Xcode
description: Узнайте о рекомендуемом способе создания пользовательских интерфейсов iOS непосредственно с помощью Xcode на компьютере Mac.
ms.prod: xamarin
ms.assetid: af9f95db-5cd6-475d-867d-f73e1574e8fc
ms.technology: xamarin-ios
author: joshspicer
ms.author: jospicer
ms.date: 10/27/2020
ms.openlocfilehash: b25245250bd9ea17034ea8f6b11388a12fc13241
ms.sourcegitcommit: d1f0e0a9100548cfe0960ed2225b979cc1d7c28f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/01/2020
ms.locfileid: "96439489"
---
# <a name="designing-user-interfaces-with-xcode"></a>Проектирование пользовательских интерфейсов с помощью Xcode

Начиная с Visual Studio 2019 версии 16,8 и Visual Studio для Mac версии 8,8, рекомендуемым способом изменения файлов. Storyboard и. NIB является изменение их в Xcode Interface Builder на компьютере Mac.

> [!NOTE]
> Начиная с Visual Studio 2019 версии 16,9, не существует поддерживаемого способа изменения раскадровок iOS в Windows. Используйте Visual Studio для Mac и Interface Builder Xcode, чтобы продолжить создание пользовательских интерфейсов Xamarin. iOS.

В этой статье рассматриваются распространенные решения для создания пользовательских интерфейсов с помощью Interface Builder Xcode.  Эта статья может быть особенно полезной, если вы ранее редактировали пользовательский интерфейс с помощью конструктора Xamarin. iOS. 

Более подробное пошаговое руководство по раскадровкам см. [в разделе раскадровки в Xamarin. iOS](./indepth-storyboard.md).

## <a name="how-to-open-a-storyboard"></a>Как Открыть раскадровку 

Откройте файл пользовательского интерфейса iOS в Visual Studio для Mac, щелкнув файл раскадровки правой кнопкой мыши и выбрав пункт **Xcode Interface Builder**:

[![Выберите Interface Builder](images/select-interface-builder.png)](images/select-interface-builder.png#lightbox)

После этого откроется окно Xcode. Все сохраненные здесь изменения будут отражены в проекте Visual Studio.

[![Окно Xcode](images/xcode.png)](images/xcode.png#lightbox)

Дополнительные сведения о Interface Builder Xcode см. в разделе [Interface Builder встроенные](https://developer.apple.com/xcode/interface-builder/).

## <a name="creating-a-new-control"></a>Создание нового элемента управления

Чтобы создать новый элемент управления с Interface Builder Xcode, сначала выберите раскадровку, которую вы хотите изменить. Затем откройте диалоговое окно Библиотека Xcode (**представление**  >  **Показать библиотеку**) и перетащите элемент управления в раскадровку.

[![Средство выбора библиотеки](images/library-picker.png)](images/library-picker.png#lightbox)

Затем откройте соответствующий файл заголовка контроллера представления.  Для пустого приложения Xamarin. iOS с одним представлением Раскадровка по умолчанию называется **Main. Storyboard**. Соответствующий файл контроллера представления называется **ViewController.CS** в Visual Studio с соответствующим файлом заголовка **ViewController. h** при просмотре из Xcode.

В Interface Builder Xcode откройте и раскадровку, и соответствующий файл заголовка контроллера представления.  Помещая **управляющую клавишу** ( **^** ), перетащите элемент управления из раскадровки в файл контроллера представления, пока Xcode не запросит диалоговое окно.

[![Демонстрационный элемент управления Link](images/demo-link-control.gif)](images/demo-link-control.gif#lightbox)

Как показано выше, соответствующий код C# будет автоматически создан в файле кода программной части контроллера представления.  Теперь вы можете получить доступ к этому элементу управления в проекте Xamarin. iOS.

## <a name="editing-an-existing-controls-name"></a>Изменение имени существующего элемента управления

Чтобы изменить имя существующего элемента управления из Interface Builder Xcode и отразить это изменение обратно в проект C#, перейдите к соответствующему файлу заголовка контроллера представления, щелкните правой кнопкой мыши выберите команду и выберите **Рефакторинг**.   

[![Элемент управления рефакторинг](images/refactor-control.png)](images/refactor-control.png#lightbox)

Файл кода программной части будет повторно создан с новым именем, что позволит получить доступ к элементу управления с помощью кода в Visual Studio для Mac.

## <a name="known-problems"></a>Известные проблемы

В этом разделе рассматриваются известные проблемы.

### <a name="visual-studio-could-not-communicate-with-xcode"></a>"Visual Studio не удалось связаться с Xcode"

В macOS Catalina или более поздней версии может возникнуть следующая ошибка:

[![не удается установить связь с ERR](images/could-not-communicate.png)](images/could-not-communicate.png#lightbox)

Во-первых, в системных настройках компьютера Mac в разделе **безопасность & конфиденциальность > Автоматизация** убедитесь, что Visual Studio отображается и **Xcode** установлен.

[![безопасность macOS](images/macos-security.png)](images/macos-security.png#lightbox)

Если **Xcode** установлен и сообщение об ошибке по-прежнему отображается, может потребоваться сбросить Visual Studio для Mac разрешений на конфиденциальность.

Это можно сделать, запустив окно терминала и выполнив следующую команду:

```bash
sudo tccutil reset All "com.microsoft.visual-studio"
```

Чтобы убедиться, что указанные выше изменения вступают в силу, сбросьте Прам компьютера Mac. Инструкции см. [в разделе Reset NVRAM или Прам на компьютере Mac](https://support.apple.com/HT204063).
