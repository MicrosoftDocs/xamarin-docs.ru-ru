---
title: Шаблоны данных Xamarin.Forms
description: Шаблон данных используется для определения внешнего вида данных в поддерживаемых элементах управления и обычно привязывается к данным для отображения.
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 682ed3b0d0f54617a0238dcc1bbf413e1ee0156a
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373111"
---
# <a name="no-locxamarinforms-data-templates"></a>Шаблоны данных Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/templates-datatemplates)

_Шаблон данных используется для определения внешнего вида данных в поддерживаемых элементах управления и обычно привязывается к данным для отображения._

## <a name="introduction"></a>[Введение](introduction.md)

Шаблоны данных Xamarin.Forms дают возможность настраивать представление данных в поддерживаемых элементах управления. В этой статье описываются шаблоны данных и объясняется их необходимость.

## <a name="creating-a-datatemplate"></a>[Создание шаблона данных](creating.md)

Шаблоны данных могут создаваться как встроенные, в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), или из пользовательского типа или соответствующего типа ячейки Xamarin.Forms. Встроенный шаблон следует использовать, если нет необходимости повторно использовать шаблон данных в другом месте. Шаблон данных можно использовать повторно, определив его как пользовательский тип или как ресурс на уровне элемента управления, страницы или приложения.

## <a name="creating-a-datatemplateselector"></a>[Создание DataTemplateSelector](selector.md)

Объект [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) можно использовать для выбора [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) в среде выполнения на основе значения свойства, привязанного к данным. Это позволяет применять несколько экземпляров `DataTemplate` к одному типу объектов, чтобы настроить внешний вид определенных объектов. В этой статье демонстрируется, как создать и использовать `DataTemplateSelector`.

## <a name="related-links"></a>Связанные ссылки

- [Шаблоны данных (пример)](/samples/xamarin/xamarin-forms-samples/templates-datatemplates)