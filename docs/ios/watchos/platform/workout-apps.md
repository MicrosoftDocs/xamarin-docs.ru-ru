---
title: watchOSные приложения в Xamarin
description: В этой статье рассматриваются улучшения, внесенные компанией Apple для тренировок приложений в watchOS 3, и способы их реализации в Xamarin.
ms.prod: xamarin
ms.assetid: F1D19635-A738-43E5-9873-1FC1BA44EEDF
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: c4fc1607667dd6201c28c4d00a2938760e429f0f
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938987"
---
# <a name="watchos-workout-apps-in-xamarin"></a>watchOSные приложения в Xamarin

_В этой статье рассматриваются улучшения, внесенные компанией Apple для тренировок приложений в watchOS 3, и способы их реализации в Xamarin._

Новые возможности для watchOS 3. приложения, связанные с тренировками, могут работать в фоновом режиме на Apple Watch и получать доступ к данным HealthKit. Их родительское приложение на основе iOS 10 также может запускать приложение на основе watchOS 3 без вмешательства пользователя.

Будут подробно рассмотрены следующие темы:

## <a name="about-workout-apps"></a>О тренировках приложений

Пользователи пригодных и тренировок приложений могут быть очень специализированными, обрабатывали несколько часов дня в соответствии с целями их работоспособности и пригодности. В результате они хотят реагировать на запросы, простые в использовании приложения, которые точно собираются и отображают данные и легко интегрируются с работоспособностью Apple.

Хорошо спроектированное приложение для тренировок и тренировки помогает пользователям создавать диаграммы своих действий, чтобы достичь целей их достижения. Используя Apple Watch, приложения для тренировок и тренировки имеют мгновенный доступ к частоте сердца, калорие записи и обнаружения действий.

