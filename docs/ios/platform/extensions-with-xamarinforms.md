---
title: Повторное использование страниц Xamarin.Forms в расширении iOS
description: В этом документе описываются расширения iOS и использование на них страниц Xamarin. Forms. Он содержит пошаговое руководство по созданию расширения, интеграции Xamarin. Forms и визуализации простого ContentPage в проекте расширения iOS.
ms.prod: xamarin
ms.assetid: 50076FFD-3EF6-41CC-BC7E-210DE3958F5B
ms.technology: xamarin-ios
author: alexeystrakh
ms.author: alstrakh
ms.date: 05/13/2020
ms.openlocfilehash: ed7b7bae452db0067b330126315d5b029a08ccab
ms.sourcegitcommit: 63029dd7ea4edb707a53ea936ddbee684a926204
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2021
ms.locfileid: "98608940"
---
# <a name="reuse-xamarinforms-pages-in-an-ios-extension"></a>Повторное использование страниц Xamarin.Forms в расширении iOS

расширения iOS позволяют настраивать существующее поведение системы, добавляя дополнительные функции в [стандартные точки расширения iOS и macOS](https://developer.apple.com/library/archive/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW2), такие как настраиваемые контекстные действия, автозаполнение паролей, фильтры входящих вызовов, модификаторы содержимого уведомлений и многое другое. Xamarin. iOS поддерживает расширения, и [это руководство](./extensions.md) поможет вам создать расширение iOS с помощью инструментов Xamarin.

Расширения распространяются как часть приложения-контейнера и активируются из определенной точки расширения в ведущем приложении. Приложение-контейнер обычно представляет собой простое приложение iOS, которое предоставляет пользователю сведения о расширении, о том, как активировать и использовать его. Существует три основных подхода к совместному использованию кода между расширением и приложением-контейнером.

1. Общий проект iOS.

    Вы можете разместить весь общий код между контейнером и расширением в общую библиотеку iOS и ссылаться на библиотеку из обоих проектов. Обычно общая библиотека содержит собственный Уивиевконтроллерс и должна быть библиотекой Xamarin. iOS.

1. Ссылки на файлы.

    В некоторых случаях приложение-контейнер предоставляет большую часть функциональных возможностей, пока расширение должно визуализировать одно `UIViewController` . При наличии нескольких файлов для совместного использования часто добавьте ссылку на файл в приложение расширения из файла, расположенного в приложении-контейнере.

1. Распространенный проект Xamarin. Forms.

    Если страницы приложений уже используются совместно с другой платформой, например Android, с помощью платформы Xamarin. Forms, распространенным подходом является повторное внедрение обязательных страниц в проекте расширения, так как расширение iOS работает с собственными Уивиевконтроллерс, а не со страницами Xamarin. Forms. Для использования Xamarin. Forms в расширении iOS необходимо выполнить дополнительные действия, описанные ниже.

## <a name="xamarinforms-in-an-ios-extension-project"></a>Xamarin. Forms в проекте расширения iOS

Возможность использования Xamarin. Forms в собственном проекте предоставляется с помощью [собственных форм](~/xamarin-forms/platform/native-forms.md). Это позволяет `ContentPage` добавлять производные страницы непосредственно в собственные проекты Xamarin. iOS. `CreateViewController`Метод расширения преобразует экземпляр страницы Xamarin. Forms в собственный `UIViewController` , который можно использовать или изменить как обычный контроллер. Так как расширение iOS является специальным видом собственного проекта iOS, здесь также можно использовать **собственные формы** .

> [!IMPORTANT]
> Существует множество [известных ограничений](./extensions.md#limitations) для расширений iOS. Несмотря на то, что Xamarin. Forms можно использовать в расширении iOS, это следует делать очень осторожно, отслеживая использование памяти и время запуска. В противном случае расширение может быть завершено iOS без какого бы то ни было способа корректной обработки.

## <a name="walkthrough"></a>Пошаговое руководство

В этом пошаговом руководстве предстоит создать приложение Xamarin. Forms, расширение Xamarin. iOS и повторно использовать общий код в проекте расширения:

1. Откройте Visual Studio для Mac, создайте новый проект Xamarin. Forms, используя шаблон **приложения пустых форм** , и назовите его **формсшарикстенсион**:

    ![Создание проекта](extensions-xf-images/1.walkthrough-createproject.png)

1. В **формсшарикстенсион/MainPage. XAML** замените содержимое следующим макетом:

    ```xaml
    <?xml version="1.0" encoding="utf-8" ?>
    <ContentPage
        x:Class="FormsShareExtension.MainPage"
        xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:d="http://xamarin.com/schemas/2014/forms/design"
        xmlns:local="clr-namespace:FormsShareExtension"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        x:DataType="local:MainPageViewModel"
        BackgroundColor="Orange"
        mc:Ignorable="d">
        <ContentPage.BindingContext>
            <local:MainPageViewModel Message="Hello from Xamarin.Forms!" />
        </ContentPage.BindingContext>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <Label
                Margin="20"
                Text="{Binding Message}"
                VerticalOptions="CenterAndExpand" />
            <Button Command="{Binding DoCommand}" Text="Do the job!" />
        </StackLayout>
    </ContentPage>
    ```

1. Добавьте новый класс с именем **маинпажевиевмоде** в проект **формсшарикстенсион** и замените содержимое класса следующим кодом:

    ```csharp
    using System;
    using System.ComponentModel;
    using System.Windows.Input;
    using Xamarin.Forms;

    namespace FormsShareExtension
    {
        public class MainPageViewModel : INotifyPropertyChanged
        {
            public event PropertyChangedEventHandler PropertyChanged;

            private string _message;
            public string Message
            {
                get { return _message; }
                set
                {
                    if (_message != value)
                    {
                        _message = value;
                        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(Message)));
                    }
                }
            }

            private ICommand _doCommand;
            public ICommand DoCommand
            {
                get { return _doCommand; }
                set
                {
                    if(_doCommand != value)
                    {
                        _doCommand = value;
                        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(DoCommand)));
                    }
                }
            }

            public MainPageViewModel()
            {
                DoCommand = new Command(OnDoCommandExecuted);
            }

            private void OnDoCommandExecuted(object state)
            {
                Message = $"Job {Environment.TickCount} has been completed!";
            }
        }
    }
    ```

    Код является общим для всех платформ и также будет использоваться расширением iOS.

1. На панели решения щелкните решение правой кнопкой мыши, выберите **добавить > новый проект > iOS > расширение действия >**, назовите **его и** нажмите кнопку **создать**:

    ![На снимке экрана показан выбор шаблона с выбранным расширением действия.](extensions-xf-images/2.walkthrough-createextension.png)

1. Чтобы использовать Xamarin. Forms в расширении iOS и общем коде, необходимо добавить необходимые ссылки:

    - Щелкните правой кнопкой мыши расширение iOS, выберите **ссылки > добавить эталонные > проекты > формсшарикстенсион** и нажмите кнопку **ОК**.

    - Щелкните правой кнопкой мыши расширение iOS, выберите **пакеты > Управление пакетами NuGet... > Xamarin. Forms**  и нажмите кнопку **Добавить пакет**.

1. Разверните проект расширения и измените точку входа для инициализации Xamarin. Forms и создания страниц. В соответствии с требованиями iOS расширение должно определять точку входа в **info. plist** как `NSExtensionMainStoryboard` или `NSExtensionPrincipalClass` . Когда точка входа активирована, в данном случае это `ActionViewController.ViewDidLoad` метод, можно создать экземпляр страницы Xamarin. Forms и отобразить его для пользователя. Поэтому откройте точку входа и замените `ViewDidLoad` метод следующей реализацией:

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();

        // Initialize Xamarin.Forms framework
        global::Xamarin.Forms.Forms.Init();
        // Create an instance of XF page with associated View Model
        var xfPage = new MainPage();
        var viewModel = (MainPageViewModel)xfPage.BindingContext;
        viewModel.Message = "Welcome to XF Page created from an iOS Extension";
        // Override the behavior to complete the execution of the Extension when a user press the button
        viewModel.DoCommand = new Command(() => DoneClicked(this));
        // Convert XF page to a native UIViewController which can be consumed by the iOS Extension
        var newController = xfPage.CreateViewController();
        // Present new view controller as a regular view controller
        this.PresentModalViewController(newController, false);
    }
    ```

    `MainPage`Экземпляр создается с помощью стандартного конструктора и перед его использованием в расширении преобразуется в машинный код с `UIViewController` помощью `CreateViewController` метода расширения. 
    
    Выполните сборку и запуск приложения:

    ![На снимке экрана показано сообщение Hello from Xamarin Dot Forms на мобильном устройстве.](extensions-xf-images/3.walkthrough-runapp.png)

    Чтобы активировать расширение, перейдите в браузер Safari, введите любой веб-адрес, например [Microsoft.com](https://microsoft.com), нажмите кнопку "Навигация", а затем нажмите значок " **поделиться** " в нижней части страницы, чтобы просмотреть доступные расширения действий. В списке доступных расширений выберите расширение **мяктион** , коснувшись его:

    ![На снимке экрана показана страница Microsoft Teams дополнительные сведения с значком общего доступа, выделенным на мобильном устройстве.](extensions-xf-images/4.walkthrough-run1.png) ![На снимке экрана показана официальная домашняя страница с Мяктион, выделенной на мобильном устройстве.](extensions-xf-images/5.walkthrough-run2.png) ![На снимке экрана показана страница X F, созданная на основе сообщения расширения i O на мобильном устройстве.](extensions-xf-images/6.walkthrough-run3.png)

    Расширение активируется, и пользователю отображается страница Xamarin. Forms. Все привязки и команды работают так же, как в приложении контейнера.

1. Исходный контроллер представления точки входа виден, так как он создается и активируется iOS. Чтобы устранить эту проблему, измените стиль модальной презентации на `UIModalPresentationStyle.FullScreen` для нового контроллера, добавив следующий код непосредственно перед `PresentModalViewController` вызовом:

    ```csharp
    newController.ModalPresentationStyle = UIModalPresentationStyle.FullScreen;
    ```

    Сборка и запуск в симуляторе iOS или на устройстве:

    ![Xamarin. Forms в расширении iOS](extensions-xf-images/7.walkthrough-result.png)

    > [!IMPORTANT]
    > Для сборки устройства убедитесь, что используются правильные параметры сборки и конфигурация **выпуска** , как [описано здесь](~/iOS/platform/extensions.md#debug-and-release-versions-of-extensions).

## <a name="related-links"></a>Связанные ссылки

- [Расширения iOS в Xamarin.iOS](~/iOS/platform/extensions.md)
- [Xamarin. Forms в проектах Xamarin Native](~/xamarin-forms/platform/native-forms.md)
- [Оптимизация эффективности и производительности расширения приложения для iOS](https://developer.apple.com/library/archive/documentation/General/Conceptual/ExtensibilityPG/ExtensionCreation.html#//apple_ref/doc/uid/TP40014214-CH5-SW7)
- [Исходный код примера](https://github.com/xamcat/xamarin-forms-ios-extension)