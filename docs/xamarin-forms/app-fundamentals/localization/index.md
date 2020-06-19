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
ms.openlocfilehash: dddb80e8fb683547d2bf6ba0b74e870fe240695c
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137570"
---
# <a name="xamarinforms-localization"></a>Локализация в Xamarin.Forms

_Встроенную платформу локализации .NET можно использовать в кроссплатформенных многоязыковых приложениях в Xamarin.Forms._

## <a name="xamarinforms-string-and-image-localizationtextmd"></a>[Локализация строк и изображений в Xamarin.Forms](text.md)

Встроенный механизм для локализации приложений использует [RESX-файлы](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps#resources-in-resx-files) и классы в пространствах имен `System.Resources` и `System.Globalization`. RESX-файлы, содержащие переведенные строки, внедряются в сборку Xamarin.Forms вместе с создаваемым компилятором классом, который обеспечивает строго типизированный доступ к переводам. Затем переведенный текст можно извлечь в коде.

## <a name="right-to-left-localization"></a>[Локализация справа налево](right-to-left.md)

Направление потока — это направление, в котором глаз человека перемещается по элементам пользовательского интерфейса на странице. Локализация справа налево поддерживает направление потока справа налево в приложениях Xamarin.Forms.
