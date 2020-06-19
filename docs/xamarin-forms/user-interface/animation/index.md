---
title: Анимация вXamarin.Forms
description: Xamarin.Formsвключает собственную инфраструктуру анимации, которая проста для создания простых анимаций, а также достаточно гибкой для создания сложных анимаций.
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 88a671c4d28d62a5f73e90a7b2fa9c45b7dbe8b1
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84129003"
---
# <a name="animation-in-xamarinforms"></a>Анимация вXamarin.Forms

_Xamarin. Forms включает собственную инфраструктуру анимации, простую в создании простых анимаций, а также достаточно гибкой для создания сложных анимаций._

Xamarin.FormsКлассы анимации предназначены для различных свойств визуальных элементов с обычной анимацией, которая постепенно изменяет свойство с одного значения на другое в течение определенного периода времени. Обратите внимание, что для классов анимации отсутствует интерфейс XAML Xamarin.Forms . Однако анимации можно инкапсулировать в [поведении](~/xamarin-forms/app-fundamentals/behaviors/index.md) , а затем ссылаться на них из XAML.

## <a name="simple-animations"></a>[Простые анимации](simple.md)

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)Класс предоставляет методы расширения, которые можно использовать для создания простых анимаций, повернутых, масштабируемых, преобразованных и конусных [`VisualElement`](xref:Xamarin.Forms.VisualElement) экземпляров. В этой статье показано, как создавать и отменять анимацию с помощью `ViewExtensions` класса.

## <a name="easing-functions"></a>[Функции плавности](easing.md)

Xamarin.Formsвключает [`Easing`](xref:Xamarin.Forms.Easing) класс, который позволяет указать функцию передачи, которая управляет скоростью анимации или замедляется при их выполнении. В этой статье показано, как использовать предварительно определенные функции плавности и как создавать пользовательские функции плавности.

## <a name="custom-animations"></a>[Пользовательские анимации](custom.md)

[`Animation`](xref:Xamarin.Forms.Animation)Класс является стандартным блоком всех Xamarin.Forms анимаций с методами расширения в классе, [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) создающими один или несколько `Animation` объектов. В этой статье показано, как использовать `Animation` класс для создания и отмены анимации, синхронизации нескольких анимаций и создания пользовательских анимаций, которые анимировать свойства, которые не анимированы с помощью существующих методов анимации.
