---
title: Xamarin.Essentials. Гироскоп
description: Класс Gyroscope в Xamarin.Essentials позволяет отслеживать показатели гироскопа устройства, который измеряет вращение вокруг трех основных осей устройства.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5a085ac5bd18e660339443ea33fe552dd86259cb
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84802293"
---
# <a name="xamarinessentials-gyroscope"></a>Xamarin.Essentials. Гироскоп

Класс **Gyroscope** позволяет отслеживать датчик гироскопа устройства, который представляет собой поворот вокруг трех основных осей устройства.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-gyroscope"></a>Использование Gyroscope

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Функция Gyroscope выполняется путем вызова методов `Start` и `Stop` для ожидания передачи данных об изменениях в гироскопе. Все изменения возвращаются через событие `ReadingChanged` в рад/с. Пример использования:

```csharp

public class GyroscopeTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public GyroscopeTest()
    {
        // Register for reading changes.
        Gyroscope.ReadingChanged += Gyroscope_ReadingChanged;
    }

    void Gyroscope_ReadingChanged(object sender, GyroscopeChangedEventArgs e)
    {
        var data = e.Reading;
        // Process Angular Velocity X, Y, and Z reported in rad/s
        Console.WriteLine($"Reading: X: {data.AngularVelocity.X}, Y: {data.AngularVelocity.Y}, Z: {data.AngularVelocity.Z}");
    }

    public void ToggleGyroscope()
    {
        try
        {
            if (Gyroscope.IsMonitoring)
              Gyroscope.Stop();
            else
              Gyroscope.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [Исходный код Gyroscope](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Gyroscope)
- [Документация по API Gyroscope](xref:Xamarin.Essentials.Gyroscope)
