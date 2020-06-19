---
title: 'Xamarin.FormsФигуры: линия'
description: Xamarin.FormsКласс Line можно использовать для рисования линий.
ms.prod: xamarin
ms.assetid: 384F1A72-6D3B-4FD3-BC40-E00A73A463EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8419a9880bb22980fd0f5465a813cd000838976c
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990809"
---
# <a name="xamarinforms-shapes-line"></a>Xamarin.FormsФигуры: линия

![](~/media/shared/preview.png "This API is currently pre-release")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Line`Класс является производным от `Shape` класса и может использоваться для рисования линий. Сведения о свойствах, которые `Line` класс наследует от `Shape` класса, см. в разделе [ Xamarin.Forms Shapes](index.md).

`Line` определяет следующие свойства:

- `X1`тип Double Указывает координату x начальной точки линии. Значение этого свойства по умолчанию равно 0,0.
- `X2`тип Double Указывает координату x конечной точки линии. Значение этого свойства по умолчанию равно 0,0.
- `Y1`тип Double Указывает координату y начальной точки линии. Значение этого свойства по умолчанию равно 0,0.
- `Y2`тип Double Указывает координату y конечной точки линии. Значение этого свойства по умолчанию равно 0,0.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

## <a name="create-a-line"></a>Создание линии

В следующем примере XAML показано, как нарисовать линию:

```xaml
<Line X1="40"
      Y1="0"
      X2="0"
      Y2="120"
      Stroke="DarkBlue"
      StrokeThickness="4"
      HeightRequest="120"
      WidthRequest="120" />
```

> [!NOTE]
> Установка `Fill` свойства объекта `Line` не оказывает влияния, так как линия не имеет внутренней части.

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsМногоугольник](index.md)
