---
title: Устранение неполадок
description: Распространенные ошибки и способы их устранения
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63291951-7375-4CBF-BCC3-2E4AD157A2C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5a84b7a5ca336b5823f1e0d2201e17cb4f152c27
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930849"
---
# <a name="troubleshooting"></a>Устранение неполадок

_Распространенные ошибки и способы их устранения_

## <a name="error-unable-to-find-a-version-of-xamarinforms-compatible-with"></a>Ошибка: "не удается найти версию, Xamarin.Forms совместимую с..."

Следующие ошибки могут появиться в окне **консоли пакетов** при обновлении всех пакетов NuGet в Xamarin.Forms решении или Xamarin.Forms проекте приложения Android:

```csharp
Attempting to resolve dependency 'Xamarin.Android.Support.v7.AppCompat (= 23.3.0.0)'.
Attempting to resolve dependency 'Xamarin.Android.Support.v4 (= 23.3.0.0)'.
Looking for updates for 'Xamarin.Android.Support.v7.MediaRouter'...
Updating 'Xamarin.Android.Support.v7.MediaRouter' from version '23.3.0.0' to '23.3.1.0' in project 'Todo.Droid'.
Updating 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0' to 'Xamarin.Android.Support.v7.MediaRouter 23.3.1.0' failed.
Unable to find a version of 'Xamarin.Forms' that is compatible with 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0'.
```

### <a name="what-causes-this-error"></a>В чем причина этой ошибки?

Visual Studio для Mac (или Visual Studio) могут указывать, что обновления доступны для Xamarin.Forms пакета NuGet *и всех его зависимостей*. В Xamarin Studio узел **пакетов** решения может выглядеть следующим образом (номера версий могут отличаться):

![Папка пакетов проектов Android](images/updates-available.png)

Эта ошибка может возникать при попытке обновить _все_ пакеты.

Это связано с тем, что в проектах Android, для которых задана Целевая или компилируемая версия Android 6,0 (API 23) или ниже, Xamarin.Forms имеет жесткую зависимость от *конкретных* версий пакетов поддержки Android. Хотя обновленные версии этих пакетов могут быть доступны, Xamarin.Forms они не обязательно совместимы с ними.

В этом случае следует обновить _только_ пакет, **Xamarin.Forms** так как они гарантируют, что зависимости останутся в совместимых версиях. Другие пакеты, добавленные в проект, также могут обновляться по отдельности, если они не приводят к обновлению пакетов поддержки Android.

> [!NOTE]
> Если вы используете Xamarin.Forms 2.3.4 или более позднюю версию, **а** Целевая версия проекта android — Android 7,0 (API 24) или более поздней версии, то эти неприменимые жесткие зависимости не применяются, и вы можете обновлять пакеты поддержки независимо от Xamarin.Forms пакета.

### <a name="fix-remove-all-packages-and-re-add-xamarinforms"></a>Исправление: удалите все пакеты и повторно добавьтеXamarin.Forms

Если пакеты **Xamarin. Android. support** были обновлены до несовместимых версий, проще всего исправить следующее:

1. Вручную удалите все пакеты NuGet в проекте Android, а затем
2. Повторно добавьте **Xamarin.Forms** пакет.

При этом будут автоматически скачаны *правильные* версии других пакетов.

Чтобы просмотреть видео об этом процессе, ознакомьтесь с этой [записью на форумах](https://forums.xamarin.com/discussion/comment/170012/#Comment_170012).
