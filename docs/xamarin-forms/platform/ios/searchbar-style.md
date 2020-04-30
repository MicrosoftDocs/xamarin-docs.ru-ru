---
title: Стиль Сеарчбар в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать зависящую от платформы iOS платформу, которая определяет, имеет ли Сеарчбар фон.
ms.prod: xamarin
ms.assetid: 3D512DD6-078E-4BC6-926E-62BA6F4DE640
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2020
ms.openlocfilehash: 7d95a90e96f868b6d8368054659f978555bc28ac
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82532955"
---
# <a name="searchbar-style-on-ios"></a>Стиль Сеарчбар в iOS

[![Скачать пример](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Этот элемент управления, относящийся к платформе [`SearchBar`](xref:Xamarin.Forms.SearchBar) iOS, определяет, имеет ли он фон. Он используется в XAML путем задания свойству `SearchBar.SearchBarStyle` BIND значения `UISearchBarStyle` перечисления.

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ios:SearchBar.SearchBarStyle="Minimal"
                   Placeholder="Enter search term" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

SearchBar searchBar = new SearchBar { Placeholder = "Enter search term" };
searchBar.On<iOS>().SetSearchBarStyle(UISearchBarStyle.Minimal);
```

`SearchBar.On<iOS>` Метод указывает, что эта платформа будет запускаться только в iOS. `SearchBar.SetSearchBarStyle` Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для управления тем, имеет ли объект [`SearchBar`](xref:Xamarin.Forms.SearchBar) фон. `UISearchBarStyle` Перечисление предоставляет три возможных значения:

- `Default`Указывает, что [`SearchBar`](xref:Xamarin.Forms.SearchBar) имеет стиль по умолчанию. Это значение по умолчанию для `SearchBar.SearchBarStyle` привязываемого свойства.
- `Prominent`Указывает, что [`SearchBar`](xref:Xamarin.Forms.SearchBar) имеет полупрозрачный фон, а поле поиска является непрозрачным.
- `Minimal`Указывает, что [`SearchBar`](xref:Xamarin.Forms.SearchBar) не имеет фона, а поле поиска прозрачно.

Кроме того, `SearchBar.GetSearchBarStyle` метод можно использовать для возврата объекта `UISearchBarStyle` , который применяется к. `SearchBar`

В результате заданный `UISearchBarStyle` элемент применяется к элементу [`SearchBar`](xref:Xamarin.Forms.SearchBar), который определяет, имеет ли фон `SearchBar` элемент.

![Снимок экрана: стили Сеарчбар в iOS](searchbar-style-images/searchbar-styles.png "Стили Сеарчбар в iOS")

На следующих снимках экрана `UISearchBarStyle` показаны элементы, [`SearchBar`](xref:Xamarin.Forms.SearchBar) применяемые к объектам `BackgroundColor` , для которых заданы свойства.

![Снимок экрана: стили Сеарчбар с цветом фона в iOS](searchbar-style-images/searchbar-background-styles.png "Стили Сеарчбар с цветом фона в iOS")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
