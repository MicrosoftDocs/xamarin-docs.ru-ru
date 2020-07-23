---
title: Каллкит в Xamarin. iOS
description: В этой статье рассматривается новый API Каллкит, выпущенный компанией Apple в iOS 10, и способы его реализации в приложениях VOIP для Xamarin. iOS.
ms.prod: xamarin
ms.assetid: 738A142D-FFD2-4738-B3ED-57C273179848
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/15/2017
ms.openlocfilehash: 116ae63619aa90defb25db31b959e36b8b44edf2
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86934736"
---
# <a name="callkit-in-xamarinios"></a>Каллкит в Xamarin. iOS

Новый API Каллкит в iOS 10 обеспечивает интеграцию приложений VOIP с пользовательским ИНТЕРФЕЙСом iPhone и предоставляет привычный интерфейс и возможности для конечного пользователя. Благодаря этому пользователи API могут просматривать вызовы VOIP и взаимодействовать с ними с экрана блокировки устройства iOS, а также управлять контактами с помощью представлений **"Избранное" и "** **недавние** " приложения "Телефон".

## <a name="about-callkit"></a>О Каллкит

В соответствии с Apple, Каллкит — это новая платформа, которая будет повышать уровень использования сторонних приложений для передачи голоса по протоколу IP (VOIP) до первого интерфейса в iOS 10. API Каллкит позволяет интегрировать приложения VOIP с пользовательским ИНТЕРФЕЙСом iPhone и предоставлять знакомый интерфейс и возможности для конечного пользователя. Как и встроенное приложение для телефона, пользователь может просматривать вызовы VOIP и взаимодействовать с ними с экрана блокировки устройства iOS, а также управлять контактами с помощью представлений **"Избранное" и "** **недавние** " приложения "Телефон".

Кроме того, API Каллкит предоставляет возможность создавать расширения приложений, которые могут связывать номер телефона с именем (ИДЕНТИФИКАТОРом звонящего) или сообщать системе, когда число должно быть заблокировано (Блокировка вызова).

### <a name="the-existing-voip-app-experience"></a>Взаимодействие с существующим приложением VOIP

Прежде чем обсудить новый API Каллкит и его возможности, ознакомьтесь с текущим пользовательским приложением VOIP в iOS 9 (и менее) с помощью вымышленного приложения VOIP под названием Монкэйкалл. Монкэйкалл — это простое приложение, позволяющее пользователю отправлять и получать вызовы VOIP с помощью существующих интерфейсов API iOS.

В настоящее время, если пользователь получает входящий вызов на Монкэйкалл, а его iPhone заблокирован, уведомление, полученное на экране блокировки, не отличается от других типов уведомлений (например, от сообщений или почтовых приложений).

Если пользователь хотел бы ответить на вызов, ему пришлось бы подать ему уведомление Монкэйкалл, чтобы открыть приложение и ввести секретный код (или сенсорный идентификатор пользователя), чтобы разблокировать телефон, прежде чем он сможет принять вызов и начать беседу.

Если телефон разблокирован, он будет работать так же утомительно. Опять же, входящий вызов Монкэйкалл отображается как стандартный баннер уведомления, в котором слайды находятся в верхней части экрана. Так как уведомление является временным, пользователь может легко пропустить его, чтобы открыть центр уведомлений и найти конкретное уведомление для ответа, а затем вызвать или найти и запустить приложение Монкэйкалл вручную.

### <a name="the-callkit-voip-app-experience"></a>Взаимодействие с приложением Каллкит VOIP

Реализуя новые API Каллкит в приложении Монкэйкалл, пользователь может значительно улучшить работу с входящим вызовом VOIP в iOS 10. Возьмем пример пользователя, получающего вызов VOIP, когда его телефон заблокирован выше. При реализации Каллкит вызов будет отображаться на экране блокировки iPhone точно так же, как если бы вызов был получен из встроенного приложения для телефона, с полноэкранным, собственным ИНТЕРФЕЙСом и стандартными функциями считывания в ответ.

Опять же, если iPhone разблокируется при получении вызова Монкэйкалл VOIP, будет представлен один и тот же полноэкранный, собственный пользовательский интерфейс и стандартные функции, позволяющие отклонить встроенное приложение Phone, а Монкэйкалл — возможность воспроизведения пользовательской мелодии звонка.

Каллкит предоставляет дополнительные функциональные возможности для Монкэйкалл, позволяя вызовам VOIP взаимодействовать с другими типами вызовов, отображаться во встроенных списках "последние" и "Избранные", использовать встроенные функции "не беспокоить" и "блокировать", запускать Монкэйкалл вызовы из Siri и предоставлять пользователям возможность назначать вызовы Монкэйкалл для пользователей в приложении "Контакты".

В следующих разделах подробно рассматривается архитектура Каллкит, входящие и исходящие потоки вызовов и API Каллкит.

## <a name="the-callkit-architecture"></a>Архитектура Каллкит

В iOS 10 компания Apple применяет Каллкит во всех системных службах, так что вызовы, выполняемые в Карплай, например, известны системным ИНТЕРФЕЙСом через Каллкит. В приведенном ниже примере, поскольку Монкэйкалл принимает Каллкит, он известен системе так же, как и встроенным системным службам, и получает все те же самые функции:

