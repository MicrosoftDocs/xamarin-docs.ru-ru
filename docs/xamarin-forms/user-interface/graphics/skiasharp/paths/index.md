---
title: Строки и пути SkiaSharp
description: В этой статье объясняется, как использовать SkiaSharp для рисования линий и графических путей в Xamarin.Forms приложениях, а также демонстрируется пример кода.
ms.prod: xamarin
ms.assetid: 316A15FE-383D-4D06-8641-BAC7EE7474CA
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1c5a83a626b9a0fab55b84c7d8cc8e4fbaea3683
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93375256"
---
# <a name="skiasharp-lines-and-paths"></a>Строки и пути SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Использование SkiaSharp для рисования линий и графических контуров_

В [предыдущем разделе](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md) было продемонстрировано, что `SKCanvas` класс SkiaSharp включает несколько методов рисования кругов, овалов, прямоугольников, скругленных прямоугольников, текста и точечных рисунков. В этом разделе и последующих разделах рассматриваются различные классы, связанные с созданием и визуализацией *графических путей*.

Графический контур — это наиболее обобщенный подход к рисованию линий и кривых в SkiaSharp. В этом разделе описывается использование [`SKPath`](xref:SkiaSharp.SKPath) объекта для рисования прямых линий и использование набора маленьких прямых линий (которые называются *ломаными* линиями) для рисования кривых, которые можно определить алгоритмически. В следующем разделе, посвященном [**SkiaSharp кривых и путям**](../curves/index.md) , обсуждаются различные виды кривых, поддерживаемые `SKPath` .

Все примеры программ в этом разделе отображаются под **строками заголовка и путями** на домашней странице программы [**скиашарпформсдемос**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) и в папке [**paths**](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths) этого решения.

## <a name="lines-and-stroke-caps"></a>[Линии и концы штрихов](lines.md)

Узнайте, как использовать SkiaSharp для рисования линий с различными наконечниками штриха.

## <a name="path-basics"></a>[Основные сведения о пути](paths.md)

Изучите `SKPath` объект SkiaSharp для объединения линий и кривых.

## <a name="the-path-fill-types"></a>[Типы заполнения пути](fill-types.md)

Узнайте о различных эффектах, возможных для типов заливки пути SkiaSharp.

## <a name="polylines-and-parametric-equations"></a>[Ломаные линии и параметрические уравнения](polylines.md)

Используйте SkiaSharp для отображения любой линии, которую можно определить с помощью параметрической формулы.

## <a name="dots-and-dashes"></a>[Точки и тире](dots.md)

Основные сложности рисования пунктирных и пунктирных линий в SkiaSharp.

## <a name="finger-painting"></a>[Рисование пальцами](finger-paint.md)

Используйте пальцы для рисования на холсте.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)