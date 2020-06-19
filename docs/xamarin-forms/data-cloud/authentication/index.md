---
title: Xamarin.FormsПроверка подлинности веб-службы
description: В этом руководство объясняется, как интегрировать службы проверки подлинности в Xamarin.Forms приложение, чтобы предоставить пользователям общий доступ к серверной части, только имея доступ к собственным данным.
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 59130739617f38ce32e0d241f8e068cf077997ac
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136088"
---
# <a name="xamarinforms-web-service-authentication"></a>Xamarin.FormsПроверка подлинности веб-службы

## <a name="authenticate-a-restful-web-service"></a>[Проверка подлинности веб-службы RESTFUL](rest.md)

Протокол HTTP поддерживает использование нескольких механизмов проверки подлинности для управления доступом к ресурсам. Обычная проверка подлинности предоставляет доступ к ресурсам только тем клиентам, которые имеют правильные учетные данные. В этой статье объясняется, как использовать обычную проверку подлинности для защиты доступа к ресурсам веб-службы RESTFUL.

## <a name="authenticate-users-with-azure-active-directory-b2c"></a>[Проверка подлинности с помощью Azure Active Directory B2C](azure-ad-b2c.md)

Azure Active Directory B2C — это облачное решение, позволяющее управлять удостоверениями в веб-приложениях и мобильных приложениях, с которыми взаимодействуют клиенты. В этой статье объясняется, как использовать библиотеку проверки подлинности Майкрософт (MSAL) и Azure Active Directory B2C для интеграции управления удостоверениями пользователей в Xamarin.Forms приложение.

## <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinformsazure-cosmosdb-authmd"></a>[Проверка подлинности пользователей с помощью Azure Cosmos DB базы данных документов иXamarin.Forms](azure-cosmosdb-auth.md)

Базы данных документов Azure Cosmos DB поддерживают секционированные коллекции, которые могут охватывать несколько серверов и секций при поддержке неограниченного объема хранилища и пропускной способности. В этой статье объясняется, как объединить контроль доступа с секционированными коллекциями, чтобы пользователь мог получить доступ к собственным документам в Xamarin.Forms приложении.
