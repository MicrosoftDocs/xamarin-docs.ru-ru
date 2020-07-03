---
title: Пошаговое руководство. привязка библиотеки цели iOS-C
description: В этой статье приводятся пошаговые инструкции по созданию привязки Xamarin. iOS для существующей библиотеки цели-C, Инфколорпиккер. В нем рассматриваются такие темы, как компиляция статической библиотеки цели-C, ее привязка и использование привязки в приложении Xamarin. iOS.
ms.prod: xamarin
ms.assetid: D3F6FFA0-3C4B-4969-9B83-B6020B522F57
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/02/2017
ms.openlocfilehash: c36159984f314ecbf90f98df6472eee2149eee92
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853162"
---
# <a name="walkthrough-binding-an-ios-objective-c-library"></a>Пошаговое руководство. привязка библиотеки цели iOS-C

> [!IMPORTANT]
> Сейчас мы изучаем использование пользовательской привязки на платформе Xamarin. Примите участие в [**опросе**](https://www.surveymonkey.com/r/KKBHNLT) , чтобы проинформировать будущие усилия по разработке.

_В этой статье приводятся пошаговые инструкции по созданию привязки Xamarin. iOS для существующей библиотеки цели-C, Инфколорпиккер. В нем рассматриваются такие темы, как компиляция статической библиотеки цели-C, ее привязка и использование привязки в приложении Xamarin. iOS._

При работе с iOS можно столкнуться с ситуациями, когда требуется использовать сторонние библиотеки цели-C. В таких ситуациях можно использовать _проект привязки_ Xamarin. iOS для создания [привязки C#](~/cross-platform/macios/binding/overview.md) , которая позволит использовать библиотеку в приложениях Xamarin. iOS.

Как правило, в экосистеме iOS библиотеки можно найти в трех разновидностях:

- Как предварительно скомпилированный файл статической библиотеки с `.a` расширением вместе со своими заголовками (h-файлами). Например, [Библиотека аналитики Google](https://developers.google.com/analytics/devguides/collection/ios/v3/sdk-download?hl=es#download_sdk)
- Как предкомпилированную платформу. Это просто папка, содержащая статическую библиотеку, заголовки и иногда дополнительные ресурсы с `.framework` расширением. Например, [Библиотека AdMob от Google](https://developers.google.com/admob/ios/download).
- Как просто файлы исходного кода. Например, Библиотека, содержащая только `.m` `.h` файлы C и цель с.

В первом и втором сценариях уже будет предкомпилированная статическая библиотека Кокоатауч, поэтому в этой статье мы рассмотрим третий сценарий. Помните, что перед началом создания привязки всегда проверяйте лицензию, поставляемую с библиотекой, чтобы убедиться, что ее можно привязать.

В этой статье приводятся пошаговые инструкции по созданию проекта привязки с использованием проекта [инфколорпиккер](https://github.com/InfinitApps/InfColorPicker) цели-c с открытым исходным кодом в качестве примера. Однако все сведения в этом руководстве можно адаптировать для использования со сторонними библиотеками цели-c. Библиотека Инфколорпиккер предоставляет доступный для повторного использования контроллер представлений, позволяющий пользователю выбирать цвет на основе его представления HSB, что делает выбор цветов более удобным для пользователя.

[![](walkthrough-images/run01.png "Example of the InfColorPicker library running on iOS")](walkthrough-images/run01.png#lightbox)

Мы обсудим все необходимые действия для использования этого конкретного API цели на C в Xamarin. iOS:

- Сначала мы создадим статическую библиотеку цели-C с помощью Xcode.
- Затем мы выполним привязку этой статической библиотеки к Xamarin. iOS.
- Далее показано, как целевое Шарпие может уменьшить рабочую нагрузку, автоматически создавая некоторые (но не все) необходимые определения API, необходимые для привязки Xamarin. iOS.
- Наконец, мы создадим приложение Xamarin. iOS, которое использует привязку.

Пример приложения демонстрирует, как использовать надежный делегат для обмена данными между API Инфколорпиккер и кодом C#. Когда мы увидели, как использовать надежный делегат, мы расскажем, как использовать слабые делегаты для выполнения тех же задач.

## <a name="requirements"></a>Требования

В этой статье предполагается, что у вас есть опыт работы с Xcode и языком цели-c, и вы прочитали нашу документацию по [цели привязки](~/cross-platform/macios/binding/index.md) . Кроме того, для выполнения представленных действий необходимо выполнить следующие действия.

- **Xcode и iOS SDK** . на компьютере разработчика необходимо установить и настроить Apple Xcode и последнюю версию API iOS.
- **[Средства командной строки Xcode](#Installing_the_Xcode_Command_Line_Tools)** . для текущей установленной версии Xcode должны быть установлены средства командной строки Xcode (Дополнительные сведения об установке см. ниже).
- **Visual Studio для Mac или Visual Studio** . на компьютере разработчика должны быть установлены и настроены последняя версия Visual Studio для Mac или Visual Studio. Для разработки приложения Xamarin. iOS требуется компьютер Apple Mac. при использовании Visual Studio необходимо подключение к [узлу сборки Xamarin. iOS](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
- **Последняя версия цели Шарпие** — текущая копия инструмента объектив Шарпие, скачанного [отсюда](~/cross-platform/macios/binding/objective-sharpie/get-started.md). Если вы уже установили цель Шарпие, вы можете обновить ее до последней версии, используя`sharpie update`

<a name="Installing_the_Xcode_Command_Line_Tools"></a>

## <a name="installing-the-xcode-command-line-tools"></a>Установка программ командной строки Xcode

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Как упоминалось выше, мы будем использовать программы командной строки Xcode ( `make` в частности и `lipo` ) в этом пошаговом руководстве. `make`Команда — это очень распространенная служебная программа для UNIX, которая автоматизирует компиляцию исполняемых программ и библиотек с помощью _файла makefile_ , который указывает, как должна быть построена программа. `lipo`Команда является служебной программой командной строки OS X для создания файлов с несколькими архитектурами. она объединяет несколько `.a` файлов в один файл, который может использоваться всеми аппаратными архитектурами.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Как упоминалось выше, мы будем использовать средства командной строки Xcode на **узле сборки Mac** (в частности `make` и `lipo` ) в этом пошаговом руководстве. `make`Команда — это очень распространенная служебная программа для UNIX, которая автоматизирует компиляцию исполняемых программ и библиотек с помощью _файла makefile_ для указания способа построения программы. `lipo`Команда является служебной программой командной строки OS X для создания файлов с несколькими архитектурами. она объединяет несколько `.a` файлов в один файл, который может использоваться всеми аппаратными архитектурами.

-----

В соответствии с [настройкой Apple из командной строки с](https://developer.apple.com/library/ios/technotes/tn2339/_index.html) документацией по Xcode FAQ в OS X 10,9 и более поздних версиях панель **загрузки** диалогового окна **предпочтения** Xcode больше не поддерживает скачивание программ командной строки.

Для установки средств необходимо использовать один из следующих методов:

- **Установка Xcode** . при установке Xcode они поставляются в комплекте со всеми программами командной строки. В оболочках совместимости OS X 10,9 (устанавливается в `/usr/bin` ) может сопоставлять любые средства, входящие в, в `/usr/bin` соответствующий инструмент в Xcode. Например, `xcrun` команда, которая позволяет найти или запустить любой инструмент в Xcode из командной строки.
- **Приложение терминала** . из приложения терминала можно установить программы командной строки, выполнив `xcode-select --install` команду:
  - Запустите приложение терминала.
  - Введите `xcode-select --install` и нажмите клавишу **Ввод**, например:

  ```bash
  Europa:~ kmullins$ xcode-select --install
  ```

  - Вам будет предложено установить программы командной строки, нажать кнопку " **установить** ":[![](walkthrough-images/xcode01.png "Установка программ командной строки")](walkthrough-images/xcode01.png#lightbox)

  - Эти средства будут скачаны и установлены с серверов Apple:[![](walkthrough-images/xcode02.png "Загрузка средств")](walkthrough-images/xcode02.png#lightbox)

- **Загружаемые файлы для разработчиков Apple** . пакет средств командной строки доступен на веб-странице [загрузки для разработчиков Apple](https://developer.apple.com/downloads/index.action) . Войдите в систему, используя идентификатор Apple ID, а затем найдите и скачайте программы командной строки:[![](walkthrough-images/xcode03.png "Поиск программ командной строки")](walkthrough-images/xcode03.png#lightbox)

После установки средств командной строки мы готовы продолжить работу с пошаговым руководством.

## <a name="walkthrough"></a>Пошаговое руководство

В этом пошаговом руководстве будут рассмотрены следующие шаги:

- **[Создание статической библиотеки](#Creating_A_Static_Library)** . Этот шаг включает создание статической библиотеки кода **Инфколорпиккер** цели-C. Статическая библиотека будет иметь `.a` расширение файла и будет внедрена в сборку .NET проекта библиотеки.
- **[Создание проекта привязки Xamarin. iOS](#Create_a_Xamarin.iOS_Binding_Project)** . когда у нас есть статическая библиотека, мы будем использовать ее для создания проекта привязки Xamarin. iOS. Проект привязки состоит из статической библиотеки, которую мы только что создали, и метаданных в виде кода C#, в котором объясняется, как можно использовать API цели-C. Эти метаданные обычно называются определениями API. Мы будем использовать **[Цель Шарпие](#Using_Objective_Sharpie)** , чтобы помочь нам создать определения API.
- **[Нормализация определений API](#Normalize_the_API_Definitions)** — цель Шарпие — отличное задание, которое поможет нам, но не может выполнить все действия. Мы обсудим некоторые изменения, которые необходимо внести в определения API, прежде чем их можно будет использовать.
- **[Использование библиотеки привязки](#Using_the_Binding)** . Наконец, мы создадим приложение Xamarin. iOS, чтобы продемонстрировать, как использовать созданный проект привязки.

Теперь, когда мы понимаем, какие шаги используются, давайте перейдем к остальной части этого пошагового руководства.

<a name="Creating_A_Static_Library"></a>

## <a name="creating-a-static-library"></a>Создание статической библиотеки

Если мы проверили код для Инфколорпиккер в GitHub:

[![](walkthrough-images/image02.png "Inspect the code for InfColorPicker in Github")](walkthrough-images/image02.png#lightbox)

В проекте можно увидеть следующие три папки:

- **Инфколорпиккер** — этот каталог содержит код цели-C для проекта.
- **Пиккерсамплепад** — этот каталог содержит пример проекта iPad.
- **Пиккерсамплефоне** — этот каталог содержит пример проекта iPhone.

Давайте загрузим проект Инфколорпиккер из [GitHub](https://github.com/InfinitApps/InfColorPicker/archive/master.zip) и распакуйте его в каталоге нашего выбора. Открыв целевой объект Xcode для `PickerSamplePhone` проекта, мы видим следующую структуру проекта в навигаторе Xcode:

[![](walkthrough-images/image03.png "The project structure in the Xcode Navigator")](walkthrough-images/image03.png#lightbox)

Этот проект достиг повторного использования кода путем непосредственного добавления исходного кода Инфколорпиккер (в красном поле) в каждый пример проекта. Код для примера проекта находится внутри синего прямоугольника. Поскольку в этом конкретном проекте нет статической библиотеки, нам нужно создать проект Xcode для компиляции статической библиотеки.

На первом шаге мы добавим исходный код Инфоколорпиккер в статическую библиотеку. Чтобы добиться этого, выполните следующие действия.

1. Запустите Xcode.
2. В меню **файл** выберите пункт **создать**  >  **проект...**:

    [![](walkthrough-images/image04.png "Starting a new project")](walkthrough-images/image04.png#lightbox)
3. Выберите **Framework & библиотека**, шаблон **статической библиотеки Cocoa Touch** и нажмите кнопку **Далее** :

    [![](walkthrough-images/image05.png "Select the Cocoa Touch Static Library template")](walkthrough-images/image05.png#lightbox)

4. Введите `InfColorPicker` для **имени проекта** и нажмите кнопку **Далее** :

    [![](walkthrough-images/image06.png "Enter InfColorPicker for the Project Name")](walkthrough-images/image06.png#lightbox)
5. Выберите расположение для сохранения проекта и нажмите кнопку **ОК** .
6. Теперь необходимо добавить источник из проекта Инфколорпиккер в наш проект статической библиотеки. Так как файл **инфколорпиккер. h** уже существует в нашей статической библиотеке (по умолчанию), Xcode не позволит нам перезаписать ее. Из **Finder**перейдите к исходному коду инфколорпиккер в исходном проекте, который был распакован из GitHub, скопируйте все файлы инфколорпиккер и вставьте их в новый проект статической библиотеки:

    [![](walkthrough-images/image12.png "Copy all of the InfColorPicker files")](walkthrough-images/image12.png#lightbox)

7. Вернитесь в Xcode, щелкните правой кнопкой мыши папку **инфколорпиккер** и выберите **Добавить файлы в "инфколорпиккер..."**:

    [![](walkthrough-images/image08.png "Adding files")](walkthrough-images/image08.png#lightbox)

8. В диалоговом окне Добавление файлов перейдите к только что скопированным файлам исходного кода Инфколорпиккер, выберите их все и нажмите кнопку **Добавить** :

    [![](walkthrough-images/image09.png "Select all and click the Add button")](walkthrough-images/image09.png#lightbox)

9. Исходный код будет скопирован в наш проект:

    [![](walkthrough-images/image10.png "The source code will be copied into the project")](walkthrough-images/image10.png#lightbox)

10. В навигаторе проекта Xcode выберите файл **инфколорпиккер. m** и закомментируйте последние две строки (из-за способа написания этой библиотеки, этот файл не используется):

    [![](walkthrough-images/image14.png "Editing the InfColorPicker.m file")](walkthrough-images/image14.png#lightbox)

11. Теперь необходимо проверить наличие платформ, необходимых для библиотеки. Эти сведения можно найти либо в файле сведений, либо при открытии одного из предлагаемых примеров проектов. В этом примере используется `Foundation.framework` , и, поэтому добавим `UIKit.framework` `CoreGraphics.framework` их.

12. Выберите **целевой > этапа сборки** и разверните раздел ссылка на **двоичный файл с библиотеками** :

    [![](walkthrough-images/image16b.png "Expand the Link Binary With Libraries section")](walkthrough-images/image16b.png#lightbox)

13. Используйте **+** кнопку, чтобы открыть диалоговое окно, в котором можно добавить необходимые платформы кадров, перечисленные выше:

    [![](walkthrough-images/image16c.png "Add the required frames frameworks listed above")](walkthrough-images/image16c.png#lightbox)

14. Раздел **ссылка на двоичный файл с библиотеками** теперь должен выглядеть следующим образом:

    [![](walkthrough-images/image16d.png "The Link Binary With Libraries section")](walkthrough-images/image16d.png#lightbox)

На этом этапе мы близки, но мы не совсем сделали этого. Статическая библиотека создана, но необходимо создать ее для создания двоичного файла FAT, включающего все необходимые архитектуры для устройств iOS и симуляторов iOS.

### <a name="creating-a-fat-binary"></a>Создание двоичного файла FAT

На всех устройствах iOS установлены процессоры на базе архитектуры ARM, разработанной со временем. Каждая новая архитектура добавила новые инструкции и другие улучшения, сохраняя при этом обратную совместимость. устройства iOS имеют наборы инструкций ARMv6, ARMv7, armv7s, arm64, хотя [ARMv6 больше не используются](~/ios/deploy-test/compiling-for-different-devices.md). Симулятор iOS не работает на базе ARM, а является симулятором на платформе x86 и x86_64. Это означает, что для каждого набора инструкций должны быть предоставлены библиотеки.

Библиотека FAT — это `.a` файл, содержащий все поддерживаемые архитектуры.

Создание двоичного файла FAT выполняется в три этапа:

- Скомпилируйте ARM64 версию ARM 7 & версии статической библиотеки.
- Скомпилируйте версию x86 и x84_64 статической библиотеки.
- Используйте `lipo` программу командной строки, чтобы объединить две статические библиотеки в одну.

Хотя эти три действия довольно просты, может потребоваться повторить их в будущем, когда библиотека цели-C получит обновления или если требуется исправление ошибок. Если вы решили автоматизировать эти шаги, это упростит дальнейшее обслуживание и поддержку проекта привязки iOS.

Существует множество средств для автоматизации таких задач, как сценарий оболочки, [Rake](https://rake.rubyforge.org/), [Xbuild](https://www.mono-project.com/docs/tools+libraries/tools/xbuild/)и make. При установке программ командной строки Xcode `make` также устанавливается, поэтому это система сборки, которая будет использоваться в этом пошаговом руководстве. Ниже приведен **файл Makefile** , который можно использовать для создания общей библиотеки с несколькими архитектурами, которая будет работать на устройстве iOS и симуляторе для любой библиотеки:

<!--markdownlint-disable MD010 -->
```makefile
XBUILD=/Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild
PROJECT_ROOT=./YOUR-PROJECT-NAME
PROJECT=$(PROJECT_ROOT)/YOUR-PROJECT-NAME.xcodeproj
TARGET=YOUR-PROJECT-NAME

all: lib$(TARGET).a

lib$(TARGET)-i386.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphonesimulator -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphonesimulator/lib$(TARGET).a $@

lib$(TARGET)-armv7.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch armv7 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

lib$(TARGET)-arm64.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch arm64 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

lib$(TARGET).a: lib$(TARGET)-i386.a lib$(TARGET)-armv7.a lib$(TARGET)-arm64.a
    xcrun -sdk iphoneos lipo -create -output $@ $^

clean:
    -rm -f *.a *.dll
```
<!--markdownlint-enable MD010 -->

Введите команды **makefile** в выбранном текстовом редакторе и обновите разделы, указав имя проекта в разделе с именем **-Project-Name** . Также важно убедиться в том, что инструкции, приведенные выше, правильно вставлены, а вкладки в этих инструкциях сохранены.

Сохраните файл с именем **makefile** в том же расположении, что и созданная ранее статическая библиотека инфколорпиккер Xcode:

[![](walkthrough-images/lib00.png "Save the file with the name Makefile")](walkthrough-images/lib00.png#lightbox)

Откройте приложение терминала на компьютере Mac и перейдите в расположение файла makefile. Введите `make` в терминал, нажмите клавишу **Ввод** , и **файл Makefile** будет выполнен:

[![](walkthrough-images/lib01.png "Sample makefile output")](walkthrough-images/lib01.png#lightbox)

При запуске вы увидите большой объем текста с прокруткой. Если все работало правильно, вы увидите слова **Сборка выполнена успешно** , а `libInfColorPicker-armv7.a` `libInfColorPicker-i386.a` `libInfColorPickerSDK.a` файлы и будут скопированы в то же расположение, что и **файл Makefile**:

[![](walkthrough-images/lib02.png "The libInfColorPicker-armv7.a, libInfColorPicker-i386.a and libInfColorPickerSDK.a files generated by the Makefile")](walkthrough-images/lib02.png#lightbox)

Вы можете проверить архитектуры в двоичном файле FAT, выполнив следующую команду:

```bash
xcrun -sdk iphoneos lipo -info libInfColorPicker.a
```

Должно отобразиться следующее:

```bash
Architectures in the fat file: libInfColorPicker.a are: i386 armv7 x86_64 arm64
```

На этом этапе мы завершили первый шаг привязки iOS, создав статическую библиотеку с помощью Xcode и программ командной строки Xcode `make` и `lipo` . Давайте перейдем к следующему шагу и используем **цели-Шарпие** , чтобы автоматизировать создание привязок API для нас.

<a name="Create_a_Xamarin.iOS_Binding_Project"></a>

## <a name="create-a-xamarinios-binding-project"></a>Создание проекта привязки Xamarin. iOS

Прежде чем можно будет использовать **Цель Шарпие** для автоматизации процесса привязки, необходимо создать проект привязки Xamarin. iOS для размещения определений API (которые мы будем использовать с помощью **цели-Шарпие** , чтобы помочь нам в построении) и создать привязку C# для нас.

Давайте выполним следующие действия.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Запустите Visual Studio для Mac.
1. В меню **файл** выберите пункт **создать**  >  **решение...**:

    ![](walkthrough-images/bind01.png "Starting a new solution")

1. В диалоговом окне новое решение выберите **Библиотека**  >  **проект привязки iOS**:

    ![](walkthrough-images/bind02.png "Select iOS Binding Project")

1. Нажмите кнопку **Далее**.

1. Введите "Инфколорпиккербиндинг" в качестве **имени проекта** и нажмите кнопку " **создать** ", чтобы создать решение:

    ![](walkthrough-images/bind02a.png "Enter InfColorPickerBinding as the Project Name")

Решение будет создано, и будут добавлены два файла по умолчанию:

![](walkthrough-images/bind03.png "The solution structure in the Solution Explorer")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Запустите среду Visual Studio.

1. В меню **файл** выберите пункт **создать**  >  **проект...**:

    ![создание проекта;](walkthrough-images/bind01vs.png "создание проекта;")

1. В диалоговом окне Новый проект выберите **Visual C# > iPhone & iPad > библиотека привязок iOS (Xamarin)**:

    [![Выбор библиотеки привязок iOS](walkthrough-images/bind02.w157-sml.png)](walkthrough-images/bind02.w157.png#lightbox)

1. Введите "Инфколорпиккербиндинг" в качестве **имени** и нажмите кнопку " **ОК** ", чтобы создать решение.

Решение будет создано, и будут добавлены два файла по умолчанию:

![](walkthrough-images/bind03vs.png "The solution structure in the Solution Explorer")

-----

- **ApiDefinition.CS** — этот файл будет содержать контракты, определяющие, каким будет оболочка API цели-C в C#.
- **Structs.CS** — этот файл будет содержать все структуры или значения перечисления, необходимые для интерфейсов и делегатов.

Далее в этом пошаговом руководстве мы будем работать с этими двумя файлами. Сначала необходимо добавить библиотеку Инфколорпиккер в проект привязки.

### <a name="including-the-static-library-in-the-binding-project"></a>Включение статической библиотеки в проект привязки

Теперь, когда наш проект базовой привязки готов, необходимо добавить двоичную библиотеку FAT, созданную ранее для библиотеки **инфколорпиккер** .

Чтобы добавить библиотеку, выполните следующие действия.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Щелкните правой кнопкой мыши папку **машинные ссылки** в панель решения и выберите **Добавить машинные ссылки**:

    ![](walkthrough-images/bind04a.png "Add Native References")

1. Перейдите к двоичному файлу FAT, созданному ранее ( `libInfColorPickerSDK.a` ), и нажмите кнопку " **Открыть** ":

    ![](walkthrough-images/bind05.png "Select the libInfColorPickerSDK.a file")
1. Файл будет добавлен в проект:

    ![](walkthrough-images/bind04.png "Including a file")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Скопируйте `libInfColorPickerSDK.a` из **узла сборки Mac** и вставьте его в проект привязки.

1. Щелкните проект правой кнопкой мыши и выберите **добавить > существующий элемент...**:

    ![](walkthrough-images/bind04vs.png "Adding an existing file")

1. Перейдите к `libInfColorPickerSDK.a` и нажмите кнопку **Добавить** :

    ![](walkthrough-images/bind05vs.png "Adding libInfColorPickerSDK.a")

1. Файл будет добавлен в проект.

-----

Когда файл **добавляется в проект** , Xamarin. IOS автоматически устанавливает для файла **действие сборки** **обжкбиндингнативелибрари**и создает специальный файл с именем `libInfColorPickerSDK.linkwith.cs` .

Этот файл содержит `LinkWith` атрибут, сообщающий Xamarin. iOS, как обрабатывается только что добавленная статическая библиотека. Содержимое этого файла показано в следующем фрагменте кода:

```csharp
using ObjCRuntime;

[assembly: LinkWith ("libInfColorPickerSDK.a", SmartLink = true, ForceLoad = true)]
```

`LinkWith`Атрибут определяет статическую библиотеку для проекта и некоторые важные флаги компоновщика.

Далее нам нужно создать определения API для проекта Инфколорпиккер. В рамках этого пошагового руководства мы будем использовать цель Шарпие для создания файла **ApiDefinition.CS**.

<a name="Using_Objective_Sharpie"></a>

## <a name="using-objective-sharpie"></a>Использование цели Шарпие

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Цель Шарпие — это средство командной строки (предоставляемое Xamarin), которое может помочь в создании определений, необходимых для привязки сторонней библиотеки цели-C к C#. В этом разделе мы будем использовать цель Шарпие для создания начального **ApiDefinition.CS** для проекта инфколорпиккер.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Цель Шарпие — это средство командной строки (предоставляемое Xamarin), которое может помочь в создании определений, необходимых для привязки сторонней библиотеки цели-C к C#. В этом разделе мы будем использовать цель Шарпие на нашем **узле сборки Mac** , чтобы создать первоначальный **ApiDefinition.CS** для проекта инфколорпиккер.

-----

Чтобы приступить к работе, скачайте целевой файл Шарпие Installer, как описано в этом [руководство](~/cross-platform/macios/binding/objective-sharpie/get-started.md#installing). Запустите установщик и следуйте всем запросам на экране мастера установки, чтобы установить цель Шарпие на нашем компьютере разработки.

После успешной установки цели Шарпие запустите приложение терминала и введите следующую команду, чтобы получить справку по всем предоставляемым средствам для помощи в привязке:

```bash
sharpie -help
```

Если выполнить указанную выше команду, будут созданы следующие выходные данные:

```bash
Europa:Resources kmullins$ sharpie -help
usage: sharpie [OPTIONS] TOOL [TOOL_OPTIONS]

Options:
  -h, --helpShow detailed help
  -v, --versionShow version information

Available Tools:
  xcode              Get information about Xcode installations and available SDKs.
  pod                Create a Xamarin C# binding to Objective-C CocoaPods
  bind               Create a Xamarin C# binding to Objective-C APIs
  update             Update to the latest release of Objective Sharpie
  verify-docs        Show cross reference documentation for [Verify] attributes
  docs               Open the Objective Sharpie online documentation
```

В рамках этого пошагового руководства мы будем использовать следующие целевые инструменты Шарпие:

- **Xcode** . это средство предоставляет нам информацию о текущей установке Xcode и версиях API iOS и Mac, которые мы установили. Эти сведения будут использоваться позже при формировании привязок.
- **BIND** — мы будем использовать это средство для анализа **h** -файлов в проекте инфколорпиккер в исходных файлах **ApiDefinition.CS** и **StructsAndEnums.CS** .

Чтобы получить справку по определенному целевому инструменту Шарпие, введите имя средства и `-help` параметр. Например, `sharpie xcode -help` возвращает следующие выходные данные:

```bash
Europa:Resources kmullins$ sharpie xcode -help
usage: sharpie xcode [OPTIONS]+

Options:
  -h, -help           Show detailed help
  -v, -verbose        Be verbose with output

Xcode Options:
  -sdks               List all available Xcode SDKs. Pass -verbose for more
                        details.
  -sdkpath SDK        Output the path of the SDK
  -frameworks SDK     List all available framework directories in a given SDK.
```

Прежде чем можно будет начать процесс привязки, необходимо получить сведения о текущих установленных пакетах SDK, введя в терминале следующую команду `sharpie xcode -sdks` :

```bash
amyb:Desktop amyb$ sharpie xcode -sdks
sdk: appletvos9.2    arch: arm64
sdk: iphoneos9.3     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: watchos2.2      arch: armv7
```

Как видно из приведенного выше, мы видим, что `iphoneos9.3` на нашем компьютере установлен пакет SDK. После этой информации мы готовы к анализу файлов проекта Инфколорпиккер `.h` в первоначальной **ApiDefinition.CS** и `StructsAndEnums.cs` для проекта инфколорпиккер.

В приложении терминала введите следующую команду:

```bash
sharpie bind --output=InfColorPicker --namespace=InfColorPicker --sdk=[iphone-os] -scope [full-path-to-project]/InfColorPicker/InfColorPicker [full-path-to-project]/InfColorPicker/InfColorPicker/*.h
```

Где `[full-path-to-project]` — полный путь к каталогу, где находится файл проекта **инфколорпиккер** Xcode на нашем компьютере, а [iPhone-OS] — это пакет SDK для iOS, который мы установили, как указано в `sharpie xcode -sdks` команде. Обратите внимание, что в этом примере мы передали ** \* . h** в качестве параметра, который включает *все* файлы заголовков в этом каталоге — обычно это не следует делать, а внимательно прочтите файлы заголовков, чтобы найти **h** -файл верхнего уровня, который ссылается на все остальные файлы, и просто передать его в целевую Шарпие. 

> [!TIP] 
> Для `-scope` аргумента передайте папку с заголовками, которые необходимо привязать. Без `-scope` аргумента цель Шарпие попытается создать привязки для всех импортируемых заголовков пакета SDK iOS, например `#import <UIKit.h>` , в результате чего создается файл огромных определений, который, скорее всего, приведет к ошибкам при компиляции проекта привязки. При использовании `-scope` набора аргументов Цель Шарпие не будет создавать привязки для всех заголовков за пределами папки с областью действия. 

В терминале будут созданы следующие [выходные данные](walkthrough-images/os05.png) :

```bash
Europa:Resources kmullins$ sharpie bind -output InfColorPicker -namespace InfColorPicker -sdk iphoneos8.1 /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h -unified
Compiler configuration:
    -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk -miphoneos-version-min=8.1 -resource-dir /Library/Frameworks/ObjectiveSharpie.framework/Versions/1.1.1/clang-resources -arch armv7 -ObjC

[  0%] parsing /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h
In file included from /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h:60:
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:28:1: warning: no 'assign',
      'retain', or 'copy' attribute is specified - 'assign' is assumed [-Wobjc-property-no-attribute]
@property (nonatomic) UIColor* sourceColor;
^
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:28:1: warning: default property
      attribute 'assign' not appropriate for non-GC object [-Wobjc-property-no-attribute]
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:29:1: warning: no 'assign',
      'retain', or 'copy' attribute is specified - 'assign' is assumed [-Wobjc-property-no-attribute]
@property (nonatomic) UIColor* resultColor;
^
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:29:1: warning: default property
      attribute 'assign' not appropriate for non-GC object [-Wobjc-property-no-attribute]
4 warnings generated.
[100%] parsing complete
[bind] InfColorPicker.cs
Europa:Resources kmullins$
```

А файлы **InfColorPicker.enums.CS** и **InfColorPicker.CS** будут созданы в нашем каталоге:

[![](walkthrough-images/os06.png "The InfColorPicker.enums.cs and InfColorPicker.cs files")](walkthrough-images/os06.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Откройте оба этих файла в проекте привязки, созданном ранее. Скопируйте содержимое файла **InfColorPicker.CS** и вставьте его в файл **ApiDefinition.CS** , заменив существующий `namespace ...` блок кода содержимым файла **InfColorPicker.CS** (без неизмененных `using` инструкций):

![](walkthrough-images/os07.png "The InfColorPickerControllerDelegate file")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Откройте оба этих файла в проекте привязки, созданном ранее. Скопируйте содержимое файла **InfColorPicker.CS** (с **узла сборки Mac**) и вставьте его в файл **ApiDefinition.CS** , заменив существующий `namespace ...` блок кода содержимым файла **InfColorPicker.CS** (без `using` изменений инструкций).

-----

<a name="Normalize_the_API_Definitions"></a>

## <a name="normalize-the-api-definitions"></a>Нормализация определений API

В целевом Шарпие иногда возникает проблема `Delegates` , поэтому необходимо изменить определение `InfColorPickerControllerDelegate` интерфейса и заменить `[Protocol, Model]` строку следующим:

```csharp
[BaseType(typeof(NSObject))]
[Model]
```

Итак, определение выглядит следующим образом:

[![](walkthrough-images/os11.png "The definition")](walkthrough-images/os11.png#lightbox)

Далее мы делаем то же самое с содержимым `InfColorPicker.enums.cs` файла, копируя и вставляя их в `StructsAndEnums.cs` файл `using` без изменений:

[![](walkthrough-images/os09.png "The contents the StructsAndEnums.cs file ")](walkthrough-images/os09.png#lightbox)

Кроме того, вы можете обнаружить, что цель Шарпие закомментировать привязку с `[Verify]` атрибутами. Эти атрибуты указывают на то, что необходимо убедиться, что цель Шарпиеа правильно, сравнив привязку с исходным объявлением C/объектив-C (которое будет предоставлено в комментарии над объявлением привязки). После проверки привязок следует удалить атрибут Verify. Дополнительные сведения см. в руководстве по [проверке](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) .

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

На этом этапе наш проект привязки должен быть завершен и готов к сборке. Давайте создадим наш проект привязки и убедитесь, что мы завершили без ошибок:

[Выполните сборку проекта привязки и убедитесь в отсутствии ошибок.](walkthrough-images/os12.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

На этом этапе наш проект привязки должен быть завершен и готов к сборке. Давайте создадим наш проект привязки и убедитесь, что мы завершились без ошибок.

-----

<a name="Using_the_Binding"></a>

## <a name="using-the-binding"></a>Использование привязки

Выполните следующие действия, чтобы создать пример приложения iPhone для использования созданной выше библиотеки привязки iOS.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. **Создание проекта Xamarin. iOS** . Добавьте в решение новый проект Xamarin. iOS с именем **инфколорпиккерсампле** , как показано на следующих снимках экрана:

    ![](walkthrough-images/use01.png "Adding a Single View App")

    ![](walkthrough-images/use01a.png "Setting the Identifier")

1. **Добавить ссылку в проект привязки** . Обновите проект **инфколорпиккерсампле** , чтобы он ссылался на проект **инфколорпиккербиндинг** :

    ![](walkthrough-images/use02.png "Adding Reference to the Binding Project")

1. **Создайте пользовательский интерфейс iPhone** . дважды щелкните файл **файл mainstoryboard. Storyboard** в проекте **инфколорпиккерсампле** , чтобы изменить его в конструкторе iOS. Добавьте **кнопку** в представление и вызовите ее `ChangeColorButton` , как показано ниже:

    ![](walkthrough-images/use03.png "Adding a Button to the view")

1. **Добавьте инфколорпиккервиев. XIB** — библиотека инфколорпиккер цели-C включает файл **XIB** . Xamarin. iOS не будет включать this **. XIB** в проект привязки, что приведет к ошибкам времени выполнения в нашем примере приложения. Чтобы решить эту проблему, добавьте файл **XIB** в проект Xamarin. iOS. Выберите проект Xamarin. iOS, щелкните правой кнопкой мыши и выберите **добавить > добавить файлы**и добавьте файл **. XIB** , как показано на следующем снимке экрана:

    ![](walkthrough-images/use04.png "Add the InfColorPickerView.xib")

1. При появлении запроса скопируйте файл **XIB** в проект.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. **Создание проекта Xamarin. iOS** . Добавление нового проекта Xamarin. iOS с именем **инфколорпиккерсампле** с помощью шаблона **приложения с одним представлением** :

    [![проект приложения iOS (Xamarin)](walkthrough-images/use01.w157-sml.png)](walkthrough-images/use01.w157.png#lightbox)

    [![Выбор шаблона](walkthrough-images/use01-2.w157-sml.png)](walkthrough-images/use01-2.w157.png#lightbox)

1. **Добавить ссылку в проект привязки** . Обновите проект **инфколорпиккерсампле** , чтобы он ссылался на проект **инфколорпиккербиндинг** :

    ![](walkthrough-images/use02vs.png "Add Reference to the Binding Project")

1. **Создайте пользовательский интерфейс iPhone** . дважды щелкните файл **файл mainstoryboard. Storyboard** в проекте **инфколорпиккерсампле** , чтобы изменить его в конструкторе iOS. Добавьте **кнопку** в представление и вызовите ее `ChangeColorButton` , как показано ниже:

    ![](walkthrough-images/use03vs.png "Create the iPhone User Interface")

1. **Добавьте инфколорпиккервиев. XIB** — библиотека инфколорпиккер цели-C включает файл **XIB** . Xamarin. iOS не будет включать this **. XIB** в проект привязки, что приведет к ошибкам времени выполнения в нашем примере приложения. Чтобы решить эту проблему, добавьте файл **. XIB** в проект Xamarin. iOS из нашего **узла сборки Mac**. Выберите проект Xamarin. iOS, щелкните правой кнопкой мыши и выберите **Добавить**  >  **существующий элемент...** и добавьте файл **. XIB** .

-----

Теперь давайте кратко рассмотрим протоколы в цели-C и то, как мы обрабатываем их в привязке и коде C#.

### <a name="protocols-and-xamarinios"></a>Протоколы и Xamarin. iOS

В цели-C протокол определяет методы (или сообщения), которые могут использоваться в определенных обстоятельствах. По сути, они очень похожи на интерфейсы в C#. Одно из основных различий между Протоколом цели и интерфейсом C# заключается в том, что протоколы могут иметь необязательные методы-методы, которые классу не требуется реализовывать. Цель-C использует @optional ключевое слово, чтобы указать, какие методы являются необязательными. Дополнительные сведения о протоколах см. в разделе [события, протоколы и делегаты](~/ios/app-fundamentals/delegates-protocols-and-events.md).

В **инфколорпиккерконтроллер** есть один такой протокол, показанный в следующем фрагменте кода:

```csharp
@protocol InfColorPickerControllerDelegate

@optional

- (void) colorPickerControllerDidFinish: (InfColorPickerController*) controller;
// This is only called when the color picker is presented modally.

- (void) colorPickerControllerDidChangeColor: (InfColorPickerController*) controller;

@end
```

Этот протокол используется **инфколорпиккерконтроллер** для информирования клиентов о том, что пользователь выбрал новый цвет и что **инфколорпиккерконтроллер** завершен. Цель Шарпие сопоставила этот протокол, как показано в следующем фрагменте кода:

```csharp
[BaseType(typeof(NSObject))]
[Model]
public partial interface InfColorPickerControllerDelegate {

    [Export ("colorPickerControllerDidFinish:")]
    void ColorPickerControllerDidFinish (InfColorPickerController controller);

    [Export ("colorPickerControllerDidChangeColor:")]
    void ColorPickerControllerDidChangeColor (InfColorPickerController controller);
}

```

При компиляции библиотеки привязок Xamarin. iOS создает абстрактный базовый класс `InfColorPickerControllerDelegate` с именем, который реализует этот интерфейс с виртуальными методами.

Этот интерфейс можно реализовать в приложении Xamarin. iOS двумя способами:

- **Strong делегирование** . использование строгого делегата подразумевает создание класса C#, который подклассует `InfColorPickerControllerDelegate` и переопределяет соответствующие методы. **Инфколорпиккерконтроллер** будет использовать экземпляр этого класса для взаимодействия с клиентами.
- **Слабый делегат** . слабый делегат — это немного другой метод, включающий создание открытого метода для некоторого класса (например, `InfColorPickerSampleViewController` ) и последующее предоставление этого метода `InfColorPickerDelegate` протоколу через `Export` атрибут.

Надежные делегаты обеспечивают технологию IntelliSense, безопасность типов и лучшую инкапсуляцию. По этим причинам следует использовать надежные делегаты там, где вы можете вместо слабого делегата.

В этом пошаговом руководстве рассматриваются оба способа: сначала реализуется надежный делегат, а затем объясняется, как реализовать слабый делегат.

### <a name="implementing-a-strong-delegate"></a>Реализация строгого делегата

Завершите работу приложения Xamarin. iOS, используя надежный делегат для ответа на `colorPickerControllerDidFinish:` сообщение:

**Подкласс инфколорпиккерконтроллерделегате** . Добавьте в проект новый класс с именем `ColorSelectedDelegate` . Измените класс так, чтобы он наработал следующий код:

```csharp
using InfColorPickerBinding;
using UIKit;

namespace InfColorPickerSample
{
  public class ColorSelectedDelegate:InfColorPickerControllerDelegate
  {
    readonly UIViewController parent;

    public ColorSelectedDelegate (UIViewController parent)
    {
      this.parent = parent;
    }

    public override void ColorPickerControllerDidFinish (InfColorPickerController controller)
    {
      parent.View.BackgroundColor = controller.ResultColor;
      parent.DismissViewController (false, null);
    }
  }
}
```

Xamarin. iOS привязывает делегат цели-C, создавая абстрактный базовый класс с именем `InfColorPickerControllerDelegate` . Подклассировать этот тип и переопределить `ColorPickerControllerDidFinish` метод для доступа к значению `ResultColor` свойства объекта `InfColorPickerController` .

**Создайте экземпляр колорселектедделегате** — наш обработчик событий потребует экземпляр `ColorSelectedDelegate` типа, созданного на предыдущем шаге. Измените класс `InfColorPickerSampleViewController` и добавьте в класс следующую переменную экземпляра:

```csharp
ColorSelectedDelegate selector;
```

**Инициализируйте переменную колорселектедделегате** — чтобы убедиться, что `selector` является допустимым экземпляром, обновите метод `ViewDidLoad` в `ViewController` , чтобы он соответствовал следующему фрагменту кода:

```csharp
public override void ViewDidLoad ()
{
  base.ViewDidLoad ();
  ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithStrongDelegate;
  selector = new ColorSelectedDelegate (this);
}
```

**Реализуйте метод хандлетаучупинсидевисстронгделегате** -Next. Реализуйте обработчик событий, когда пользователь касается **колорчанжебуттон**. Измените `ViewController` и добавьте следующий метод:

```csharp
using InfColorPicker;
...

private void HandleTouchUpInsideWithStrongDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.Delegate = selector;
    picker.PresentModallyOverViewController (this);
}

```

Сначала мы получаем экземпляр с `InfColorPickerController` помощью статического метода, и этот экземпляр будет осведомлен о нашем строгом делегате через свойство `InfColorPickerController.Delegate` . Это свойство было автоматически создано для нас с целью Шарпие. Наконец, мы вызываем, `PresentModallyOverViewController` чтобы показать представление `InfColorPickerSampleViewController.xib` , чтобы пользователь мог выбрать цвет.

**Запустите приложение** . на этом этапе мы сделали весь наш код. При запуске приложения вы сможете изменить цвет фона, `InfColorColorPickerSampleView` как показано на следующих снимках экрана:

[![](walkthrough-images/run01.png "Running the Application")](walkthrough-images/run01.png#lightbox)

Поздравляем! На этом этапе вы успешно создали и привязываете библиотеку цели-C для использования в приложении Xamarin. iOS. Теперь давайте познакомимся с использованием слабых делегатов.

### <a name="implementing-a-weak-delegate"></a>Реализация слабого делегата

Вместо создания подкласса класса, привязанного к протоколу цели-C для конкретного делегата, Xamarin. iOS также позволяет реализовать методы протокола в любом классе, производном от, дополняя `NSObject` методы с помощью `ExportAttribute` , а затем предоставляя соответствующие селекторы. При использовании этого подхода экземпляр класса назначается `WeakDelegate` свойству, а не `Delegate` свойству. Слабый делегат обеспечивает гибкость для использования класса делегата в другой иерархии наследования. Давайте посмотрим, как реализовать и использовать слабый делегат в приложении Xamarin. iOS.

**Создание обработчика событий для таучупинсиде** . Давайте создадим новый обработчик событий для `TouchUpInside` события кнопки изменить цвет фона. Этот обработчик будет заполнять ту же роль, что и `HandleTouchUpInsideWithStrongDelegate` обработчик, созданный в предыдущем разделе, но будет использовать слабый делегат вместо строгого делегата. Измените класс `ViewController` и добавьте следующий метод:

```csharp
private void HandleTouchUpInsideWithWeakDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.WeakDelegate = this;
    picker.SourceColor = this.View.BackgroundColor;
    picker.PresentModallyOverViewController (this);
}
```

**Обновление ViewDidLoad** . необходимо изменить, `ViewDidLoad` чтобы использовался только что созданный обработчик событий. Измените `ViewController` и измените, `ViewDidLoad` чтобы они наглядели следующим фрагментом кода:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithWeakDelegate;
}

```

**Обработайте колорпиккерконтроллердидфиниш: Message** — когда `ViewController` закончится, iOS отправит сообщение `colorPickerControllerDidFinish:` в `WeakDelegate` . Нам нужно создать метод C#, который может справиться с этим сообщением. Для этого мы создадим метод C#, а затем додекоративим его с помощью `ExportAttribute` . Измените `ViewController` и добавьте в класс следующий метод:

```csharp
[Export("colorPickerControllerDidFinish:")]
public void ColorPickerControllerDidFinish (InfColorPickerController controller)
{
    View.BackgroundColor = controller.ResultColor;
    DismissViewController (false, null);
}

```

Запустите приложение. Теперь он должен работать точно так же, как и раньше, но вместо строгого делегата используется слабый делегат. На этом этапе вы успешно завершили это пошаговое руководство. Теперь у вас должно быть представление о том, как создать и использовать проект привязки Xamarin. iOS.

## <a name="summary"></a>Сводка

В этой статье рассматривается процесс создания и использования проекта привязки Xamarin. iOS. Во-первых, мы рассмотрели компиляцию существующей библиотеки цели-C в статическую библиотеку. Затем мы рассмотрели создание проекта привязки Xamarin. iOS и использование цели Шарпие для создания определений API для библиотеки цели-C. Мы обсуждали, как обновить и настроить созданные определения API, чтобы сделать их пригодными для общедоступного потребления. После завершения проекта привязки Xamarin. iOS мы перешли к использованию этой привязки в приложении Xamarin. iOS, а также сосредоточиться на использовании строгих делегатов и слабых делегатов.

## <a name="related-links"></a>Связанные ссылки

- [Привязка библиотек Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Сведения о привязке](~/cross-platform/macios/binding/overview.md)
- [Справочное руководство по типам привязки](~/cross-platform/macios/binding/binding-types-reference.md)
- [Xamarin для разработчиков цели-C](~/ios/get-started/objective-c-developers/index.md)
- [Рекомендации по проектированию платформы](https://msdn.microsoft.com/library/ms229042.aspx)
