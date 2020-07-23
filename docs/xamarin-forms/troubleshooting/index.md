---
title: 'Выявлен:::no-loc(Xamarin.Forms):::'
description: Распространенные ошибки и способы их устранения
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63291951-7375-4CBF-BCC3-2E4AD157A2C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
no-loc:
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: 857c729ac7642003f40e34afa024c6cfcbaabb39
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "86996569"
---
# <a name="troubleshooting-no-locxamarinforms"></a>Выявлен:::no-loc(Xamarin.Forms):::

_Распространенные ошибки и способы их устранения_

## <a name="error-unable-to-find-a-version-of-no-locxamarinforms-compatible-with"></a>Ошибка: "не удается найти версию, :::no-loc(Xamarin.Forms)::: совместимую с..."

Следующие ошибки могут появиться в окне **консоли пакетов** при обновлении всех пакетов NuGet в :::no-loc(Xamarin.Forms)::: решении или :::no-loc(Xamarin.Forms)::: проекте приложения Android:

```csharp
Attempting to resolve dependency 'Xamarin.Android.Support.v7.AppCompat (= 23.3.0.0)'.
Attempting to resolve dependency 'Xamarin.Android.Support.v4 (= 23.3.0.0)'.
Looking for updates for 'Xamarin.Android.Support.v7.MediaRouter'...
Updating 'Xamarin.Android.Support.v7.MediaRouter' from version '23.3.0.0' to '23.3.1.0' in project 'Todo.Droid'.
Updating 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0' to 'Xamarin.Android.Support.v7.MediaRouter 23.3.1.0' failed.
Unable to find a version of ':::no-loc(Xamarin.Forms):::' that is compatible with 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0'.
```

### <a name="what-causes-this-error"></a>В чем причина этой ошибки?

Visual Studio для Mac (или Visual Studio) могут указывать, что обновления доступны для :::no-loc(Xamarin.Forms)::: пакета NuGet *и всех его зависимостей*. В Xamarin Studio узел **пакетов** решения может выглядеть следующим образом (номера версий могут отличаться):

![Папка пакетов проектов Android](images/updates-available.png)

Эта ошибка может возникать при попытке обновить _все_ пакеты.

Это связано с тем, что в проектах Android, для которых задана Целевая или компилируемая версия Android 6,0 (API 23) или ниже, :::no-loc(Xamarin.Forms)::: имеет жесткую зависимость от *конкретных* версий пакетов поддержки Android. Хотя обновленные версии этих пакетов могут быть доступны, :::no-loc(Xamarin.Forms)::: они не обязательно совместимы с ними.

В этом случае следует обновить _только_ пакет, **:::no-loc(Xamarin.Forms):::** так как они гарантируют, что зависимости останутся в совместимых версиях. Другие пакеты, добавленные в проект, также могут обновляться по отдельности, если они не приводят к обновлению пакетов поддержки Android.

> [!NOTE]
> Если вы используете :::no-loc(Xamarin.Forms)::: 2.3.4 или более позднюю версию, **а** Целевая версия проекта android — Android 7,0 (API 24) или более поздней версии, то эти неприменимые жесткие зависимости не применяются, и вы можете обновлять пакеты поддержки независимо от :::no-loc(Xamarin.Forms)::: пакета.

### <a name="fix-remove-all-packages-and-re-add-no-locxamarinforms"></a>Исправление: удалите все пакеты и повторно добавьте:::no-loc(Xamarin.Forms):::

Если пакеты **Xamarin. Android. support** были обновлены до несовместимых версий, проще всего исправить следующее:

1. Вручную удалите все пакеты NuGet в проекте Android, а затем
2. Повторно добавьте **:::no-loc(Xamarin.Forms):::** пакет.

При этом будут автоматически скачаны *правильные* версии других пакетов.

Чтобы просмотреть видео об этом процессе, ознакомьтесь с этой [записью на форумах](https://forums.xamarin.com/discussion/comment/170012/#Comment_170012).
