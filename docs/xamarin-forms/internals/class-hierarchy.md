---
title: Xamarin.Forms Управляет иерархией классов
description: Разработчики должны быть знакомы с иерархией типов, используемых для создания пользовательского интерфейса Xamarin.Forms приложения.
ms.prod: xamarin
ms.assetid: C89E6B98-464D-4BBE-BF11-13A5FCBBF420
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ed3fc6d7aab48886f0c6166390b0ff224f66da60
ms.sourcegitcommit: 1decf2c65dc4c36513f7dd459a5df01e170a036f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98115205"
---
# <a name="no-locxamarinforms-controls-class-hierarchy"></a>Xamarin.Forms Управляет иерархией классов

Xamarin.Forms состоит из сотен типов, которые находятся в нескольких пространствах имен. Разработчики должны быть знакомы с иерархией типов, используемых для создания пользовательского интерфейса Xamarin.Forms приложения, которое находится в `Xamarin.Forms` пространстве имен.

Эти типы можно разделить на страницы, макеты, представления и ячейки. Xamarin.FormsСтраница обычно занимает весь экран, а все типы страниц являются производными от [`Page`](xref:Xamarin.Forms.Page) класса. Страницы обычно содержат макет, а все типы макетов являются производными от [`Layout`](xref:Xamarin.Forms.Layout) класса. Макет обычно содержит представления и, возможно, другие макеты, и все типы представлений в конечном итоге являются производными от [`View`](xref:Xamarin.Forms.View) класса. Наконец, ячейки представляют собой специализированные элементы управления, которые используются при отображении данных в элементах [`TableView`](xref:Xamarin.Forms.TableView) [`ListView`](xref:Xamarin.Forms.ListView) управления и. Страницы, макеты, представления и ячейки в конечном итоге являются производными от [`Element`](xref:Xamarin.Forms.Element) класса.

На следующей схеме классов показана иерархия типов, которые обычно используются для построения пользовательского интерфейса в Xamarin.Forms .

[![::: No-Loc (Xamarin. Forms)::: элементы управления схема классов](class-hierarchy-images/class-diagram.png "::: No-Loc (Xamarin. Forms)::: элементы управления схема классов")](class-hierarchy-images/class-diagram-large.png#lightbox "::: No-Loc (Xamarin. Forms)::: элементы управления схема классов")

Однако обратите внимание, что на диаграмме показан только один тип оболочки.

> [!NOTE]
> Версию схемы классов с высоким разрешением можно скачать [отсюда](class-hierarchy-images/class-diagram-high-resolution.png).

## <a name="related-links"></a>Связанные ссылки

- [Xamarin.Forms Справочник по элементам управления](~/xamarin-forms/user-interface/controls/index.md)
