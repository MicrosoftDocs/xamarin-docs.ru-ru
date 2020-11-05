---
title: Направление движения RefreshView в Windows
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Windows, которая позволяет изменить направление извлечения Рефрешвиев.
ms.prod: xamarin
ms.assetid: 407A862B-281E-4384-9696-C0655830B84D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2aebd86b4b15dec1a79cb60057e0c04f2e196ac5
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93372903"
---
# <a name="refreshview-pull-direction-on-windows"></a>Направление движения RefreshView в Windows

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Этот универсальная платформа Windows зависящий от платформы, позволяет изменить направление извлечения объекта, `RefreshView` чтобы оно совпадало с ориентацией прокручиваемого элемента управления, отображающего данные. Он используется в XAML путем задания `RefreshView.RefreshPullDirection` свойству BIND значения `RefreshPullDirection` перечисления.

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <RefreshView windows:RefreshView.RefreshPullDirection="LeftToRight"
                 IsRefreshing="{Binding IsRefreshing}"
                 Command="{Binding RefreshCommand}">
        <ScrollView>
            ...
        </ScrollView>
    </RefreshView>
 </ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...
refreshView.On<Windows>().SetRefreshPullDirection(RefreshPullDirection.LeftToRight);
```

`RefreshView.On<Windows>`Метод указывает, что данная платформа будет запускаться только на универсальная платформа Windows. `RefreshView.SetRefreshPullDirection`Метод в [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространстве имен используется для задания направления извлечения объекта `RefreshView` , при этом `RefreshPullDirection` перечисление предоставляет четыре возможных значения:

- `LeftToRight` Указывает, что операция извлечения слева направо инициирует обновление.
- `TopToBottom` Указывает, что операция извлечения сверху вниз инициирует обновление, а является направлением по умолчанию для `RefreshView` .
- `RightToLeft` Указывает, что операция извлечения справа налево инициирует обновление.
- `BottomToTop` Указывает, что операция извлечения снизу вверх инициирует обновление.

Кроме того, `GetRefreshPullDirection` метод можно использовать для возврата текущего `RefreshPullDirection` объекта `RefreshView` .

В результате заданный объект `RefreshPullDirection` применяется к `RefreshView` , чтобы задать направление извлечения, совпадающее с ориентацией прокручиваемого элемента управления, отображающего данные. На следующем снимке экрана показан объект `RefreshView` с `LeftToRight` направлением извлечения:

[![Снимок экрана Рефрешвиев с направлением извлечения слева направо в UWP](refreshview-pulldirection-images/refreshview-pulldirection.png "Рефрешвиев с направлением извлечения слева направо")](refreshview-pulldirection-images/refreshview-pulldirection-large.png#lightbox "Рефрешвиев с направлением извлечения слева направо")

> [!NOTE]
> При изменении направления запроса начальное расположение круга хода выполнения автоматически поворачивается таким образом, чтобы стрелка начиналась в соответствующем положении для направления запроса.

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ВиндовсспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)