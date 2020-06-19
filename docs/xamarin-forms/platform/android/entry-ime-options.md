---
title: Параметры редактора метода ввода для Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать зависящую от платформы Android платформу, которая задает параметры редактора метода ввода для мягкой клавиатуры для записи.
ms.prod: xamarin
ms.assetid: 7909C738-04B2-4476-9A3B-A6D79BC3B9B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: bb77e9fafe39bf76a7d4290dba0bc658cd15094f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84140040"
---
# <a name="entry-input-method-editor-options-on-android"></a>Параметры редактора метода ввода для Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа Android определяет параметры редактора метода ввода (IME) для экранной клавиатуры для [`Entry`](xref:Xamarin.Forms.Entry) . Это включает в себя настройку кнопки действия пользователя в нижнем углу экранной клавиатуры и взаимодействие с `Entry` . Он используется в XAML путем присвоения [`Entry.ImeOptions`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty) свойству присоединенного свойства значения [`ImeFlags`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) перечисления:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Entry ... android:Entry.ImeOptions="Send" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

entry.On<Android>().SetImeOptions(ImeFlags.Send);
```

`Entry.On<Android>`Метод указывает, что эта платформа будет запускаться только в Android. [ `Entry.SetImeOptions` ] (Xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Entry. Сетимеоптионс ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Entry}, Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Имефлагс)). в [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространстве имен используется для задания параметра действия метода ввода для программируемой клавиатуры для [`Entry`](xref:Xamarin.Forms.Entry) с [`ImeFlags`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) перечислением, предоставляющей следующие значения:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default)— Указывает, что никаких конкретных ключей действий не требуется, и что базовый элемент управления будет создавать собственный, если это возможно. Это будет значение `Next` или `Done` .
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None)— Указывает, что ключ действия не будет предоставлен.
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go)— Указывает, что ключ действия выполняет операцию "Go", предпринимая пользователя на целевой объект введенного текста.
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search)— Указывает, что ключ действия выполняет операцию поиска, предпринимая пользователю результаты поиска введенного текста.
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send)— Указывает, что ключ действия выполняет операцию Send, доставляющую текст в ее целевой объект.
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next)— Указывает, что ключ действия выполняет операцию "Далее", предпринимая пользователю следующее поле, которое будет принимать текст.
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done)— Указывает, что ключ действия выполняет операцию "Done", закрывая мягкую клавиатуру.
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous)— Указывает, что ключ действия выполняет операцию "назад", предпринимая пользователя на предыдущее поле, которое будет принимать текст.
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction)— Маска для выбора параметров действия.
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning)— Указывает, что средство проверки орфографии не будет изучено от пользователя и не предложит исправления на основе того, что пользователь ввел ранее.
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen)— Указывает, что пользовательский интерфейс не должен двигаться на весь экран.
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi)— Указывает, что для извлеченного текста не будет отображаться пользовательский интерфейс.
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction)— Указывает, что для настраиваемых действий пользовательский интерфейс отображаться не будет.

В результате заданное [`ImeFlags`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) значение применяется к экранной клавиатуре для [`Entry`](xref:Xamarin.Forms.Entry) , который задает параметры редактора метода ввода:

[![Специальный редактор метода ввода для платформы](entry-ime-options-images/entry-imeoptions.png "Специальный редактор метода ввода для платформы")](entry-ime-options-images/entry-imeoptions-large.png#lightbox "Специальный редактор метода ввода для платформы")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
