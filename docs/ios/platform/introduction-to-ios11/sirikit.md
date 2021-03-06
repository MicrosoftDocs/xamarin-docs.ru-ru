---
title: Обновления SiriKit в iOS 11
description: В этом документе описывается работа с SiriKit в iOS 11. В частности, он изучает работу с задачами и заметками и предоставляет альтернативные имена для приложения.
ms.prod: xamarin
ms.assetid: 8F75300B-B591-42ED-9D17-001992A5C381
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/07/2017
ms.openlocfilehash: 7bc102069d673b9459c863282b0423952c8fa59d
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91437322"
---
# <a name="sirikit-updates-in-ios-11"></a>Обновления SiriKit в iOS 11

SiriKit был представлен в iOS 10 с несколькими доменами служб (включая тренировки, выполнив бронирование и совершая звонки). Сведения о концепциях SiriKit и реализации SiriKit в приложении см. в [разделе SiriKit](~/ios/platform/sirikit/index.md) .

![Демонстрация списка задач Siri](sirikit-images/sirikit.png)

SiriKit в iOS 11 добавляет новые и обновленные домены с намерением:

- [**Списки и примечания**](#listsnotes) — новые! Предоставляет API для приложений для обработки задач и заметок.
- **Визуальные коды** — новые! Siri может отображать QR-коды для совместного использования контактных данных или участия в платежных транзакциях.
- **Платежи** — добавлены сведения о способах поиска и перемещения для взаимодействий оплаты.
- Применяйте **резервирование** — добавлены ответы для отмены и обратной связи.

Другие новые возможности:

- [**Альтернативные имена приложений**](#alternativenames) — предоставляет псевдонимы, которые помогают клиентам сообщить Siri о необходимости использовать другие имена и произношение.
- **Запуск тренировок** — позволяет начать тренировку в фоновом режиме.

Ниже описаны некоторые из этих функций. Дополнительные сведения о других возможностях см. в [документации Apple SiriKit](https://developer.apple.com/documentation/sirikit).

<a name="listsnotes"></a>

## <a name="lists-and-notes"></a>Списки и примечания

Новый домен списков и заметок предоставляет API для приложений, позволяющих обрабатывать задачи и заметки с помощью запросов Siri Voice.

**Задачи**

- Иметь заголовок и состояние завершения.
- При необходимости укажите крайний срок и расположение.

**Примечания**

- Иметь заголовок и поле содержимого.

Задачи и заметки можно организовывать в группы. В оставшейся части этого раздела описывается, как реализовать этот новый домен с помощью SiriKit, используя [Пример Таскснотес SiriKit](/samples/xamarin/ios-samples/ios11-sirikitsample).

### <a name="how-to-process-a-sirikit-request"></a>Обработка запроса SiriKit

Обработайте запрос SiriKit, выполнив следующие действия.

1. **Resolve** — Проверка параметров и запрос дополнительных сведений от пользователя (при необходимости).
2. **Подтверждение** — окончательная проверка и проверка того, что запрос может быть обработан.
3. **Handle** — выполнение операции (обновление данных или выполнение сетевых операций).

Первые два шага являются необязательными (но рекомендуется), и требуется последний шаг.
Подробные инструкции см. в [разделе SiriKit](~/ios/platform/sirikit/index.md).

### <a name="resolve-and-confirm-methods"></a>Методы разрешения и подтверждения

Эти необязательные методы позволяют коду выполнять проверку, выбирать значения по умолчанию или запрашивать дополнительные сведения от пользователя.

В качестве примера для `IINCreateTaskListIntent` интерфейса требуется метод `HandleCreateTaskList` . Существует четыре необязательных метода, которые обеспечивают больший контроль над взаимодействием Siri:

- `ResolveTitle` — Проверяет заголовок, задает заголовок по умолчанию (при необходимости) или сигнализирует, что данные не являются обязательными.
- `ResolveTaskTitles` — Проверяет список задач, произнесенных пользователем.
- `ResolveGroupName` — Проверяет имя группы, выбирает группу по умолчанию или сообщает о том, что данные не являются обязательными.
- `ConfirmCreateTaskList` — Проверяет, может ли код выполнять запрошенную операцию, но не выполняет его (только `Handle*` методы должны изменять данные).

### <a name="handle-the-intent"></a>Обработайте намерение

Существует шесть целей в домене списков и заметок, три для задач и три для заметок.
Методы, которые необходимо реализовать для решения этих целей:

- Для задач:
  - `HandleAddTasks`
  - `HandleCreateTaskList`
  - `HandleSetTaskAttribute`
- Примечания:
  - `HandleCreateNote`
  - `HandleAppendToNote`
  - `HandleSearchForNotebookItems`

Каждому методу передается конкретный тип намерения, который содержит всю информацию Siri, проанализированную по запросу пользователя (и, возможно, обновленную в `Resolve*` `Confirm*` методах и).
Приложение должно проанализировать предоставленные данные, а затем выполнить некоторые действия по сохранению или обработке данных, а также возвратить результат, который Siri говорит и показывает пользователю.

### <a name="response-codes"></a>Коды ответов

Обязательные `Handle*` и необязательные `Confirm*` методы указывают код ответа, устанавливая значение для объекта, передаваемого в обработчик завершения. Ответы берутся из `INCreateTaskListIntentResponseCode` перечисления:

- `Ready` — Возвращает на этапе подтверждения (IE. из `Confirm*` метода, но не из `Handle*` метода).
- `InProgress` — Используется для длительных задач (например, для работы сети или сервера).
- `Success` — Возвращает сведения об успешной операции (только из `Handle*` метода).
- `Failure` — Означает, что произошла ошибка и операция не может быть завершена.
- `RequiringAppLaunch` — Не может быть обработано намерением, но операция возможна в приложении.
- `Unspecified` — Не использовать: пользователю будет отображаться сообщение об ошибке.

Дополнительные сведения об этих методах и ответах см. в [документации Apple SiriKit Lists and Notes](https://developer.apple.com/documentation/sirikit/lists_and_notes).

### <a name="implementing-lists-and-notes"></a>Реализация списков и заметок

[Пример Таскснотес SiriKit](/samples/xamarin/ios-samples/ios11-sirikitsample) был создан с помощью следующих шагов, чтобы добавить поддержку SiriKit в пустое приложение iOS.

Прежде всего, чтобы добавить поддержку SiriKit, выполните следующие действия для приложения iOS:

1. **SiriKit** Tick в правах **. plist**.
2. Добавьте ключ " **Конфиденциальность — Siri Usage** " в **info. plist**вместе с сообщением для клиентов.
3. Вызовите `INPreferences.RequestSiriAuthorization` метод в приложении, чтобы предложить пользователю разрешить взаимодействие Siri.
4. Добавьте SiriKit в идентификатор приложения на портале разработчика и повторно создайте профили подготовки, чтобы включить новый объем обслуживания.

Затем добавьте в приложение новый проект расширения для обработки запросов Siri:

1. Щелкните решение правой кнопкой мыши и выберите **добавить > добавить новый проект...**.
2. Выберите шаблон **расширения iOS > расширения >** .
3. Будут добавлены два новых проекта: намерения и Интентуи. Настройка пользовательского интерфейса является необязательной, поэтому в образце содержится только код в проекте **намерения** .

Проект расширения — это место, где будут обрабатываться все запросы SiriKit. Как отдельное расширение, оно не имеет автоматического способа взаимодействия с основным приложением — это обычно разрешается путем реализации общего хранилища файлов с помощью групп приложений.

#### <a name="configure-the-intenthandler"></a>Настройка Интенсандлер

`IntentHandler`Класс является точкой входа для запросов Siri — каждое намерение передается `GetHandler` методу, который возвращает объект, который может обработать запрос.

В приведенном ниже коде показана простая реализация:

```csharp
[Register("IntentHandler")]
public partial class IntentHandler : INExtension, IINNotebookDomainHandling
{
  protected IntentHandler(IntPtr handle) : base(handle)
  {}
  public override NSObject GetHandler(INIntent intent)
  {
    // This is the default implementation.  If you want different objects to handle different intents,
    // you can override this and return the handler you want for that particular intent.
    return this;
  }
  // add intent handlers here!
}
```

Класс должен наследовать от `INExtension` , а поскольку этот пример будет обрабатывать списки и заметки, он также реализует `IINNotebookDomainHandling` .

> [!NOTE]
>
> - В .NET есть соглашение об использовании интерфейсов с прописными буквами `I` , которые в Xamarin соответствуют при связывании протоколов из пакета SDK для iOS.
> - Xamarin также сохраняет имена типов из iOS, а Apple использует первые два символа в именах типов для отражения платформы, которой принадлежит тип.
> - Для `Intents` платформы типы начинаются с префикса `IN*` (например, `INExtension`), но они _не_ являются интерфейсами.
> - Кроме того, в итоге используются протоколы (которые становятся интерфейсами в C#) с двумя `I` , такими как `IINAddTasksIntentHandling` .

#### <a name="handling-intents"></a>Обработка целей

Каждое намерение (Добавление задачи, задание атрибута задачи и т. д.) реализуется в одном методе, аналогичном показанному ниже. Метод должен выполнять три основные функции:

1. **Обработка намерения** — данные, проанализированные с помощью Siri, становятся доступными в `intent` объекте, относящемся к типу намерения. Приложение может проверить эти данные с помощью дополнительных `Resolve*` методов.
2. **Проверка и обновление хранилища данных** — сохранение данных в файловой системе (с помощью групп приложений, чтобы основное приложение iOS также может получить к нему доступ) или через сетевой запрос.
3. **Предоставление ответа** — используйте `completion` обработчик, чтобы отправить ответ обратно в Siri для чтения и вывода пользователю:

```csharp
public void HandleCreateTaskList(INCreateTaskListIntent intent, Action<INCreateTaskListIntentResponse> completion)
{
  var list = TaskList.FromIntent(intent);
  // TODO: have to create the list and tasks... in your app data store
  var response = new INCreateTaskListIntentResponse(INCreateTaskListIntentResponseCode.Success, null)
  {
    CreatedTaskList = list
  };
  completion(response);
}
```

Обратите внимание, что в `null` ответ передается как второй параметр — это параметр действия пользователя, а если он не указан, будет использоваться значение по умолчанию.
Вы можете задать настраиваемый тип действия до тех пор, пока приложение iOS его поддерживает с помощью `NSUserActivityTypes` ключа в **info. plist**. Затем можно обслужить этот случай при открытии приложения и выполнять определенные операции (такие как открытие соответствующего контроллера представления и загрузка данных из операции Siri).

Пример также сделанным `Success` результат, но в реальных сценариях следует добавить соответствующие отчеты об ошибках.

### <a name="test-phrases"></a>Тестовые фразы

Следующие фразы теста должны работать в примере приложения:

- «Сделайте список продуктов, используя яблоки, «полукруглые» и «груши» в Таскснотес»
- "Добавить задачу ВВДК в Таскснотес"
- "Добавить задачу ВВДК в список обучения в Таскснотес"
- "Марка участника ВВДК как завершенного в Таскснотес"
- "В Таскснотес напомнить о покупке iPhone при получении домашнего"
- "Марк купить iPhone как завершенный в Таскснотес"
- "Напомнить о выходе дома по адресу 8:00 в Таскснотес"

![Создать новый пример списка](sirikit-images/createtasklist-sml.png) ![Пример задания задачи как полного](sirikit-images/settaskattribute-sml.png)

> [!NOTE]
> Имитатор iOS 11 поддерживает тестирование с помощью Siri (в отличие от более ранних версий).
>
> При тестировании на реальных устройствах не забудьте настроить идентификатор приложения и профили подготовки для поддержки SiriKit.

<a name="alternativenames"></a>

## <a name="alternative-names"></a>Альтернативные имена

Эта новая функция iOS 11 означает, что можно настроить альтернативные имена для приложения, чтобы помочь пользователям правильно активировать его с помощью Siri. Добавьте следующие ключи в файл **info. plist** проекта приложения iOS:

![Info. plist, где показаны альтернативные ключи и значения имени приложения](sirikit-images/alternative-names.png)

После установки альтернативных имен приложений следующие фразы также будут работать для примера приложения (которое фактически называется **таскснотес**):

- «Сделайте список продуктов, используя яблоки, «полукруглые» и «груши» в _монкэйнотес_»
- "Добавить задачу ВВДК в _монкэйтодо_"

## <a name="troubleshooting"></a>Устранение неполадок

Некоторые ошибки, которые могут возникнуть при выполнении примера или добавлении SiriKit в свои приложения:

### <a name="nsinternalinconsistencyexception"></a>нсинтерналинконсистенциексцептион

_Вызвано исключение цели-C.  Имя: Нсинтерналинконсистенциексцептион причина: использование класса Preferences <:0x60400082ff00> из приложения требует прав com. Apple. Developer. Siri. Вы включили функцию Siri в проект Xcode?_

- SiriKit подается на такты в правах **. plist**.
- Права **. plist** настраивается в **параметрах проекта > сборка > подписывание пакета iOS**.

  [![Параметры проекта, отображающие права, настроенные правильно](sirikit-images/set-entitlements-sml.png)](sirikit-images/set-entitlements.png#lightbox)

- (для развертывания устройства) Для идентификатора приложения включен SiriKit и профиль подготовки.

## <a name="related-links"></a>Связанные ссылки

- [SiriKit (Apple)](https://developer.apple.com/documentation/sirikit)
- [Пример SiriKit Таскснотес](/samples/xamarin/ios-samples/ios11-sirikitsample)
- [Новые возможности в SiriKit (ВВДК) (видео)](https://developer.apple.com/videos/play/wwdc2017/214/)