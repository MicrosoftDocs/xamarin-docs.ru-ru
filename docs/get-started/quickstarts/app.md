---
title: Краткое руководство по созданию приложения Xamarin.Forms
description: В этой статье объясняется, как создать приложение Оболочки в Xamarin.Forms, которое упрощает разработку мобильных приложений, предоставляя основные возможности, которые необходимы для большинства мобильных приложений.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 359F5012-4409-42A7-BEDF-C1DB9AF86DED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.custom: contperf-fy21q3
ms.date: 01/26/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4d21daa89322021e80a46753c864da3cb671b2d6
ms.sourcegitcommit: 1f391667869a4541dd9b42d78862dc01d69ed160
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99818726"
---
# <a name="create-a-xamarinforms-application-quickstart"></a>Краткое руководство по созданию приложения Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/getstarted-notes-app/)

В этом кратком руководстве рассматриваются следующие темы:

- Создание приложения Оболочки в Xamarin.Forms
- Определение пользовательского интерфейса страницы с помощью языка XAML и взаимодействие с элементами XAML из кода.
- Описание визуальной иерархии приложения Оболочки путем создания подкласса класса `Shell`.

В этом кратком руководстве приводятся инструкции по созданию кроссплатформенного приложения Оболочки в Xamarin.Forms, которое позволяет ввести заметку и сохранить ее в хранилище устройства. Ниже показано итоговое приложение:

