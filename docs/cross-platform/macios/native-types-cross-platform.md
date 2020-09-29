---
title: Работа с собственными типами в кроссплатформенных приложениях
description: В этой статье рассматривается использование новых типов iOS Unified API Native Type (НИНТ, нуинт, нфлоат) в кросс-платформенном приложении, где код используется для устройств, не относящихся к iOS, таких как Android или Windows Phoneные ОС.
ms.prod: xamarin
ms.assetid: B9C56C3B-E196-4ADA-A1DE-AC10D1001C2A
author: davidortinau
ms.author: daortin
ms.date: 04/07/2016
ms.openlocfilehash: 4819f8fc88a8a2c730fc25215e862d68cc5bd643
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457293"
---
# <a name="working-with-native-types-in-cross-platform-apps"></a>Работа с собственными типами в кроссплатформенных приложениях

_В этой статье рассматривается использование новых типов iOS Unified API Native Type (НИНТ, нуинт, нфлоат) в кросс-платформенном приложении, где код используется для устройств, не относящихся к iOS, таких как Android или Windows Phoneные ОС._

Типы 64-Types работают с API iOS и Mac. При написании общего кода, который выполняется в Android или Windows, необходимо управлять преобразованием Объединенных типов в обычные типы .NET, к которым можно предоставить общий доступ.

В этом документе обсуждаются различные способы взаимодействия с Unified API из общего или общего кода.

## <a name="when-to-use-the-native-types"></a>Когда следует использовать собственные типы

В единые API-интерфейсы Xamarin. iOS и Xamarin. Mac все еще включены `int` `uint` типы данных и, а `float` также `RectangleF` `SizeF` `PointF` типы и. Эти существующие типы данных должны продолжать использоваться в любом совместно используемом, кросс-платформенном коде. Новые собственные типы данных следует использовать только при вызове интерфейса API Mac или iOS, где требуется поддержка типов, поддерживающих архитектуру.

В зависимости от природы кода, к которому предоставляется общий доступ, могут возникнуть ситуации, когда для межплатформенного кода может потребоваться работать с `nint` `nuint` `nfloat` типами данных и. Например, Библиотека, которая обрабатывает преобразования прямоугольных данных, которые ранее использовались `System.Drawing.RectangleF` для совместного использования функциональных возможностей версий приложения Xamarin. iOS и Xamarin. Android, должны быть обновлены для обработки собственных типов в iOS.

Обработка этих изменений зависит от размера и сложности приложения, а также от того, какой формат использовался для совместного использования кода, как будет показано в следующих разделах.

## <a name="code-sharing-considerations"></a>Рекомендации по совместному использованию кода

Как указано в документе о [параметрах совместного использования кода](~/cross-platform/app-fundamentals/code-sharing.md) , существует два основных способа совместного использования кода между межплатформенными проектами: общие проекты и переносимые библиотеки классов. Какой из двух типов был использован, ограничит параметры, которые имеются при обработке собственных типов данных в кросс-платформенном коде.

### <a name="portable-class-library-projects"></a>Проекты переносимой библиотеки классов

Переносимая библиотека классов (PCL) позволяет ориентироваться на платформы, которые требуется поддерживать, и использовать интерфейсы для предоставления функциональных возможностей для конкретных платформ.

Так как тип проекта PCL компилируется в `.DLL` и не имеет смысла Unified API, вы вынуждены использовать существующие типы данных ( `int` , `uint` , `float` ) в исходном коде PCL и вводить вызовы к классам и методам PCL в интерфейсных приложениях. Пример:

```csharp
using NativePCL;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

### <a name="shared-projects"></a>Общие проекты

Тип проекта общий ресурс позволяет организовать исходный код в отдельном проекте, который затем включается и компилируется в отдельные клиентские приложения для конкретных платформ, а `#if` также использует директивы компилятора, необходимые для управления требованиями конкретной платформы.

При выборе метода поддержки собственных типов данных в межплатформенном проекте общих ресурсов необходимо учитывать размер и сложность мобильных приложений, использующих общий код, а также размер и сложность кода, к которым предоставляется общий доступ.

На основе этих факторов можно реализовать следующие типы решений с помощью `if __UNIFIED__ ... #endif` директив компилятора, чтобы обрабатывать Unified API конкретные изменения в коде.

#### <a name="using-duplicate-methods"></a>Использование повторяющихся методов

