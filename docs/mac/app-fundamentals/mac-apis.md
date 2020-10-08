---
title: API-интерфейсы macOS для разработчиков Xamarin. Mac
description: В этом документе описывается чтение селекторов цели-C и способы поиска соответствующих методов C# в приложении Xamarin. Mac.
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/02/2017
ms.openlocfilehash: efac76338af710e716decf47635652bf8de74910
ms.sourcegitcommit: 6d347e1d7641ac1d2b389fb1dc7a6882a08f7c00
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/08/2020
ms.locfileid: "91851513"
---
# <a name="macos-apis-for-xamarinmac-developers"></a>API-интерфейсы macOS для разработчиков Xamarin. Mac

## <a name="overview"></a>Обзор

Для большей части времени разработки с помощью Xamarin. Mac вы можете думать, читать и писать на C#, не заботясь о базовых API-интерфейсах целевой платформы. Однако иногда вам потребуется прочитать документацию по API от Apple, преобразовать ответ из Stack Overflow в решение проблемы или сравнить с существующим примером.

## <a name="reading-enough-objective-c-to-be-dangerous"></a>Чтение достаточной цели-C, чтобы быть опасной

Иногда потребуется прочитать определение цели или вызов метода и перевести его в эквивалентный метод C#. Давайте взглянем на определение функции-C и разбейте его на части. Этот метод ( *селектор* в цели-C) можно найти в `NSTableView` :

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

Объявление может быть прочитано слева направо:

- `-`Префикс означает, что это метод экземпляра (не статический). + означает, что это метод класса (статический)
- `(BOOL)` Тип возвращаемого значения (bool в C#)
- `canDragRowsWithIndexes` Первая часть имени.
- `(NSIndexSet *)rowIndexes` параметр является первым параметром и имеет тип. Первый параметр имеет формат: `(Type) paramName`
- `atPoint:(NSPoint)mouseDownPoint` Второй параметр и его тип. Каждый параметр после первого является форматом: `selectorPart:(Type) paramName`
- Полное имя этого селектора сообщений: `canDragRowsWithIndexes:atPoint:` . Обратите внимание, что в `:` конечном итоге это важно.
- Фактическая привязка C# для Xamarin. Mac: `bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

Этот вызов селектора может быть прочитан таким же образом:

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- Экземпляр `v` имеет свой `canDragRowsWithIndexes:atPoint` селектор, вызываемый с двумя параметрами, `set` и `point` , переданным в.
- В C# вызов метода выглядит следующим образом: `v.CanDragRows (set, point);`

<a name="finding_selector"></a>

## <a name="finding-the-c-member-for-a-given-selector"></a>Поиск элемента C# для данного селектора

Теперь, когда вы нашли селектор цели-C, который необходимо вызвать, следующий шаг сопоставляется с эквивалентным элементом C#. Существует четыре подхода, которые можно попробовать (продолжение работы с `NSTableView CanDragRows` примером):

1. Используйте список автозавершения, чтобы быстро проверить наличие что-то с тем же именем. Так как известно, что это экземпляр, `NSTableView` можно ввести:

    - `NSTableView x;`
    - `x.` [Ctrl + пробел, если список не отображается).
    - `CanDrag` входить
    - Щелкните метод правой кнопкой мыши, перейдите к объявлению, чтобы открыть браузер сборок, где можно сравнить `Export` атрибут с выбранным селектором.

2. Поиск по всей привязке класса. Так как известно, что это экземпляр, `NSTableView` можно ввести:

    - `NSTableView x;`
    - Щелчок правой кнопкой мыши `NSTableView` , переход к объявлению в браузере сборки
    - Поиск рассматриваемого селектора

3. Вы можете использовать [интерактивную документацию по API Xamarin. Mac](/dotnet/api/?view=xamarinmac-3.0) .

4. Мигель предоставляет представление "Розеттском" для API Xamarin. Mac [здесь](https://tirania.org/tmp/rosetta.html) , где можно выполнять поиск по заданному API. Если ваш API не является AppKit или macOS, его можно найти там.

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
