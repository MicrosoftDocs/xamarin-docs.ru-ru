---
ms.openlocfilehash: 58ed33adbc25e026431609370075c24c3b6ba690
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574651"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`Image`](xref:Xamarin.Forms.Image), чтобы настроить его внешний вид.

    ```xaml
    <Image Source="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
           Aspect="Fill"
           HeightRequest="{OnPlatform iOS=300, Android=250}"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    Этот код задает свойство [`Aspect`](xref:Xamarin.Forms.Image.Aspect), которое определяет режим масштабирования изображения, для [`Fill`](xref:Xamarin.Forms.Aspect.Fill). Элемент `Fill` определен в перечислении [`Aspect`](xref:Xamarin.Forms.Aspect) и растягивает изображение, чтобы полностью заполнить вид, независимо от того, является ли изображение искажено. Дополнительные сведения о масштабировании изображения см. в разделе [Displaying images](~/xamarin-forms/user-interface/images.md#display-images) (Отображение изображений) в руководстве [Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md) (Изображения в Xamarin.Forms).

    Расширение разметки `OnPlatform` позволяет настраивать внешний вид пользовательского интерфейса для каждой платформы. В этом примере расширение разметки используется для установки свойств [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) и [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) равным 300 независимым от устройства единицам на iOS и 250 независимым от устройства единицам на Android. Дополнительные сведения о расширении разметки `OnPlatform` см. в разделе [Расширение разметки OnPlatform](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension) в руководстве [Использование расширений разметки XAML](~/xamarin-forms/xaml/markup-extensions/consuming.md).

    Кроме того, свойство [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) указывает, что изображение будет отцентрировано по горизонтали.

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS удаленной работы или эмуляторе Android:

    [![Снимок экрана: изображение с разными размерами в iOS и Android](../images/customize-appearance.png "Изображение с разными размерами в зависимости от платформы")](../images/customize-appearance-large.png#lightbox "Изображение с разными размерами в зависимости от платформы")

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`Image`](xref:Xamarin.Forms.Image), чтобы настроить его внешний вид.

    ```xaml
    <Image Source="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
           Aspect="Fill"
           HeightRequest="{OnPlatform iOS=300, Android=250}"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    Этот код задает свойство [`Aspect`](xref:Xamarin.Forms.Image.Aspect), которое определяет режим масштабирования изображения, для [`Fill`](xref:Xamarin.Forms.Aspect.Fill). Элемент `Fill` определен в перечислении [`Aspect`](xref:Xamarin.Forms.Aspect) и растягивает изображение, чтобы полностью заполнить вид, независимо от того, является ли изображение искажено. Дополнительные сведения о масштабировании изображения см. в разделе [Displaying images](~/xamarin-forms/user-interface/images.md#display-images) (Отображение изображений) в руководстве [Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md) (Изображения в Xamarin.Forms).

    Расширение разметки `OnPlatform` позволяет настраивать внешний вид пользовательского интерфейса для каждой платформы. В этом примере расширение разметки используется для установки свойств [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) и [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) равными 300 в iOS и 250 в Android. Дополнительные сведения о расширении разметки `OnPlatform` см. в разделе [Расширение разметки OnPlatform](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension) в руководстве [Использование расширений разметки XAML](~/xamarin-forms/xaml/markup-extensions/consuming.md).

    Кроме того, свойство [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) указывает, что изображение будет отцентрировано по горизонтали.

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android.

    [![Снимок экрана: изображение с разными размерами в iOS и Android](../images/customize-appearance.png "Изображение с разными размерами в зависимости от платформы")](../images/customize-appearance-large.png#lightbox "Изображение с разными размерами в зависимости от платформы")
