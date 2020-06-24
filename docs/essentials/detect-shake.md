---
title: Xamarin.Essentials. Обнаружение тряски
description: Класс Accelerometer в Xamarin.Essentials позволяет обнаруживать движение встряхивания устройства.
ms.assetid: 07513D32-120F-4F12-8757-A47802A8027B
author: jamesmontemagno
ms.author: jamont
ms.date: 05/28/2019
ms.custom: video
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4fd4c7d0e3ffe04ebbb936cff0992272a9085649
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84802425"
---
# <a name="xamarinessentials-detect-shake"></a>Xamarin.Essentials. Обнаружение тряски

Класс **[Accelerometer](accelerometer.md)** позволяет считывать данные с датчика акселерометра устройства, который показывает ускорение устройства в трехмерном пространстве. Кроме того, он позволяет регистрировать события, когда пользователь трясет устройство.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-detect-shake"></a>Использование обнаружения тряски

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Для обнаружения тряски устройства необходимо использовать функциональные возможности акселерометра, вызвав методы `Start` и `Stop` для прослушивания изменений ускорения и обнаружения тряски. При каждом обнаружении встряхивания срабатывает событие `ShakeDetected`. Рекомендуется использовать для `SensorSpeed` значение `Game` или указать более высокую скорость. Пример использования:

```csharp

public class DetectShakeTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Game;

    public DetectShakeTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ShakeDetected  += Accelerometer_ShakeDetected ;
    }

    void Accelerometer_ShakeDetected (object sender, EventArgs e)
    {
        // Process shake event
    }

    public void ToggleAccelerometer()
    {
        try
        {
            if (Accelerometer.IsMonitoring)
              Accelerometer.Stop();
            else
              Accelerometer.Start(speed);
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

## <a name="implementation-details"></a>Сведения о реализации

Для обнаружения тряски API использует необработанные показания акселерометра для вычисления ускорения. Он использует простой механизм очереди для определения того, произошло ли 3/4 последних событий акселерометра за последнюю половину секунды. Ускорение рассчитывается путем добавления квадрата показаний X, Y и Z от акселерометра и его сравнения с определенным порогом.

## <a name="api"></a>API

- [Исходный код класса Accelerometer](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Accelerometer)
- [Документация по API Accelerometer](xref:Xamarin.Essentials.Accelerometer)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Detect-Shake-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
