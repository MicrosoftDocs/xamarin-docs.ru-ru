---
title: Экспериментальные флаги Xamarin. Forms
description: Экспериментальные флаги Xamarin. Forms позволяют группе инженеров-разработчиков поставлять новые функции пользователям быстрее, в то же время сохраняя возможность изменять API функций до того, как они переходят в стабильный выпуск.
ms.prod: xamarin
ms.assetid: AF4BDD27-89F6-48AE-A8CD-D7E4DDA2CCA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/14/2020
ms.openlocfilehash: cca377a7a88599bc34fd66695ad303162e6be200
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82516549"
---
# <a name="xamarinforms-experimental-flags"></a>Экспериментальные флаги Xamarin. Forms

Когда реализована новая функция Xamarin. Forms, она иногда помещается за экспериментальный флаг. Это позволяет группе разработчиков предоставлять новые функции быстрее, в то же время сохраняя возможность изменения API-интерфейсов функций до того, как они переходят в стабильный выпуск. Затем экспериментальный флаг удаляется после того, как функция переместится в стабильный выпуск.

Xamarin. Forms включает следующие экспериментальные флаги:

- `AppTheme_Experimental`
- `CarouselView_Experimental`
- `Expander_Experimental`
- `IndicatorView_Experimental`
- `Markup_Experimental`
- `MediaElement_Experimental`
- `RadioButton_Experimental`
- `Shell_UWP_Experimental`
- `StateTriggers_Experimental`
- `SwipeView_Experimental`

Использование функций, которые находятся за экспериментальным флагом, требует включения флага или флагов в приложении. Существует два подхода к включению экспериментальных флагов.

- Включите экспериментальный флаг или флаги в проектах платформы.
- Включите экспериментальный флаг или флаги в своем `App` классе.

> [!WARNING]
> Использование функциональных возможностей, которые находятся за экспериментальным флагом, без включения флага, приведет к тому, что приложение создаст исключение, указывающее, какой флаг должен быть включен.

## <a name="enable-flags-in-platform-projects"></a>Включение флагов в проектах платформы

`Xamarin.Forms.Forms.SetFlags` Метод можно использовать для включения экспериментального флага в проектах платформы:

```csharp
Xamarin.Forms.Forms.SetFlags("CarouselView_Experimental");
```

`SetFlags` Метод должен быть вызван в вашем `AppDelegate` классе в iOS, в `MainActivity` классе в Android и в `App` классе в UWP.

> [!IMPORTANT]
> Включение экспериментального флага в проектах платформы должно происходить до вызова `Forms.Init` метода.

`Xamarin.Forms.Forms.SetFlags` Метод принимает аргумент `string` массива, что позволяет включить несколько экспериментальных флагов в одном вызове метода:

```csharp
Xamarin.Forms.Forms.SetFlags(new string[] { "CarouselView_Experimental", "IndicatorView_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> Никогда не вызывайте `SetFlags` метод более одного раза, так как последующие вызовы будут перезаписывать результат предыдущих вызовов.

## <a name="enable-flags-in-your-app-class"></a>Включение флагов в классе приложения

`Device.SetFlags` Метод можно использовать для включения экспериментального флага в `App` классе в проекте общего кода:

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

`Device.SetFlags` Метод принимает `IReadOnlyList<string>` аргумент, что делает возможным включение нескольких экспериментальных флагов в одном вызове метода:

```csharp
Device.SetFlags(new string[]{ "CarouselView_Experimental", "MediaElement_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> Никогда не вызывайте `SetFlags` метод более одного раза, так как последующие вызовы будут перезаписывать результат предыдущих вызовов.
