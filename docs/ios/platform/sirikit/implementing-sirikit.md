---
title: Реализация SiriKit в Xamarin. iOS
description: В этом документе описаны шаги, необходимые для реализации поддержки SiriKit в приложениях Xamarin. iOS. Здесь обсуждаются расширения и расширения пользовательского интерфейса для целей.
ms.prod: xamarin
ms.assetid: 20FFB981-EB10-48BA-BF79-40F37F0291EB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/03/2018
ms.openlocfilehash: ed31e0262fecccf8974961d45d02388aeb8e85ea
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91434430"
---
# <a name="implementing-sirikit-in-xamarinios"></a>Реализация SiriKit в Xamarin. iOS

_В этой статье рассматриваются шаги, необходимые для реализации поддержки SiriKit в приложениях Xamarin. iOS._

Новое в iOS 10, SiriKit позволяет приложению Xamarin. iOS предоставлять службы, доступные пользователю с помощью Siri и приложения Maps на устройстве iOS. В этой статье рассматриваются шаги, необходимые для реализации поддержки SiriKit в приложениях Xamarin. iOS путем добавления необходимых вспомогательных расширений, способов расширения пользовательского интерфейса и словаря.

Siri работает с понятием **доменов**, группами известных действий для связанных задач. Каждое взаимодействие приложения с Siri должно находиться в одном из известных доменов служб следующим образом:

- Вызов аудио или видео.
- Заменяйте его.
- Управление тренировки.
- Отправка.
- Поиск фотографий.
- Отправка или получение платежей.

Когда пользователь выполняет запрос Siri, включающий одну из служб расширения приложения, SiriKit отправляет расширение объект **намерения** , описывающий запрос пользователя вместе с любыми вспомогательными данными. затем расширение приложения создает соответствующий объект **ответа** для заданной **цели**, чтобы получить сведения о том, как расширение может справиться с запросом.

В этом руководстве приведен краткий пример включения поддержки SiriKit в существующее приложение. Для этого примера мы будем использовать фиктивное приложение Монкэйчат:

