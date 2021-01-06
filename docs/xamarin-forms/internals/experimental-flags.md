---
title: Xamarin.Forms экспериментальные флаги
description: Xamarin.Forms экспериментальные флаги позволяют группе инженеров-разработчиков поставлять новые функции пользователям быстрее, в то же время сохраняя возможность изменять API функций до того, как они переходят в стабильный выпуск.
ms.prod: xamarin
ms.assetid: AF4BDD27-89F6-48AE-A8CD-D7E4DDA2CCA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 55a710ace10834cffdecb5247c2df410e8e1396e
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2021
ms.locfileid: "97939827"
---
# <a name="no-locxamarinforms-experimental-flags"></a>Xamarin.Forms экспериментальные флаги

При реализации новой Xamarin.Forms функции она иногда помещается за экспериментальный флаг. Это позволяет группе разработчиков предоставлять новые функции быстрее, в то же время сохраняя возможность изменения API-интерфейсов функций до того, как они переходят в стабильный выпуск. Затем экспериментальный флаг удаляется после того, как функция переместится в стабильный выпуск.

Xamarin.Forms включает следующие экспериментальные флаги:

- `Shell_UWP_Experimental`

Использование функций, которые находятся за экспериментальным флагом, требует включения флага или флагов в приложении. Существует два подхода к включению экспериментальных флагов.

- Включите экспериментальный флаг в проектах платформы.
- Включите экспериментальный флаг в своем `App` классе.

> [!WARNING]
> Использование функциональных возможностей, которые находятся за экспериментальным флагом, без включения флага, приведет к тому, что приложение создаст исключение, указывающее, какой флаг должен быть включен.

## <a name="enable-flags-in-platform-projects"></a>Включение флагов в проектах платформы

`Xamarin.Forms.Forms.SetFlags`Метод можно использовать для включения экспериментального флага в проектах платформы:

```csharp
Xamarin.Forms.Forms.SetFlags("Shell_UWP_Experimental");
```

`SetFlags`Метод должен быть вызван в вашем классе в `AppDelegate` iOS, в `MainActivity` классе в Android и в классе в `App` UWP.

> [!IMPORTANT]
> Включение экспериментального флага в проектах платформы должно происходить до `Forms.Init` вызова метода.

`Xamarin.Forms.Forms.SetFlags`Метод принимает `string` аргумент массива, что позволяет включить несколько экспериментальных флагов в одном вызове метода:

```csharp
Xamarin.Forms.Forms.SetFlags(new string[] { "Shell_UWP_Experimental", "AnotherFeature_Experimental" });
```

> [!WARNING]
> Никогда не вызывайте `SetFlags` метод более одного раза, так как последующие вызовы будут перезаписывать результат предыдущих вызовов.

## <a name="enable-flags-in-your-app-class"></a>Включение флагов в классе приложения

`Device.SetFlags`Метод можно использовать для включения экспериментального флага в `App` классе в проекте общего кода:

```csharp
Device.SetFlags(new string[]{ "Shell_UWP_Experimental" });
```

`Device.SetFlags`Метод принимает `IReadOnlyList<string>` аргумент, что делает возможным включение нескольких экспериментальных флагов в одном вызове метода:

```csharp
Device.SetFlags(new string[]{ "Shell_UWP_Experimental", "AnotherFeature_Experimental" });
```

> [!WARNING]
> Никогда не вызывайте `SetFlags` метод более одного раза, так как последующие вызовы будут перезаписывать результат предыдущих вызовов.

## <a name="old-experimental-flags"></a>Старые экспериментальные флаги

В следующей таблице перечислены экспериментальные флаги для функций, которые теперь доступны в общедоступной версии, и Xamarin.Forms выпуск, в котором был удален экспериментальный флаг:

| Флаг | Xamarin.Forms Отпускании |
| ---- | --------------------- |
| `AppTheme_Experimental` | 4.8 |
| `Brush_Experimental` | 5.0 |
| `CarouselView_Experimental` | 5.0 |
| `CollectionView_Experimental` | 4.3 |
| `DragAndDrop_Experimental` | 5,0 |
| `FastRenderers_Experimental` | 4.0 |
| `IndicatorView_Experimental` | 4.7 |
| `Markup_Experimental` | 5.0 |
| `RadioButton_Experimental` | 5.0 |
| `Shapes_Experimental` | 5.0 |
| `Shell_Experimental` | 4.0  |
| `StateTriggers_Experimental` | 4.7 |
| `SwipeView_Experimental` | 5,0 |
| `Visual_Experimental` | 3,6 |
