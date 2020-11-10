---
ms.assetid: EA2D979E-9151-4CE9-9289-13B6A979838B
title: Использование библиотек C/C++ в Xamarin
description: Visual Studio для Mac можно использовать для создания кроссплатформенного кода C++/C и его интеграции в мобильные приложения для Android и iOS с помощью Xamarin и C#. В этой статье объясняется, как настроить и отладить проект C++ в приложении Xamarin.
author: mikeparker104
ms.author: miparker
ms.date: 11/07/2019
ms.openlocfilehash: 8422affab86ea176cad4e57833188dcd5738a99a
ms.sourcegitcommit: d1980b2251999224e71c1289e4b4097595b7e261
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2020
ms.locfileid: "92928507"
---
# <a name="use-cc-libraries-with-xamarin"></a>Использование библиотек C/C++ в Xamarin

## <a name="overview"></a>Обзор

Xamarin позволяет разработчикам создавать собственные кроссплатформенные мобильные приложения с использованием Visual Studio. Как правило, для предоставления разработчикам существующих компонентов платформы используются привязки C#. Однако бывают случаи, когда приложениям Xamarin требуется работать с существующими базами кода. Иногда у команд разработчиков просто нет времени, бюджета или ресурсов для переноса больших, хорошо протестированных и оптимизированных баз кода C#.

[Visual C++ для разработки кроссплатформенных мобильных приложений](/visualstudio/cross-platform/visual-cpp-for-cross-platform-mobile-development) позволяет создавать код C/C++ и C# в рамках одного решения, предлагая множество преимуществ, включая унифицированный процесс отладки. Корпорация Майкрософт использует C/C++ и Xamarin, чтобы предоставлять такие приложения, как [Hyperlapse Mobile](https://www.microsoft.com/p/hyperlapse-mobile/9wzdncrd1prw) и [Pix Camera](https://www.microsoft.com/microsoftpix).

Однако в некоторых случаях хочется (или требуется) сохранить существующие инструменты и процессы C/C++, а также отделить код библиотек от приложения, чтобы библиотеки использовались как сторонние компоненты. В таких ситуациях необходимо не только предоставить соответствующие элементы в C#, но и управлять библиотекой как зависимостью. И, конечно, следует автоматизировать как можно большую часть этого процесса.  

В этой публикации рассматривается обобщенный подход к этому сценарию и демонстрируется простой пример.

## <a name="background"></a>История

C/C++ считается кроссплатформенным языком, но необходимо уделить исходному коду особое внимание, чтобы он стал действительно кроссплатформенным, используя только версии C/C++, поддерживаемые всеми целевыми компиляторами, и практически не используя условно добавляемый код для определенных платформ или компиляторов.

В конечном итоге код должен успешно компилироваться и выполняться на всех целевых платформах, поэтому все сводится к унификации кода для нескольких целевых платформ (и компиляторов). Проблемы могут возникать и из-за незначительных различий между компиляторами, поэтому становится все более важным тщательное тестирование (желательно автоматическое) на каждой целевой платформе.  

## <a name="high-level-approach"></a>Обобщенный подход

На рисунке ниже представлен подход из четырех этапов, используемый для преобразования исходного кода C/C++ в кроссплатформенную библиотеку Xamarin, которая предоставляется посредством NuGet, а затем используется в приложении Xamarin.Forms.

![Обобщенный подход к использованию C/C++ в Xamarin](images/cpp-steps.jpg)

Этот подход состоит из 4 этапов:

1. компиляция исходного кода C/C++ в собственные библиотеки платформы;
2. создание оболочки для собственных библиотек с помощью решения Visual Studio;
3. упаковка и отправка пакета NuGet для оболочки .NET;
4. использование пакета NuGet из приложения Xamarin.

### <a name="stage-1-compiling-the-cc-source-code-into-platform-specific-native-libraries"></a>Этап 1. Компиляция исходного кода C/C++ в собственные библиотеки платформы

Цель этого этапа — создать собственные библиотеки, которые могут вызываться оболочкой C#. В зависимости от конкретной ситуации это может быть уместным или неуместным. Многие инструменты и процессы, которые могут быть включены в этот распространенный сценарий, выходят за рамки данной статьи. Основной идеей является обеспечение синхронизации базы кода C/C++ с любым собственным кодом оболочки, достаточное модульное тестирование и автоматизация сборки.

Библиотеки в пошаговом руководстве были созданы в Visual Studio Code с помощью сопутствующего сценария оболочки. Расширенную версию данного пошагового руководства можно найти в [ репозитории GitHub Mobile CAT](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin), в котором более подробно рассматривается эта часть примера. В этом случае собственные библиотеки обрабатываются как сторонняя зависимость, однако этот этап показан для контекста.

Для простоты в этом пошаговом руководстве рассматривается только подмножество архитектур. В iOS используется служебная программа lipo для создания одного расширенного двоичного файла на основе отдельных двоичных файлов конкретной архитектуры. Android будет использовать динамические двоичные файлы с расширением ".so", а iOS будет использовать статический расширенный двоичный файл с расширением ".a".

### <a name="stage-2-wrapping-the-native-libraries-with-a-visual-studio-solution"></a>Этап 2. Создание оболочки для собственных библиотек с помощью решения Visual Studio

Следующий этап заключается в заключении в оболочку собственных библиотек, чтобы их можно было легко использовать в .NET. Для этого используется решение Visual Studio с четырьмя проектами. В общем проекте содержится общий код. Проекты, нацеленные на Xamarin.Android, Xamarin.iOS и .NET Standard, позволяют ссылаться на библиотеку независимым от платформы образом.

