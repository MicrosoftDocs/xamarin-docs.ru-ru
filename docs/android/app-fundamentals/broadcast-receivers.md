---
title: Широковещательные приемники в Xamarin. Android
description: В этом разделе описывается использование широковещательного приемника.
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/20/2018
ms.openlocfilehash: 9e702bdb31563588d20b9cd70e35b67aefe67e0c
ms.sourcegitcommit: d1980b2251999224e71c1289e4b4097595b7e261
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2020
ms.locfileid: "92928377"
---
# <a name="broadcast-receivers-in-xamarinandroid"></a>Широковещательные приемники в Xamarin. Android

_В этом разделе описывается использование широковещательного приемника._

## <a name="broadcast-receiver-overview"></a>Обзор широковещательного приемника

_Широковещательный приемник_ — это компонент Android, позволяющий приложению реагировать на сообщения (Android [`Intent`](xref:Android.Content.Intent) ), которые рассылаются операционной системой Android или приложением. Широковещательные рассылки следуют за тем, что в рамках события « _публикация-подписка_ » &ndash; событие вызывает публикацию и получение широковещательных данных компонентами, заинтересованными в событии.

Android определяет два типа широковещательных рассылок:

- **Явная трансляция** &ndash; Эти типы вещания предназначены для конкретного приложения. Наиболее распространенным применением явного вещания является запуск действия. Пример явного вещания, когда приложению необходимо набрать номер телефона; она отправит цель, предназначенную для телефонного приложения на Android, и передаст номер телефона для набора. Затем Android перенаправит цель в приложение для телефона.
- **Неявная трансляция** &ndash; Эти широковещательные рассылки отправлены всем приложениям на устройстве. Примером неявной трансляции является `ACTION_POWER_CONNECTED` намерение. Эта цель публикуется каждый раз, когда Android обнаруживает, что батарея устройства заряжается. Android направит эту цель всем приложениям, зарегистрированным для этого события.

Широковещательный приемник является подклассом `BroadcastReceiver` типа и должен переопределять [`OnReceive`](xref:Android.Content.BroadcastReceiver.OnReceive*) метод. Android будет выполняться `OnReceive` в основном потоке, поэтому этот метод должен быть спроектирован для быстрого выполнения. Следует соблюдать осторожность при создании потоков в `OnReceive` , так как Android может завершить процесс после завершения метода. Если широковещательный получатель должен выполнить длительную работу, рекомендуется запланировать _Задание_ с помощью `JobScheduler` _диспетчера заданий или Firebase_ . Планирование работы с заданием будет обсуждаться в отдельном руководством.

_Фильтр с намерением_ используется для регистрации широковещательного приемника, чтобы Android мог правильно маршрутизировать сообщения. Фильтр намерения можно указать во время выполнения (иногда это называется _приемником контекста_ или _динамической регистрацией_ ) или его можно статически определить в манифесте Android ( _получатель, зарегистрированный манифестом_ ). Xamarin. Android предоставляет атрибут C#, `IntentFilterAttribute` который будет статически регистрировать фильтр намерения (это будет обсуждаться более подробно далее в этом разделе). Начиная с Android 8,0, приложение не может статически регистрироваться для неявной трансляции.

Основное различие между зарегистрированным манифестом и приемником, зарегистрированным в контексте, заключается в том, что зарегистрированный в контексте получатель будет отвечать только за широковещательные передачи во время выполнения приложения, а получатель, зарегистрированный в манифесте, может отвечать на широковещательные передачи, даже если приложение может не работать.  

Существует два набора API-интерфейсов для управления широковещательным приемником и отправки широковещательных пакетов:

