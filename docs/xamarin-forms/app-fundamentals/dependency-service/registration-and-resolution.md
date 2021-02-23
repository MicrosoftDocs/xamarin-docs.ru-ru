---
title: Регистрация и разрешение класса Xamarin.Forms DependencyService
description: В этой статье объясняется, как зарегистрировать реализации платформы с помощью класса DependencyService Xamarin.Forms для вызова собственных функций платформы.
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/05/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 632d2256ec30602aa9d5ed4dee9e781864b2f462
ms.sourcegitcommit: e7a5d1ec9e50a09b3b24f4c57850a4763c3406d2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/18/2021
ms.locfileid: "101087458"
---
# <a name="xamarinforms-dependencyservice-registration-and-resolution"></a>Регистрация и разрешение класса Xamarin.Forms DependencyService

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/dependencyservice/)

При использовании Xamarin.Forms [`DependencyService`](xref:Xamarin.Forms.DependencyService) для вызова собственных функций платформы реализации платформы нужно зарегистрировать в `DependencyService`, после чего разрешить их из общего кода для вызова.

## <a name="register-platform-implementations"></a>Регистрация реализаций платформы

Реализации платформы должны быть зарегистрированы в [`DependencyService`](xref:Xamarin.Forms.DependencyService), чтобы платформа Xamarin.Forms могла их обнаруживать во время выполнения.

Регистрацию можно выполнить с помощью [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) или методов [`Register`](xref:Xamarin.Forms.DependencyService.Register*) и `RegisterSingleton`.

> [!IMPORTANT]
> Сборки выпуска проектов универсальной платформы Windows, использующие компиляцию в собственный код .NET, должны регистрировать реализации платформы с помощью методов [`Register`](xref:Xamarin.Forms.DependencyService.Register*).

### <a name="registration-by-attribute"></a>Регистрация с помощью атрибута

Можно использовать [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute), чтобы зарегистрировать реализацию платформы в [`DependencyService`](xref:Xamarin.Forms.DependencyService). Атрибут указывает, что заданный тип предоставляет конкретную реализацию интерфейса.

В следующем примере используется [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) для регистрации реализации интерфейса `IDeviceOrientationService` для iOS.

```csharp
using Xamarin.Forms;

[assembly: Dependency(typeof(DeviceOrientationService))]
namespace DependencyServiceDemos.iOS
{
    public class DeviceOrientationService : IDeviceOrientationService
    {
        public DeviceOrientation GetOrientation()
        {
            ...
        }
    }
}
```

В этом примере [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) регистрирует `DeviceOrientationService` в [`DependencyService`](xref:Xamarin.Forms.DependencyService). Это приводит к регистрации конкретного типа с интерфейсом, который он реализует.

Аналогичным образом реализации интерфейса `IDeviceOrientationService` на других платформах должны быть зарегистрированы с помощью [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute).

> [!NOTE]
> Регистрация с помощью [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) выполняется на уровне пространства имен.

### <a name="registration-by-method"></a>Регистрация с помощью метода

Вы можете использовать методы [`DependencyService.Register`](xref:Xamarin.Forms.DependencyService.Register*) и `RegisterSingleton`, чтобы зарегистрировать реализацию платформы в [`DependencyService`](xref:Xamarin.Forms.DependencyService).

В следующем примере используется метод [`Register`](xref:Xamarin.Forms.DependencyService.Register*) для регистрации реализации интерфейса `IDeviceOrientationService` для iOS.

```csharp
[Register("AppDelegate")]
public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
{
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new App());
        DependencyService.Register<IDeviceOrientationService, DeviceOrientationService>();
        return base.FinishedLaunching(app, options);
    }
}
```

В этом примере метод [`Register`](xref:Xamarin.Forms.DependencyService.Register*) регистрирует конкретный тип `DeviceOrientationService` с интерфейсом `IDeviceOrientationService`. Таким же образом можно использовать перегрузку метода [`Register`](xref:Xamarin.Forms.DependencyService.Register*), чтобы зарегистрировать реализацию платформы в [`DependencyService`](xref:Xamarin.Forms.DependencyService).

```csharp
DependencyService.Register<DeviceOrientationService>();
```