Оболочка использует [прием с подменой](https://github.com/JFMG/Bait-and-Switch-PCL-example). Это не единственный способ, но он упрощает добавление ссылки на библиотеку и позволяет избежать необходимости явно управлять реализациями, зависящими от платформы, в самом приложении. Этот прием гарантирует, что целевые платформы (.NET Standard, Android и iOS) совместно используют одно и то же пространство имен, имя сборки и структуру классов. Так как NuGet всегда предпочитает библиотеку для конкретной платформы, версия .NET Standard никогда не используется во время выполнения.

Большая часть усилий на этом шаге посвящена использованию P/Invoke для вызова методов собственных библиотек и управления ссылками на базовые объекты. Задача заключается в предоставлении функциональных возможностей библиотеки потребителю и абстрагировании любой сложности. Разработчикам для Xamarin.Forms не нужно знать, как работает неуправляемая библиотека изнутри. Они должны использовать ее, как любую другую управляемую библиотеку C#.

В конечном итоге результатом этого этапа будет набор библиотек .NET (по одной для каждой целевой платформы), а также NUSPEC-файл, содержащий сведения, необходимые для сборки пакета на следующем шаге.

**Этап 3. Упаковка и отправка пакета NuGet для оболочки .NET**

Третий этап — создание пакета NuGet с помощью артефактов сборки из предыдущего шага. Результатом этого этапа является пакет NuGet, который можно использовать из приложения Xamarin. В данном пошаговом руководстве в качестве веб-канала NuGet используется локальный каталог. В рабочей среде на этом шаге пакет должен публиковаться в общедоступном или частном веб-канале NuGet, и этот процесс должен быть полностью автоматизирован.

**Этап 4. Использование пакета NuGet из приложения Xamarin.Forms**

Последним шагом является добавление ссылки на пакет NuGet и его использование из приложения Xamarin.Forms. Для этого необходимо настроить веб-канал NuGet в Visual Studio, чтобы использовать веб-канал, определенный на предыдущем шаге.

После настройки веб-канала ссылку на пакет нужно добавить в каждый проект в кроссплатформенном приложении Xamarin.Forms. Прием с подменой обеспечивает идентичные интерфейсы, поэтому функции собственной библиотеки можно вызывать с помощью кода, определенного в одном месте.

Репозиторий исходного кода содержит [список дополнительных статей](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin#wrapping-up) о настройке частного веб-канала NuGet в Azure DevOps и о том, как отправить пакет в этот веб-канал. Хотя для настройки такого веб-канала потребуется немного больше времени, чем для настройки локального каталога, этот тип веб-канала лучше использовать в среде коллективной разработки.

## <a name="walk-through"></a>Пошаговое руководство

Приведенные действия относятся к **Visual Studio для Mac** , но описанная структура подходит и для **Visual Studio 2017**.

### <a name="prerequisites"></a>Предварительные требования

Для работы разработчику потребуется:

- [командная строка NuGet](/nuget/tools/nuget-exe-cli-reference#macoslinux);

- [*Visual Studio* *для Mac*](https://visualstudio.microsoft.com/downloads).

> [!NOTE]
> Для развертывания приложений на iPhone требуется активная [**учетная запись разработчика Apple**](https://developer.apple.com/).

## <a name="creating-the-native-libraries-stage-1"></a>Создание собственных библиотек (этап 1)

Функциональные возможности собственной библиотеки основаны на примере из раздела [Пошаговое руководство. Создание и использование статической библиотеки (C++)](/cpp/windows/walkthrough-creating-and-using-a-static-library-cpp?view=vs-2017).

В этом пошаговом руководстве пропускается первый этап — создание собственных библиотек, так как в этом сценарии библиотека предоставляется как сторонняя зависимость. Предварительно скомпилированные собственные библиотеки можно добавить вместе с [примером кода](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin) или [скачать](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin/Sample/Artefacts) напрямую.

### <a name="working-with-the-native-library"></a>Работа с собственной библиотекой

Исходный пример *MathFuncsLib* содержит отдельный класс с `MyMathFuncs` со следующим определением.

```cpp
namespace MathFuncs
{
    class MyMathFuncs
    {
    public:
        double Add(double a, double b);
        double Subtract(double a, double b);
        double Multiply(double a, double b);
        double Divide(double a, double b);
    };
}
```

Дополнительный класс определяет функции-оболочки, позволяющие потребителю .NET создавать, удалять базовый собственный класс `MyMathFuncs` и взаимодействовать с ним.

```cpp
#include "MyMathFuncs.h"
using namespace MathFuncs;

extern "C" {
    MyMathFuncs* CreateMyMathFuncsClass();
    void DisposeMyMathFuncsClass(MyMathFuncs* ptr);
    double MyMathFuncsAdd(MyMathFuncs *ptr, double a, double b);
    double MyMathFuncsSubtract(MyMathFuncs *ptr, double a, double b);
    double MyMathFuncsMultiply(MyMathFuncs *ptr, double a, double b);
    double MyMathFuncsDivide(MyMathFuncs *ptr, double a, double b);
}
```

Это функции-оболочки будут использоваться на стороне [Xamarin](https://visualstudio.microsoft.com/xamarin/).

## <a name="wrapping-the-native-library-stage-2"></a>Создание оболочки для собственной библиотеки (этап 2)

Для этого этапа требуются [предварительно скомпилированные библиотеки](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin/Sample/Artefacts), описанные в [предыдущем разделе](#creating-the-native-libraries-stage-1).

### <a name="creating-the-visual-studio-solution"></a>Создание решения Visual Studio

1. В **Visual Studio для Mac** щелкните **Новый проект** (на *странице приветствия* ) или **Новое решение** (в меню *Файл* ).
2. В окне **Новый проект** выберите **Общий проект** ( *Многоплатформенность > Библиотека* ), а затем нажмите кнопку **Далее**.
3. Заполните перечисленные ниже поля и нажмите кнопку **Создать**.

    - **Имя проекта** : MathFuncs.Shared  
    - **Имя решения** : MathFuncs  
    - **Расположение**. укажите папку для сохранения по умолчанию (или выберите другую папку).   
    - **Создать каталог проекта в каталоге решения** : установите этот флажок.
4. В **обозревателе решений** дважды щелкните проект **MathFuncs.Shared** и перейдите к разделу **Основные параметры**.
5. Удалите **.Shared** из поля **Пространство имен по умолчанию** , оставив только **MathFuncs** , а затем нажмите кнопку **ОК**.
6. Откройте **MyClass.cs** (созданный шаблоном), а затем переименуйте класс и файл в **MyMathFuncsWrapper** и измените пространство имен на **MathFuncs**.
7. **Нажмите клавишу CONTROL и щелкните** решение **MathFuncs** , а затем в меню **Добавить** выберите **Добавить новый проект...** .
8. В окне **Новый проект** выберите **Библиотека .NET Standard** ( *Многоплатформенность > Библиотека* ), а затем нажмите кнопку **Далее**.
9. Выберите **.NET Standard 2.0** и нажмите кнопку **Далее**.
10. Заполните перечисленные ниже поля и нажмите кнопку **Создать**.

    - **Имя проекта** : MathFuncs.Standard  
    - **Расположение**. укажите то же расположение для сохранения, что и для общего проекта.   

11. В **обозревателе решений** дважды щелкните проект **MathFuncs.Standard**.
12. Перейдите в раздел **Основные параметры** , а затем в поле **Пространство имен по умолчанию** укажите **MathFuncs**.
13. Перейдите к параметрам **выходных данных** , а затем измените **имя сборки** на **MathFuncs**.
14. Перейдите к параметрам **компилятора** , измените параметр **Конфигурация** на **Выпуск** , для параметра **Отладочная информация** выберите значение **Только символы** , после чего нажмите кнопку **ОК**.
15. Удалите **Class1.cs/Getting Started** из проекта (если один из этих компонентов был включен в шаблон).
16. **Нажмите клавишу CONTROL и щелкните** в проекте папку **Dependencies/References** , а затем выберите **Изменить ссылки**.
17. На вкладке **Проекты** выберите **MathFuncs.Shared** , а затем нажмите кнопку **ОК**.
18. Повторите шаги 7–17 (пропустив шаг 9), используя приведенные ниже конфигурации.

    | **Имя проекта**  | **Имя шаблона**   | **Меню "Новый проект"**   |
    |-------------------| --------------------| -----------------------|
    | MathFuncs.Android | Библиотека классов       | Android > Библиотека      |
    | MathFuncs.iOS     | Библиотеки привязок     | iOS > Библиотека          |

19. В **обозревателе решений** дважды щелкните проект **MathFuncs.Android** и перейдите к параметрам **компилятора**.

20. Выбрав для параметра **Конфигурация** значение **Отладка** , измените значение **Определить символы** , чтобы добавить **Android;** .

21. Измените значение параметра **Конфигурация** на **Выпуск** , затем измените значение **Определить символы** , чтобы добавить **Android;** .

22. Повторите шаги 19–20 для **MathFuncs.iOS** , но добавляйте в значение **Определите символы** **iOS.** вместо **Android;** .

23. Выполните сборку решения с конфигурацией **Выпуск** ( **CONTROL + COMMAND + B** ) и убедитесь, что все три выходные сборки (для Android, iOS, .NET Standard, расположенные в соответствующих папках Bin проекта) используют один и тот же файл **MathFuncs.dll**.

На этом этапе в решении должны быть три цели, по одной для платформ Android, iOS и .NET Standard, а также общий проект, на который ссылается каждая из трех целей. Они должны быть настроены для использования одного и того же пространства имен по умолчанию и выходных сборок с одинаковым именем. Это необходимо для реализации приема с подменой, упомянутого ранее.

### <a name="adding-the-native-libraries"></a>Добавление собственных библиотек

Процесс добавления собственных библиотек в решение-оболочку незначительно различается для Android и iOS.

#### <a name="native-references-for-mathfuncsandroid"></a>Собственные ссылки для MathFuncs.Android

1. **Нажмите клавишу CONTROL и щелкните** проект **MathFuncs.Android** , а затем в меню **Добавить** выберите **Создать папку** и укажите имя **libs**.

2. Для каждого **ABI** (двоичный интерфейс приложения) **нажмите клавишу CONTROL и щелкните** папку **libs** , а затем в меню **Добавить** выберите **Создать папку** и укажите имя, соответствующее выбранному **ABI**. В данном случае:

    - arm64-v8a
    - armeabi-v7a
    - x86
    - x86_64  

    > [!NOTE]
    > Чтобы узнать больше, ознакомьтесь с разделом об [архитектурах и ЦП](https://developer.android.com/ndk/guides/arch) [руководства для разработчиков NDK](https://developer.android.com/ndk/guides/), в частности с разделом о [машинном коде в пакетах приложения](https://developer.android.com/ndk/guides/abis#native-code-in-app-packages).

3. Проверьте структуру папок.  

    ```folders
    - lib
        - arm64-v8a
        - armeabi-v7a
        - x86
        - x86_64
    ```

4. Добавьте соответствующие библиотеки **SO** -файлов в каждую папку **ABI** , следуя приведенной ниже схеме:

    **arm64-v8a:** libs/Android/arm64

    **armeabi-v7a:** libs/Android/arm  

    **x86:** libs/Android/x86

    **x86_64:** libs/Android/x86_64

    > [!NOTE]
    > Чтобы добавить файлы, **нажмите клавишу CONTROL и щелкните** папку соответствующего **ABI** , а затем в меню **Добавить** выберите **Добавить файлы**. Выберите соответствующую библиотеку (из каталога **PrecompiledLibs** ), щелкните **Открыть** и нажмите кнопку **ОК** , оставив выбранным параметр по умолчанию *Скопировать файл в каталог*.

5. **Нажмите клавишу CONTROL и щелкните** каждый **SO** -файл, затем в меню **Действие сборки** выберите **EmbeddedNativeLibrary**.

Теперь папка **libs** должна выглядеть следующим образом.

```folders
- lib
    - arm64-v8a
        - libMathFuncs.so
    - armeabi-v7a
        - libMathFuncs.so
    - x86
        - libMathFuncs.so
    - x86_64
        - libMathFuncs.so
```

#### <a name="native-references-for-mathfuncsios"></a>Собственные ссылки для MathFuncs.iOS

1. **Нажмите клавишу CONTROL и щелкните** проект **MathFuncs.iOS** , а затем в меню **Добавить** выберите **Добавить собственную ссылку**.
2. Выберите библиотеку **libMathFuncs.a** (из папки libs/ios в каталоге **PrecompiledLibs** ), а затем щелкните **Открыть**.
3. **Нажмите клавишу CONTROL и щелкните** файл **libMathFuncs** (в папке **Native References** ), затем выберите в меню пункт **Свойства**.  
4. Настройте свойства **собственной ссылки** , чтобы они были выбраны (помечены галочкой) на **Панели свойств**.

    - Принудительная загрузка
    - Относится к C++
    - Интеллектуальная связь

    > [!NOTE]
    > Использование типа проекта библиотеки привязки вместе с [собственной ссылкой](../macios/native-references.md) позволяет внедрить статическую библиотеку и автоматически связать ее с приложением Xamarin.iOS, которое ссылается на нее (даже если библиотека добавлена как пакет NuGet).

5. Откройте **ApiDefinition.cs** и удалите закомментированный шаблонный код (оставив только пространство имен `MathFuncs`), а затем выполните то же действие для файла **Structs.cs**.

    > [!NOTE]
    > Эти файлы (как и действия сборки *ObjCBindingApiDefinition* и *ObjCBindingCoreSource* ) нужны для сборки проекта библиотеки привязки. Однако мы напишем код, вызывающий нашу собственную библиотеку, за пределами этих файлов, чтобы его можно было совместно использовать в целевых объектах библиотек Android и iOS с помощью стандартного метода P/Invoke.

### <a name="writing-the-managed-library-code"></a>Написание кода управляемой библиотеки

Теперь напишите C# код для вызова собственной библиотеки. Наша задача — скрыть все сложности базовой структуры. Потребителю не нужны навыки работы с внутренними компонентами собственных библиотек или знания принципов работы P/Invoke.  

#### <a name="creating-a-safehandle"></a>Создание SafeHandle

1. **Нажмите клавишу CONTROL и щелкните** проект **MathFuncs.Shared** , а затем в меню **Добавить** выберите **Добавить файл**.
2. Выберите **Пустой класс** в окне **Новый файл** , назовите его **MyMathFuncsSafeHandle** , а затем щелкните **Создать**.
3. Реализуйте класс **MyMathFuncsSafeHandle**.

    ```csharp
    using System;
    using Microsoft.Win32.SafeHandles;

    namespace MathFuncs
    {
        internal class MyMathFuncsSafeHandle : SafeHandleZeroOrMinusOneIsInvalid
        {
            public MyMathFuncsSafeHandle() : base(true) { }

            public IntPtr Ptr => handle;

            protected override bool ReleaseHandle()
            {
                // TODO: Release the handle here
                return true;
            }
        }
    }
    ```

    > [!NOTE]
    > Рекомендуется использовать [SafeHandle](/dotnet/api/system.runtime.interopservices.safehandle?view=netframework-4.7.2) для работы с неуправляемыми ресурсами в управляемом коде. Это позволяет абстрагировать большой объем стандартного кода, связанного с критическим завершением и жизненным циклом объектов. Владелец этого дескриптора сможет впоследствии обрабатывать его так же, как и любой другой управляемый ресурс, и ему не придется реализовывать полный [высвобождаемый шаблон](/dotnet/standard/garbage-collection/implementing-dispose).

#### <a name="creating-the-internal-wrapper-class"></a>Создание внутреннего класса-оболочки

1. Откройте **MyMathFuncsWrapper.cs** , изменив его на внутренний статический класс.

    ```csharp
    namespace MathFuncs
    {
        internal static class MyMathFuncsWrapper
        {
        }
    }
    ```

2. В том же самом файле добавьте в класс приведенный ниже условный оператор.

    ```csharp
    #if Android
        const string DllName = "libMathFuncs.so";
    #else
        const string DllName = "__Internal";
    #endif
    ```

    > [!NOTE]
    > Это позволит задать значение константы **DllName** в зависимости от того, для какой платформы выполняется сборка библиотеки, **Android** или **iOS**. Это позволяет соблюсти различные соглашения об именовании, используемые каждой платформой, а также учесть тип библиотеки, используемой в каждом случае. Android использует динамическую библиотеку, поэтому ожидает имя файла, включая расширение. Для iOS требуется имя с " *__Internal* ", так как используется статическая библиотека.

3. Добавьте ссылку на **System.Runtime.InteropServices** в начало файла **MyMathFuncsWrapper.cs**.

    ```csharp
    using System.Runtime.InteropServices;
    ```

4. Добавьте методы-оболочки для создания и удаления класса **MyMathFuncs**.

    ```csharp
    [DllImport(DllName, EntryPoint = "CreateMyMathFuncsClass")]
    internal static extern MyMathFuncsSafeHandle CreateMyMathFuncs();

    [DllImport(DllName, EntryPoint = "DisposeMyMathFuncsClass")]
    internal static extern void DisposeMyMathFuncs(MyMathFuncsSafeHandle ptr);
    ```

    > [!NOTE]
    > Мы передаем константу **DllName** в атрибут **DllImport** вместе с **EntryPoint** , который явно указывает среде выполнения .NET имя функции для вызова в этой библиотеке. Технически не нужно предоставлять значение **EntryPoint** , если имена управляемых методов идентичны неуправляемым. Если значение **EntryPoint** не указано, то вместо него будет использоваться имя управляемого метода. Однако лучше задать это значение явно.  

5. Добавьте методы-оболочки, чтобы мы могли работать с классом **MyMathFuncs** , используя следующий код.

    ```csharp
    [DllImport(DllName, EntryPoint = "MyMathFuncsAdd")]
    internal static extern double Add(MyMathFuncsSafeHandle ptr, double a, double b);

    [DllImport(DllName, EntryPoint = "MyMathFuncsSubtract")]
    internal static extern double Subtract(MyMathFuncsSafeHandle ptr, double a, double b);

    [DllImport(DllName, EntryPoint = "MyMathFuncsMultiply")]
    internal static extern double Multiply(MyMathFuncsSafeHandle ptr, double a, double b);

    [DllImport(DllName, EntryPoint = "MyMathFuncsDivide")]
    internal static extern double Divide(MyMathFuncsSafeHandle ptr, double a, double b);
    ```

    > [!NOTE]
    > В этом примере мы используем простые типы для параметров. Так как маршалинг означает побитовое копирование, в этом случае с нашей стороны больше действий не требуется. Также обратите внимание на использование класса **MyMathFuncsSafeHandle** вместо стандартного **IntPtr**. **IntPtr** автоматически сопоставляется с **SafeHandle** в процессе маршалинга.

6. Убедитесь, что завершенный класс **MyMathFuncsWrapper** выглядит следующим образом.

    ```csharp
    using System.Runtime.InteropServices;

    namespace MathFuncs
    {
        internal static class MyMathFuncsWrapper
        {
            #if Android
                const string DllName = "libMathFuncs.so";
            #else
                const string DllName = "__Internal";
            #endif

            [DllImport(DllName, EntryPoint = "CreateMyMathFuncsClass")]
            internal static extern MyMathFuncsSafeHandle CreateMyMathFuncs();

            [DllImport(DllName, EntryPoint = "DisposeMyMathFuncsClass")]
            internal static extern void DisposeMyMathFuncs(MyMathFuncsSafeHandle ptr);

            [DllImport(DllName, EntryPoint = "MyMathFuncsAdd")]
            internal static extern double Add(MyMathFuncsSafeHandle ptr, double a, double b);

            [DllImport(DllName, EntryPoint = "MyMathFuncsSubtract")]
            internal static extern double Subtract(MyMathFuncsSafeHandle ptr, double a, double b);

            [DllImport(DllName, EntryPoint = "MyMathFuncsMultiply")]
            internal static extern double Multiply(MyMathFuncsSafeHandle ptr, double a, double b);

            [DllImport(DllName, EntryPoint = "MyMathFuncsDivide")]
            internal static extern double Divide(MyMathFuncsSafeHandle ptr, double a, double b);
        }
    }
    ```

#### <a name="completing-the-mymathfuncssafehandle-class"></a>Завершение класса MyMathFuncsSafeHandle

1. Откройте класс **MyMathFuncsSafeHandle** и перейдите к заполняющему комментарию **TODO** в методе **ReleaseHandle**.

    ```csharp
    // TODO: Release the handle here
    ```

1. Замените строку **TODO**.

    ```csharp
    MyMathFuncsWrapper.DisposeMyMathFuncs(handle);
    ```

#### <a name="writing-the-mymathfuncs-class"></a>Написание класса MyMathFuncs

Теперь, когда оболочка завершена, создайте класс MyMathFuncs, который будет управлять ссылкой на неуправляемый объект C++ MyMathFuncs.  

1. **Нажмите клавишу CONTROL и щелкните** проект **MathFuncs.Shared** , а затем в меню **Добавить** выберите **Добавить файл**.
2. Выберите **Пустой класс** в окне **Новый файл** , назовите его **MyMathFuncs** , а затем щелкните **Создать**.
3. Добавьте следующие члены в класс **MyMathFuncs**.

    ```csharp
    readonly MyMathFuncsSafeHandle handle;
    ```

4. Реализуйте конструктор класса, чтобы он создавал и сохранял дескриптор для собственного объекта **MyMathFuncs** при создании экземпляра класса.

    ```csharp
    public MyMathFuncs()
    {
        handle = MyMathFuncsWrapper.CreateMyMathFuncs();
    }
    ```

5. Реализуйте интерфейс **IDisposable** , используя следующий код.

    ```csharp
    public class MyMathFuncs : IDisposable
    {
        ...

        protected virtual void Dispose(bool disposing)
        {
            if (handle != null && !handle.IsInvalid)
                handle.Dispose();
        }

        public void Dispose()
        {
            Dispose(true);
            GC.SuppressFinalize(this);
        }

        // ...
    }
    ```

6. Реализуйте методы **MyMathFuncs** с помощью класса **MyMathFuncsWrapper** , чтобы выполнять внутренние операции, передавая указатель, сохраненный в базовом неуправляемом объекте. Код должен выглядеть следующим образом.

    ```csharp
    public double Add(double a, double b)
    {
        return MyMathFuncsWrapper.Add(handle, a, b);
    }

    public double Subtract(double a, double b)
    {
        return MyMathFuncsWrapper.Subtract(handle, a, b);
    }

    public double Multiply(double a, double b)
    {
        return MyMathFuncsWrapper.Multiply(handle, a, b);
    }

    public double Divide(double a, double b)
    {
        return MyMathFuncsWrapper.Divide(handle, a, b);
    }
    ```

#### <a name="creating-the-nuspec"></a>Создание NUSPEC-файла

Для упаковки и распространения библиотеки с помощью NuGet решению требуется **NUSPEC** -файл. Он определит, какие из полученных сборок будут включаться для каждой поддерживаемой платформы.

1. **Нажмите клавишу CONTROL и щелкните** решение **MathFuncs** , а затем в меню **Добавить** выберите **Добавить папку решения** и укажите имя **SolutionItems**.
2. **Нажмите клавишу CONTROL и щелкните** папку **SolutionItems** , а затем в меню **Добавить** выберите **Новый файл**.
3. Выберите **Empty XML File** (Пустой XML-файл) в окне **Новый файл** , назовите его **MathFuncs.nuspec** , а затем щелкните **Создать**.
4. Измените **MathFuncs.nuspec** , добавив в него простые метаданные пакета, которые будут отображаться для потребителя **NuGet**. Пример:

    ```xml
    <?xml version="1.0"?>
    <package>
        <metadata>
            <id>MathFuncs</id>
            <version>$version$</version>
            <authors>Microsoft Mobile Customer Advisory Team</authors>
            <description>Sample C++ Wrapper Library</description>
            <requireLicenseAcceptance>false</requireLicenseAcceptance>
            <copyright>Copyright 2018</copyright>
        </metadata>
    </package>
    ```

    > [!NOTE]
    > Дополнительные сведения о схеме, используемой для этого манифеста, см. в документе по [NUSPEC](/nuget/reference/nuspec).

5. Добавьте элемент `<files>` в качестве дочернего элемента `<package>` (под `<metadata>`), обозначая каждый файл с помощью отдельного элемента `<file>`.

    ```xml
    <files>

        <!-- Android -->

        <!-- iOS -->

        <!-- netstandard2.0 -->

    </files>
    ```

    > [!NOTE]
    > При установке пакета в проект и наличии нескольких сборок с одним и тем же именем NuGet эффективно выберет сборку, наиболее подходящую для заданной платформы.

6. Добавьте элементы `<file>` для сборок **Android**.

    ```xml
    <file src="MathFuncs.Android/bin/Release/MathFuncs.dll" target="lib/MonoAndroid81/MathFuncs.dll" />
    <file src="MathFuncs.Android/bin/Release/MathFuncs.pdb" target="lib/MonoAndroid81/MathFuncs.pdb" />
    ```

7. Добавьте элементы `<file>` для сборок **iOS**.

    ```xml
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.dll" target="lib/Xamarin.iOS10/MathFuncs.dll" />
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.pdb" target="lib/Xamarin.iOS10/MathFuncs.pdb" />
    ```

8. Добавьте элементы `<file>` для сборок **netstandard2.0**.

    ```xml
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.dll" target="lib/netstandard2.0/MathFuncs.dll" />
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.pdb" target="lib/netstandard2.0/MathFuncs.pdb" />
    ```

9. Проверьте **NUSPEC** -файл манифеста.

    ```xml
    <?xml version="1.0"?>
    <package>
    <metadata>
        <id>MathFuncs</id>
        <version>$version$</version>
        <authors>Microsoft Mobile Customer Advisory Team</authors>
        <description>Sample C++ Wrapper Library</description>
        <requireLicenseAcceptance>false</requireLicenseAcceptance>
        <copyright>Copyright 2018</copyright>
    </metadata>
    <files>

        <!-- Android -->
        <file src="MathFuncs.Android/bin/Release/MathFuncs.dll" target="lib/MonoAndroid81/MathFuncs.dll" />
        <file src="MathFuncs.Android/bin/Release/MathFuncs.pdb" target="lib/MonoAndroid81/MathFuncs.pdb" />

        <!-- iOS -->
        <file src="MathFuncs.iOS/bin/Release/MathFuncs.dll" target="lib/Xamarin.iOS10/MathFuncs.dll" />
        <file src="MathFuncs.iOS/bin/Release/MathFuncs.pdb" target="lib/Xamarin.iOS10/MathFuncs.pdb" />

        <!-- netstandard2.0 -->
        <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.dll" target="lib/netstandard2.0/MathFuncs.dll" />
        <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.pdb" target="lib/netstandard2.0/MathFuncs.pdb" />

    </files>
    </package>
    ```

    > [!NOTE]
    > Этот файл указывает пути вывода сборки для конфигурации **Выпуск** , поэтому обязательно выполните сборку решения, используя эту конфигурацию.

На этом этапе решение содержит 3 сборки .NET и вспомогательный **NUSPEC** -файл манифеста.

## <a name="distributing-the-net-wrapper-with-nuget"></a>Распространение оболочки .NET с помощью NuGet

Следующим шагом является упаковка и распространение пакета NuGet, чтобы его можно было легко использовать в приложении и управлять им как зависимостью. Упаковку и потребление можно выполнять в рамках одного решения, но распространение библиотеки с помощью NuGet поможет разделить эти базы кода и управлять ими независимо друг от друга.

### <a name="preparing-a-local-packages-directory"></a>Подготовка локального каталога пакетов

Самая простая форма веб-канала NuGet — это локальный каталог.

1. В **Finder** перейдите к удобному каталогу. Например, **/Users**.
2. В меню **Файл** выберите **Создать папку** и укажите понятное имя, например **local-nuget-feed**.

### <a name="creating-the-package"></a>Создание пакета

1. Для параметра **Конфигурация сборки** выберите значение **Выпуск** и выполните сборку, нажав клавиши **COMMAND + B**.
2. Откройте **Терминал** и перейдите в каталог, содержащую **NUSPEC** -файл.
3. В **Терминале** выполните команду **nuget pack** , указав **NUSPEC** -файл, значение **Version** (например, 1.0.0) и **OutputDirectory** , указав папку, созданную на [предыдущем шаге](#preparing-a-local-packages-directory), то есть **local-nuget-feed**. Пример:

    ```bash
    nuget pack MathFuncs.nuspec -Version 1.0.0 -OutputDirectory ~/local-nuget-feed
    ```

4. **Убедитесь** , что в каталоге **local-nuget-feed** создан файл **MathFuncs.1.0.0.nupkg**.

### <a name="optional-using-a-private-nuget-feed-with-azure-devops"></a>[Дополнительно] Использование частного веб-канала NuGet в Azure DevOps

Более надежная методика описана в разделе о [начале работы с пакетами NuGet в Azure DevOps](/azure/devops/artifacts/get-started-nuget?tabs=new-nav&view=vsts#publish-a-package), в котором показано, как создать частный веб-канал и отправить в него пакет, созданный на предыдущем шаге.

Рекомендуется полностью автоматизировать этот рабочий процесс, например с помощью [Azure Pipelines](/azure/devops/pipelines/index?view=vsts). Чтобы узнать больше, ознакомьтесь с разделом [Get started with Azure Pipelines](/azure/devops/pipelines/get-started/index?view=vsts) (Приступая к работе с Azure Pipelines).

## <a name="consuming-the-net-wrapper-from-a-xamarinforms-app"></a>Использование оболочки .NET из приложения Xamarin.Forms

Чтобы выполнить данное пошаговое руководство, создайте приложение **Xamarin.Forms** для использования пакета, только что опубликованного в локальном веб-канале **NuGet**.

### <a name="creating-the-xamarinforms-project"></a>Создание проекта **Xamarin.Forms**

1. Откройте новый экземпляр **Visual Studio для Mac**. Это можно сделать из **Терминала**.

    ```bash
    open -n -a "Visual Studio"
    ```

2. В **Visual Studio для Mac** щелкните **Новый проект** (на *странице приветствия* ) или **Новое решение** (в меню *Файл* ).
3. В окне **Новый проект** выберите **Приложение с пустыми формами** ( *Многоплатформенность > Приложение* ), а затем нажмите кнопку **Далее**.
4. Заполните перечисленные ниже поля и нажмите кнопку **Далее**.

    - **Имя приложения** : MathFuncsApp.
    - **Идентификатор организации** : укажите пространство имен задом наперед, например _com.{ваша_организация}_ .
    - **Целевые платформы** : используйте значение по умолчанию (будут выбраны Android и iOS).
    - **Общий код** : присвойте этому параметру значение ".NET Standard" (можно использовать решение "Общая библиотека", но это выходит за рамки данного пошагового руководства).

5. Заполните перечисленные ниже поля и нажмите кнопку **Создать**.

    - **Имя проекта** : MathFuncsApp.
    - **Имя решения** : MathFuncsApp.  
    - **Расположение**. укажите папку для сохранения по умолчанию (или выберите другую папку).

6. В **обозревателе решений** **нажмите клавишу CONTROL и щелкните** целевой объект ( **MathFuncsApp.Android** или **MathFuncsApp.iOS** ) для начального тестирования, а затем выберите **Назначить в качестве автозагружаемого проекта**.
7. Выберите предпочитаемое **устройство** или щелкните **Симулятор**/**Эмулятор**.
8. Запустите решение (нажмите клавиши **COMMAND + RETURN** ), чтобы убедиться в том, что проект на основе шаблона **Xamarin.Forms** создан и работает нормально.

    > [!NOTE]
    > **iOS** (в частности симулятор), как правило, обеспечивает самую быструю сборку и развертывание.

### <a name="adding-the-local-nuget-feed-to-the-nuget-configuration"></a>Добавление локального веб-канала NuGet в конфигурацию NuGet

1. В **Visual Studio** выберите **Настройка**  (в меню **Visual Studio** ).
2. В разделе **NuGet** выберите **Источники** , а затем щелкните **Добавить**.
3. Заполните перечисленные ниже поля и нажмите кнопку **Добавить источник**.

    - **Name.** укажите понятное имя, например Local-Packages.  
    - **Расположение**. укажите папку **local-nuget-feed** , созданную на [предыдущем шаге](#preparing-a-local-packages-directory).

    > [!NOTE]
    > В этом случае нет необходимости указывать **имя пользователя** и **пароль**.

4. Нажмите кнопку **ОК**.

### <a name="referencing-the-package"></a>Указание ссылок на пакет

Повторите следующие шаги для каждого проекта ( **MathFuncsApp** , **MathFuncsApp.Android** и **MathFuncsApp.iOS** ).

1. **Нажмите клавишу CONTROL и щелкните** проект, затем в меню **Добавить** выберите **Add NuGet Packages** (Добавить пакеты NuGet).
2. Выполните поиск **MathFuncs**.
3. Убедитесь, что **версия** пакета имеет значение **1.0.0** и другие сведения отображаются правильно, например, отображаются **заголовок** и **описание** , то есть *MathFuncs* и *Sample C++ Wrapper Library*.
4. Выберите пакет **MathFuncs** , а затем щелкните **Добавить пакет**.

### <a name="using-the-library-functions"></a>Использование функций библиотеки

Мы добавили ссылки на пакет **MathFuncs** в каждый проект, и теперь его функции можно использовать в коде C#.

1. Откройте **MainPage.xaml.cs** из общего проекта **Xamarin.Forms** **MathFuncsApp** (на который ссылаются как **MathFuncsApp.Android** , так и **MathFuncsApp.iOS** ).
2. Добавьте операторы **using** для **System.Diagnostics** и **MathFuncs** в начало этого файла.

    ```csharp
    using System.Diagnostics;
    using MathFuncs;
    ```

3. Объявите экземпляр класса `MyMathFuncs` в начале класса `MainPage`.

    ```csharp
    MyMathFuncs myMathFuncs;
    ```

4. Переопределите методы `OnAppearing` и `OnDisappearing` базового класса `ContentPage`.

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
    }
    ```

5. Измените метод `OnAppearing`, чтобы инициализировать переменную `myMathFuncs`, объявленную ранее.

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();
        myMathFuncs = new MyMathFuncs();
    }
    ```

6. Теперь измените метод `OnDisappearing`, добавив вызов метода `Dispose` для `myMathFuncs`.

    ```csharp
    protected override void OnDisappearing()
    {
        base.OnAppearing();
        myMathFuncs.Dispose();
    }
    ```

7. Реализуйте частный метод **TestMathFuncs** следующим образом.

    ```csharp
    private void TestMathFuncs()
    {
        var numberA = 1;
        var numberB = 2;

        // Test Add function
        var addResult = myMathFuncs.Add(numberA, numberB);

        // Test Subtract function
        var subtractResult = myMathFuncs.Subtract(numberA, numberB);

        // Test Multiply function
        var multiplyResult = myMathFuncs.Multiply(numberA, numberB);

        // Test Divide function
        var divideResult = myMathFuncs.Divide(numberA, numberB);

        // Output results
        Debug.WriteLine($"{numberA} + {numberB} = {addResult}");
        Debug.WriteLine($"{numberA} - {numberB} = {subtractResult}");
        Debug.WriteLine($"{numberA} * {numberB} = {multiplyResult}");
        Debug.WriteLine($"{numberA} / {numberB} = {divideResult}");
    }
    ```

8. Наконец, добавьте вызов `TestMathFuncs` в конец метода `OnAppearing`.

    ```csharp
    TestMathFuncs();
    ```

9. Запустите приложение на каждой целевой платформе и проверьте выходные данные на панели **выходных данных приложения** , как показано ниже.

    ```csharp
    1 + 2 = 3
    1 - 2 = -1
    1 * 2 = 2
    1 / 2 = 0.5
    ```

    > [!NOTE]
    > В случае обнаружения исключения *DLLNotFoundException* при тестировании в Android или при возникновении ошибки сборки в iOS убедитесь, что архитектура ЦП устройства, эмулятора или симулятора, которую вы используете, совместима с подмножеством архитектур, которые мы выбрали для поддержки.

## <a name="summary"></a>Сводка

В этой статье объясняется, как создать приложение Xamarin.Forms, использующее собственные библиотеки через общую оболочку .NET, распространяемую как пакет NuGet. Пример, приведенный в этом пошаговом руководстве, намеренно очень упрощен в целях демонстрации метода. Реальное приложение потребует реализации сложных механизмов, таких как обработка исключений, обратные вызовы, маршалинг более сложных типов и привязка к другим библиотекам зависимостей. Основной идеей является процесс, с помощью которого развитие C++ кода координируется и синхронизируется с оболочкой и клиентскими приложениями. Этот процесс может отличаться в зависимости от того, отвечает ли за выполнение обеих задач одна группа разработчиков. В любом случае, автоматизация является реальным преимуществом. Ниже приведены ссылки на материалы для дальнейшего чтения, посвященные основным принципам, а также ссылки для скачивания соответствующих компонентов.

### <a name="downloads"></a>Файлы для загрузки

- [Программы командной строки NuGet](/nuget/tools/nuget-exe-cli-reference#macoslinux)
- [Visual Studio](https://visualstudio.microsoft.com/vs)

### <a name="examples"></a>Примеры

- [Использование Hyperlapse для разработки кроссплатформенных мобильных приложений на языке C++](https://blogs.msdn.microsoft.com/vcblog/2015/06/26/hyperlapse-cross-platform-mobile-development-with-visual-c-and-xamarin/)
- [Microsoft PIX (C++ и Xamarin)](https://devblogs.microsoft.com/xamarin/microsoft-research-ships-intelligent-apps-with-the-power-of-c-and-ai/)
- [Перенос примера Mono San Angeles](/samples/xamarin/monodroid-samples/sanangeles-ndk/)

### <a name="further-reading"></a>Дополнительные материалы

[Статьи, связанные с темой этой публикации](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin#wrapping-up)