1. **`Context`**&ndash; `Android.Content.Context` Класс можно использовать для регистрации широковещательного приемника, который будет отвечать на события на уровне системы. `Context`Также используется для публикации широковещательных рассылок на уровне системы.
2. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))**&ndash;Это API, доступный в [пакете NuGet библиотеки поддержки Xamarin версии 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Этот класс используется для синхронизации широковещательных и широковещательных приемников в контексте приложения, использующего их. Этот класс может быть полезен, чтобы другие приложения не отвечали на широковещательные рассылки только приложений или отправляли сообщения частным получателям.

Широковещательный приемник может не отображать диалоговые окна, поэтому настоятельно не рекомендуется запускать действие в широковещательном приемнике. Если широковещательный получатель должен уведомить пользователя, он должен опубликовать уведомление.

Невозможно выполнить привязку или запустить службу в широковещательном приемнике.

В этом руководство рассматривается создание широковещательного приемника и его регистрация для получения широковещательных рассылок.

## <a name="creating-a-broadcast-receiver"></a>Создание широковещательного приемника

Для создания широковещательного приемника в Xamarin. Android приложение должно создать подкласс `BroadcastReceiver` класса, сделать его графическим `BroadcastReceiverAttribute` и переопределить `OnReceive` метод:

```csharp
[BroadcastReceiver(Enabled = true, Exported = false)]
public class SampleReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here.

        String value = intent.GetStringExtra("key");
    }
}
```

Когда Xamarin. Android компилирует класс, он также обновляет AndroidManifest с помощью метаданных, необходимых для регистрации получателя. Для статически зарегистрированного широковещательного приемника `Enabled` должно быть задано значение, в `true` противном случае Android не сможет создать экземпляр получателя.

`Exported`Свойство определяет, может ли получатель вещания принимать сообщения извне приложения. Если свойство не задано явно, значение по умолчанию для свойства определяется Android на основе наличия фильтров намерения, связанных с широковещательным приемником. Если существует хотя бы один фильтр намерения для широковещательного приемника, Android предполагает, что `Exported` свойство имеет значение `true` . При отсутствии фильтров с намерением, связанных с широковещательным приемником, Android считает, что значение равно `false` .

`OnReceive`Метод получает ссылку на объект, отправленный в `Intent` широковещательный приемник. Это дает возможность отправителю цели передавать значения получателю вещания.

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>Статическая регистрация широковещательного приемника с фильтром намерения

Если a `BroadcastReceiver` снабжен [`IntentFilterAttribute`](xref:Android.App.IntentFilterAttribute) , Xamarin. Android добавит необходимый `<intent-filter>` элемент в манифест Android во время компиляции. В следующем фрагменте кода приведен пример широковещательного приемника, который будет выполняться после завершения загрузки устройства (если пользователь предоставил соответствующие разрешения Android):

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { Android.Content.Intent.ActionBootCompleted })]
public class MyBootReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Work that should be done when the device boots.     
    }
}
```

> [!NOTE]
> В Android 8,0 (API 26 и более поздней версии) [компания Google накладывает ограничения](https://developer.android.com/about/versions/oreo/background) на то, какие приложения могут делать, пока пользователи не взаимодействуют с ними напрямую. Эти ограничения относятся к фоновым службам и неявным получателям широковещательной рассылки, таким как `Android.Content.Intent.ActionBootCompleted` . Из-за этих ограничений могут возникнуть проблемы с регистрацией `Boot Completed` широковещательного приемника в более новых версиях Android. Если это так, обратите внимание, что эти ограничения не применяются к службам переднего плана, которые могут быть вызваны из широковещательного приемника.

Также можно создать фильтр с намерением, который будет отвечать на пользовательские цели. Рассмотрим следующий пример.

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { "com.xamarin.example.TEST" })]
public class MySampleBroadcastReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here
    }
}
```

