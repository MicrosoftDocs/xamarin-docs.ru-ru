---
ms.openlocfilehash: d0b00cf71a6121026dfa368592da8ce8d3be0373
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98635010"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

Для работы с этим руководством у вас должен быть последний выпуск Visual Studio 2019 с установленной рабочей нагрузкой **Разработка мобильных приложений на .NET**. Кроме того, вам потребуется компьютер Mac для сборки учебного приложения на iOS. Сведения об установке платформы Xamarin см. в статье [Установка Xamarin](~/get-started/installation/index.md). Сведения о подключении Visual Studio 2019 к узлу сборки Mac см. в статье [Связывание с Mac при разработке для Xamarin.iOS](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Запустите Visual Studio и создайте пустое приложение Xamarin.Forms **AppLifecycleTutorial**.

    > [!IMPORTANT]
    > Для фрагментов кода на C# и XAML в этом руководстве необходимо решение с именем **AppLifecycleTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. В **обозревателе решений** в проекте **AppLifecycleTutorial** разверните **App.xaml** и дважды щелкните файл **App.xaml.cs**, чтобы открыть его. Затем, в файле **App.xaml.cs** обновите методы `OnStart`, `OnSleep` и `OnResume`, переопределив их следующим образом:

    ```csharp
    protected override void OnStart()
    {
        Console.WriteLine("OnStart");
    }

    protected override void OnSleep()
    {
        Console.WriteLine("OnSleep");
    }

    protected override void OnResume()
    {
        Console.WriteLine("OnResume");
    }
    ```

    Этот код обновит переопределения метода жизненного цикла приложения с помощью инструкций `Console.WriteLine`, которые указывают, когда был вызван каждый из методов.

    - Метод `OnStart` вызывается при запуске приложения.
    - Метод `OnSleep` вызывается когда приложение переводится в фоновый режим.
    - Метод `OnResume` вызывается при выходе из фонового режима.

    > [!NOTE]
    > Метода, отвечающего за завершение работы приложения, не существует. В обычных условиях завершение работы приложения происходит из-за метода `OnSleep`.

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном удаленном симуляторе iOS или эмуляторе Android. При запуске приложения вызывается метод `OnStart`, а затем **OnStart** выводится в Visual Studio в окно **Вывод**.

    ```
    [Mono] Found as 'java_interop_jnienv_get_object_array_element'.
    OnStart
    [OpenGLRenderer] HWUI GL Pipeline
    ```

    Когда приложение переходит в фоновый режим (после нажатия кнопки "Домой" в iOS или Android), вызывается метод `OnSleep`.

    ```
    [EGL_emulation] eglMakeCurrent: 0x83ee2920: ver 3 0 (tinfo 0x8357eff0)
    OnSleep
    [Mono] Image addref System.Runtime.Serialization[0x83ee19c0] -> System.Runtime.Serialization.dll[0x83f57b00]: 2
    ```

    Затем, когда восстанавливается работа приложения (после нажатия значка приложения в iOS, нажатия кнопки "Обзор" в Android, и выбора приложения AppLifecycleTutorial), вызывается метод `OnResume`.

    ```
    Thread finished: <Thread Pool> #5
    OnResume
    [EGL_emulation] eglMakeCurrent: 0x83ee2920: ver 3 0 (tinfo 0x8357eff0)
    ```

    > [!NOTE]
    > В этих блоках с кодом приведен пример результата выполнения приложения на Android.

    В Visual Studio остановите приложение.

    Дополнительные сведения о жизненном цикле приложения Xamarin.Forms см. в статье [Жизненный цикл приложения Xamarin.Forms](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

Для работы с этим руководством вам нужно установить Visual Studio для Mac (последний выпуск) с поддержкой платформ Android и iOS. Кроме того, вам потребуется Xcode (последний выпуск). Дополнительные сведения об установке платформы Xamarin см. в статье [Установка Xamarin](~/get-started/installation/index.md).

1. Запустите Visual Studio для Mac и создайте пустое приложение Xamarin.Forms **AppLifecycleTutorial**.

    > [!IMPORTANT]
    > Для фрагментов кода на C# и XAML в этом руководстве необходимо решение с именем **AppLifecycleTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. На **Панели решения** в проекте **AppLifecycleTutorial** разверните **App.xaml** и дважды щелкните файл **App.xaml.cs**, чтобы открыть его. Затем, в файле **App.xaml.cs** обновите методы `OnStart`, `OnSleep` и `OnResume`, переопределив их следующим образом:

    ```csharp
    protected override void OnStart()
    {
        Console.WriteLine("OnStart");
    }

    protected override void OnSleep()
    {
        Console.WriteLine("OnSleep");
    }

    protected override void OnResume()
    {
        Console.WriteLine("OnResume");
    }
    ```

    Этот код обновит переопределения метода жизненного цикла приложения с помощью инструкций `Console.WriteLine`, которые указывают, когда был вызван каждый из методов.

    - Метод `OnStart` вызывается при запуске приложения.
    - Метод `OnSleep` вызывается когда приложение переводится в фоновый режим.
    - Метод `OnResume` вызывается при выходе из фонового режима.

    > [!NOTE]
    > Метода, отвечающего за завершение работы приложения, не существует. В обычных условиях завершение работы приложения происходит из-за метода `OnSleep`.

1. На панели инструментов Visual Studio для Mac нажмите клавишу **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android. При запуске приложения вызывается метод `OnStart`, а затем **OnStart** выводится в Visual Studio для Mac в окно **Вывод приложения**.

    ```
    2019-02-11 12:05:23.164761+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskLoadEntitlements failed error=22 cs_flags=200, pid=4089
    2019-02-11 12:05:23.165297+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskCopyDebugDescription: AppLifecycleTuto[4089]/0#-1 LF=0
    2019-02-11 12:05:23.685430+0000 AppLifecycleTutorial.iOS[4089:361037] OnStart
    ```

    Когда приложение переходит в фоновый режим (после нажатия кнопки "Домой" в iOS или Android), вызывается метод `OnSleep`.

    ```
    2019-02-11 12:06:12.394301+0000 AppLifecycleTutorial.iOS[4089:361037] OnSleep
    Thread started: <Thread Pool> #3
    Thread started: <Thread Pool> #4
    Thread started: <Thread Pool> #5
    Thread started: <Thread Pool> #6
    2019-02-11 12:06:13.056968+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskLoadEntitlements failed error=22 cs_flags=200, pid=4089
    2019-02-11 12:06:13.057264+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskCopyDebugDescription: AppLifecycleTuto[4089]/0#-1 LF=0
    ```

    Затем, когда восстанавливается работа приложения (после нажатия значка приложения в iOS, нажатия кнопки "Обзор" в Android, и выбора приложения AppLifecycleTutorial), вызывается метод `OnResume`.

    ```
    2019-02-11 12:07:10.321905+0000 AppLifecycleTutorial.iOS[4130:365695] SecTaskLoadEntitlements failed error=22 cs_flags=200, pid=4130
    2019-02-11 12:07:10.322557+0000 AppLifecycleTutorial.iOS[4130:365695] SecTaskCopyDebugDescription: AppLifecycleTuto[4130]/0#-1 LF=0
    2019-02-11 12:07:17.110938+0000 AppLifecycleTutorial.iOS[4130:365695] OnResume
    ```

    > [!NOTE]
    > В этих блоках с кодом приведен пример результата выполнения приложения на iOS.

    В Visual Studio для Mac остановите приложение.

    Дополнительные сведения о жизненном цикле приложения Xamarin.Forms см. в статье [Жизненный цикл приложения Xamarin.Forms](~/xamarin-forms/app-fundamentals/app-lifecycle.md).
