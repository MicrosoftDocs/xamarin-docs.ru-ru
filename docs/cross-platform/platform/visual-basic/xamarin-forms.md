---
title: Xamarin. Forms с использованием Visual Basic.NET
description: Шаблон проекта Xamarin. Forms можно изменить, чтобы использовать Visual Basic для основной сборки. Это позволяет эффективно создавать кросс-платформенные мобильные приложения с помощью VB.NET.
ms.prod: xamarin
ms.assetid: da4b4ba9-9205-47dc-8bae-23272ede2c50
author: davidortinau
ms.author: daortin
ms.date: 04/24/2019
ms.openlocfilehash: 8cb26c3d8cec03cfaa12f7acb974c9944119bdfc
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374112"
---
# <a name="xamarinforms-using-visual-basicnet"></a>Xamarin. Forms с использованием Visual Basic.NET

Xamarin не поддерживает Visual Basic напрямую. Следуйте инструкциям на этой странице, чтобы создать решение Xamarin. Forms на C#, а затем замените проект .NET Standard C# на Visual Basic.

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/mobile-samples/visualbasic-xamarinformsvb/)

[![Создайте решение Xamarin. Forms, а затем замените .NET Standard проект Visual Basic](xamarin-forms-images/hero-sml.png)](xamarin-forms-images/hero.png#lightbox)

> [!NOTE]
> Для программирования с Visual Basic необходимо использовать Visual Studio в Windows.

## <a name="xamarinforms-with-visual-basic-walkthrough"></a>Пошаговое руководство по Xamarin. Forms с Visual Basic

Выполните следующие действия, чтобы создать простой проект Xamarin. Forms, использующий Visual Basic:

1. В Visual Studio 2019 выберите **создать новый проект**.

2. В окне **Создание нового проекта** введите **Xamarin. Forms** , чтобы отфильтровать список и выбрать **мобильное приложение (Xamarin. Forms)** , а затем нажмите кнопку **Далее**.

    [![Фильтр для приложений Xamarin. Forms](xamarin-forms-images/02-sml.png)](xamarin-forms-images/02.png#lightbox)

3. На следующем экране введите имя проекта и нажмите кнопку **создать**.

4. Выберите **пустой** шаблон и нажмите кнопку " **ОК** ":

    [![Пустой шаблон Xamarin. Forms](xamarin-forms-images/04-sml.png)](xamarin-forms-images/04.png#lightbox)

    При этом в Visual Studio создается решение Xamarin. Forms с использованием C#. Следующие шаги позволяют изменить решение для использования Visual Basic.

5. Щелкните решение правой кнопкой мыши и выберите **добавить > новый проект...**

6. Введите **Visual Basic библиотеку** , чтобы отфильтровать параметры проекта, и выберите параметр **библиотека классов (.NET Standard)** со значком Visual Basic:

    [![Фильтр для библиотеки Visual Basic](xamarin-forms-images/06-sml.png)](xamarin-forms-images/06.png#lightbox)

7. На следующем экране введите имя проекта и нажмите кнопку **создать**.

8. Щелкните правой кнопкой мыши проект Visual Basic и выберите пункт **Свойства** , а затем измените **пространство имен по умолчанию** , чтобы оно совпадало с существующими проектами C#:

    [![Убедитесь, что корневое пространство имен Visual Basic соответствует приложению Xamarin. Forms](xamarin-forms-images/07a-sml.png)](xamarin-forms-images/07a.png#lightbox)

9. Щелкните правой кнопкой мыши новый проект Visual Basic и выберите **Управление пакетами NuGet** , затем установите **Xamarin. Forms** и закройте окно диспетчера пакетов.

    [![Формы и закрытие окна диспетчера пакетов](xamarin-forms-images/07b-sml.png)](xamarin-forms-images/07b.png#lightbox)

10. Переименуйте файл **Class1. vb** по умолчанию в **app. vb** :

    [![Переименование файла Class1 по умолчанию и класса в приложение](xamarin-forms-images/08.png)](xamarin-forms-images/08.png#lightbox)

11. Вставьте следующий код в файл **app. vb** , который станет начальной точкой приложения Xamarin. Forms:

    ```vb
    Imports Xamarin.Forms

    Public Class App
        Inherits Application

        Public Sub New()
            Dim label = New Label With {.HorizontalTextAlignment = TextAlignment.Center,
                                        .FontSize = Device.GetNamedSize(NamedSize.Medium, GetType(Label)),
                                        .Text = "Welcome to Xamarin.Forms with Visual Basic.NET"}

            Dim stack = New StackLayout With {
                .VerticalOptions = LayoutOptions.Center
            }
            stack.Children.Add(label)

            Dim page = New ContentPage
            page.Content = stack
            MainPage = page

        End Sub

    End Class
    ```

12. Обновите проекты Android и iOS, чтобы они ссылались на новый проект Visual Basic (а не на проект C#, созданный шаблоном).
Щелкните правой кнопкой мыши узел **ссылки** в проектах Android и iOS, чтобы открыть **Диспетчер ссылок**. Отмените деление библиотеки C# и тикайте библиотеку Visual Basic (не забудьте сделать это для проектов Android и iOS).

    [![Удалить старую ссылку на проект, добавить Visual Basic ссылку](xamarin-forms-images/10-sml.png)](xamarin-forms-images/10.png#lightbox)

13. Удалите проект C#. Добавьте новые **VB** файлы для создания приложения Xamarin. Forms. Ниже приведен шаблон для новых `ContentPage` Visual Basic.

    ```vb
    Imports Xamarin.Forms

    Public Class Page2
    Inherits ContentPage

        Public Sub New()
            Dim label = New Label With {.HorizontalTextAlignment = TextAlignment.Center,
                                        .FontSize = Device.GetNamedSize(NamedSize.Medium, GetType(Label)),
                                        .Text = "Visual Basic ContentPage"}

            Dim stack = New StackLayout With {
                .VerticalOptions = LayoutOptions.Center
            }
            stack.Children.Add(label)

            Content = stack
        End Sub
    End Class
    ```

## <a name="limitations-of-visual-basic-in-xamarinforms"></a>Ограничения Visual Basic в Xamarin. Forms

Как указано на [странице Portable Visual Basic.NET](~/cross-platform/platform/visual-basic/index.md), Xamarin не поддерживает язык Visual Basic. Это означает, что существуют некоторые ограничения на то, где можно использовать Visual Basic:

- Страницы XAML не могут быть добавлены в проект Visual Basic. генератор кода программной части может создавать только C#. XAML можно включить в отдельную переносимую библиотеку классов C#, на которую имеется ссылка, и использовать привязку данных для заполнения XAML-файлов с помощью Visual Basic моделей (пример этого включен в [Пример](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)).

- Пользовательские модули подготовки отчетов не могут быть написаны на Visual Basic, они должны быть написаны на C# в проектах собственной платформы.

- Реализации служб зависимостей не могут быть написаны на Visual Basic, они должны быть написаны на C# в проектах собственной платформы.

## <a name="related-links"></a>Связанные ссылки

- [Ксамаринформсвб (пример)](/samples/xamarin/mobile-samples/visualbasic-xamarinformsvb/)
- [Межплатформенная разработка с помощью .NET Framework](/dotnet/standard/cross-platform/)