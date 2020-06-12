---
title: Диспетчер заданий Firebase
description: В этом руководстве описано, как планировать фоновую работу с помощью библиотеки диспетчера заданий Firebase от Google.
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/05/2018
ms.openlocfilehash: 0ade609997e391e24d4a6da250172efa81a5d490
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571341"
---
# <a name="firebase-job-dispatcher"></a>Диспетчер заданий Firebase

_В этом руководстве описано, как планировать фоновую работу с помощью библиотеки диспетчера заданий Firebase от Google._

## <a name="overview"></a>Обзор

Один из лучших способов поддерживать хорошую отзывчивость приложения Android — выполнять сложную или длительную работу в фоновом режиме. При этом важно, чтобы фоновая работа не влияла негативно на взаимодействие пользователя с устройством. 

Например, фоновое задание может опрашивать веб-сайт каждые три или четыре минуты, чтобы получать изменения в определенном наборе данных. Это кажется пустяком, но это может повлиять на время работы аккумулятора. Приложение будет раз за разом выводить устройство из спящего режима, повышать уровень энергопотребления ЦП, включать беспроводную связь, выполнять сетевые запросы и обрабатывать результаты. Все усложняется тем, что устройство не будет немедленно выключаться и возвращаться в режим ожидания с низким энергопотреблением. Плохо спланированная фоновая работа может привести к тому, что устройство самопроизвольно и без необходимости будет переходить в режим повышенного энергопотребления. На первый взгляд безобидное действие (опрос веб-сайта) довольно быстро сделает устройство непригодным для использования.

Android предоставляет приведенные ниже API для выполнения работы в фоновом режиме, но их недостаточно для интеллектуального планирования заданий. 

- **[Службы намерения](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** . Службы намерения отлично подходят для выполнения работы, но они не позволяют планировать ее.
- **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)** . Эти API помогают только планировать работу по расписанию, но не позволяют фактически выполнять работу. Кроме того, AlarmManager поддерживает только ограничения на основе времени, то есть создает оповещение в определенное время или по истечении определенного времени. 
- **[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)** . JobSchedule — это отличный API, который работает с операционной системой для планирования заданий. Однако он доступен только для тех приложений Android, которые нацелены на уровень API 21 или более поздней версии. 
- **[Широковещательные приемники](~/android/app-fundamentals/broadcast-receivers.md)** . Приложение Android может настроить широковещательные приемники для выполнения работы в ответ на события или намерения уровня системы. Однако широковещательные приемники не позволяют управлять временем выполнения заданий. Кроме того, изменения в операционной системе Android ограничивают возможности выполнения широковещательных приемников, а также типы работы, на которую они могут отвечать. 

Существуют два ключевых аспекта эффективного выполнения фоновой работы (иногда называемой _фоновым заданием_ или просто _заданием_):

1. **Рациональное планирование работы.** Если приложение выполняет работу в фоновом режиме, оно должно делать это рационально. В идеале приложение не должно требовать выполнения задания. Вместо этого оно должно указать условия, при которых задание может быть выполнено, а затем запланировать выполнение работы при выполнении этих условий. Это позволяет Android выполнять работу интеллектуально. Например, сетевые запросы могут быть пакетными и выполняться одновременно, чтобы оптимизировать дополнительные временные затраты, связанные с сетью.
2. **Инкапсуляция работы**. Код для выполнения фоновой работы должен быть инкапсулирован в отдельный компонент, который может работать независимо от пользовательского интерфейса и который будет сравнительно просто повторно запланировать, если по какой-либо причине работу не удастся выполнить.

Диспетчер заданий Firebase — это библиотека от Google, которая предоставляет текучий API для упрощения планирования фоновой работы. Она должна заменить Google Cloud Manager. Диспетчер заданий Firebase содержит следующие интерфейсы API.

