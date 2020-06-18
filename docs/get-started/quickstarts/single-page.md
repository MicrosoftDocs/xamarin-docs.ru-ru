---
title: 'title: "Создание одностраничного приложения Xamarin.Forms" description: "В этой статье описывается, как с помощью Xamarin.Forms создать одностраничное кроссплатформенное приложение, которое позволяет ввести заметку и сохранить ее в хранилище устройства".'
description: 'zone_pivot_groups: platform-dev16 ms.topic: quickstart ms.prod: xamarin ms.assetid: E8CF05B1-54B9-428B-8518-D068837BD61E ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 04/01/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
zone_pivot_groups: platform-dev16
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: E8CF05B1-54B9-428B-8518-D068837BD61E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b9a3017fc8188d3669b64d95c968b2d0a5325358
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/10/2020
ms.locfileid: "84136075"
---
# <a name="create-a-single-page-xamarinforms-application"></a>Создание одностраничного приложения Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-singlepage/)

В этом кратком руководстве рассматриваются следующие темы:

- Создание кроссплатформенного приложения Xamarin.Forms.
- Определение пользовательского интерфейса страницы с помощью языка XAML.
- Взаимодействие с элементами пользовательского интерфейса XAML из кода.

В этом кратком руководстве приводятся инструкции по созданию кроссплатформенного приложения Xamarin.Forms, которое позволяет ввести заметку и сохранить ее в хранилище устройства. Ниже показано итоговое приложение:

