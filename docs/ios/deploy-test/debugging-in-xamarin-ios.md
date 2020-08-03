---
title: Отладка приложений Xamarin.iOS
description: Этот документ описывает, как использовать отладчик в Visual Studio для Mac или Visual Studio 2019 для отладки приложения Xamarin.iOS, включая задание точек останова, беспроводную отладку и многое другое.
ms.prod: xamarin
ms.assetid: 05460010-99E1-DC38-F855-2D691EF54484
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: a5135d9bb481afcaefc851e392196a87b8ef80dd
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997206"
---
# <a name="debugging-xamarinios-apps"></a>Отладка приложений Xamarin.iOS

_Для отладки приложений Xamarin.iOS можно использовать встроенный отладчик Visual Studio или Visual Studio для Mac._

Используйте встроенное средство отладки Visual Studio для Mac при отладке кода C# и других управляемых языков. Используйте [LLDB](https://lldb.llvm.org/tutorial.html) при отладке кода C, C++ или Objective C, привязываемого к проекту Xamarin.iOS.

> [!NOTE]
> При компиляции в режиме отладки приложения характеризуются низкой скоростью и большим размером, так как Xamarin.iOS нужно инструментировать каждую строку кода. Обязательно переведите сборку в режим выпуска, прежде чем осуществлять выпуск.

Отладчик Xamarin.iOS интегрирован в IDE и позволяет разработчикам выполнять отладку приложений Xamarin.iOS, созданных на любом поддерживаемом Xamarin.iOS управляемом языке, в симуляторе или на устройстве.

