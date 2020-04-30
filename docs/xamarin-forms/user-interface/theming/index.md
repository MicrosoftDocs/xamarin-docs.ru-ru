---
title: Применение тем в приложении Xamarin.Forms
description: Приложения Xamarin. Forms поддерживают их, создавая ResourceDictionary для каждой темы, а затем загружая ресурсы с помощью расширения разметки DynamicResource.
ms.prod: xamarin
ms.assetId: BF92AEDD-EF23-4D08-A972-B089066E75F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2020
ms.openlocfilehash: 5988437b40ac875b8b59f9af0f25d4b5c60ded97
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517132"
---
# <a name="theming-a-xamarinforms-application"></a>Их применение в приложении Xamarin. Forms

## <a name="theme-an-application"></a>[Тема приложения](theming.md)

Их можно реализовать в приложениях Xamarin. Forms, создав [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) для каждой темы, а затем загрузив ресурсы с расширением `DynamicResource` разметки.

## <a name="respond-to-system-theme-changes"></a>[Реагирование на изменения темы системы](system-theme-changes.md)

Устройства обычно включают в себя светло-и темные темы, каждый из которых ссылается на широкий набор предпочтений внешнего вида, который можно задать на уровне операционной системы. Приложения должны учитывать эти системные темы и отвечать сразу после изменения темы системы.
