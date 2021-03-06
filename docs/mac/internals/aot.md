---
title: Компиляция Xamarin. Mac перед компиляцией времени
description: В этом документе описывается Предварительная компиляция по времени в Xamarin. Mac. Он сравнивает компиляцию AOT с JIT-компиляцией, объясняет, как включить AOT и рассматривает гибридную AOT.
ms.prod: xamarin
ms.assetid: 38B8A017-5A58-429C-A6E9-9860A1DCEF63
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 11/10/2017
ms.openlocfilehash: dac98ba74f389bec9016e52fa7a3f2f34ec71f0a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029970"
---
# <a name="xamarinmac-ahead-of-time-compilation"></a>Компиляция Xamarin. Mac перед компиляцией времени

## <a name="overview"></a>Обзор

Компиляция до времени (AOT) — это мощный метод оптимизации для повышения производительности при запуске. Однако это также влияет на время сборки, размер приложения и выполнение программы с помощью более глубокого способа. Чтобы понять, какие компромиссы он накладывает, мы собираемся немного познакомиться с компиляцией и выполнением приложения.

Код, написанный на управляемых языках, C# таких F#как и, компилируется в промежуточное представление с именем IL. Этот IL, хранящийся в библиотеках и сборках программ, является относительно компактным и переносимым между архитектурой процессора. Однако IL является только промежуточным набором инструкций и в некоторый момент, что IL необходимо преобразовать в машинный код, относящийся к процессору.

Эту обработку можно выполнить двумя точками:

- **JIT — во** время запуска и выполнения приложения IL компилируется в память в машинный код.
- **Впереди времени (AOT)** — во время сборки Il компилируется и записывается в собственные библиотеки и хранится в пакете приложений.

Каждый вариант имеет ряд преимуществ и компромиссов:

- **JIT**
  - **Время запуска** — JIT-компиляция должна выполняться при запуске. Для большинства приложений это происходит в порядке 100 мс, но для больших приложений это время может быть значительно больше.
  - **Выполнение** — так как JIT-код можно оптимизировать для конкретного используемого процессора, можно создать немного более качественный код. В большинстве приложений это несколько процентных точек быстрее.
- **AOT**
  - **Время запуска** — Загрузка предварительно скомпилированного дилибс выполняется значительно быстрее, чем JIT-сборки.
  - **Место на диске** — эти дилибс могут занимать значительный объем дискового пространства. В зависимости от того, какие сборки являются Аотед, это может быть вдвое или более размера части кода приложения.
  - **Время сборки** — компиляция AOT значительно ЗАМЕДЛЯЕТ работу JIT и замедляет сборки с ее использованием. Это значение может варьироваться в диапазоне от секунд до минуты или более, в зависимости от размера и количества скомпилированных сборок.
  - **Маскировка** — в качестве IL, который значительно упрощает процесс реконструирования, чем машинный код, необязательно требуется, чтобы его можно было устранить, чтобы упростить процесс маскировки для конфиденциального кода. Для этого требуется "гибридная" функция, описанная ниже.

## <a name="enabling-aot"></a>Включение AOT

Параметры AOT будут добавлены в область сборки Mac в будущем обновлении. До этого момента включение AOT требует передачи аргумента командной строки через поле "дополнительные аргументы MMP" в сборке Mac. Это свойство может принимать следующие значения.

```
--aot[=VALUE]          Specify assemblies that should be AOT compiled
                          - none - No AOT (default)
                          - all - Every assembly in MonoBundle
                          - core - Xamarin.Mac, System, mscorlib
                          - sdk - Xamarin.Mac.dll and BCL assemblies
                          - |hybrid after option enables hybrid AOT which
                          allows IL stripping but is slower (only valid
                          for 'all')
                          - Individual files can be included for AOT via +
                          FileName.dll and excluded via -FileName.dll

                          Examples:
                            --aot:all,-MyAssembly.dll
                            --aot:core,+MyOtherAssembly.dll,-mscorlib.dll
```

## <a name="hybrid-aot"></a>Гибридная AOT

Во время выполнения приложения macOS среда выполнения по умолчанию использует машинный код, загруженный из собственных библиотек, созданных компиляцией AOT. Однако существуют некоторые области кода, такие как трамполинес, где JIT-компиляция может значительно оптимизировать результаты. Для этого необходимо, чтобы на языке IL были доступны управляемые сборки. В iOS приложения ограничиваются любым использованием JIT-компиляции. Эти фрагменты кода также компилируются в AOT.

Гибридный вариант предписывает компилятору компилировать эти разделы (например, iOS), а также предполагает, что IL будет недоступен во время выполнения. Затем этот IL может быть удален после сборки. Как отмечалось выше, среда выполнения будет вынуждена использовать менее оптимизированные подпрограммы в некоторых местах.

## <a name="further-considerations"></a>Дополнительные вопросы

Отрицательные последствия масштабирования AOT с учетом размеров и количества обработанных сборок. Полная [Целевая платформа](~/mac/platform/target-framework.md) , например, содержит значительно более крупную библиотеку базовых классов (BCL), чем современная, и, таким образом, AOT занимает значительно больше времени и создает более крупные пакеты. Это связано с несовместимостью полной целевой платформы с компоновкой, которая удаляет неиспользуемый код. Рассмотрите возможность переноса приложения на современные и включения связывания для получения наилучших результатов.

Одним из дополнительных преимуществ AOT является улучшенное взаимодействие с отладкой машинного кода и цепочек инструментов профилирования. Поскольку большая часть базы кода будет скомпилирована заранее, она будет содержать имена функций и символы, которые проще читать в собственных отчетах о сбоях, профилировании и отладке. Функции, созданные JIT-компилятором, не имеют этих имен и часто отображаются как неименованные шестнадцатеричные смещения, которые очень трудно устранить.
