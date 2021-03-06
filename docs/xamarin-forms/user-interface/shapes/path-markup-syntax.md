---
title: 'Xamarin.Forms Фигуры: синтаксис разметки пути'
description: Xamarin.Forms Синтаксис разметки пути позволяет сжимать геометрические контуры в XAML.
ms.prod: xamarin
ms.assetid: A2C1BD59-1A16-4E26-A825-0338E2AF9E65
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/13/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 542d06970cb414b7590cd13757b4e4902af3dd7f
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2021
ms.locfileid: "97939086"
---
# <a name="no-locxamarinforms-shapes-path-markup-syntax"></a>Xamarin.Forms Фигуры: синтаксис разметки пути

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Xamarin.Forms синтаксис разметки пути позволяет сжимать геометрические контуры в XAML. Синтаксис задается в виде строкового значения для `Path.Data` Свойства:

```xaml
<Path Stroke="Black"
      Data="M13.908992,16.207977 L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983Z" />
```

Синтаксис разметки пути состоит из необязательного `FillRule` значения и одного или нескольких описаний рисунков. Этот синтаксис можно выразить следующим образом: `<Path Data="` *[fillRule]* *фигуредескриптион* *[фигуредескриптион]* * `" ... />`

В этом синтаксисе:

- *fillRule* является необязательным `Xamarin.Forms.Shapes.FillRule` , который указывает, должна ли геометрия использовать `EvenOdd` или `Nonzero` `FillRule` . `F0` используется для указания `EvenOdd` правила заполнения, а `F1` используется для указания `Nonzero` правила заполнения. Дополнительные сведения о правилах заливки см. в разделе [ Xamarin.Forms фигуры: правила заливки](fillrules.md).
- *фигуредескриптион* представляет рисунок, состоящий из команды Move, рисования команд и необязательной команды Close. Команда перемещения задает начальную точку фигуры. Команды рисования описывают содержимое рисунка, а Необязательная команда Close закрывает рисунок.

В приведенном выше примере синтаксис разметки пути задает начальную точку с помощью команды Move ( `M` ), ряд прямых линий с помощью команды line ( `L` ) и закрывает путь с помощью команды Close ( `Z` ).

В синтаксисе разметки пути пробелы не требуются перед командами или после них. Кроме того, два числа не обязательно должны быть разделены запятыми или пробелами, но это может быть достигнуто только в том случае, если строка является однозначной.

> [!TIP]
> Синтаксис разметки пути совместим с определениями пути к графическому рисунку (SVG), поэтому он может быть полезен для переноса графики из формата SVG.

Хотя синтаксис разметки пути предназначен для использования в XAML, его можно преобразовать в `Geometry` объект в коде, вызвав `ConvertFromInvariantString` метод в `PathGeometryConverter` классе:

```csharp
Geometry pathData = (Geometry)new PathGeometryConverter().ConvertFromInvariantString("M13.908992,16.207977 L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983Z");
```

## <a name="move-command"></a>Команда перемещения

Команда Move указывает начальную точку новой фигуры. Для этой команды используется следующий синтаксис: `M` *StartPoint* или `m` *StartPoint*.

В этом синтаксисе *StartPoint* — это [`Point`](xref:Xamarin.Forms.Point) структура, указывающая начальную точку новой фигуры. Если после команды move перечислено несколько точек, то линия будет отображаться на этих точках.

`M 10,10` Пример допустимой команды Move.

## <a name="draw-commands"></a>Команды рисования

Команда рисования может состоять из нескольких команд формы. Доступны следующие команды рисования:

- Line ( `L` или `l` ).
- Горизонтальная линия ( `H` или `h` ).
- Вертикальная линия ( `V` или `v` ).
- Эллиптическая дуга ( `A` или `a` ).
- Кривая Безье третьего порядка ( `C` или `c` ).
- Кривая Безье квадратичных кривых ( `Q` или `q` ).
- Гладкая кривая Безье третьего порядка ( `S` или `s` ).
- Гладкая кривая Безье квадратичных кривых ( `T` или `t` ).