[![Пример приложения для тренировок и тренировки](workout-apps-images/workout01.png)](workout-apps-images/workout01.png#lightbox)

Если вы не знакомы с watchOS 3, то в _фоновом режиме_ вы получаете связанные с тренировками приложения, которые могут выполняться в фоновом режиме на Apple Watch и получать доступ к данным HealthKit.

В этом документе рассматривается функция фонового выполнения, охватывающая жизненный цикл приложения, а также показано, как приложение тренировки может участвовать в работе на _кольцах действий_ пользователя на Apple Watch.

## <a name="about-workout-sessions"></a>О семинарах

Основой каждого приложения для тренировки является _сеанс тренировки_ ( `HKWorkoutSession` ), который пользователь может запускать и прекращать. API-интерфейс тренировки прост в реализации и предоставляет несколько преимуществ для тренировки в приложении, например:

- Обнаружение движения и калорие записи на основе типа действия.
- Автоматическое участие в кольцах действий пользователя.
- Во время сеанса приложение будет автоматически отображаться каждый раз, когда пользователь выводит устройство из спящего режима (либо путем порождения своего перевода, либо взаимодействия с Apple Watch).

## <a name="about-background-running"></a>О фоновом запуске

Как упоминалось выше, с помощью watchOS 3 приложение тренировки можно настроить для запуска в фоновом режиме. Использование фонового приложения для тренировки может обрабатывать данные с датчиков Apple Watch при выполнении в фоновом режиме. Например, приложение может продолжать отслеживать частоту сердца пользователя, даже если оно больше не отображается на экране.

В фоновом режиме также предоставляется возможность предоставлять пользователю интерактивную обратную связь в любое время во время активного сеанса тренировки, например отправку оповещения хаптик для информирования пользователя о его текущем состоянии.

Кроме того, фоновое выполнение позволяет приложению быстро обновить пользовательский интерфейс, чтобы пользователь получил актуальные данные при быстром взгляде на их Apple Watch.

Чтобы обеспечить высокую производительность Apple Watch, приложение наблюдения, работающее в фоновом режиме, должно ограничить объем фоновой работы для экономии аккумулятора. Если приложение использует слишком много ресурсов ЦП в фоновом режиме, оно может быть приостановлено watchOS.

### <a name="enabling-background-running"></a>Включение фонового выполнения

Для включения фонового выполнения выполните следующие действия.

1. В **Обозреватель решений**дважды щелкните файл приложения на вспомогательном устройстве iPhone для расширения контрольных значений, `Info.plist` чтобы открыть его для редактирования.
2. Переключитесь в представление **исходного кода** : 

    [![Представление источника](workout-apps-images/plist01.png)](workout-apps-images/plist01.png#lightbox)
3. Добавьте новый раздел с именем `WKBackgroundModes` и задайте для **Type** него тип `Array` : 

    [![Добавьте новый раздел с именем Вкбаккграундмодес.](workout-apps-images/plist02.png)](workout-apps-images/plist02.png#lightbox)
4. Добавьте новый элемент в массив с **типом** `String` и значением `workout-processing` : 

    [![Добавить новый элемент в массив с типом строки и значением для обработки тренировок](workout-apps-images/plist03.png)](workout-apps-images/plist03.png#lightbox)
5. Сохраните изменения в файле.

## <a name="starting-a-workout-session"></a>Начало сеанса тренировки

Запуск сеанса тренировки состоит из трех основных этапов.

[![Три основных шага для запуска сеанса тренировки](workout-apps-images/workout02.png)](workout-apps-images/workout02.png#lightbox)

1. Приложение должно запросить авторизацию для доступа к данным в HealthKit.
2. Создайте объект конфигурации тренировки, чтобы начать работу с типом тренировки.
3. Создайте и запустите сеанс тренировки, используя только что созданную конфигурацию тренировки.

### <a name="requesting-authorization"></a>Запрос авторизации

Прежде чем приложение сможет получить доступ к данным HealthKit пользователя, оно должно запросить и получить авторизацию от пользователя. В зависимости от характера тренировки приложение может делать запросы следующих типов:

- Авторизация для записи данных:
  - Тренировки
- Авторизация для чтения данных:
  - Потребляемая энергия
  - расстояние;
  - Частота сердца  

Прежде чем приложение сможет запросить авторизацию, его необходимо настроить для доступа к HealthKit.

Выполните следующие действия.

1. В **обозревателе решений** дважды щелкните файл `Entitlements.plist`, чтобы открыть его для редактирования.
2. Прокрутите вниз и установите флажок **включить HealthKit**: 

    [![Установите флажок Включить HealthKit](workout-apps-images/auth01.png)](workout-apps-images/auth01.png#lightbox)
3. Сохраните изменения в файле.
4. Следуйте инструкциям в разделе [ЯВНЫЙ идентификатор приложения и профиль подготовки](~/ios/platform/healthkit.md) , чтобы [связать идентификатор приложения и профиль подготовки с приложением Xamarin. iOS](~/ios/platform/healthkit.md) статьи [Общие сведения о HealthKit](~/ios/platform/healthkit.md) , чтобы правильно подготовить приложение.
5. Наконец, используйте инструкции в пакете " [программирование работоспособности](~/ios/platform/healthkit.md) " и [запросите разрешение из](~/ios/platform/healthkit.md) разделов "пользователь" статьи [Общие сведения о HealthKit](~/ios/platform/healthkit.md) , чтобы запросить авторизацию для доступа к хранилищу данных HealthKit пользователя.

### <a name="setting-the-workout-configuration"></a>Настройка конфигурации тренировки

Сеансы тренировки создаются с помощью объекта настройки тренировки ( `HKWorkoutConfiguration` ), который указывает тип тренировки (например, `HKWorkoutActivityType.Running` ) и место тренировки (например, `HKWorkoutSessionLocationType.Outdoor` ):

```csharp
using HealthKit;
...

// Create a workout configuration
var configuration = new HKWorkoutConfiguration () {
  ActivityType = HKWorkoutActivityType.Running,
  LocationType = HKWorkoutSessionLocationType.Outdoor
};
```

### <a name="creating-a-workout-session-delegate"></a>Создание делегата сеанса тренировки 

Чтобы обрабатывались события, которые могут возникнуть во время сеанса тренировки, приложению потребуется создать экземпляр делегата для сеанса тренировки. Добавьте в проект новый класс и выключить его из `HKWorkoutSessionDelegate` класса. Для примера внешнего запуска он может выглядеть следующим образом:

```csharp
using System;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
  public class OutdoorRunDelegate : HKWorkoutSessionDelegate
  {
    #region Computed Properties
    public HKHealthStore HealthStore { get; private set; }
    public HKWorkoutSession WorkoutSession { get; private set;}
    #endregion

    #region Constructors
    public OutdoorRunDelegate (HKHealthStore healthStore, HKWorkoutSession workoutSession)
    {
      // Initialize
      this.HealthStore = healthStore;
      this.WorkoutSession = workoutSession;

      // Attach this delegate to the session
      workoutSession.Delegate = this;
    }
    #endregion

    #region Override Methods
    public override void DidFail (HKWorkoutSession workoutSession, NSError error)
    {
      // Handle workout session failing
      RaiseFailed ();
    }

    public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
    {
      // Take action based on the change in state
      switch (toState) {
      case HKWorkoutSessionState.NotStarted:
        break;
      case HKWorkoutSessionState.Paused:
        RaisePaused ();
        break;
      case HKWorkoutSessionState.Running:
        RaiseRunning ();
        break;
      case HKWorkoutSessionState.Ended:
        RaiseEnded ();
        break;
      }

    }

    public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
    {
      base.DidGenerateEvent (workoutSession, @event);
    }
    #endregion

    #region Events
    public delegate void OutdoorRunEventDelegate ();

    public event OutdoorRunEventDelegate Failed;
    internal void RaiseFailed ()
    {
      if (this.Failed != null) this.Failed ();
    }

    public event OutdoorRunEventDelegate Paused;
    internal void RaisePaused ()
    {
      if (this.Paused != null) this.Paused ();
    }

    public event OutdoorRunEventDelegate Running;
    internal void RaiseRunning ()
    {
      if (this.Running != null) this.Running ();
    }

    public event OutdoorRunEventDelegate Ended;
    internal void RaiseEnded ()
    {
      if (this.Ended != null) this.Ended ();
    }
    #endregion
  }
}
```

Этот класс создает несколько событий, которые будут вызываться при изменении состояния сеанса тренировки ( `DidChangeToState` ) и в случае сбоя сеанса тренировки ( `DidFail` ). 

### <a name="creating-a-workout-session"></a>Создание сеанса тренировки

С помощью делегата настройки тренировки и сеанса тренировки, созданного выше, чтобы создать новый сеанс тренировки и запустить его в стандартном хранилище HealthKit пользователя по умолчанию:

```csharp
using HealthKit;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public OutdoorRunDelegate RunDelegate { get; set; }
#endregion
...

private void StartOutdoorRun ()
{
  // Create a workout configuration
  var configuration = new HKWorkoutConfiguration () {
    ActivityType = HKWorkoutActivityType.Running,
    LocationType = HKWorkoutSessionLocationType.Outdoor
  };

  // Create workout session
  // Start workout session
  NSError error = null;
  var workoutSession = new HKWorkoutSession (configuration, out error);

  // Successful?
  if (error != null) {
    // Report error to user and return
    return;
  }

  // Create workout session delegate and wire-up events
  RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

  RunDelegate.Failed += () => {
    // Handle the session failing
  };

  RunDelegate.Paused += () => {
    // Handle the session being paused
  };

  RunDelegate.Running += () => {
    // Handle the session running
  };

  RunDelegate.Ended += () => {
    // Handle the session ending
  };

  // Start session
  HealthStore.StartWorkoutSession (workoutSession);
}
```

Если приложение запускает этот сеанс тренировки и пользователь переключается на свой экран, рядом с ним отображается маленький зеленый значок "работающий мужчина":

[![Маленький зеленый значок с изображением Man, отображаемый над лицом](workout-apps-images/workout03.png)](workout-apps-images/workout03.png#lightbox)

Если пользователь отменяет этот значок, он переводится обратно в приложение.

## <a name="data-collection-and-control"></a>Сбор и управление данными

После настройки и запуска сеанса тренировки приложению потребуется получить данные о сеансе (например, частоту использования пользователя) и управлять состоянием сеанса:

[![Схема сбора и управления данными](workout-apps-images/workout04.png)](workout-apps-images/workout04.png#lightbox)

1. **Наблюдение за примерами** . приложение должно получать сведения от HealthKit, которые будут обрабатываться и отображаться для пользователя.
2. **Наблюдение за событиями** . приложение должно реагировать на события, созданные HealthKit или из пользовательского интерфейса приложения (например, приостановка тренировки).
3. **Введите состояние выполнения** — сеанс запущен и выполняется в данный момент.
4. **Введите приостановленное состояние** — пользователь приостановил текущий сеанс тренировки и сможет перезапустить его позже. Пользователь может несколько раз переключаться между запущенным и приостановленным состояниями в одном сеансе тренировки.
5. **Завершение тренировки** в любой момент, когда пользователь может закончить сеанс тренировки, или срок его действия истекает и заканчивается самостоятельно, если это был лимитный тренировка (например, два сеанса работы).

Последним шагом является сохранение результатов тренировки в хранилище данных HealthKit пользователя.

### <a name="observing-healthkit-samples"></a>Наблюдение за примерами HealthKit

Приложению потребуется открыть _запрос объекта привязки_ для каждой из HealthKit точек данных, в которых он заинтересован, например частоту сердца или активную энергию. Для каждой наблюдаемой точки данных необходимо создать обработчик обновлений для записи новых данных по мере их отправки в приложение.

Из этих точек данных приложение может накапливать итоги (например, общее расстояние выполнения) и при необходимости обновлять пользовательский интерфейс. Кроме того, приложение может уведомлять пользователей о достижении определенной цели или достижения, например о завершении следующей мили запуска.

Взгляните на следующий пример кода:

```csharp
private void ObserveHealthKitSamples ()
{
  // Get the starting date of the required samples
  var datePredicate = HKQuery.GetPredicateForSamples (WorkoutSession.StartDate, null, HKQueryOptions.StrictStartDate);

  // Get data from the local device
  var devices = new NSSet<HKDevice> (new HKDevice [] { HKDevice.LocalDevice });
  var devicePredicate = HKQuery.GetPredicateForObjectsFromDevices (devices);

  // Assemble compound predicate
  var queryPredicate = NSCompoundPredicate.CreateAndPredicate (new NSPredicate [] { datePredicate, devicePredicate });

  // Get ActiveEnergyBurned
  var queryActiveEnergyBurned = new HKAnchoredObjectQuery (HKQuantityType.Create (HKQuantityTypeIdentifier.ActiveEnergyBurned), queryPredicate, null, HKSampleQuery.NoLimit, (query, addedObjects, deletedObjects, newAnchor, error) => {
    // Valid?
    if (error == null) {
      // Yes, process all returned samples
      foreach (HKSample sample in addedObjects) {
        var quantitySample = sample as HKQuantitySample;
        ActiveEnergyBurned += quantitySample.Quantity.GetDoubleValue (HKUnit.Joule);
      }
      
      // Update User Interface
      ...
    }
  });

  // Start Query
  HealthStore.ExecuteQuery (queryActiveEnergyBurned);
                                        
}
```

Он создает предикат, чтобы задать начальную дату, на которую требуется получить данные с помощью `GetPredicateForSamples` метода. Он создает набор устройств для получения сведений о HealthKit с помощью `GetPredicateForObjectsFromDevices` метода, в данном случае только локальные Apple Watch ( `HKDevice.LocalDevice` ). Два предиката объединяются в составной предикат ( `NSCompoundPredicate` ) с помощью `CreateAndPredicate` метода.

`HKAnchoredObjectQuery`Для нужной точки данных создается новый объект (в этом случае `HKQuantityTypeIdentifier.ActiveEnergyBurned` — активная точка данных, записанная в энергопотреблении), ограничения на объем возвращаемых данных () не накладываются, `HKSampleQuery.NoLimit` а обработчик обновлений определяется для обработки данных, возвращаемых приложению из HealthKit. 

Обработчик обновлений будет вызываться каждый раз, когда новые данные доставляются в приложение для заданной точки данных. Если ошибка не возвращается, приложение может безопасно считывать данные, выполнять необходимые вычисления и обновлять пользовательский интерфейс по мере необходимости.

Код выполняет циклическое переключение всех примеров ( `HKSample` ), возвращаемых в `addedObjects` массиве, и приводит их к примеру Quantity ( `HKQuantitySample` ). Затем он получает двойное значение примера как жауле ( `HKUnit.Joule` ) и накапливает его в суммарный объем активной энергии, записанной для тренировки, и обновляет пользовательский интерфейс.

### <a name="achieved-goal-notification"></a>Уведомление о достижении цели

Как упоминалось выше, когда пользователь достигает цели в приложении тренировки (например, завершение первой мили выполнения), он может отправить хаптик отзыв пользователю с помощью Taptic Engine. Приложение также должно обновить пользовательский интерфейс на этом этапе, так как пользователь, скорее всего, получит свое мнение, чтобы увидеть событие, предлагающее отправить отзыв.

Чтобы воспроизвести отзыв хаптик, используйте следующий код:

```csharp
// Play haptic feedback
WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);
```

### <a name="observing-events"></a>Наблюдение за событиями

События — это метки времени, которые приложение может использовать для выделения определенных точек во время тренировки пользователя. Некоторые события будут созданы непосредственно приложением и сохранены в тренировки, и некоторые события будут автоматически созданы HealthKit.

Чтобы наблюдать за событиями, создаваемыми HealthKit, приложение переопределит `DidGenerateEvent` метод `HKWorkoutSessionDelegate` :

```csharp
using System.Collections.Generic;
...

public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
  base.DidGenerateEvent (workoutSession, @event);
  
  // Save HealthKit generated event
  WorkoutEvents.Add (@event);
  
  // Take action based on the type of event
  switch (@event.Type) {
  case HKWorkoutEventType.Lap:
    break;
  case HKWorkoutEventType.Marker:
    break;
  case HKWorkoutEventType.MotionPaused:
    break;
  case HKWorkoutEventType.MotionResumed:
    break;
  case HKWorkoutEventType.Pause:
    break;
  case HKWorkoutEventType.Resume:
    break;
  }
}
```

Компания Apple добавила следующие новые типы событий в watchOS 3:

- `HKWorkoutEventType.Lap`— Для событий, которые нарушают тренировку на одинаковые расстояния. Например, для пометки на одну запись во время выполнения.
- `HKWorkoutEventType.Marker`— Для произвольных точек, представляющих интерес, в ходе тренировки. Например, достижение определенной точки на маршруте внешнего запуска.

Эти новые типы могут быть созданы приложением и сохранены в тренировки для последующего использования при создании графов и статистики.

Чтобы создать событие маркера, выполните следующие действия.

```csharp
using System.Collections.Generic;
...

public float MilesRun { get; set; }
public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

public void ReachedNextMile ()
{
  // Create and save marker event
  var markerEvent = HKWorkoutEvent.Create (HKWorkoutEventType.Marker, NSDate.Now);
  WorkoutEvents.Add (markerEvent);

  // Notify user
  NotifyUserOfReachedMileGoal (++MilesRun);
}
```

Этот код создает новый экземпляр события маркера ( `HKWorkoutEvent` ) и сохраняет его в закрытой коллекции событий (которая позже будет записана в сеанс тренировки) и уведомляет пользователя о событии через хаптикс.

### <a name="pausing-and-resuming-workouts"></a>Приостановка и возобновление тренировок

В любой момент в ходе тренировки пользователь может временно приостановить тренировку и возобновить ее позже. Например, они могут приостановить выполнение внутри, чтобы получить важный вызов и возобновить выполнение после завершения вызова.

Пользовательский интерфейс приложения должен обеспечивать способ приостановки и возобновления тренировки (путем вызова в HealthKit), чтобы Apple Watch мог экономить как мощность, так и объем данных, пока пользователь приостановил действие. Кроме того, приложение должно игнорировать любые новые точки данных, которые могут быть получены, когда сеанс тренировки находится в приостановленном состоянии.

HealthKit отвечает на паузу и возобновление вызовов путем создания событий приостановки и возобновления. Пока сеанс тренировки приостанавливается, новые события или данные не будут отправляться в приложение, HealthKit до возобновления сеанса.

Чтобы приостановить и возобновить сеанс тренировки, используйте следующий код:

```csharp
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public HKWorkoutSession WorkoutSession { get; set;}
...

public void PauseWorkout ()
{
  // Pause the current workout
  HealthStore.PauseWorkoutSession (WorkoutSession);
}

public void ResumeWorkout ()
{
  // Pause the current workout
  HealthStore.ResumeWorkoutSession (WorkoutSession);
}
```

События приостановки и возобновления, которые будут созданы из HealthKit, могут быть обработаны путем переопределения `DidGenerateEvent` метода из `HKWorkoutSessionDelegate` :

```csharp
public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
  base.DidGenerateEvent (workoutSession, @event);

  // Take action based on the type of event
  switch (@event.Type) {
  case HKWorkoutEventType.Pause:
    break;
  case HKWorkoutEventType.Resume:
    break;
  }
}
```

### <a name="motion-events"></a>События движения

Кроме того, в watchOS 3 — это события, приостановленные движением ( `HKWorkoutEventType.MotionPaused` ) и движением с возобновлением движения ( `HKWorkoutEventType.MotionResumed` ). Эти события автоматически создаются HealthKit во время выполнения тренировки, когда пользователь запускает и прекращает перемещение.

Когда приложение получает событие приостановки движения, оно должно остановить сбор данных до тех пор, пока пользователь не возобновит движение и не будет получено событие возобновления движения. Приложению не следует приостанавливать сеанс тренировки в ответ на событие, приостановленное перемещением.

> [!IMPORTANT]
> События "пауза перемещения" и "возобновление движения" поддерживаются только для типа действия Руннингворкаут ( `HKWorkoutActivityType.Running` ).

Опять же, эти события могут быть обработаны путем переопределения `DidGenerateEvent` метода класса `HKWorkoutSessionDelegate` :

```csharp
public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
  base.DidGenerateEvent (workoutSession, @event);
  
  // Take action based on the type of event
  switch (@event.Type) {
  case HKWorkoutEventType.MotionPaused:
    break;
  case HKWorkoutEventType.MotionResumed:
    break;
  }
}

```

## <a name="ending-and-saving-the-workout-session"></a>Завершение и сохранение сеанса тренировки

Когда пользователь завершит тренировку, приложению потребуется завершить текущий сеанс тренировки и сохранить его в базе данных HealthKit. Тренировки, сохраненные в HealthKit, будут автоматически отображаться в списке действий тренировки.

Новые возможности iOS 10, включая список действий тренировки на iPhone пользователя. Таким образом, даже если Apple Watch не находится поблизости, тренировка будет представлена на телефоне.

Тренировки, включающие образцы энергии, будут обновлять кольцо пользователя в приложении "действия", чтобы сторонние приложения могли участвовать в ежедневном перемещении пользователя.

Для завершения и сохранения сеанса тренировки необходимо выполнить следующие действия.

[![Завершение и сохранение схемы семинара тренировок](workout-apps-images/workout05.png)](workout-apps-images/workout05.png#lightbox)

1. Во первых, приложению придется завершить сеанс тренировки.
2. Сеанс тренировки сохраняется в HealthKit.
3. Добавьте все образцы (например, запись энергии или расстояние) в сохраненный сеанс тренировки.

### <a name="ending-the-session"></a>Завершение сеанса

Чтобы завершить сеанс тренировки, вызовите `EndWorkoutSession` метод, `HKHealthStore` передаваемый в `HKWorkoutSession` :

```csharp
public HKHealthStore HealthStore { get; private set; }
public HKWorkoutSession WorkoutSession { get; private set;}
...

public void EndOutdoorRun ()
{
  // End the current workout session
  HealthStore.EndWorkoutSession (WorkoutSession);
}
```

Это приведет к сбросу датчиков устройств в нормальный режим. Когда HealthKit завершает тренировку, он получит обратный вызов `DidChangeToState` метода класса `HKWorkoutSessionDelegate` :

```csharp
public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
{
  // Take action based on the change in state
  switch (toState) {
  ...
  case HKWorkoutSessionState.Ended:
    StopObservingHealthKitSamples ();
    RaiseEnded ();
    break;
  }

}
```

### <a name="saving-the-session"></a>Идет сохранение сеанса

Когда приложение завершит сеанс тренировки, необходимо создать тренировку ( `HKWorkout` ) и сохранить ее (вместе с событиями) в хранилище данных HealthKit ( `HKHealthStore` ):

```csharp
public HKHealthStore HealthStore { get; private set; }
public HKWorkoutSession WorkoutSession { get; private set;}
public float MilesRun { get; set; }
public double ActiveEnergyBurned { get; set;}
public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

private void SaveWorkoutSession ()
{
  // Build required workout quantities 
  var energyBurned = HKQuantity.FromQuantity (HKUnit.Joule, ActiveEnergyBurned);
  var distance = HKQuantity.FromQuantity (HKUnit.Mile, MilesRun);

  // Create any required metadata
  var metadata = new NSMutableDictionary ();
  metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));

  // Create workout
  var workout = HKWorkout.Create (HKWorkoutActivityType.Running, 
                                  WorkoutSession.StartDate, 
                                  NSDate.Now, 
                                  WorkoutEvents.ToArray (), 
                                  energyBurned, 
                                  distance, 
                                  metadata);

  // Save to HealthKit
  HealthStore.SaveObject (workout, (successful, error) => {
    // Handle any errors
    if (error == null) {
      // Was the save successful
      if (successful) {

      }
    } else {
      // Report error
    }
  });

}
```

Этот код создает общий объем записанной энергии и расстояние для тренировки в качестве `HKQuantity` объектов. Будет создан словарь метаданных, определяющих создание тренировок, и указано расположение тренировки:

```csharp
metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));
```

Создается новый `HKWorkout` объект с такими же значениями `HKWorkoutActivityType` `HKWorkoutSession` , как, начальная и конечная даты, список событий (накапливается из приведенных выше разделов), число записанных в энергосбережений, общее расстояние и словарь метаданных. Этот объект сохраняется в хранилище данных о работоспособности и обрабатываются все ошибки.  

### <a name="adding-samples"></a>Добавление образцов

Когда приложение сохраняет набор примеров для тренировки, HealthKit создает соединение между образцами и самой тренировкой, чтобы приложение могла позднее запрашивать HealthKit для всех примеров, связанных с данным тренировкой. Используя эти сведения, приложение может создавать графы на основе данных тренировок и отображать их на временной шкале тренировки.

Чтобы приложение участвовало в круге перемещения приложения действия, оно должно содержать образцы энергии с сохраненной тренировкой. Кроме того, итоговые значения для расстояния и энергии должны соответствовать сумме всех выборок, которые приложение связывает с сохраненной тренировкой.

Чтобы добавить образцы в сохраненную тренировку, выполните следующие действия.

```csharp
using System.Collections.Generic;
using WatchKit;
using HealthKit;
...

public HKHealthStore HealthStore { get; private set; }
public List<HKSample> WorkoutSamples { get; set; } = new List<HKSample> ();
...

private void SaveWorkoutSamples (HKWorkout workout)
{
  // Add samples to saved workout
  HealthStore.AddSamples (WorkoutSamples.ToArray (), workout, (success, error) => {
    // Handle any errors
    if (error == null) {
      // Was the save successful
      if (success) {

      }
    } else {
      // Report error
    }
  });
}
```

При необходимости приложение может вычислить и создать меньшее подмножество образцов или один пример мегасимволов (охватывающий весь спектр тренировок), который затем будет связан с сохраненной тренировкой.

## <a name="workouts-and-ios-10"></a>Тренировки и iOS 10

Каждое приложение тренировки watchOS 3 имеет родительское приложение для работы с iOS 10, а новое в iOS 10. это приложение iOS можно использовать для запуска тренировки, который будет размещать Apple Watch в режиме тренировки (без вмешательства пользователя) и запускать приложение watchOS в фоновом режиме (см. сведения [о фоновом запуске](#about-background-running) выше).

Пока приложение watchOS работает, оно может использовать Ватчконнективити для обмена сообщениями и взаимодействия с родительским приложением iOS.

Посмотрим, как работает этот процесс:

[![Схема связи iPhone и Apple Watch](workout-apps-images/workout06.png)](workout-apps-images/workout06.png#lightbox)

1. Приложение iPhone создает `HKWorkoutConfiguration` объект и задает тип тренировки и расположение.
2. `HKWorkoutConfiguration`Объект отправляет Apple Watch версию приложения, и, если он еще не запущен, он запускается системой.
3. С помощью переданной конфигурации тренировки приложение watchOS 3 запускает новый сеанс тренировки ( `HKWorkoutSession` ).

> [!IMPORTANT]
> Чтобы родительское приложение iPhone начало тренировку на Apple Watch, приложение watchOS 3 должно работать в фоновом режиме. Дополнительные сведения см. в разделе [Включение фонового выполнения](#enabling-background-running) выше.

Этот процесс очень похож на процесс запуска сеанса тренировки в приложении watchOS 3 напрямую. На iPhone используйте следующий код:

```csharp
using System;
using HealthKit;
using WatchConnectivity;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set; } = new HKHealthStore ();
public WCSession ConnectivitySession { get; set; } = WCSession.DefaultSession;
#endregion
...

private void StartOutdoorRun ()
{
  // Can the app communicate with the watchOS version of the app?
  if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
    // Create a workout configuration
    var configuration = new HKWorkoutConfiguration () {
      ActivityType = HKWorkoutActivityType.Running,
      LocationType = HKWorkoutSessionLocationType.Outdoor
    };

    // Start watch app
    HealthStore.StartWatchApp (configuration, (success, error) => {
      // Handle any errors
      if (error == null) {
        // Was the save successful
        if (success) {
          ...
        }
      } else {
        // Report error
        ...
      }
    });
  }
}
```

Этот код гарантирует, что watchOS версия приложения установлена и в первую очередь к ней будет подключена версия iPhone:

```csharp
if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
  ...
}
```

Затем он создает `HKWorkoutConfiguration` как обычно и использует `StartWatchApp` метод объекта, `HKHealthStore` чтобы отправить его в Apple Watch и запустить приложение и сеанс тренировки.

В приложении для ОС контрольных значений используйте следующий код в `WKExtensionDelegate` :

```csharp
using WatchKit;
using HealthKit;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public OutdoorRunDelegate RunDelegate { get; set; }
#endregion
...

public override void HandleWorkoutConfiguration (HKWorkoutConfiguration workoutConfiguration)
{
  // Create workout session
  // Start workout session
  NSError error = null;
  var workoutSession = new HKWorkoutSession (workoutConfiguration, out error);

  // Successful?
  if (error != null) {
    // Report error to user and return
    return;
  }

  // Create workout session delegate and wire-up events
  RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

  RunDelegate.Failed += () => {
    // Handle the session failing
  };

  RunDelegate.Paused += () => {
    // Handle the session being paused
  };

  RunDelegate.Running += () => {
    // Handle the session running
  };

  RunDelegate.Ended += () => {
    // Handle the session ending
  };

  // Start session
  HealthStore.StartWorkoutSession (workoutSession);
}
```

Он принимает `HKWorkoutConfiguration` и создает новый объект `HKWorkoutSession` и присоединяет экземпляр пользовательского `HKWorkoutSessionDelegate` . Сеанс тренировки начинается с хранилища данных о работоспособности HealthKit пользователя.

## <a name="bringing-all-the-pieces-together"></a>Объединение всех элементов

Учитывая всю информацию, представленную в этом документе, приложение для тренировки на основе watchOS 3 и его родительское приложение для работы с iOS 10 может включать в себя следующие компоненты:

1. **iOS 10 `ViewController.cs` ** — Обрабатывает запуск сеанса подключения наблюдения и тренировки на Apple Watch.
2. **watchOS 3 `ExtensionDelegate.cs` ** — Обрабатывает версию watchOS 3 приложения тренировки.
3. **watchOS 3 `OutdoorRunDelegate.cs` ** — Пользовательский объект `HKWorkoutSessionDelegate` для управления событиями тренировки.

> [!IMPORTANT]
> Код, приведенный в следующих разделах, содержит только части, необходимые для реализации новых улучшенных функций, предоставляемых для тренировки приложений в watchOS 3. Весь вспомогательный код и код для представления и обновления пользовательского интерфейса не включаются, но его можно легко создать, следуя нашей другой документации watchOS.<p/>

### <a name="viewcontrollercs"></a>ViewController.cs

`ViewController.cs`Файл в родительской версии iOS 10 приложения тренировки будет включать в себя следующий код:

```csharp
using System;
using HealthKit;
using UIKit;
using WatchConnectivity;

namespace MonkeyWorkout
{
  public partial class ViewController : UIViewController
  {
    #region Computed Properties
    public HKHealthStore HealthStore { get; set; } = new HKHealthStore ();
    public WCSession ConnectivitySession { get; set; } = WCSession.DefaultSession;
    #endregion

    #region Constructors
    protected ViewController (IntPtr handle) : base (handle)
    {
      // Note: this .ctor should not contain any initialization logic.
    }
    #endregion

    #region Private Methods
    private void InitializeWatchConnectivity ()
    {
      // Is Watch Connectivity supported?
      if (!WCSession.IsSupported) {
        // No, abort
        return;
      }

      // Is the session already active?
      if (ConnectivitySession.ActivationState != WCSessionActivationState.Activated) {
        // No, start session
        ConnectivitySession.ActivateSession ();
      }
    }

    private void StartOutdoorRun ()
    {
      // Can the app communicate with the watchOS version of the app?
      if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
        // Create a workout configuration
        var configuration = new HKWorkoutConfiguration () {
          ActivityType = HKWorkoutActivityType.Running,
          LocationType = HKWorkoutSessionLocationType.Outdoor
        };

        // Start watch app
        HealthStore.StartWatchApp (configuration, (success, error) => {
          // Handle any errors
          if (error == null) {
            // Was the save successful
            if (success) {
              ...
            }
          } else {
            // Report error
            ...
          }
        });
      }
    }
    #endregion

    #region Override Methods
    public override void ViewDidLoad ()
    {
      base.ViewDidLoad ();

      // Start Watch Connectivity
      InitializeWatchConnectivity ();
    }
    #endregion
  }
}
```

### <a name="extensiondelegatecs"></a>ExtensionDelegate.cs

`ExtensionDelegate.cs`Файл в версии watchOS 3 приложения тренировки будет включать в себя следующий код:

```csharp
using System;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
  public class ExtensionDelegate : WKExtensionDelegate
  {
    #region Computed Properties
    public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
    public OutdoorRunDelegate RunDelegate { get; set; }
    #endregion

    #region Constructors
    public ExtensionDelegate ()
    {
      
    }
    #endregion

    #region Private Methods
    private void StartWorkoutSession (HKWorkoutConfiguration workoutConfiguration)
    {
      // Create workout session
      // Start workout session
      NSError error = null;
      var workoutSession = new HKWorkoutSession (workoutConfiguration, out error);

      // Successful?
      if (error != null) {
        // Report error to user and return
        return;
      }

      // Create workout session delegate and wire-up events
      RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

      RunDelegate.Failed += () => {
        // Handle the session failing
        ...
      };

      RunDelegate.Paused += () => {
        // Handle the session being paused
        ...
      };

      RunDelegate.Running += () => {
        // Handle the session running
        ...
      };

      RunDelegate.Ended += () => {
        // Handle the session ending
        ...
      };
      
      RunDelegate.ReachedMileGoal += (miles) => {
        // Handle the reaching a session goal
        ...
      };

      RunDelegate.HealthKitSamplesUpdated += () => {
        // Update UI as required
        ...
      };

      // Start session
      HealthStore.StartWorkoutSession (workoutSession);
    }

    private void StartOutdoorRun ()
    {
      // Create a workout configuration
      var workoutConfiguration = new HKWorkoutConfiguration () {
        ActivityType = HKWorkoutActivityType.Running,
        LocationType = HKWorkoutSessionLocationType.Outdoor
      };

      // Start the session
      StartWorkoutSession (workoutConfiguration);
    }
    #endregion

    #region Override Methods
    public override void HandleWorkoutConfiguration (HKWorkoutConfiguration workoutConfiguration)
    {
      // Start the session
      StartWorkoutSession (workoutConfiguration);
    }
    #endregion
  }
}
```

### <a name="outdoorrundelegatecs"></a>OutdoorRunDelegate.cs

`OutdoorRunDelegate.cs`Файл в версии watchOS 3 приложения тренировки будет включать в себя следующий код:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
  public class OutdoorRunDelegate : HKWorkoutSessionDelegate
  {
    #region Private Variables
    private HKAnchoredObjectQuery QueryActiveEnergyBurned;
    #endregion

    #region Computed Properties
    public HKHealthStore HealthStore { get; private set; }
    public HKWorkoutSession WorkoutSession { get; private set;}
    public float MilesRun { get; set; }
    public double ActiveEnergyBurned { get; set;}
    public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
    public List<HKSample> WorkoutSamples { get; set; } = new List<HKSample> ();
    #endregion

    #region Constructors
    public OutdoorRunDelegate (HKHealthStore healthStore, HKWorkoutSession workoutSession)
    {
      // Initialize
      this.HealthStore = healthStore;
      this.WorkoutSession = workoutSession;

      // Attach this delegate to the session
      workoutSession.Delegate = this;

    }
    #endregion

    #region Private Methods
    private void ObserveHealthKitSamples ()
    {
      // Get the starting date of the required samples
      var datePredicate = HKQuery.GetPredicateForSamples (WorkoutSession.StartDate, null, HKQueryOptions.StrictStartDate);

      // Get data from the local device
      var devices = new NSSet<HKDevice> (new HKDevice [] { HKDevice.LocalDevice });
      var devicePredicate = HKQuery.GetPredicateForObjectsFromDevices (devices);

      // Assemble compound predicate
      var queryPredicate = NSCompoundPredicate.CreateAndPredicate (new NSPredicate [] { datePredicate, devicePredicate });

      // Get ActiveEnergyBurned
      QueryActiveEnergyBurned = new HKAnchoredObjectQuery (HKQuantityType.Create (HKQuantityTypeIdentifier.ActiveEnergyBurned), queryPredicate, null, HKSampleQuery.NoLimit, (query, addedObjects, deletedObjects, newAnchor, error) => {
        // Valid?
        if (error == null) {
          // Yes, process all returned samples
          foreach (HKSample sample in addedObjects) {
            // Accumulate totals
            var quantitySample = sample as HKQuantitySample;
            ActiveEnergyBurned += quantitySample.Quantity.GetDoubleValue (HKUnit.Joule);

            // Save samples
            WorkoutSamples.Add (sample);
          }

          // Inform caller
          RaiseHealthKitSamplesUpdated ();
        }
      });

      // Start Query
      HealthStore.ExecuteQuery (QueryActiveEnergyBurned);
                                            
    }

    private void StopObservingHealthKitSamples ()
    {
      // Stop query
      HealthStore.StopQuery (QueryActiveEnergyBurned);
    }

    private void ResumeObservingHealthkitSamples ()
    {
      // Resume current queries 
      HealthStore.ExecuteQuery (QueryActiveEnergyBurned);
    }

    private void NotifyUserOfReachedMileGoal (float miles)
    {
      // Play haptic feedback
      WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);

      // Raise event
      RaiseReachedMileGoal (miles);
    }

    private void SaveWorkoutSession ()
    {
      // Build required workout quantities
      var energyBurned = HKQuantity.FromQuantity (HKUnit.Joule, ActiveEnergyBurned);
      var distance = HKQuantity.FromQuantity (HKUnit.Mile, MilesRun);

      // Create any required metadata
      var metadata = new NSMutableDictionary ();
      metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));

      // Create workout
      var workout = HKWorkout.Create (HKWorkoutActivityType.Running, 
                                      WorkoutSession.StartDate, 
                                      NSDate.Now, 
                                      WorkoutEvents.ToArray (), 
                                      energyBurned, 
                                      distance, 
                                      metadata);

      // Save to HealthKit
      HealthStore.SaveObject (workout, (successful, error) => {
        // Handle any errors
        if (error == null) {
          // Was the save successful
          if (successful) {
            // Add samples to workout
            SaveWorkoutSamples (workout);
          }
        } else {
          // Report error
          ...
        }
      });

    }

    private void SaveWorkoutSamples (HKWorkout workout)
    {
      // Add samples to saved workout
      HealthStore.AddSamples (WorkoutSamples.ToArray (), workout, (success, error) => {
        // Handle any errors
        if (error == null) {
          // Was the save successful
          if (success) {
            ...
          }
        } else {
          // Report error
          ...
        }
      });
    }
    #endregion

    #region Public Methods
    public void PauseWorkout ()
    {
      // Pause the current workout
      HealthStore.PauseWorkoutSession (WorkoutSession);
    }

    public void ResumeWorkout ()
    {
      // Pause the current workout
      HealthStore.ResumeWorkoutSession (WorkoutSession);
    }

    public void ReachedNextMile ()
    {
      // Create and save marker event
      var markerEvent = HKWorkoutEvent.Create (HKWorkoutEventType.Marker, NSDate.Now);
      WorkoutEvents.Add (markerEvent);

      // Notify user
      NotifyUserOfReachedMileGoal (++MilesRun);
    }

    public void EndOutdoorRun ()
    {
      // End the current workout session
      HealthStore.EndWorkoutSession (WorkoutSession);
    }
    #endregion

    #region Override Methods
    public override void DidFail (HKWorkoutSession workoutSession, NSError error)
    {
      // Handle workout session failing
      RaiseFailed ();
    }

    public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
    {
      // Take action based on the change in state
      switch (toState) {
      case HKWorkoutSessionState.NotStarted:
        break;
      case HKWorkoutSessionState.Paused:
        StopObservingHealthKitSamples ();
        RaisePaused ();
        break;
      case HKWorkoutSessionState.Running:
        if (fromState == HKWorkoutSessionState.Paused) {
          ResumeObservingHealthkitSamples ();
        } else {
          ObserveHealthKitSamples ();
        }
        RaiseRunning ();
        break;
      case HKWorkoutSessionState.Ended:
        StopObservingHealthKitSamples ();
        SaveWorkoutSession ();
        RaiseEnded ();
        break;
      }

    }

    public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
    {
      base.DidGenerateEvent (workoutSession, @event);

      // Save HealthKit generated event
      WorkoutEvents.Add (@event);

      // Take action based on the type of event
      switch (@event.Type) {
      case HKWorkoutEventType.Lap:
        ...
        break;
      case HKWorkoutEventType.Marker:
        ...
        break;
      case HKWorkoutEventType.MotionPaused:
        ...
        break;
      case HKWorkoutEventType.MotionResumed:
        ...
        break;
      case HKWorkoutEventType.Pause:
        ...
        break;
      case HKWorkoutEventType.Resume:
        ...
        break;
      }
    }
    #endregion

    #region Events
    public delegate void OutdoorRunEventDelegate ();
    public delegate void OutdoorRunMileGoalDelegate (float miles);

    public event OutdoorRunEventDelegate Failed;
    internal void RaiseFailed ()
    {
      if (this.Failed != null) this.Failed ();
    }

    public event OutdoorRunEventDelegate Paused;
    internal void RaisePaused ()
    {
      if (this.Paused != null) this.Paused ();
    }

    public event OutdoorRunEventDelegate Running;
    internal void RaiseRunning ()
    {
      if (this.Running != null) this.Running ();
    }

    public event OutdoorRunEventDelegate Ended;
    internal void RaiseEnded ()
    {
      if (this.Ended != null) this.Ended ();
    }

    public event OutdoorRunMileGoalDelegate ReachedMileGoal;
    internal void RaiseReachedMileGoal (float miles)
    {
      if (this.ReachedMileGoal != null) this.ReachedMileGoal (miles);
    }

    public event OutdoorRunEventDelegate HealthKitSamplesUpdated;
    internal void RaiseHealthKitSamplesUpdated ()
    {
      if (this.HealthKitSamplesUpdated != null) this.HealthKitSamplesUpdated ();
    }
    #endregion
  }
}
```

## <a name="best-practices"></a>Рекомендации

Компания Apple предлагает использовать следующие рекомендации при проектировании и реализации тренировок в watchOS 3 и iOS 10:

- Убедитесь, что приложение для тренировки watchOS 3 по-прежнему работает даже в том случае, если не удается подключиться к iPhone и версии iOS 10 приложения.
- Используйте HealthKitное расстояние, когда GPS недоступен, так как он способен создавать образцы расстояний без GPS.
- Разрешить пользователю начать тренировку с Apple Watch или iPhone.
- Позволяет приложению отображать тренировки из других источников (например, других сторонних приложений) в своих исторических представлениях данных.
- Убедитесь, что приложение не отображает удаленные тренировки в исторических данных.

## <a name="summary"></a>Сводка

В этой статье были рассмотрены улучшения, внесенные компанией Apple для тренировок приложений в watchOS 3, и способы их реализации в Xamarin.

## <a name="related-links"></a>Связанные ссылки

- [Примеры iOS 10](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [Введение в HealthKit](~/ios/platform/healthkit.md)