[![](single-page-images/screenshots-sml.png "Notes Application")](single-page-images/screenshots.png#lightbox "Notes Application")

::: zone pivot="windows"

### <a name="prerequisites"></a>Предварительные требования

- Последний выпуск Visual Studio 2019 с установленной рабочей нагрузкой **Разработка мобильных приложений на .NET**.
- Знание языка C#.
- (Необязательно) Парный компьютер Mac для построения приложения в iOS.

Дополнительные сведения об этих предварительных требованиях см. в разделе [Установка Xamarin](~/get-started/installation/index.md). Сведения о подключении Visual Studio 2019 к узлу сборки Mac см. в статье [Связывание с Mac при разработке для Xamarin.iOS](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

## <a name="get-started-with-visual-studio-2019"></a>Начало работы с Visual Studio 2019

1. Запустите Visual Studio 2019 и в начальном окне щелкните **Создать проект**, чтобы создать новый проект:

    ![](single-page-images/vs/new-solution-2019.png "New Project")

2. В окне **Создать проект** в раскрывающемся списке **Тип проекта** выберите **Мобильное приложение**, а затем выберите шаблон **Мобильное приложение (Xamarin.Forms)** и нажмите кнопку **Далее**:

    ![](single-page-images/vs/new-project-2019.png "Cross-Platform Project Templates")

3. В диалоговом окне **Настроить новый проект** в поле **Имя проекта** укажите **Notes**, выберите подходящее расположение для проекта и нажмите кнопку **Создать**:

    ![](single-page-images/vs/configure-project.png "Configure your Project")

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого краткого руководства предполагают, что решение называется **Notes**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого краткого руководства в решение.

4. В диалоговом окне **Новое кроссплатформенное приложение** выберите **Пустое приложение** и нажмите кнопку **ОК**:

    ![](single-page-images/vs/new-app-2019.png "New Cross-Platform App")

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](deepdive.md#anatomy-of-a-xamarinforms-application) статьи [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

5. В **обозревателе решений** дважды щелкните файл **MainPage.xaml** в проекте **Notes**, чтобы открыть его:

    ![](single-page-images/vs/open-mainpage-xaml-2019.png "Open MainPage.xaml")

6. Удалите из **MainPage.xaml** весь шаблонный код и замените его приведенным ниже.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, которая состоит из [`Label`](xref:Xamarin.Forms.Label) для отображения текста, [`Editor`](xref:Xamarin.Forms.Editor) для ввода текста, а также двух экземпляров [`Button`](xref:Xamarin.Forms.Button), которые помогают приложению сохранить или удалить файл. Два экземпляра `Button` располагаются по горизонтали в [`Grid`](xref:Xamarin.Forms.Grid), а `Label`, `Editor` и `Grid` — по вертикали в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Дополнительные сведения о создании пользовательского интерфейса см. в разделе [Пользовательский интерфейс](deepdive.md#user-interface) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **MainPage.xaml**, нажав клавиши **CTRL+S**, и закройте файл.

7. В **обозревателе решений** в проекте **Notes** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его:

    ![](single-page-images/vs/open-mainpage-codebehind-2019.png "Open MainPage.xaml.cs")

8. Удалите из **MainPage.xaml.cs** весь шаблонный код и замените его приведенным ниже.

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                editor.Text = string.Empty;
            }
        }
    }
    ```

    Этот код определяет поле `_fileName`, которое ссылается на файл с именем `notes.txt`, где будут храниться данные с заметками в локальной папке данных для приложения. При выполнении конструктора страниц файл считывается, если он существует, и отображается в [`Editor`](xref:Xamarin.Forms.Editor). При нажатии кнопки **Сохранить** [`Button`](xref:Xamarin.Forms.Button) выполняется обработчик событий `OnSaveButtonClicked`, который сохраняет содержимое `Editor` в файле. При нажатии кнопки **Удалить** `Button` выполняется обработчик событий `OnDeleteButtonClicked`, который удаляет файл при условии, что он существует, и весь текст из `Editor`. Дополнительные сведения о взаимодействии с пользователем см. в разделе [Реакция на действия пользователей](deepdive.md#responding-to-user-interaction) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **MainPage.xaml.cs**, нажав клавиши **CTRL+S**, и закройте файл.

### <a name="building-the-quickstart"></a>Сборка примера из краткого руководства

1. В Visual Studio выберите элемент меню **Сборка > Построить решение** (или нажмите клавишу F6). Выполняется сборка решения, а в строке состояния Visual Studio отображается сообщение об успешном выполнении:

      ![](single-page-images/vs/build-succeeded.png "Build Succeeded")

    При наличии ошибок повторите предыдущие шаги и исправьте все ошибки, пока сборка решения не будет проходить успешно.

2. На панели инструментов Visual Studio нажмите клавишу **Запустить** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном эмуляторе Android.

    ![](single-page-images/vs/android-start.png "Visual Studio Android Toolbar")

    [![](single-page-images/vs/notes-android.png "Notes in the Android Emulator")](single-page-images/vs/notes-android-large.png#lightbox "Notes in the Android Simulator")

    Введите примечание и нажмите кнопку **Сохранить**.

    Дополнительные сведения о запуске приложения на каждой платформе см. в разделе [Запуск приложения на каждой платформе](deepdive.md#launching-the-application-on-each-platform) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    > [!NOTE]
    > Следующие шаги следует выполнить, только если у вас есть [связанный компьютер Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md), отвечающий требованиям к системе для разработки приложений Xamarin.Forms.

3. На панели инструментов Visual Studio щелкните правой кнопкой мыши проект **Notes.iOS**, а затем выберите команду **Назначить запускаемым проектом**.

      ![](single-page-images/vs/set-as-startup-project-ios.png "Set iOS as Startup Project")

4. На панели инструментов Visual Studio нажмите клавишу **Запустить** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном [удаленном эмуляторе для iOS](~/tools/ios-simulator/index.md).

    ![](single-page-images/vs/ios-start.png "Visual Studio iOS Toolbar")

    [![](single-page-images/vs/notes-ios.png "Notes in the iOS Simulator")](single-page-images/vs/notes-ios-large.png#lightbox "Notes in the iOS Simulator")

    Введите примечание и нажмите кнопку **Сохранить**.

    Дополнительные сведения о запуске приложения на каждой платформе см. в разделе [Запуск приложения на каждой платформе](deepdive.md#launching-the-application-on-each-platform) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

::: zone-end
::: zone pivot="win-vs2017"

### <a name="prerequisites"></a>Предварительные требования

- Выпуск Visual Studio 2017 с установленной рабочей нагрузкой **Разработка мобильных приложений на .NET**.
- Знание языка C#.
- (Необязательно) Парный компьютер Mac для построения приложения в iOS.

Дополнительные сведения об этих предварительных требованиях см. в разделе [Установка Xamarin](~/get-started/installation/index.md). Сведения о подключении Visual Studio 2019 к узлу сборки Mac см. в статье [Связывание с Mac при разработке для Xamarin.iOS](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

## <a name="get-started-with-visual-studio-2017"></a>Начало работы с Visual Studio 2017

1. Запустите Visual Studio 2017 и на начальной странице щелкните **Создать проект**, чтобы создать новый проект:

    ![](single-page-images/vs/new-solution.png "New Project")

2. В диалоговом окне **Новый проект** щелкните **Кроссплатформенный**, выберите шаблон **Мобильное приложение (Xamarin.Forms)** , присвойте параметру "Имя" значение **Notes**, выберите подходящее расположение для проекта и нажмите кнопку **ОК**:

    ![](single-page-images/vs/new-project.png "Cross-Platform Project Templates")

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого краткого руководства предполагают, что решение называется **Notes**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого краткого руководства в решение.

3. В диалоговом окне **Новое кроссплатформенное приложение** щелкните **Пустое приложение**, выберите **.NET Standard** в качестве стратегии совместного использования кода, а затем нажмите кнопку **OK**:

    ![](single-page-images/vs/new-app.png "New Cross-Platform App")

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](deepdive.md#anatomy-of-a-xamarinforms-application) статьи [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

4. В **обозревателе решений** дважды щелкните файл **MainPage.xaml** в проекте **Notes**, чтобы открыть его:

    ![](single-page-images/vs/open-mainpage-xaml.png "Open MainPage.xaml")

5. Удалите из **MainPage.xaml** весь шаблонный код и замените его приведенным ниже.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, которая состоит из [`Label`](xref:Xamarin.Forms.Label) для отображения текста, [`Editor`](xref:Xamarin.Forms.Editor) для ввода текста, а также двух экземпляров [`Button`](xref:Xamarin.Forms.Button), которые помогают приложению сохранить или удалить файл. Два экземпляра `Button` располагаются по горизонтали в [`Grid`](xref:Xamarin.Forms.Grid), а `Label`, `Editor` и `Grid` — по вертикали в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Дополнительные сведения о создании пользовательского интерфейса см. в разделе [Пользовательский интерфейс](deepdive.md#user-interface) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **MainPage.xaml**, нажав клавиши **CTRL+S**, и закройте файл.

6. В **обозревателе решений** в проекте **Notes** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его:

    ![](single-page-images/vs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

7. Удалите из **MainPage.xaml.cs** весь шаблонный код и замените его приведенным ниже.

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                editor.Text = string.Empty;
            }
        }
    }
    ```

    Этот код определяет поле `_fileName`, которое ссылается на файл с именем `notes.txt`, где будут храниться данные с заметками в локальной папке данных для приложения. При выполнении конструктора страниц файл считывается, если он существует, и отображается в [`Editor`](xref:Xamarin.Forms.Editor). При нажатии кнопки **Сохранить** [`Button`](xref:Xamarin.Forms.Button) выполняется обработчик событий `OnSaveButtonClicked`, который сохраняет содержимое `Editor` в файле. При нажатии кнопки **Удалить** `Button` выполняется обработчик событий `OnDeleteButtonClicked`, который удаляет файл при условии, что он существует, и весь текст из `Editor`. Дополнительные сведения о взаимодействии с пользователем см. в разделе [Реакция на действия пользователей](deepdive.md#responding-to-user-interaction) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **MainPage.xaml.cs**, нажав клавиши **CTRL+S**, и закройте файл.

### <a name="building-the-quickstart"></a>Сборка примера из краткого руководства

1. В Visual Studio выберите элемент меню **Сборка > Построить решение** (или нажмите клавишу F6). Выполняется сборка решения, а в строке состояния Visual Studio отображается сообщение об успешном выполнении:

      ![](single-page-images/vs/build-succeeded.png "Build Succeeded")

    При наличии ошибок повторите предыдущие шаги и исправьте все ошибки, пока сборка решения не будет проходить успешно.

2. На панели инструментов Visual Studio нажмите клавишу **Запустить** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном эмуляторе Android.

    ![](single-page-images/vs/android-start.png "Visual Studio Android Toolbar")

    [![](single-page-images/vs/notes-android.png "Notes in the Android Emulator")](single-page-images/vs/notes-android-large.png#lightbox "Notes in the Android Simulator")

    Введите примечание и нажмите кнопку **Сохранить**.

    Дополнительные сведения о запуске приложения на каждой платформе см. в разделе [Запуск приложения на каждой платформе](deepdive.md#launching-the-application-on-each-platform) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    > [!NOTE]
    > Следующие шаги следует выполнить, только если у вас есть [связанный компьютер Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md), отвечающий требованиям к системе для разработки приложений Xamarin.Forms.

3. На панели инструментов Visual Studio щелкните правой кнопкой мыши проект **Notes.iOS**, а затем выберите команду **Назначить запускаемым проектом**.

      ![](single-page-images/vs/set-as-startup-project-ios.png "Set iOS as Startup Project")

4. На панели инструментов Visual Studio нажмите клавишу **Запустить** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном [удаленном эмуляторе для iOS](~/tools/ios-simulator/index.md).

    ![](single-page-images/vs/ios-start.png "Visual Studio iOS Toolbar")

    [![](single-page-images/vs/notes-ios.png "Notes in the iOS Simulator")](single-page-images/vs/notes-ios-large.png#lightbox "Notes in the iOS Simulator")

    Введите примечание и нажмите кнопку **Сохранить**.

    Дополнительные сведения о запуске приложения на каждой платформе см. в разделе [Запуск приложения на каждой платформе](deepdive.md#launching-the-application-on-each-platform) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

::: zone-end
::: zone pivot="macos"

### <a name="prerequisites"></a>Предварительные требования

- Visual Studio для Mac (последний выпуск) с поддержкой платформ Android и iOS.
- Xcode (последний выпуск).
- Знание языка C#.

Дополнительные сведения об этих предварительных требованиях см. в разделе [Установка Xamarin](~/get-started/installation/index.md).

## <a name="get-started-with-visual-studio-for-mac"></a>Начало работы с Visual Studio для Mac

1. Запустите Visual Studio для Mac и в начальном окне щелкните **Создать**, чтобы создать новый проект:

    ![](single-page-images/vsmac/new-project.png "New Solution")

2. В диалоговом окне **Выберите шаблон из нового проекта** щелкните **Многоплатформенность > Приложение** и выберите шаблон **Приложение с пустыми формами**, а затем нажмите кнопку **Далее**:

    ![](single-page-images/vsmac/choose-template.png "Choose a Template")

3. В диалоговом окне **Настройка приложения с пустыми формами** присвойте новому приложению имя **Notes**, убедитесь, что переключатель установлен в положение **Использовать .NET Standard** и нажмите кнопку **Далее**:    

    ![](single-page-images/vsmac/configure-app.png "Configure the Forms Application")

4. В диалоговом окне **Настроить новое приложение с пустыми формами** сохраните для проекта и решения имя **Notes**, выберите подходящее расположение для проекта и нажмите кнопку **Создать** для создания проекта:

    ![](single-page-images/vsmac/configure-project.png "Configure the Forms Project")

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого краткого руководства предполагают, что решение и проект называются **Notes**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого краткого руководства в проект.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](deepdive.md#anatomy-of-a-xamarinforms-application) статьи [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

5. В **панели решения** дважды щелкните файл **MainPage.xaml** в проекте **Notes**, чтобы открыть его:

    ![](single-page-images/vsmac/mainpage-xaml.png "MainPage.xaml")

6. Удалите из **MainPage.xaml** весь шаблонный код и замените его приведенным ниже.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, которая состоит из [`Label`](xref:Xamarin.Forms.Label) для отображения текста, [`Editor`](xref:Xamarin.Forms.Editor) для ввода текста, а также двух экземпляров [`Button`](xref:Xamarin.Forms.Button), которые помогают приложению сохранить или удалить файл. Два экземпляра `Button` располагаются по горизонтали в [`Grid`](xref:Xamarin.Forms.Grid), а `Label`, `Editor` и `Grid` — по вертикали в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Дополнительные сведения о создании пользовательского интерфейса см. в разделе [Пользовательский интерфейс](deepdive.md#user-interface) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в **MainPage.xaml**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**, и закройте файл.

7. В **панели решений** в проекте **Notes** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его:

    ![](single-page-images/vsmac/mainpage-xaml-cs.png "MainPage.xaml.cs")

8. Удалите из **MainPage.xaml.cs** весь шаблонный код и замените его приведенным ниже.

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                editor.Text = string.Empty;
            }
        }
    }
    ```

    Этот код определяет поле `_fileName`, которое ссылается на файл с именем `notes.txt`, где будут храниться данные с заметками в локальной папке данных для приложения. При выполнении конструктора страниц файл считывается, если он существует, и отображается в [`Editor`](xref:Xamarin.Forms.Editor). При нажатии кнопки **Сохранить** [`Button`](xref:Xamarin.Forms.Button) выполняется обработчик событий `OnSaveButtonClicked`, который сохраняет содержимое `Editor` в файле. При нажатии кнопки **Удалить** `Button` выполняется обработчик событий `OnDeleteButtonClicked`, который удаляет файл при условии, что он существует, и весь текст из `Editor`. Дополнительные сведения о взаимодействии с пользователем см. в разделе [Реакция на действия пользователей](deepdive.md#responding-to-user-interaction) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в **MainPage.xaml.cs**, выбрав **Файл > Сохранить** (или нажав клавиши **&#8984; + S**), а затем закройте файл.

### <a name="building-the-quickstart"></a>Сборка примера из краткого руководства

1. В Visual Studio для Mac выберите элемент меню **Сборка > Собрать все** (или нажмите клавиши **&#8984;+B**). Выполняется сборка проектов, а на панели инструментов Visual Studio для Mac отображается сообщение об успешном выполнении:

      ![](single-page-images/vsmac/build-successful.png "Build Successful")

    При наличии ошибок повторите предыдущие шаги и исправьте все ошибки, пока сборка проектов не будет проходить успешно.

2. В **панели решения** щелкните правой кнопкой мыши проект **Notes.iOS** и выберите команду **Назначить запускаемым проектом**:

      ![](single-page-images/vsmac/set-startup-project-ios.png "Set iOS as Startup Project")

3. На панели инструментов Visual Studio для Mac нажмите клавишу **Запустить** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS:

      ![](single-page-images/vsmac/start.png "Visual Studio for Mac Toolbar")

      [![](single-page-images/vsmac/notes-ios.png "Notes in the iOS Simulator")](single-page-images/vsmac/notes-ios-large.png#lightbox "Notes in the iOS Simulator")

    Введите примечание и нажмите кнопку **Сохранить**.

    Дополнительные сведения о запуске приложения на каждой платформе см. в разделе [Запуск приложения на каждой платформе](deepdive.md#launching-the-application-on-each-platform) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

4. В **панели решения** щелкните правой кнопкой мыши проект **Notes.Droid** и выберите команду **Назначить запускаемым проектом**:

      ![](single-page-images/vsmac/set-startup-project-android.png "Set Android as Startup Project")

5. На панели инструментов Visual Studio для Mac нажмите клавишу **Запустить** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном эмуляторе Android:

      [![](single-page-images/vsmac/notes-android.png "Notes in the Android Emulator")](single-page-images/vsmac/notes-android-large.png#lightbox "Notes in the Android Simulator")

    Введите примечание и нажмите кнопку **Сохранить**.

    Дополнительные сведения о запуске приложения на каждой платформе см. в разделе [Запуск приложения на каждой платформе](deepdive.md#launching-the-application-on-each-platform) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

::: zone-end

## <a name="next-steps"></a>Следующие шаги

В этом кратком руководстве рассматривались следующие темы:

- Создание кроссплатформенного приложения Xamarin.Forms.
- Определение пользовательского интерфейса страницы с помощью языка XAML.
- Взаимодействие с элементами пользовательского интерфейса XAML из кода.

Чтобы узнать, как создать многостраничное приложение на основе этого одностраничного приложения, перейдите к следующему краткому руководству.

> [!div class="nextstepaction"]
> [Вперед](multi-page.md)

## <a name="related-links"></a>Связанные ссылки

- [Заметки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-singlepage/)
- [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md)
