---
title: Общие сведения о Xamarin.Forms DependencyService
description: В этой статье объясняется, как использовать класс Xamarin.Forms DependencyService для вызова собственных функций платформы.
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/12/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2d63ac91031fc4a2cc127272a075f15a8ea81f4e
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91555286"
---
# <a name="no-locxamarinforms-dependencyservice-introduction"></a>Общие сведения о Xamarin.Forms DependencyService

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice/)

Класс [`DependencyService`](xref:Xamarin.Forms.DependencyService) — это указатель служб, который позволяет приложениям Xamarin.Forms вызывать собственные функции платформы из общего кода.

Использование [`DependencyService`](xref:Xamarin.Forms.DependencyService) для вызова собственных функций платформы включает следующее:

1. Создание интерфейса для собственных функций платформы в общем коде. Дополнительные сведения см. в разделе [Создание интерфейса](#create-an-interface).
1. Реализацию интерфейса в требуемых проектах платформы. Дополнительные сведения см. в разделе [Реализация интерфейса для каждой платформы](#implement-the-interface-on-each-platform).
1. Регистрацию реализаций платформы в [`DependencyService`](xref:Xamarin.Forms.DependencyService). Это позволяет Xamarin.Forms найти реализации платформы во время выполнения. Дополнительные сведения см. в разделе [Регистрация реализаций платформы](#register-the-platform-implementations).
1. Разрешение реализаций платформы из общего кода и их вызов. Дополнительные сведения см. в разделе [Разрешение реализаций платформы](#resolve-the-platform-implementations).

На следующей схеме показано, как собственные функции платформы вызываются в приложении Xamarin.Forms.

![Обнаружение служб с помощью класса Xamarin.Forms DependencyService](introduction-images/dependency-service.png "Расположение службы DependencyService")

## <a name="create-an-interface"></a>Создание интерфейса

Чтобы обеспечить возможность вызова собственных функций платформы из общего кода, сперва нужно создать интерфейс, который определяет API для взаимодействия с собственными функциями платформы. Этот интерфейс должен находиться в проекте с общим кодом.

Следующий пример демонстрирует интерфейс для API, который может использоваться для получения данных об ориентации устройства:

```csharp
public interface IDeviceOrientationService
{
    DeviceOrientation GetOrientation();
}
```

## <a name="implement-the-interface-on-each-platform"></a>Реализация интерфейса для каждой платформы

Созданный интерфейс, определяющий API для взаимодействия с собственными функциями платформы, необходимо реализовать в каждом проекте платформы.

### <a name="ios"></a>iOS

В следующем примере кода показана реализация интерфейса `IDeviceOrientationService` для iOS:

```csharp
namespace DependencyServiceDemos.iOS
{
    public class DeviceOrientationService : IDeviceOrientationService
    {
        public DeviceOrientation GetOrientation()
        {
            UIInterfaceOrientation orientation = UIApplication.SharedApplication.StatusBarOrientation;

            bool isPortrait = orientation == UIInterfaceOrientation.Portrait ||
                orientation == UIInterfaceOrientation.PortraitUpsideDown;
            return isPortrait ? DeviceOrientation.Portrait : DeviceOrientation.Landscape;
        }
    }
}
```

### <a name="android"></a>Android

В следующем примере кода показана реализация интерфейса `IDeviceOrientationService` для Android:

```csharp
namespace DependencyServiceDemos.Droid
{
    public class DeviceOrientationService : IDeviceOrientationService
    {
        public DeviceOrientation GetOrientation()
        {
            IWindowManager windowManager = Android.App.Application.Context.GetSystemService(Context.WindowService).JavaCast<IWindowManager>();

            SurfaceOrientation orientation = windowManager.DefaultDisplay.Rotation;
            bool isLandscape = orientation == SurfaceOrientation.Rotation90 ||
                orientation == SurfaceOrientation.Rotation270;
            return isLandscape ? DeviceOrientation.Landscape : DeviceOrientation.Portrait;
        }
    }
}
```

### <a name="universal-windows-platform"></a>Универсальная платформа Windows

В следующем примере кода показана реализация интерфейса `IDeviceOrientationService` для универсальной платформы Windows (UWP):

```csharp
namespace DependencyServiceDemos.UWP
{
    public class DeviceOrientationService : IDeviceOrientationService
    {
        public DeviceOrientation GetOrientation()
        {
            ApplicationViewOrientation orientation = ApplicationView.GetForCurrentView().Orientation;
            return orientation == ApplicationViewOrientation.Landscape ? DeviceOrientation.Landscape : DeviceOrientation.Portrait;
        }
    }
}
```

## <a name="register-the-platform-implementations"></a>Регистрация реализаций платформы

После реализации интерфейса в каждом проекте платформы необходимо зарегистрировать реализации платформы в [`DependencyService`](xref:Xamarin.Forms.DependencyService), чтобы платформа Xamarin.Forms могла находить их во время выполнения. Обычно это делается с помощью класса [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute), который указывает, что заданный тип предоставляет реализацию интерфейса.

В следующем примере демонстрируется использование [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) для регистрации реализации интерфейса `IDeviceOrientationService` для iOS.

```csharp
using Xamarin.Forms;

[assembly: Dependency(typeof(DependencyServiceDemos.iOS.DeviceOrientationService))]
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

В этом примере [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) регистрирует `DeviceOrientationService` в [`DependencyService`](xref:Xamarin.Forms.DependencyService). Аналогичным образом реализации интерфейса `IDeviceOrientationService` на других платформах должны быть зарегистрированы с помощью [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute).

Дополнительные сведения о регистрации реализаций платформы в [`DependencyService`](xref:Xamarin.Forms.DependencyService) см. в статье [Регистрация и разрешение класса Xamarin.Forms DependencyService](registration-and-resolution.md).

## <a name="resolve-the-platform-implementations"></a>Разрешение реализаций платформы

Зарегистрированные реализации платформы в [`DependencyService`](xref:Xamarin.Forms.DependencyService) необходимо разрешить до их вызова. Обычно это делается в общем коде с помощью метода [`DependencyService.Get<T>`](xref:Xamarin.Forms.DependencyService.Get*).

Следующий пример кода демонстрирует вызов метода [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) для разрешения интерфейса `IDeviceOrientationService` и последующего вызова его метода `GetOrientation`:

```csharp
IDeviceOrientationService service = DependencyService.Get<IDeviceOrientationService>();
DeviceOrientation orientation = service.GetOrientation();
```

Этот код можно сократить до одной строки:

```csharp
DeviceOrientation orientation = DependencyService.Get<IDeviceOrientationService>().GetOrientation();
```

Дополнительные сведения о разрешении реализаций платформы в [`DependencyService`](xref:Xamarin.Forms.DependencyService) см. в статье [Регистрация и разрешение класса Xamarin.Forms DependencyService](registration-and-resolution.md).

## <a name="related-links"></a>Связанные ссылки

- [Использование класса DependencyService (пример)](/samples/xamarin/xamarin-forms-samples/dependencyservice/)
- [Регистрация и разрешение класса Xamarin.Forms DependencyService](registration-and-resolution.md)