---
title: Локализация в Xamarin.Forms
description: Встроенную платформу локализации .NET можно использовать в кроссплатформенных многоязыковых приложениях в Xamarin.Forms. Можно локализовать текст и изображения, а приложения могут поддерживать направление потока справа налево.
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5dbcc63eb162454845b78fc24a3beb0ca8296453
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91558158"
---
# <a name="no-locxamarinforms-localization"></a>Локализация в Xamarin.Forms

_Встроенную платформу локализации .NET можно использовать в кроссплатформенных многоязыковых приложениях в Xamarin.Forms._

## <a name="no-locxamarinforms-string-and-image-localization"></a>[Локализация строк и изображений в Xamarin.Forms](text.md)

Встроенный механизм для локализации приложений использует [RESX-файлы](/dotnet/framework/resources/creating-resource-files-for-desktop-apps#resources-in-resx-files) и классы в пространствах имен `System.Resources` и `System.Globalization`. RESX-файлы, содержащие переведенные строки, внедряются в сборку Xamarin.Forms вместе с создаваемым компилятором классом, который обеспечивает строго типизированный доступ к переводам. Затем переведенный текст можно извлечь в коде.

## <a name="right-to-left-localization"></a>[Локализация справа налево](right-to-left.md)

Направление потока — это направление, в котором глаз человека перемещается по элементам пользовательского интерфейса на странице. Локализация справа налево поддерживает направление потока справа налево в приложениях Xamarin.Forms.