[![Стек службы Каллкит](callkit-images/callkit01.png)](callkit-images/callkit01.png#lightbox)

Подробнее рассмотрим приложение Монкэйкалл на приведенной выше схеме. Приложение содержит весь свой код для взаимодействия с собственной сетью и содержит собственные пользовательские интерфейсы. Он ссылается на Каллкит для взаимодействия с системой:

[![Архитектура приложения Монкэйкалл](callkit-images/callkit02.png)](callkit-images/callkit02.png#lightbox)

В Каллкит есть два основных интерфейса, которые использует приложение:

- `CXProvider`— Это позволяет приложению Монкэйкалл информировать систему о любых возможностях нестандартных уведомлений, которые могут возникнуть.
- `CXCallController`— Разрешает приложению Монкэйкалл уведомлять систему о действиях локальных пользователей.

### <a name="the-cxprovider"></a>Ккспровидер

Как указано выше, `CXProvider` позволяет приложению уведомлять систему о любых возможностях нестандартных уведомлений, которые могут возникнуть. Это уведомление, которое не происходит из-за действий локального пользователя, но происходит из-за внешних событий, таких как входящие вызовы.

Приложение должно использовать `CXProvider` для следующих действий:

- Сообщает о входящем вызове системы.
- Сообщает о том, что исходящий вызов подключен к системе.
- Сообщить удаленному пользователю, завершающему вызов системы.

Когда приложение хочет взаимодействовать с системой, оно использует `CXCallUpdate` класс и, когда система должна взаимодействовать с приложением, используется `CXAction` класс:

[![Взаимодействие с системой через Ккспровидер](callkit-images/callkit03.png)](callkit-images/callkit03.png#lightbox)

### <a name="the-cxcallcontroller"></a>Ккскаллконтроллер

`CXCallController`Позволяет приложению уведомлять систему о локальных действиях пользователей, таких как пользователь, запускающий вызов VoIP. Реализация `CXCallController` приложения взаимозависимость с другими типами вызовов в системе. Например, если уже выполняется телефонный вызов телефонии, `CXCallController` может позволить приложению VoIP разместить этот вызов на удержании и запуске или ответить на вызов VoIP.

Приложение должно использовать `CXCallController` для следующих действий:

- Сообщать, когда пользователь начал исходящий вызов к системе.
- Сообщать, когда пользователь отвечает на входящий вызов системы.
- Сообщать, когда пользователь завершает вызов системы.

Когда приложение хочет передать в систему действия локальных пользователей, оно использует `CXTransaction` класс:

[![Создание отчетов для системы с помощью Ккскаллконтроллер](callkit-images/callkit04.png)](callkit-images/callkit04.png#lightbox)

## <a name="implementing-callkit"></a>Реализация Каллкит

В следующих разделах показано, как реализовать Каллкит в приложении VOIP для Xamarin. iOS. В качестве примера этот документ будет использовать код из вымышленного приложения Монкэйкалл VOIP. Представленный здесь код представляет несколько вспомогательных классов, которые подробно рассматриваются в следующих разделах.

### <a name="the-activecall-class"></a>Класс Активекалл

`ActiveCall`Класс используется приложением монкэйкалл для хранения всей информации о вызове VoIP, который в настоящее время активен следующим образом:

```csharp
using System;
using CoreFoundation;
using Foundation;

namespace MonkeyCall
{
    public class ActiveCall
    {
        #region Private Variables
        private bool isConnecting;
        private bool isConnected;
        private bool isOnhold;
        #endregion

        #region Computed Properties
        public NSUuid UUID { get; set; }
        public bool isOutgoing { get; set; }
        public string Handle { get; set; }
        public DateTime StartedConnectingOn { get; set;}
        public DateTime ConnectedOn { get; set;}
        public DateTime EndedOn { get; set; }

        public bool IsConnecting {
            get { return isConnecting; }
            set {
                isConnecting = value;
                if (isConnecting) StartedConnectingOn = DateTime.Now;
                RaiseStartingConnectionChanged ();
            }
        }

        public bool IsConnected {
            get { return isConnected; }
            set {
                isConnected = value;
                if (isConnected) {
                    ConnectedOn = DateTime.Now;
                } else {
                    EndedOn = DateTime.Now;
                }
                RaiseConnectedChanged ();
            }
        }

        public bool IsOnHold {
            get { return isOnhold; }
            set {
                isOnhold = value;
            }
        }
        #endregion

        #region Constructors
        public ActiveCall ()
        {
        }

        public ActiveCall (NSUuid uuid, string handle, bool outgoing)
        {
            // Initialize
            this.UUID = uuid;
            this.Handle = handle;
            this.isOutgoing = outgoing;
        }
        #endregion

        #region Public Methods
        public void StartCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call starting successfully
            completionHandler (true);

            // Simulate making a starting and completing a connection
            DispatchQueue.MainQueue.DispatchAfter (new DispatchTime(DispatchTime.Now, 3000), () => {
                // Note that the call is starting
                IsConnecting = true;

                // Simulate pause before connecting
                DispatchQueue.MainQueue.DispatchAfter (new DispatchTime (DispatchTime.Now, 1500), () => {
                    // Note that the call has connected
                    IsConnecting = false;
                    IsConnected = true;
                });
            });
        }

        public void AnswerCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call being answered
            IsConnected = true;
            completionHandler (true);
        }

        public void EndCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call ending
            IsConnected = false;
            completionHandler (true);
        }
        #endregion

        #region Events
        public delegate void ActiveCallbackDelegate (bool successful);
        public delegate void ActiveCallStateChangedDelegate (ActiveCall call);

        public event ActiveCallStateChangedDelegate StartingConnectionChanged;
        internal void RaiseStartingConnectionChanged ()
        {
            if (this.StartingConnectionChanged != null) this.StartingConnectionChanged (this);
        }

        public event ActiveCallStateChangedDelegate ConnectedChanged;
        internal void RaiseConnectedChanged ()
        {
            if (this.ConnectedChanged != null) this.ConnectedChanged (this);
        }
        #endregion
    }
}
```

`ActiveCall`содержит несколько свойств, определяющих состояние вызова, и два события, которые могут быть вызваны при изменении состояния вызова. Поскольку это только пример, для имитации запуска, ответа и завершения вызова используются три метода.

### <a name="the-startcallrequest-class"></a>Класс Старткаллрекуест

`StartCallRequest`Статический класс предоставляет несколько вспомогательных методов, которые будут использоваться при работе с исходящими вызовами:

```csharp
using System;
using Foundation;
using Intents;

namespace MonkeyCall
{
    public static class StartCallRequest
    {
        public static string URLScheme {
            get { return "monkeycall"; }
        }

        public static string ActivityType {
            get { return INIntentIdentifier.StartAudioCall.GetConstant ().ToString (); }
        }

        public static string CallHandleFromURL (NSUrl url)
        {
            // Is this a MonkeyCall handle?
            if (url.Scheme == URLScheme) {
                // Yes, return host
                return url.Host;
            } else {
                // Not handled
                return null;
            }
        }

        public static string CallHandleFromActivity (NSUserActivity activity)
        {
            // Is this a start call activity?
            if (activity.ActivityType == ActivityType) {
                // Yes, trap any errors
                try {
                    // Get first contact
                    var interaction = activity.GetInteraction ();
                    var startAudioCallIntent = interaction.Intent as INStartAudioCallIntent;
                    var contact = startAudioCallIntent.Contacts [0];

                    // Get the person handle
                    return contact.PersonHandle.Value;
                } catch {
                    // Error, report null
                    return null;
                }
            } else {
                // Not handled
                return null;
            }
        }
    }
}
```

`CallHandleFromURL`Классы и `CallHandleFromActivity` используются в AppDelegate для получения маркера контакта пользователя, вызываемого в исходящем вызове. Дополнительные сведения см. в разделе [Обработка исходящих вызовов](#handling-outgoing-calls) ниже.

### <a name="the-activecallmanager-class"></a>Класс Активекаллманажер

`ActiveCallManager`Класс обрабатывает все открытые вызовы в приложении монкэйкалл.

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using CallKit;

namespace MonkeyCall
{
    public class ActiveCallManager
    {
        #region Private Variables
        private CXCallController CallController = new CXCallController ();
        #endregion

        #region Computed Properties
        public List<ActiveCall> Calls { get; set; }
        #endregion

        #region Constructors
        public ActiveCallManager ()
        {
            // Initialize
            this.Calls = new List<ActiveCall> ();
        }
        #endregion

        #region Private Methods
        private void SendTransactionRequest (CXTransaction transaction)
        {
            // Send request to call controller
            CallController.RequestTransaction (transaction, (error) => {
                // Was there an error?
                if (error == null) {
                    // No, report success
                    Console.WriteLine ("Transaction request sent successfully.");
                } else {
                    // Yes, report error
                    Console.WriteLine ("Error requesting transaction: {0}", error);
                }
            });
        }
        #endregion

        #region Public Methods
        public ActiveCall FindCall (NSUuid uuid)
        {
            // Scan for requested call
            foreach (ActiveCall call in Calls) {
                if (call.UUID.Equals(uuid)) return call;
            }

            // Not found
            return null;
        }

        public void StartCall (string contact)
        {
            // Build call action
            var handle = new CXHandle (CXHandleType.Generic, contact);
            var startCallAction = new CXStartCallAction (new NSUuid (), handle);

            // Create transaction
            var transaction = new CXTransaction (startCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void EndCall (ActiveCall call)
        {
            // Build action
            var endCallAction = new CXEndCallAction (call.UUID);

            // Create transaction
            var transaction = new CXTransaction (endCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void PlaceCallOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, true);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void RemoveCallFromOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, false);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }
        #endregion
    }
}
```

Опять же, поскольку это только имитация, объект `ActiveCallManager` поддерживает только коллекцию `ActiveCall` объектов и имеет подпрограммы для поиска данного вызова по его `UUID` свойству. Он также включает методы для запуска, завершения и изменения состояния в режиме ожидания исходящего вызова. Дополнительные сведения см. в разделе [Обработка исходящих вызовов](#handling-outgoing-calls) ниже.

### <a name="the-providerdelegate-class"></a>Класс Провидерделегате

Как упоминалось выше, a `CXProvider` обеспечивает двустороннюю связь между приложением и системой для получения уведомлений по каналу. Разработчику необходимо предоставить пользовательский `CXProviderDelegate` и подключить его к, чтобы `CXProvider` приложение обрабатывало каллкит события внешнего вида. Монкэйкалл использует следующее `CXProviderDelegate` :

```csharp
using System;
using Foundation;
using CallKit;
using UIKit;

namespace MonkeyCall
{
    public class ProviderDelegate : CXProviderDelegate
    {
        #region Computed Properties
        public ActiveCallManager CallManager { get; set;}
        public CXProviderConfiguration Configuration { get; set; }
        public CXProvider Provider { get; set; }
        #endregion

        #region Constructors
        public ProviderDelegate (ActiveCallManager callManager)
        {
            // Save connection to call manager
            CallManager = callManager;

            // Define handle types
            var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };

            // Get Image Template
            var templateImage = UIImage.FromFile ("telephone_receiver.png");

            // Setup the initial configurations
            Configuration = new CXProviderConfiguration ("MonkeyCall") {
                MaximumCallsPerCallGroup = 1,
                SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
                IconTemplateImageData = templateImage.AsPNG(),
                RingtoneSound = "musicloop01.wav"
            };

            // Create a new provider
            Provider = new CXProvider (Configuration);

            // Attach this delegate
            Provider.SetDelegate (this, null);

        }
        #endregion

        #region Override Methods
        public override void DidReset (CXProvider provider)
        {
            // Remove all calls
            CallManager.Calls.Clear ();
        }

        public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
        {
            // Create new call record
            var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

            // Monitor state changes
            activeCall.StartingConnectionChanged += (call) => {
                if (call.isConnecting) {
                    // Inform system that the call is starting
                    Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNSDate());
                }
            };

            activeCall.ConnectedChanged += (call) => {
                if (call.isConnected) {
                    // Inform system that the call has connected
                    provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNSDate ());
                }
            };

            // Start call
            activeCall.StartCall ((successful) => {
                // Was the call able to be started?
                if (successful) {
                    // Yes, inform the system
                    action.Fulfill ();

                    // Add call to manager
                    CallManager.Calls.Add (activeCall);
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.AnswerCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.EndCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Remove call from manager's queue
                    CallManager.Calls.Remove (call);

                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformSetHeldCallAction (CXProvider provider, CXSetHeldCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Update hold status
            call.isOnHold = action.OnHold;

            // Inform system of success
            action.Fulfill ();
        }

        public override void TimedOutPerformingAction (CXProvider provider, CXAction action)
        {
            // Inform user that the action has timed out
        }

        public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // Start the calls audio session here
        }

        public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // End the calls audio session and restart any non-call
            // related audio
        }
        #endregion

        #region Public Methods
        public void ReportIncomingCall (NSUuid uuid, string handle)
        {
            // Create update to describe the incoming call and caller
            var update = new CXCallUpdate ();
            update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

            // Report incoming call to system
            Provider.ReportNewIncomingCall (uuid, update, (error) => {
                // Was the call accepted
                if (error == null) {
                    // Yes, report to call manager
                    CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
                } else {
                    // Report error to user here
                    Console.WriteLine ("Error: {0}", error);
                }
            });
        }
        #endregion
    }
}
```

При создании экземпляра этого делегата ему передается объект `ActiveCallManager` , который будет использоваться для выполнения любого действия вызова. Затем определяются типы обработчиков ( `CXHandleType` ), на которые `CXProvider` будет реагировать ответ:

```csharp
// Define handle types
var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };
```

Он получает образ шаблона, который будет применен к значку приложения при выполнении вызова.

```csharp
// Get Image Template
var templateImage = UIImage.FromFile ("telephone_receiver.png");
```

Эти значения будут объединены в объект `CXProviderConfiguration` , который будет использоваться для настройки `CXProvider` :

```csharp
// Setup the initial configurations
Configuration = new CXProviderConfiguration ("MonkeyCall") {
    MaximumCallsPerCallGroup = 1,
    SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
    IconTemplateImageData = templateImage.AsPNG(),
    RingtoneSound = "musicloop01.wav"
};
```

Затем делегат создает новый объект `CXProvider` с этими конфигурациями и подключается к нему.

```csharp
// Create a new provider
Provider = new CXProvider (Configuration);

// Attach this delegate
Provider.SetDelegate (this, null);
```

При использовании Каллкит приложение больше не будет создавать и обслуживать собственные звуковые сеансы, вместо этого потребуется настроить и использовать звуковой сеанс, который система создаст и обработает для нее. 

Если это реальное приложение, `DidActivateAudioSession` метод будет использован для запуска вызова с предварительно настроенным объектом `AVAudioSession` , предоставленным системой:

```csharp
public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // Start the call's audio session here...
}
```

Он также будет использовать `DidDeactivateAudioSession` метод для завершения и освобождения подключения к системному сеансу, предоставленному системой:

```csharp
public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // End the calls audio session and restart any non-call
    // releated audio
}
```

Остальная часть кода будет подробно рассмотрена в следующих разделах.

### <a name="the-appdelegate-class"></a>Класс AppDelegate

Монкэйкалл использует AppDelegate для хранения экземпляров `ActiveCallManager` и `CXProviderDelegate` будет использоваться во всем приложении:

```csharp
using Foundation;
using UIKit;
using Intents;
using System;

namespace MonkeyCall
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        #region Constructors
        public override UIWindow Window { get; set; }
        public ActiveCallManager CallManager { get; set; }
        public ProviderDelegate CallProviderDelegate { get; set; }
        #endregion

        #region Override Methods
        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Initialize the call handlers
            CallManager = new ActiveCallManager ();
            CallProviderDelegate = new ProviderDelegate (CallManager);

            return true;
        }

        public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
        {
            // Get handle from url
            var handle = StartCallRequest.CallHandleFromURL (url);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from URL: {0}", url); 
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
        {
            var handle = StartCallRequest.CallHandleFromActivity (userActivity);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        ...
        #endregion
    }
}
```

`OpenUrl` `ContinueUserActivity` Методы переопределения и используются, когда приложение обрабатывает исходящий вызов. Дополнительные сведения см. в разделе [Обработка исходящих вызовов](#handling-outgoing-calls) ниже.

## <a name="handling-incoming-calls"></a>Обработка входящих вызовов

Существует несколько состояний и процессов, которые может проходить входящий вызов VOIP во время обычного рабочего процесса входящего вызова, например:

- Уведомление пользователя (и системы) о том, что входящий вызов существует.
- Получение уведомления, когда пользователь хочет ответить на вызов и инициализировать вызов другим пользователем.
- Сообщает системе и сети связи, когда пользователь хочет завершить текущий вызов.

В следующих разделах приводятся подробные сведения о том, как приложение может использовать Каллкит для обработки рабочего процесса входящего вызова, опять же используя приложение Монкэйкалл VOIP в качестве примера.

### <a name="informing-user-of-incoming-call"></a>Уведомление пользователя о входящем вызове

Когда удаленный пользователь запустил сеанс VOIP с локальным пользователем, происходит следующее:

[![Удаленный пользователь начал беседу VOIP](callkit-images/callkit05.png)](callkit-images/callkit05.png#lightbox)

1. Приложение получает уведомление из сети обмена данными о входящем вызове VOIP.
2. Приложение использует `CXProvider` для отправки в систему, `CXCallUpdate` уведомляя его о вызове.
3. Система публикует вызов пользовательского интерфейса системы, системных служб и любых других приложений VOIP с помощью Каллкит.

Например, в `CXProviderDelegate` :

```csharp
public void ReportIncomingCall (NSUuid uuid, string handle)
{
    // Create update to describe the incoming call and caller
    var update = new CXCallUpdate ();
    update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

    // Report incoming call to system
    Provider.ReportNewIncomingCall (uuid, update, (error) => {
        // Was the call accepted
        if (error == null) {
            // Yes, report to call manager
            CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
        } else {
            // Report error to user here
            Console.WriteLine ("Error: {0}", error);
        }
    });
}
```

Этот код создает новый `CXCallUpdate` экземпляр и прикрепляет к нему маркер, который будет обозначать вызывающий объект. Далее используется `ReportNewIncomingCall` метод `CXProvider` класса для информирования системы о вызове. Если это происходит успешно, вызов добавляется в коллекцию активных вызовов приложения, если это не так, то ошибка должна быть передана пользователю.

### <a name="user-answering-incoming-call"></a>Ответ пользователя на входящий вызов

Если пользователь хочет ответить на входящий вызов VOIP, происходит следующее:

[![Пользователь отвечает на входящий вызов VOIP](callkit-images/callkit06.png)](callkit-images/callkit06.png#lightbox)

1. Пользовательский интерфейс системы информирует систему, что пользователь хочет ответить на вызов VOIP.
2. Система отправляет в `CXAnswerCallAction` приложение `CXProvider` сообщение о намерении ответа.
3. Приложение информирует свою сеть связи, что пользователь отвечает на вызов, и вызов VOIP выполняется обычным образом.

Например, в `CXProviderDelegate` :

```csharp
public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.AnswerCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Этот код сначала ищет данный вызов в списке активных вызовов. Если вызов не найден, система получает уведомления и метод завершает работу. Если он найден, `AnswerCall` `ActiveCall` вызывается метод класса для запуска вызова, а система — сведения, если она завершается успешно или неудачно.

### <a name="user-ending-incoming-call"></a>Завершенный входящий вызов пользователя

Если пользователь желает завершить вызов из пользовательского интерфейса приложения, происходит следующее:

[![](callkit-images/callkit07.png "The user terminates the call from within the app's UI")](callkit-images/callkit07.png#lightbox)

1. Приложение создает `CXEndCallAction` , которое входит в состав `CXTransaction` , который отправляется в систему, чтобы сообщить ему о завершении вызова.
2. Система проверяет цель конечного вызова и отправляет `CXEndCallAction` обратно в приложение через `CXProvider` .
3. Затем приложение информирует свою сеть связи о завершении вызова.

Например, в `CXProviderDelegate` :

```csharp
public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.EndCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Remove call from manager's queue
            CallManager.Calls.Remove (call);

            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Этот код сначала ищет данный вызов в списке активных вызовов. Если вызов не найден, система получает уведомления и метод завершает работу. Если он найден, `EndCall` метод `ActiveCall` класса вызывается для завершения вызова, а система — сведения, если она завершается успешно или неудачно. В случае успеха вызов удаляется из коллекции активных вызовов.

## <a name="managing-multiple-calls"></a>Управление несколькими вызовами

Большинство приложений VOIP могут одновременно выполнять несколько вызовов. Например, если в настоящее время имеется активный вызов VOIP и приложение получает уведомление о том, что имеется новый входящий вызов, пользователь может приостановить или зависнуть первый вызов, чтобы ответить на второй.

В приведенной выше ситуации система отправит в `CXTransaction` приложение, которое будет включать список нескольких действий (например, `CXEndCallAction` и `CXAnswerCallAction` ). Все эти действия должны выполняться по отдельности, чтобы система могла соответствующим образом обновить пользовательский интерфейс.

## <a name="handling-outgoing-calls"></a>Обработка исходящих вызовов

Если пользователь отправит запись из списка последних элементов (в приложении для телефона), например из вызова, принадлежащего приложению, то в системе будет отправлено _назначение вызова Start_ .

[![Получение цели вызова для начала](callkit-images/callkit08.png)](callkit-images/callkit08.png#lightbox)

1. Приложение создаст _действие начать вызов_ на основе цели вызова Start, полученной от системы. 
2. Приложение будет использовать `CXCallController` для запроса действия "начать вызов" из системы.
3. Если система принимает действие, оно будет возвращено приложению через `XCProvider` делегат.
4. Приложение запускает исходящий вызов со своей сетью связи.

Дополнительные сведения о целях см. в документации по [расширениям пользовательского интерфейса для целей и](~/ios/platform/sirikit/understanding-sirikit.md) целей. 

### <a name="the-outgoing-call-lifecycle"></a>Жизненный цикл исходящего вызова

При работе с Каллкит и исходящим вызовом приложение должно сообщить системе о следующих событиях жизненного цикла:

1. **Starting** — сообщает системе, что начнется исходящий вызов.
2. **Started** — сообщает системе, что начался исходящий вызов.
3. **Подключение** . сообщает системе о том, что исходящий вызов подключается.
4. **Connected** — уведомление о том, что исходящий вызов подключен, и что обе стороны могут поговорить.

Например, следующий код приведет к запуску исходящего вызова:

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void StartCall (string contact)
{
    // Build call action
    var handle = new CXHandle (CXHandleType.Generic, contact);
    var startCallAction = new CXStartCallAction (new NSUuid (), handle);

    // Create transaction
    var transaction = new CXTransaction (startCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

Он создает `CXHandle` и использует его для настройки объекта, который входит в `CXStartCallAction` состав `CXTransaction` , который отправляется в систему с помощью `RequestTransaction` метода `CXCallController` класса. Вызывая `RequestTransaction` метод, система может разместить все существующие вызовы на удержании независимо от источника (приложения Phone, FaceTime, VoIP и т. д.), прежде чем начать новый вызов.

Запрос на запуск исходящего вызова VOIP может поступать из нескольких различных источников, например Siri, записи на карточке контакта (в приложении "Контакты") или из списка последних сообщений (в приложении Phone). В таких ситуациях приложение будет отправлять цель вызова Start в, `NSUserActivity` а AppDelegate потребуется его обрабатывать:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    var handle = StartCallRequest.CallHandleFromActivity (userActivity);

    // Found?
    if (handle == null) {
        // No, report to system
        Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
        return false;
    } else {
        // Yes, start call and inform system
        CallManager.StartCall (handle);
        return true;
    }
}
```

Здесь `CallHandleFromActivity` метод вспомогательного класса `StartCallRequest` используется для получения маркера для вызываемого человека (см. [класс старткаллрекуест](#the-startcallrequest-class) выше).

`PerformStartCallAction`Метод [класса провидерделегате](#the-providerdelegate-class) используется, чтобы запустить фактический исходящий вызов и информировать систему о его жизненном цикле:

```csharp
public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
{
    // Create new call record
    var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

    // Monitor state changes
    activeCall.StartingConnectionChanged += (call) => {
        if (call.IsConnecting) {
            // Inform system that the call is starting
            Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNSDate());
        }
    };

    activeCall.ConnectedChanged += (call) => {
        if (call.IsConnected) {
            // Inform system that the call has connected
            Provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNSDate ());
        }
    };

    // Start call
    activeCall.StartCall ((successful) => {
        // Was the call able to be started?
        if (successful) {
            // Yes, inform the system
            action.Fulfill ();

            // Add call to manager
            CallManager.Calls.Add (activeCall);
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Он создает экземпляр `ActiveCall` класса (для хранения сведений о выполняемом вызове) и заполняется вызываемым пользователем. `StartingConnectionChanged`События и `ConnectedChanged` используются для мониторинга и отправки отчетов о жизненном цикле исходящего вызова. Вызов запускается, и система сообщила о том, что действие было выполнено.

### <a name="ending-an-outgoing-call"></a>Завершение исходящего вызова

Когда пользователь завершает работу с исходящим вызовом и хочет завершить его, можно использовать следующий код:

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void EndCall (ActiveCall call)
{
    // Build action
    var endCallAction = new CXEndCallAction (call.UUID);

    // Create transaction
    var transaction = new CXTransaction (endCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

Если создает объект `CXEndCallAction` с UUID вызова End, объединяет его в объект `CXTransaction` , который отправляется в систему с помощью `RequestTransaction` метода `CXCallController` класса. 

## <a name="additional-callkit-details"></a>Дополнительные сведения о Каллкит

В этом разделе рассматриваются некоторые дополнительные сведения, которые необходимо учитывать разработчику при работе с Каллкит, например:

- Конфигурация поставщика
- Ошибки действий
- Системные ограничения
- Звук VOIP

### <a name="provider-configuration"></a>Конфигурация поставщика

Конфигурация поставщика позволяет приложению VOIP iOS 10 настраивать взаимодействие с пользователем (внутри собственного пользовательского интерфейса вызова) при работе с Каллкит.

Приложение может выполнять следующие типы настроек:

- Отображение локализованного имени.
- Включить поддержку видеовызовов.
- Настройте кнопки в пользовательском интерфейсе, выполнив собственный значок изображения шаблона. Взаимодействие с пользователем с пользовательскими кнопками отправляется непосредственно в приложение для обработки. 

### <a name="action-errors"></a>Ошибки действий

приложения для iOS 10 VOIP, использующие Каллкит, нуждаются в некорректной обработке действий и постоянно сообщают пользователю о состоянии действия. 

Примите во внимание следующий пример:

1. Приложение получило действие "начать вызов" и начал процесс инициализации нового вызова VOIP с помощью сети связи.
2. Так как функция сетевого подключения ограничена или отсутствует, это подключение завершается сбоем.
3. Приложение *должно* отправить сообщение о **сбое** обратно в действие вызова Start (), `Action.Fail()` чтобы сообщить системе о сбое.
4. Это позволяет системе информировать пользователя о состоянии вызова. Например, для вывода пользовательского интерфейса ошибки вызова.

Кроме того, приложение iOS 10 VOIP должно реагировать на _ошибки времени ожидания_ , которые могут возникать, если ожидаемое действие не может быть обработано в течение заданного промежутка времени. С каждым типом действия, предоставленным Каллкит, связано максимальное значение времени ожидания. Эти значения времени ожидания гарантируют, что любое действие Каллкит, запрошенное пользователем, обрабатывается на время реагирования, тем самым обеспечивая возможность гибкого и быстрого реагирования операционной системы.

Существует несколько методов делегата поставщика ( `CXProviderDelegate` ), которые должны быть переопределены для корректной работы этих ситуаций времени ожидания.

### <a name="system-restrictions"></a>Системные ограничения

В зависимости от текущего состояния устройства iOS, на котором выполняется приложение iOS 10 VOIP, могут быть применены некоторые ограничения системы.

Например, входящий вызов VOIP может быть ограничен системой, если:

1. Человек, вызывающий, находится в списке заблокированных вызывающих объектов пользователя.
2. Устройство iOS пользователя находится в режиме "не беспокоить".

Если вызов VOIP ограничен любой из этих ситуаций, используйте следующий код, чтобы его обработать:

```csharp
public class ProviderDelegate : CXProviderDelegate
{
...

    public void ReportIncomingCall (NSUuid uuid, string handle)
    {
        // Create update to describe the incoming call and caller
        var update = new CXCallUpdate ();
        update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);
    
        // Report incoming call to system
        Provider.ReportNewIncomingCall (uuid, update, (error) => {
            // Was the call accepted
            if (error == null) {
                // Yes, report to call manager
                CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
            } else {
                // Report error to user here
                if (error.Code == (int)CXErrorCodeIncomingCallError.CallUuidAlreadyExists) {
                    // Handle duplicate call ID
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByBlockList) {
                    // Handle call from blocked user
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByDoNotDisturb) {
                    // Handle call while in do-not-disturb mode
                } else {
                    // Handle unknown error
                }
            }
        });
    }

}
```

### <a name="voip-audio"></a>Звук VOIP

Каллкит предоставляет несколько преимуществ для обработки звуковых ресурсов, которые требуются приложению VOIP iOS 10 при динамическом вызове VOIP. Одним из крупнейших преимуществ является то, что при работе в iOS 10 приоритетные сеансы приложения будут иметь повышенные приоритеты. Это тот же уровень приоритета, что и у встроенных приложений Phone и FaceTime, и Этот улучшенный уровень приоритета не позволит другим работающим приложениям прерывать сеанс звука приложения VOIP.

Кроме того, Каллкит имеет доступ к другим указаниям для маршрутизации аудио, которые могут повысить производительность и интеллектуально маршрутизировать звук VOIP к конкретным выходным устройствам в ходе интерактивного вызова на основе пользовательских настроек и состояний устройств. Например, на основе подключенных устройств, таких как наушники Bluetooth, динамическое подключение Карплай или параметры специальных возможностей.

Во время жизненного цикла обычного вызова VOIP с использованием Каллкит приложению потребуется настроить поток аудио, который Каллкит предоставит. Взгляните на следующий пример:

[![Последовательность действий "начать вызов"](callkit-images/callkit09.png)](callkit-images/callkit09.png#lightbox)

1. Приложение получает действие начать вызов, чтобы ответить на входящий вызов.
2. Прежде чем это действие будет выполнено приложением, он предоставляет конфигурацию, которая потребуется для его `AVAudioSession` .
3. Приложение информирует систему о том, что действие выполнено.
4. Перед подключением вызов Каллкит предоставляет высокий приоритет, `AVAudioSession` соответствующий конфигурации, запрошенной приложением. Приложение будет уведомлено с помощью `DidActivateAudioSession` метода своего `CXProviderDelegate` .

## <a name="working-with-call-directory-extensions"></a>Работа с расширениями каталога вызовов

При работе с Каллкит _расширения каталога вызовов_ позволяют добавлять заблокированные номера вызовов и указывать номера, относящиеся к конкретному приложению VoIP, для контактов в приложении-контакте на устройстве iOS.

### <a name="implementing-a-call-directory-extension"></a>Реализация расширения каталога вызовов

Чтобы реализовать расширение каталога вызова в приложении Xamarin. iOS, выполните следующие действия.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Откройте решение приложения в Visual Studio для Mac.
2. Щелкните правой кнопкой мыши имя решения в **Обозреватель решений** и выберите **Добавить**  >  **Добавить новый проект**.
3. Выберите **iOS**  >  **расширения**iOS  >  **расширения каталога** и нажмите кнопку **Далее** : 

    [![Создание нового расширения каталога вызовов](callkit-images/calldir01.png)](callkit-images/calldir01.png#lightbox)
4. Введите **имя** для расширения и нажмите кнопку **Далее** : 

    [![Ввод имени для расширения](callkit-images/calldir02.png)](callkit-images/calldir02.png#lightbox)
5. При необходимости измените **имя проекта** или **имя решения** , а затем нажмите кнопку **создать** : 

    [![Создание проекта](callkit-images/calldir03.png)](callkit-images/calldir03.png#lightbox) 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Откройте решение приложения в Visual Studio.
2. Щелкните правой кнопкой мыши имя решения в **Обозреватель решений** и выберите **Добавить**  >  **Добавить новый проект**.
3. Выберите **iOS**  >  **расширения**iOS  >  **расширения каталога** и нажмите кнопку **Далее** : 

    [![Создание нового расширения каталога вызовов](callkit-images/calldir01w.png)](callkit-images/calldir01.png#lightbox)
4. Введите **имя** расширения и нажмите кнопку " **ОК** ".

-----

В проект будет добавлен `CallDirectoryHandler.cs` класс, который выглядит следующим образом:

```csharp
using System;

using Foundation;
using CallKit;

namespace MonkeyCallDirExtension
{
    [Register ("CallDirectoryHandler")]
    public class CallDirectoryHandler : CXCallDirectoryProvider, ICXCallDirectoryExtensionContextDelegate
    {
        #region Constructors
        protected CallDirectoryHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void BeginRequest (CXCallDirectoryExtensionContext context)
        {
            context.Delegate = this;

            if (!AddBlockingPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add blocking phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 1, null);
                context.CancelRequest (error);
                return;
            }

            if (!AddIdentificationPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add identification phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 2, null);
                context.CancelRequest (error);
                return;
            }

            context.CompleteRequest (null);
        }
        #endregion

        #region Private Methods
        private bool AddBlockingPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to block from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 14085555555, 18005555555 };

            foreach (var phoneNumber in phoneNumbers)
                context.AddBlockingEntry (phoneNumber);

            return true;
        }

        private bool AddIdentificationPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to identify and their identification labels from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 18775555555, 18885555555 };
            string [] labels = { "Telemarketer", "Local business" };

            for (var i = 0; i < phoneNumbers.Length; i++) {
                long phoneNumber = phoneNumbers [i];
                string label = labels [i];
                context.AddIdentificationEntry (phoneNumber, label);
            }

            return true;
        }
        #endregion

        #region Public Methods
        public void RequestFailed (CXCallDirectoryExtensionContext extensionContext, NSError error)
        {
            // An error occurred while adding blocking or identification entries, check the NSError for details.
            // For Call Directory error codes, see the CXErrorCodeCallDirectoryManagerError enum.
            //
            // This may be used to store the error details in a location accessible by the extension's containing app, so that the
            // app may be notified about errors which occurred while loading data even if the request to load data was initiated by
            // the user in Settings instead of via the app itself.
        }
        #endregion
    }
}
```

`BeginRequest`Метод в обработчике каталога вызовов потребуется изменить, чтобы обеспечить требуемую функциональность. В примере выше он пытается задать список заблокированных и доступных номеров в базе данных Contacts приложения VOIP. Если по какой либо причине произошел сбой любого запроса, создайте `NSError` для описания сбоя и передайте ему `CancelRequest` метод `CXCallDirectoryExtensionContext` класса.

Чтобы задать заблокированные числа, используйте `AddBlockingEntry` метод `CXCallDirectoryExtensionContext` класса. Числа, предоставленные методу, _должны_ быть отсортированы по возрастанию. Для оптимальной производительности и использования памяти при наличии большого числа телефонных номеров рассмотрите возможность загрузки подмножества чисел в заданный момент времени и использования пулов автоосвобождения для освобождения объектов, выделенных во время каждого пакета загружаемых пакетов.

Чтобы сообщить приложению о контактных номерах, известном для приложения VOIP, используйте `AddIdentificationEntry` метод `CXCallDirectoryExtensionContext` класса и укажите число и идентифицирующую метку. Опять же, числа, предоставленные методу, _должны_ быть отсортированы по возрастанию. Для оптимальной производительности и использования памяти при наличии большого числа телефонных номеров рассмотрите возможность загрузки подмножества чисел в заданный момент времени и использования пулов автоосвобождения для освобождения объектов, выделенных во время каждого пакета загружаемых пакетов.

## <a name="summary"></a>Итоги

В этой статье рассматривается новый API Каллкит, выпущенный компанией Apple в iOS 10, и способы его реализации в приложениях VOIP для Xamarin. iOS. В нем показано, как Каллкит позволяет интегрировать приложение в систему iOS, а также предоставляет возможность учитывать соответствие функций встроенным приложениям (например, телефону) и тем, как она увеличивает видимость приложения в iOS в таких местах, как блокировки и домашние экраны, через Siriные взаимодействия и через приложения контактов.

## <a name="related-links"></a>Связанные ссылки

- [Примеры iOS 10](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
