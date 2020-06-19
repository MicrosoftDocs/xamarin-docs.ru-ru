---
title: Xamarin.Forms DependencyService
description: Класс Xamarin.Forms DependencyService — это указатель служб, который позволяет приложениям Xamarin.Forms вызывать собственные функции платформы из общего кода.
ms.prod: xamarin
ms.assetid: 403479F2-6751-41F2-ADCE-3AF595062FE4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/05/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 126e2d2373bad923fe1d66fe355ad811c15fbe4f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138376"
---
# <a name="xamarinforms-dependencyservice"></a>Xamarin.Forms DependencyService

## <a name="introduction"></a>[Введение](introduction.md)

Класс [`DependencyService`](xref:Xamarin.Forms.DependencyService) — это указатель служб, который позволяет приложениям Xamarin.Forms вызывать собственные функции платформы из общего кода.

## <a name="registration-and-resolution"></a>[Регистрация и разрешение](registration-and-resolution.md)

Реализации платформы должны быть зарегистрированы в [`DependencyService`](xref:Xamarin.Forms.DependencyService), после чего разрешены из общего кода для их вызова.

## <a name="picking-a-photo-from-the-library"></a>[Выбор фотографии в библиотеке](photo-picker.md)

В статье объясняется, как использовать класс Xamarin.Forms [`DependencyService`](xref:Xamarin.Forms.DependencyService) для выбора фотографии в библиотеке рисунков на телефоне.