Приложения, предназначенные для Android 8,0 (уровень API 26) или более поздней версии, не могут статически регистрироваться для неявного вещания. Приложения по-прежнему могут статически регистрироваться для явного вещания. Существует небольшой список неявных широковещательных рассылок, которые исключены из этого ограничения. Эти исключения описаны в руководстве по [исключениям неявных рассылок](https://developer.android.com/guide/components/broadcast-exceptions.html) в документации по Android. Приложения, заинтересованные в неявных широковещательных рассылках, должны делать это динамически с помощью `RegisterReceiver` метода. Это описано далее.

### <a name="context-registering-a-broadcast-receiver"></a>Context-Registering широковещательного приемника

Регистрация контекста (также называемая динамической регистрацией) получателя выполняется путем вызова `RegisterReceiver` метода, и широковещательный приемник должен быть отменен с помощью вызова `UnregisterReceiver` метода. Чтобы предотвратить утечку ресурсов, важно отменить регистрацию получателя, если он больше не подходит для контекста (действия или службы). Например, служба может транслировать цель для информирования о том, что обновления доступны для отображения пользователю. При запуске действия он регистрируется для этих целей. Когда действие перемещается в фон и больше не отображается для пользователя, оно должно отменить регистрацию получателя, так как пользовательский интерфейс для отображения обновлений больше не отображается. В следующем фрагменте кода приведен пример регистрации и отмены регистрации широковещательного приемника в контексте действия.

```csharp
[Activity(Label = "MainActivity", MainLauncher = true, Icon = "@mipmap/icon")]
public class MainActivity: Activity
{
    MySampleBroadcastReceiver receiver;

    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        receiver = new MySampleBroadcastReceiver();

        // Code omitted for clarity
    }

    protected override void OnResume()
    {
        base.OnResume();
        RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
        // Code omitted for clarity
    }

    protected override void OnPause()
    {
        UnregisterReceiver(receiver);
        // Code omitted for clarity
        base.OnPause();
    }
}
```

В предыдущем примере, когда действие поступает в передний план, оно регистрирует широковещательного приемника, который будет прослушивать пользовательское намерение с помощью `OnResume` метода Lifecycle. Когда действие перемещается в фоновый режим, метод отменяет `OnPause()` регистрацию получателя.

## <a name="publishing-a-broadcast"></a>Публикация вещания

Вещание можно опубликовать во всех приложениях, установленных на устройстве, создавая объект намерения и отправляют его с помощью `SendBroadcast` `SendOrderedBroadcast` метода или.  

1. Методы Context. **сендброадкаст** &ndash; Существует несколько реализаций этого метода.
   Эти методы будут рассылать цель во всю систему. Широковещательные приемники, которые будут принимать намерение в неопределенном порядке. Это обеспечивает большую гибкость, но означает, что другие приложения могут зарегистрироваться и получить цель. Это может представлять потенциальную угрозу безопасности. Приложениям может потребоваться реализовать дополнительные средства безопасности, чтобы предотвратить несанкционированный доступ. Одним из возможных решений является использование, `LocalBroadcastManager` которое позволяет отправлять сообщения только в пределах частного пространства приложения. Этот фрагмент кода является одним из примеров того, как можно отправить намерение с помощью одного из `SendBroadcast` методов:

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   message.PutExtra("key", "value");
   SendBroadcast(message);
   ```

    Этот фрагмент кода является еще одним примером отправки вещания с помощью  `Intent.SetAction` метода для указания действия:

    ```csharp
    Intent intent = new Intent();
    intent.SetAction("com.xamarin.example.TEST");
    intent.PutExtra("key", "value");
    SendBroadcast(intent);
    ```

2. **Context. сендордередброадкаст.** &ndash; этот метод очень похож на `Context.SendBroadcast` , и разница заключается в том, что цель будет опубликована по одному получателю в порядке, в котором были зарегистрированы получатели.

### <a name="localbroadcastmanager"></a>локалброадкастманажер

[Библиотека поддержки Xamarin версии 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) предоставляет вспомогательный класс с именем [`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html) . `LocalBroadcastManager`Предназначен для приложений, которые не хотят отправлять или получать широковещательные сообщения от других приложений на устройстве. `LocalBroadcastManager`Будет публиковать сообщения только в контексте приложения и только в тех широковещательных приемниках, которые зарегистрированы в `LocalBroadcastManager` . Этот фрагмент кода является примером регистрации широковещательного приемника с помощью `LocalBroadcastManager` :

```csharp
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this). RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
```

Другие приложения на устройстве не могут получать сообщения, опубликованные с помощью `LocalBroadcastManager` . В этом фрагменте кода показано, как отправить намерение с помощью `LocalBroadcastManager` :

```csharp
Intent message = new Intent("com.xamarin.example.TEST");
// If desired, pass some values to the broadcast receiver.
message.PutExtra("key", "value");
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
```

## <a name="related-links"></a>Связанные ссылки

- [API BroadcastReceiver](xref:Android.Content.BroadcastReceiver)
- [API context. Регистеррецеивер](xref:Android.Content.Context.RegisterReceiver*)
- [API context. Сендброадкаст](xref:Android.Content.Context.SendBroadcast*)
- [API context. Унрегистеррецеивер](xref:Android.Content.Context.UnregisterReceiver*)
- [API намерения](xref:Android.Content.Intent)
- [API Интентфилтер](xref:Android.App.IntentFilterAttribute)
- [Локалброадкастманажер (документы Android)](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))
- [Локальные уведомления в программе Android](~/android/app-fundamentals/notifications/local-notifications.md)
- [Библиотека поддержки Android v4 (NuGet)](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