[![Значок Монкэйчат](implementing-sirikit-images/monkeychat01.png)](implementing-sirikit-images/monkeychat01.png#lightbox)

Монкэйчат сохраняет собственную контактную книгу друзей пользователей, каждый из которых связан с именем экрана (например, Бобо), и позволяет пользователю отправить текстовые обсуждения друг другу по имени экрана.

## <a name="extending-the-app-with-sirikit"></a>Расширение приложения с помощью SiriKit

Как показано в разделе [Общие сведения об основных понятиях SiriKit](~/ios/platform/sirikit/understanding-sirikit.md) , существует три основные части, участвующие в расширении приложения с помощью SiriKit.

[![Расширение приложения с помощью схемы SiriKit](implementing-sirikit-images/elements01.png)](implementing-sirikit-images/elements01.png#lightbox)

приведенные ниже.

1. **Расширение целей** — проверяет ответы пользователей, подтверждает, что приложение может выполнить запрос и фактически выполняет задачу для выполнения запроса пользователя.
2. Расширение пользовательского интерфейса для **целей**  -  *Необязательно*, предоставляет пользовательский интерфейс для ответов в среде Siri и может переносить пользовательский интерфейс приложений и фирменную символику в Siri, чтобы расширить возможности пользователя.
3. **App** — предоставляет приложение с словари, предназначенным для пользователя, чтобы помочь Siri в работе с ним. 

Все эти элементы и шаги по их включению в приложение будут подробно рассмотрены в следующих разделах.

## <a name="preparing-the-app"></a>Подготовка приложения

Однако SiriKit построен на расширениях, прежде чем добавлять в приложение какие-либо расширения, существует несколько моментов, которые необходимо выполнить разработчику, чтобы помочь в внедрении SiriKit.

### <a name="moving-common-shared-code"></a>Перемещение общего общего кода 

Во-первых, разработчик может переместить часть общего кода, который будет совместно использоваться приложением и расширениями, в _Общие проекты_, _переносимые библиотеки классов (PCL)_ или _собственные библиотеки_.

Расширения должны иметь возможность выполнять все действия, которые выполняет приложение. В терминах примера приложения Монкэйчат такие вещи, как поиск контактов, добавление новых контактов, отправка сообщений и получение журнала сообщений.

Перемещая этот общий код в общий проект, PCL или собственную библиотеку, он упрощает поддержание этого кода в одном месте и гарантирует, что расширение и родительское приложение предоставляют пользователю Единообразные возможности и функции.

В случае с примером приложения Монкэйчат модели данных и код обработки, такие как доступ к сети и базе данных, будут перемещены в собственную библиотеку.

Выполните следующие действия.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Запустите Visual Studio для Mac и откройте приложение Монкэйчат.
2. Щелкните правой кнопкой мыши имя решения в **панель решения** и выберите **Добавить**  >  **Новый проект...**: 

    [![Добавление нового проекта](implementing-sirikit-images/prep01.png)](implementing-sirikit-images/prep01.png#lightbox)
3. Выберите **iOS**  >  **Библиотека**  >  **классов** iOS и нажмите кнопку **Далее** : 

    [![Выбор библиотеки классов](implementing-sirikit-images/prep02.png)](implementing-sirikit-images/prep02.png#lightbox)
4. Введите `MonkeyChatCommon` в поле **имя** и нажмите кнопку **создать** : 

    [![Введите Монкэйчаткоммон для имени](implementing-sirikit-images/prep03.png)](implementing-sirikit-images/prep03.png#lightbox)
5. Щелкните правой кнопкой мыши папку **References** основного приложения на **Обозреватель решений** и выберите **изменить ссылки.**... Проверьте проект **монкэйчаткоммон** и нажмите кнопку **ОК** : 

    [![Проверка проекта Монкэйчаткоммон](implementing-sirikit-images/prep05.png)](implementing-sirikit-images/prep05.png#lightbox)
6. В **Обозреватель решений**перетащите общий общий код из основного приложения в собственную библиотеку.
7. В случае с Монкэйчат перетащите папки **модели** данных и **процессоры** из основного приложения в собственную библиотеку: 

    [![Папки моделей и процессоров в обозреватель решений](implementing-sirikit-images/prep06.png)](implementing-sirikit-images/prep06.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Запустите Visual Studio и откройте приложение Монкэйчат.
2. Щелкните правой кнопкой мыши имя решения в **Обозреватель решений** и выберите **Добавить**  >  **Новый проект...**.
3. Выберите **Visual C#**  >  **общий проект** Visual C# и нажмите кнопку **Далее** : 

    [![Выбор библиотеки классов](implementing-sirikit-images/prep02.w157-sml.png)](implementing-sirikit-images/prep02.w157.png#lightbox)
4. Введите `MonkeyChatCommon` для **имени** и нажмите кнопку **создать** .
5. Щелкните правой кнопкой мыши папку **References** основного приложения на **Обозреватель решений** и выберите **изменить ссылки.**... Проверьте проект **монкэйчаткоммон** и нажмите кнопку **ОК** : 

    [![Проверка проекта Монкэйчаткоммон](implementing-sirikit-images/prep05w.png)](implementing-sirikit-images/prep05w.png#lightbox)
6. В **Обозреватель решений**перетащите общий общий код из основного приложения в общий проект.
7. В случае с Монкэйчат перетащите папки **модели** данных и **процессоры** из основного приложения в собственную библиотеку.

-----

Измените любой из файлов, которые были перемещены в собственную библиотеку, и измените пространство имен, чтобы оно совпадало с форматом библиотеки. Например, изменение `MonkeyChat` на `MonkeyChatCommon` :

```csharp
using System;
namespace MonkeyChatCommon
{
    /// <summary>
    /// A message sent from one user to another within a conversation.
    /// </summary>
    public class MonkeyMessage
    {
        public MonkeyMessage ()
        {
        }
        ...
    }
}
```

Затем вернитесь к основному приложению и добавьте `using` оператор для пространства имен собственной библиотеки, где в приложении используется один из перенесенных классов:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using MonkeyChatCommon;

namespace MonkeyChat
{
    public partial class MasterViewController : UITableViewController
    {
        public DetailViewController DetailViewController { get; set; }

        DataSource dataSource;
        ...
    }
}
```

### <a name="architecting-the-app-for-extensions"></a>Разработка архитектуры приложения для расширений

Как правило, приложение регистрируется для нескольких целей, и разработчик должен убедиться в том, что приложение является архитектурой для соответствующего количества расширений с намерением.

В ситуации, когда приложению требуется более одной цели, разработчик имеет возможность поместить все его намерения в одно расширение намерения или создать отдельное расширение намерения для каждой цели.

Если вы создаете отдельное расширение намерения для каждой цели, разработчик может в итоге дублировать большой объем стандартного кода в каждом расширении и создать большой объем ресурсов процессора и памяти.

Сведения о выборе между двумя вариантами см. в разделе Если один из целей естественным образом принадлежит друг к другу. Например, приложение, которое выполняет вызовы аудио и видео, может захотеть включить оба этих подхода в однонаправленное расширение, так как они обрабатывают похожие задачи и могут предоставлять наибольшее использование кода.

Для любой цели или группы целей, которые не помещаются в существующую группу, создайте новое расширение намерения в решении приложения, чтобы оно содержало их.

### <a name="setting-the-required-entitlements"></a>Настройка необходимых прав

В любом приложении Xamarin. iOS, которое включает интеграцию SiriKit, должны быть установлены правильные права. Если разработчик не установил эти требуемые права правильно, они не смогут установить или протестировать приложение на реальном устройстве iOS 10 (или более поздней версии), что также необходимо, поскольку симулятор iOS 10 не поддерживает SiriKit.

Выполните следующие действия.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Дважды щелкните `Entitlements.plist` файл в **Обозреватель решений** , чтобы открыть его для редактирования.
2. Перейдите на вкладку **Источник**.
3. Добавьте `com.apple.developer.siri` **свойство**, задайте **тип** `Boolean` и **значение** `Yes` : 

    [![Добавление свойства com. Apple. Developer. Siri](implementing-sirikit-images/setup01.png)](implementing-sirikit-images/setup01.png#lightbox)
4. Сохраните изменения в файле.
5. Дважды щелкните **файл проекта** на **Обозреватель решений** , чтобы открыть его для редактирования.
6. Выберите **Подписывание пакета iOS** и убедитесь, что `Entitlements.plist` файл выбран в поле **Настраиваемые** права: 

    [![Выберите файл прав. plist в поле настраиваемые права.](implementing-sirikit-images/setup02.png)](implementing-sirikit-images/setup02.png#lightbox)
7. Нажмите кнопку **ОК**, чтобы сохранить изменения.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Дважды щелкните `Entitlements.plist` файл в **Обозреватель решений** , чтобы открыть его для редактирования.
2. Добавьте `com.apple.developer.siri` **свойство**, задайте **тип** `Boolean` и **значение** `Yes` : 

    [![Добавление свойства com. Apple. Developer. Siri](implementing-sirikit-images/setup01w.png)](implementing-sirikit-images/setup01w.png#lightbox)
3. Сохраните изменения в файле.
4. Дважды щелкните **файл проекта** на **Обозреватель решений** , чтобы открыть его для редактирования.
5. Выберите **Подписывание пакета iOS** и убедитесь, что `Entitlements.plist` файл выбран в поле **Настраиваемые** права.

-----

По завершении `Entitlements.plist` файл приложения должен выглядеть следующим образом (в открытом во внешнем редакторе):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.siri</key>
    <true/>
</dict>
</plist>
```

### <a name="correctly-provisioning-the-app"></a>Правильная подготовка приложения

Из-за максимальной безопасности, которую компания Apple поместила вокруг SiriKit Framework, любое приложение Xamarin. iOS, реализующее SiriKit, _должно_ иметь правильный идентификатор приложения и права (см. раздел выше) и должен быть подписан соответствующим подготовительным профилем.

На компьютере Mac выполните следующие действия.

1. В веб-браузере перейдите к [https://developer.apple.com](https://developer.apple.com) своей учетной записи и войдите в нее.
2. Щелкните **Сертификаты**, **идентификаторы** и **Профили**.
3. Выберите **профили подготовки** и выберите **Идентификаторы приложений**, а затем нажмите кнопку **+** .
4. Введите **имя** нового профиля.
5. Введите **идентификатор пакета** , следующий за рекомендацией по именованию Apple.
6. Прокрутите вниз до раздела **службы приложений** , выберите **SiriKit** и нажмите кнопку **Continue (продолжить** ): 

    [![Выбор SiriKit](implementing-sirikit-images/setup03.png)](implementing-sirikit-images/setup03.png#lightbox)
7. Проверьте все параметры, а затем **отправьте** идентификатор приложения.
8. Выберите **Разработка профили подготовки**  >  **Development**, нажмите кнопку **+** , выберите **идентификатор Apple ID**, а затем нажмите кнопку **продолжить**.
9. Щелкните выбрать **все**, а затем нажмите кнопку **продолжить**.
10. Щелкните **выбрать все** еще раз, а затем нажмите кнопку **продолжить**.
11. Введите **имя профиля** с помощью предложений по именованию Apple, а затем нажмите кнопку **продолжить**.
12. Запустите Xcode.
13. В меню Xcode выберите **предпочтения...**
14. Выберите **учетные записи**, а затем щелкните **Просмотр сведений...** переключатель 

    [![Выбор учетных записей](implementing-sirikit-images/setup04.png)](implementing-sirikit-images/setup04.png#lightbox)
15. Нажмите кнопку **скачать все профили** в левом нижнем углу экрана: 

    [![Скачать все профили](implementing-sirikit-images/setup05.png)](implementing-sirikit-images/setup05.png#lightbox)
16. Убедитесь, что созданный выше **профиль подготовки** установлен в Xcode.
17. Откройте проект, чтобы добавить поддержку SiriKit в в Visual Studio для Mac.
18. Дважды щелкните `Info.plist` файл в **Обозреватель решений**.
19. Убедитесь, что **идентификатор пакета** соответствует тому, который был создан на портале разработчика Apple выше: 

    [![Идентификатор пакета](implementing-sirikit-images/setup06.png)](implementing-sirikit-images/setup06.png#lightbox)
20. В **Обозреватель решений**выберите **проект**.
21. Щелкните правой кнопкой мыши проект и выберите пункт **Параметры**.
22. Выберите **Подписывание пакета iOS**, выберите **удостоверение подписывания** и **профиль подготовки** , созданные выше: 

    [![Выберите удостоверение подписывания и профиль подготовки](implementing-sirikit-images/setup07.png)](implementing-sirikit-images/setup07.png#lightbox)
23. Нажмите кнопку **ОК**, чтобы сохранить изменения.

> [!IMPORTANT]
> Тестирование SiriKit работает только на устройстве с устройством iOS 10, а не в симуляторе iOS 10. При возникновении проблем с установкой приложения Xamarin. iOS SiriKit на реальном оборудовании убедитесь, что необходимые права, идентификатор приложения, идентификатор подписывания и профиль подготовки правильно настроены на портале разработчика Apple и в Visual Studio для Mac.

### <a name="requesting-siri-authorization"></a>Запрос авторизации Siri

Перед тем как приложение добавит пользовательский словарь или расширения целей, подключаются к Siri, оно должно запросить авторизацию пользователя для доступа к Siri.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Измените `Info.plist` файл приложения, переключитесь в представление **исходного кода** и добавьте `NSSiriUsageDescription` ключ со строковым значением, описывающим, как приложение будет использовать Siri и какие типы данных будут отправляться. Например, приложение Монкэйчат может сказать «Монкэйчат Contacts to Siri»:

[![Нссириусажедескриптион в редакторе info. plist](implementing-sirikit-images/request01.png)](implementing-sirikit-images/request01.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Измените `Info.plist` файл приложения и добавьте `NSSiriUsageDescription` ключ со строковым значением, описывающим, как приложение будет использовать Siri и какие типы данных будут отправляться. Например, приложение Монкэйчат может сказать «Монкэйчат Contacts to Siri»:

[![Нссириусажедескриптион в редакторе info. plist](implementing-sirikit-images/request01w.png)](implementing-sirikit-images/request01w.png#lightbox)

-----

Вызовите `RequestSiriAuthorization` метод `INPreferences` класса при первом запуске приложения. Измените `AppDelegate.cs` класс и сделайте `FinishedLaunching` метод следующим:

```csharp
using Intents;
...

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{

    // Request access to Siri
    INPreferences.RequestSiriAuthorization ((INSiriAuthorizationStatus status) => {
        // Respond to returned status
        switch (status) {
        case INSiriAuthorizationStatus.Authorized:
            break;
        case INSiriAuthorizationStatus.Denied:
            break;
        case INSiriAuthorizationStatus.NotDetermined:
            break;
        case INSiriAuthorizationStatus.Restricted:
            break;
        }
    });

    return true;
}
```

При первом вызове этого метода отображается предупреждение, предлагающее пользователю разрешить приложению доступ к Siri. В этом предупреждении будет отображаться сообщение, добавленное разработчиком в `NSSiriUsageDescription` вышеуказанное предупреждение. Если пользователь изначально запрещает доступ, он может использовать приложение **параметров** для предоставления доступа к приложению.

В любое время приложение может проверить возможность доступа приложения к Siri, вызвав `SiriAuthorizationStatus` метод `INPreferences` класса.

### <a name="localization-and-siri"></a>Локализация и Siri

На устройстве iOS пользователь может выбрать язык для Siri, отличающийся от системы по умолчанию. При работе с локализованными данными приложению потребуется использовать `SiriLanguageCode` метод `INPreferences` класса для получения кода языка из Siri. Пример:

```csharp
var language = INPreferences.SiriLanguageCode();

// Take action based on language
if (language == "en-US") {
    // Do something...
}
```

### <a name="adding-user-specific-vocabulary"></a>Добавление пользовательского словаря

Пользовательский словарь будет предоставлять слова или фразы, уникальные для отдельных пользователей приложения. Они будут предоставлены во время выполнения из основного приложения (а не из расширений приложения) в виде упорядоченного набора терминов, упорядоченного в наиболее значимом приоритете использования для пользователей, с наиболее важными условиями в начале списка.

Пользовательский словарь должен принадлежать к одной из следующих категорий:

- Имена контактов (которые не управляются платформой контактов).
- Теги фото.
- Названия фотоальбомов.
- Имена тренировок.

При выборе терминологии для регистрации в качестве пользовательского словаря следует выбирать только те термины, которые могут быть непонятными для пользователя, не знакомого с приложением. Никогда не зарегистрируйте такие общие термины, как «моя тренировка» или «мой альбом». Например, приложение Монкэйчат будет регистрировать псевдонимы, связанные с каждым контактом в адресной книге пользователя.

Приложение предоставляет пользовательский словарь с помощью вызова `SetVocabularyStrings` метода `INVocabulary` класса и передачи `NSOrderedSet` коллекции из основного приложения. Приложение всегда должно `RemoveAllVocabularyStrings` сначала вызывать метод, чтобы удалить все существующие условия перед добавлением новых. Пример:

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Foundation;
using Intents;

namespace MonkeyChatCommon
{
    public class MonkeyAddressBook : NSObject
    {
        #region Computed Properties
        public List<MonkeyContact> Contacts { get; set; } = new List<MonkeyContact> ();
        #endregion

        #region Constructors
        public MonkeyAddressBook ()
        {
        }
        #endregion

        #region Public Methods
        public NSOrderedSet<NSString> ContactNicknames ()
        {
            var nicknames = new NSMutableOrderedSet<NSString> ();

            // Sort contacts by the last time used
            var query = Contacts.OrderBy (contact => contact.LastCalledOn);

            // Assemble ordered list of nicknames by most used to least
            foreach (MonkeyContact contact in query) {
                nicknames.Add (new NSString (contact.ScreenName));
            }

            // Return names
            return new NSOrderedSet<NSString> (nicknames.AsSet ());
        }

        // This method MUST only be called on a background thread!
        public void UpdateUserSpecificVocabulary ()
        {
            // Clear any existing vocabulary
            INVocabulary.SharedVocabulary.RemoveAllVocabularyStrings ();

            // Register new vocabulary
            INVocabulary.SharedVocabulary.SetVocabularyStrings (ContactNicknames (), INVocabularyStringType.ContactName);
        }
        #endregion
    }
}
```

С помощью этого кода его можно вызвать следующим образом:

```csharp
using System;
using System.Threading;
using UIKit;
using MonkeyChatCommon;
using Intents;

namespace MonkeyChat
{
    public partial class ViewController : UIViewController
    {
        #region AppDelegate Access
        public AppDelegate ThisApp {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do we have access to Siri?
            if (INPreferences.SiriAuthorizationStatus == INSiriAuthorizationStatus.Authorized) {
                // Yes, update Siri's vocabulary
                new Thread (() => {
                    Thread.CurrentThread.IsBackground = true;
                    ThisApp.AddressBook.UpdateUserSpecificVocabulary ();
                }).Start ();
            }
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

> [!IMPORTANT]
> Siri рассматривает пользовательский словарь в качестве подсказок и будет включать в себя максимально возможную терминологию. Тем не менее пространство для пользовательского словаря ограничено, поэтому важно зарегистрировать _только_ терминологию, которая может показаться запутанной, и, таким образом, сохранить общее число зарегистрированных терминов как минимум.

Дополнительные сведения см. в [справочнике по словарю для отдельных пользователей](~/ios/platform/sirikit/understanding-sirikit.md) и [справочнике по пользовательскому словарю](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1)Apple.

### <a name="adding-app-specific-vocabulary"></a>Добавление словаря для конкретного приложения

Словарь для конкретного приложения определяет конкретные слова и фразы, которые будут известны всем пользователям приложения, таким как типы транспортных средств или имена тренировок. Поскольку они являются частью приложения, они определяются в `AppIntentVocabulary.plist` файле как часть основного пакета приложений. Кроме того, эти слова и фразы должны быть локализованы.

Термины словаря для конкретного приложения должны принадлежать к одной из следующих категорий:

- Параметры.
- Имена тренировок.

Файл словаря для конкретного приложения содержит два ключа корневого уровня:

- `ParameterVocabularies`**Обязательный** параметр. определяет пользовательские термины и параметры намерения приложения, к которым они применяются.
- `IntentPhrases`**Необязательный** — содержит примеры фраз, в которых используются пользовательские термины, определенные в `ParameterVocabularies` .

Каждая запись в `ParameterVocabularies` должна содержать строку идентификатора, термин и цель, к которым применяется термин. Кроме того, один термин может применяться к нескольким смыслам.

Полный список допустимых значений и требуемой структуры файлов см. в [справочнике по формату файлов словаря приложения](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1)Apple.

Чтобы добавить `AppIntentVocabulary.plist` файл в проект приложения, выполните следующие действия.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Щелкните правой кнопкой мыши имя проекта в **Обозреватель решений** и выберите **Добавить**  >  **новый файл...**  >  **iOS**:

    [![Добавление списка свойств](implementing-sirikit-images/plist01.png)](implementing-sirikit-images/plist01.png#lightbox)
2. Дважды щелкните `AppIntentVocabulary.plist` файл в **Обозреватель решений** , чтобы открыть его для редактирования.
3. Нажмите кнопку **+** , чтобы добавить ключ, присвойте **Name** ему имя `ParameterVocabularies` и **введите** `Array` :

    [![Задайте имя Параметервокабулариес и тип в Array.](implementing-sirikit-images/plist02.png)](implementing-sirikit-images/plist02.png#lightbox)
4. Разверните `ParameterVocabularies` и нажмите кнопку **+** и задайте для параметра **тип** значение `Dictionary` :

    [![Задать тип для словаря](implementing-sirikit-images/plist03.png)](implementing-sirikit-images/plist03.png#lightbox)
5. Нажмите кнопку, **+** чтобы добавить новый ключ, присвойте ему **имя** `ParameterNames` и **введите** `Array` :

    [![Задайте имя Параметернамес и тип в Array.](implementing-sirikit-images/plist04.png)](implementing-sirikit-images/plist04.png#lightbox)
6. Нажмите кнопку, **+** чтобы добавить новый ключ с **типом** `String` и значением в качестве одного из доступных имен параметров. Например, `INStartWorkoutIntent.workoutName`:

    [![Ключ Инстартворкаутинтент. Воркаутнаме](implementing-sirikit-images/plist05.png)](implementing-sirikit-images/plist05.png#lightbox)
7. Добавьте `ParameterVocabulary` ключ в `ParameterVocabularies` раздел с **типом** `Array` :

    [![Добавьте ключ Параметервокабулари в ключ Параметервокабулариес с типом массива.](implementing-sirikit-images/plist06.png)](implementing-sirikit-images/plist06.png#lightbox)
8. Добавьте новый ключ с **типом** `Dictionary` :

    [![Добавление нового ключа с типом словаря](implementing-sirikit-images/plist07.png)](implementing-sirikit-images/plist07.png#lightbox)
9. Добавьте `VocabularyItemIdentifier` ключ с **типом** `String` и укажите уникальный идентификатор для термина:

    [![Добавьте ключ Вокабуларитемидентифиер с типом строки и укажите уникальный идентификатор.](implementing-sirikit-images/plist08.png)](implementing-sirikit-images/plist08.png#lightbox)
10. Добавьте `VocabularyItemSynonyms` ключ с **типом** `Array` :

    [![Добавьте ключ Вокабуларитемсинонимс с типом массива.](implementing-sirikit-images/plist09.png)](implementing-sirikit-images/plist09.png#lightbox)
11. Добавьте новый ключ с **типом** `Dictionary` :

    [![Добавление нового ключа с типом словаря](implementing-sirikit-images/plist10.png)](implementing-sirikit-images/plist10.png#lightbox)
12. Добавьте `VocabularyItemPhrase` ключ с **типом** `String` и термином, определяющим приложение:

    [![Добавьте ключ Вокабуларитемфрасе с типом строки и термином, определяющим приложение.](implementing-sirikit-images/plist11.png)](implementing-sirikit-images/plist11.png#lightbox)
13. Добавьте `VocabularyItemPronunciation` ключ с **типом** `String` и фонетическим произношением термина:

    [![Добавьте ключ ВокабуларитемпронунЦиатион с типом строки и фонетическим произношением термина](implementing-sirikit-images/plist12.png)](implementing-sirikit-images/plist12.png#lightbox)
14. Добавьте `VocabularyItemExamples` ключ с **типом** `Array` :

    [![Добавьте ключ Вокабуларитемексамплес с типом массива.](implementing-sirikit-images/plist13.png)](implementing-sirikit-images/plist13.png#lightbox)
15. Добавьте несколько `String` ключей с примером использования термина:

    [![Добавьте несколько строковых ключей с примером использования термина](implementing-sirikit-images/plist14.png)](implementing-sirikit-images/plist14.png#lightbox)
16. Повторите описанные выше действия для всех других настраиваемых условий, которые необходимо определить в приложении.
17. Сверните `ParameterVocabularies` ключ.
18. Добавьте `IntentPhrases` ключ с **типом** `Array` :

    [![Добавьте ключ Интентфрасес с типом массива.](implementing-sirikit-images/plist15.png)](implementing-sirikit-images/plist15.png#lightbox)
19. Добавьте новый ключ с **типом** `Dictionary` :

    [![Добавление нового ключа с типом словаря](implementing-sirikit-images/plist16.png)](implementing-sirikit-images/plist16.png#lightbox)
20. Добавьте `IntentName` ключ с **типом** `String` и намерением для примера:

    [![Добавьте ключ Интентнаме с типом строки и намерением для примера.](implementing-sirikit-images/plist17.png)](implementing-sirikit-images/plist17.png#lightbox)
21. Добавьте `IntentExamples` ключ с **типом** `Array` :

    [![Добавьте ключ Интентексамплес с типом массива.](implementing-sirikit-images/plist18.png)](implementing-sirikit-images/plist18.png#lightbox)
22. Добавьте несколько `String` ключей с примером использования термина:

    [![Добавьте несколько строковых ключей с примером использования термина](implementing-sirikit-images/plist19.png)](implementing-sirikit-images/plist19.png#lightbox)
23. Повторите описанные выше действия для всех целей, которые требуются приложению для указания примера использования.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Щелкните правой кнопкой мыши имя проекта в **Обозреватель решений** и выберите **Добавить > новый элемент... > список свойств Apple > > info. plist**:

    [![Добавить новый info. plist](implementing-sirikit-images/plist01.w157-sml.png)](implementing-sirikit-images/plist01.w157.png#lightbox)

2. Дважды щелкните `AppIntentVocabulary.plist` файл в **Обозреватель решений** , чтобы открыть его для редактирования.
3. Нажмите кнопку **+** , чтобы добавить ключ, присвойте **Name** ему имя `ParameterVocabularies` и **введите** `Array` :

    [![Задайте имя Параметервокабулариес и тип в Array.](implementing-sirikit-images/plist02w.png)](implementing-sirikit-images/plist02w.png#lightbox)
4. Разверните `ParameterVocabularies` и нажмите кнопку **+** и задайте для параметра **тип** значение `Dictionary` :

    [![Задать тип для словаря](implementing-sirikit-images/plist03w.png)](implementing-sirikit-images/plist03w.png#lightbox)
5. Нажмите кнопку, **+** чтобы добавить новый ключ, присвойте ему **имя** `ParameterNames` и **введите** `Array` :

    [![Задайте имя Параметернамес и тип в Array.](implementing-sirikit-images/plist04w.png)](implementing-sirikit-images/plist04w.png#lightbox)
6. Нажмите кнопку, **+** чтобы добавить новый ключ с **типом** `String` и значением в качестве одного из доступных имен параметров. Например, `INStartWorkoutIntent.workoutName`:

    [![Ключ Инстартворкаутинтент. Воркаутнаме](implementing-sirikit-images/plist05w.png)](implementing-sirikit-images/plist05w.png#lightbox)
7. Добавьте `ParameterVocabulary` ключ в `ParameterVocabularies` раздел с **типом** `Array` :

    [![Добавьте ключ Параметервокабулари в ключ Параметервокабулариес с типом массива.](implementing-sirikit-images/plist06w.png)](implementing-sirikit-images/plist06w.png#lightbox)
8. Добавьте новый ключ с **типом** `Dictionary` :

    [![Добавление нового ключа с типом словаря](implementing-sirikit-images/plist07w.png)](implementing-sirikit-images/plist07w.png#lightbox)
9. Добавьте `VocabularyItemIdentifier` ключ с **типом** `String` и укажите уникальный идентификатор для термина:

    [![Добавьте ключ Вокабуларитемидентифиер с типом строки и укажите уникальный идентификатор для термина.](implementing-sirikit-images/plist08w.png)](implementing-sirikit-images/plist08w.png#lightbox)
10. Добавьте `VocabularyItemSynonyms` ключ с **типом** `Array` :

    [![Добавьте ключ Вокабуларитемсинонимс с типом массива.](implementing-sirikit-images/plist09w.png)](implementing-sirikit-images/plist09w.png#lightbox)
11. Добавьте новый ключ с **типом** `Dictionary` :

    [![Добавление нового ключа с типом словаря](implementing-sirikit-images/plist10w.png)](implementing-sirikit-images/plist10w.png#lightbox)
12. Добавьте `VocabularyItemPhrase` ключ с **типом** `String` и термином, определяющим приложение:

    [![Добавьте ключ Вокабуларитемфрасе с типом строки и термином, определяющим приложение.](implementing-sirikit-images/plist11w.png)](implementing-sirikit-images/plist11w.png#lightbox)
13. Добавьте `VocabularyItemPronunciation` ключ с **типом** `String` и фонетическим произношением термина:

    [![Добавьте ключ ВокабуларитемпронунЦиатион с типом строки и фонетическим произношением термина](implementing-sirikit-images/plist12w.png)](implementing-sirikit-images/plist12w.png#lightbox)
14. Добавьте `VocabularyItemExamples` ключ с **типом** `Array` :

    [![Добавьте ключ Вокабуларитемексамплес с типом массива.](implementing-sirikit-images/plist13w.png)](implementing-sirikit-images/plist13w.png#lightbox)
15. Добавьте несколько `String` ключей с примером использования термина:

    [![Добавьте несколько строковых ключей с примером использования термина](implementing-sirikit-images/plist14w.png)](implementing-sirikit-images/plist14w.png#lightbox)
16. Повторите описанные выше действия для всех других настраиваемых условий, которые необходимо определить в приложении.
17. Сверните `ParameterVocabularies` ключ.
18. Добавьте `IntentPhrases` ключ с **типом** `Array` :

    [![Добавьте ключ Интентфрасес с типом массива.](implementing-sirikit-images/plist15w.png)](implementing-sirikit-images/plist15w.png#lightbox)
19. Добавьте новый ключ с **типом** `Dictionary` :

    [![Добавление нового ключа с типом словаря](implementing-sirikit-images/plist16w.png)](implementing-sirikit-images/plist16w.png#lightbox)
20. Добавьте `IntentName` ключ с **типом** `String` и намерением для примера:

    [![Добавьте ключ Интентнаме с типом строки и намерением для примера.](implementing-sirikit-images/plist17w.png)](implementing-sirikit-images/plist17w.png#lightbox)
21. Добавьте `IntentExamples` ключ с **типом** `Array` :

    [![Добавьте ключ Интентексамплес с типом массива.](implementing-sirikit-images/plist18w.png)](implementing-sirikit-images/plist18w.png#lightbox)
22. Добавьте несколько `String` ключей с примером использования термина:

    [![Добавьте несколько строковых ключей с примером использования термина](implementing-sirikit-images/plist19w.png)](implementing-sirikit-images/plist19w.png#lightbox)
23. Повторите описанные выше действия для всех целей, которые требуются приложению для указания примера использования.

-----

> [!IMPORTANT]
> `AppIntentVocabulary.plist`Будет зарегистрировано в Siri на тестовых устройствах во время разработки, и в Siri может потребоваться некоторое время для включения пользовательского словаря. В результате тест-инженер должен подождать несколько минут, прежде чем пытаться протестировать словарь конкретного приложения при его обновлении.

Дополнительные сведения см. в [справочнике по словарю для конкретного приложения](~/ios/platform/sirikit/understanding-sirikit.md) и [справочнике по пользовательскому словарю](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1)Apple.

## <a name="adding-an-intents-extension"></a>Добавление расширения «удержания»

Теперь, когда приложение подготовлено к внедрению SiriKit, разработчику необходимо добавить в решение один (или несколько) целей для обработки целей, необходимых для интеграции Siri.

Для каждого необходимого расширения целей выполните следующие действия.

- Добавьте проект расширения «удержания» в решение приложения Xamarin. iOS.
- Настройте файл расширения целей `Info.plist` .
- Измените класс main расширения для целей.

Дополнительные сведения см. [в справочнике по расширениям целей](~/ios/platform/sirikit/understanding-sirikit.md) и [в статье Создание ссылки на расширение целей](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CreatingtheIntentsExtension.html#//apple_ref/doc/uid/TP40016875-CH4-SW1)компании Apple.

### <a name="creating-the-extension"></a>Создание расширения

Чтобы добавить в решение расширение «удержания», выполните следующие действия.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Щелкните правой кнопкой мыши **имя решения** в **панель решения** и выберите **добавить**  >  **Добавить новый проект...**.
2. В диалоговом окне выберите **iOS**  >  **Extensions**  >  **расширение с намерением** расширения iOS и нажмите кнопку **Далее** : 

    [![Выбор расширения намерения](implementing-sirikit-images/intents05.png)](implementing-sirikit-images/intents05.png#lightbox)
3. Затем введите **имя** для расширения намерения и нажмите кнопку **Далее** : 

    [![Введите имя для расширения намерения](implementing-sirikit-images/intents06.png)](implementing-sirikit-images/intents06.png#lightbox)
4. Наконец, нажмите кнопку " **создать** ", чтобы добавить расширение намерения в решение "приложения": 

    [![Добавление расширения намерения в решение "приложения"](implementing-sirikit-images/intents07.png)](implementing-sirikit-images/intents07.png#lightbox)
5. В **Обозреватель решений**щелкните правой кнопкой мыши папку **References** созданного расширения намерения. Проверьте имя общего проекта библиотеки общих кодов (созданного ранее приложения) и нажмите кнопку **ОК** : 

    [![Выберите имя общего проекта библиотеки общих кодов](implementing-sirikit-images/intents08.png)](implementing-sirikit-images/intents08.png#lightbox)
    
# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Щелкните правой кнопкой мыши **имя решения** в **Обозреватель решений** и выберите **добавить**  >  **Добавить новый проект...**.
2. В диалоговом окне выберите **Visual C# > расширения iOS > расширение намерения** и нажмите кнопку **Далее** :

    [![Выбор расширения намерения](implementing-sirikit-images/intents05.w157-sml.png)](implementing-sirikit-images/intents05.w157.png#lightbox)
3. Затем введите **имя** для расширения намерения и нажмите кнопку **ОК** .
4. В **Обозреватель решений**щелкните правой кнопкой мыши папку **ссылки** созданного расширения "намерения" и выберите команду **Добавить > ссылку**. Проверьте имя общего проекта библиотеки общих кодов (созданного ранее приложения) и нажмите кнопку **ОК** :

    [![Выберите имя общего проекта библиотеки общих кодов](implementing-sirikit-images/intents08w.png)](implementing-sirikit-images/intents08w.png#lightbox)
    
-----

Повторите эти действия для числа расширений намерения (на основе [архитектуры приложения для расширений](#architecting-the-app-for-extensions) выше), которые потребуется приложению.

### <a name="configuring-the-infoplist"></a>Настройка info. plist

Для каждого из расширений целей, добавленных в решение приложения, необходимо настроить `Info.plist` файлы для работы с приложением.

Как и любое стандартное расширение приложения, у приложения будут существующие ключи `NSExtension` и `NSExtensionAttributes` . Для расширения целей необходимо настроить два новых атрибута:

[![Два новых атрибута, которые должны быть настроены](implementing-sirikit-images/intents01.png)](implementing-sirikit-images/intents01.png#lightbox)

- **Интентссуппортед** — является обязательным и состоит из массива имен намерений, которые приложение желает поддерживать из расширения намерения.
- **Интентсрестриктедвхилелоккед** — необязательный ключ, позволяющий приложению указать поведение расширения экрана блокировки. Он состоит из массива имен намерений, которым приложение хочет потребовать от пользователя войти в систему для использования из расширения намерения.

Чтобы настроить файл расширения намерения `Info.plist` , дважды щелкните его в **Обозреватель решений** , чтобы открыть его для редактирования. Затем переключитесь в представление **исходного кода** , а затем `NSExtension` разверните `NSExtensionAttributes` ключи и в редакторе:

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Ключи Нсекстенсион и Нсекстенсионаттрибутес в редакторе](implementing-sirikit-images/intents02.png)](implementing-sirikit-images/intents02.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Ключи Нсекстенсион и Нсекстенсионаттрибутес в редакторе](implementing-sirikit-images/intents02w.png)](implementing-sirikit-images/intents02w.png#lightbox)

-----

Разверните `IntentsSupported` ключ и добавьте имя любого класса намерения, который будет поддерживаться этим расширением. Для примера приложения Монкэйчат он поддерживает `INSendMessageIntent` :

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Ключ Инсендмессажеинтент](implementing-sirikit-images/intents09.png)](implementing-sirikit-images/intents09.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Ключ Инсендмессажеинтент](implementing-sirikit-images/intents09w.png)](implementing-sirikit-images/intents09w.png#lightbox)

-----

Если приложение дополнительно требует, чтобы пользователь вошел в систему на устройстве, чтобы использовать заданную цель, разверните `IntentRestrictedWhileLocked` ключ и добавьте имена классов для целей с ограниченным доступом. Для примера приложения Монкэйчат пользователь должен войти в систему, чтобы отправить сообщение разговора, чтобы мы добавили `INSendMessageIntent` :

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Добавленный ключ Инсендмессажеинтент](implementing-sirikit-images/intents10.png)](implementing-sirikit-images/intents10.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Добавленный ключ Инсендмессажеинтент](implementing-sirikit-images/intents10w.png)](implementing-sirikit-images/intents10w.png#lightbox)

-----

Полный список доступных доменов с намерением см. в [справочнике по доменам намерения](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)Apple.

### <a name="configuring-the-main-class"></a>Настройка класса Main

Далее разработчику потребуется настроить основной класс, который выступает в качестве основной точки входа для расширения намерения в Siri. Он должен быть подклассом, `INExtension` который соответствует `IINIntentHandler` делегату. Пример:

```csharp
using System;
using System.Collections.Generic;

using Foundation;
using Intents;

namespace MonkeyChatIntents
{
    [Register ("IntentHandler")]
    public class IntentHandler : INExtension, IINSendMessageIntentHandling, IINSearchForMessagesIntentHandling, IINSetMessageAttributeIntentHandling
    {
        #region Constructors
        protected IntentHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override NSObject GetHandler (INIntent intent)
        {
            // This is the default implementation.  If you want different objects to handle different intents,
            // you can override this and return the handler you want for that particular intent.

            return this;
        }
        #endregion
        ...
    }
}
```

Существует один метод одиночные, который приложение должно реализовать в основном классе с намерением расширения, `GetHandler` методе. Этот метод передается с намерением SiriKit, и приложение должно возвращать **обработчик намерения** , соответствующий типу данного намерения.

Так как пример приложения Монкэйчат обрабатывает только одно намерение, оно возвращается в `GetHandler` метод. Если расширение обрабатывает несколько намерений, разработчик добавляет класс для каждого типа намерения и возвращает экземпляр на основе `Intent` переданного в метод метода.

### <a name="handling-the-resolve-stage"></a>Обработка этапа разрешения

На этапе разрешения вы узнаете, что расширение намерения проверит и проверит параметры, переданные из Siri и установленные с помощью диалога пользователя.

Для каждого параметра, отправляемого из Siri, существует `Resolve` метод. Приложению потребуется реализовать этот метод для каждого параметра, который может потребоваться приложению Siri для получения правильного ответа от пользователя.

В случае с примером приложения Монкэйчат в качестве расширения намерения потребуется один или несколько получателей, которым будет отправлено сообщение. Для каждого получателя в списке расширение должно выполнить поиск контакта, который может иметь следующий результат:

- Найден ровно один совпадающий контакт.
- Найдено два или более совпадающих контактов.
- Соответствующие контакты не найдены.

Кроме того, для Монкэйчат требуется содержимое текста сообщения. Если пользователь не предоставил этого, Siri должен запросить у пользователя содержимое.

В каждом из этих случаев должно быть корректно обработано расширение намерения.

```csharp
[Export ("resolveRecipientsForSearchForMessages:withCompletion:")]
public void ResolveRecipients (INSendMessageIntent intent, Action<INPersonResolutionResult []> completion)
{
    var recipients = intent.Recipients;
    // If no recipients were provided we'll need to prompt for a value.
    if (recipients.Length == 0) {
        completion (new INPersonResolutionResult [] { (INPersonResolutionResult)INPersonResolutionResult.NeedsValue });
        return;
    }

    var resolutionResults = new List<INPersonResolutionResult> ();

    foreach (var recipient in recipients) {
        var matchingContacts = new INPerson [] { recipient }; // Implement your contact matching logic here to create an array of matching contacts
        if (matchingContacts.Length > 1) {
            // We need Siri's help to ask user to pick one from the matches.
            resolutionResults.Add (INPersonResolutionResult.GetDisambiguation (matchingContacts));
        } else if (matchingContacts.Length == 1) {
            // We have exactly one matching contact
            resolutionResults.Add (INPersonResolutionResult.GetSuccess (recipient));
        } else {
            // We have no contacts matching the description provided
            resolutionResults.Add ((INPersonResolutionResult)INPersonResolutionResult.Unsupported);
        }
    }

    completion (resolutionResults.ToArray ());
}

[Export ("resolveContentForSendMessage:withCompletion:")]
public void ResolveContent (INSendMessageIntent intent, Action<INStringResolutionResult> completion)
{
    var text = intent.Content;
    if (!string.IsNullOrEmpty (text))
        completion (INStringResolutionResult.GetSuccess (text));
    else
        completion ((INStringResolutionResult)INStringResolutionResult.NeedsValue);
}
```

Дополнительные сведения см. в справочнике [по этапам разрешения](~/ios/platform/sirikit/understanding-sirikit.md) и об [устранении и обработке целей](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ResolvingandHandlingIntents.html#//apple_ref/doc/uid/TP40016875-CH5-SW1)Apple.

### <a name="handling-the-confirm-stage"></a>Обработка этапа подтверждения

На этапе Confirm выполняется проверка того, что расширение намерения проверяет наличие всех данных для выполнения запроса пользователя. Приложение хочет отправить подтверждение, а также все вспомогательные сведения о том, что будет происходить в Siri, чтобы его можно было подтвердить с помощью пользователя, который является предполагаемым действием.

```csharp
[Export ("confirmSendMessage:completion:")]
public void ConfirmSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Verify user is authenticated and the app is ready to send a message.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Ready, userActivity);
    completion (response);
}
```

Дополнительные сведения см. [в справочнике по этапам подтверждения](~/ios/platform/sirikit/understanding-sirikit.md).

### <a name="processing-the-intent"></a>Обработка намерения

Это точка, в которой расширение цели фактически выполняет задачу по выполнению запроса пользователя и передает результаты обратно в Siri, чтобы пользователь мог получить уведомление.

```csharp
public void HandleSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Implement the application logic to send a message here.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, userActivity);
    completion (response);
}

public void HandleSearchForMessages (INSearchForMessagesIntent intent, Action<INSearchForMessagesIntentResponse> completion)
{
    // Implement the application logic to find a message that matches the information in the intent.
    ...

    var userActivity = new NSUserActivity (nameof (INSearchForMessagesIntent));
    var response = new INSearchForMessagesIntentResponse (INSearchForMessagesIntentResponseCode.Success, userActivity);

    // Initialize with found message's attributes
    var sender = new INPerson (new INPersonHandle ("sarah@example.com", INPersonHandleType.EmailAddress), null, "Sarah", null, null, null);
    var recipient = new INPerson (new INPersonHandle ("+1-415-555-5555", INPersonHandleType.PhoneNumber), null, "John", null, null, null);
    var message = new INMessage ("identifier", "I am so excited about SiriKit!", NSDate.Now, sender, new INPerson [] { recipient });
    response.Messages = new INMessage [] { message };
    completion (response);
}

public void HandleSetMessageAttribute (INSetMessageAttributeIntent intent, Action<INSetMessageAttributeIntentResponse> completion)
{
    // Implement the application logic to set the message attribute here.
    ...

    var userActivity = new NSUserActivity (nameof (INSetMessageAttributeIntent));
    var response = new INSetMessageAttributeIntentResponse (INSetMessageAttributeIntentResponseCode.Success, userActivity);
    completion (response);
}
```

Дополнительные сведения см. [в справочнике по этапам работы с маркерами](~/ios/platform/sirikit/understanding-sirikit.md).

## <a name="adding-an-intents-ui-extension"></a>Добавление расширения пользовательского интерфейса для целей

Дополнительное расширение пользовательского интерфейса для целей предоставляет возможность перенести пользовательский интерфейс приложения и фирменную символику в Siriный интерфейс и заставить пользователей подключаться к приложению. Благодаря этому расширению приложение может перенести торговую марку, а также визуальную и другую информацию в запись.

[![Пример выходных данных расширения пользовательского интерфейса](implementing-sirikit-images/intentsui01.png)](implementing-sirikit-images/intentsui01.png#lightbox)

Как и расширение «удержания», разработчик выполняет следующий шаг для расширения пользовательского интерфейса для целей.

- Добавьте проект расширения пользовательского интерфейса «удержания» в решение приложения Xamarin. iOS.
- Настройте файл расширения пользовательского интерфейса для целей `Info.plist` .
- Измените класс main расширения пользовательского интерфейса для целей.

Дополнительные сведения см. [в справочнике по расширениям пользовательского интерфейса для целей](~/ios/platform/sirikit/understanding-sirikit.md) , а Apple [предоставляет ссылку на пользовательский интерфейс](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ProvidingaCustomInterface.html#//apple_ref/doc/uid/TP40016875-CH7-SW1).

### <a name="creating-the-extension"></a>Создание расширения

Чтобы добавить в решение расширение пользовательского интерфейса для целей, выполните следующие действия.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Щелкните правой кнопкой мыши **имя решения** в **панель решения** и выберите **добавить**  >  **Добавить новый проект...**.
2. В диалоговом окне выберите расширения **iOS**с  >  **Extensions**  >  **намерением расширения пользовательского интерфейса** и нажмите кнопку **Далее** : 

    [![Выбрать расширение пользовательского интерфейса намерения](implementing-sirikit-images/intents11.png)](implementing-sirikit-images/intents11.png#lightbox)
3. Затем введите **имя** для расширения намерения и нажмите кнопку **Далее** : 

    [![Введите имя для расширения намерения](implementing-sirikit-images/intents12.png)](implementing-sirikit-images/intents12.png#lightbox)
4. Наконец, нажмите кнопку " **создать** ", чтобы добавить расширение намерения в решение "приложения": 

    [![Добавление расширения намерения в решение "приложения"](implementing-sirikit-images/intents13.png)](implementing-sirikit-images/intents13.png#lightbox)
5. В **Обозреватель решений**щелкните правой кнопкой мыши папку **References** созданного расширения намерения. Проверьте имя общего проекта библиотеки общих кодов (созданного ранее приложения) и нажмите кнопку **ОК** : 

    [![Выберите имя общего проекта библиотеки общих кодов](implementing-sirikit-images/intents14.png)](implementing-sirikit-images/intents14.png#lightbox)
    
# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Щелкните правой кнопкой мыши **имя решения** в **Обозреватель решений** и выберите **добавить**  >  **Добавить новый проект...**
2. В диалоговом окне выберите расширения **iOS**с  >  **Extensions**  >  **намерением расширения пользовательского интерфейса** и нажмите кнопку **Далее** .
3. Затем введите **имя** для расширения намерения и нажмите кнопку **ОК** .
4. В **Обозреватель решений**щелкните правой кнопкой мыши папку **References** созданного расширения намерения. Проверьте имя общего проекта библиотеки общих кодов (созданного ранее приложения) и нажмите кнопку **ОК** .
    
-----

### <a name="configuring-the-infoplist"></a>Настройка info. plist

Настройте файл расширения пользовательского интерфейса целей `Info.plist` для работы с приложением.

Как и любое стандартное расширение приложения, у приложения будут существующие ключи `NSExtension` и `NSExtensionAttributes` . Для расширения целей существует один новый атрибут, который должен быть настроен:

[![Один новый атрибут, который должен быть настроен](implementing-sirikit-images/intents03.png)](implementing-sirikit-images/intents03.png#lightbox)

**Интентссуппортед** является обязательным и состоит из массива имен намерений, которые приложение должно поддерживать из расширения намерения.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Чтобы настроить файл с намерением расширения пользовательского интерфейса `Info.plist` , дважды щелкните его в **Обозреватель решений** , чтобы открыть его для редактирования. Затем переключитесь в представление **исходного кода** , а затем `NSExtension` разверните `NSExtensionAttributes` ключи и в редакторе:

[![Ключи Нсекстенсион и Нсекстенсионаттрибутес в редакторе](implementing-sirikit-images/intents04.png)](implementing-sirikit-images/intents04.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Чтобы настроить файл с намерением расширения пользовательского интерфейса `Info.plist` , дважды щелкните его в **Обозреватель решений** , чтобы открыть его для редактирования. Разверните `NSExtension` разделы и `NSExtensionAttributes` в редакторе:

[![Tне Нсекстенсион и Нсекстенсионаттрибутес ключи в редакторе](implementing-sirikit-images/intents04w.png)](implementing-sirikit-images/intents04w.png#lightbox)

-----

Разверните `IntentsSupported` ключ и добавьте имя любого класса намерения, который будет поддерживаться этим расширением. Для примера приложения Монкэйчат он поддерживает `INSendMessageIntent` :

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Ключ Инсендмессажеинтент](implementing-sirikit-images/intents15.png)](implementing-sirikit-images/intents15.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Ключ Инсендмессажеинтент](implementing-sirikit-images/intents15w.png)](implementing-sirikit-images/intents15w.png#lightbox)

-----

Полный список доступных доменов с намерением см. в [справочнике по доменам намерения](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)Apple.

### <a name="configuring-the-main-class"></a>Настройка класса Main

Настройте основной класс, который выступает в качестве основной точки входа для расширения пользовательского интерфейса намерения в Siri. Он должен быть подклассом, `UIViewController` который соответствует `IINUIHostedViewController` интерфейсу. Пример:

```csharp
using System;
using Foundation;
using CoreGraphics;
using Intents;
using IntentsUI;
using UIKit;

namespace MonkeyChatIntentsUI
{
    public partial class IntentViewController : UIViewController, IINUIHostedViewControlling
    {
        #region Constructors
        protected IntentViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }

        public override void DidReceiveMemoryWarning ()
        {
            // Releases the view if it doesn't have a superview.
            base.DidReceiveMemoryWarning ();

            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Public Methods
        [Export ("configureWithInteraction:context:completion:")]
        public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
        {
            // Do configuration here, including preparing views and calculating a desired size for presentation.

            if (completion != null)
                completion (DesiredSize ());
        }

        [Export ("desiredSize:")]
        public CGSize DesiredSize ()
        {
            return ExtensionContext.GetHostedViewMaximumAllowedSize ();
        }
        #endregion
    }
}
```

Siri передаст `INInteraction` экземпляр класса `Configure` методу `UIViewController` экземпляра внутри НАМЕРЕННОГО расширения пользовательского интерфейса.

`INInteraction`Объект предоставляет модулю три ключевых фрагмента информации:

1. Обрабатываемый объект намерения.
2. Объект отклика намерения из `Confirm` `Handle` методов и для расширения намерения.
3. Состояние взаимодействия, определяющее состояние взаимодействия между приложением и Siri.

`UIViewController`Экземпляр является классом принципов взаимодействия с Siri и, поскольку он наследует от `UIViewController` , он имеет доступ ко всем функциям UIKit.

Когда Siri вызывает `Configure` метод, `UIViewController` он передается в контекст представления, указывая, что контроллер представления будет размещен на карте Siri Сниппит или Maps.

Siri также передает обработчик завершения, который приложение должно вернуть желаемый размер представления после того, как приложение закончит настройку.

### <a name="design-the-ui-in-ios-designer"></a>Разработка пользовательского интерфейса в конструкторе iOS

Макет пользовательского интерфейса расширения пользовательского интерфейса для целей в конструкторе iOS. Дважды щелкните `MainInterface.storyboard` файл расширения в **Обозреватель решений** , чтобы открыть его для редактирования. Перетащите все необходимые элементы пользовательского интерфейса, чтобы создать пользовательский интерфейс и сохранить изменения.

> [!IMPORTANT]
> Хотя можно добавить интерактивные элементы `UIButtons` , например или `UITextFields` в расширение пользовательского интерфейса `UIViewController` , они строго запрещены в качестве намеренного пользовательского интерфейса в неинтерактивном режиме, и пользователь не сможет взаимодействовать с ними.

### <a name="wire-up-the-user-interface"></a>Подключить пользовательский интерфейс

С помощью пользовательского интерфейса расширения пользовательского интерфейса, созданного в конструкторе iOS, измените `UIViewController` подкласс и переопределите `Configure` метод следующим образом:

```csharp
[Export ("configureWithInteraction:context:completion:")]
public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
{
    // Do configuration here, including preparing views and calculating a desired size for presentation.
    ...

    // Return desired size
    if (completion != null)
        completion (DesiredSize ());
}

[Export ("desiredSize:")]
public CGSize DesiredSize ()
{
    return ExtensionContext.GetHostedViewMaximumAllowedSize ();
}
```

### <a name="overriding-the-default-siri-ui"></a>Переопределение пользовательского интерфейса Siri по умолчанию

Расширение пользовательского интерфейса для целей всегда будет отображаться вместе с другим содержимым Siri, таким как значок приложения и имя в верхней части пользовательского интерфейса, или, в конце концов, кнопки (например, "Отправить" или "Отмена") могут отображаться внизу.

Существует несколько экземпляров, в которых приложение может заменить сведения, которые Siri отображается пользователю по умолчанию, такие как обмен сообщениями или карты, в которых приложение может заменить интерфейс по умолчанию на одно приложение, настроенное для приложения.

Если расширение пользовательского интерфейса для целей должно переопределять элементы пользовательского интерфейса Siri по умолчанию, `UIViewController` подкласс должен реализовать `IINUIHostedViewSiriProviding` интерфейс и согласие на отображение определенного элемента интерфейса.

Добавьте следующий код в подкласс, `UIViewController` чтобы сообщить Siri, что расширение пользовательского интерфейса уже отображает содержимое сообщения:

```csharp
public bool DisplaysMessage {
    get {return true;}
}
```

### <a name="considerations"></a>Рекомендации

Компания Apple предлагает разработчикам принимать во внимание следующие соображения, касающиеся проектирования и реализации расширений пользовательского интерфейса.

- **Не следует понимать использование памяти** . так как расширения пользовательского интерфейса намерения являются временными и отображаются только в течение короткого времени, система накладывает более строгие ограничения памяти, чем использование с полным приложением.
- **Рассмотрите минимальные и максимальные размеры представлений** . Убедитесь, что расширения пользовательского интерфейса имеют хорошее значение для каждого типа устройства iOS, его размер и ориентацию. Кроме того, требуемый размер, который приложение отправляет обратно в Siri, может быть не может быть предоставлено.
- **Используйте гибкие и адаптивные шаблоны макета** , чтобы убедиться, что пользовательский интерфейс выглядит отлично на каждом устройстве.

## <a name="summary"></a>Сводка

В этой статье были рассмотрены SiriKit и показано, как ее можно добавить в приложения Xamarin. iOS для предоставления служб, доступных пользователю с помощью Siri и приложения Maps на устройстве iOS.

## <a name="related-links"></a>Связанные ссылки

- [Пример Елизачат](/samples/xamarin/ios-samples/ios10-elizachat)
- [SiriKit по программированию](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Ссылка на платформу целей](https://developer.apple.com/reference/intents)
- [Справочник по структуре пользовательского интерфейса для целей](https://developer.apple.com/reference/intentsui)
- [Справочник по доменам намерения](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)