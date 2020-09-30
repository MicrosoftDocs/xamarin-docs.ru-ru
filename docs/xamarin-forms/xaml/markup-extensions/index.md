---
title: Расширения разметки XAML
description: В этой статье объясняется, как использовать Xamarin.Forms расширения разметки XAML для расширения возможностей и гибкости XAML, разрешая установку атрибутов элементов из источников, отличных от текстовых строк.
ms.prod: xamarin
ms.assetid: EB06C8B7-3FD5-47B7-A09C-A13063BD110F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 98eb35697aee9022a837a7b0b531edb0c53b2239
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562149"
---
# <a name="xaml-markup-extensions"></a>Расширения разметки XAML

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

Расширения разметки XAML помогают расширить возможности и гибкость XAML, разрешая установку атрибутов элементов из источников, отличных от текстовых строк.

Например, обычно свойство имеет значение `Color` `BoxView` следующим образом:

```xaml
<BoxView Color="Blue" />
```

Можно также задать шестнадцатеричное значение цвета RGB:

```xaml
<BoxView Color="#FF0080" />
```

В любом случае текстовая строка, заданная `Color` атрибутом, преобразуется в `Color` значение, определяемое [`ColorTypeConverter`](xref:Xamarin.Forms.ColorTypeConverter) классом.

Вместо этого можно задать `Color` атрибут из значения, хранящегося в словаре ресурсов, или из значения статического свойства класса, который вы создали, либо из свойства типа `Color` другого элемента на странице или из отдельного значения оттенка, насыщенности и яркости.

Все эти параметры можно использовать с помощью расширений разметки XAML. Но не дайте фразе "расширения разметки" пугают: расширения разметки XAML *не* являются расширениями для XML. Даже при использовании расширений разметки XAML XAML всегда является допустимым XML.

Расширение разметки — это, по сути, другой способ выразить атрибут элемента. Расширения разметки XAML обычно идентифицируются с помощью параметра атрибута, заключенного в фигурные скобки:

```xaml
<BoxView Color="{StaticResource themeColor}" />
```

Любой параметр атрибута в фигурных скобках *всегда* является расширением разметки XAML. Однако, как вы увидите, на расширения разметки XAML также можно ссылаться без использования фигурных скобок.

Эта статья состоит из двух частей:

## <a name="consuming-xaml-markup-extensions"></a>[Использование расширений разметки XAML](consuming.md)  

Используйте расширения разметки XAML, определенные в Xamarin.Forms .

## <a name="creating-xaml-markup-extensions"></a>[Создание расширений разметки XAML](creating.md)

Напишите собственные пользовательские расширения разметки XAML.

## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки (пример)](/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [Глава о расширениях разметки XAML из Xamarin.Forms книги](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Динамические стили](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Привязка данных](~/xamarin-forms/app-fundamentals/data-binding/index.md)