---
title: SkiaSharp графика в Xamarin.Forms
description: SkiaSharp — это двухмерная графическая система для .NET и C# на базе графического подсистемы СКИА с открытым кодом, которая широко используется в продуктах Google. В этом руководство объясняется, как использовать SkiaSharp для двухмерной графики в Xamarin.Forms приложениях.
ms.prod: xamarin
ms.assetid: 2C348BEA-81DF-4794-8857-EB1DFF5E11DB
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b91d8f459992d33f417e99f5d92ece63f887f691
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373436"
---
# <a name="skiasharp-graphics-in-no-locxamarinforms"></a>SkiaSharp графика в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Использование SkiaSharp для двухмерной графики в Xamarin.Forms приложениях_

SkiaSharp — это двухмерная графическая система для .NET и C# на базе графического подсистемы СКИА с открытым кодом, которая широко используется в продуктах Google. SkiaSharp можно использовать в Xamarin.Forms приложениях для рисования двухмерной векторной графики, точечных рисунков и текста.

В этом учебнике предполагается, что вы знакомы с Xamarin.Forms программированием.

> [!VIDEO https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms/player]

**Веб – семинар: SkiaSharp для Xamarin.Forms**

## <a name="skiasharp-preliminaries"></a>SkiaSharp предварительные действия

SkiaSharp для Xamarin.Forms упаковывается как пакет NuGet. После создания Xamarin.Forms решения в Visual Studio или Visual Studio для Mac можно использовать диспетчер пакетов NuGet для поиска пакета **SkiaSharp. views. Forms** и его добавления в решение. Если вы проверите раздел **References** каждого проекта после добавления SkiaSharp, вы увидите, что различные библиотеки **SkiaSharp** добавлены в каждый из проектов решения.

Если Xamarin.Forms приложение предназначено для iOS, измените файл **info. plist** , чтобы изменить минимальный целевой объект развертывания на iOS 8,0.

На любой странице C#, использующей SkiaSharp, необходимо включить `using` директиву для [`SkiaSharp`](xref:SkiaSharp) пространства имен, охватывающую все классы, структуры и перечисления SkiaSharp, которые будут использоваться в программировании графики. Кроме того, вам потребуется `using` директива для [`SkiaSharp.Views.Forms`](xref:SkiaSharp.Views.Forms) пространства имен для классов, относящихся к Xamarin.Forms . Это пространство имен намного меньше, а наиболее важным классом является [`SKCanvasView`](xref:SkiaSharp.Views.Forms.SKCanvasView) . Этот класс является производным от Xamarin.Forms `View` класса и размещает выходные данные SkiaSharp Graphics.

> [!IMPORTANT]
> `SkiaSharp.Views.Forms`Пространство имен также содержит `SKGLView` класс, производный от, `View` но для отрисовки графики использует OpenGL. В целях простоты в этом разделе не рассматривается само по себе `SKCanvasView` , но использование `SKGLView` вместо этого весьма похоже.

## <a name="skiasharp-drawing-basics"></a>[Основы рисования в SkiaSharp](basics/index.md)

Некоторые из простейших рисунков, которые можно нарисовать с помощью SkiaSharp, — это круги, овалы и прямоугольники. При отображении этих рисунков вы узнаете о SkiaSharp координатах, размерах и цветах. Отображение текста и точечных рисунков сложнее, но в этих статьях также представлены эти методы.

## <a name="skiasharp-lines-and-paths"></a>[Строки и пути SkiaSharp](paths/index.md)

Графический контур — это ряд соединенных прямых линий и кривых. Контуры могут быть обводками, заполненными или обоими. В этой статье рассматриваются различные аспекты рисования линий, включая концы штриха и объединения, а также пунктирные и пунктирные линии, но останавливаются небольшие геометрические фигуры.

## <a name="skiasharp-transforms"></a>[Преобразование SkiaSharp](transforms/index.md)

Преобразования обеспечивают единообразное преобразование, масштабирование, вращение и наклон графических объектов. В этой статье также показано, как можно использовать стандартную матрицу преобразования объемом 3 на 3 для создания неаффинных преобразований и применения преобразований к путям.

## <a name="skiasharp-curves-and-paths"></a>[Пути и кривые SkiaSharp](curves/index.md)

Изучение путей приводит к добавлению кривых к объектам пути и использованию других мощных возможностей пути. Вы увидите, как можно указать полный путь в краткой текстовой строке, как использовать эффекты пути и как изучить внутренние пути.

## <a name="skiasharp-bitmaps"></a>[Растровые изображения SkiaSharp](bitmaps/index.md)

Точечные рисунки представляют собой прямоугольные массивы битов, соответствующие пикселам устройства вывода. В этой серии статей показано, как загружать, сохранять, отображать, создавать, рисовать, анимировать и получать доступ к битам точечных рисунков SkiaSharp.

## <a name="skiasharp-effects"></a>[Эффекты SkiaSharp](effects/index.md)

Эффекты — это свойства, которые изменяют нормальное отображение графики, включая линейные и круговые градиенты, мозаичное разбиение на карты, режимы смешения, размытия и др.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [SkiaSharp с помощью веб Xamarin.Forms -семинара (видео)](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)