- `Firebase.JobDispatcher.JobService` — это абстрактный класс, который должен быть расширен с помощью логики, которая будет выполняться в фоновом задании.
- `Firebase.JobDispatcher.JobTrigger` объявляет, когда задание должно быть запущено. Обычно это период времени, например, ожидание не менее 30 секунд перед запуском задания, но при этом задание должно быть запущено в течение 5 минут.
- `Firebase.JobDispatcher.RetryStrategy` содержит сведения о том, что следует делать, если задание не выполняется должным образом. Стратегия повтора указывает время ожидания перед повторной попыткой выполнения задания. 
- `Firebase.JobDispatcher.Constraint` — это необязательное значение, описывающее условие, которое должно быть выполнено для запуска задания. Например, устройство не должно находиться в сети с оплатой трафика или заряжаться.
- `Firebase.JobDispatcher.Job` — это API, который объединяет предыдущие API в единицу работы, которую может запланировать `JobDispatcher`. Класс `Job.Builder` используется для создания экземпляра `Job`.
- `Firebase.JobDispatcher.JobDispatcher` использует три описанных выше API, чтобы планировать работу с помощью операционной системой и предоставить способ отмены заданий, если это потребуется.

Чтобы запланировать работу с помощью диспетчера заданий Firebase, приложение Xamarin.Android должно инкапсулировать код в типе, который расширяет класс `JobService`. `JobService` содержит три метода жизненного цикла, которые могут быть вызваны в течение времени существования задания.

- **`bool OnStartJob(IJobParameters parameters)`** . Этот метод позволяет выполнять работу и всегда должен быть реализован. Он выполняется в основном потоке. Этот метод возвращает `true`, если работа еще не выполнена, и `false`, если она завершена. 
- **`bool OnStopJob(IJobParameters parameters)`** . Этот метод вызывается, когда выполнение задания по какой-либо причине останавливается. Он должен возвращать `true`, если задание следует повторно запланировать.
- **`JobFinished(IJobParameters parameters, bool needsReschedule)`** . Этот метод вызывается, когда `JobService` завершает какую-либо асинхронную работу. 

Чтобы запланировать задание, приложение создаст экземпляр объекта `JobDispatcher`. Затем `Job.Builder` используется для создания объекта `Job`. Этот объект предоставляется `JobDispatcher`, который попытается запланировать выполнение задания.

В этом руководство описано, как добавить диспетчер заданий Firebase в приложение Xamarin.Android и использовать его для планирования фоновой работы.

## <a name="requirements"></a>Требования

Диспетчеру заданий Firebase требуется уровень API Android 9 или более поздней версии. Библиотека диспетчера заданий Firebase использует некоторые компоненты, предоставляемые Сервисами Google Play, поэтому устройстве должны быть установлены Сервисы Google Play.

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Использование библиотеки диспетчера заданий Firebase в Xamarin.Android

Чтобы приступить к работе с диспетчером заданий Firebase, сначала добавьте [пакет NuGet Xamarin.Firebase.JobDispatcher](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher) в проект Xamarin.Android. Найдите в диспетчере пакетов NuGet пакет **Xamarin.Firebase.JobDispatcher** (который пока находится на этапе предварительной версии).

После добавления библиотеки диспетчера заданий Firebase создайте класс `JobService` и запланируйте его запуск с помощью экземпляра `FirebaseJobDispatcher`.

### <a name="creating-a-jobservice"></a>Создание JobService

Вся работа, выполняемая библиотекой диспетчера заданий Firebase, должна быть выполнена в типе, расширяющем абстрактный класс `Firebase.JobDispatcher.JobService`. Создание `JobService` очень похоже на создание `Service` с помощью платформы Android. 

1. Расширьте класс `JobService`.
2. Декорируйте подкласс с помощью `ServiceAttribute`. Хотя это и не обязательно, рекомендуется явно задать параметр `Name`, чтобы упростить отладку `JobService`. 
3. Добавьте `IntentFilter`, чтобы объявить `JobService` в **AndroidManifest.xml**. Это также поможет библиотеке диспетчера заданий Firebase найти и вызвать `JobService`.

