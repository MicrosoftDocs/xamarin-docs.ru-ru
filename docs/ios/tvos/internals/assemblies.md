---
title: Сборки, поддерживаемые Xamarin для tvOS
description: Для пояснения функций, доступных для tvOS приложений, в этом документе содержится список сборок, поддерживаемых Xamarin для разработки tvOS.
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/07/2016
ms.openlocfilehash: 2fde84441eeb9342d1a86e4dc565faf51d08c57b
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573564"
---
# <a name="assemblies-supported-by-xamarin-for-tvos"></a>Сборки, поддерживаемые Xamarin для tvOS

## <a name="supported-assemblies"></a>Поддерживаемые сборки

Это список сборок, поддерживаемых Xamarin для приложений Xamarin. tvOS. Ниже приведен подробный список этих сведений.  В число заметных опущений входят `System.EnterpriseServices` стек ASP.NET и Windows. Forms.

|Сборка|Добавлено|Совместимость API|
|---|---|---|
|Mono.CompilerServices.SymbolWriter.dll|1.0|Для модулей записи компилятора.|
|Mono.Data.Sqlite.dll|1.2|Поставщик ADO.NET для SQLite; см. раздел [ограничения](~/ios/data-cloud/system.data.md).|
|Mono.Data.Tds.dll|1.2|Поддержка протокола TDS; используется для поддержки [System. Data. SqlClient](xref:System.Data.SqlClient) в [System. Data](~/ios/data-cloud/system.data.md).|
|Mono.Security.dll|1.0|Криптографические API.|
|monotouch.dll|1.0|Эта сборка содержит [привязку C# к API кокоатауч](https://docs.microsoft.com/dotnet/api/?view=xamarinios-10.8).|
|mscorlib.dll|1.0|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|OpenTK.dll|1.0|API-интерфейсы OpenGL/Open Object, [Расширенные для обеспечения поддержки устройств iPhone](xref:OpenGLES).|
|System.dll|1.0|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), а также типы из следующих пространств имен: <ul><li>System.Collections.Specialized</li> <li>System.ComponentModel</li> <li>System. ComponentModel. Design</li> <li>System.Diagnostics</li> <li>System.IO.Compression</li> <li>System.Net</li> <li>System .NET. Cache</li> <li>System.Net.Mail</li> <li>System .NET. MIME</li> <li>System.Net.NetworkInformation</li> <li>System.Net.Security</li> <li>System.Net.Sockets</li> <li>System.Security.Authentication</li> <li>System.Security.Cryptography</li> <li>Системные. Timers</li></ul>|
|System.Core.dll|1.0|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Data.dll|1.2|[.Net 3,5](https://msdn.microsoft.com/library/ms229335.aspx) [с удаленными функциями](~/ios/data-cloud/system.data.md).|
|System. Data. Service. Client. dll|3.x|Полный клиент oData.|
|System.Drawing;|1.0|API System. Drawing — только Classic API.<br />_System. Drawing не поддерживается в Unified API для платформы Xamarin. Mac .NET 4,5 или Mobile._|
|System.Json.dll|1.1|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Runtime.Serialization.dll|?|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.dll|1.1|Стек [WCF](https://docs.microsoft.com/xamarin/cross-platform/data-cloud/web-services/) , имеющийся в [Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.Web.dll|?|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), а также типы из следующих пространств имен: <ul><li>Система</li><li>System.ServiceModel.Channels</li><li>System.ServiceModel.Description</li><li>System.ServiceModel.Web</li></ul>|
|System.Transactions.dll|1.2|[.Net 3,5](https://msdn.microsoft.com/library/ms229335.aspx); часть поддержки [System. Data](https://docs.microsoft.com/xamarin/ios/data-cloud/system.data) .|
|System.Web.Services|1.1|[Основные веб-службы](https://docs.microsoft.com/xamarin/cross-platform/data-cloud/web-services/) из профиля .NET 3,5 с удаленными компонентами сервера.|
|System.Xml.dll|1.0|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|
|System.Xml.Linq.dll|1.0|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|

<a name="Summary"></a>

## <a name="portable-class-libraries"></a>Переносимые библиотеки классов

Помимо привязок Mac, Xamarin. tvOS может использовать [переносимые библиотеки классов .NET](~/cross-platform/app-fundamentals/pcl.md).

## <a name="related-links"></a>Связанные ссылки

- [tvOS](https://developer.apple.com/tvos/)
- [Руководства по tvOSму интерфейсу](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Руководством по программированию приложений для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
