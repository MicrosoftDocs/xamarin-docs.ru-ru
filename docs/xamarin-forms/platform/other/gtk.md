---
title: 'Установка платформы GTK #'
description: 'Xamarin.Formsтеперь имеет поддержку предварительной версии для платформы GTK #'
ms.prod: xamarin
ms.assetid: 3417FB95-3E4B-47DA-85D0-F34832747236
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a5635da9f7c083609ce1e0f120d0613fff9bd77b
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84198097"
---
# <a name="gtk-platform-setup"></a>Установка платформы GTK #

![Preview (Предварительный просмотр)](~/media/shared/preview.png)

Xamarin.Formsтеперь имеет поддержку предварительной версии для приложений GTK #. GTK # — это графический набор средств пользовательского интерфейса, который связывает библиотеку GTK + и разнообразные библиотеки GNOME, что позволяет разрабатывать полностью собственные приложения GNOME Graphics с помощью Mono и .NET. В этой статье показано, как добавить проект GTK # в Xamarin.Forms решение.

> [!IMPORTANT]
> Xamarin.FormsПоддержка GTK # предоставляется сообществом. Дополнительные сведения см. в разделе [ Xamarin.Forms поддержка платформ](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support).

Прежде чем начать, создайте новое Xamarin.Forms решение или используйте существующее Xamarin.Forms решение, например [**гамеофлифе**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife).

> [!NOTE]
> Хотя в этой статье рассматривается добавление приложения GTK # в Xamarin.Forms решение в VS2017 и Visual Studio для Mac, его также можно выполнить в [MonoDevelop](https://www.monodevelop.com/) для Linux.

## <a name="adding-a-gtk-app"></a>Добавление приложения GTK #

GTK # для macOS и Linux устанавливаются как часть [Mono](https://www.mono-project.com/download/stable/). GTK # для .NET можно установить в Windows с помощью [программы установки GTK #](https://www.mono-project.com/download/stable/#download-win).

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Выполните эти инструкции, чтобы добавить приложение GTK #, которое будет выполняться на рабочем столе Windows:

1. В Visual Studio 2019 щелкните правой кнопкой мыши имя решения в **Обозреватель решений** и выберите **Добавить > новый проект...**.

2. В окне **Новый проект** слева выберите **Visual C#** и **классический рабочий стол Windows**. В списке типов проектов выберите **Библиотека классов (.NET Framework)** и убедитесь, что в раскрывающемся списке " **платформа** " установлено минимум .NET Framework 4,7.

3. Введите имя проекта с расширением **GTK** , например **гамеофлифе. GTK**. Нажмите кнопку **Обзор** , выберите папку, содержащую проекты других платформ, и нажмите кнопку **выбрать папку**. Проект GTK будет размещен в том же каталоге, что и другие проекты в решении.

    ![Добавление нового проекта GTK](gtk-images/win/add-new-project.png "Добавление нового проекта GTK")

    Нажмите кнопку **ОК** , чтобы создать проект.

4. В **Обозреватель решений**щелкните правой кнопкой мыши новый проект GTK и выберите пункт **Управление пакетами NuGet**. Перейдите на вкладку **Обзор** и выполните поиск **Xamarin.Forms** 3,0 или более.

    ![Выберите Xamarin.Forms пакет NuGet](gtk-images/win/select-forms-nuget-package.png "Выберите [! Операцион. NO-LOC (Xamarin. Forms)] пакет NuGet")

    Выберите пакет и нажмите кнопку " **установить** ".

5. Теперь найдите ** Xamarin.Forms . Пакет Platform. GTK** 3,0 или более поздней версии.

    ![Выберите Xamarin.Forms . Пакет NuGet Platform. GTK](gtk-images/win/select-forms-platform-nuget-package.png "Выберите [! Операцион. NO-LOC (Xamarin. Forms)]. Пакет NuGet Platform. GTK")

    Выберите пакет и нажмите кнопку " **установить** ".

