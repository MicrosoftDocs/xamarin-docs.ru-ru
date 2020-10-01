---
title: Введение в ContentProvider
description: Для упрощения доступа к общим данным, таким как файлы мультимедиа, контакты и сведения о календаре, операционная система Android использует поставщики содержимого. В этой статье представлен класс ContentProvider, а также два примера его использования.
ms.prod: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 00da8b668ea26aa9146ff0c30d07a60cefe6695a
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91454017"
---
# <a name="intro-to-contentproviders"></a>Введение в ContentProvider

_Для упрощения доступа к общим данным, таким как файлы мультимедиа, контакты и сведения о календаре, операционная система Android использует поставщики содержимого. В этой статье представлен класс ContentProvider, а также два примера его использования._

## <a name="content-providers-overview"></a>Общие сведения о поставщиках содержимого

*Поставщик содержимого* инкапсулирует репозиторий данных и предоставляет API для доступа к нему. Поставщик существует как часть приложения Android, которая обычно также предоставляет пользовательский интерфейс для отображения данных и управления ими. Основное преимущество поставщика содержимого заключается в том, что другие приложения могут легко получать доступ к инкапсулированным данным с помощью клиентского объекта поставщика (называемого *ContentResolver*). Вместе поставщик и сопоставитель содержимого обеспечивают единообразный работающий между приложениями API для доступа к данным. Этот API прост в создании и использовании. Любое приложение может использовать `ContentProviders` для внутреннего управления данными, а также для их предоставления другим приложениям.

`ContentProvider` также необходимо для приложения, чтобы предоставить настраиваемые варианты поиска, или если вы хотите предоставить возможность копирования сложных данных из приложения для вставки в другие приложения. В этом документе показано, как получить доступ к `ContentProviders` и создавать его с помощью Xamarin.Android.

Эта статья имеет следующую структуру.

- **Принцип работы** &ndash; обзор того, для чего предназначен `ContentProvider` и как он работает.

- **Использование поставщика содержимого** &ndash; пример доступа к списку контактов.

- **Использование ContentProvider для совместного использования данных** &ndash; запись и использование `ContentProvider` в одном приложении.

`ContentProviders` и курсоры, работающие с их данными, часто используются для заполнения ListViews. Дополнительные сведения об использовании этих классов см. в руководстве [Xamarin.Android ListView](~/android/user-interface/layouts/list-view/index.md).

`ContentProviders`, предоставляемые Android (или другими приложениями), — это простой способ включения в приложение данных из других источников. Эти классы позволяют получать доступ к данным, таким как список контактов, фотографии или события календаря в приложении, и предоставлять пользователю возможность взаимодействовать с ними.

Настраиваемые классы `ContentProviders` — это удобный способ упаковки данных для использования в собственном приложении или для использования другими приложениями (включая особые варианты использования, такие как пользовательский поиск и копирование или вставка).

В темах этого раздела приводятся некоторые простые примеры использования и написания кода `ContentProvider`.

## <a name="related-links"></a>Связанные ссылки

- [Демонстрационный пример для ContactsAdapter](/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)
- [SimpleContentProvider (пример)](/samples/xamarin/monodroid-samples/platformfeatures-simplecontentprovider)
- [Поставщики содержимого](https://developer.android.com/guide/topics/providers/content-providers.html)
- [Класс ContentProvider](xref:Android.Content.ContentProvider)
- [Класс ContentResolver](xref:Android.Content.ContentResolver)
- [Класс ListView](xref:Android.Widget.ListView)
- [Класс CursorAdapter](xref:Android.Widget.CursorAdapter)
- [Класс UriMatcher](xref:Android.Content.UriMatcher)
- [Пространство имен Android.Provider](xref:Android.Provider)
- [Класс ContactsContract](xref:Android.Provider.ContactsContract)