В этом примере [`Register`](xref:Xamarin.Forms.DependencyService.Register*) регистрирует `DeviceOrientationService` в [`DependencyService`](xref:Xamarin.Forms.DependencyService). Это приводит к регистрации конкретного типа с интерфейсом, который он реализует.

Кроме того, можно зарегистрировать отдельный существующий экземпляр объекта с помощью метода `RegisterSingleton`:

```csharp
var service = new DeviceOrientationService();
DependencyService.RegisterSingleton<IDeviceOrientationService>(service);
```

В этом примере метод `RegisterSingleton` регистрирует отдельный экземпляр объекта `DeviceOrientationService` с интерфейсом `IDeviceOrientationService`.

Аналогичным образом реализации интерфейса `IDeviceOrientationService` на других платформах можно зарегистрировать с помощью методов [`Register`](xref:Xamarin.Forms.DependencyService.Register*) или `RegisterSingleton`.

> [!IMPORTANT]
> Регистрация с помощью методов [`Register`](xref:Xamarin.Forms.DependencyService.Register*) и `RegisterSingleton` должна выполняться в проектах платформы до того, как функции, предоставляемые реализацией платформы, будут вызваны из общего кода.

## <a name="resolve-the-platform-implementations"></a>Разрешение реализаций платформы

Реализации платформы необходимо разрешить до их вызова. Обычно это делается в общем коде с помощью метода [`DependencyService.Get<T>`](xref:Xamarin.Forms.DependencyService.Get*). Это также можно сделать с помощью метода [`DependencyService.Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*).

По умолчанию [`DependencyService`](xref:Xamarin.Forms.DependencyService) разрешает только зависящие от платформы реализации с конструкторами без параметров. Однако в Xamarin.Forms можно внедрить метод разрешения зависимостей, который разрешает зависящие от платформы реализации с помощью контейнера внедрения зависимостей или фабричных методов. Такой подход позволяет разрешать зависящие от платформы реализации, имеющие конструкторы с параметрами. Дополнительные сведения см. в статье [Разрешение зависимостей в Xamarin.Forms](~/xamarin-forms/internals/dependency-resolution.md).

> [!IMPORTANT]
> Вызов реализации платформы, которая не была зарегистрирована с помощью [`DependencyService`](xref:Xamarin.Forms.DependencyService) приведет к возникновению `NullReferenceException`.

### <a name="resolve-using-the-getlttgt-method"></a>Разрешение с помощью метода Get&lt;T&gt;

Метод [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) извлекает реализуемый платформой интерфейс `T` во время выполнения и:

- либо создает его отдельный экземпляр;
- либо возвращает отдельный существующий экземпляр, зарегистрированный с помощью `DependencyService` методом `RegisterSingleton`.

В обоих случаях этот экземпляр существует в течение всего времени существования приложения и его будут получать все последующие вызовы для разрешения той же реализации платформы.

Следующий пример кода демонстрирует вызов метода [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) для разрешения интерфейса `IDeviceOrientationService` и последующего вызова его метода `GetOrientation`:

```csharp
IDeviceOrientationService service = DependencyService.Get<IDeviceOrientationService>();
DeviceOrientation orientation = service.GetOrientation();
```

Этот код можно сократить до одной строки:

```csharp
DeviceOrientation orientation = DependencyService.Get<IDeviceOrientationService>().GetOrientation();
```

> [!NOTE]
> По умолчанию метод [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) возвращает отдельный экземпляр интерфейса `T`, реализуемого платформой. Хотя это можно изменить. Дополнительные сведения см. в разделе [Управлении временем существования разрешенных объектов](#manage-the-lifetime-of-resolved-objects).

### <a name="resolve-using-the-resolvelttgt-method"></a>Разрешение с помощью метода Resolve&lt;T&gt;

Метод [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) извлекает интерфейс `T`, реализуемый платформой во время выполнения, используя метод разрешения зависимостей, который внедряется в Xamarin.Forms с помощью класса [`DependencyResolver`](xref:Xamarin.Forms.Internals.DependencyResolver). Если метод разрешения зависимостей не был внедрен в Xamarin.Forms, метод `Resolve<T>` переключается на вызов метода [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*), чтобы извлечь реализацию платформы. Дополнительные сведения о внедрении метода разрешения зависимостей в Xamarin.Forms см. в статье о [разрешении зависимостей в Xamarin.Forms](~/xamarin-forms/internals/dependency-resolution.md).

Следующий пример кода демонстрирует вызов метода [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) для разрешения интерфейса `IDeviceOrientationService` и последующего вызова его метода `GetOrientation`:

```csharp
IDeviceOrientationService service = DependencyService.Resolve<IDeviceOrientationService>();
DeviceOrientation orientation = service.GetOrientation();
```

Этот код можно сократить до одной строки:

```csharp
DeviceOrientation orientation = DependencyService.Resolve<IDeviceOrientationService>().GetOrientation();
```

> [!NOTE]
> Когда метод [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) переключается на вызов метода [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*), он по умолчанию возвращает отдельный экземпляр интерфейса `T`, реализуемого платформой. Хотя это можно изменить. Дополнительные сведения см. в разделе [Управлении временем существования разрешенных объектов](#manage-the-lifetime-of-resolved-objects).

## <a name="manage-the-lifetime-of-resolved-objects"></a>Управлении временем существования разрешенных объектов

По умолчанию класс [`DependencyService`](xref:Xamarin.Forms.DependencyService) разрешает отдельную реализацию платформы. Таким образом, реализации платформы будут существовать в течение всего срока существования приложения.

Такое поведение определяется необязательным аргументом [`DependencyFetchTarget`](xref:Xamarin.Forms.DependencyFetchTarget) в методах [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) и [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*). Перечисление [`DependencyFetchTarget`](xref:Xamarin.Forms.DependencyFetchTarget) определяет два члена:

- `GlobalInstance`, который возвращает отдельную реализацию платформы;
- `NewInstance`, который возвращает новый экземпляр реализации платформы. После этого управление временем существования экземпляра реализации платформы передается приложению.

Методы [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) и [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) назначают [`DependencyFetchTarget.GlobalInstance`](xref:Xamarin.Forms.DependencyFetchTarget) свои необязательные аргументы, поэтому всегда разрешаются отдельные экземпляры реализаций платформы. Это поведение можно изменить так, чтобы создавались новые экземпляры реализаций платформы, указав [`DependencyFetchTarget.NewInstance`](xref:Xamarin.Forms.DependencyFetchTarget) в качестве аргументов в методах `Get<T>` и `Resolve<T>`.

```csharp
ITextToSpeechService service = DependencyService.Get<ITextToSpeechService>(DependencyFetchTarget.NewInstance);
```

В этом примере [`DependencyService`](xref:Xamarin.Forms.DependencyService) создает новый экземпляр реализации платформы для интерфейса `ITextToSpeechService`. Все последующие вызовы для разрешения `ITextToSpeechService` будут также создавать новые экземпляры.

В результате постоянного создания новых экземпляров реализации платформы управление временем существования экземпляров передается приложению. Следовательно, если вы подписаны на событие, определяемое в реализации платформы, вы должны отменить подписку на него, когда реализация платформы больше не требуется. Кроме того, это означает, что реализациям платформы может потребоваться реализация `IDisposable` и очистка своих ресурсов с помощью методов `Dispose`. Пример приложения демонстрирует этот сценарий с реализациями платформы `TextToSpeechService`.

Когда приложение завершает использование реализации платформы, реализующей `IDisposable`, оно должно вызывать реализацию `Dispose` объекта. Один из способов сделать это — воспользоваться инструкцией `using`.

```csharp
ITextToSpeechService service = DependencyService.Get<ITextToSpeechService>(DependencyFetchTarget.NewInstance);
using (service as IDisposable)
{
    await service.SpeakAsync("Hello world");
}
```

В этом примере после вызова метода `SpeakAsync` инструкция `using` автоматически удаляет объект реализации платформы. Это приводит к вызову метода `Dispose` объекта, который выполняет необходимую очистку.

Дополнительные сведения о вызове метода `Dispose` объекта см. в статье [Использование объектов, реализующих IDisposable](/dotnet/standard/garbage-collection/using-objects).

## <a name="related-links"></a>Связанные ссылки

- [Использование класса DependencyService (пример)](/samples/xamarin/xamarin-forms-samples/dependencyservice/)
- [Разрешение зависимостей в Xamarin.Forms](~/xamarin-forms/internals/dependency-resolution.md)