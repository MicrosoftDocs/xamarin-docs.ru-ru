---
title: Справочник по элементам управления
description: 'Описание всех элементов пользовательского интерфейса, используемых для создания :::no-loc(Xamarin.Forms)::: приложения. В этой статье перечислены группы элементов управления, которые составляют пользовательский интерфейс :::no-loc(Xamarin.Forms)::: приложения.'
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2019
no-loc:
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: b9e3f8b61ebfc73a26a967b83f60e005a652563f
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997382"
---
# <a name="controls-reference"></a>Справочник по элементам управления

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

Пользовательский интерфейс :::no-loc(Xamarin.Forms)::: приложения состоит из объектов, которые сопоставляются с собственными элементами управления каждой целевой платформы. Это позволяет приложениям для конкретных платформ для iOS, Android и универсальная платформа Windows использовать :::no-loc(Xamarin.Forms)::: код, содержащийся в [библиотеке .NET Standard](~/cross-platform/app-fundamentals/net-standard.md).

Ниже приведены четыре основные группы управления, используемые для создания пользовательского интерфейса :::no-loc(Xamarin.Forms)::: приложения.

- [**См**](pages.md)
- [**Макеты**](layouts.md)
- [**Представления**](views.md)
- [**Ячейки**](cells.md)

:::no-loc(Xamarin.Forms):::Страница обычно занимает весь экран. Страница обычно содержит макет, который содержит представления и, возможно, другие макеты. Ячейки — это специализированные компоненты, используемые в соединении с [`TableView`](xref::::no-loc(Xamarin.Forms):::.TableView) и [`ListView`](xref::::no-loc(Xamarin.Forms):::.ListView) . Схема классов, в которой показана иерархия типов, которые обычно используются для построения пользовательского интерфейса в, :::no-loc(Xamarin.Forms)::: можно найти в [ :::no-loc(Xamarin.Forms)::: иерархии классов элементов управления](~/xamarin-forms/internals/class-hierarchy.md).

В четырех статьях о [**страницах**](pages.md), [**макетах**](layouts.md), [**представлениях**](views.md)и [**ячейках**](cells.md)каждый тип элемента управления описывается ссылками на документацию по API, статья, описывающая ее использование (если она существует), и один или несколько примеров программ (если они существуют). Каждый тип элемента управления также сопровождается снимком экрана, показывающим страницу из примера [**формсгаллери**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) , работающего на устройствах iOS и Android. Под каждым снимком экрана расположены ссылки на исходный код страницы C#, эквивалентную страницу XAML и (при необходимости) файл кода программной части C# для страницы XAML.

> [!NOTE]
> Страницы, макеты и представления являются производными от `VisualElement` класса. `VisualElement`Класс предоставляет разнообразные свойства, методы и события, которые полезны при создании производных классов. Дополнительные сведения см. в разделе [висуалелемент Properties, Methods and Events](common-properties.md).

Помимо элементов управления, предоставляемых с :::no-loc(Xamarin.Forms)::: , доступны сторонние элементы управления. Дополнительные сведения см. в разделе [сторонние элементы управления](thirdparty.md).

## <a name="related-links"></a>Связанные ссылки

- [:::no-loc(Xamarin.Forms):::Пример Формсгаллери](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [:::no-loc(Xamarin.Forms):::Управляет иерархией классов](~/xamarin-forms/internals/class-hierarchy.md)
- [Документация по API](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