Отладчик Xamarin.iOS использует ["мягкий" отладчик Mono](https://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/), который обеспечивает взаимодействие сгенерированного кода и среды выполнения Mono со средой IDE в процессе отладки. Этим он отличается от "жестких" отладчиков, таких как LLDB или MDB, которые контролируют выполнение отлаживаемой программы без взаимодействия с ней и учета ее особенностей.

## <a name="setting-breakpoints"></a>Задание точек останова

Приступая к процессу отладки, в первую очередь необходимо [задать для приложения точки останова](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint). Для этого щелкните на полях редактора рядом с номером той строки кода, где нужно установить точку:

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Задание точек останова](debugging-in-xamarin-ios-images/debugging1.png)](debugging-in-xamarin-ios-images/debugging1.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Задание точек останова](debugging-in-xamarin-ios-images/debugging1a.png)](debugging-in-xamarin-ios-images/debugging1a.png#lightbox)

-----

Вы можете просмотреть все точки останова, заданные в коде, на **панели точек останова**:

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Панель точек останова](debugging-in-xamarin-ios-images/image0a.png)](debugging-in-xamarin-ios-images/image0a.png#lightbox)

 Если панель точки останова не отображается автоматически, выберите _"Представление" > "Отладка Windows" > "Точки останова"_

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Панель точек останова](debugging-in-xamarin-ios-images/image0.png)](debugging-in-xamarin-ios-images/image0.png#lightbox)

 Если панель точки останова не отображается автоматически, выберите _"Отладка" > "Windows" > "Точки останова"_

-----

Прежде чем приступать к отладке любого приложения, обязательно переведите конфигурацию в режим **Отладки**, поскольку он содержит полезный набор инструментов для выполнения отладки, включая точки останова, визуализаторы данных и просмотр стека вызовов:

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Отладка в симуляторе](debugging-in-xamarin-ios-images/debugging7.png)](debugging-in-xamarin-ios-images/debugging7.png#lightbox)
[![Отладка на физическом устройстве](debugging-in-xamarin-ios-images/debugging7a.png)](debugging-in-xamarin-ios-images/debugging7a.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Отладка в симуляторе](debugging-in-xamarin-ios-images/debugging7c.png)](debugging-in-xamarin-ios-images/debugging7c.png#lightbox)
[![Отладка на физическом устройстве](debugging-in-xamarin-ios-images/debugging7d.png)](debugging-in-xamarin-ios-images/debugging7d.png#lightbox)

-----

## <a name="start-debugging"></a>Начало отладки
Чтобы начать отладку, выберите целевое или аналогичное устройство в IDE:

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Выбор целевого устройства](debugging-in-xamarin-ios-images/debugging7b.png)](debugging-in-xamarin-ios-images/debugging7b.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Выбор целевого устройства](debugging-in-xamarin-ios-images/debugging7e.png)](debugging-in-xamarin-ios-images/debugging7e.png#lightbox)

-----

После этого разверните приложение, нажав кнопку **Воспроизвести**.

При попадании в точку останова код выделяется желтым цветом:

[![Код будет выделен желтым цветом](debugging-in-xamarin-ios-images/image2.png)](debugging-in-xamarin-ios-images/image2.png#lightbox)

На этом этапе вы можете использовать средства отладки, например проверку значений объектов, для получения дополнительных сведений о том, что происходит в коде:

[![Отображение значения цвета](debugging-in-xamarin-ios-images/image3.png)](debugging-in-xamarin-ios-images/image3.png#lightbox)

## <a name="conditional-breakpoints"></a>Условные точки останова

Вы также можете задавать правила, определяющие условия активации точки останова. Это называется добавлением *условной точки останова*.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Чтобы задать условную точку останова, перейдите в окно **Свойства точки останова**, что можно сделать двумя способами:

- Чтобы добавить условную точку останова, щелкните правой кнопкой мыши поле редактора слева от номера строки кода, где требуется задать точку останова, а затем выберите пункт "Создать точку останова":

  [![Выбор новой точки останова](debugging-in-xamarin-ios-images/image4.png)](debugging-in-xamarin-ios-images/image4.png#lightbox)

- Чтобы добавить условие в существующую точку останова, щелкните ее правой кнопкой мыши и выберите пункт ***Свойства точки останова** или на **панели точек останова** нажмите кнопку свойств, как показано ниже:

  [![Панель точек останова](debugging-in-xamarin-ios-images/image5.png)](debugging-in-xamarin-ios-images/image5.png#lightbox)

Затем вы можете ввести условие, при котором эта точка останова активируется:

[![Ввод условия для точки останова](debugging-in-xamarin-ios-images/image6.png)](debugging-in-xamarin-ios-images/image6.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Чтобы задать условную точку останова в Visual Studio, сначала [задайте обычную точку останова](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint). Щелкните точку останова правой кнопкой мыши, чтобы открыть контекстное меню:

 [![Контекстное меню точки останова](debugging-in-xamarin-ios-images/image4vs.png)](debugging-in-xamarin-ios-images/image4vs.png#lightbox)

Выберите пункт **Условия…** , чтобы открыть меню _Параметры точки останова_:

 [![Меню параметров точки останова](debugging-in-xamarin-ios-images/image6vs.png)](debugging-in-xamarin-ios-images/image6vs.png#lightbox)

Здесь вы можете ввести условия, при которых эта точка останова активируется

Дополнительные сведения об использовании условных точек останова в более ранних версиях Visual Studio см. в [документации по Visual Studio](https://docs.microsoft.com/visualstudio/debugger/using-breakpoints).

-----

## <a name="navigating-through-code"></a>Переход по коду

При достижении точки останова инструменты отладки позволяют получить контроль над выполнением программы. В вашей IDE появятся четыре кнопки для запуска и пошагового выполнения кода.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

В Visual Studio для Mac они будут выглядеть так:

 [![Инструменты отладки позволяют разработчику контролировать выполнение программы](debugging-in-xamarin-ios-images/image7.png)](debugging-in-xamarin-ios-images/image7.png#lightbox)

Эти особые значения приведены ниже.

- **Воспроизведение/остановка** — запускает и останавливает выполнение кода до следующей точки останова.
- **Шаг с обходом** — выполняет следующую строку кода. Если она содержит вызов функции, эта функция выполняется, а затем выполнение останавливается на _следующей_ за ней строке.
- **Шаг с заходом** — также выполняет следующую строку кода. Если она является вызовом функции, эта команда останавливается на первой строке функции, позволяя продолжить ее отладку по строкам. Если следующая строка не является функцией, команда работает аналогично шагу с обходом.
- **Шаг с выходом** — возвращается к строке, где была вызвана текущая функция.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

В Visual Studio они будут выглядеть так:

[![Инструменты отладки позволяют разработчику контролировать выполнение программы](debugging-in-xamarin-ios-images/image7vs.png)](debugging-in-xamarin-ios-images/image7vs.png#lightbox)

Эти особые значения приведены ниже.

- **Воспроизведение/остановка** — запускает и останавливает выполнение кода до следующей точки останова.
- **Шаг с обходом (F11)**  — выполняет следующую строку кода. Если она содержит вызов функции, эта функция выполняется, а затем выполнение останавливается на _следующей_ за ней строке.
- **Шаг с заходом (F10)**  — также выполняет следующую строку кода. Если она является вызовом функции, эта команда останавливается на первой строке функции, позволяя продолжить ее отладку по строкам. Если следующая строка не является функцией, команда работает аналогично шагу с обходом.
- **Шаг с выходом (Shift + F11)**  — возвращается к строке, где была вызвана текущая функция.

Дополнительные сведения в об отладке см. в документации по [перемещению по коду с помощью отладчика Visual Studio](https://docs.microsoft.com/visualstudio/debugger/navigating-through-code-with-the-debugger).

-----

### <a name="breakpoints"></a>Точки останова

Необходимо отметить, что iOS дает приложению всего несколько секунд (10) на запуск и выполнение метода `FinishedLaunching` в делегате приложения. Если приложение не выполнит метод в течение 10 секунд, iOS прервет этот процесс.

Это означает, что задать точки останова в коде запуска вашей программы практически невозможно. Если вы хотите выполнить отладку кода запуска, необходимо задержать часть его инициализации и поместить ее в метод, вызываемый таймером, или в любой другой метод обратного вызова, который выполняется после завершения процесса FinishedLaunching.

## <a name="device-diagnostics"></a>Диагностика устройства

Если при настройке отладчика возникает ошибка, включите подробную диагностику, добавив "-v -v -v" к дополнительным аргументам mtouch в параметрах проекта. Это позволит отобразить подробные сведения об ошибке в консоли устройства.

 <a name="WiFi_Debugging"></a>

## <a name="wireless-debugging"></a>Отладка по беспроводному соединению

По умолчанию Xamarin.iOS использует USB-подключение для отладки приложений на устройствах. Однако иногда при разработке приложений, в которых применяется ExternalEccessory, необходимо использовать USB-устройство для проверки подключения и отключения кабеля. В таких случаях вы можете выполнять отладку по беспроводной сети.

Дополнительные сведения о развертывании и отладке по беспроводному соединению см. в [этом руководстве](~/ios/deploy-test/wireless-deployment.md).

<a name="Technical_Details"></a>

## <a name="technical-details"></a>Технические сведения

Xamarin.iOS использует новый "мягкий" отладчик Mono. В отличие от стандартного отладчика Mono, который представляет собой программу, управляющую отдельным процессом с помощью интерфейсов операционной системы, "мягкий" отладчик открывает доступ к функциям отладки среды выполнения Mono через соединительный протокол.

При запуске отлаживаемое приложение обращается к отладчику, и отладчик начинает работу. Xamarin.iOS для Visual Studio использует агент Xamarin для Mac в качестве посредника между приложением (в Visual Studio) и отладчиком.

Когда отладка выполняется на устройстве, "мягкий" отладчик обеспечивает тесное взаимодействие с приложением. Это означает, что двоичные файлы сборки, создаваемые в процессе отладки, будут иметь больший размер, так как отладка подразумевает инструментирование с включением дополнительного кода в каждой точке последовательности.

<a name="Accessing_the_Console"></a>

## <a name="accessing-the-console"></a>Доступ к консоли

Журналы аварийного завершения и выходные данные класса консоли будут отправляться в консоль iPhone. Для получения доступа к этой консоли используйте Organizer (Организатор) в Xcode, выбрав в нем нужное устройство.

Если вы не хотите запускать Xcode, воспользуйтесь программой настройки iPhone Configuration Utility от Apple, обеспечивающей прямой доступ к консоли. Она также позволяет открывать журналы консоли с компьютера Windows, если вы выполняете отладку в полевых условиях.

Для пользователей Visual Studio в окне вывода доступно несколько журналов, однако для получения более подробных журналов перейдите на компьютер Mac.

-----

<a name="Debugging_Mono's_Class_Libraries"></a>

## <a name="debugging-monos-class-libraries"></a>Отладка библиотек классов Mono

Xamarin.iOS поставляется с исходным кодом для библиотек классов Mono, что позволяет вам легко изучать внутренние взаимодействия через пошаговое выполнение из отладчика.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Поскольку эта функция требует больше памяти во время отладки, по умолчанию она отключена.

Чтобы включить ее, снимите флажок **Отладка только кода проекта (без захода в код платформы)** в меню _"Visual Studio для Mac" > "Параметры" > "Отладчик"_ , как показано ниже:

[![Отладка библиотек классов Mono](debugging-in-xamarin-ios-images/debugging6.png)](debugging-in-xamarin-ios-images/debugging6.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Для отладки библиотек классов в Visual Studio необходимо снять флажок **Только мой код** в меню _"Отладка" > "Параметры"_ . В узле _"Отладка" > "Общие"_ снимите флажок **Включить только мой код**:

[![Отладка библиотек классов Mono](debugging-in-xamarin-ios-images/debugging6vs.png)](debugging-in-xamarin-ios-images/debugging6vs.png#lightbox)

-----

После этого вы сможете запускать приложение и использовать пошаговое выполнение для любой из базовых библиотек классов Mono.

## <a name="related-links"></a>Связанные ссылки

- [Отладка с помощью Xamarin](/visualstudio/mac/debugging/)
- [Визуализация данных](/visualstudio/mac/data-visualizations/)
- [Задание точки останова](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint)
- [Пошаговое прохождение кода](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code)
- [Вывод информации в окно журнала](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window)