Следующий код является примером простейшего `JobService` для приложения, использующего TPL для асинхронного выполнения некоторой работы.

```csharp
[Service(Name = "com.xamarin.fjdtestapp.DemoJob")]
[IntentFilter(new[] {FirebaseJobServiceIntent.Action})]
public class DemoJob : JobService
{
    static readonly string TAG = "X:DemoService";

    public override bool OnStartJob(IJobParameters jobParameters)
    {
        Task.Run(() =>
        {
            // Work is happening asynchronously (code omitted)
                       
        });

        // Return true because of the asynchronous work
        return true;  
    }

    public override bool OnStopJob(IJobParameters jobParameters)
    {
        Log.Debug(TAG, "DemoJob::OnStartJob");
        // nothing to do.
        return false;
    }
}
```

### <a name="creating-a-firebasejobdispatcher"></a>Создание FirebaseJobDispatcher

Прежде чем можно будет запланировать работу, необходимо создать объект `Firebase.JobDispatcher.FirebaseJobDispatcher`. `FirebaseJobDispatcher` отвечает за планирование `JobService`. В следующем фрагменте кода показан один из способов создания экземпляра `FirebaseJobDispatcher`. 

 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

В предыдущем фрагменте кода `GooglePlayDriver` — это класс, помогающий `FirebaseJobDispatcher` взаимодействовать с некоторыми API планирования в Сервисах Google Play на устройстве. Параметр `context` — это любой `Context` Android, например действие. Сейчас `GooglePlayDriver` является единственной реализацией `IDriver` в библиотеке диспетчера заданий Firebase. 

Привязка Xamarin.Android для диспетчера заданий Firebase обеспечивает метод расширения для создания `FirebaseJobDispatcher` из `Context`. 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

После создания экземпляра `FirebaseJobDispatcher` можно создать `Job` и выполнить код в классе `JobService`. `Job` создается объектом `Job.Builder` и будет рассмотрен в следующем разделе.

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Создание Firebase.JobDispatcher.Job с помощью Job.Builder

Класс `Firebase.JobDispatcher.Job` отвечает за инкапсуляцию метаданных, необходимых для запуска `JobService`. `Job` содержит такие сведения, как любые ограничения, которые должны быть соблюдены для выполнения задания, если `Job` повторяется или для выполнения задания используются триггеры.  Как минимум, `Job` должен содержать _тег_ (уникальная строка, указывающая задание для `FirebaseJobDispatcher`) и тип выполняемого объекта `JobService`. Диспетчер заданий Firebase создаст экземпляр `JobService`, когда наступит время запуска задания.  `Job` создается с помощью экземпляра класса `Firebase.JobDispatcher.Job.JobBuilder`. 

Следующий фрагмент кода является простым примером создания `Job` с помощью привязки Xamarin.Android.

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

`Job.Builder` выполнит несколько простых проверок входных значений для задания. Если `Job.Builder` не удастся создать `Job`, будет порождено исключение.  `Job.Builder` создаст `Job` со следующими параметрами по умолчанию.

