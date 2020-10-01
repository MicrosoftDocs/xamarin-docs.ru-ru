---
title: Добавление распознавателя жестов сдвига
description: В этой статье описывается, как использовать жест сдвига для смещения изображения по горизонтали или вертикали, когда оно не помещается полностью в окне просмотра.
ms.prod: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8c9968ac7de45f3e6e239f82ba43b53abd05a647
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91558522"
---
# <a name="add-a-pan-gesture-recognizer"></a>Добавление распознавателя жестов сдвига

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-pangesture)

_Жест сдвига используется для обнаружения движения пальцев по экрану и применения этого движения к содержимому. Он реализуется с помощью класса `PanGestureRecognizer`. Жест сдвига обычно используется для смещения изображения по горизонтали или вертикали, когда оно не помещается полностью в окне просмотра. Для этого изображение перемещается в пределах окна просмотра, как показано в этой статье._

Чтобы элемент пользовательского интерфейса можно было перемещать жестом сдвига, необходимо создать экземпляр [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer), реализовать обработку события [`PanUpdated`](xref:Xamarin.Forms.PanGestureRecognizer.PanUpdated) и добавить новый распознаватель жестов в коллекцию [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) элемента пользовательского интерфейса. В следующем примере кода показан распознаватель `PanGestureRecognizer`, присоединенный к элементу [`Image`](xref:Xamarin.Forms.Image):

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

Реализация в XAML показана в следующем примере кода:

```xaml
<Image Source="MonoMonkey.jpg">
  <Image.GestureRecognizers>
    <PanGestureRecognizer PanUpdated="OnPanUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Затем код обработчика событий `OnPanUpdated` добавляется в файл кода программной части:

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  // Handle the pan
}
```

## <a name="creating-a-pan-container"></a>Создание контейнера сдвига

В этом разделе представлен универсальный вспомогательный класс для произвольного сдвига. Он обычно подходит для навигации по изображениям и картам. Для выполнения преобразований в пользовательском интерфейсе в ответ на жест сдвига требуется ряд математических операций. Они используются только для сдвига в пределах границ элемента пользовательского интерфейса, заключенного в оболочку. Следующий пример кода демонстрирует класс `PanContainer`:

```csharp
public class PanContainer : ContentView
{
  double x, y;

  public PanContainer ()
  {
    // Set PanGestureRecognizer.TouchPoints to control the
    // number of touch points needed to pan
    var panGesture = new PanGestureRecognizer ();
    panGesture.PanUpdated += OnPanUpdated;
    GestureRecognizers.Add (panGesture);
  }

  void OnPanUpdated (object sender, PanUpdatedEventArgs e)
  {
    ...
  }
}
```

Этот класс может служить оболочкой для элемента пользовательского интерфейса, содержимое которого сдвигается в ответ на жест. В следующем примере кода XAML показан контейнер `PanContainer`, содержащий элемент [`Image`](xref:Xamarin.Forms.Image).

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PanGesture"
             x:Class="PanGesture.HomePage">
    <ContentPage.Content>
        <AbsoluteLayout>
            <local:PanContainer>
                <Image Source="MonoMonkey.jpg" WidthRequest="1024" HeightRequest="768" />
            </local:PanContainer>
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

В следующем примере кода показано, как элемент [`Image`](xref:Xamarin.Forms.Image) заключается в контейнер `PanContainer` на странице C#.

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new AbsoluteLayout {
      Padding = new Thickness (20),
      Children = {
        new PanContainer {
          Content = new Image {
            Source = ImageSource.FromFile ("MonoMonkey.jpg"),
            WidthRequest = 1024,
            HeightRequest = 768
          }
        }
      }
    };
  }
}
```

В обоих примерах свойствам [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) и [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) присваиваются значения ширины и высоты отображаемого изображения.

Когда для элемента [`Image`](xref:Xamarin.Forms.Image) выполняется жест сдвига, отображаемое изображение смещается. Сдвиг осуществляется с помощью метода `PanContainer.OnPanUpdated`, который показан в следующем примере кода.

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  switch (e.StatusType) {
  case GestureStatus.Running:
    // Translate and ensure we don't pan beyond the wrapped user interface element bounds.
    Content.TranslationX =
      Math.Max (Math.Min (0, x + e.TotalX), -Math.Abs (Content.Width - App.ScreenWidth));
    Content.TranslationY =
      Math.Max (Math.Min (0, y + e.TotalY), -Math.Abs (Content.Height - App.ScreenHeight));
    break;

  case GestureStatus.Completed:
    // Store the translation applied during the pan
    x = Content.TranslationX;
    y = Content.TranslationY;
    break;
  }
}
```

Этот метод обновляет видимое содержимое заключенного в оболочку элемента пользовательского интерфейса в соответствии с жестом сдвига пользователя. Направление и расстояние сдвига рассчитываются исходя из значений свойств [`TotalX`](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalX) и [`TotalY`](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalY) экземпляра [`PanUpdatedEventArgs`](xref:Xamarin.Forms.PanUpdatedEventArgs). Свойства `App.ScreenWidth` и `App.ScreenHeight` содержат высоту и ширину окна просмотра. В качестве их значений задаются высота и ширина экрана устройства в зависимости от платформы. После этого заключенный в оболочку элемент интерфейса сдвигается путем присвоения свойствам [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) и [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) вычисленных значений.

Когда содержимое сдвигается внутри элемента, который занимает не весь экран, высоту и ширину окна просмотра можно получить из свойств [`Height`](xref:Xamarin.Forms.VisualElement.Height) и [`Width`](xref:Xamarin.Forms.VisualElement.Width) элемента.

> [!NOTE]
> Отображение изображений в высоком разрешении может значительно увеличивать объем памяти, занимаемой приложением. Поэтому их следует создавать только при необходимости и сразу же освобождать, если они больше не нужны приложению. Дополнительные сведения см. в разделе [Оптимизация графических ресурсов](~/xamarin-forms/deploy-test/performance.md#optimize-image-resources).

## <a name="related-links"></a>Связанные ссылки

- [PanGesture (пример)](/samples/xamarin/xamarin-forms-samples/workingwithgestures-pangesture)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PanGestureRecognizer](xref:Xamarin.Forms.PanGestureRecognizer)