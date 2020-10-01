---
title: Сочетания клавиш Siri в Xamarin. iOS
description: В этом документе описывается использование сочетаний клавиш Siri в iOS 12. Здесь обсуждаются Нсусерактивитиес, пользовательские намерения, назначение сочетаний голоса, соответствующие сочетания клавиш и многое другое.
ms.prod: xamarin
ms.assetid: 86424F79-3A7D-436E-927D-9A3267DA333B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/08/2018
ms.openlocfilehash: 9ad4a5cfb32aa1256d73eb6c9d2f5fe6953b6852
ms.sourcegitcommit: c7d82b0684b1c26cdcbc16766b636894a64a80fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/01/2020
ms.locfileid: "91607181"
---
# <a name="siri-shortcuts-in-xamarinios"></a>Сочетания клавиш Siri в Xamarin. iOS

В [iOS 10](~/ios/platform/sirikit/index.md)компания Apple представила SiriKit, что позволяет создавать сообщения, вызовы VoIP, платежи, тренировки, а также приложения для поиска фотографий, которые взаимодействуют с Siri.

В [iOS 11](~/ios/platform/introduction-to-ios11/sirikit.md)SiriKit получила поддержку для большего числа типов приложений и большую гибкость при настройке пользовательского интерфейса.

в iOS 12 Добавлено сочетание клавиш Siri, что позволяет всем типам приложений предоставлять свои функции Siri. Siri узнает, когда некоторые задачи на основе приложений наиболее важны для пользователя и используют эти знания для предложения возможных действий с помощью _сочетаний клавиш_. При нажатии сочетания клавиш или при вызове с помощью голоса команда открывает приложение или выполняет фоновую задачу.

Сочетания клавиш следует использовать для ускорения работы пользователя при выполнении общей задачи — во многих случаях даже при отсутствии необходимости открывать рассматриваемое приложение.

## <a name="sample-app-soup-chef"></a>Пример приложения: полный курс Chef

Чтобы лучше понять Siriные сочетания клавиш, ознакомьтесь с примером приложения [полный курс Chef](/samples/xamarin/ios-samples/ios12-soupchef) . Полный курс Chef позволяет пользователям размещать заказы из воображаемого полный курсного ресторана, просматривать историю их заказов и определять фразы, используемые при упорядочении полный курс путем взаимодействия с Siri.

> [!TIP]
> Перед тестированием полный курс Chef на симуляторе или устройстве iOS 12, включите следующие два параметра, которые полезны при отладке сочетаний клавиш:
>
> - В приложении " **Параметры** " включите " **разработчик > отобразить последние ярлыки**".
> - В приложении " **Параметры** " включите **разработчик > отображать пожертвования на экране блокировки**.
>
> Эти параметры отладки упрощают поиск недавно созданных (а не прогнозируемых) ярлыков на экране блокировки iOS и на экране поиска.

Чтобы использовать пример приложения, выполните следующие действия.