- _Время существования_ `Job` (запланированное время выполнения) будет длиться только до тех пор, пока устройство не будет перезагружено &ndash; после перезагрузки устройства `Job` утрачивается.
- `Job` не повторяется, а будет выполняться только один раз.
- Запуск `Job` будет запланирован как можно скорее.
- Стратегия повтора по умолчанию для `Job` заключается в использовании _экспоненциальной задержки_ (см. дополнительные сведения ниже в разделе [Настройка RetryStrategy](#Setting_a_RetryStrategy)).

### <a name="scheduling-a-job"></a>Планирование задания

После создания `Job` его необходимо запланировать с помощью `FirebaseJobDispatcher`, прежде чем выполнять. Запланировать `Job` можно двумя способами.

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

Значение, возвращаемое `FirebaseJobDispatcher.Schedule`, будет одним из следующих целочисленных значений:

- `FirebaseJobDispatcher.ScheduleResultSuccess` &ndash; планирование `Job` успешно выполнено;
- `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; возникла неизвестная проблема, воспрепятствовавшая планированию `Job`;
- `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; использован недопустимый `IDriver` или `IDriver` был недоступен; 
- `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; `Trigger` не поддерживался;
- `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; служба настроена неправильно или недоступна.

### <a name="configuring-a-job"></a>Настройка задания

Задание можно настроить. Ниже приведены примеры того, как можно настроить задание.

- [Передача параметров в задание](#Passing_Parameters_to_a_Job). Для выполнения `Job` могут потребоваться дополнительные значения (например, для скачивания файла).
- [Установка ограничений](#Setting_Constraints). Может потребоваться запускать задание только при соблюдении определенных условий. Например, может потребоваться запускать `Job` только при зарядке устройства. 
- [Указание времени выполнения `Job`](#Setting_Job_Triggers). Диспетчер заданий Firebase позволяет приложениям указывать время выполнения задания.  
- [Объявление стратегии повтора для заданий с ошибками](#Setting_a_RetryStrategy). _Стратегия повтора_ предоставляет `FirebaseJobDispatcher` указания о том, что делать с заданием `Jobs`, которое не удалось завершить. 

В следующих разделах каждая из этих тем рассматривается более подробно.

<a name="Passing_Parameters_to_a_Job"></a>

#### <a name="passing-parameters-to-a-job"></a>Передача параметров в задание

Параметры передаются в задание путем создания `Bundle`, который передается вместе с методом `Job.Builder.SetExtras`.

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

Доступ к `Bundle` осуществляется из свойства `IJobParameters.Extras` метода `OnStartJob`.

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints"></a>

#### <a name="setting-constraints"></a>Установка ограничений

Ограничения могут снизить расходы или истощение аккумулятора на устройстве. Класс `Firebase.JobDispatcher.Constraint` определяет эти ограничения как целочисленные значения.

- `Constraint.OnUnmeteredNetwork` &ndash; задание запускается только в том случае, если устройство подключено к сети без оплаты трафика. Это удобно, чтобы пользователи не переплачивали за передачу данных.
- `Constraint.OnAnyNetwork` &ndash; задание запускается в любой сети, к которой подключено устройство. Если указано вместе с `Constraint.OnUnmeteredNetwork`, то это значение будет иметь приоритет.
- `Constraint.DeviceCharging` &ndash; задание запускается только при зарядке устройства.

Ограничения задаются с помощью метода `Job.Builder.SetConstraint`. 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers"></a>

`JobTrigger` предоставляет инструкции операционной системе о том, когда должно начаться выполнение задания. У `JobTrigger` есть _период выполнения_, который определяет запланированное время выполнения `Job`. У периода выполнения имеются значения _периода начала_ и _периода конца_. Период начала — это количество секунд, которое устройство должно ожидать перед выполнением задания, а период конца — это максимальное число секунд ожидания перед запуском `Job`. 

`JobTrigger` можно создать с помощью метода `Firebase.Jobdispatcher.Trigger.ExecutionWindow`.  Например, `Trigger.ExecutionWindow(15,60)` означает, что задание должно быть запущено через 15–60 секунд после времени, на которое оно запланировано. Используется метод `Job.Builder.SetTrigger`. 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

`JobTrigger` по умолчанию для задания представлен значением `Trigger.Now`, которое указывает, что задание должно быть запущено как можно быстрее после того, как оно запланировано.

<a name="Setting_a_RetryStrategy"></a>

#### <a name="setting-a-retrystrategy"></a>Настройка RetryStrategy

`Firebase.JobDispatcher.RetryStrategy` используется для указания того, какую задержку должно использовать устройство, прежде чем пытаться повторно запустить невыполненное задание. У `RetryStrategy` имеется _политика_, которая определяет временной алгоритм, который будет использоваться для повторного планирования невыполненного задания, и период выполнения, на который это задание должно быть запланировано. Данные _период повторного планирования_ определяется двумя значениями. Первое значение — это время ожидания в секундах перед повторным планированием задания (значение _начальной задержки_). Второе число — это максимальное время в секундах, по истечении которого задание должно быть запущено (значение _максимальной задержки_). 

Два типа политик повтора определяют приведенные ниже целочисленные значения.

- `RetryStrategy.RetryPolicyExponential`. Политика _экспоненциальной задержки_ экспоненциально повышает значение начальной задержки после каждого сбоя. При первом сбое задания библиотека будет ждать в течение периода начальной задержки, заданного до повторного планирования задания &ndash; (например, в течение 30 секунд). При втором сбое задания перед попыткой его запуска библиотека будет ожидать не менее 60 секунд. После третьей неудачной попытки библиотека будет ждать 120 секунд и т. д. `RetryStrategy` по умолчанию для библиотеки диспетчера заданий Firebase представляет объект `RetryStrategy.DefaultExponential`. Он использует начальную задержку в 30 секунд и максимальную задержку в 3600 секунд.
- `RetryStrategy.RetryPolicyLinear`. Эта стратегия использует _линейную задержку_, то есть выполнение задания должно повторно планироваться через заданные интервалы (до тех пор, пока оно не будет выполнено). Линейная задержка лучше всего подходит для работы, которая должна быть выполнена как можно скорее, или при наличии проблем, которые быстро исчезают сами по себе. Библиотека диспетчера заданий Firebase определяет `RetryStrategy.DefaultLinear` с периодом повторного планирования от 30 до 3600 секунд.

Можно определить пользовательскую стратегию `RetryStrategy` с помощью метода `FirebaseJobDispatcher.NewRetryStrategy`. Он принимает три параметра.

1. `int policy` &ndash; _политика_ определяется одним из предыдущих `RetryStrategy` значений, `RetryStrategy.RetryPolicyLinear` или `RetryStrategy.RetryPolicyExponential`.
2. `int initialBackoffSeconds` &ndash; задается _начальная задержка_ в секундах, которая действует перед повторной попыткой выполнения задания. По умолчанию это значение равно 30 секундам. 
3. `int maximumBackoffSeconds` &ndash; значение _максимальной задержки_ задает максимальное время задержки (в секундах) перед повторной попыткой выполнения задания. Значение по умолчанию — 3600 секунд. 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>Отмена задания

Можно отменить все запланированные задания или только одно задание, используя метод `FirebaseJobDispatcher.CancelAll()` или метод `FirebaseJobDispatcher.Cancel(string)`.

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

Любой из этих методов возвращает целочисленное значение.

- `FirebaseJobDispatcher.CancelResultSuccess` &ndash; задание успешно отменено.
- `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; отменить задание не удалось из-за ошибки.
- `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; `FirebaseJobDispatcher` не удалось отменить задание, так как нет допустимых `IDriver`.

## <a name="summary"></a>Сводка

В этом руководство описано, как использовать диспетчер заданий Firebase для интеллектуального выполнения работы в фоновом режиме. Мы изучили, как инкапсулировать работу, которую необходимо выполнить в качестве `JobService`, и как использовать `FirebaseJobDispatcher`, чтобы запланировать эту работу, указав критерии с помощью `JobTrigger` и способ обработки ошибок с помощью `RetryStrategy`.

## <a name="related-links"></a>Связанные ссылки

- [Xamarin.Firebase.JobDispatcher в NuGet](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [firebase-job-dispatcher на портале GitHub](https://github.com/firebase/firebase-jobdispatcher-android)
- [Привязка Xamarin.Firebase.JobDispatcher](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [Интеллектуальное планирование заданий](https://developer.android.com/topic/performance/scheduling.html)
- [Google I/O 2016: оптимизация потребления аккумулятора и памяти для Android (видео)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
