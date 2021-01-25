---
ms.openlocfilehash: 9932dbf5f03ba148fa49a24026738a870c21cfd5
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98634904"
---
[`Grid`](xref:Xamarin.Forms.Grid) — это макет, позволяющий упорядочивать дочерние элементы в строки и столбцы, которые могут иметь пропорциональные или абсолютные размеры. По умолчанию `Grid` содержит одну строку и один столбец.

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

Для работы с этим руководством у вас должен быть последний выпуск Visual Studio 2019 с установленной рабочей нагрузкой **Разработка мобильных приложений на .NET**. Кроме того, вам потребуется компьютер Mac для сборки учебного приложения на iOS. Сведения об установке платформы Xamarin см. в статье [Установка Xamarin](~/get-started/installation/index.md). Сведения о подключении Visual Studio 2019 к узлу сборки Mac см. в статье [Связывание с Mac при разработке для Xamarin.iOS](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Запустите Visual Studio и создайте пустое приложение Xamarin.Forms **GridTutorial**.

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого руководства предполагают, что решение называется **GridTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. В **обозревателе решений** дважды щелкните файл **MainPage.xaml** в проекте **GridTutorial**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="GridTutorial.MainPage">
        <Grid Margin="20,35,20,20">
            <Label Text="The Grid has its Margin property set, to control the rendering position of the Grid." />
        </Grid>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Label`](xref:Xamarin.Forms.Label) в [`Grid`](xref:Xamarin.Forms.Grid). По умолчанию `Grid` помещает свои дочерние представления в одном месте. Таким образом, `Grid`, который содержит несколько дочерних элементов, должен указывать столбцы и строки, которые будут рассматриваться в следующем упражнении. Кроме того, свойство [`Margin`](xref:Xamarin.Forms.View.Margin) указывает позицию отрисовки `Grid` в [`ContentPage`](xref:Xamarin.Forms.ContentPage).

    > [!NOTE]
    > В дополнение к свойству [`Margin`](xref:Xamarin.Forms.View.Margin), у [`Grid`](xref:Xamarin.Forms.Grid) также можно установить свойство [`Padding`](xref:Xamarin.Forms.Layout.Padding). Значение свойства [`Padding`](xref:Xamarin.Forms.Layout.Padding) указывает расстояние между границами `Grid` и его дочерних элементов. Дополнительные сведения см. в статье [Поля и заполнение](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

1. На панели инструментов Visual Studio нажмите клавишу **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном удаленном симуляторе iOS или эмуляторе Android.

    [![Снимок экрана: метка в сетке в iOS и Android](../images/create-grid.png "Сетка, содержащая метку")](../images/create-grid-large.png#lightbox "Сетка, содержащая метки")

    Дополнительные сведения о [`Grid`](xref:Xamarin.Forms.Grid) см. в статье [Сетка Xamarin.Forms](~/xamarin-forms/user-interface/layouts/grid.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

Для работы с этим руководством вам нужно установить Visual Studio для Mac (последний выпуск) с поддержкой платформ Android и iOS. Кроме того, вам потребуется Xcode (последний выпуск). Дополнительные сведения об установке платформы Xamarin см. в статье [Установка Xamarin](~/get-started/installation/index.md).

1. Запустите Visual Studio для Mac и создайте пустое приложение Xamarin.Forms **GridTutorial**.

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого руководства предполагают, что решение называется **GridTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. На **Панели решения** дважды щелкните файл **MainPage.xaml** в проекте **GridTutorial**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="GridTutorial.MainPage">
        <Grid Margin="20,35,20,20">
            <Label Text="The Grid has its Margin property set, to control the rendering position of the Grid." />
        </Grid>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Label`](xref:Xamarin.Forms.Label) в [`Grid`](xref:Xamarin.Forms.Grid). По умолчанию `Grid` помещает свои дочерние представления в одном месте. Таким образом, `Grid`, который содержит несколько дочерних элементов, должен указывать столбцы и строки, которые будут рассматриваться в следующем упражнении. Кроме того, свойство [`Margin`](xref:Xamarin.Forms.View.Margin) указывает позицию отрисовки `Grid` в [`ContentPage`](xref:Xamarin.Forms.ContentPage).

    > [!NOTE]
    > В дополнение к свойству [`Margin`](xref:Xamarin.Forms.View.Margin), у [`Grid`](xref:Xamarin.Forms.Grid) также можно установить свойство [`Padding`](xref:Xamarin.Forms.Layout.Padding). Значение свойства [`Padding`](xref:Xamarin.Forms.Layout.Padding) задает расстояние между представлениями в `Grid`. Дополнительные сведения см. в статье [Поля и заполнение](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android.

    [![Снимок экрана: метка в сетке в iOS и Android](../images/create-grid.png "Сетка, содержащая метку")](../images/create-grid-large.png#lightbox "Сетка, содержащая метки")

    Дополнительные сведения о [`Grid`](xref:Xamarin.Forms.Grid) см. в статье [Сетка Xamarin.Forms](~/xamarin-forms/user-interface/layouts/grid.md).
