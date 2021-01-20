---
title: Установка платформы WPF
description: Xamarin.Forms имеет поддержку предварительной версии для платформы WPF.
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 948dd586a3b60897531cc96187f288687668f60b
ms.sourcegitcommit: 63029dd7ea4edb707a53ea936ddbee684a926204
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2021
ms.locfileid: "98609096"
---
# <a name="wpf-platform-setup"></a>Установка платформы WPF

![Метка предварительного просмотра](~/media/shared/preview.png)

Xamarin.Forms имеет поддержку предварительной версии для Windows Presentation Foundation (WPF) на .NET Framework и в .NET Core 3. В этой статье показано, как добавить проект WPF, предназначенный для .NET Framework, в Xamarin.Forms решение.

> [!IMPORTANT]
> Xamarin.Forms поддержка WPF предоставляется сообществом. Дополнительные сведения см. в разделе [ Xamarin.Forms поддержка платформ](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support).

Прежде чем начать, создайте новое Xamarin.Forms решение в Visual Studio 2019 или используйте существующее Xamarin.Forms решение, например [**боксвиевклокк**](/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock). Приложения WPF можно добавлять в Xamarin.Forms решение только в Windows.

## <a name="add-a-wpf-application"></a>Добавление приложения WPF

Выполните эти инструкции, чтобы добавить приложение WPF, которое будет выполняться на настольных компьютерах под управлением Windows 7, 8 и 10:

1. В Visual Studio 2019 щелкните правой кнопкой мыши имя решения в **Обозреватель решений** и выберите **Добавить > новый проект...**.

2. В окне **Добавление нового проекта** в раскрывающемся списке **языки** выберите пункт **C#** , в раскрывающемся списке **платформы** выберите пункт **Windows** и в раскрывающемся списке **тип проекта** выберите **Рабочий стол** . В списке типов проектов выберите **приложение WPF (.NET Framework)**:

    ![На снимке экрана показано диалоговое окно Добавление нового проекта с выбранным приложением W P F.](wpf-images/add-project.png "Добавить новый проект WPF")

    Нажмите кнопку **Далее** .

    > [!NOTE]
    > Xamarin.Forms 4,7 включает поддержку приложений WPF, работающих в .NET Core 3.

3. В окне **Настройка нового проекта** введите имя проекта с расширением **WPF** , например **боксвиевклокк. WPF**. Нажмите кнопку " **Обзор** ", выберите папку **боксвиевклокк** и нажмите кнопку " **выбрать папку** ", чтобы разместить проект WPF в том же каталоге, что и другие проекты в решении:

    ![На снимке экрана отображается диалоговое окно Настройка нового проекта со значениями имя проекта, расположение и платформа.](wpf-images/configure-project.png "Добавить новый проект WPF")

    Нажмите кнопку **создать** , чтобы создать проект.

4. В **Обозреватель решений** щелкните правой кнопкой мыши новый проект **боксвиевклокк. WPF** и выберите пункт **Управление пакетами NuGet...**. Перейдите на вкладку **Обзор** и выполните поиск по запросу **Xamarin.Forms . Platform. WPF**:

    ![Выберите пакет NuGet](wpf-images/select-nuget-package.png "Выберите пакет NuGet")

    Выберите пакет и нажмите кнопку " **установить** ".

5. Щелкните правой кнопкой мыши имя решения в **Обозреватель решений** и выберите пункт **Управление пакетами NuGet для решения...**. Перейдите на вкладку **обновления** и выберите **Xamarin.Forms** пакет. Выберите все проекты и обновите их до одной и той же Xamarin.Forms версии:

    ![Обновление пакета NuGet](wpf-images/update-nuget-package.png "Обновление пакета NuGet")

6. В проекте WPF щелкните правой кнопкой мыши **ссылки** и выберите команду **Добавить ссылку...**. В диалоговом окне **Диспетчер ссылок** выберите **проекты** слева и установите флажок рядом с проектом **боксвиевклокк** :

    ![Ссылка на общий проект](wpf-images/reference-shared-project.png "Ссылка на общий проект")

    Нажмите кнопку **ОК** .

7. Измените файл **MainWindow. XAML** проекта WPF. В `Window` теге добавьте объявление пространства имен XML для **Xamarin.Forms . Сборка Platform. WPF** и пространство имен:

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    Теперь измените `Window` тег на `wpf:FormsApplicationPage` . Измените `Title` значение параметра на имя приложения, например **боксвиевклокк**. Завершенный файл XAML должен выглядеть следующим образом:

    ```xaml
    <wpf:FormsApplicationPage x:Class="BoxViewClock.WPF.MainWindow"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:BoxViewClock.WPF"
            xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"            
            mc:Ignorable="d"
            Title="BoxViewClock" Height="450" Width="800">
        <Grid>

        </Grid>
    </wpf:FormsApplicationPage>
    ```

8. Измените файл **MainWindow.XAML.CS** проекта WPF. Добавьте две новые `using` директивы:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    Измените базовый класс `MainWindow` с `Window` на `FormsApplicationPage` . После `InitializeComponent` вызова добавьте следующие две инструкции:

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```

    Кроме комментариев и неиспользуемых `using` директив, полный файл **MainWindows.XAML.CS** должен выглядеть следующим образом:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;

    namespace BoxViewClock.WPF
    {
        public partial class MainWindow : FormsApplicationPage
        {
            public MainWindow()
            {
                InitializeComponent();

                Forms.Init();
                LoadApplication(new BoxViewClock.App());
            }
        }
    }
    ```

9. Щелкните правой кнопкой мыши проект WPF в **Обозреватель решений** и выберите **Назначить запускаемым проектом**. Нажмите клавишу F5, чтобы запустить программу с помощью отладчика Visual Studio на рабочем столе Windows:

    ![Часы Боксвиев в WPF](wpf-images/wpf-boxviewclock.png "Часы Боксвиев в WPF" )

## <a name="platform-specifics"></a>Особенности платформы

Вы можете определить, на какой платформе Xamarin.Forms выполняется приложение, с помощью кода или XAML. Это позволяет изменять характеристики программы при выполнении в WPF. В коде Сравните значение `Device.RuntimePlatform` с `Device.WPF` константой (которая равна строке "WPF"). Если найдено совпадение, приложение работает в WPF.

В XAML можно использовать `OnPlatform` тег для выбора значения свойства, характерного для платформы:

```xaml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White" />
        <On Platform="macOS" Value="White" />
        <On Platform="Android" Value="Black" />
        <On Platform="WPF" Value="Blue" />
    </OnPlatform>
</Button.TextColor>
```

## <a name="window-size"></a>Размер окна

Начальный размер окна можно скорректировать в файле WPF **MainWindow. XAML** :

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>Проблемы

Это предварительная версия, поэтому следует рассчитывать на то, что все готово к производству. Не все пакеты NuGet для Xamarin.Forms WPF готовы, а некоторые функции могут быть не полностью работоспособными.

## <a name="related-video"></a>Связанные видео

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Xamarin.Forms видео о поддержке WPF 3,0**