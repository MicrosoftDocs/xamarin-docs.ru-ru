---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 46a1b4d00b9eea276b9a3b3d5bffbdac3d31e0ef
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136582"
---
# <a name="refreshview-pull-direction-on-windows"></a>Направление движения RefreshView в Windows

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

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

- `LeftToRight`Указывает, что операция извлечения слева направо инициирует обновление.
- `TopToBottom`Указывает, что операция извлечения сверху вниз инициирует обновление, а является направлением по умолчанию для `RefreshView` .
- `RightToLeft`Указывает, что операция извлечения справа налево инициирует обновление.
- `BottomToTop`Указывает, что операция извлечения снизу вверх инициирует обновление.

Кроме того, `GetRefreshPullDirection` метод можно использовать для возврата текущего `RefreshPullDirection` объекта `RefreshView` .

В результате заданный объект `RefreshPullDirection` применяется к `RefreshView` , чтобы задать направление извлечения, совпадающее с ориентацией прокручиваемого элемента управления, отображающего данные. На следующем снимке экрана показан объект `RefreshView` с `LeftToRight` направлением извлечения:

[![Снимок экрана Рефрешвиев с направлением извлечения слева направо в UWP](refreshview-pulldirection-images/refreshview-pulldirection.png "Рефрешвиев с направлением извлечения слева направо")](refreshview-pulldirection-images/refreshview-pulldirection-large.png#lightbox "Рефрешвиев с направлением извлечения слева направо")

> [!NOTE]
> При изменении направления запроса начальное расположение круга хода выполнения автоматически поворачивается таким образом, чтобы стрелка начиналась в соответствующем положении для направления запроса.

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ВиндовсспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
