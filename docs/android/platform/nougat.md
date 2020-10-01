---
title: Возможности Nougat
description: Начните использовать Xamarin.Android для разработки приложений для Android Nougat.
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/02/2018
ms.openlocfilehash: 135303743cce1f23bb1d70731f6e760188df11e2
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457215"
---
# <a name="nougat-features"></a>Возможности Nougat

_Начните использовать Xamarin.Android для разработки приложений для Android Nougat._

В этой статье представлен обзор возможностей Android Nougat, описывается подготовка к разработке Xamarin.Android для Android Nougat и приводятся ссылки на примеры приложений, иллюстрирующие использование возможностей Android Nougat в приложениях Xamarin.Android.

## <a name="overview"></a>Обзор

[ Android Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html) — это следующий выпуск Android 6.0 Marshmallow Google. Xamarin.Android обеспечивает поддержку **привязок Android 7.x** в Xamarin Android 7.0 и более поздних версиях. В Android Nougat добавлены множество новых API для возможностей Nougat, описанных ниже. Эти API доступны для приложений Xamarin.Android при использовании Xamarin.Android 7.0.

[![Главный имиджевый баннер с изображением планшета и телефонов с Android Nougat](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Дополнительные сведения об API Android 7.x см. в статье [Android 7.1 для разработчиков](https://developer.android.com/preview/api-overview.html).
Список известных проблем Xamarin.Android 7.0 можно найти в этих [заметках о выпуске](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_7/xamarin.android_7.0/index.md).

Android Nougat предоставляет множество новых возможностей, представляющих интерес для разработчиков Xamarin.Android. Эти функции включают перечисленные ниже.

- **Поддержка многооконного режима** — это улучшение позволяет пользователям одновременно открывать два приложения на экране.

- **Улучшенные уведомления** — усовершенствованная система уведомлений в Android Nougat предусматривает возможность *прямого ответа*, которая позволяет пользователям быстро отвечать на текстовые сообщения напрямую из пользовательского интерфейса уведомления. Кроме того, если приложение создает уведомления о полученных сообщениях, новая возможность *объединенных уведомлений* объединяет их в группу при получении нескольких сообщений.

- **Экономия данных** — эта возможность представляет собой новую системную службу, которая помогает сократить использование сотовых данных приложениями. Она дает пользователям возможность управлять им.

Кроме того, в Android Nougat введены множество других улучшений, интересных разработчикам приложений, такие как новая возможность настройки безопасности сети, режим Doze on the Go, аттестация ключей, новые API быстрых настроек, поддержка нескольких языковых стандартов, API ICU4J, усовершенствованные веб-представления, доступ к возможностям языка Java 8, доступ к каталогам с заданной областью, API настраиваемого указателя, поддержка платформы виртуальной реальности, виртуальные файлы и оптимизация фоновой обработки.

В этой статье описано, как приступить к сборке приложений для Android Nougat, чтобы испытать новые возможности и спланировать миграцию или работу функций на новой платформе Android Nougat.

## <a name="requirements"></a>Требования

Для использования новых возможностей Android Nougat в приложениях на основе Xamarin требуется следующее:

- **Visual Studio или Visual Studio для Mac** — если вы используете Visual Studio, требуются Инструменты Visual Studio версии 4.2.0.628 или более поздней версии для Xamarin. Если вы используете Visual Studio для Mac, требуется версия 6.1.0 или более поздняя.

- **Xamarin. Android** — необходимо установить Xamarin.Android 7.0 или более поздней версии и настроить это расширение в Visual Studio или Visual Studio для Mac.

- **Пакет SDK для Android** — нужно установить пакет SDK для Android 7.0 (API 24) или более поздней версии через Диспетчер SDK Android.

- **Java Developer Kit** — при разработке Xamarin Android 7.0 требуется [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) или более поздней версии, чтобы использовать API уровня 24 или выше (JDK 8 также поддерживает уровни API ниже 24). 64-разрядная версия JDK 8 необходима, если используются пользовательские элементы управления или средство предварительного просмотра Forms.

> [!IMPORTANT]
> Xamarin.Android не поддерживает пакет JDK 9.

Обратите внимание, что приложения должны быть пересобраны для использования Xamarin C6SR4 или более поздней версии для надежной работы с Android Nougat. Так как Android Nougat может ссылаться только на [встроенные библиотеки NDK](https://developer.android.com/about/versions/nougat/android-7.0-changes.html), существующие приложения, которые используют такие библиотеки, как **Mono.Data.Sqlite.dll**, могут аварийно завершать свою работу при выполнении в Nougat Android, если они не правильно пересобраны.

## <a name="getting-started"></a>Начало работы

Чтобы приступить к разработке приложений для Android Nougat с использованием Xamarin.Android, необходимо скачать и установить последние версии средств и пакетов SDK до создания проекта для Android Nougat:

1. Установите последние обновления Xamarin.Android из Xamarin.

2. Установите **пакеты и средства Android 7.0 (API 24)** или более поздней версии.

3. Создайте проект Xamarin.Android, предназначенный для Android Nougat.

4. Настройте эмулятор или устройство для Android Nougat.

В разделах ниже отдельно рассматривается каждый шаг.

### <a name="install-xamarin-updates"></a>Установка обновлений Xamarin

Чтобы добавить поддержку Xamarin для Android Nougat, измените канал обновления в Visual Studio или Visual Studio для Mac на стабильный канал и примените последние обновления. Если вам также нужны возможности, доступные только в канале альфа- или бета-версии, можно переключиться на соответствующий канал (обе версии также обеспечивают поддержку Android 7.x). Сведения о том, как изменить канал обновлений (выпусков), см. в разделе [Изменение канала обновлений](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel).

### <a name="install-the-android-sdk"></a>Установка пакета SDK для Android

Чтобы создать проект с использованием Xamarin.Android 7.0, необходимо сначала открыть Диспетчер SDK Android, чтобы установить платформу SDK для **Android N (API уровня 24)** или более поздней версии. Необходимо также установить последнюю версию **Android SDK Tools**:

1. Запустите Диспетчер SDK Android (в Visual Studio для Mac выберите элементы **Сервис > Открыть диспетчер пакетов SDK Android&hellip;** , а в Visual Studio — **Сервис > Android > Диспетчер пакетов SDK Android**).

2. Установите **Android 7.0 (API 24)** или более поздней версии:

    [![Выбор пакетов Android 7.0 в Диспетчере SDK Android](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3. Установите последние средства пакета SDK для Android:

    [![Выбор последних инструментов пакета SDK для Android в Диспетчере SDK Android](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Вам необходимо установить Android SDK Tools редакции 25.2.2 или более поздней, инструменты платформы SDK для Android 24.0.3 или более поздней версии и инструменты сборки SDK для Android 24.0.2 или более поздней версии.

4. Убедитесь, что для параметра **Расположение пакета Java Development Kit** настроен JDK 1.8:

    [![Настройка пути к JDK 8 в разделе параметров инструментов](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Чтобы просмотреть этот параметр в Visual Studio, выберите элементы **Сервис > Параметры > Xamarin > Параметры Android**. В Visual Studio для Mac выберите элементы **Параметры > Проекты > Расположения пакетов SDK > Android**.

### <a name="start-a-xamarinandroid-project"></a>Запуск проекта Xamarin.Android

Создайте проект Xamarin.Android. Если вы не знакомы с разработкой приложений для Android с использованием Xamarin, ознакомьтесь с [этой статьей](~/android/get-started/hello-android/index.md), чтобы узнать о создании проектов Xamarin.Android.

При создании проекта Android необходимо настроить параметры версии для целевой версии Android 7.0 или более поздней. Например, чтобы создать проект для Android 7.0, необходимо настроить целевой уровень API Android проекта, задав **Android 7.0 (API 24 — Nougat)** . Рекомендуем задать целевой уровень платформы API 24 или выше. Дополнительные сведения о настройке уровней API Android см. в [этой статье](~/android/app-fundamentals/android-api-levels.md).

> [!NOTE]
> Сейчас необходимо установить **самую раннюю версию Android** — **Android 7.0 (API 24 — Nougat)** для развертывания приложения на устройствах или эмуляторах Android Nougat.

### <a name="configure-an-emulator-or-device"></a>Настройка эмулятора или устройства

Если вы используете эмулятор, запустите AVD Manager (диспетчер виртуальных устройств Android) и создайте устройство, используя следующие параметры:

- Устройство: Nexus 5X, Nexus 6, Nexus 6P, Nexus Player, Nexus 9 или Pixel C.
- Цель: Android 7.0 с API уровня 24
- ABI: x86 или x86\_64

Например, это виртуальное устройство настроено для эмуляции Nexus 6:

[![Настройка в AVD устройства Nexus 6, целевой ОС Android 7.0 и ЦП или ABI Intel Atom x86](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

При использовании физического устройства, такого как Nexus 5X, 6 или 9, можно либо обновить устройство с помощью автоматического обновления по беспроводной связи (OTA), либо скачать образ системы и напрямую установить его на устройство. Дополнительные сведения обновлении устройства до Android Nougat вручную см. в статье [Полные образы OTA для устройств Nexus и Pixel](https://developers.google.com/android/nexus/ota).

Обратите внимание, что устройства Nexus 5 не поддерживаются в Android Nougat.

## <a name="new-features"></a>Новые функции

Android Nougat представляет множество новых функций и возможностей, таких как поддержка нескольких окон, усовершенствованные уведомления и экономия данных. В следующих разделах описаны эти функции и приведены ссылки, которые помогут приступить к их использованию в приложении.

### <a name="multi-window-mode"></a>Многооконный режим

Многооконный режим позволяет пользователям одновременно открывать два приложения с полной поддержкой многозадачности. Эти приложения могут работать параллельно (в альбомной ориентации) или одно над другим (в книжной ориентации) в режиме разделения экрана.
Пользователи могут перетаскивать разделитель приложений, чтобы изменять их размер, а также могут вырезать и вставлять содержимое из приложения в приложение. Когда два приложения представлены в многооконном режиме, выбранное действие продолжает выполняться, а отмененное действие приостанавливается, но остается видимым. Многооконный режим не изменяет жизненный цикл действия Android.

[![Примеры приложений, запущенных в многооконном режиме в книжной и альбомной ориентации](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Вы можете настроить то, как действия приложения Xamarin.Android будут поддерживать многооконный режим. Например, можно настроить атрибуты, устанавливающие минимальный размер, высоту и ширину окна приложения по умолчанию в многооконном режиме. Вы можете использовать новое свойство `Activity.IsInMultiWindowMode`, чтобы определить, применяется ли ваше действие в многооконном режиме. Пример:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

Пример приложения [MultiWindowPlayground](/samples/xamarin/monodroid-samples/android-n-multiwindowplayground) включает в себя код C#, демонстрирующий использование преимуществ многооконных пользовательских интерфейсов на приложении.

Дополнительные сведения о многооконном режиме см. в разделе о [поддержке этого режима](https://developer.android.com/guide/topics/ui/multi-window.html).

### <a name="enhanced-notifications"></a>Улучшенные уведомления

В Android Nougat представлена усовершенствованная система уведомлений. В ней реализована новая возможность прямого ответа, позволяющая пользователям быстро отвечать через уведомления на входящие текстовые сообщения напрямую в пользовательском интерфейсе уведомления. Начиная с Android 7.0, уведомления можно объединять в группу при получении нескольких сообщений. Кроме того, разработчики могут настраивать представления уведомлений, оформления системы и новые шаблоны уведомлений, используемые при создании уведомлений.

#### <a name="direct-reply"></a>Прямой ответ

Когда пользователь получает уведомление о входящем сообщении, в Android Nougat можно ответить на сообщение через уведомление (вместо того, чтобы открывать приложение для обмена сообщениями и отправлять ответ).
Эта функция встроенного ответа позволяет пользователям быстро отвечать на SMS или текстовые сообщения напрямую в интерфейсе уведомления:

[![Снимок экрана уведомления с полем встроенного прямого ответа](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

Чтобы обеспечить поддержку этой возможности в приложении, необходимо добавить *действия встроенного ответа* в приложение с помощью объекта [RemoteInput](xref:Android.App.RemoteInput), чтобы пользователи могли ответить текстом прямо в интерфейсе уведомления.
Например, следующий код создает `RemoteInput` для получения входного текста, создает намерение ожидания действия ответа и действие с поддержкой удаленного ввода:

```csharp
// Build a RemoteInput for receiving text input:
var remoteInput = new Android.Support.V4.App.RemoteInput.Builder (EXTRA_REMOTE_REPLY)
    .SetLabel (GetString (Resource.String.reply))
    .Build ();

// Build a Pending Intent for the reply action to trigger:
PendingIntent replyIntent = PendingIntent.GetBroadcast (ApplicationContext,
                                conversation.ConversationId,
                                GetMessageReplyIntent (conversation.ConversationId),
                                PendingIntentFlags.UpdateCurrent);

// Build an Android 7.0 compatible Remote Input enabled action:
NotificationCompat.Action actionReplyByRemoteInput = new NotificationCompat.Action.Builder (
    Resource.Drawable.notification_icon,
    GetString (Resource.String.reply),
    replyIntent).AddRemoteInput (remoteInput).Build ();
```

Это действие добавляется к уведомлению:

```csharp
// Create the notification:
NotificationCompat.Builder builder = new NotificationCompat.Builder (ApplicationContext)
   .SetSmallIcon (Resource.Drawable.notification_icon)
   ...
   .AddAction (actionReplyByRemoteInput);
```

Пример приложения [службы обмена сообщениями](/samples/xamarin/monodroid-samples/android-n-messagingservice) включает в себя код C#, демонстрирующий способ расширения возможностей уведомлений с помощью объекта `RemoteInput`. Дополнительные сведения о добавлении действий встроенного ответа в приложение для Android 7.0 или более поздней версии см. в статье [Обзор уведомлений](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct).

#### <a name="bundled-notifications"></a>Объединенные уведомления

Android Nougat может группировать уведомления (например, по теме сообщений) и показывать группу, а не каждое отдельное сообщение.
Эта функция *объединенных уведомлений* позволяет пользователям закрывать или архивировать группу уведомлений одним действием. Пользователь может потянуть содержимое вниз, чтобы открыть пакет уведомлений и подробно просмотреть каждое из них:

[![Снимок экрана с примеров объединенных уведомлений](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

Для поддержки пакетов уведомлений приложение может использовать метод [Builder.SetGroup](xref:Android.App.Notification.Builder.SetGroup*), чтобы объединять аналогичные уведомления. Дополнительные сведения о пакетных группах уведомлений в Android N см. в разделе [Обновления и группы уведомлений](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle).

#### <a name="custom-views"></a>Пользовательские представления

Android Nougat позволяет создавать пользовательские представления уведомлений с использованием заголовков системных уведомлений, действий и развертываемых макетов. Дополнительные сведения о пользовательских представлениях уведомлений в Android Nougat см. в разделе [Новые возможности уведомлений](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements).

### <a name="data-saver"></a>Экономия данных

Начиная с Android Nougat, пользователи могут включить новый параметр *экономии данных*, который блокирует фоновое использование данных. Этот параметр также сигнализирует приложению использовать меньше данных на переднем плане, когда это возможно. В Android Nougat добавлен [ConnectivityManager](xref:Android.Net.ConnectivityManager), чтобы приложение могло проверить, включил ли пользователь экономию данных, и ограничить использование данных, если включена функция экономии.

Дополнительные сведения о новой функции экономии данных в Android Nougat см. в разделе [Оптимизация использования сетевых данных](https://developer.android.com/training/basics/network-ops/data-saver.html).

### <a name="app-shortcuts"></a>Сочетания клавиш приложения

В Android 7.1 появилась *возможность сочетаний клавиш приложения*, позволяющая пользователям быстро запускать обычные или рекомендуемые задачи с приложением.
Чтобы активировать меню ярлыков, пользователь долго нажимает значок приложения во второй раз или больше, меню отображается с быстрой вибрацией.
Если перестать нажимать, меню остается открытым:

[![Пример экрана контекстного меню приложения для обмена сообщениями](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

Эта возможность доступна только в API уровня 25.
Дополнительные сведения о новой возможности сочетаний клавиш приложений в Android 7.1 см. в разделе [Типы сочетаний клавиш](https://developer.android.com/guide/topics/ui/shortcuts.html).

### <a name="sample-code"></a>Пример кода

Существует несколько примеров Xamarin.Android, демонстрирующих использование преимуществ возможностей Android Nougat:

- [MultiWindowPlayground](/samples/xamarin/monodroid-samples/android-n-multiwindowplayground) демонстрирует использование многооконного API, доступного в Android Nougat. Вы можете переключить пример приложения в многооконный режим, чтобы увидеть, как он влияет на жизненный цикл и поведение приложения.

- [Служба обмена сообщениями](/samples/xamarin/monodroid-samples/android-n-messagingservice) — это простая служба, которая отправляет уведомления с помощью `NotificationCompatManager`. Она также расширяет возможности уведомления благодаря объекту `RemoteInput`, что позволяет устройствам Android Nougat отвечать текстом напрямую через уведомления без необходимости открывать приложение.

- С помощью [активных уведомлений](/samples/xamarin/monodroid-samples/android-n-activenotifications) показано использование API `NotificationManager`, чтобы узнать, сколько уведомлений сейчас отображается в приложении.

- [Доступ к каталогу с заданной областью](/samples/xamarin/monodroid-samples/android-n-scopeddirectoryaccess) показывает, как использовать API прямого доступа к каталогам с заданной областью для простого доступа к конкретным каталогам. Это служит альтернативой определению разрешения `READ_EXTERNAL_STORAGE` или `WRITE_EXTERNAL_STORAGE` в манифесте.

- [Прямая загрузка](/samples/xamarin/monodroid-samples/android-n-directboot) — в этом примере описано, как хранить данные в хранилище, зашифрованном с помощью устройства, которое всегда доступно, пока устройство загружается как до, так и после ввода учетных данных пользователя (ПИН-кода/рисунка/пароля).

## <a name="summary"></a>Сводка

Из этой статьи вы узнали об Android Nougat, установке и настройке последних средств и пакетов для разработки приложений Xamarin.Android для Android Nougat. В нем также предоставлен обзор основных возможностей, доступных в Android Nougat, со ссылками на примеры исходного кода, которые помогут вам приступить к созданию приложений для Android Nougat.

## <a name="related-links"></a>Связанные ссылки

- [Android 7.1 для разработчиков](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Заметки о выпуске Xamarin Android 7.0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_7/xamarin.android_7.0/index.md)