- Установите и запустите пример приложения полный курс Chef на симуляторе или на [устройстве](#testing-on-device)iOS 12.
- Нажмите кнопку **+** в правом верхнем углу, чтобы создать новый заказ.
- Выберите тип полный курс, укажите количество и параметры, а затем нажмите кнопку **разместить заказ**.
- На экране **История заказа** коснитесь созданного заказа, чтобы просмотреть сведения о нем.
- В нижней части экрана сведения о заказе выберите **Добавить в Siri**.
- Запишите голосовую фразу, чтобы связать ее с заказом, и коснитесь элемента **Готово**.
- Сократите полный курс Chef, вызовите Siri и поместите заказ еще раз, используя записанную голосовую фразу.
- После того, как Siri завершит заказ, откройте полный курс Chef и обратите внимание, что новый заказ отображается на экране **История заказа** .

В примере приложения показано, как:

- [Чтобы открыть приложение, используйте ярлык нсусерактивити](#using-an-nsuseractivity-shortcut-to-open-an-app).
- [Для выполнения задачи используйте ярлык с пользовательскими намерениями](#using-a-custom-intent-shortcut-to-perform-a-task).
- [Предоставьте пользовательский интерфейс для пользовательской цели](#providing-a-user-interface-for-a-custom-intent).
- [Создайте ярлык голоса](#creating-a-voice-shortcut).

## <a name="infoplist-and-entitlementsplist"></a>Info. plist и прав. plist

Прежде чем продвигаясь вглубь более глубоко в код полный курс Chef, ознакомьтесь с файлами **info. plist** и **. plist** .

### <a name="infoplist"></a>Info.plist

Файл **info. plist** в проекте **Саупчеф** определяет **идентификатор пакета** как `com.xamarin.SoupChef` . Этот идентификатор пакета будет использоваться в качестве префикса для идентификаторов пакетов, используемых в расширениях пользовательского интерфейса для целей и целей, которые обсуждаются далее в этом документе.

Файл **info. plist** также содержит следующую запись:

```xml
<key>NSUserActivityTypes</key>
<array>
    <string>OrderSoupIntent</string>
    <string>com.xamarin.SoupChef.viewMenu</string>
</array>
```

Эта `NSUserActivityTypes` пара "ключ-значение" указывает, что полный курс Chef знает, как обрабатывает `OrderSoupIntent` и [`NSUserActivity`](xref:Foundation.NSUserActivity) имеет [`ActivityType`](xref:Foundation.NSUserActivity.ActivityType) "com. Xamarin. саупчеф. виевмену".

Действия и пользовательские методы, передаваемые в само приложение, в отличие от его расширений, обрабатываются в `AppDelegate` (a [`UIApplicationDelegate`](xref:UIKit.UIApplicationDelegate) [`ContinueUserActivity`](xref:UIKit.UIApplicationDelegate.ContinueUserActivity*) методом.

### <a name="entitlementsplist"></a>Entitlements.plist

Файл прав **. plist** в проекте **саупчеф** содержит следующие записи:

```xml
<key>com.apple.security.application-groups</key>
<array>
    <string>group.com.xamarin.SoupChef</string>
</array>
<key>com.apple.developer.siri</key>
<true/>
```

Эта конфигурация указывает, что приложение использует группу приложений "Group. com. Xamarin. Саупчеф". Расширение приложения **саупчефинтентс** использует ту же группу приложений, которая позволяет двум проектам совместно использовать [`NSUserDefaults`](xref:Foundation.NSUserDefaults)
данных.

`com.apple.developer.siri`Ключ указывает, что приложение взаимодействует с Siri.

> [!NOTE]
> Конфигурация сборки проекта **саупчеф** задает **Настраиваемые** права для прав **. plist**.

## <a name="using-an-nsuseractivity-shortcut-to-open-an-app"></a>Открытие приложения с помощью ярлыка Нсусерактивити

Чтобы создать ярлык, открывающий приложение для отображения определенного содержимого, создайте `NSUserActivity` и прикрепите его к контроллеру представления для экрана, который нужно открыть.

### <a name="setting-up-an-nsuseractivity"></a>Настройка Нсусерактивити

На экране меню `SoupMenuViewController` создает объект `NSUserActivity` и назначает его [`UserActivity`](xref:UIKit.UIResponder.UserActivity) свойству контроллера представления:

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();
    UserActivity = NSUserActivityHelper.ViewMenuActivity;
}
```

Установка свойства подает `UserActivity` действию _значение_ Siri. В этом пожертвовании Siri получает информацию о том, когда и где это действие имеет отношение к пользователю, и узнает, как лучше предлагать его в будущем.

`NSUserActivityHelper` — Это служебный класс, включенный в решение **саупчеф** в библиотеке классов **саупкит** . Он создает `NSUserActivity` и устанавливает различные свойства, связанные с Siri и поиском:

```csharp
public static string ViewMenuActivityType = "com.xamarin.SoupChef.viewMenu";

public static NSUserActivity ViewMenuActivity {
    get
    {
        var userActivity = new NSUserActivity(ViewMenuActivityType)
        {
            Title = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_LUNCH_TITLE", "View menu activity title"),
            EligibleForSearch = true,
            EligibleForPrediction = true
        };

        var attributes = new CSSearchableItemAttributeSet(NSUserActivityHelper.SearchableItemContentType)
        {
            ThumbnailData = UIImage.FromBundle("tomato").AsPNG(),
            Keywords = ViewMenuSearchableKeywords,
            DisplayName = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_LUNCH_TITLE", "View menu activity title"),
            ContentDescription = NSBundleHelper.SoupKitBundle.GetLocalizedString("VIEW_MENU_CONTENT_DESCRIPTION", "View menu content description")
        };
        userActivity.ContentAttributeSet = attributes;

        var phrase = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_LUNCH_SUGGESTED_PHRASE", "Voice shortcut suggested phrase");
        userActivity.SuggestedInvocationPhrase = phrase;
        return userActivity;
    }
}
```

Обратите внимание на следующие особенности.

- Значение `EligibleForPrediction` `true` указывает, что Siri может прогнозировать это действие и отображать его как ярлык.
- [`ContentAttributeSet`](xref:Foundation.NSUserActivity.ContentAttributeSet)Массив является стандартом, [`CSSearchableItemAttributeSet`](xref:CoreSpotlight.CSSearchableItemAttributeSet) используемым для включения `NSUserActivity` в результаты поиска iOS.
- [`SuggestedInvocationPhrase`](xref:Foundation.NSUserActivity.SuggestedInvocationPhrase) — Это фраза, которую Siri предлагает пользователю в качестве потенциального выбора при назначении Фразы ярлыку.

### <a name="handling-an-nsuseractivity-shortcut"></a>Обработка ярлыка Нсусерактивити

Для работы с `NSUserActivity` ярлыком, вызываемым пользователем, приложение iOS должно переопределять `ContinueUserActivity` метод `AppDelegate` класса, реагируя на `ActivityType` поле переданного `NSUserActivity` объекта:

```csharp
public override bool ContinueUserActivity(UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    // ...
    else if (userActivity.ActivityType == NSUserActivityHelper.ViewMenuActivityType)
    {
        HandleUserActivity();
        return true;
    }
    // ...
}
```

Этот метод вызывает `HandleUserActivity` , который находит перехода на экране меню и вызывает его:

```csharp
void HandleUserActivity()
{
    var rootViewController = Window?.RootViewController as UINavigationController;
    var orderHistoryViewController = rootViewController?.ViewControllers?.FirstOrDefault() as OrderHistoryTableViewController;
    if (orderHistoryViewController is null)
    {
        Console.WriteLine("Failed to access OrderHistoryTableViewController.");
        return;
    }
    var segue = OrderHistoryTableViewController.SegueIdentifiers.SoupMenu;
    orderHistoryViewController.PerformSegue(segue, null);
}
```

### <a name="assigning-a-phrase-to-an-nsuseractivity"></a>Назначение фразы Нсусерактивити

Чтобы назначить фразу `NSUserActivity` , откройте приложение iOS **Settings** и выберите **Siri & Search > Мои ярлыки**. Затем выберите сочетание клавиш (в данном случае «заказ обедов») и запишите фразу.

При вызове Siri и использовании этой фразы в окне меню откроется полный курс Chef.

## <a name="using-a-custom-intent-shortcut-to-perform-a-task"></a>Использование ярлыка с пользовательскими намерениями для выполнения задачи

### <a name="defining-a-custom-intent"></a>Определение пользовательской цели

Чтобы предоставить ярлык, позволяющий пользователю быстро завершить определенную задачу, связанную с вашим приложением, создайте пользовательскую намерение. Пользовательская цель представляет задачу, которую может захотеть выполнить пользователь, параметры, относящиеся к этой задаче, и потенциальные ответы, полученные в результате выполнения задачи. В зависимости от того, как определена пользовательская цель, ее вызов может либо открыть приложение, либо выполнить фоновую задачу.

Используйте Xcode 10 для создания пользовательских целей. В [репозитории саупчеф](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef)пользовательская цель определяется в **Ордерсаупинтенткодежен**, проекте цели-C. Откройте этот проект и выберите в **навигаторе проекта** файл **интентдефинитион.** Files, чтобы просмотреть намерение **ордерсауп** .

Обратите внимание на следующие возможности.

- Цель имеет **категорию** **Order**. Существуют различные предварительно определенные категории, которые можно использовать для пользовательских целей. Выберите тот, который наиболее полно соответствует задаче, которую включит пользовательская цель. Так как это решение является полный курс приложением, **ордерсаупинтент** использует **Order**.
- Флажок **подтверждения** указывает, должно ли Siri запрашивать подтверждение перед выполнением задачи. В полный курс Chef с намерением **Order полный курс** этот параметр включен, так как пользователь делает покупку.
- В разделе **Parameters** файла **. интентдефинитион** определяются параметры, относящиеся к ярлыку. Чтобы поместить заказ полный курс, полный курс Chef должен иметь сведения о типе полный курс, его количестве и связанных параметрах.
Каждый параметр имеет тип; параметр, который не может быть представлен предопределенным типом, задан как **Пользовательский**.
- Интерфейс **типов ярлыков** описывает различные сочетания параметров, которые Siri могут использовать при предложении сочетания клавиш. Связанные разделы **Title** и **субтитры** позволяют определить сообщения, которые Siri будут использовать при предоставлении пользователю предложенного ярлыка.
- Флажок **Поддержка фонового выполнения** должен быть установлен для любого ярлыка, который может быть выполнен без открытия приложения для дальнейшего взаимодействия с пользователем.

### <a name="defining-custom-intent-responses"></a>Определение ответов с пользовательскими намерениями

Элемент **ответа** , вложенный ниже цели **ордерсауп** , представляет потенциальные ответы, являющиеся результатом полный курс заказа.

В определении ответа цели **ордерсауп** Обратите внимание на следующие особенности.

- **Свойства** ответа можно использовать для настройки сообщения, переданного пользователю. Ответ намерения **ордерсауп** имеет свойства **полный курс** и **время ожидания** .
- **Шаблоны ответов** указывают различные сообщения об успешном выполнении и сбоях, которые можно использовать для указания состояния после завершения задачи намерения.
- Для ответов, указывающих на успешное выполнение, должен быть выбран флажок **успешно** .
- Ответ об успешном выполнении **ордерсаупинтент** использует свойства **полный курс** и **время ожидания** , чтобы предоставить понятное и полезное сообщение, описывающее, когда заказ полный курс будет готов.

### <a name="generating-code-for-the-custom-intent"></a>Создание кода для пользовательской цели

Создание проекта Xcode, содержащего это определение пользовательской намерения, приводит к тому, что Xcode создает код, который можно использовать для программного взаимодействия с пользовательской целью и его ответами.

Чтобы просмотреть созданный код, сделайте следующее:

- Откройте **AppDelegate. m**.
- Добавьте импорт в файл заголовка пользовательской цели: `#import "OrderSoupIntent.h"`
- В любом методе в классе добавьте ссылку на `OrderSoupIntent` .
- Щелкните правой кнопкой мыши `OrderSoupIntent` и выберите **Перейти к определению**.
- Щелкните правой кнопкой мыши вновь открытый файл **ордерсаупинтент. h**и выберите команду " **отобразить в Finder**".
- При этом откроется окно **Finder** , содержащее файл с **расширением h** и **m** , содержащий созданный код.

Этот созданный код включает:

- `OrderSoupIntent` — Класс, представляющий пользовательскую намерение.
- `OrderSoupIntentHandling` — Протокол, определяющий методы, которые будут использоваться для подтверждения выполнения намерений и метода, который фактически выполняет его.
- `OrderSoupIntentResponseCode` — Перечисление, определяющее различные состояния ответа.
- `OrderSoupIntentResponse` — класс, представляющий ответ на выполнение намерения.

### <a name="creating-a-binding-to-the-custom-intent"></a>Создание привязки к пользовательской цели

Чтобы использовать код, созданный Xcode в приложении Xamarin. iOS, создайте для него привязку C#.

#### <a name="creating-a-static-library-and-c-binding-definitions"></a>Создание статической библиотеки и определений привязок C#

В [репозитории саупчеф](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef)просмотрите папку **ордерсаупинтентстатиклиб** и откройте проект **ордерсаупинтентстатиклиб. xcodeproj** Xcode.

Этот проект **статической библиотеки Cocoa Touch** содержит файлы **ордерсаупинтент. h** и **Ордерсаупинтент. m** , созданные Xcode.

#### <a name="configuring-the-static-library-project-build-settings"></a>Настройка параметров сборки проекта статической библиотеки

В **навигаторе проекта**Xcode выберите проект верхнего уровня **ордерсаупинтентстатиклиб**и перейдите к **этапам сборки > источники компиляции**.
Обратите внимание, что **ордерсаупинтент. m** (импортируемый **ордерсаупинтент. h**) указан здесь. Обратите внимание, что в **Link binary с библиотеками**включены. **Framework** и **Foundation. Framework** .
После этих настроек платформа будет правильно построена.

#### <a name="building-the-static-library-and-generating-c-bindings-definitions"></a>Построение статической библиотеки и создание определений привязок C#

Чтобы создать статическую библиотеку и создать для нее определения привязок C#, выполните следующие действия.

- [Установите целевое Шарпие](../../../cross-platform/macios/binding/objective-sharpie/get-started.md?context=xamarin%252fmac#installing-objective-sharpie), инструмент, используемый для создания определений привязок из файлов **h** и **m** , созданных Xcode.

- Настройте систему для использования **программ командной строки**Xcode 10:

  > [!WARNING]
  > Обновление выбранных **программ командной строки** влияет на все установленные версии Xcode в системе. Когда вы завершите работу с примером приложения полный курс Chef, не забудьте вернуть этот параметр к исходной конфигурации.

  - В Xcode выберите **Xcode > предпочтения > расположения** и задайте для **средств командной строки** самую последнюю установку Xcode 10, доступную в системе.

- В терминале в `cd` каталог **ордерсаупинтентстатиклиб** .

- Тип `make` , который создает:

  - Статическая библиотека **либордерсаупинтентстатиклиб. a**
  - В каталоге **выходных данных, определения** привязок C#:
    - **ApiDefinitions.cs**
    - **StructsAndEnums.cs**

Проект **ордерсаупинтентбиндингс** , который использует эту статическую библиотеку и связанные с ней определения привязок, автоматически создает эти элементы.
Однако выполнение описанного выше процесса вручную обеспечит его сборку должным образом.

Дополнительные сведения о создании статической библиотеки и использовании цели Шарпие для создания определений привязок C# см. в пошаговом руководстве по [привязке к библиотеке цели iOS-C](../binding-objective-c/walkthrough.md?tabs=vsmac#creating-a-static-library) .

#### <a name="creating-a-bindings-library"></a>Создание библиотеки привязок

После создания статической библиотеки и определений привязок C# оставшаяся часть, необходимая для использования кода, созданного Xcode намерением в проекте Xamarin. iOS, является библиотекой привязок.

В [репозитории полный курс Chef](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef)откройте файл **саупчеф. sln** . Помимо прочего, это решение содержит **ордерсаупинтентбиндинг**, библиотеку привязок для статической библиотеки, созданную ранее.

Обратите особое внимание на то, что в этот проект входят:

- **ApiDefinitions.CS** — файл, созданный ранее целевой Шарпие и добавленный в этот проект. Для **действия сборки** этого файла задано значение **обжкбиндингапидефинитион**.
- **StructsAndEnums.CS** — другой файл, созданный ранее целевой Шарпие и добавленный в этот проект. Для **действия сборки** этого файла задано значение **обжкбиндингкоресаурце**.
- **Собственная ссылка** на **либордерсаупинтентстатиклиб. a**, созданная ранее статическая библиотека. Обновите исходные свойства ссылки и укажите следующие значения:

    1. Платформы = `Foundation Intents`
    1. Интеллектуальная связь = `On`
    1. Принудительная Загрузка = `On`
    1. Тип = `Static`

> [!NOTE]
> И **ApiDefinitions.CS** , и **StructsAndEnums.CS** содержат атрибуты, такие как `[Watch (5,0), iOS (12,0)]` . Эти атрибуты, созданные целевым Шарпие, были добавлены в комментарий, так как они не нужны для этого проекта.

Дополнительные сведения о создании библиотеки привязок C# см. в пошаговом руководстве по [привязке цели и библиотеки](../binding-objective-c/walkthrough.md?tabs=vsmac#create-a-xamarinios-binding-project) на языке C для iOS.

Обратите внимание, что проект **саупчеф** содержит ссылку на **ордерсаупинтентбиндинг**, что означает, что теперь он может получить доступ к классам, интерфейсам и перечислениям в C#, которые он содержит:

- `OrderSoupIntent`
- `OrderSoupIntentHandling`
- `OrderSoupIntentResponse`
- `OrderSoupIntenseResponseCode`

### <a name="creating-a-swift-framework"></a>Создание платформы SWIFT

Собственный код определения намерения создается Xcode по умолчанию с использованием языка собственного проекта. Если в проекте SWIFT вы определили файл **. интентдефинитион** , Xcode создаст один файл SWIFT со всеми необходимыми классами, которые можно использовать для создания платформы SWIFT.

> [!TIP]
> Вы можете выбрать нужный язык для созданного кода намерения в параметрах сборки Xcode. Выберите **цель цели > параметры сборки > определение намерения компилятора — создание кода** и выбор SWIFT или цели-C. Вы также можете сделать его автоматическим в соответствии с целевым языком.

Процесс создания платформы SWIFT аналогичен описанному выше.

1. Создайте новый проект платформы SWIFT.
1. Скопируйте автоматически созданный файл SWIFT с кодом намерения в этот проект. Этот файл можно найти, как описано [здесь](#generating-code-for-the-custom-intent).
1. Включите [заголовок моста "цель-c](../binding-swift/walkthrough.md#build-a-native-library)", чтобы платформа была автоматически создана с помощью файла заголовков Required-c Шарпие.

После сборки платформы выполните те [же действия](#creating-a-bindings-library) , которые были описаны ранее, чтобы создать привязку Xamarin. Дополнительные сведения о создании привязки для платформы SWIFT можно узнать [здесь](../binding-swift/walkthrough.md).

### <a name="adding-the-intent-definition-file-to-your-solution"></a>Добавление файла определения намерения в решение

В решении C# **саупчеф** проект **саупкит** содержит код, совместно используемый приложением и его расширениями. Файл **. интентдефинитион** был помещен в каталог **base. лпрож** **Саупкит**и имеет **действие сборки** **Content**. Процесс сборки копирует этот файл в пакет приложений полный курс Chef, где он необходим для правильной работы приложения.

### <a name="donating-an-intent"></a>Пожертвование намерения

Чтобы Siri предложить ярлык, он должен сначала понять, когда это сочетание уместно.

Чтобы дать Siri в этом понимании, полный курс _Chef_ полагает намерения на Siri каждый раз, когда пользователь помещает полный курс заказ. На основе этого пожертвования — когда она была пожертвована, где она была пожертвована, содержащиеся в ней параметры – Siri, когда вы хотите предложить это в будущем.

**Саупчеф** использует `SoupOrderDataManager` класс для размещения пожертвования.
При вызове для размещения полный курс заказа для пользователя `PlaceOrder` метод, в свою очередь, вызывает [`DonateInteraction`](xref:Intents.INInteraction.DonateInteraction*) :

```csharp
void DonateInteraction(Order order)
{
    var interaction = new INInteraction(order.Intent, null);
    interaction.Identifier = order.Identifier.ToString();
    interaction.DonateInteraction((error) =>
    {
        // ...
    });
}
```

После выборке намерения он упаковывается в [`INInteraction`](xref:Intents.INInteraction) .
Получает `INInteraction` объект [`Identifier`](xref:Intents.INInteraction.Identifier*)
Это соответствует уникальному ИДЕНТИФИКАТОРу заказа (он будет полезен позже при удалении пожертвований с намерениями, которые больше не действительны). Затем взаимодействие передается в Siri.

При вызове `order.Intent` метода получения `OrderSoupIntent` , который представляет порядок, задается его `Quantity` изображение, `Soup` , `Options` и, а также фраза вызова для использования в качестве предложения, когда пользователь записывает фразу для Siri, чтобы связать с намерением.

```csharp
public OrderSoupIntent Intent
{
    get
    {
        var orderSoupIntent = new OrderSoupIntent();
        orderSoupIntent.Quantity = new NSNumber(Quantity);
        orderSoupIntent.Soup = new INObject(MenuItem.ItemNameKey, MenuItem.LocalizedString);

        var image = UIImage.FromBundle(MenuItem.IconImageName);
        if (!(image is null))
        {
            var data = image.AsPNG();
            orderSoupIntent.SetImage(INImage.FromData(data), "soup");
        }

        orderSoupIntent.Options = MenuItemOptions
            .ToArray<MenuItemOption>()
            .Select<MenuItemOption, INObject>(arg => new INObject(arg.Value, arg.LocalizedString))
            .ToArray<INObject>();

        var comment = "Suggested phrase for ordering a specific soup";
        var phrase = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_SOUP_SUGGESTED_PHRASE", comment);
        orderSoupIntent.SuggestedInvocationPhrase = String.Format(phrase, MenuItem.LocalizedString);

        return orderSoupIntent;
    }
}
```

#### <a name="removing-invalid-donations"></a>Удаление недопустимых пожертвований

Важно удалить пожертвования, которые больше не являются допустимыми, чтобы Siri не помогая или путаницу в сокращенных предложениях.

В полный курс Chef экран **меню Настройка** можно использовать для пометки элемента меню как недоступного. Siri больше не должен предлагать ярлык для заказа недоступного пункта меню, поэтому `RemoveDonation` метод `SoupMenuManager` удаления пожертвования для пунктов меню, которые больше не доступны. Приложение реализует эту функцию следующим образом.

- Поиск заказов, связанных с пунктом меню "сейчас недоступно".
- Получение идентификаторов.
- Удаление взаимодействий с одинаковыми идентификаторами.

```csharp
void RemoveDonation(MenuItem menuItem)
{
    if (!menuItem.IsAvailable)
    {
        Order[] orderHistory = OrderManager?.OrderHistory.ToArray<Order>();
        if (orderHistory is null)
        {
            return;
        }

        string[] orderIdentifiersToRemove = orderHistory
            .Where<Order>((order) => order.MenuItem.ItemNameKey == menuItem.ItemNameKey)
            .Select<Order, string>((order) => order.Identifier.ToString())
            .ToArray<string>();

        INInteraction.DeleteInteractions(orderIdentifiersToRemove, (error) =>
        {
            if (!(error is null))
            {
                Console.WriteLine($"Failed to delete interactions with error {error.ToString()}");
            }
            else
            {
                Console.WriteLine("Successfully deleted interactions");
            }
        });
    }
}
```

#### <a name="validating-successful-donations"></a>Проверка успешных пожертвований

Решение включает несколько проектов и определенную конфигурацию. В некоторых случаях приложение может аварийно завершить работу из-за неполной настройки, в других случаях оно может не дать никаких сообщений о взаимодействии. Важно проверить успешность пожертвования и параметры разработчиков iOS. Перейдите к разделу **параметры > Developer** и включите следующие параметры для разработчиков, чтобы просмотреть последние пожертвования и сочетания клавиш:

- Отображение последних сочетаний клавиш
- Отображение пожертвований на экране блокировки

После включения все успешные пожертвования будут отображаться на экране блокировки и ниже параметров Siri. Если после запуска приложения вы не видите пожертвования, ознакомьтесь со следующими вариантами устранения неполадок:

1. Приложению не удается создать исключение `OrderSoupIntent` со следующей ошибкой:

    > Не удалось создать собственный экземпляр типа "Нативелибрари. Ордерсаупинтент": собственный класс не загружен.

    Эта ошибка означает, что Xamarin не сможет загрузить собственный класс через привязку Xamarin. Чтобы устранить эту проблему, убедитесь, что собственная библиотека содержит необходимый код, на который ссылается проект привязки, а также установлены правильные флаги, как описано [здесь](#creating-a-bindings-library), установите `Force Load` для флага значение `On` .

1. Приложению не удается инициализировать загруженный машинный экземпляр класса намерения со следующей ошибкой:

    > Не удалось инициализировать экземпляр типа "Нативелибрари. Ордерсаупинтент": собственный метод "Init" вернул nil.

    Эта ошибка связана с отсутствующим файлом определения намерения. Приложение Xamarin должно включать файл исходного определения намерения `Content` типа, как описано [здесь](#adding-the-intent-definition-file-to-your-solution).

1. Приложение создает намерение и вызывает метод пожертвования без сбоя, но в выходных данных консоли отображается предупреждение о неизвестном типе намерения и не выполняется никакой пожертвования:

    > Невозможно пожертвование взаимодействия с Ордерсаупинтент, не имеющими допустимых типов ярлыков

    Чтобы устранить проблему, необходимо правильно определить назначение [Siri](#entitlementsplist) и выбрать [его](#infoplist)для текущей конфигурации сборки с помощью параметров проекта.

    **Info. plist** приложения:

    ```plist
    <key>NSUserActivityTypes</key>
    <array>
        <string>ScheduleMeetingIntent</string>
    </array>
    ```

    **Plist** приложения с возможностью Siri:

    ```plist
    <key>com.apple.developer.siri</key>
    <true/>
    ```

    Для целевой конфигурации сборки должны быть выбраны настраиваемые права. Последовательно выберите **Параметры проекта > создать > подписывание пакета iOS** и настройте **Настраиваемые** права для файла прав **. plist** , содержащего необходимые права.

### <a name="creating-an-intents-extension"></a>Создание расширения целей

Код, который выполняется, когда Siri вызывает намерение, помещается в расширение целей, которое можно добавить как новый проект в то же решение, что и существующее приложение Xamarin. iOS, такое как полный курс Chef. В решении **саупчеф** расширение называется **саупчефинтентс**.

#### <a name="soupchefintents-infoplist-and-entitlementsplist"></a>Саупчефинтентс — info. plist и права. plist

##### <a name="soupchefintents-infoplist"></a>Саупчефинтентс — info. plist

Параметр **info. plist** в проекте **Саупчефинтентс** определяет **идентификатор пакета** как `com.xamarin.SoupChef.SoupChefIntents` .

Файл **info. plist** также содержит следующую запись:

```xml
<key>NSExtension</key>
<dict>
    <key>NSExtensionAttributes</key>
    <dict>
        <key>IntentsRestrictedWhileLocked</key>
        <array/>
        <key>IntentsSupported</key>
        <array>
            <string>OrderSoupIntent</string>
        </array>
        <key>IntentsRestrictedWhileProtectedDataUnavailable</key>
        <array/>
    </dict>
    <key>NSExtensionPointIdentifier</key>
    <string>com.apple.intents-service</string>
    <key>NSExtensionPrincipalClass</key>
    <string>IntentHandler</string>
</dict>
```

В предыдущем файле **info. plist**:

- `IntentsRestrictedWhileLocked` Список целей, которые должны быть обработаны при разблокировании устройства.
- `IntentsSupported` Список целей, обрабатываемых этим расширением.
- `NSExtensionPointIdentifier` Указывает тип расширения приложения. Дополнительные сведения см. в [документации Apple](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/AppExtensionKeys.html#//apple_ref/doc/uid/TP40014212-SW15).
- `NSExtensionPrincipalClass` Указывает класс, который должен использоваться для обработки целей, поддерживаемых этим расширением.

##### <a name="soupchefintents-entitlementsplist"></a>Саупчефинтентс — права. plist

Права на **plist** в проекте **Саупчефинтентс** имеют возможности **групп приложений** . Эта возможность настроена для использования той же группы приложений, что и проект **саупчеф** :

```xml
<key>com.apple.security.application-groups</key>
<array>
    <string>group.com.xamarin.SoupChef</string>
</array>
```

Полный курс Chef сохраняет данные с помощью `NSUserDefaults` . Чтобы предоставить общий доступ к данным между приложением и расширением приложения, они ссылаются на одну группу приложений в файлах прав **. plist** .

> [!NOTE]
> Конфигурация сборки проекта **саупчефинтентс** задает **Настраиваемые** права для прав **. plist**.

#### <a name="handling-an-ordersoupintent-background-task"></a>Обработка фоновой задачи Ордерсаупинтент

Расширение целей выполняет необходимые фоновые задачи для ярлыка на основе пользовательской намеренности.

Siri вызывает [`GetHandler`](xref:Intents.INExtension.GetHandler*) метод `IntentHandler` класса (определенного в **info. plist** как `NSExtensionPrincipalClass` ), чтобы получить экземпляр класса, который расширяет класс `OrderSoupIntentHandling` , который может использоваться для управления `OrderSoupIntent` :

```csharp
[Register("IntentHandler")]
public class IntentHandler : INExtension
{
    public override NSObject GetHandler(INIntent intent)
    {
        if (intent is OrderSoupIntent)
        {
            return new OrderSoupIntentHandler();
        }
        throw new Exception("Unhandled intent type: ${intent}");
    }

    protected IntentHandler(IntPtr handle) : base(handle) { }
}
```

`OrderSoupIntentHandler`, определенный в проекте **саупкит** общего кода, реализует два важных метода:

- `ConfirmOrderSoup` — Подтверждает, действительно ли задача, связанная с намерением, должна выполняться
- `HandleOrderSoup` — Помещает порядок полный курс и реагирует на пользователя путем вызова переданного обработчика завершения.

#### <a name="handling-an-ordersoupintent-that-opens-the-app"></a>Обработка Ордерсаупинтент, который открывает приложение

Приложение должно правильно обработано намерения, которые не выполняются в фоновом режиме.
Эти способы обрабатываются так же, как и `NSUserActivity` ярлыки, в `ContinueUserActivity` методе `AppDelegate` :

```csharp
public override bool ContinueUserActivity(UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    var intent = userActivity.GetInteraction()?.Intent as OrderSoupIntent;
    if (!(intent is null))
    {
        HandleIntent(intent);
        return true;
    }
    // ...
}  
```

## <a name="providing-a-user-interface-for-a-custom-intent"></a>Предоставление пользовательского интерфейса для пользовательской цели

Расширение пользовательского интерфейса для целей предоставляет настраиваемый пользовательский интерфейс для расширения целей. В решении **Саупчеф** **саупчефинтентсуи** — это расширение пользовательского интерфейса, предоставляющее интерфейс для **саупчефинтентс**.

### <a name="soupchefintentsui--infoplist-and-entitlementsplist"></a>Саупчефинтентсуи — info. plist и права. plist

#### <a name="soupchefintentsui-infoplist"></a>Саупчефинтентсуи — info. plist

Параметр **info. plist** в проекте **Саупчефинтентсуи** определяет **идентификатор пакета** как `com.xamarin.SoupChef.SoupChefIntentsui` .

Файл **info. plist** также содержит следующую запись:

```xml
<key>NSExtension</key>
<dict>
    <key>NSExtensionAttributes</key>
    <dict>
        <key>IntentsSupported</key>
        <array>
            <string>OrderSoupIntent</string>
        </array>
        <!-- ... -->
    </dict>
    <key>NSExtensionPointIdentifier</key>
    <string>com.apple.intents-ui-service</string>
    <key>NSExtensionMainStoryboard</key>
    <string>MainInterface</string>
</dict>
```

В предыдущем файле **info. plist**:

- `IntentsSupported` Указывает, что объект `OrderSoupIntent` обрабатывается этим расширением пользовательского интерфейса.
- `NSExtensionPointIdentifier` Указывает тип расширения приложения. Дополнительные сведения см. в [документации Apple](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/AppExtensionKeys.html#//apple_ref/doc/uid/TP40014212-SW15).
- `NSExtensionMainStoryboard` Указывает раскадровку, определяющую основной интерфейс этого расширения

#### <a name="soupchefintentsui-entitlementsplist"></a>Саупчефинтентсуи — права. plist

Для проекта **саупчефинтентсуи** не требуется файл прав **. plist** .

### <a name="creating-the-user-interface"></a>Создание пользовательского интерфейса

Поскольку файл **info. plist** для **саупчефинтентсуи** задает `NSExtensionMainStoryboard` для ключа значение `MainInterface` , в файле **маининтераце. Storyboard** определяется интерфейс для расширения пользовательского интерфейса для целей.

В этой раскадровке имеется единственный контроллер представления типа **интентвиевконтроллер**. Он ссылается на два представления:

- **инвоицевиев**, тип `InvoiceView`
- **конфирматионвиев**, тип `ConfirmOrderView`

> [!NOTE]
> Интерфейсы для **инвоицевиев** и **Конфирматионвиев** определены в **главном. Storyboard** как дополнительные представления. Конструктор iOS в Visual Studio для Mac и Visual Studio 2017 не поддерживает просмотр и редактирование дополнительных представлений. для этого откройте **главное. Storyboard** в Interface Builder Xcode.

`IntentViewController` реализует класс [`IINUIHostedViewControlling`](xref:IntentsUI.IINUIHostedViewControlling)
интерфейс, используемый для предоставления пользовательского интерфейса при работе с Siri. [`ConfigureView`](xref:IntentsUI.INUIHostedViewControlling_Extensions.ConfigureView*)
метод вызывается для настройки интерфейса, отображения подтверждения или счета в зависимости от того, подтверждается ли взаимодействие ( [`INIntentHandlingStatus.Ready`](xref:Intents.INIntentHandlingStatus) ) или выполнено успешно ( [`INIntentHandlingStatus.Success`](xref:Intents.INIntentHandlingStatus) ):

```csharp
[Export("configureViewForParameters:ofInteraction:interactiveBehavior:context:completion:")]
public void ConfigureView(
    NSSet<INParameter> parameters,
    INInteraction interaction,
    INUIInteractiveBehavior interactiveBehavior,
    INUIHostedViewContext context,
    INUIHostedViewControllingConfigureViewHandler completion)
{
    // ...
    if (interaction.IntentHandlingStatus == INIntentHandlingStatus.Ready)
    {
        desiredSize = DisplayInvoice(order, intent);
    }
    else if(interaction.IntentHandlingStatus == INIntentHandlingStatus.Success)
    {
        var response = interaction.IntentResponse as OrderSoupIntentResponse;
        if (!(response is null))
        {
            desiredSize = DisplayOrderConfirmation(order, intent, response);
        }
    }
    completion(true, parameters, desiredSize);
}
```

> [!TIP]
> Дополнительные сведения о `ConfigureView` методе см. в статье Apple ввдк 2017, [что нового в SiriKit](https://developer.apple.com/videos/play/wwdc2017/214/).

## <a name="creating-a-voice-shortcut"></a>Создание ярлыка голоса

Полный курс Chef предоставляет интерфейс для назначения для каждого заказа ярлыка голоса, что позволяет заказать полный курс с Siri. Фактически, интерфейс, используемый для записи и назначения речевых ярлыков, предоставляется iOS и не требует дополнительного пользовательского кода.

В `OrderDetailViewController` , когда пользователь касается строки **Add to Siri** таблицы, [`RowSelected`](xref:UIKit.UITableViewSource.RowSelected*) метод предоставляет экран для добавления или изменения ярлыка.

```csharp
public override void RowSelected(UITableView tableView, NSIndexPath indexPath)
{
    // ...
    else if (TableConfiguration.Sections[indexPath.Section].Type == OrderDetailTableConfiguration.SectionType.VoiceShortcut)
    {
        INVoiceShortcut existingShortcut = VoiceShortcutDataManager?.VoiceShortcutForOrder(Order);
        if (!(existingShortcut is null))
        {
            var editVoiceShortcutViewController = new INUIEditVoiceShortcutViewController(existingShortcut);
            editVoiceShortcutViewController.Delegate = this;
            PresentViewController(editVoiceShortcutViewController, true, null);
        }
        else
        {
            // Since the app isn't yet managing a voice shortcut for
            // this order, present the add view controller
            INShortcut newShortcut = new INShortcut(Order.Intent);
            if (!(newShortcut is null))
            {
                var addVoiceShortcutVC = new INUIAddVoiceShortcutViewController(newShortcut);
                addVoiceShortcutVC.Delegate = this;
                PresentViewController(addVoiceShortcutVC, true, null);
            }
        }
    }
}
```

В зависимости от того, существует ли имеющееся сочетание голоса для текущего порядка отображения, `RowSelected` представляет контроллер представления типа [`INUIEditVoiceShortcutViewController`](xref:IntentsUI.INUIEditVoiceShortcutViewController) или [`INUIAddVoiceShortcutViewController`](xref:IntentsUI.INUIAddVoiceShortcutViewController) .
В каждом случае `OrderDetailViewController` присваивает себя как контроллер представления `Delegate` , поэтому он также реализует [`IINUIAddVoiceShortcutViewControllerDelegate`](xref:IntentsUI.IINUIAddVoiceShortcutViewControllerDelegate)
и [`IINUIEditVoiceShortcutViewControllerDelegate`](xref:IntentsUI.IINUIEditVoiceShortcutViewControllerDelegate).

## <a name="testing-on-device"></a>Тестирование на устройстве

Чтобы запустить полный курс Chef на устройстве, следуйте инструкциям в этом разделе. Также прочитайте [Примечание об автоматической подготовке](#automatic-provisioning).

### <a name="app-group-app-ids-provisioning-profiles"></a>Группа приложений, идентификаторы приложений, профили подготовки

В разделе **сертификаты, идентификаторы & профили** на [портале разработчика Apple](https://developer.apple.com/)выполните следующие действия.

- Создайте группу приложений для обмена данными между приложением полный курс Chef и его расширениями. Например: **Group. com. yourcompanyname. саупчеф**

- Создайте три идентификатора приложения: один для самого приложения, один для расширения "удержания", а другой для расширения пользовательского интерфейса "для целей". Пример:

  - Приложение: **com. yourcompanyname. саупчеф**
    - Для этого идентификатора приложения назначьте возможности групп SiriKit и **приложений** .

  - Расширение целей: **com. yourcompanyname. саупчеф.-целей**
    - Для этого идентификатора приложения назначьте возможность **групп приложений** .

  - Расширение пользовательского интерфейса целей: **com. yourcompanyname. саупчеф. интентсуи**
    - Для этого идентификатора приложения не требуются специальные возможности.

- После создания указанных выше идентификаторов приложений измените возможности **групп приложений** , назначенные приложению, и расширение «определения целей», указав конкретную группу приложений, созданную ранее.

- Создайте три новых профиля подготовки для разработки — по одному для каждого из новых идентификаторов приложений.

- Скачайте эти профили подготовки и дважды щелкните каждую из них, чтобы установить их. Если Visual Studio для Mac или Visual Studio 2017 уже запущены, перезапустите ее, чтобы убедиться в регистрации новых профилей подготовки.

### <a name="editing-infoplist-entitlementsplist-and-source-code"></a>Редактирование info. plist, прав. plist и исходного кода

В Visual Studio для Mac или Visual Studio 2017 выполните следующие действия.

- Обновите различные файлы **info. plist** в решении. Задайте для параметра приложение, расширение целей и **идентификатор пакета** расширения пользовательского интерфейса для идентификатора приложения, определенного ранее:

  - Приложение: **com. yourcompanyname. саупчеф**
  - Расширение целей: **com. yourcompanyname. саупчеф.-целей**
  - Расширение пользовательского интерфейса целей: **com. yourcompanyname. саупчеф. интентсуи**

- Обновите файл прав **. plist** для проекта **саупчеф** :
  - Для возможности **группы приложений** присвойте группе новую группу приложений, созданную ранее (в приведенном выше примере это **Group. com. yourcompanyname. саупчеф**).
  - Убедитесь, что **SiriKit** включен.

- Обновите файл прав **. plist** для проекта **саупчефинтентс** :
  - Для возможности **группы приложений** присвойте группе новую группу приложений, созданную ранее (в приведенном выше примере это **Group. com. yourcompanyname. саупчеф**).

- Наконец, откройте **NSUserDefaultsHelper.CS**. Задайте `AppGroup` переменную в качестве значения для новой группы приложений (например, задайте для нее значение `group.com.yourcompanyname.SoupChef` ).

### <a name="configuring-the-build-settings"></a>Настройка параметров сборки

В Visual Studio для Mac или Visual Studio 2017:

- Откройте параметры/свойства для проекта **саупчеф** . На вкладке **Подписывание пакета iOS** задайте для параметра **удостоверение подписывания** значение Автоматический и **профиль подготовки** для нового созданного ранее профиля подготовки для конкретного приложения.

- Откройте параметры/свойства для проекта **саупчефинтентс** . На вкладке **Подписывание пакета iOS** задайте для параметра **удостоверение подписывания** значение Автоматический и **профиль подготовки** для нового профиля подготовки для конкретного расширения, созданного ранее.

- Откройте параметры/свойства для проекта **саупчефинтентсуи** . На вкладке **Подписывание пакета iOS** задайте для параметра **удостоверение подписывания** значение Автоматический и **профиль подготовки** новый пользовательский профиль подготовки, созданный ранее.

После внесения этих изменений приложение будет запущено на устройстве iOS.

### <a name="automatic-provisioning"></a>Автоматическая подготовка

Вы можете использовать [автоматическую подготовку](../../get-started/installation/device-provisioning/automatic-provisioning.md) для выполнения многих из этих задач подготовки непосредственно в интегрированной среде разработки.
Однако автоматическая подготовка не настраивает группы приложений. Вам потребуется вручную настроить права **. plist** файлы с именем группы приложений, которую вы хотите использовать, посетите портал разработчика Apple, чтобы создать группу приложений, назначьте эту группу приложений каждому идентификатору приложения, созданному с помощью автоматической подготовки, повторно создайте профили подготовки (расширение app, удержания, расширение пользовательского интерфейса), чтобы включить только что созданную группу приложений, а затем скачайте и установите их.

## <a name="related-links"></a>Связанные ссылки

- [Полный курс Chef (Xamarin)](/samples/xamarin/ios-samples/ios12-soupchef)
- [Полный курс Chef (SWIFT)](https://developer.apple.com/documentation/sirikit/accelerating_app_interactions_with_shortcuts?language=objc)
- [SiriKit (Apple)](https://developer.apple.com/sirikit/)
- [Общие сведения о сочетаниях клавиш Siri — ВВДК 2018](https://developer.apple.com/videos/play/wwdc2018/211/)
- [Создание для голоса с помощью сочетаний клавиш Siri — ВВДК 2018](https://developer.apple.com/videos/play/wwdc2018/214/)
- [Siri ярлыки на Siri смотреть лицо — ВВДК 2018](https://developer.apple.com/videos/play/wwdc2018/217/)
- [Новые возможности в SiriKit – ВВДК 2017](https://developer.apple.com/videos/play/wwdc2017/214/)
- [Создание расширения приложения для целей (Apple)](https://developer.apple.com/documentation/sirikit/creating_an_intents_app_extension?language=objc)
- [Привязка библиотеки Swift для iOS](../binding-swift/walkthrough.md)