Каждая команда Draw указывается с буквой без учета регистра. При последовательном вводе нескольких команд одного типа можно опустить повторяющуюся команду. Например `L 100,200 300,400` , эквивалентно `L 100,200 L 300,400` .

### <a name="line-command"></a>Команда линии

Команда line создает прямую линию между текущей точкой и указанной конечной точкой. Для этой команды используется следующий синтаксис: `L` *Endpoint* или `l` *Endpoint*.

В этом синтаксисе *Конечная точка* [`Point`](xref:Xamarin.Forms.Point) представляет собой конечную точку линии.

`L 20,30` и `L 20 30` являются примерами допустимых команд line.

Сведения о создании прямой линии в виде `PathGeometry` объекта см. в разделе [Создание LineSegment](geometries.md#create-a-linesegment).

### <a name="horizontal-line-command"></a>Команда горизонтальной линии

Команда горизонтальной линии создает горизонтальную линию между текущей точкой и заданной координатой x. Для этой команды используется следующий синтаксис: `H` *x* или `h` *x*.

В этом синтаксисе *x* — это `double` , представляющее координату x конечной точки линии.

`H 90` — пример допустимой команды рисования горизонтальной линии.

### <a name="vertical-line-command"></a>Команда вертикальной линии

Команда вертикальная линия создает вертикальную линию между текущей точкой и заданной координатой y. Для этой команды используется следующий синтаксис: `V` *y* или `v` *y*.

В этом синтаксисе *y* представляет собой `double` координату y конечной точки линии.

`V 90` — пример допустимой команды рисования вертикальной линии.

### <a name="elliptical-arc-command"></a>Команда дуги эллипса

Команда эллиптической дуги Создает эллиптическую дугу между текущей точкой и заданной конечной точкой. Для этой команды используется следующий синтаксис: `A` *size* *ротатионангле* *исларжеаркфлаг* *свипдиректионфлаг* *Конечная точка* или `a` *Размер* *ротатионангле* *исларжеаркфлаг* *свипдиректионфлаг* *Конечная точка*.

В этом синтаксисе:

- `size` — Это объект [`Size`](xref:Xamarin.Forms.Size) , представляющий радиус координат x и y для дуги.
- `rotationAngle` значение типа `double` , представляющее поворот эллипса (в градусах).
- `isLargeArcFlag` должно иметь значение 1, если угол дуги должен быть 180 градусов или выше, в противном случае — значение 0.
- `sweepDirectionFlag` должно иметь значение 1, если дуга рисуется в положительном направлении, в противном случае — значение 0.
- `endPoint` Объект, [`Point`](xref:Xamarin.Forms.Point) с которым рисуется дуга.

`A 150,150 0 1,0 150,-150` — Пример допустимой команды эллиптической дуги.

Сведения о создании эллиптической дуги в качестве `PathGeometry` объекта см. в разделе [Создание ArcSegment](geometries.md#create-an-arcsegment).

### <a name="cubic-bezier-curve-command"></a>Команда кривой Безье третьего порядка

Команда кривой Безье третьего порядка создает кривую Безье третьего порядка между текущей точкой и заданной конечной точкой с помощью двух заданной контрольной точки. Для этой команды используется следующий синтаксис: `C` *ControlPoint1* *ControlPoint2* *Endpoint* или `c` *ControlPoint1* *ControlPoint2* *Endpoint*.

В этом синтаксисе:

- *ControlPoint1* — это объект [`Point`](xref:Xamarin.Forms.Point) , представляющий первую контрольную точку кривой, которая определяет начальный тангенс кривой.
- *ControlPoint2* — это объект [`Point`](xref:Xamarin.Forms.Point) , представляющий вторую контрольную точку кривой, которая определяет конечный тангенс кривой.
- *Конечная точка* — это объект [`Point`](xref:Xamarin.Forms.Point) , представляющий точку, в которую рисуется кривая.

`C 100,200 200,400 300,200` — Пример допустимой команды кривой Безье третьего порядка.

Сведения о создании кривой Безье третьего порядка в виде `PathGeometry` объекта см. в разделе [Создание BezierSegment](geometries.md#create-a-beziersegment).

### <a name="quadratic-bezier-curve-command"></a>Команда кривой Безье квадратичных кривых

Команда кривой Безье второго типа создает кривую Безье квадратичной кривой между текущей точкой и заданной конечной точкой с помощью заданной контрольной точки. Для этой команды используется следующий синтаксис: `Q` *контролпоинт* *Endpoint* или `q` *контролпоинт* *Endpoint*.

В этом синтаксисе:

- *контролпоинт* — это объект [`Point`](xref:Xamarin.Forms.Point) , представляющий контрольную точку кривой, которая определяет начальные и конечные тангенс кривой.
- *Конечная точка* — это объект [`Point`](xref:Xamarin.Forms.Point) , представляющий точку, в которую рисуется кривая.

`Q 100,200 300,200` — пример допустимой команды рисования кривой Безье второго порядка.

Сведения о создании кривой Безье квадратичных кривых в виде `PathGeometry` объекта см. в разделе [Создание QuadraticBezierSegment](geometries.md#create-a-quadraticbeziersegment).

### <a name="smooth-cubic-bezier-curve-command"></a>Команда гладкой кривой Безье третьего порядка

Команда гладкая кривая Безье третьего порядка создает кривую Безье третьего порядка между текущей точкой и заданной конечной точкой с помощью заданной контрольной точки. Для этой команды используется следующий синтаксис: `S` *ControlPoint2* *Endpoint* или `s` *ControlPoint2* *Endpoint*.  

В этом синтаксисе:

- *ControlPoint2* — это объект [`Point`](xref:Xamarin.Forms.Point) , представляющий вторую контрольную точку кривой, которая определяет конечный тангенс кривой.
- *Конечная точка* — это объект [`Point`](xref:Xamarin.Forms.Point) , представляющий точку, в которую рисуется кривая.

Предполагается, что Первая контрольная точка является отражением второй контрольной точки предыдущей команды относительно текущей точки. Если предыдущая команда отсутствует или Предыдущая команда не была командой кривой Безье третьего порядка или команды плавной кривой Безье, Первая контрольная точка считается коинЦидент текущей точкой.

`S 100,200 200,300` — Пример допустимой команды гладкой кривой Безье третьего порядка.

### <a name="smooth-quadratic-bezier-curve-command"></a>Команда Smooth Безье квадратичной кривой

Команда Smooth Безье квадратичной кривой создает кривую Безье квадратичной кривой между текущей точкой и заданной конечной точкой с помощью контрольной точки. Для этой команды используется следующий синтаксис: `T` *Endpoint* или `t` *Endpoint*.

В этом синтаксисе *Конечная точка* [`Point`](xref:Xamarin.Forms.Point) представляет собой точку, в которую рисуется кривая.

Предполагается, что контрольная точка является отражением контрольной точки предыдущей команды относительно текущей точки. Если предыдущая команда отсутствует или Предыдущая команда не была кривой Безье второго или гладкой кривой Безье, то предполагается, что контрольная точка коинЦидент с текущей точкой.

`T 100,30` — Пример допустимой команды гладкой кривой Безье третьего порядка.

## <a name="close-command"></a>Команда закрытия

Команда Close завершает текущую фигуру и создает линию, соединяющую текущую точку с начальной точкой фигуры. Поэтому эта команда создает соединение линий между последним сегментом и первым сегментом фигуры.

Синтаксис команды Close: `Z` или `z` .

## <a name="additional-values"></a>Дополнительные значения

Вместо стандартного числового значения можно также использовать следующие специальные значения, учитывающие регистр:

- `Infinity` представляет `double.PositiveInfinity` .
- `-Infinity`представляет `double.NegativeInfinity` .
- `NaN` представляет `double.NaN` .

Кроме того, вы также можете использовать экспоненциальное представление без учета регистра. Таким образом, `+1.e17` является допустимым значением.

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms Фигуры: геометрические объекты](geometries.md)
- [Xamarin.Forms Фигуры: правила заливки](fillrules.md)
