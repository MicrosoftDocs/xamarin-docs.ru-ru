---
title: Как задать переменные среды выполнения Mono для проектов iOS в Xamarin Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/31/2017
ms.openlocfilehash: 30585bede5569edfa50ab9f450bcb62e04c8a10d
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938589"
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Как задать переменные среды выполнения Mono для проектов iOS в Xamarin Studio?

Если необходимо задать переменные среды выполнения для Mono, их можно задать в **параметрах проекта > запуска > страница Общие** .

Примечание. переменные среды сборки мусора для SGen ( \_ Параметры Mono GC \_ ) заданы таким образом, только при запуске из Xamarin Studio. При запуске приложения с устройства параметры SGen будут игнорироваться. 

Чтобы окончательно задать переменную среды для приложения, необходимо добавить ее в качестве дополнительного аргумента mtouch (для всех соответствующих конфигураций):

```csharp
   --setenv=NAME=VALUE
```

Чтобы просмотреть переменные среды, которые можно задать, перейдите на страницу с именем человека Mono. [http://docs.go-mono.com/?link=man%3amono(1)](http://docs.go-mono.com/?link=man%3amono(1)) см. раздел:`ENVIRONMENT VARIABLES`

![Задание переменных среды для проекта](xs-mono-runtime-images/environment-variables.jpg)
