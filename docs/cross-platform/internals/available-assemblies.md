---
title: Доступные сборки
description: В этом документе перечислены сборки, доступные для использования в Xamarin. iOS, Xamarin. Android и Xamarin. Mac. Он также содержит ссылки на документацию по библиотекам .NET Standard и переносимым библиотекам классов.
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: davidortinau
ms.author: daortin
ms.date: 03/13/2018
ms.openlocfilehash: cb5c4a463a8368d6a82d89baba08145f161a95d6
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457943"
---
# <a name="available-assemblies"></a>Доступные сборки

Все устройства Xamarin. iOS, Xamarin. Android и Xamarin. Mac поставляются с более чем десятком сборок. Так же, как Silverlight — это расширенное подмножество сборок для настольных систем .NET, платформы Xamarin также являются расширенным подмножеством нескольких сборок Silverlight и настольных систем .NET.

Платформы Xamarin не совместимы с интерфейсом ABI с существующими сборками, скомпилированными для другого профиля. Необходимо перекомпилировать исходный код, чтобы создать сборки, предназначенные для правильного профиля (так же, как необходимо повторно скомпилировать исходный код для Silverlight и .NET 3,5 отдельно).

Приложения Xamarin. Mac могут быть скомпилированы в трех режимах: в одном из которых используется проверенный мобильный профиль Xamarin, платформа Xamarin. Mac .NET 4,5, позволяющая ориентироваться на существующие сборки с полным рабочим столом, а также неподдерживаемый, использующий API-интерфейс .NET, обнаруженный в системе Mono. Дополнительные сведения см. в документации по [целевым платформам](~/mac/platform/target-framework.md) .

## <a name="net-standard-libraries"></a>Библиотеки .NET Standard

Помимо привязок iOS, Android и Mac, проекты Xamarin могут использовать [библиотеки .NET Standard](~/cross-platform/app-fundamentals/net-standard.md).

## <a name="portable-class-libraries"></a>Переносимые библиотеки классов

Проекты Xamarin также могут использовать [переносимые библиотеки классов .NET](~/cross-platform/app-fundamentals/pcl.md), хотя эта технология является устаревшей в пользу .NET Standard.

## <a name="supported-assemblies"></a>Поддерживаемые сборки

Это сборки, доступные в **диспетчере ссылок > сборки > Framework** (Visual Studio 2017) и **ссылки Edit > packages** (Visual Studio для Mac) и их совместимость с платформами Xamarin.

> [!div class="mx-tdCol2BreakAll"]
> |Сборка|Совместимость API|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |l18N.dll|Включает ККЯ, Ближний Восток, другие, редкие, Западная|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |Microsoft.CSharp.dll| |![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |Mono.CSharp.dll| |![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |Mono.Data.Sqlite.dll|Поставщик ADO.NET для SQLite; см. раздел ограничения.|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |Mono.Data.Tds.dll|Поддержка протокола TDS; используется для поддержки [System. Data. SqlClient](xref:System.Data.SqlClient) в [System. Data](xref:System.Data).|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |Моно. Dynamic. &#8203;Interpreter.dll| |![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")| | |
> |Mono.Security.dll|Криптографические API.|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |monotouch.dll|Эта сборка содержит привязку C# к API Кокоатауч. Она доступна только в классических проектах iOS.|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")| | |
> |Котушь. &#8203;Dialog-1.dll| |![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")| | |
> |Котушь. &#8203;NUnitLite.dll| |![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")| | |
> |mscorlib.dll|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |OpenTK-1.0.dll|API-интерфейсы OpenGL/Open Object, расширенные для обеспечения поддержки устройств iPhone.|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |System.dll|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95)), а также типы из следующих пространств имен:<br />System.Collections.Specialized<br />System. &#8203;ComponentModel<br />System. ComponentModel. Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System. IO. Compression. FileSystem<br />System.Net<br />System .NET. Cache<br />System.Net.Mail<br />System .NET. MIME<br />System.Net. &#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System. Runtime. &#8203;InteropServices<br />System.Runtime.Versioning<br />System. Security. &#8203;AccessControl<br />System.Security.Authentication<br />Шифрование System. Security. &#8203;<br />System. Security. Permissions<br />System.Threading<br />Системные. Timers|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |System. &#8203;ComponentModel. &#8203;Composition.dll| |![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |System. &#8203;ComponentModel. &#8203;DataAnnotations.dll| |![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |System.Core.dll|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |System.Data.dll|[.Net 3,5](/previous-versions/ms229335(v=vs.100)) с [удаленными функциями](~/ios/data-cloud/system.data.md).|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |System. Data. &#8203;Services. &#8203;Client.dll|Полный клиент oData.|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |Сжатие System.IO. &#8203;| |![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |Сжатие System.IO. &#8203;. &#8203;FileSystem| |![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |System.Json.dll|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |System.Net. &#8203;Http.dll| |![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |Numerics.dll System. &#8203;| |![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |System. Runtime. &#8203;Serialization.dll|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |ServiceModel.dll System. &#8203;|Стек WCF, имеющийся в [Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |System. &#8203;ServiceModel. &#8203;Internals.dll| |![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |System. &#8203;ServiceModel. &#8203;Web.dll|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95)), а также типы из следующих пространств имен: <br />Система<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |Transactions.dll System. &#8203;|[.Net 3,5](/previous-versions/ms229335(v=vs.100)); часть поддержки [System. Data](~/ios/data-cloud/system.data.md) .|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |Services.dll System. Web. &#8203;|Основные веб-службы из профиля .NET 3,5 с удаленными компонентами сервера.|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |Windows.dll System. &#8203;| |![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |Xml.dll System. &#8203;|[.NET 3.5](/previous-versions/ms229335(v=vs.100))|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |System.Xml. &#8203;Linq.dll|[.NET 3.5](/previous-versions/ms229335(v=vs.100))|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |System.Xml.Serialization.dll| |![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")|![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")|![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |Xamarin.iOS.dll|Эта сборка содержит привязку C# к API Кокоатауч. Это используется только в Объединенных проектах iOS.|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")| | |
> |Java.Interop.dll| | |![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")| |
> |Mono.Android.dll| | |![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")| |
> |Export.dll Mono. Android. &#8203;| | |![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")| |
> |Mono.Posix.dll| | |![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")| |
> |EnterpriseServices.dll System. &#8203;| | |![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")| |
> |Xamarin. Android. &#8203;NUnitLite.dll| | |![Поддерживается Xamarin. Android](~/media/shared/yes.png "Поддерживается Xamarin. Android")| |
> |SymbolWriter.dll Mono. CompilerServices. &#8203;|Для модулей записи компилятора.| | |![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |Xamarin.Mac.dll| | | |![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|
> |Drawing.dll System. &#8203;|System. Drawing не поддерживается в Unified API для платформ Xamarin. Mac, .NET 4,5 или Mobile. Поддержку System. Drawing можно добавить в iOS и macOS с помощью библиотеки [сисдравинг-кореграфикс](https://github.com/mono/sysdrawing-coregraphics) .|![Поддержка Xamarin. iOS](~/media/shared/yes.png "Поддержка Xamarin. iOS")| |![Поддержка Xamarin. Mac](~/media/shared/yes.png "Поддержка Xamarin. Mac")|