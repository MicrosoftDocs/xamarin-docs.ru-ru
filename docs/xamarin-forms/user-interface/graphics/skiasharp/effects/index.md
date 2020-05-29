---
title: ''
description: ''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d9fa710f5dfc61c2892b8fc409a39b37cf449018
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136309"
---
# <a name="skiasharp-effects"></a>Эффекты SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Класс SkiaSharp [`SKPaint`](xref:SkiaSharp.SKPaint) определяет шесть свойств, которые можно классифицировать по общему условию _влияния_. Это свойства, которые каким образом изменяют нормальное отображение графических объектов. Эффекты SkiaSharp делятся на шесть категорий:

## <a name="path-effects"></a>[Эффекты пути](../curves/effects.md)

Присвойте [`PathEffect`](xref:SkiaSharp.SKPaint.PathEffect) свойству `SKPaint` объекта типа значение [`SKPathEffect`](xref:SkiaSharp.SKPathEffect) , чтобы отображать пунктирные линии, а также обводку или заливку области с помощью шаблона, созданного из путей. Эффект пути был рассмотрен ранее в этой серии в разделе [**эффекты пути в SkiaSharp**](../curves/effects.md).

## <a name="shaders"></a>[Шейдеры](shaders/index.md)

Присвойте [`Shader`](xref:SkiaSharp.SKPaint.Shader) свойству `SKPaint` объекта типа значение [`SKShader`](xref:SkiaSharp.SKShader) , чтобы отображать линейные или круговые градиенты, мозаичные точечные рисунки и шаблоны шума Perl.

## <a name="blend-modes"></a>[Режимы смешения](blend-modes/index.md)

Установите [`BlendMode`](xref:SkiaSharp.SKPaint.BlendMode) свойство элемента `SKPaint` на элемент [`SKBlendMode`](xref:SkiaSharp.SKBlendMode) перечисления, чтобы определить, что происходит при отображении исходного изображения в назначении. SkiaSharp поддерживает все режимы компоновки и смешения CSS, в том числе режимы Портер-Дуфф, режимы наложения отделяемых и неотделяемых режимы смешения.

## <a name="mask-filters"></a>[Фильтры масок](mask-filters.md)

Задайте [`MaskFilter`](xref:SkiaSharp.SKPaint.MaskFilter) свойство `SKPaint` объекта типа [`SKMaskFilter`](xref:SkiaSharp.SKMaskFilter) для размытия и других альфа-эффектов.

## <a name="image-filters"></a>[Фильтры изображений](image-filters.md)

Задайте [`ImageFilter`](xref:SkiaSharp.SKPaint.ImageFilter) свойство `SKPaint` объекта типа [`SKImageFilter`](xref:SkiaSharp.SKImageFilter) для размытия растровых изображений и создания теней, рельефов или енгравинг эффектов.

## <a name="color-filters"></a>[Фильтры цветов](color-filters.md)

Задайте [`ColorFilter`](xref:SkiaSharp.SKPaint.ColorFilter) свойство `SKPaint` объекта типа [`SKColorFilter`](xref:SkiaSharp.SKColorFilter) , чтобы изменить цвета с помощью таблиц или преобразований матрицы.

Весь пример кода для этих статей находится в [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos). На домашней странице выберите **эффекты SkiaSharp**.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
