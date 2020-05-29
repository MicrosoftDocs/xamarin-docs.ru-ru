---
title: Xamarin.FormsУправляет иерархией классов
description: Разработчики должны быть знакомы с иерархией типов, используемых для создания пользовательского интерфейса Xamarin.Forms приложения.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0087e2bb81c7c9204a782519a9eeb9891adc297a
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138647"
---
# <a name="xamarinforms-controls-class-hierarchy"></a>Xamarin.FormsУправляет иерархией классов

Xamarin.Formsсостоит из сотен типов, которые находятся в нескольких пространствах имен. Разработчики должны быть знакомы с иерархией типов, используемых для создания пользовательского интерфейса Xamarin.Forms приложения, которое находится в `Xamarin.Forms` пространстве имен.

Эти типы можно разделить на страницы, макеты, представления и ячейки. Xamarin.FormsСтраница обычно занимает весь экран, а все типы страниц являются производными от [`Page`](xref:Xamarin.Forms.Page) класса. Страницы обычно содержат макет, а все типы макетов являются производными от [`Layout`](xref:Xamarin.Forms.Layout) класса. Макет обычно содержит представления и, возможно, другие макеты, и все типы представлений в конечном итоге являются производными от [`View`](xref:Xamarin.Forms.View) класса. Наконец, ячейки представляют собой специализированные элементы управления, которые используются при отображении данных в элементах [`TableView`](xref:Xamarin.Forms.TableView) [`ListView`](xref:Xamarin.Forms.ListView) управления и. Страницы, макеты, представления и ячейки в конечном итоге являются производными от [`Element`](xref:Xamarin.Forms.Element) класса.

На следующей схеме классов показана иерархия типов, которые обычно используются для построения пользовательского интерфейса в Xamarin.Forms .

[![Xamarin.FormsДиаграмма классов элементов управления](class-hierarchy-images/class-diagram.png "[! Операцион. НЕТ-LOC (Xamarin. Forms)] элементы управления схема классов")](class-hierarchy-images/class-diagram-large.png#lightbox "[! Операцион. НЕТ-LOC (Xamarin. Forms)] элементы управления схема классов")

> [!NOTE]
> Версию схемы классов с высоким разрешением можно скачать [отсюда](class-hierarchy-images/class-diagram-high-resolution.png). Однако обратите внимание, что на диаграмме показан только один тип оболочки.

## <a name="related-links"></a>Связанные ссылки

- [Xamarin.FormsСправочник по элементам управления](~/xamarin-forms/user-interface/controls/index.md)
