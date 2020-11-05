---
title: Создание пользовательского макета в Xamarin.Forms
description: В этой статье объясняется, как написать пользовательский класс макета и демонстрируется ориентированный на ориентацию класс Враплайаут, который упорядочивает свои дочерние элементы на странице по горизонтали, а затем переносит отображение последующих дочерних элементов в дополнительные строки.
ms.prod: xamarin
ms.assetid: B0CFDB59-14E5-49E9-965A-3DCCEDAC2E31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/29/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2a7aa9ec588879eb4f59e42cf9848d6e3c560625
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373800"
---
# <a name="create-a-custom-layout-in-no-locxamarinforms"></a>Создание пользовательского макета в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-customlayout-wraplayout)

_Xamarin.Forms определяет пять классов макета — StackLayout, Абсолутелайаут, RelativeLayout, Grid и Флекслайаут, каждый из которых упорядочивает свои дочерние элементы другим способом. Однако иногда необходимо упорядочить содержимое страницы, используя макет, не предоставляемый Xamarin.Forms . В этой статье объясняется, как написать пользовательский класс макета и демонстрируется ориентированный на ориентацию класс Враплайаут, который упорядочивает свои дочерние элементы на странице по горизонтали, а затем переносит отображение последующих дочерних элементов в дополнительные строки._

В Xamarin.Forms все классы макета являются производными от [`Layout<T>`](xref:Xamarin.Forms.Layout`1) класса и ограничивают универсальный тип [`View`](xref:Xamarin.Forms.View) и его производные типы. В свою очередь, `Layout<T>` класс является производным от [`Layout`](xref:Xamarin.Forms.Layout) класса, который предоставляет механизм для позиционирования и изменения размеров дочерних элементов.

Каждый визуальный элемент отвечает за определение собственного предпочтительного размера, который называется *запрошенным* размером. [`Page`](xref:Xamarin.Forms.Page), [`Layout`](xref:Xamarin.Forms.Layout) и [`Layout<View>`](xref:Xamarin.Forms.Layout`1) производные типы отвечают за определение расположения и размера своих дочерних или дочерних элементов относительно самих. Таким образом, макет включает связь типа «родители-потомки», где родительский элемент определяет, какой размер дочернего элемента должен быть, но пытается разместить запрошенный размер дочернего элемента.

Xamarin.FormsЧтобы создать пользовательский макет, необходимо глубокое понимание циклов макета и недействительности. Сейчас будут обсуждаться эти циклы.

## <a name="layout"></a>Layout

Макет начинается в верхней части визуального дерева со страницей и проходит через все ветви визуального дерева, чтобы охватывать каждый визуальный элемент на странице. Элементы, являющиеся родительскими для других элементов, отвечают за изменение размера и размещение своих потомков относительно самих.

[`VisualElement`](xref:Xamarin.Forms.VisualElement)Класс определяет [ `Measure` ] (xref: Xamarin.Forms . Висуалелемент. Measure (System. Double, System. Double, Xamarin.Forms . Меасурефлагс)) метод, который измеряет элемент для операций макета и [ `Layout` ] (xref: Xamarin.Forms . Висуалелемент. Layout ( Xamarin.Forms . Прямоугольник)), который указывает прямоугольную область, в которой будет отображаться элемент. При запуске приложения и отображении первой страницы *цикл макета* , состоящий из первых `Measure` вызовов, а затем `Layout` вызывает метод, начинает с [`Page`](xref:Xamarin.Forms.Page) объекта:

1. Во время цикла макета каждый родительский элемент отвечает за вызов `Measure` метода для его дочерних элементов.
1. После измерения дочерних элементов каждый родительский элемент отвечает за вызов `Layout` метода для его дочерних элементов.

Этот цикл гарантирует, что каждый визуальный элемент на странице будет получать вызовы `Measure` к `Layout` методам и. Этот процесс показан на следующей схеме:

![::: No-Loc (Xamarin. Forms)::: цикл макета](custom-images/layout-cycle.png)

