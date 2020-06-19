---
title: Xamarin.Formsи веб-службы
description: В этом руководство объясняется, как взаимодействовать с различными веб-службами для предоставления приложению функций создания, чтения, обновления и удаления (CRUD) Xamarin.Forms . Рассматриваемые темы включают взаимодействие со службами ASMX, службами WCF, службами RESTFUL.
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5b2613b94d2c347d9bc6a94086f869b07ab8a55b
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84131889"
---
# <a name="xamarinforms-and-web-services"></a>Xamarin.Formsи веб-службы

## <a name="introduction"></a>[Введение](introduction.md)

В этой статье представлено пошаговое руководство по Xamarin.Forms использованию примера приложения, которое показывает, как взаимодействовать с различными веб-службами. Рассматриваемые темы включают в себя анатомию приложения, страницы, модель данных и вызов операций веб-службы.

## <a name="consume-an-aspnet-web-service-asmx"></a>[Использование веб-службы ASP.NET (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md)

ASP.NET Web Services (ASMX) предоставляют возможность создавать веб-службы, отправляющие сообщения по протоколу HTTP с помощью протокола SOAP. SOAP — это независимый от платформы и не зависящий от языка протокол для создания веб-служб и доступа к ним. Потребители службы ASMX не должны знать ничего о платформе, объектной модели или языке программирования, используемом для реализации службы. Им нужно только разобраться, как отправлять и получать сообщения SOAP. В этой статье показано, как использовать веб-службу ASMX из Xamarin.Forms приложения.

## <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>[Использование веб-службы Windows Communication Foundation (WCF)](~/xamarin-forms/data-cloud/web-services/wcf.md)

WCF — это единая платформа Майкрософт для создания приложений, ориентированных на службы. Она позволяет разработчикам создавать безопасные, надежные, транзакционные и совместимые распределенные приложения. Существуют различия между ASP.NET Web Services (ASMX) и WCF, но важно понимать, что WCF поддерживает те же возможности, которые предоставляет ASMX — сообщения SOAP по протоколу HTTP. В этой статье показано, как использовать службу WCF SOAP из Xamarin.Forms приложения.

## <a name="consume-a-restful-web-service"></a>[Использование веб-службы RESTFUL](~/xamarin-forms/data-cloud/web-services/rest.md)

Перестроение данных о состоянии (остальное) — это архитектурный стиль для создания веб-служб. Запросы RESTFUL выполняются по протоколу HTTP с использованием тех же глаголов HTTP, которые используются веб-браузерами для получения веб-страниц и отправки данных на серверы. В этой статье показано, как использовать веб-службу RESTFUL из Xamarin.Forms приложения.
