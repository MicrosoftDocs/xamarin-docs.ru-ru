---
title: Индикатор активности в Xamarin.Forms
description: Элемент управления Активитиндикатор указывает пользователям, что приложение вовлечено в длительное действие без указания хода выполнения. В этой статье объясняется, как использовать Активитиндикатор в XAML и коде.
ms.prod: xamarin
ms.assetid: 4CEED02D-5CA3-4C3A-B7ED-3193FC272261
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/10/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 739fc90eda513e4627f5804283bdcab9c0cbacb4
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373124"
---
# <a name="no-locxamarinforms-activityindicator"></a>Xamarin.Forms активитиндикатор
[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)

Xamarin.Forms [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) Элемент управления отображает анимацию, чтобы показать, что приложение вовлечено в длительное действие. В отличие от [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) , параметр `ActivityIndicator` не дает указания о ходе выполнения. Объект `ActivityIndicator` наследует от [`View`](xref:Xamarin.Forms.View) .

На следующих снимках экрана показан `ActivityIndicator` элемент управления iOS и Android:

![Снимок экрана Активитиндикатор в iOS и Android](activityindicator-images/activityindicators-default.png "Снимок экрана Активитиндикатор в iOS и Android")

`ActivityIndicator`Элемент управления определяет следующие свойства:

* [`Color`](xref:Xamarin.Forms.ActivityIndicator.Color)`Color`значение, определяющее цвет дисплея `ActivityIndicator` .
* [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning)`bool`значение, указывающее, должен ли объект `ActivityIndicator` быть видимым, анимировать или скрываться. Если значение параметра является `false` `ActivityIndicator` невидимым.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что `ActivityIndicator` можно использовать стиль и цель привязок данных.

## <a name="create-an-activityindicator"></a>Создание Активитиндикатор

`ActivityIndicator`Экземпляр класса можно создать на языке XAML. Его `IsRunning` свойство определяет, является ли элемент управления видимым и анимацией. `IsRunning`По умолчанию свойство имеет значение `false` . В следующем примере показано, как создать экземпляр `ActivityIndicator` в XAML с необязательным `IsRunning` набором свойств:

```xaml
<ActivityIndicator IsRunning="true" />
```

`ActivityIndicator`Также можно создать в коде:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { IsRunning = true };
```

## <a name="activityindicator-appearance-properties"></a>Свойства внешнего вида Активитиндикатор

`Color`Свойство определяет `ActivityIndicator` цвет. В следующем примере показано, как создать экземпляр `ActivityIndicator` в XAML с `Color` набором свойств:

```xaml
<ActivityIndicator Color="Orange" />
```

Это `Color` свойство также может быть задано при создании `ActivityIndicator` в коде:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { Color = Color.Orange };
```

На следующих снимках экрана показано, `ActivityIndicator` `Color` для чего свойство имеет значение `Color.Orange` в iOS и Android:

![Снимок экрана с стилем Активитиндикатор в iOS и Android](activityindicator-images/activityindicators-styled.png "Снимок экрана с стилем Активитиндикатор в iOS и Android")

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации Активитиндикатор](/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)
* [ProgressBar](~/xamarin-forms/user-interface/progressbar.md)