> [!NOTE]
> Обратите внимание, что циклы макета также могут возникать в подмножестве визуального дерева, если какое-либо изменение повлияет на макет. Сюда входят элементы, добавляемые или удаляемые из коллекции, например в [`StackLayout`](xref:Xamarin.Forms.StackLayout) , изменение [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) свойства элемента или изменение размера элемента.

Каждый Xamarin.Forms класс, у которого есть `Content` свойство или, `Children` имеет переопределяемый [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) метод. Пользовательские классы макета, производные от, [`Layout<View>`](xref:Xamarin.Forms.Layout`1) должны переопределять этот метод и гарантировать, что [ `Measure` ] (xref: Xamarin.Forms . Висуалелемент. Measure (System. Double, System. Double, Xamarin.Forms . Меасурефлагс)) и [ `Layout` ] (xref: Xamarin.Forms . Висуалелемент. Layout ( Xamarin.Forms . ). Методы вызываются для всех дочерних элементов элемента, чтобы обеспечить требуемый пользовательский макет.

Кроме того, каждый класс, производный от [`Layout`](xref:Xamarin.Forms.Layout) или, [`Layout<View>`](xref:Xamarin.Forms.Layout`1) должен переопределять [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) метод, в котором класс макета определяет размер, который он должен сделать, выполнив вызовы [ `Measure` ] (xref: Xamarin.Forms . Висуалелемент. Measure (System. Double, System. Double, Xamarin.Forms . Меасурефлагс)) дочерними методами.

> [!NOTE]
> Элементы определяют их размер на основе *ограничений* , которые указывают, сколько места доступно для элемента в родительском элементе. Ограничения, передаваемые [ `Measure` ] (xref: Xamarin.Forms . Висуалелемент. Measure (System. Double, System. Double, Xamarin.Forms . Меасурефлагс)) и [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) методы могут находиться в диапазоне от 0 до `Double.PositiveInfinity` . Элемент *ограничен или* *полностью ограничен* при получении вызова его [ `Measure` ] (xref: Xamarin.Forms . Висуалелемент. Measure (System. Double, System. Double, Xamarin.Forms . Меасурефлагс)) метод с бесконечными аргументами — элемент ограничен определенным размером. Элемент не *ограничен* или *частично ограничен* , когда он получает вызов его `Measure` метода по крайней мере с одним аргументом, равным `Double.PositiveInfinity` – бесконечное ограничение можно рассматривать как указывающее на автоматическое изменение размера.

## <a name="invalidation"></a>Недействительность

Недействительность — это процесс, с помощью которого изменение элемента на странице вызывает новый цикл макета. Элементы считаются недопустимыми, если они больше не имеют нужного размера или расположения. Например, если [`FontSize`](xref:Xamarin.Forms.Button.FontSize) свойство [`Button`](xref:Xamarin.Forms.Button) изменено, то `Button` считается недопустимым, так как он больше не будет иметь правильный размер. Изменение размера элемента `Button` может привести к изменению макета с помощью оставшейся части страницы.

Элементы становятся недействительными при вызове [`InvalidateMeasure`](xref:Xamarin.Forms.VisualElement.InvalidateMeasure) метода, как правило, когда изменяется свойство элемента, которое может привести к новому размеру элемента. Этот метод вызывает [`MeasureInvalidated`](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) событие, которое родительские дескрипторы элемента активируют новый цикл макета.

[`Layout`](xref:Xamarin.Forms.Layout)Класс задает обработчик для [`MeasureInvalidated`](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) события для каждого дочернего элемента, добавленного к его `Content` свойству или `Children` коллекции, и отсоединяет обработчик при удалении дочернего элемента. Таким образом, каждый элемент в визуальном дереве, имеющий дочерние элементы, уведомляется каждый раз, когда изменяется размер одного из его дочерних элементов. На следующей схеме показано, как изменение размера элемента в визуальном дереве может привести к изменениям, которые привели к дереву:

![Недействительность в визуальном дереве](custom-images/invalidation.png)

Однако `Layout` класс пытается ограничить влияние изменения размера дочернего элемента на макет страницы. Если размер макета ограничен, изменение размера дочернего элемента не влияет на содержимое, превышающее родительский макет в визуальном дереве. Однако обычно изменение размера макета влияет на то, как макет упорядочивает свои дочерние элементы. Таким образом, любое изменение размера макета приведет к запуску цикла макета для макета, и макет будет принимать вызовы к его [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) методам и.

