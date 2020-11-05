---
title: " Сеарчбар Проверка орфографии в Windows"
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Windows, которая позволяет Сеарчбар взаимодействовать с модулем проверки орфографии.
ms.prod: xamarin
ms.assetid: 7974C91F-7502-4DB3-B0E9-C45E943DDA26
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b97ce2015ba632d100b946a5b287f0eae737123c
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93368691"
---
# <a name="searchbar-spell-check-on-windows"></a>Сеарчбар Проверка орфографии в Windows

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Это универсальная платформа Windows, зависящее от платформы, позволяет [`SearchBar`](xref:Xamarin.Forms.SearchBar) взаимодействовать с модулем проверки орфографии. Он используется в XAML путем присвоения [`SearchBar.IsSpellCheckEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) свойству присоединенного свойства `boolean` значения:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

`SearchBar.On<Windows>`Метод указывает, что данная платформа будет запускаться только на универсальная платформа Windows. [ `SearchBar.SetIsSpellCheckEnabled` ] (Xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Сеарчбар. Сетисспеллчеккенаблед ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Сеарчбар}, System. Boolean)) в [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространстве имен включает и выключает проверку орфографии. Кроме того, `SearchBar.SetIsSpellCheckEnabled` метод можно использовать для переключения средства проверки орфографии путем вызова [ `SearchBar.GetIsSpellCheckEnabled` ] (xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Сеарчбар. Жетисспеллчеккенаблед ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Сеарчбар})) метод, который возвращает значение, определяющее, включена ли проверка орфографии:

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

Результат заключается в том, что текст, введенный в параметре, [`SearchBar`](xref:Xamarin.Forms.SearchBar) можно проверить орфографию, при этом пользователю будут указаны неправильные слова:

![Сеарчбар проверка орфографии для конкретной платформы](searchbar-spell-check-images/searchbar-spellcheck.png "Сеарчбар проверка орфографии для конкретной платформы")

> [!NOTE]
> `SearchBar`Класс в [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространстве имен также имеет [`EnableSpellCheck`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) методы и [`DisableSpellCheck`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) , которые можно использовать для включения и отключения проверки орфографии в `SearchBar` , соответственно.

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ВиндовсспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)