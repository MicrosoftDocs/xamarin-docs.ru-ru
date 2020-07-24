---
title: Шаг 1. Регистрация приложения для использования Azure Active Directory
description: В этом документе описывается регистрация приложения Azure с Azure Active Directory для безопасного доступа мобильных клиентов.
ms.prod: xamarin
ms.assetid: 0B17991A-4573-4F6C-9E86-D4B9D1A47E4D
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 6b10b2fd489881292d95bbf79375a6488aa88a17
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86932561"
---
# <a name="step-1-register-an-app-to-use-azure-active-directory"></a>Шаг 1. Регистрация приложения для использования Azure Active Directory

1. Перейдите по адресу [WindowsAzure.com](https://manage.windowsazure.com) и войдите с помощью учетной записи Майкрософт или учетной записи организации на портале Azure. Если у вас нет подписки Azure, вы можете получить пробную версию от [Azure.com](https://www.azure.com) .

2. После входа перейдите в раздел **Active Directory** (1) и выберите каталог, в котором нужно зарегистрировать приложение (2).

   [![(раздел) и выберите каталог, в котором необходимо зарегистрировать приложение.](register-images/01.-active-directory-in-azure-portal-sml.jpg)](register-images/01.-active-directory-in-azure-portal.jpg#lightbox)

3. Нажмите кнопку **Добавить** , чтобы создать новое приложение, а затем выберите **Добавить приложение, разрабатываемое моей организацией** .

   [![Добавить приложение, разрабатываемое моей организацией](register-images/02.-add-new-application-sml.jpg)](register-images/02.-add-new-application.jpg#lightbox)

4. На следующем экране присвойте имя приложению (например, КСАМ — ДЕМОНСТРАЦИЯ).
   Убедитесь, что в качестве типа приложения выбрано **собственное клиентское приложение** .

   ![Убедитесь, что в качестве типа приложения выбрано собственное клиентское приложение.](register-images/03.-app-name.jpg)

5. На последнем экране введите **URI перенаправления* , уникальный для приложения, так как он вернется к этому URI после завершения проверки подлинности.

   ![На последнем экране укажите URI перенаправления, уникальный для приложения, так как он вернется к этому URI после завершения проверки подлинности.](register-images/04.-app-redirect.jpg)

6. После создания приложения перейдите на вкладку **Настройка** . Запишите **идентификатор клиента** , который мы будем использовать в нашем приложении позже. Кроме того, на этом экране можно предоставить мобильному приложению доступ к Active Directory или добавить другое приложение, например веб-API или Office 365, которое может использоваться мобильным приложением после завершения проверки подлинности.

   ![Кроме того, на этом экране можно предоставить мобильному приложению доступ к Active Directory или добавить другое приложение, например веб-API или Office 365.](register-images/05.-configure.jpg)

## <a name="related-links"></a>Связанные ссылки

- [Пример Microsoft NativeClient](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
