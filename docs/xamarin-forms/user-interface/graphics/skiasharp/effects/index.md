---
title: Эффекты SkiaSharp
description: Узнайте, как изменить нормальное отображение графики, используя градиенты, мозаичное разбиение на карты, режимы наложения, размытие и другие эффекты.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: B3E06572-8E2A-49FA-90D1-444C394CD516
author: davidbritch
ms.author: dabritch
ms.date: 08/22/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c6179f94b43f12a7bf4b91a05702c0539f3c8658
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93367794"
---
# <a name="skiasharp-effects"></a>Эффекты SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Класс SkiaSharp [`SKPaint`](xref:SkiaSharp.SKPaint) определяет шесть свойств, которые можно классифицировать по общему условию _влияния_. Это свойства, которые каким образом изменяют нормальное отображение графических объектов. Эффекты SkiaSharp делятся на шесть категорий:

## <a name="path-effects"></a>[Эффекты пути](../curves/effects.md)

Присвойте [`PathEffect`](xref:SkiaSharp.SKPaint.PathEffect) свойству `SKPaint` объекта типа значение [`SKPathEffect`](xref:SkiaSharp.SKPathEffect) , чтобы отображать пунктирные линии, а также обводку или заливку области с помощью шаблона, созданного из путей. Эффект пути был рассмотрен ранее в этой серии в разделе [**эффекты пути в SkiaSharp**](../curves/effects.md).

## <a name="shaders"></a>[Шейдеры](shaders/index.md)

Присвойте [`Shader`](xref:SkiaSharp.SKPaint.Shader) свойству `SKPaint` объекта типа значение [`SKShader`](xref:SkiaSharp.SKShader) , чтобы отображать линейные или круговые градиенты, мозаичные точечные рисунки и шаблоны шума Perl.

## <a name="blend-modes"></a>[Режимы смешения](blend-modes/index.md)

Установите [`BlendMode`](xref:SkiaSharp.SKPaint.BlendMode) свойство элемента `SKPaint` на элемент [`SKBlendMode`](xref:SkiaSharp.SKBlendMode) перечисления, чтобы определить, что происходит при отображении исходного изображения в назначении. SkiaSharp поддерживает все режимы компоновки и смешения CSS, в том числе режимы Porter-Duff, режимы наложения отделяемых и режимы смешения, отличные от отделяемых.

## <a name="mask-filters"></a>[Фильтры масок](mask-filters.md)

Задайте [`MaskFilter`](xref:SkiaSharp.SKPaint.MaskFilter) свойство `SKPaint` объекта типа [`SKMaskFilter`](xref:SkiaSharp.SKMaskFilter) для размытия и других альфа-эффектов.

## <a name="image-filters"></a>[Фильтры изображений](image-filters.md)

Задайте [`ImageFilter`](xref:SkiaSharp.SKPaint.ImageFilter) свойство `SKPaint` объекта типа [`SKImageFilter`](xref:SkiaSharp.SKImageFilter) для размытия растровых изображений и создания теней, рельефов или енгравинг эффектов.

## <a name="color-filters"></a>[Фильтры цветов](color-filters.md)

Задайте [`ColorFilter`](xref:SkiaSharp.SKPaint.ColorFilter) свойство `SKPaint` объекта типа [`SKColorFilter`](xref:SkiaSharp.SKColorFilter) , чтобы изменить цвета с помощью таблиц или преобразований матрицы.

Весь пример кода для этих статей находится в [**скиашарпформсдемос**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos). На домашней странице выберите **эффекты SkiaSharp**.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)