6. В **Обозреватель решений**щелкните правой кнопкой мыши имя решения и выберите пункт **Управление пакетами NuGet для решения**. Выберите вкладку **Обновление** и **Xamarin.Forms** пакет. Выберите все проекты и обновите их до той же Xamarin.Forms версии, которая используется в проекте GTK.

7. В **Обозреватель решений**щелкните правой кнопкой мыши **ссылки** в проекте GTK. В диалоговом окне **Диспетчер ссылок** выберите **проекты** слева и установите флажок рядом с .NET Standard или общим проектом:

    ![Ссылка на общий проект](gtk-images/win/reference-shared-project.png "Ссылка на общий проект")

8. В диалоговом окне **Диспетчер ссылок** нажмите кнопку **Обзор** и перейдите в папку **C:\Program Files (x86) \GtkSharp\2.12\lib** и выберите **atk-sharp.dll**, **gdk-sharp.dll**, **glade-sharp.dll**, **glib-sharp.dll**, **gtk-dotnet.dll**и **gtk-sharp.dll** файлов.

    ![Ссылки на библиотеки GTK #](gtk-images/win/reference-gtk-libraries.png "Ссылки на библиотеки GTK #")

    Нажмите кнопку **ОК** , чтобы добавить ссылки.

9. В проекте GTK переименуйте **Class1.CS** в **Program.CS**.

10. В проекте GTK измените файл **Program.CS** , чтобы он соответствовал следующему коду:

    ```csharp
    using System;
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.GTK;

    namespace GameOfLife.GTK
    {
        class MainClass
        {
            [STAThread]
            public static void Main(string[] args)
            {
                Gtk.Application.Init();
                Forms.Init();

                var app = new App();
                var window = new FormsWindow();
                window.LoadApplication(app);
                window.SetApplicationTitle("Game of Life");
                window.Show();

                Gtk.Application.Run();
            }
        }
    }
    ```

    Этот код инициализирует GTK # и Xamarin.Forms создает окно приложения и запускает приложение.

11. В **Обозреватель решений**щелкните правой кнопкой мыши проект GTK и выберите пункт **свойства**.

12. В окне **Свойства** перейдите на вкладку **приложение** и измените раскрывающийся список **тип выходных данных** на **приложение Windows**.

    ![Изменение типа выходных данных проекта](gtk-images/win/change-project-output-type.png "Изменение типа выходных данных проекта")

13. В **Обозреватель решений**щелкните правой кнопкой мыши проект GTK и выберите **Назначить запускаемым проектом**. Нажмите клавишу F5, чтобы запустить программу с помощью отладчика Visual Studio на рабочем столе Windows:

    ![Игра GTK #](gtk-images/win/gtk-gameoflife.png "Игра GTK #")

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Выполните эти инструкции, чтобы добавить приложение GTK #, которое будет выполняться на рабочем столе Mac:

1. В Visual Studio для Mac щелкните решение правой кнопкой мыши Xamarin.Forms и выберите **Добавить > добавить новый проект...**.

2. В окне **Новый проект** выберите **другие > проект .NET > GTK # 2,0** и нажмите кнопку **Далее**.

3. Введите имя проекта с расширением **GTK** , например **гамеофлифе. GTK**, и нажмите кнопку **создать**.

4. В **панель решения**щелкните правой кнопкой мыши **пакеты > добавить пакеты...** для проекта GTK и добавьте Xamarin.Forms пакет NuGet предварительной версии 3,0 или выше.

    ![Выберите Xamarin.Forms пакет NuGet](gtk-images/mac/select-forms-nuget-package.png "Выберите [! Операцион. NO-LOC (Xamarin. Forms)] пакет NuGet")

5. В **панель решения**щелкните правой кнопкой мыши **пакеты > добавить пакеты...** для проекта GTK и добавьте Xamarin.Forms . Platform. GTK 3,0 предварительный выпуск пакета NuGet или более поздней версии.

    ![Выберите Xamarin.Forms . Пакет NuGet Platform. GTK](gtk-images/mac/select-forms-platform-nuget-package.png "Выберите [! Операцион. NO-LOC (Xamarin. Forms)]. Пакет NuGet Platform. GTK")

6. Обновите другие проекты платформы для использования той же Xamarin.Forms версии, которая используется в проекте GTK.

7. В **панель решения**щелкните правой кнопкой мыши **ссылки > изменить ссылки...** для проекта GTK и добавьте ссылку на Xamarin.Forms проект (либо в .NET Standard, либо в общий проект).

    ![Ссылка на общий проект](gtk-images/mac/reference-shared-project.png "Ссылка на общий проект")

8. Измените файл **Program.CS** проекта GTK, чтобы он соответствовал следующему коду:

    ```csharp
    using System;
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.GTK;

    namespace GameOfLife.GTK
    {
        class MainClass
        {
            [STAThread]
            public static void Main(string[] args)
            {
                Gtk.Application.Init();
                Forms.Init();

                var app = new App();
                var window = new FormsWindow();
                window.LoadApplication(app);
                window.SetApplicationTitle("Game of Life");
                window.Show();

                Gtk.Application.Run();
            }
        }
    }
    ```

    Этот код инициализирует GTK # и Xamarin.Forms создает окно приложения и запускает приложение.

9. В **панель решения**щелкните правой кнопкой мыши проект GTK и выберите **Назначить запускаемым проектом**.

10. На панели инструментов Visual Studio для Mac нажмите кнопку **Пуск** (треугольная кнопка, напоминающая кнопку воспроизведения), чтобы запустить приложение.

    ![Игра GTK #](gtk-images/mac/gtk-gameoflife.png "Игра GTK #")

-----

## <a name="next-steps"></a>Next Steps

### <a name="platform-specifics"></a>Особенности платформы

Вы можете определить, на какой платформе Xamarin.Forms выполняется приложение, с помощью XAML или кода. Это позволяет изменять характеристики программы при выполнении в GTK #. В коде Сравните значение `Device.RuntimePlatform` с `Device.GTK` константой (которая равна строке "GTK"). При наличии совпадения приложение будет выполняться в GTK #.

В XAML можно использовать `OnPlatform` тег для выбора значения свойства, характерного для платформы:

```xaml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White" />
        <On Platform="macOS" Value="White" />
        <On Platform="Android" Value="Black" />
        <On Platform="GTK" Value="Blue" />
    </OnPlatform>
</Button.TextColor>
```

### <a name="application-icon"></a>Значок приложения

Значок приложения можно задать при запуске:

```csharp
window.SetApplicationIcon("icon.png");
```

### <a name="themes"></a>Темы

Для GTK # доступны разнообразные темы, которые можно использовать из Xamarin.Forms приложения:

```csharp
GtkThemes.Init ();
GtkThemes.LoadCustomTheme ("Themes/gtkrc");
```

### <a name="native-forms"></a>Собственные формы

Собственные формы позволяют Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) использовать производные страницы в собственных проектах, включая проекты GTK #. Это можно сделать путем создания экземпляра страницы, производной от класса, [`ContentPage`](xref:Xamarin.Forms.ContentPage) и преобразования ее в собственный тип GTK # с помощью `CreateContainer` метода расширения:

```csharp
var settingsView = new SettingsView().CreateContainer();
vbox.PackEnd(settingsView, true, true, 0);
```

Дополнительные сведения о собственных формах см. в разделе [собственные формы](~/xamarin-forms/platform/native-forms.md).

## <a name="issues"></a>Проблемы

Это предварительная версия, поэтому следует рассчитывать на то, что все готово к производству. Сведения о текущем состоянии реализации см. в разделе [состояние](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Status.md), а сведения о текущих известных проблемах см. [в разделе ожидающие & известные проблемы](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Issues-Pending.md).
