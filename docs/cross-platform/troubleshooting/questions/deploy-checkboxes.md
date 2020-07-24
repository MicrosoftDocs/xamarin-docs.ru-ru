---
title: Отключены флажки развертывания в Configuration Manager
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: davidortinau
ms.author: daortin
ms.date: 12/02/2016
ms.openlocfilehash: c3d0b8f2da971238b98253405f3b8fe08699ab56
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997219"
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Отключены флажки развертывания в Configuration Manager

Так как Xamarin 3,5 проекты Xamarin. iOS разворачиваются автоматически при нажатии кнопки **Пуск** на панели инструментов или выборе пункта меню **Отладка > начать отладку** . Перед выполнением любой из этих команд вам все равно потребуется задать проект приложения Xamarin. iOS в качестве **запускаемого проекта** .

По этой причине флажки **развертывания** намеренно отключены в Configuration Manager Visual Studio для проектов Xamarin. iOS:

![Visual Studio Configuration Manager показывает, что флажок развертывания отключен для проекта Xamarin. iOS в Xamarin 3,5](deploy-checkboxes-images/configuration.png)

Это изменение устраняет ошибку, которая может появиться в более старых версиях Xamarin (версии 3,3 и более ранних), если проект приложения Xamarin. iOS не был настроен для развертывания:

![Диалоговое окно ошибки: проект iPhoneApp1 необходимо развернуть, прежде чем его можно будет запустить. Убедитесь, что проект выбран для развертывания в Configuration Manager решения.](deploy-checkboxes-images/error.png)
