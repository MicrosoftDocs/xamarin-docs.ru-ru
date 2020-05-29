---
title: Xamarin.FormsПриложения
description: Xamarin.Formsприложения поддерживают их, создавая ResourceDictionary для каждой темы, а затем загружая ресурсы с помощью расширения разметки DynamicResource.
ms.prod: ''
ms.assetId: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 80660ae7d3af0fe5948a5ae4ffdb35d2f9c2a40f
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136140"
---
# <a name="theming-a-xamarinforms-application"></a>Xamarin.FormsПриложения

## <a name="theme-an-application"></a>[Тема приложения](theming.md)

Их можно реализовать в Xamarin.Forms приложениях, создав [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) для каждой темы, а затем загрузив ресурсы с `DynamicResource` расширением разметки.

## <a name="respond-to-system-theme-changes"></a>[Реагирование на изменения темы системы](system-theme-changes.md)

Устройства обычно включают в себя светло-и темные темы, каждый из которых ссылается на широкий набор предпочтений внешнего вида, который можно задать на уровне операционной системы. Приложения должны учитывать эти системные темы и отвечать сразу после изменения темы системы.
