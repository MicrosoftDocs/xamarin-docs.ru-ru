---
title: Устранение неполадок Xamarin Live Player
description: В этом документе описаны известные проблемы с Xamarin Live Player и возможными исправлениями. В нем обсуждаются проблемы подключения, проблемы с конфигурацией и многое другое.
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
author: davidortinau
ms.author: daortin
ms.date: 06/13/2019
ms.openlocfilehash: 86cc5e30efd16be5826dfd641549dca47cfbc0e9
ms.sourcegitcommit: 4bbf54d2bc1df96af69814e2e5dae47be12e0474
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2021
ms.locfileid: "102602285"
---
# <a name="troubleshooting-xamarin-live-player"></a>Устранение неполадок Xamarin Live Player

![Предварительная версия](~/media/shared/preview.png)

> [!WARNING]
> Предварительный просмотр Xamarin Live Player завершен. Приложение больше не доступно. Приведенные ниже инструкции предназначены для пользователей, которые продолжают использовать предварительную версию с Visual Studio 2017.

> [!TIP]
> Вы можете использовать [горячую перезагрузку XAML (~/ксамарин-Формс/ксамл/хот-релоад/индекс.МД) в Visual Studio 2019 или Visual Studio для Mac для просмотра своих макетов на экране по мере их редактирования.

В этой статье описаны ограничения для Live Player и некоторые распространенные проблемы с действиями по их устранению.

## <a name="limitations-of-xamarin-live-player"></a>Ограничения Xamarin Live Player

### <a name="ide-requirements"></a>Требования к интегрированной среде разработки

Предварительная версия Live Player доступна только в Visual Studio 2017.

### <a name="device-requirements"></a>Требования к устройствам

Приложение Xamarin Live Player поддерживает следующие устройства Android:

- Android 4,2 или более поздней версии.
- Процессор ARM-v7a, ARM-v8a, ARM64-v8a, x86 или x86_64.

### <a name="ios-limitations"></a>ограничения iOS

Динамический проигрыватель недоступен для iOS.

### <a name="xamarinforms-limitations"></a>Ограничения Xamarin. Forms

- Пользовательские модули подготовки отчетов не поддерживаются.
- Эффекты не поддерживаются.
- Пользовательские элементы управления с настраиваемыми связываемыми свойствами не поддерживаются.
- Внедренные ресурсы не поддерживаются (IE. Встраивание изображений или других ресурсов в PCL).
- Сторонние платформы MVVM не поддерживаются (IE. Prism, MVVM Cross, MVVM Light и т. д.).

### <a name="other-project-type-limitations"></a>Ограничения других типов проектов

- Live Player не предназначен для собственных проектов Android (которые используют XML-код Android для пользовательского интерфейса).

### <a name="miscellaneous-limitations"></a>Прочие ограничения

- Ограниченная поддержка отражения (в настоящее время затрагивает некоторые популярные NuGet, такие как SQLite и Json.NET). Другие NuGet могут по-прежнему поддерживаться.
- Некоторые системные классы не могут быть переопределены (например, нельзя реализовать подкласс).
- Некоторые функции платформы, требующие подготовки, не могут работать в Xamarin Live Player приложении (однако оно настроено для выполнения стандартных операций, таких как доступ к коллекции фотографий).
- Пользовательские целевые объекты и шаги сборки игнорируются. Например, такие средства, как Фоди, refit, AutoFac и автосопоставитель, не могут быть включены.
- Проекты F # не поддерживаются
- Расширенные сценарии с пользовательскими универсальными классами и интерфейсами могут не поддерживаться.

## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>Мобильное устройство не подключается после сканирования штрихкода (или ввода кода)

Происходит, когда мобильное устройство, на котором работает Xamarin Live Player, находится не в той же сети, что и компьютер с интегрированной средой разработки. Ознакомьтесь с приведенными ниже примерами.

- Убедитесь, что устройство и компьютер находятся в одной сети Wi-Fi.
  - Если компьютер также подключен к проводной сети, попробуйте отключить проводное подключение.
- Сеть может быть тесно защищена (например, в некоторых корпоративных сетях), блокируя порты, необходимые для Xamarin Live Player.
- Закройте приложение Xamarin Live Player и перезапустите его.

## <a name="error-while-trying-to-deploy-message-in-ide"></a>Ошибка при попытке развернуть сообщение в интегрированной среде разработки

**"IOException: не удается считать данные из транспортного соединения: операция на неблокирующем сокете блокирует"**

Эта ошибка часто возникает, когда мобильное устройство, на котором работает Xamarin Live Player, находится не в той же сети, что и компьютер с Visual Studio. Это часто происходит при подключении к устройству, которое было успешно парно.

- Убедитесь, что устройство и компьютер находятся в одной сети Wi-Fi.
- Сеть может быть тесно защищена (например, в некоторых корпоративных сетях), блокируя порты, необходимые для Xamarin Live Player. Для Xamarin Live Player требуются следующие порты:
  - 37847 — внутренний доступ к сети 
  - 8090 — доступ к внешней сети

## <a name="manually-configure-device"></a>Настройка устройства вручную

Если не удается подключиться к устройству по Wi-Fi можно попытаться вручную настроить устройство с помощью файла конфигурации, выполнив следующие действия.

**Шаг 1. Открытие файла конфигурации**

Перейдите к папке данных приложения:

- Windows: **%усерпрофиле%\аппдата\роаминг**
- macOS: **~/Users/$User/.конфиг**

В этой папке можно найти **PlayerDeviceList.xml** если она не существует, потребуется ее создать.

**Шаг 2. получение IP-адреса**

В приложении Xamarin Live Player перейдите к разделу **о > подключения проверка > начать проверку подключения**.

Запишите IP-адрес. IP-адрес должен быть указан при настройке устройства.

**Шаг 3. получение кода связывания**

В Xamarin Live Player **пару пара** или **пара** нажмите клавишу **Ввод вручную**. Отобразится числовой код, в котором потребуется обновить файл конфигурации.

**Шаг 4. Создание GUID**

Перейдите к: https://www.guidgenerator.com/online-guid-generator.aspx и создайте новый GUID и убедитесь, что верхний регистр включен.

**Шаг 5. Настройка устройства**

Откройте **PlayerDeviceList.xml** в редакторе, например Visual Studio или Visual Studio Code. Вам нужно настроить устройство вручную в этом файле. По умолчанию файл должен содержать следующий пустой `Devices` XML-элемент:

```xml
<DeviceList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Devices>

</Devices>
</DeviceList>
```

**Добавить устройство Android:**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>Android Player</Name>
<Platform>Android</Platform>
<AndroidApiLevel>24</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:34:42.2332328Z</LastConnectTimeUtc>
</PlayerDevice>
```

**Закройте и снова откройте Visual Studio.** Ваше устройство должно отображаться в списке.

## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>Сообщение "тип или пространство имен не удается найти" в IDE

Убедитесь, что выбран **запускаемый проект** , соответствующий типу устройства (например, Android) и конфигурация соответствует этому типу устройства (например, **Отладка** для Android).

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>"Конструктор для типа" Интерпретедксамарин. Forms. Button "не найден" в проигрывателе

Некоторые системные классы не могут быть переопределены, например:

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>"MainActivity.cs:" ресурс. Layout "не содержит определения для" Main "

Эта ошибка возникает для проектов Android с пользовательскими интерфейсами, определенными в файлах AXML.
Файлы AXML в настоящее время не поддерживаются в Xamarin Live Player.

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Панель инструментов и вкладки Android неправильно отображаются с помощью Xamarin. Forms

Проекты Android для Xamarin. Forms должны использовать "Toolbar. axml" и "таббар. axml" для имен соответствующих файлов макета. В шаблоне по умолчанию используются эти имена. Переименование этих файлов приведет к проблемам с отрисовкой.

## <a name="related-links"></a>Связанные ссылки

- [Установка](~/tools/live-player/install.md)
