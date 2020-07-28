---
title: Навигация в Xamarin.Forms
description: В этом руководстве содержатся сведения о переходах в приложениях Xamarin.Forms. Xamarin.Forms предоставляет ряд различных способов перехода по страницам в зависимости от используемого типа страницы.
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 65aa0f060e4d48834017a334d69b2f21645825f3
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937193"
---
# <a name="xamarinforms-navigation"></a>Навигация в Xamarin.Forms

_Xamarin.Forms предоставляет ряд различных способов перехода по страницам в зависимости от используемого типа страницы._

Типы страниц ![Xamarin.Forms](images/page-types.png)

Кроме того, приложения оболочки Xamarin.Forms предоставляют улучшенные возможности навигации на основе URI по интерфейсу без соблюдения строгой заданной иерархии. Дополнительные сведения см. в разделе [Навигация по оболочке Xamarin.Forms](~/xamarin-forms/app-fundamentals/shell/navigation.md).

## <a name="hierarchical-navigation"></a>[Иерархическая навигация](hierarchical.md)

Класс [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) обеспечивает иерархическую навигацию, при которой пользователь может переходить по страницам вперед и назад по своему желанию. Этот класс реализует навигацию на основе стека объектов [`Page`](xref:Xamarin.Forms.Page) по методу LIFO (последним поступил — первым обслужен).

## <a name="tabbedpage"></a>[TabbedPage](tabbed-page.md)

Xamarin.Forms [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) состоит из списка вкладок и большой области сведений, где каждая вкладка загружает содержимое в область сведений.

## <a name="carouselpage"></a>[CarouselPage](carousel-page.md)

Xamarin.Forms [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) — это страница, по которой пользователи могут проводить из стороны в сторону, чтобы переходить по страницам содержимого, например по страницам коллекции.

## <a name="masterdetailpage"></a>[MasterDetailPage](master-detail-page.md)

Xamarin.Forms [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) представляет собой страницу, управляющую двумя страницами связанных данных, — главной страницей, которая представляет элементы, и страницей сведений, которая представляет сведения об элементах на главной странице.

## <a name="modal-pages"></a>[Модальные страницы](modal.md)

Xamarin.Forms также поддерживает модальные страницы. На модальной странице пользователь должен выполнить отдельную задачу, причем он не может уйти с этой страницы, пока задача не будет выполнена или отменена.
