---
title: Xamarin.Forms ImageButton
description: Элемент ImageButton отображает изображение и реагирует на касание или щелчок, который направляет приложение для выполнения определенной задачи.
ms.prod: xamarin
ms.assetid: B5906AB6-3F79-4FCB-8C78-1F0AF18AB39E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3b27ef8ecbd5f357eabd728423b5787ea222c593
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562305"
---
# <a name="no-locxamarinforms-imagebutton"></a>Xamarin.Forms ImageButton

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Элемент ImageButton отображает изображение и реагирует на касание или щелчок, который направляет приложение для выполнения определенной задачи._

`ImageButton`Представление объединяет [`Button`](xref:Xamarin.Forms.Button) представление и [`Image`](xref:Xamarin.Forms.Image) представление для создания кнопки, содержимое которой является изображением. Пользователь нажимает `ImageButton` с помощью пальца или щелкает его с помощью мыши, чтобы направить приложение на выполнение определенной задачи. Однако, в отличие от `Button` представления, `ImageButton` представление не имеет концепции текста и внешнего вида текста.

> [!NOTE]
> Пока [`Button`](xref:Xamarin.Forms.Button) представление определяет [`Image`](xref:Xamarin.Forms.Button.Image) свойство, которое позволяет отображать изображение в `Button` , это свойство предназначено для использования при отображении маленького значка рядом с `Button` текстом.

Примеры кода из этого руководств взяты из [примера формсгаллери](/samples/xamarin/xamarin-forms-samples/formsgallery).

## <a name="setting-the-image-source"></a>Задание источника изображения

`ImageButton` Определяет `Source` свойство, которое должно быть задано для изображения, отображаемого на кнопке, в качестве источника изображения — файл, URI, ресурс или поток. Дополнительные сведения о загрузке изображений из разных источников см. [в разделе изображения Xamarin.Forms в ](images.md).