Возьмем пример библиотеки, выполняющей преобразование для прямоугольных данных, указанных выше. Если библиотека содержит только один или два очень простых метода, вы можете создать дублирующиеся версии этих методов для Xamarin. iOS и Xamarin. Android. Пример:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #else
            public static float CalculateArea(RectangleF rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #endif
        #endregion
    }
}
```

В приведенном выше коде, поскольку `CalculateArea` подпрограмма очень проста, мы использовали условную компиляцию и создали отдельную Unified API версию метода. С другой стороны, если в библиотеке содержалось много подпрограмм или несколько сложных подпрограмм, это решение было бы нецелесообразным, так как в этом случае возникает проблема с поддержанием всех методов в синхронизации для внесения изменений или исправления ошибок.

#### <a name="using-method-overloads"></a>Использование перегрузок методов

В этом случае решением может быть создание перегруженной версии методов с использованием 32-разрядных типов данных, чтобы они теперь были в `CGRect` качестве параметра и (или) возвращаемого значения, преобразуют это значение в `RectangleF` (зная, что преобразование из `nfloat` в `float` — это преобразование с потерей данных) и вызвать исходную версию подпрограммы для выполнения фактической работы. Пример:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

                // Call original routine to calculate area
                return (nfloat)CalculateArea((RectangleF)rect);

            }
        #endif

        public static float CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }

        #endregion
    }
}

```

Опять же, это хорошее решение, если потери точности не влияют на результаты конкретных потребностей вашего приложения.

#### <a name="using-alias-directives"></a>Использование директив псевдонима

Для областей, где вероятность потери точности является проблемой, другое возможное решение заключается в использовании `using` директив для создания псевдонима для типов данных native и кореграфикс, включая следующий код в начало файлов с общим исходным кодом и преобразуя все необходимые `int` `uint` значения, или `float` `nint` `nuint` `nfloat` :

```csharp
#if __UNIFIED__
    // Mappings Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Mappings Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif
```

Итак, наш пример кода выглядит следующим образом:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
    // Map Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Map Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif

namespace NativeShared
{

    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        public static nfloat CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }
        #endregion
    }
}
```

Обратите внимание, что мы изменили `CalculateArea` метод, чтобы он возвращал `nfloat` вместо стандартного `float` . Это было сделано, чтобы не было возникать ошибка компиляции при попытке _неявного_ преобразования `nfloat` результата вычисления (поскольку оба значения, умноженные на, имеют тип `nfloat` ) в `float` возвращаемое значение.

Если код компилируется и выполняется на неUnified APIном устройстве, объект `using nfloat = global::System.Single;` сопоставляет с, `nfloat` `Single` который неявно преобразуется в, что позволяет использовать `float` клиентское приложение для вызова `CalculateArea` метода без изменения.

#### <a name="using-type-conversions-in-the-front-end-app"></a>Использование преобразований типов в приложении внешнего интерфейса

В случае, если клиентские приложения делают всего несколько вызовов библиотеки общего кода, другое решение может быть неизменным и выполнять приведение типов в приложении Xamarin. iOS или Xamarin. Mac при вызове существующей подпрограммы. Пример:

```csharp
using NativeShared;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

Если используемое приложение делает сотни вызовов к общей библиотеке кода, это, возможно, не будет хорошим решением.

Основываясь на архитектуре нашего приложения, мы могли бы использовать одно или несколько из перечисленных выше решений для поддержки собственных типов данных (где это необходимо) в коде кросс-платформенного кода.

## <a name="xamarinforms-applications"></a>Приложения Xamarin. Forms

Для использования Xamarin. Forms для межплатформенного пользовательского интерфейса, который также будет использоваться совместно с Unified API приложением, необходимо следующее:

- Все решение должно использовать пакет NuGet для Xamarin. Forms версии 1.3.1 (или более поздней).
- Для любого пользовательского рендеринга Xamarin. iOS используйте те же типы решений, которые были представлены выше, в зависимости от того, как был предоставлен общий доступ к коду пользовательского интерфейса (общий проект или PCL).

Как и в стандартном кросс-платформенном приложении, существующие 32-разрядные типы данных должны использоваться в любом многоплатформенном коде в большинстве случаев. Новые собственные типы данных следует использовать только при вызове интерфейса API Mac или iOS, где требуется поддержка типов, поддерживающих архитектуру.

Дополнительные сведения см. в документации по [обновлению существующих приложений Xamarin. Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) .

## <a name="summary"></a>Сводка

В этой статье мы рассмотрели, когда использовать собственные типы данных в Unified API приложении и их последствия для разных платформ. Мы предпредставил несколько решений, которые можно использовать в ситуациях, когда новые собственные типы данных должны использоваться в межплатформенных библиотеках. Кроме того, мы рассмотрели краткое руководство по поддержке унифицированных интерфейсов API в приложениях на разных платформах Xamarin. Forms.

## <a name="related-links"></a>Связанные ссылки

- [Unified API](~/cross-platform/macios/unified/index.md)
- [Собственные типы](~/cross-platform/macios/nativetypes.md)
- [Параметры общего доступа к коду](~/cross-platform/app-fundamentals/code-sharing.md)
- [Пример общего доступа к коду](/samples/xamarin/mobile-samples/sharingcode/)