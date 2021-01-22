---
ms.openlocfilehash: 11427616ce20e47fb2b4d87c62098ececd3ed972
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98690179"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

Для работы с этим руководством у вас должен быть последний выпуск Visual Studio 2019 с установленной рабочей нагрузкой **Разработка мобильных приложений на .NET**. Кроме того, вам потребуется компьютер Mac для сборки учебного приложения на iOS. Сведения об установке платформы Xamarin см. в статье [Установка Xamarin](~/get-started/installation/index.md). Сведения о подключении Visual Studio 2019 к узлу сборки Mac см. в статье [Связывание с Mac при разработке для Xamarin.iOS](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Запустите Visual Studio и создайте пустое приложение Xamarin.Forms **ImageTutorial**.

    > [!IMPORTANT]
    > Для фрагментов кода на C# и XAML в этом руководстве необходимо решение с именем **ImageTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. В **обозревателе решений** дважды щелкните файл **MainPage.xaml** в проекте **ImageTutorial**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ImageTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Image Source="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
                   HeightRequest="300" />
        </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Image`](xref:Xamarin.Forms.Image) в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Свойство [`Image.Source`](xref:Xamarin.Forms.Image.Source) задает отображаемый образ с помощью универсального кода ресурса (URI). Свойство [`Image.Source`](xref:Xamarin.Forms.Image.Source) типа [`ImageSource`](xref:Xamarin.Forms.ImageSource) позволяет такие источники образов, как файлы, универсальные коды ресурса (URI) или ресурсы. Дополнительные сведения см. в разделе [Displaying images](~/xamarin-forms/user-interface/images.md#display-images) (Отображение изображений) в руководстве [Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md) (Изображения в Xamarin.Forms).

    Свойство [`HeightRequest`](xref:Xamarin.Forms.VisualElement) указывает высоту `Image` в аппаратно-независимых единицах.

    > [!NOTE]
    > В этом примере нет необходимости задавать свойство [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest). Это обусловлено тем, что [`Image`](xref:Xamarin.Forms.Image) по умолчанию сохраняет пропорции изображения.

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS удаленной работы или эмуляторе Android:

    [![Снимок экрана: изображение в iOS и Android](../images/create-image.png "Представление изображения: изображение")](../images/create-image-large.png#lightbox "Представление изображения: изображение")

    > [!NOTE]
    > Просмотр [`Image`](xref:Xamarin.Forms.Image) автоматически кэширует скачанные изображения на 24 часа. Дополнительные сведения см. в разделе [Downloaded image caching](~/xamarin-forms/user-interface/images.md#downloaded-image-caching) (Кэширование скачанного изображения) в руководстве [Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md) (Изображения в Xamarin.Forms).

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

Для работы с этим руководством вам нужно установить Visual Studio для Mac (последний выпуск) с поддержкой платформ Android и iOS. Кроме того, вам потребуется Xcode (последний выпуск). Дополнительные сведения об установке платформы Xamarin см. в статье [Установка Xamarin](~/get-started/installation/index.md).

1. Запустите Visual Studio для Mac и создайте пустое приложение Xamarin.Forms **ImageTutorial**.

    > [!IMPORTANT]
    > Для фрагментов кода на C# и XAML в этом руководстве необходимо решение с именем **ImageTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. На **Панели решений** в проекте **ImageTutorial** дважды щелкните файл **MainPage.xaml**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ImageTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Image Source="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
                   HeightRequest="300" />
        </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Image`](xref:Xamarin.Forms.Image) в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Свойство [`Image.Source`](xref:Xamarin.Forms.Image.Source) задает отображаемый образ с помощью универсального кода ресурса (URI). Свойство [`Image.Source`](xref:Xamarin.Forms.Image.Source) типа [`ImageSource`](xref:Xamarin.Forms.ImageSource) позволяет такие источники образов, как файлы, универсальные коды ресурса (URI) или ресурсы. Дополнительные сведения см. в разделе [Displaying images](~/xamarin-forms/user-interface/images.md#display-images) (Отображение изображений) в руководстве [Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md) (Изображения в Xamarin.Forms).

    Свойство [`HeightRequest`](xref:Xamarin.Forms.VisualElement) указывает высоту `Image` в аппаратно-независимых единицах.

    > [!NOTE]
    > В этом примере нет необходимости задавать свойство [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest). Это обусловлено тем, что [`Image`](xref:Xamarin.Forms.Image) по умолчанию сохраняет пропорции изображения.

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android.

    [![Снимок экрана: изображение в iOS и Android](../images/create-image.png "Представление изображения: изображение")](../images/create-image-large.png#lightbox "Представление изображения: изображение")

    > [!NOTE]
    > Просмотр [`Image`](xref:Xamarin.Forms.Image) автоматически кэширует скачанные изображения на 24 часа. Дополнительные сведения см. в разделе [Downloaded image caching](~/xamarin-forms/user-interface/images.md#downloaded-image-caching) (Кэширование скачанного изображения) в руководстве [Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md) (Изображения в Xamarin.Forms).