[![Приложение Notes](app-images/screenshots1-sml.png)](app-images/screenshots1.png#lightbox)
[![Страница About приложения Notes](app-images/screenshots2-sml.png)](app-images/screenshots2.png#lightbox)

::: zone pivot="windows"

### <a name="prerequisites"></a>Предварительные требования

- Последний выпуск Visual Studio 2019 с установленной рабочей нагрузкой **Разработка мобильных приложений на .NET**.
- Знание языка C#.
- (Необязательно) Парный компьютер Mac для построения приложения в iOS.

Дополнительные сведения об этих предварительных требованиях см. в разделе [Установка Xamarin](~/get-started/installation/index.md). Сведения о подключении Visual Studio 2019 к узлу сборки Mac см. в статье [Связывание с Mac при разработке для Xamarin.iOS](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

## <a name="get-started-with-visual-studio-2019"></a>Начало работы с Visual Studio 2019

1. Запустите Visual Studio 2019 и в начальном окне щелкните **Создать проект**, чтобы создать новый проект:

    ![Новое решение](app-images/vs/new-solution.png)

2. В окне **Создать проект** в раскрывающемся списке **Тип проекта** щелкните **Мобильное приложение**, а затем выберите шаблон **Мобильное приложение (Xamarin.Forms)** и нажмите кнопку **Далее**:

    ![Выбор шаблона](app-images/vs/new-project.png)

3. В диалоговом окне **Настроить новый проект** в поле **Имя проекта** укажите **Notes**, выберите подходящее расположение для проекта и нажмите кнопку **Создать**:   

    ![Настройка приложения Оболочки](app-images/vs/configure-project.png)

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого краткого руководства предполагают, что решение и проект называются **Notes**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого краткого руководства в проект.

4. В диалоговом окне **Новое мобильное приложение** выберите шаблон **С вкладками** и нажмите кнопку **Создать**:

    ![Создание приложения Оболочки](app-images/vs/create-project.png)

    После создания проекта закройте файл **GettingStarted.txt**.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Оболочки в Xamarin.Forms](deepdive.md#anatomy-of-a-xamarinforms-application) статьи [Подробное изучение кратких руководств по Оболочке в Xamarin.Forms](deepdive.md).

5. В **обозревателе решений** в проекте **Notes** удалите следующие папки (и их содержимое):

    - **Models**
    - **Службы**
    - **ViewModels**
    - **Представления**

6. В **обозревателе решений** в проекте **Notes** удалите **GettingStarted.txt**.

7. В **обозревателе решений** в проект **Notes** добавьте новую папку с именем **Views**.

8. В **обозревателе решений** в проекте **Notes** выберите папку **Views**, щелкните правой кнопкой мыши и выберите **Добавить > Новый элемент...** : В диалоговом окне **Добавление нового элемента** выберите **Элементы Visual C# > Xamarin.Forms > Страница содержимого**, присвойте новому файлу имя **NotesPage** и нажмите кнопку **Добавить**.

    ![Добавление NotesPage](app-images/vs/add-notespage.png)

    В результате этого в папку **Views** будет добавлена новая страница с именем **NotesPage**. Эта страница будет основной страницей в приложении.

9. В **обозревателе решений** дважды щелкните файл **NotesPage.xaml** в проекте **Notes**, чтобы открыть его:

    ![Открытие NotesPage.xaml](app-images/vs/open-notespage-xaml.png)

10. Удалите из **NotesPage.xaml** весь шаблонный код и замените его приведенным ниже.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NotesPage"
                 Title="Notes">
        <!-- Layout children vertically -->
        <StackLayout Margin="20">
            <Editor x:Name="editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <!-- Layout children in two columns -->
            <Grid ColumnDefinitions="*,*">
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Editor`](xref:Xamarin.Forms.Editor) для ввода текста, а также двух объектов [`Button`](xref:Xamarin.Forms.Button), которые предписывают приложению сохранить или удалить файл. Два объекта `Button` располагаются по горизонтали в [`Grid`](xref:Xamarin.Forms.Grid), а `Editor` и `Grid` — по вертикали в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Дополнительные сведения о создании пользовательского интерфейса см. в разделе [Пользовательский интерфейс](deepdive.md#user-interface) в статье [Подробное изучение кратких руководств по Оболочке в Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **NotesPage.xaml**, нажав клавиши **CTRL+S**.  

12. В **обозревателе решений** дважды щелкните файл **NotesPage.xaml.cs** в проекте **Notes**, чтобы открыть его:

    ![Открытие NotesPage.xaml.cs](app-images/vs/open-notespage-codebehind.png)

12. Удалите из **NotesPage.xaml.cs** весь шаблонный код и замените его приведенным ниже.

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes.Views
    {
        public partial class NotesPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public NotesPage()
            {
                InitializeComponent();

                // Read the file.
                if (File.Exists(_fileName))
                {
                    editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                // Save the file.
                File.WriteAllText(_fileName, editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                // Delete the file.
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                editor.Text = string.Empty;
            }
        }
    }
    ```

    Этот код определяет поле `_fileName`, которое ссылается на файл с именем `notes.txt`, где будут храниться данные с заметками в локальной папке данных для приложения. При выполнении конструктора страниц файл считывается, если он существует, и отображается в [`Editor`](xref:Xamarin.Forms.Editor). При нажатии кнопки **Сохранить** [`Button`](xref:Xamarin.Forms.Button) выполняется обработчик событий `OnSaveButtonClicked`, который сохраняет содержимое `Editor` в файле. При нажатии кнопки **Удалить** `Button` выполняется обработчик событий `OnDeleteButtonClicked`, который удаляет файл при условии, что он существует, и весь текст из `Editor`. Дополнительные сведения о взаимодействии с пользователем см. в разделе [Реакция на действия пользователей](deepdive.md#responding-to-user-interaction) в статье [Подробное изучение кратких руководств по Оболочке в Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **NotesPage.xaml.cs**, нажав клавиши **CTRL+S**.    

13. В **обозревателе решений** в проекте **Notes** выберите папку **Views**, щелкните правой кнопкой мыши и выберите **Добавить > Новый элемент...** : В диалоговом окне **Добавление нового элемента** выберите **Элементы Visual C# > Xamarin.Forms > Страница содержимого**, присвойте новому файлу имя **AboutPage** и нажмите кнопку **Добавить**.

    ![Добавление AboutPage](app-images/vs/add-aboutpage.png)

    В результате этого в папку **Views** будет добавлена новая страница с именем **AboutPage**.

14. В **обозревателе решений** дважды щелкните файл **AboutPage.xaml** в проекте **Notes**, чтобы открыть его:

    ![Открытие файла AboutPage.xaml](app-images/vs/open-aboutpage-xaml.png)

15. Удалите из **AboutPage.xaml** весь шаблонный код и замените его приведенным ниже.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.AboutPage"
                 Title="About">
        <!-- Layout children in two rows -->
        <Grid RowDefinitions="Auto,*">
            <Image Source="xamarin_logo.png"
                   BackgroundColor="{OnPlatform iOS=LightSlateGray, Android=#2196F3}"
                   VerticalOptions="Center"
                   HeightRequest="64" />
            <!-- Layout children vertically -->
            <StackLayout Grid.Row="1"
                         Margin="20"
                         Spacing="20">
                <Label FontSize="22">
                    <Label.FormattedText>
                        <FormattedString>
                            <FormattedString.Spans>
                                <Span Text="Notes"
                                      FontAttributes="Bold"
                                      FontSize="22" />
                                <Span Text=" v1.0" />
                            </FormattedString.Spans>
                        </FormattedString>
                    </Label.FormattedText>
                </Label>
                <Label Text="This app is written in XAML and C# with the Xamarin Platform." />
                <Button Text="Learn more"
                        Clicked="OnButtonClicked" />
            </StackLayout>
        </Grid>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, которая состоит из [`Image`](xref:Xamarin.Forms.Image), двух объектов [`Label`](xref:Xamarin.Forms.Label), которые отображают текст, и [`Button`](xref:Xamarin.Forms.Button). Два объекта `Label` и `Button` располагаются по горизонтали в [`StackLayout`](xref:Xamarin.Forms.StackLayout), а `Image` и `StackLayout` — по вертикали в [`Grid`](xref:Xamarin.Forms.Grid). Дополнительные сведения о создании пользовательского интерфейса см. в разделе [Пользовательский интерфейс](deepdive.md#user-interface) в статье [Подробное изучение кратких руководств по Оболочке в Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **AboutPage.xaml**, нажав клавиши **CTRL+S**.    

16. В **обозревателе решений** дважды щелкните файл **AboutPage.xaml.cs** в проекте **Notes**, чтобы открыть его:

    ![Открытие файла AboutPage.xaml.cs](app-images/vs/open-aboutpage-codebehind.png)

17. Удалите из **AboutPage.xaml.cs** весь шаблонный код и замените его приведенным ниже.

    ```csharp
    using System;
    using Xamarin.Essentials;
    using Xamarin.Forms;

    namespace Notes.Views
    {
        public partial class AboutPage : ContentPage
        {
            public AboutPage()
            {
                InitializeComponent();
            }

            async void OnButtonClicked(object sender, EventArgs e)
            {
                // Launch the specified URL in the system browser.
                await Launcher.OpenAsync("https://aka.ms/xamarin-quickstart");
            }
        }
    }
    ```

    Этот код определяет обработчик событий `OnButtonClicked`, который выполняется при нажатии кнопки **Подробнее** [`Button`](xref:Xamarin.Forms.Button). При нажатии кнопки запускается веб-браузер и отображается страница, представленная аргументом URI для метода `OpenAsync`. Дополнительные сведения о взаимодействии с пользователем см. в разделе [Реакция на действия пользователей](deepdive.md#responding-to-user-interaction) в статье [Подробное изучение кратких руководств по Оболочке в Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **AboutPage.xaml.cs**, нажав клавиши **CTRL+S**.

18. В **обозревателе решений** дважды щелкните файл **AppShell.xaml** в проекте **Notes**, чтобы открыть его:

    ![Открытие файла AppShell.xaml](app-images/vs/open-appshell-xaml.png)

19. Удалите из **AppShell.xaml** весь шаблонный код и замените его приведенным ниже.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <Shell xmlns="http://xamarin.com/schemas/2014/forms"
           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
           xmlns:views="clr-namespace:Notes.Views"
           x:Class="Notes.AppShell">
        <!-- Display a bottom tab bar containing two tabs -->   
        <TabBar>
            <ShellContent Title="Notes"
                          Icon="icon_feed.png"
                          ContentTemplate="{DataTemplate views:NotesPage}" />
            <ShellContent Title="About"
                          Icon="icon_about.png"
                          ContentTemplate="{DataTemplate views:AboutPage}" />
        </TabBar>
    </Shell>
    ```    

    Этот код декларативно определяет визуальную иерархию приложения, которая состоит из `TabBar`, где содержатся два объекта `ShellContent`. Эти объекты не представляют собой какие-либо элементы пользовательского интерфейса, они служат лишь для организации визуальной структуры приложения. На основе этих объектов Оболочка создает пользовательский интерфейс для содержимого. Дополнительные сведения о создании пользовательского интерфейса см. в разделе [Пользовательский интерфейс](deepdive.md#user-interface) в статье [Подробное изучение кратких руководств по Оболочке в Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **AppShell.xaml**, нажав клавиши **CTRL+S**.    

20. В **обозревателе решений** в проекте **Notes** разверните **AppShell.xaml** и дважды щелкните файл **AppShell.xaml.cs**, чтобы открыть его:

    ![Открытие файла AppShell.xaml.cs](app-images/vs/open-appshell-codebehind.png)

21. Удалите из **AppShell.xaml.cs** весь шаблонный код и замените его приведенным ниже.

    ```csharp
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class AppShell : Shell
        {
            public AppShell()
            {
                InitializeComponent();
            }
        }
    }
    ```

    Сохраните изменения в файле **AppShell.xaml.cs**, нажав клавиши **CTRL+S**.

22. В **обозревателе решений** дважды щелкните файл **App.xaml** в проекте **Notes**, чтобы открыть его:

    ![Открытие файла App.xaml](app-images/vs/open-app-xaml.png)

23. Удалите из **App.xaml** весь шаблонный код и замените его приведенным ниже.

    ```xaml
    <?xml version="1.0" encoding="utf-8" ?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">

    </Application>
    ```

    Этот код декларативно определяет класс `App`, который отвечает за создание экземпляра приложения.

    Сохраните изменения в файле **App.xaml**, нажав клавиши **CTRL+S**.    

24. В **обозревателе решений** в проекте **Notes** разверните **App.xaml** и дважды щелкните файл **App.xaml.cs**, чтобы открыть его:

    ![Открытие App.xaml.cs](app-images/vs/open-app-codebehind.png)

25. Удалите из **App.xaml.cs** весь шаблонный код и замените его приведенным ниже.

    ```csharp
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class App : Application
        {

            public App()
            {
                InitializeComponent();
                MainPage = new AppShell();
            }

            protected override void OnStart()
            {
            }

            protected override void OnSleep()
            {
            }

            protected override void OnResume()
            {
            }
        }
    }
    ```

    Этот код определяет код программной части для класса `App`, который отвечает за создание экземпляра приложения. Он инициализирует свойство [`MainPage`](xref:Xamarin.Forms.Application.MainPage) для производного класса `Shell`.

    Сохраните изменения в файле **App.xaml.cs**, нажав клавиши **CTRL+S**.    

### <a name="building-the-quickstart"></a>Сборка примера из краткого руководства

1. В Visual Studio выберите элемент меню **Сборка > Построить решение** (или нажмите клавишу F6). Выполняется сборка решения, а в строке состояния Visual Studio отображается сообщение об успешном выполнении:

      ![Сборка успешно выполнена](app-images/vs/build-successful.png)

    При наличии ошибок повторите предыдущие шаги и исправьте все ошибки, пока сборка проектов не будет проходить успешно.

2. На панели инструментов Visual Studio нажмите клавишу **Запустить** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном эмуляторе Android.

      ![Панель инструментов Visual Studio для Android](app-images/vs/android-start.png)

      ![Приложение Notes в эмуляторе Android](app-images/vs/notes1-android.png)

    Введите примечание и нажмите кнопку **Сохранить**. Закройте приложение и повторно запустите его, чтобы убедиться, что введенные заметки перезагружены.

    Нажмите значок **About** для перехода к `AboutPage`:

      ![Страница About приложения Notes в Android Emulator](app-images/vs/notes2-android.png)

    Нажмите кнопку **Подробнее**, чтобы запустить веб-страницу кратких руководств.

    Дополнительные сведения о запуске приложения на каждой платформе см. в разделе [Запуск приложения на каждой платформе](deepdive.md#launch-the-application-on-each-platform) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    > [!NOTE]
    > Следующие шаги следует выполнить, только если у вас есть [связанный компьютер Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md), отвечающий требованиям к системе для разработки приложений Xamarin.Forms.    

3. На панели инструментов Visual Studio щелкните правой кнопкой мыши проект **Notes.iOS**, а затем выберите команду **Назначить запускаемым проектом**.

      ![Назначение Notes.iOS запускаемым проектом](app-images/vs/set-startup-project-ios.png)

4. На панели инструментов Visual Studio нажмите клавишу **Запустить** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном [удаленном эмуляторе для iOS](~/tools/ios-simulator/index.md).

      ![Панель инструментов Visual Studio для iOS](app-images/vs/ios-start.png)

      [![Приложение Notes в симуляторе iOS](app-images/vs/notes1-ios.png)](app-images/vs/notes1-ios-large.png#lightbox)

    Введите примечание и нажмите кнопку **Сохранить**. Закройте приложение и повторно запустите его, чтобы убедиться, что введенные заметки перезагружены.

    Нажмите значок **About** для перехода к `AboutPage`:

      [![Страница About приложения Notes в симуляторе iOS](app-images/vs/notes2-ios.png)](app-images/vs/notes2-ios-large.png#lightbox)

    Нажмите кнопку **Подробнее**, чтобы запустить веб-страницу кратких руководств.

    Дополнительные сведения о запуске приложения на каждой платформе см. в разделе [Запуск приложения на каждой платформе](deepdive.md#launch-the-application-on-each-platform) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

::: zone-end
::: zone pivot="macos"

### <a name="prerequisites"></a>Предварительные требования

- Visual Studio для Mac (последний выпуск) с поддержкой платформ Android и iOS.
- Xcode (последний выпуск).
- Знание языка C#.

Дополнительные сведения об этих предварительных требованиях см. в разделе [Установка Xamarin](~/get-started/installation/index.md).

## <a name="get-started-with-visual-studio-for-mac"></a>Начало работы с Visual Studio для Mac

1. Запустите Visual Studio для Mac и в начальном окне щелкните **Создать**, чтобы создать новый проект:

    ![Новое решение](app-images/vsmac/new-project.png)

2. В диалоговом окне **Выберите шаблон из нового проекта** щелкните **Многоплатформенность > Приложение** и выберите шаблон **Приложение Shell Forms**, а затем нажмите кнопку **Далее**:

    ![Выбор шаблона](app-images/vsmac/choose-template.png)

3. В диалоговом окне **Configure your Shell Forms app** (Настройка приложения Shell Forms) присвойте новому приложению имя **Notes**, а затем нажмите кнопку **Далее**:    

    ![Настройка приложения Оболочки](app-images/vsmac/configure-app.png)

4. В диалоговом окне **Configure your new Shell Forms app** (Настройка нового приложения Shell Forms) сохраните для проекта и решения имя **Notes**, выберите подходящее расположение для проекта и нажмите кнопку **Создать** для создания проекта:

    ![Настройка проекта Оболочки](app-images/vsmac/configure-project.png)

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого краткого руководства предполагают, что решение и проект называются **Notes**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого краткого руководства в проект.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Оболочки в Xamarin.Forms](deepdive.md#anatomy-of-a-xamarinforms-application) статьи [Подробное изучение кратких руководств по Оболочке в Xamarin.Forms](deepdive.md).

5. На **Панели решения** в проекте **Notes** удалите следующие папки (и их содержимое):

    - **Models**
    - **Службы**
    - **ViewModels**
    - **Представления**

6. На **Панели решения** в проекте **Notes** удалите **GettingStarted.txt**.

7. На **Панели решения** в проект **Notes** добавьте новую папку с именем **Views**.

8. На **Панели решения** в проекте **Notes** выберите папку **Views**, щелкните правой кнопкой мыши и выберите **Добавить > Новый файл...** : В диалоговом окне **Новый файл** выберите **Forms > Forms ContentPage XAML**, назовите новый файл **NotesPage** и нажмите кнопку **Создать**.

    ![Добавление NotesPage](app-images/vsmac/add-notespage.png)

    В результате этого в папку **Views** будет добавлена новая страница с именем **NotesPage**. Эта страница будет основной страницей в приложении.

9. На **Панели решения** дважды щелкните файл **NotesPage.xaml** в проекте **Notes**, чтобы открыть его:

    ![Открытие NotesPage.xaml](app-images/vsmac/open-notespage-xaml.png)

10. Удалите из **NotesPage.xaml** весь шаблонный код и замените его приведенным ниже.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NotesPage"
                 Title="Notes">
        <!-- Layout children vertically -->
        <StackLayout Margin="20">
            <Editor x:Name="editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <!-- Layout children in two columns -->
            <Grid ColumnDefinitions="*,*">
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Editor`](xref:Xamarin.Forms.Editor) для ввода текста, а также двух объектов [`Button`](xref:Xamarin.Forms.Button), которые предписывают приложению сохранить или удалить файл. Два объекта `Button` располагаются по горизонтали в [`Grid`](xref:Xamarin.Forms.Grid), а `Editor` и `Grid` — по вертикали в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Дополнительные сведения о создании пользовательского интерфейса см. в разделе [Пользовательский интерфейс](deepdive.md#user-interface) в статье [Подробное изучение кратких руководств по Оболочке в Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **NotesPage.xaml**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

12. На **Панели решения** дважды щелкните файл **NotesPage.xaml.cs** в проекте **Notes**, чтобы открыть его:

    ![Открытие NotesPage.xaml.cs](app-images/vsmac/open-notespage-codebehind.png)

12. Удалите из **NotesPage.xaml.cs** весь шаблонный код и замените его приведенным ниже.

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes.Views
    {
        public partial class NotesPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public NotesPage()
            {
                InitializeComponent();

                // Read the file.
                if (File.Exists(_fileName))
                {
                    editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                // Save the file.
                File.WriteAllText(_fileName, editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                // Delete the file.
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                editor.Text = string.Empty;
            }
        }
    }
    ```

    Этот код определяет поле `_fileName`, которое ссылается на файл с именем `notes.txt`, где будут храниться данные с заметками в локальной папке данных для приложения. При выполнении конструктора страниц файл считывается, если он существует, и отображается в [`Editor`](xref:Xamarin.Forms.Editor). При нажатии кнопки **Сохранить** [`Button`](xref:Xamarin.Forms.Button) выполняется обработчик событий `OnSaveButtonClicked`, который сохраняет содержимое `Editor` в файле. При нажатии кнопки **Удалить** `Button` выполняется обработчик событий `OnDeleteButtonClicked`, который удаляет файл при условии, что он существует, и весь текст из `Editor`. Дополнительные сведения о взаимодействии с пользователем см. в разделе [Реакция на действия пользователей](deepdive.md#responding-to-user-interaction) в статье [Подробное изучение кратких руководств по Оболочке в Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **NotesPage.xaml.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

13. На **Панели решения** в проекте **Notes** выберите папку **Views**, щелкните правой кнопкой мыши и выберите **Добавить > Новый файл...** : В диалоговом окне **Новый файл** выберите **Forms > Forms ContentPage XAML**, назовите новый файл **AboutPage** и нажмите кнопку **Создать**.

    ![Добавление AboutPage](app-images/vsmac/add-aboutpage.png)

14. На **Панели решения** дважды щелкните файл **AboutPage.xaml** в проекте **Notes**, чтобы открыть его:

    ![Открытие файла AboutPage.xaml](app-images/vsmac/open-aboutpage-xaml.png)

    В результате этого в папку **Views** будет добавлена новая страница с именем **AboutPage**.

15. Удалите из **AboutPage.xaml** весь шаблонный код и замените его приведенным ниже.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.AboutPage"
                 Title="About">
        <!-- Layout children in two rows -->
        <Grid RowDefinitions="Auto,*">
            <Image Source="xamarin_logo.png"
                   BackgroundColor="{OnPlatform iOS=LightSlateGray, Android=#2196F3}"
                   VerticalOptions="Center"
                   HeightRequest="64" />
            <!-- Layout children vertically -->
            <StackLayout Grid.Row="1"
                         Margin="20"
                         Spacing="20">
                <Label FontSize="22">
                    <Label.FormattedText>
                        <FormattedString>
                            <FormattedString.Spans>
                                <Span Text="Notes"
                                      FontAttributes="Bold"
                                      FontSize="22" />
                                <Span Text=" v1.0" />
                            </FormattedString.Spans>
                        </FormattedString>
                    </Label.FormattedText>
                </Label>
                <Label Text="This app is written in XAML and C# with the Xamarin Platform." />
                <Button Text="Learn more"
                        Clicked="OnButtonClicked" />
            </StackLayout>
        </Grid>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, которая состоит из [`Image`](xref:Xamarin.Forms.Image), двух объектов [`Label`](xref:Xamarin.Forms.Label), которые отображают текст, и [`Button`](xref:Xamarin.Forms.Button). Два объекта `Label` и `Button` располагаются по горизонтали в [`StackLayout`](xref:Xamarin.Forms.StackLayout), а `Image` и `StackLayout` — по вертикали в [`Grid`](xref:Xamarin.Forms.Grid). Дополнительные сведения о создании пользовательского интерфейса см. в разделе [Пользовательский интерфейс](deepdive.md#user-interface) в статье [Подробное изучение кратких руководств по Оболочке в Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **AboutPage.xaml**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

16. На **Панели решения** дважды щелкните файл **AboutPage.xaml.cs** в проекте **Notes**, чтобы открыть его:

    ![Открытие файла AboutPage.xaml.cs](app-images/vsmac/open-aboutpage-codebehind.png)

17. Удалите из **AboutPage.xaml.cs** весь шаблонный код и замените его приведенным ниже.

    ```csharp
    using System;
    using Xamarin.Essentials;
    using Xamarin.Forms;

    namespace Notes.Views
    {
        public partial class AboutPage : ContentPage
        {
            public AboutPage()
            {
                InitializeComponent();
            }

            async void OnButtonClicked(object sender, EventArgs e)
            {
                // Launch the specified URL in the system browser.
                await Launcher.OpenAsync("https://aka.ms/xamarin-quickstart");
            }
        }
    }
    ```

    Этот код определяет обработчик событий `OnButtonClicked`, который выполняется при нажатии кнопки **Подробнее** [`Button`](xref:Xamarin.Forms.Button). При нажатии кнопки запускается веб-браузер и отображается страница, представленная аргументом URI для метода `OpenAsync`. Дополнительные сведения о взаимодействии с пользователем см. в разделе [Реакция на действия пользователей](deepdive.md#responding-to-user-interaction) в статье [Подробное изучение кратких руководств по Оболочке в Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **AboutPage.xaml.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

18. На **Панели решения** дважды щелкните файл **AppShell.xaml** в проекте **Notes**, чтобы открыть его:

    ![Открытие файла AppShell.xaml](app-images/vsmac/open-appshell-xaml.png)

19. Удалите из **AppShell.xaml** весь шаблонный код и замените его приведенным ниже.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <Shell xmlns="http://xamarin.com/schemas/2014/forms"
           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
           xmlns:views="clr-namespace:Notes.Views"
           x:Class="Notes.AppShell">
        <!-- Display a bottom tab bar containing two tabs -->
        <TabBar>
            <ShellContent Title="Notes"
                          Icon="icon_feed.png"
                          ContentTemplate="{DataTemplate views:NotesPage}" />
            <ShellContent Title="About"
                          Icon="icon_about.png"
                          ContentTemplate="{DataTemplate views:AboutPage}" />
        </TabBar>
    </Shell>
    ```    

    Этот код декларативно определяет визуальную иерархию приложения, которая состоит из `TabBar`, где содержатся два объекта `ShellContent`. Эти объекты не представляют собой какие-либо элементы пользовательского интерфейса, они служат лишь для организации визуальной структуры приложения. На основе этих объектов Оболочка создает пользовательский интерфейс для содержимого. Дополнительные сведения о создании пользовательского интерфейса см. в разделе [Пользовательский интерфейс](deepdive.md#user-interface) в статье [Подробное изучение кратких руководств по Оболочке в Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **AppShell.xaml**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

20. На **Панели решения** в проекте **Notes** разверните **AppShell.xaml** и дважды щелкните файл **AppShell.xaml.cs**, чтобы открыть его:

    ![Открытие файла AppShell.xaml.cs](app-images/vsmac/open-appshell-codebehind.png)

21. Удалите из **AppShell.xaml.cs** весь шаблонный код и замените его приведенным ниже.

    ```csharp
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class AppShell : Shell
        {
            public AppShell()
            {
                InitializeComponent();
            }
        }
    }
    ```

    Сохраните изменения в файле **AppShell.xaml.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

22. На **Панели решения** дважды щелкните файл **App.xaml** в проекте **Notes**, чтобы открыть его.

    ![Открытие файла App.xaml](app-images/vsmac/open-app-xaml.png)

23. Удалите из **App.xaml** весь шаблонный код и замените его приведенным ниже.

    ```xaml
    <?xml version="1.0" encoding="utf-8" ?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">

    </Application>
    ```

    Этот код декларативно определяет класс `App`, который отвечает за создание экземпляра приложения.

    Сохраните изменения в файле **App.xaml**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

24. На **Панели решения** в проекте **Notes** разверните **App.xaml** и дважды щелкните файл **App.xaml.cs**, чтобы открыть его:

    ![Открытие App.xaml.cs](app-images/vsmac/open-app-codebehind.png)

25. Удалите из **App.xaml.cs** весь шаблонный код и замените его приведенным ниже.

    ```csharp
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class App : Application
        {

            public App()
            {
                InitializeComponent();
                MainPage = new AppShell();
            }

            protected override void OnStart()
            {
            }

            protected override void OnSleep()
            {
            }

            protected override void OnResume()
            {
            }
        }
    }
    ```

    Этот код определяет код программной части для класса `App`, который отвечает за создание экземпляра приложения. Он инициализирует свойство [`MainPage`](xref:Xamarin.Forms.Application.MainPage) для производного класса `Shell`.

    Сохраните изменения в файле **App.xaml.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

### <a name="building-the-quickstart"></a>Сборка примера из краткого руководства

1. В Visual Studio для Mac выберите элемент меню **Сборка > Собрать все** (или нажмите клавиши **&#8984;+B**). Будет выполнена сборка проектов, а на панели инструментов Visual Studio для Mac отобразится сообщение об успешном выполнении:

      ![Сборка успешно выполнена](app-images/vsmac/build-successful.png)

    При наличии ошибок повторите предыдущие шаги и исправьте все ошибки, пока сборка проектов не будет проходить успешно.

2. В **панели решения** щелкните правой кнопкой мыши проект **Notes.iOS** и выберите команду **Назначить запускаемым проектом**:

      ![Назначение iOS запускаемым проектом](app-images/vsmac/set-startup-project-ios.png)

3. На панели инструментов Visual Studio для Mac нажмите клавишу **Запустить** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS:

      ![Панель инструментов Visual Studio для Mac](app-images/vsmac/start.png)

      ![Приложение Notes в симуляторе iOS](app-images/vsmac/notes1-ios.png)

    Введите примечание и нажмите кнопку **Сохранить**. Закройте приложение и повторно запустите его, чтобы убедиться, что введенные заметки перезагружены.

    Нажмите значок **About** для перехода к `AboutPage`:

      ![Страница About приложения Notes в симуляторе iOS](app-images/vsmac/notes2-ios.png)

    Нажмите кнопку **Подробнее**, чтобы запустить веб-страницу кратких руководств.

    Дополнительные сведения о запуске приложения на каждой платформе см. в разделе [Запуск приложения на каждой платформе](deepdive.md#launch-the-application-on-each-platform) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

4. В **панели решения** щелкните правой кнопкой мыши проект **Notes.Droid** и выберите команду **Назначить запускаемым проектом**:

      ![Назначение Android запускаемым проектом](app-images/vsmac/set-startup-project-android.png)

5. На панели инструментов Visual Studio для Mac нажмите клавишу **Запустить** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном эмуляторе Android:

      ![Приложение Notes в эмуляторе Android](app-images/vsmac/notes1-android.png)

    Введите примечание и нажмите кнопку **Сохранить**. Закройте приложение и повторно запустите его, чтобы убедиться, что введенные заметки перезагружены.

    Нажмите значок **About** для перехода к `AboutPage`:

      ![Страница About приложения Notes в Android Emulator](app-images/vsmac/notes2-android.png)

    Нажмите кнопку **Подробнее**, чтобы запустить веб-страницу кратких руководств.

    Дополнительные сведения о запуске приложения на каждой платформе см. в разделе [Запуск приложения на каждой платформе](deepdive.md#launch-the-application-on-each-platform) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

::: zone-end

## <a name="next-steps"></a>Следующие шаги

В этом кратком руководстве рассматривались следующие темы:

- Создание приложения Оболочки в Xamarin.Forms
- Определение пользовательского интерфейса страницы с помощью языка XAML и взаимодействие с элементами XAML из кода.
- Описание визуальной иерархии приложения Оболочки путем создания подкласса класса `Shell`.

Перейдите к следующему краткому руководству, чтобы добавить дополнительные страницы в это приложение Оболочки в Xamarin.Forms.

> [!div class="nextstepaction"]
> [Вперед](navigation.md)

## <a name="related-links"></a>Связанные ссылки

- [Заметки (пример)](/samples/xamarin/xamarin-forms-samples/getstarted-notes-app/)
- [Подробное изучение кратких руководств по Оболочке в Xamarin.Forms](deepdive.md)