[`Layout`](xref:Xamarin.Forms.Layout)Класс также определяет [`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout) метод, который имеет аналогичное назначение для [`InvalidateMeasure`](xref:Xamarin.Forms.VisualElement.InvalidateMeasure) метода. `InvalidateLayout`Метод должен вызываться при каждом изменении, которое влияет на положение макета и размеры его дочерних элементов. Например, `Layout` класс вызывает `InvalidateLayout` метод всякий раз, когда дочерний элемент добавляется в макет или удаляется из него.

[`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout)Можно переопределить, чтобы реализовать кэш для снижения числа повторяющихся вызовов [ `Measure` ] (xref: Xamarin.Forms . Висуалелемент. Measure (System. Double, System. Double, Xamarin.Forms . Меасурефлагс)) методы дочерних элементов макета. Переопределение `InvalidateLayout` метода предоставит уведомление о том, что дочерние элементы добавляются в макет или удаляются из него. Аналогичным образом, [`OnChildMeasureInvalidated`](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) метод можно переопределить, чтобы предоставить уведомление, когда изменяется размер одного из дочерних элементов макета. Для обоих переопределений методов пользовательский макет должен реагировать на очистку кэша. Дополнительные сведения см. в разделе [Вычисление и кэширование данных макета](#calculate-and-cache-layout-data).

## <a name="create-a-custom-layout"></a>Создание пользовательского макета

Процесс создания пользовательского макета выглядит следующим образом:

1. Создайте класс, производный от класса `Layout<View>`. Дополнительные сведения см. [в разделе Создание враплайаут](#create-a-wraplayout).
1. [ *необязательно* ] Добавьте свойства, которые поддерживаются связываемыми свойствами, для всех параметров, которые должны быть установлены для класса макета. Дополнительные сведения см. в разделе [Добавление свойств, поддерживаемых связываемыми свойствами](#add-properties-backed-by-bindable-properties).
1. Переопределите [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) метод для вызова [ `Measure` ] (xref: Xamarin.Forms . Висуалелемент. Measure (System. Double, System. Double, Xamarin.Forms . Меасурефлагс)) для всех дочерних элементов макета и возвращают запрошенный размер для макета. Дополнительные сведения см. [в разделе переопределение метода onmeasure](#override-the-onmeasure-method).
1. Переопределите [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) метод для вызова [ `Layout` ] (xref: Xamarin.Forms . Висуалелемент. Layout ( Xamarin.Forms . Прямоугольник)) для всех дочерних элементов макета. Не удалось вызвать [ `Layout` ] (xref: Xamarin.Forms . Висуалелемент. Layout ( Xamarin.Forms . Прямоугольник)). метод для каждого дочернего элемента в макете приведет к тому, что дочерний объект никогда не будет получать правильный размер или положение, поэтому дочерний элемент не станет видимым на странице. Дополнительные сведения см. [в разделе переопределение метода лайаутчилдрен](#override-the-layoutchildren-method).

    > [!NOTE]
    > При перечислении дочерних элементов [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) в [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) переопределениях и пропустите все дочерние элементы, [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) свойство которых имеет значение `false` . Это обеспечит, что пользовательский макет не оставляет место для невидимых дочерних элементов.

1. [ *необязательно* ] Переопределите [`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout) метод, чтобы получать уведомления при добавлении или удалении дочерних элементов в макете. Дополнительные сведения см. [в разделе переопределение метода инвалидателайаут](#override-the-invalidatelayout-method).
1. [ *необязательно* ] Переопределите [`OnChildMeasureInvalidated`](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) метод, чтобы получать уведомления при изменении размера одного из дочерних элементов макета. Дополнительные сведения см. [в разделе переопределение метода ончилдмеасуреинвалидатед](#override-the-onchildmeasureinvalidated-method).

> [!NOTE]
> Обратите внимание, что [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) Переопределение не будет вызываться, если размер макета регулируется родительским элементом, а не его потомками. Однако переопределение будет вызываться, если одно или оба ограничения имеют бесконечное значение или если класс макета имеет значения, отличные от значений по умолчанию [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) или [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) свойств. По этой причине [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) Переопределение не может полагаться на дочерние размеры, полученные во время [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) вызова метода. Вместо этого `LayoutChildren` должен вызывать [ `Measure` ] (xref: Xamarin.Forms . Висуалелемент. Measure (System. Double, System. Double, Xamarin.Forms . Меасурефлагс)) для дочерних элементов макета перед вызовом [ `Layout` ] (xref: Xamarin.Forms . Висуалелемент. Layout ( Xamarin.Forms . Прямоугольник)). Кроме того, размер дочерних элементов, полученных в `OnMeasure` переопределении, можно кэшировать, чтобы избежать более поздних `Measure` вызовов в `LayoutChildren` переопределении, но класс макета должен знать, когда размеры необходимо получить снова. Дополнительные сведения см. в разделе [Вычисление и кэширование данных макета](#calculate-and-cache-layout-data).

Затем класс макета можно использовать, добавив его в [`Page`](xref:Xamarin.Forms.Page) и добавив дочерние элементы в макет. Дополнительные сведения см. [в разделе Использование враплайаут](#consume-the-wraplayout).

### <a name="create-a-wraplayout"></a>Создание Враплайаут

Пример приложения демонстрирует ориентированный на ориентацию `WrapLayout` класс, который упорядочивает его дочерние элементы по горизонтали на странице, а затем переносит отображение последующих дочерних элементов к дополнительным строкам.

`WrapLayout`Класс выделяет одинаковый объем пространства для каждого дочернего элемента, называемого *размером ячейки* , на основе максимального размера дочерних элементов. Дочерние элементы, размер которых меньше размера ячейки, можно разместить в ячейке на основе их [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) значений свойств и.

`WrapLayout`Определение класса показано в следующем примере кода:

```csharp
public class WrapLayout : Layout<View>
{
  Dictionary<Size, LayoutData> layoutDataCache = new Dictionary<Size, LayoutData>();
  ...
}
```

#### <a name="calculate-and-cache-layout-data"></a>Вычисление и кэширование данных макета

`LayoutData`Структура хранит данные о коллекции дочерних элементов в ряде свойств:

- `VisibleChildCount` — число дочерних элементов, видимых в макете.
- `CellSize` — максимальный размер всех дочерних элементов, настраивается в соответствии с размером макета.
- `Rows` — число строк.
- `Columns` — число столбцов.

`layoutDataCache`Поле используется для хранения нескольких `LayoutData` значений. При запуске приложения `LayoutData` в словарь будут помещены два объекта `layoutDataCache` для текущей ориентации: один для аргументов ограничения `OnMeasure` переопределения, а другой — для `width` `height` аргументов `LayoutChildren` переопределения. При повороте устройства на альбомную ориентацию `OnMeasure` Переопределение и `LayoutChildren` Переопределение снова будет вызываться, что приведет к кэшированию других двух `LayoutData` объектов в словаре. Однако при возврате устройства к книжной ориентации дальнейшие вычисления не требуются, поскольку у `layoutDataCache` уже есть необходимые данные.

В следующем примере кода показан `GetLayoutData` метод, который вычисляет свойства `LayoutData` структурированного объекта на основе определенного размера:

```csharp
LayoutData GetLayoutData(double width, double height)
{
  Size size = new Size(width, height);

  // Check if cached information is available.
  if (layoutDataCache.ContainsKey(size))
  {
    return layoutDataCache[size];
  }

  int visibleChildCount = 0;
  Size maxChildSize = new Size();
  int rows = 0;
  int columns = 0;
  LayoutData layoutData = new LayoutData();

  // Enumerate through all the children.
  foreach (View child in Children)
  {
    // Skip invisible children.
    if (!child.IsVisible)
      continue;

    // Count the visible children.
    visibleChildCount++;

    // Get the child's requested size.
    SizeRequest childSizeRequest = child.Measure(Double.PositiveInfinity, Double.PositiveInfinity);

    // Accumulate the maximum child size.
    maxChildSize.Width = Math.Max(maxChildSize.Width, childSizeRequest.Request.Width);
    maxChildSize.Height = Math.Max(maxChildSize.Height, childSizeRequest.Request.Height);
  }

  if (visibleChildCount != 0)
  {
    // Calculate the number of rows and columns.
    if (Double.IsPositiveInfinity(width))
    {
      columns = visibleChildCount;
      rows = 1;
    }
    else
    {
      columns = (int)((width + ColumnSpacing) / (maxChildSize.Width + ColumnSpacing));
      columns = Math.Max(1, columns);
      rows = (visibleChildCount + columns - 1) / columns;
    }

    // Now maximize the cell size based on the layout size.
    Size cellSize = new Size();

    if (Double.IsPositiveInfinity(width))
      cellSize.Width = maxChildSize.Width;
    else
      cellSize.Width = (width - ColumnSpacing * (columns - 1)) / columns;

    if (Double.IsPositiveInfinity(height))
      cellSize.Height = maxChildSize.Height;
    else
      cellSize.Height = (height - RowSpacing * (rows - 1)) / rows;

    layoutData = new LayoutData(visibleChildCount, cellSize, rows, columns);
  }

  layoutDataCache.Add(size, layoutData);
  return layoutData;
}
```

`GetLayoutData`Метод выполняет следующие операции:

- Он определяет, что вычисляемое `LayoutData` значение уже находится в кэше, и возвращает его, если он доступен.
- В противном случае он перебирает все дочерние элементы, вызывая `Measure` метод для каждого дочернего элемента с неограниченной шириной и высотой, и определяет максимальный размер дочернего элемента.
- При условии, что имеется по крайней мере один видимый дочерний элемент, он вычисляет необходимое количество строк и столбцов, а затем вычисляет размер ячейки для дочерних элементов на основе измерений `WrapLayout` . Обратите внимание, что размер ячейки, как правило, немного шире, чем максимальный размер дочернего элемента, но он также может быть меньше, если `WrapLayout` недостаточно для самого широкого дочернего или достаточного размера для самого высокого дочернего элемента.
- Он сохраняет новое `LayoutData` значение в кэше.

#### <a name="add-properties-backed-by-bindable-properties"></a>Добавление свойств, которые поддерживаются с помощью привязки свойств

`WrapLayout`Класс определяет `ColumnSpacing` Свойства и `RowSpacing` , значения которых используются для разделения строк и столбцов в макете и для которых поддерживаются привязываемые свойства. Свойства, доступные для привязки, показаны в следующем примере кода:

```csharp
public static readonly BindableProperty ColumnSpacingProperty = BindableProperty.Create(
  "ColumnSpacing",
  typeof(double),
  typeof(WrapLayout),
  5.0,
  propertyChanged: (bindable, oldvalue, newvalue) =>
  {
    ((WrapLayout)bindable).InvalidateLayout();
  });

public static readonly BindableProperty RowSpacingProperty = BindableProperty.Create(
  "RowSpacing",
  typeof(double),
  typeof(WrapLayout),
  5.0,
  propertyChanged: (bindable, oldvalue, newvalue) =>
  {
    ((WrapLayout)bindable).InvalidateLayout();
  });
```

Обработчик изменения свойств для каждого привязываемого свойства вызывает `InvalidateLayout` Переопределение метода, чтобы активировать новый проход макета в `WrapLayout` . Дополнительные сведения см. в разделе [Переопределение метода инвалидателайаут](#override-the-invalidatelayout-method) и [Переопределение метода ончилдмеасуреинвалидатед](#override-the-onchildmeasureinvalidated-method).

#### <a name="override-the-onmeasure-method"></a>Переопределение метода onmeasure

`OnMeasure`Переопределение показано в следующем примере кода:

```csharp
protected override SizeRequest OnMeasure(double widthConstraint, double heightConstraint)
{
  LayoutData layoutData = GetLayoutData(widthConstraint, heightConstraint);
  if (layoutData.VisibleChildCount == 0)
  {
    return new SizeRequest();
  }

  Size totalSize = new Size(layoutData.CellSize.Width * layoutData.Columns + ColumnSpacing * (layoutData.Columns - 1),
                layoutData.CellSize.Height * layoutData.Rows + RowSpacing * (layoutData.Rows - 1));
  return new SizeRequest(totalSize);
}
```

Переопределение вызывает `GetLayoutData` метод и создает `SizeRequest` объект из возвращенных данных, а также учитывает `RowSpacing` `ColumnSpacing` значения свойств и. Дополнительные сведения о `GetLayoutData` методе см. в разделе [Вычисление и кэширование данных макета](#calculate-and-cache-layout-data).

> [!IMPORTANT]
> [ `Measure` ] (Xref: Xamarin.Forms . Висуалелемент. Measure (System. Double, System. Double, Xamarin.Forms . Меасурефлагс)) и [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) методы никогда не должны запрашивать бесконечное измерение, возвращая [`SizeRequest`](xref:Xamarin.Forms.SizeRequest) значение со свойством, равным `Double.PositiveInfinity` . Однако по крайней мере один из аргументов ограничения `OnMeasure` может иметь значение `Double.PositiveInfinity` .

#### <a name="override-the-layoutchildren-method"></a>Переопределение метода Лайаутчилдрен

`LayoutChildren`Переопределение показано в следующем примере кода:

```csharp
protected override void LayoutChildren(double x, double y, double width, double height)
{
  LayoutData layoutData = GetLayoutData(width, height);

  if (layoutData.VisibleChildCount == 0)
  {
    return;
  }

  double xChild = x;
  double yChild = y;
  int row = 0;
  int column = 0;

  foreach (View child in Children)
  {
    if (!child.IsVisible)
    {
      continue;
    }

    LayoutChildIntoBoundingRegion(child, new Rectangle(new Point(xChild, yChild), layoutData.CellSize));
    if (++column == layoutData.Columns)
    {
      column = 0;
      row++;
      xChild = x;
      yChild += RowSpacing + layoutData.CellSize.Height;
    }
    else
    {
      xChild += ColumnSpacing + layoutData.CellSize.Width;
    }
  }
}
```

Переопределение начинается с вызова `GetLayoutData` метода, а затем перечисляет все дочерние элементы для изменения размера и размещает их в каждой ячейке дочернего элемента. Это достигается путем вызова [ `LayoutChildIntoBoundingRegion` ] (xref: Xamarin.Forms . Layout. Лайаутчилдинтобаундингрегион ( Xamarin.Forms . Висуалелемент, Xamarin.Forms . ). Этот метод используется для размещения дочернего элемента внутри прямоугольника на основе его [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) значений свойств и. Это эквивалентно выполнению вызова [ `Layout` ] (xref: Xamarin.Forms . Висуалелемент. Layout ( Xamarin.Forms . Прямоугольник)).

> [!NOTE]
> Обратите внимание, что прямоугольник, переданный в `LayoutChildIntoBoundingRegion` метод, включает всю область, в которой может находиться дочерний элемент.

Дополнительные сведения о `GetLayoutData` методе см. в разделе [Вычисление и кэширование данных макета](#calculate-and-cache-layout-data).

#### <a name="override-the-invalidatelayout-method"></a>Переопределение метода Инвалидателайаут

[`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout)Переопределение вызывается, когда дочерние элементы добавляются в макет или удаляются из макета или когда одно из `WrapLayout` свойств изменяет значение, как показано в следующем примере кода:

```csharp
protected override void InvalidateLayout()
{
  base.InvalidateLayout();
  layoutInfoCache.Clear();
}
```

Переопределение делает недействительным макет и отменяет все кэшированные сведения о макете.

> [!NOTE]
> Чтобы предотвратить [`Layout`](xref:Xamarin.Forms.Layout) вызов класса [`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout) при каждом добавлении или удалении дочернего элемента из макета, переопределите [ `ShouldInvalidateOnChildAdded` ] (xref: Xamarin.Forms . Layout. Шаулдинвалидатеончилдаддед ( Xamarin.Forms . View)) и [ `ShouldInvalidateOnChildRemoved` ] (xref: Xamarin.Forms . Layout. Шаулдинвалидатеончилдремовед ( Xamarin.Forms . View)) методы и возвращают `false` . Затем класс макета может реализовать пользовательский процесс при добавлении или удалении дочерних элементов.

#### <a name="override-the-onchildmeasureinvalidated-method"></a>Переопределение метода Ончилдмеасуреинвалидатед

[`OnChildMeasureInvalidated`](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated)Переопределение вызывается при изменении размера одного из дочерних элементов макета и показан в следующем примере кода:

```csharp
protected override void OnChildMeasureInvalidated()
{
  base.OnChildMeasureInvalidated();
  layoutInfoCache.Clear();
}
```

Переопределение делает недействительным дочерний макет и удаляет все кэшированные сведения о макете.

### <a name="consume-the-wraplayout"></a>Использование Враплайаут

`WrapLayout`Класс можно использовать, поместив его в [`Page`](xref:Xamarin.Forms.Page) производный тип, как показано в следующем примере кода XAML:

```xaml
<ContentPage ... xmlns:local="clr-namespace:ImageWrapLayout">
    <ScrollView Margin="0,20,0,20">
        <local:WrapLayout x:Name="wrapLayout" />
    </ScrollView>
</ContentPage>
```

Ниже приведен эквивалентный код на C#:

```csharp
public class ImageWrapLayoutPageCS : ContentPage
{
  WrapLayout wrapLayout;

  public ImageWrapLayoutPageCS()
  {
    wrapLayout = new WrapLayout();

    Content = new ScrollView
    {
      Margin = new Thickness(0, 20, 0, 20),
      Content = wrapLayout
    };
  }
  ...
}
```

Потомки дочерние элементы могут быть добавлены в `WrapLayout` при необходимости. В следующем примере кода показаны [`Image`](xref:Xamarin.Forms.Image) элементы, добавляемые в `WrapLayout` :

```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    var images = await GetImageListAsync();
    if (images != null)
    {
        foreach (var photo in images.Photos)
        {
            var image = new Image
            {
                Source = ImageSource.FromUri(new Uri(photo))
            };
            wrapLayout.Children.Add(image);
        }
    }
}

async Task<ImageList> GetImageListAsync()
{
    try
    {
        string requestUri = "https://raw.githubusercontent.com/xamarin/docs-archive/master/Images/stock/small/stock.json";
        string result = await _client.GetStringAsync(requestUri);
        return JsonConvert.DeserializeObject<ImageList>(result);
    }
    catch (Exception ex)
    {
        Debug.WriteLine($"\tERROR: {ex.Message}");
    }

    return null;
}
```

Когда отображается страница, содержащая `WrapLayout` , пример приложения асинхронно обращается к удаленному JSON-файлу, содержащему список фотографий, создает [`Image`](xref:Xamarin.Forms.Image) элемент для каждой фотографии и добавляет его в `WrapLayout` . Результат показан на следующих снимках экрана.

![Пример приложения, портретные снимки экрана](custom-images/portait-screenshots.png)

На следующих снимках экрана показано, что `WrapLayout` после поворота к альбомной ориентации:

![Пример экрана с альбомной ориентацией на примере приложения iOS пример приложения на снимке экрана с альбомной ориентацией на приложение ](custom-images/landscape-ios.png)
 ![ ](custom-images/landscape-android.png)
 ![ UWP](custom-images/landscape-uwp.png)

Количество столбцов в каждой строке зависит от размера фотографии, ширины экрана и количества пикселов на устройство, независимое от устройства. [`Image`](xref:Xamarin.Forms.Image)Элементы асинхронно загружают фотографии, поэтому `WrapLayout` класс будет получать частые вызовы к своему [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) методу, так как каждый `Image` элемент получает новый размер на основе загруженной фотографии.

## <a name="related-links"></a>Связанные ссылки

- [Враплайаут (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-customlayout-wraplayout)
- [Пользовательские макеты](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter26.md)
- [Создание пользовательских макетов в Xamarin.Forms (видео)](https://www.youtube.com/watch?v=sxjOqNZFhKU)
- [Layout\<T>](xref:Xamarin.Forms.Layout`1)
- [Макет](xref:Xamarin.Forms.Layout)
- [висуалелемент](xref:Xamarin.Forms.VisualElement)