---
title: Устранение неполадок watchOS
description: В этом документе обсуждаются известные проблемы и способы их решения для разработки watchOS с помощью Xamarin. Здесь описываются образы с проблемами, Добавление файлов контроллера интерфейса вручную, запуск приложения наблюдения из командной строки и многое другое.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 27C31DB8-451E-4888-BBC1-CE0DFC2F9DEC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 17ccc67b2976b93fbb290a1d2425168cab50228e
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84568793"
---
# <a name="watchos-troubleshooting"></a>Устранение неполадок watchOS

На этой странице содержатся дополнительные сведения и способы решения проблем, которые могут возникнуть.

- [Известные проблемы](#knownissues)

- [Удаление альфа-канала из изображений значков](#noalpha)

- [Добавление файлов контроллера интерфейса](#add) для Interface Builder Xcode вручную.

- [Запуск ватчапп из командной строки](#command_line).

<a name="knownissues"></a>

## <a name="known-issues"></a>Известные проблемы

### <a name="general"></a>Общие

<a name="deploy"></a>

- Более ранние выпуски Visual Studio для Mac неправильно отображают один из значков **апплекомпанионсеттингс** как 88x88 пикселов; Это приводит к **ошибке отсутствующего значка** при попытке отправить в App Store.
    Этот значок должен быть 87x87 пикселей (29 единиц для **@3x** экранов Retina). Вы не можете исправить это в Visual Studio для Mac. Измените ресурс изображения в Xcode или вручную измените файл **Content. JSON** .

- Если **идентификатор пакета WKApp "info. > plist** " проекта расширения контрольных значений [неправильно установлен](~/ios/watchos/get-started/project-references.md) в соответствии с **идентификатором пакета**приложения Watch, отладчику не удастся подключиться, и Visual Studio для Mac будет ожидать сообщение *"Ожидание подключения отладчика"*.

- Отладка поддерживается в режиме **уведомлений** , но может быть ненадежной. Повторная попытка иногда будет работать. Убедитесь, что значение **info. plist** приложения Watch `WKCompanionAppBundleIdentifier` установлено в соответствии с идентификатором пакета приложения iOS (родительское или контейнером), которое выполняется на устройстве iPhone.

- в конструкторе iOS не отображаются стрелки точки входа для быстрого просмотра или контроллеров интерфейса уведомлений.

- Нельзя добавить две `WKNotificationControllers` к раскадровке.
    Обходное решение. `notificationCategory` элемент в XML-коде раскадровки всегда вставляется с тем же `id` . Чтобы обойти эту проблему, можно добавить два (или более) контроллера уведомлений, открыть файл раскадровки в текстовом редакторе, а затем вручную изменить `id` элемент на уникальный.

    [![](troubleshooting-images/duplicate-id-sml.png "Opening the storyboard file in a text editor and manually change the id element to be unique")](troubleshooting-images/duplicate-id.png#lightbox)

- При попытке запустить приложение может появиться сообщение об ошибке "приложение не было собрано". Это происходит после **очистки** , если запускаемый проект установлен в проект расширения Watch.
    Исправлением является выбор **сборки > перестроить все** , а затем повторно запустить приложение.

### <a name="visual-studio"></a>Visual Studio

Для поддержки набора наблюдения в конструкторе iOS *необходимо* правильно настроить решение. Если ссылки на проект не заданы (см. раздел [как установить ссылки](~/ios/watchos/get-started/project-references.md)), область конструктора будет работать неправильно.

<a name="noalpha"></a>

## <a name="removing-the-alpha-channel-from-icon-images"></a>Удаление альфа-канала из изображений значков

Значки не должны содержать альфа-канал (канал альфа определяет прозрачные области изображения), в противном случае приложение будет отклонено во время отправки в магазин приложений с ошибкой, аналогичной следующей:

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/Icon-27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Вы можете легко удалить альфа-канал на Mac OS X с помощью **предварительной версии** приложения:

1. Откройте изображение значка в **области предварительного просмотра** , а затем выберите **файл > экспорт**.

2. Диалоговое окно, которое отображается, будет содержать флажок **альфа** -канала, если имеется канал Alpha.

    ![](troubleshooting-images/remove-alpha-sml.png "The dialog that appears will include an Alpha checkbox if an alpha channel is present")

3. *Untick* Снимите флажок **альфа-канала** и **Сохраните** файл в нужном месте.

4. Теперь изображение значка должно передавать проверки Apple.

<a name="add"></a>

## <a name="manually-adding-interface-controller-files"></a>Добавление файлов контроллера интерфейса вручную

> [!IMPORTANT]
> Поддержка WatchKit в Xamarin включает проектирование раскадровок просмотра в конструкторе iOS (как в Visual Studio для Mac, так и в Visual Studio), что не требует действий, описанных ниже. Просто присвойте контроллеру интерфейса имя класса на панели свойств Visual Studio для Mac, и файлы кода C# будут созданы автоматически.

*Если* вы используете Xcode Interface Builder, выполните следующие действия, чтобы создать новые контроллеры интерфейса для приложения для просмотра контрольных данных и включить синхронизацию с Xcode, чтобы в C# можно было использовать следующие возможности и действия.

1. Откройте интерфейс Watch приложения **. раскадровка** в **Xcode Interface Builder**.

    ![](troubleshooting-images/add-6.png "Opening the storyboard in Xcode Interface Builder")

2. Перетащите новый элемент `InterfaceController` на раскадровку:

    ![](troubleshooting-images/add-1.png "A InterfaceController")

3. Теперь можно перетаскивать элементы управления на контроллер интерфейса (например, Метки и кнопки), но вы не можете создавать розетки или действия, так как отсутствует **h** -файл заголовка. Следующие шаги приведут к созданию обязательного файла заголовка **. h** .

    ![](troubleshooting-images/add-2.png "A button in the layout")

4. Закройте раскадровку и вернитесь к Visual Studio для Mac. Создайте новый файл C# **MyInterfaceController.CS** (или любое имя) в проекте " **Контрольное** значение" (а не в приложении для просмотра, где находится Раскадровка). Добавьте следующий код (с обновлением пространства имен, ClassName и имени конструктора):

    ```csharp
    using System;
    using WatchKit;
    using Foundation;

    namespace WatchAppExtension  // remember to update this
    {
        public partial class MyInterfaceController // remember to update this
        : WKInterfaceController
        {
            public MyInterfaceController // remember to update this
            (IntPtr handle) : base (handle)
            {
            }
            public override void Awake (NSObject context)
            {
                base.Awake (context);
                // Configure interface objects here.
                Console.WriteLine ("{0} awake with context", this);
            }
            public override void WillActivate ()
            {
                // This method is called when the watch view controller is about to be visible to the user.
                Console.WriteLine ("{0} will activate", this);
            }
            public override void DidDeactivate ()
            {
                // This method is called when the watch view controller is no longer visible to the user.
                Console.WriteLine ("{0} did deactivate", this);
            }
        }
    }
    ```

5. Создайте еще один новый файл C# **MyInterfaceController.Designer.CS** в проекте " **Контрольное расширение приложения** " и добавьте приведенный ниже код. Не забудьте обновить пространство имен, ClassName и `Register` атрибут:

    ```csharp
    using Foundation;
    using System.CodeDom.Compiler;

    namespace HelloWatchExtension  // remember to update this
    {
        [Register ("MyInterfaceController")] // remember to update this
        partial class MyInterfaceController  // remember to update this
        {
            void ReleaseDesignerOutlets ()
            {
            }
        }
    }
    ```

    > [!TIP]
    > Можно (необязательно) сделать этот файл дочерним узлом первого файла, перетащив его на другой файл C# в Visual Studio для Mac Панель решения. Затем он будет выглядеть следующим образом:

    ![](troubleshooting-images/add-5.png "The Solution pad")

6. Выберите **сборка > собрать все** , чтобы в ходе синхронизации Xcode был распознан новый класс (через `Register` атрибут), который мы использовали.

7. Повторно откройте раскадровку, щелкнув файл раскадровки Watch приложения правой кнопкой мыши и выбрав пункт **Открыть с помощью > Xcode Interface Builder**:

    ![](troubleshooting-images/add-6.png "Opening the storyboard in Interface Builder")

8. Выберите новый контроллер интерфейса и присвойте ему значение className, указанное выше, например. `MyInterfaceController`.
    Если все работало правильно, оно должно появиться автоматически в раскрывающемся списке **класс:** , и его можно выбрать из этого списка.

    ![](troubleshooting-images/add-4.png "Setting a custom class")

9. Выберите представление **редактора помощника** в Xcode (значок с двумя перекрывающимися кружками), чтобы можно было увидеть раскадровку и код параллельно:

    ![](troubleshooting-images/add-7.png "The Assistant Editor toolbar item")

    Если фокус находится на панели кода, убедитесь, что вы просматриваете файл заголовка **h** и не щелкнули правой кнопкой мыши в строке навигации, и выберите нужный файл (**минтерфацеконтроллер. h**).

    ![](troubleshooting-images/add-8.png "Select MyInterfaceController")

10. Теперь вы можете создавать розетки и действия, **удерживая нажатой клавишу CTRL + перетаскивание** из раскадровки в файл заголовка **h** .

    ![](troubleshooting-images/add-9.png "Creating outlets and actions")

    При освобождении перетаскивания вам будет предложено выбрать, следует ли создать розетку или действие, и выбрать его имя:

    ![](troubleshooting-images/add-a.png "The outlet and an action dialog")

11. Когда изменения раскадровки сохранены и Xcode закрывается, вернитесь в Visual Studio для Mac. Он определит изменения в файле заголовка и автоматически добавит код в файл **Designer.CS** :

    ```csharp
    [Register ("MyInterfaceController")]
    partial class MyInterfaceController
    {
        [Outlet]
        WatchKit.WKInterfaceButton myButton { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (myButton != null) {
                myButton.Dispose ();
                myButton = null;
            }
        }
    }
    ```

Теперь вы можете ссылаться на элемент управления (или реализовать действие) в C#!

<a name="command_line"></a>

## <a name="launching-the-watch-app-from-the-command-line"></a>Запуск приложения Watch из командной строки

> [!IMPORTANT]
> Вы можете запустить приложение Watch в нормальном режиме приложения по умолчанию, а также в режимах **уведомлений** **или с** помощью [настраиваемых параметров выполнения](~/ios/watchos/get-started/installation.md#custommodes) в Visual Studio для Mac и Visual Studio.

Вы также можете использовать командную строку для управления симулятором iOS. Программа командной строки, используемая для запуска контрольных приложений, — **mtouch**.

Ниже приведен полный пример (выполняется как отдельная строка в терминале):

```bash
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin/mtouch --sdkroot=/Applications/Xcode.app/Contents/Developer/ --device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

Параметр, который необходимо обновить, чтобы отразить ваше приложение `launchsimwatch` :

### <a name="--launchsimwatch"></a>--лаунчсимватч

Полный путь к главному пакету приложений *для приложения iOS, содержащего приложение и расширение Watch*.

> [!NOTE]
> Путь, который необходимо предоставить, предназначен для *файла приложения iPhone. app*, т. е. который будет развернут в симуляторе iOS и содержит как расширение, так и контрольное приложение.

Пример.

```bash
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

## <a name="notification-mode"></a>Режим уведомления

Чтобы проверить [режим **уведомления** ](~/ios/watchos/platform/notifications.md)приложения, присвойте `watchlaunchmode` параметру значение `Notification` и укажите путь к JSON-файлу, содержащему полезные данные тестового уведомления.

Параметр полезных данных является *обязательным* для режима уведомления.

Например, добавьте следующие аргументы в команду mtouch:

```bash
--watchlaunchmode=Notification --watchnotificationpayload=/path/to/file.json
```

## <a name="other-arguments"></a>Другие аргументы

Остальные аргументы описаны ниже.

### <a name="--sdkroot"></a>--sdkroot добавлен

Обязательный. Указывает путь к Xcode (6,2 или более поздней версии).

Пример.

```bash
 --sdkroot /Applications/Xcode.app/Contents/Developer/
```

### <a name="--device"></a>--устройство

Устройство имитатора для выполнения. Это можно указать двумя способами: с помощью UDID конкретного устройства или с помощью сочетания среды выполнения и типа устройства.

Точные значения различаются между компьютерами, и их можно запрашивать с помощью средства Apple **симктл** .

```bash
/Applications/Xcode.app/Contents/Developer/usr/bin/simctl list
```

**UDID.**

Пример.

```bash
--device=:v2:udid=AAAAAAA-BBBB-CCCC-DDDD-EEEEEEEEEEEE
```

**Среда выполнения и тип устройства**

Пример.

```bash
--device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
```

## <a name="related-links"></a>Связанные ссылки

- [Ватчкиткаталог (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Ватчтаблес (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchtables)