В следующем примере показано, как создать экземпляр `ImageButton` в XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FormsGallery.XamlExamples.ImageButtonDemoPage"
             Title="ImageButton Demo">
    <StackLayout>
        <Label Text="ImageButton"
               FontSize="50"
               FontAttributes="Bold"
               HorizontalOptions="Center" />

       <ImageButton Source="XamarinLogo.png"
                    HorizontalOptions="Center"
                    VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Source`Свойство определяет изображение, которое отображается в `ImageButton` . В этом примере задается локальный файл, который будет загружен из каждого проекта платформы, в результате чего будут отображены следующие снимки экрана:

[![Базовая ImageButton](imagebutton-images/BasicImageButton.png "Базовая ImageButton")](imagebutton-images/BasicImageButton-Large.png#lightbox "Базовая ImageButton")

По умолчанию `ImageButton` является прямоугольным, но можно передать скругленные углы с помощью `CornerRadius` Свойства. Дополнительные сведения о `ImageButton` внешнем представлении см. в разделе [представление ImageButton](#imagebutton-appearance).

> [!NOTE]
> Хотя `ImageButton` может загрузить анимированный GIF-файл, он будет отображать только первый кадр GIF-файла.

В следующем примере показано, как создать страницу, которая функционально эквивалентна предыдущему примеру XAML, но полностью в C#:

```csharp
public class ImageButtonDemoPage : ContentPage
{
    public ImageButtonDemoPage()
    {
        Label header = new Label
        {
            Text = "ImageButton",
            FontSize = 50,
            FontAttributes = FontAttributes.Bold,
            HorizontalOptions = LayoutOptions.Center
        };

        ImageButton imageButton = new ImageButton
        {
            Source = "XamarinLogo.png",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        // Build the page.
        Title = "ImageButton Demo";
        Content = new StackLayout
        {
            Children = { header, imageButton }
        };
    }
}
```

## <a name="handling-imagebutton-clicks"></a>Обработка щелчков ImageButton

`ImageButton` Определяет `Clicked` событие, возникающее при касании пользователем `ImageButton` пальцем или указателем мыши. Событие возникает при отпускании кнопки с пальцем или кнопкой мыши с поверхности `ImageButton` . Свойство должно иметь значение, равное, чтобы `ImageButton` `IsEnabled` `true` реагировать на касания.

В следующем примере показано, как создать экземпляр `ImageButton` в XAML и обменять его `Clicked` событие:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FormsGallery.XamlExamples.ImageButtonDemoPage"
             Title="ImageButton Demo">
    <StackLayout>
        <Label Text="ImageButton"
               FontSize="50"
               FontAttributes="Bold"
               HorizontalOptions="Center" />

       <ImageButton Source="XamarinLogo.png"
                    HorizontalOptions="Center"
                    VerticalOptions="CenterAndExpand"
                    Clicked="OnImageButtonClicked" />

        <Label x:Name="label"
               Text="0 ImageButton clicks"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Clicked`Для события задается обработчик событий с именем `OnImageButtonClicked` , который находится в файле кода программной части:

```csharp
public partial class ImageButtonDemoPage : ContentPage
{
    int clickTotal;

    public ImageButtonDemoPage()
    {
        InitializeComponent();
    }

    void OnImageButtonClicked(object sender, EventArgs e)
    {
        clickTotal += 1;
        label.Text = $"{clickTotal} ImageButton click{(clickTotal == 1 ? "" : "s")}";
    }
}
```

При касании `ImageButton` выполняется метод `OnImageButtonClicked`. `sender`Аргумент является `ImageButton` ответственным за это событие. Его можно использовать для доступа к `ImageButton` объекту или для различения нескольких объектов, `ImageButton` совместно использующих одно и то же `Clicked` событие.

Этот конкретный `Clicked` обработчик увеличивает счетчик и отображает значение счетчика в [`Label`](xref:Xamarin.Forms.Label) :

[![Базовая кнопка ImageButton Click](imagebutton-images/ImageButton.png "Базовая кнопка ImageButton Click")](imagebutton-images/ImageButton-Large.png#lightbox "Базовая кнопка ImageButton Click")

В следующем примере показано, как создать страницу, которая функционально эквивалентна предыдущему примеру XAML, но полностью в C#:

```csharp
public class ImageButtonDemoPage : ContentPage
{
    Label label;
    int clickTotal = 0;

    public ImageButtonDemoPage()
    {
        Label header = new Label
        {
            Text = "ImageButton",
            FontSize = 50,
            FontAttributes = FontAttributes.Bold,
            HorizontalOptions = LayoutOptions.Center
        };

        ImageButton imageButton = new ImageButton
        {
            Source = "XamarinLogo.png",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };
        imageButton.Clicked += OnImageButtonClicked;

        label = new Label
        {
            Text = "0 ImageButton clicks",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        // Build the page.
        Title = "ImageButton Demo";
        Content = new StackLayout
        {
            Children =
            {
                header,
                imageButton,
                label
            }
        };
    }

    void OnImageButtonClicked(object sender, EventArgs e)
    {
        clickTotal += 1;
        label.Text = $"{clickTotal} ImageButton click{(clickTotal == 1 ? "" : "s")}";
    }
}
```

## <a name="disabling-the-imagebutton"></a>Отключение ImageButton

Иногда приложение находится в определенном состоянии, в котором конкретный `ImageButton` щелчок не является допустимой операцией. В таких случаях `ImageButton` следует отключить, задав `IsEnabled` свойству значение `false` .

## <a name="using-the-command-interface"></a>Использование интерфейса команд

Приложение может реагировать на `ImageButton` касания без обработки `Clicked` события. `ImageButton`Класс реализует альтернативный механизм уведомления, называемый _командой_ или интерфейсом _командной строки_ . Это состоит из двух свойств:

- `Command` типа [`ICommand`](xref:System.Windows.Input.ICommand) — интерфейс, определенный в [`System.Windows.Input`](xref:System.Windows.Input) пространстве имен.
- `CommandParameter` свойство типа [`Object`](xref:System.Object) .

Этот подход подходит для связи с привязкой данных, особенно при реализации архитектуры Model-View-ViewModel (MVVM).

Дополнительные сведения об использовании интерфейса командной строки см [. в разделе](button.md) [Использование интерфейса команды](button.md#using-the-command-interface) в этой панели.

## <a name="pressing-and-releasing-the-imagebutton"></a>Нажатие и освобождение ImageButton

Помимо события `Clicked``ImageButton` также определяет события `Pressed` и `Released`. Это `Pressed` событие возникает, когда палец нажимает на `ImageButton` , или кнопка мыши нажата с указателем, расположенным над `ImageButton` . Это `Released` событие возникает при отпускании кнопки мыши или пальца. Как правило, `Clicked` событие также срабатывает в то же время, что и `Released` событие, но если палец или указатель мыши выходят за пределы области `ImageButton` до выпуска, `Clicked` событие может не произойти.

Дополнительные сведения об этих событиях см. в разделе, посвященном [нажатию и отпустите кнопку](button.md#pressing-and-releasing-the-button) [в данной панели](button.md) .

## <a name="imagebutton-appearance"></a>Вид ImageButton

В дополнение к свойствам, `ImageButton` унаследованным от [`View`](xref:Xamarin.Forms.View) класса, `ImageButton` также определяет несколько свойств, влияющих на его внешний вид:

- `Aspect` показывает, как изображение будет масштабироваться в соответствии с отображаемой областью.
- `BorderColor` цвет области, окружающей `ImageButton` .
- `BorderWidth` Ширина границы.
- `CornerRadius` Радиус угла `ImageButton` .

`Aspect`Свойству может быть присвоено одно из членов [`Aspect`](xref:Xamarin.Forms.Aspect) перечисления:

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) — растягивает изображение до полного и точного заполнения `ImageButton` . Это может привести к искажению изображения.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) — обрезает изображение таким образом, чтобы оно заполнило с `ImageButton` сохранением пропорций.
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) -леттербоксес изображение (при необходимости), чтобы весь образ поместился в `ImageButton` , с добавлением пробела в верхнюю или нижнюю часть или в зависимости от того, является ли изображение широким или максимальным. Это значение перечисления по умолчанию [`Aspect`](xref:Xamarin.Forms.Aspect) .

> [!NOTE]
> `ImageButton`Класс также имеет [`Margin`](xref:Xamarin.Forms.View.Margin) Свойства и `Padding` , управляющие поведением макета `ImageButton` . Дополнительные сведения см. в статье [Поля и заполнение](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

## <a name="imagebutton-visual-states"></a>Визуальные состояния ImageButton

`ImageButton` имеет объект `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) , который можно использовать для инициации визуального изменения в `ImageButton` при нажатии пользователем, при условии, что он включен.

В следующем примере XAML показано, как определить визуальное состояние для `Pressed` состояния:

```xaml
<ImageButton Source="XamarinLogo.png"
             ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="1" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="0.8" />
                </VisualState.Setters>
            </VisualState>

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</ImageButton>
```

`Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) Указывает, что при `ImageButton` нажатии элемента его [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) свойство будет заменено значением по умолчанию от 1 до 0,8. `Normal` `VisualState` Указывает, что если `ImageButton` находится в нормальном состоянии, его `Scale` свойство будет установлено в значение 1. Таким образом, общий результат заключается в том, что при `ImageButton` нажатии кнопки она масштабируется немного меньше, а при `ImageButton` освобождении она масштабируется до размера по умолчанию.

Дополнительные сведения о визуальных состояниях см. [в разделе Xamarin.Forms Диспетчер визуальных состояний](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Связанные ссылки

- [Пример Формсгаллери](/samples/xamarin/xamarin-forms-samples/formsgallery)