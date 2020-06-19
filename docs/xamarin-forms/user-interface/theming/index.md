---
title: Xamarin.FormsПриложения
description: Xamarin.Formsприложения поддерживают их, создавая ResourceDictionary для каждой темы, а затем загружая ресурсы с помощью расширения разметки DynamicResource.
ms.prod: xamarin
ms.assetId: BF92AEDD-EF23-4D08-A972-B089066E75F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 80660ae7d3af0fe5948a5ae4ffdb35d2f9c2a40f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136140"
---
# <a name="theming-a-xamarinforms-application"></a>Xamarin.FormsПриложения

## <a name="theme-an-application"></a>[Тема приложения](theming.md)

Их можно реализовать в Xamarin.Forms приложениях, создав [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) для каждой темы, а затем загрузив ресурсы с `DynamicResource` расширением разметки.

## <a name="respond-to-system-theme-changes"></a>[Реагирование на изменения темы системы](system-theme-changes.md)

Устройства обычно включают в себя светло-и темные темы, каждый из которых ссылается на широкий набор предпочтений внешнего вида, который можно задать на уровне операционной системы. Приложения должны учитывать эти системные темы и отвечать сразу после изменения темы системы.
