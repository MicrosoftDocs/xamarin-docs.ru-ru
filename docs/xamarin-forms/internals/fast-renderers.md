---
title: Xamarin.Forms Быстрые модули подготовки отчетов
description: В этой статье представлены быстрые модули подготовки отчетов, которые уменьшают затраты на инфляцию и отрисовку Xamarin.Forms элемента управления в Android путем преобразования итоговой иерархии собственных элементов управления.
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c9d3b023acc3c38bf1ad056a140145680fbbba10
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2021
ms.locfileid: "97939801"
---
# <a name="no-locxamarinforms-fast-renderers"></a>Xamarin.Forms Быстрые модули подготовки отчетов

Обычно большинство исходных модулей подготовки элементов управления в Android состоят из двух представлений:

- Собственный элемент управления, например `Button` или `TextView` .
- Контейнер `ViewGroup` , который обрабатывает некоторую работу макета, обработку жестов и другие задачи.

Однако этот подход приводит к снижению производительности в том, что для каждого логического элемента управления создается два представления, что приводит к более сложному визуальному дереву, которое требует больше памяти и больше обрабатывается на экране.

Быстрые модули подготовки к просмотру уменьшают расходы на инфляцию и отрисовку Xamarin.Forms элемента управления в единое представление. Таким образом, вместо создания двух представлений и добавления их в дерево представлений создается только одно из них. Это повышает производительность за счет создания меньшего числа объектов, который, в свою очередь, означает менее сложное дерево представления и меньшее использование памяти (что также приводит к меньшему количеству приостановленных операций сборки мусора).

Быстрые модули подготовки отчетов доступны для следующих элементов управления в Xamarin.Forms Android:

- [`Button`](xref:Xamarin.Forms.Button)
- [`Frame`](xref:Xamarin.Forms.Frame)
- [`Image`](xref:Xamarin.Forms.Image)
- [`Label`](xref:Xamarin.Forms.Label)

Функционально, эти быстрые модули подготовки не отличаются от стандартных модулей подготовки отчетов. Начиная с Xamarin.Forms 4,0, все приложения, нацеленные на, `FormsAppCompatActivity` будут использовать эти быстрые модули подготовки отчетов по умолчанию. Модули подготовки отчетов для всех новых элементов управления, включая [`ImageButton`](xref:Xamarin.Forms.ImageButton) и [`CollectionView`](xref:Xamarin.Forms.CollectionView) , используют метод быстрой визуализации.

Повышение производительности при использовании быстрых модулей подготовки отчетов будет различным для каждого приложения в зависимости от сложности макета. Например, улучшение производительности в x2 возможно при прокрутке по [`ListView`](xref:Xamarin.Forms.ListView) содержащимся тысячам строк данных, где ячейки в каждой строке состоят из элементов управления, использующих быстрые модули подготовки, что приводит к заметному плавным прокрутке.

> [!NOTE]
> Пользовательские модули подготовки отчетов можно создавать для быстрых модулей подготовки отчетов, используя тот же подход, который используется для модулей подготовки отчетов прежних версий. Дополнительные сведения см. в статье [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## <a name="backwards-compatibility"></a>Обратная совместимость

Быстрые модули подготовки отчетов могут быть переопределены следующими подходами:

1. Включение модулей подготовки к просмотру прежних версий путем добавления следующей строки кода в `MainActivity` класс перед вызовом `Forms.Init` :

    ```csharp
    Forms.SetFlags("UseLegacyRenderers");
    ```

1. Использование пользовательских модулей подготовки отчетов, предназначенных для устаревших модулей подготовки отчетов. Любые существующие пользовательские модули подготовки отчетов продолжат работать с устаревшими модулями подготовки отчетов.
1. Указание другого `View.Visual` , например `Material` , с использованием различных модулей подготовки отчетов. Дополнительные сведения о визуальном элементе Material см. в статье [ Xamarin.Forms визуальный элемент Material](~/xamarin-forms/user-interface/visual/material-visual.md).

## <a name="related-links"></a>Связанные ссылки

- [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
