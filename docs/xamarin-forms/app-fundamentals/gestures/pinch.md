---
title: "Добавление распознавателя жестов сжатия" description: "В этой статье поясняется, как использовать жест сжатия для интерактивного масштабирования изображения в месте сжатия".
ms.prod: xamarin ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 01/21/2016 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="adding-a-pinch-gesture-recognizer"></a>Добавление распознавателя жестов сжатия

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-pinchgesture)

_Жест сжатия используется для интерактивного масштабирования и реализуется с помощью класса PinchGestureRecognizer. Стандартное назначение жеста сжатия — интерактивное масштабирование изображения в месте сжатия. Для этого изменяется масштаб содержимого в окне просмотра, как показано в этой статье._

Чтобы элемент пользовательского интерфейса можно было масштабировать жестом сжатия, необходимо создать экземпляр [`PinchGestureRecognizer`](xref:Xamarin.Forms.PinchGestureRecognizer), реализовать обработку события [`PinchUpdated`](xref:Xamarin.Forms.PinchGestureRecognizer.PinchUpdated) и добавить новый распознаватель жестов в коллекцию [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) элемента пользовательского интерфейса. В следующем примере кода показан распознаватель `PinchGestureRecognizer`, присоединенный к элементу [`Image`](xref:Xamarin.Forms.Image).

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

Реализация в XAML показана в следующем примере кода:

```xaml
<Image Source="waterfront.jpg">
  <Image.GestureRecognizers>
    <PinchGestureRecognizer PinchUpdated="OnPinchUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Затем код обработчика событий `OnPinchUpdated` добавляется в файл кода программной части:

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  // Handle the pinch
}
```

## <a name="creating-a-pinchtozoom-container"></a>Создание контейнера PinchToZoom

Для выполнения преобразований в пользовательском интерфейсе в ответ на жест сжатия требуется ряд математических операций. В этом разделе представлен универсальный вспомогательный класс для интерактивного масштабирования любого элемента пользовательского интерфейса. Следующий пример кода демонстрирует класс `PinchToZoomContainer`:

```csharp
public class PinchToZoomContainer : ContentView
{
  ...

  public PinchToZoomContainer ()
  {
    var pinchGesture = new PinchGestureRecognizer ();
    pinchGesture.PinchUpdated += OnPinchUpdated;
    GestureRecognizers.Add (pinchGesture);
  }

  void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
  {
    ...
  }
}
```

Этот класс может служить оболочкой для элемента пользовательского интерфейса, содержимое которого масштабируется в ответ на жест сжатия. В следующем примере кода XAML показан контейнер `PinchToZoomContainer`, содержащий элемент [`Image`](xref:Xamarin.Forms.Image).

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PinchGesture;assembly=PinchGesture"
             x:Class="PinchGesture.HomePage">
    <ContentPage.Content>
        <Grid Padding="20">
            <local:PinchToZoomContainer>
                <local:PinchToZoomContainer.Content>
                    <Image Source="waterfront.jpg" />
                </local:PinchToZoomContainer.Content>
            </local:PinchToZoomContainer>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

В следующем примере кода показано, как элемент [`Image`](xref:Xamarin.Forms.Image) заключается в контейнер `PinchToZoomContainer` на странице C#.

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new Grid {
      Padding = new Thickness (20),
      Children = {
        new PinchToZoomContainer {
          Content = new Image { Source = ImageSource.FromFile ("waterfront.jpg") }
        }
      }
    };
  }
}
```

Когда для элемента [`Image`](xref:Xamarin.Forms.Image) выполняется жест сжатия, масштаб отображаемого изображения увеличивается или уменьшается. Масштабирование осуществляется с помощью метода `PinchZoomContainer.OnPinchUpdated`, который показан в следующем примере кода.

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  if (e.Status == GestureStatus.Started) {
    // Store the current scale factor applied to the wrapped user interface element,
    // and zero the components for the center point of the translate transform.
    startScale = Content.Scale;
    Content.AnchorX = 0;
    Content.AnchorY = 0;
  }
  if (e.Status == GestureStatus.Running) {
    // Calculate the scale factor to be applied.
    currentScale += (e.Scale - 1) * startScale;
    currentScale = Math.Max (1, currentScale);

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the X pixel coordinate.
    double renderedX = Content.X + xOffset;
    double deltaX = renderedX / Width;
    double deltaWidth = Width / (Content.Width * startScale);
    double originX = (e.ScaleOrigin.X - deltaX) * deltaWidth;

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the Y pixel coordinate.
    double renderedY = Content.Y + yOffset;
    double deltaY = renderedY / Height;
    double deltaHeight = Height / (Content.Height * startScale);
    double originY = (e.ScaleOrigin.Y - deltaY) * deltaHeight;

    // Calculate the transformed element pixel coordinates.
    double targetX = xOffset - (originX * Content.Width) * (currentScale - startScale);
    double targetY = yOffset - (originY * Content.Height) * (currentScale - startScale);

    // Apply translation based on the change in origin.
    Content.TranslationX = targetX.Clamp (-Content.Width * (currentScale - 1), 0);
    Content.TranslationY = targetY.Clamp (-Content.Height * (currentScale - 1), 0);

    // Apply scale factor.
    Content.Scale = currentScale;
  }
  if (e.Status == GestureStatus.Completed) {
    // Store the translation delta's of the wrapped user interface element.
    xOffset = Content.TranslationX;
    yOffset = Content.TranslationY;
  }
}
```

Этот метод изменяет масштаб элемента пользовательского интерфейса, заключенного в оболочку, в соответствии с жестом сжатия. Масштаб, применяемый в месте сжатия, рассчитывается исходя из значений свойств [`Scale`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale), [`ScaleOrigin`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin) и [`Status`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Status) экземпляра [`PinchGestureUpdatedEventArgs`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs). После этого заключенный в оболочку элемент интерфейса масштабируется в месте сжатия путем присвоения свойствам [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX), [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) и [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) вычисленных значений.

## <a name="related-links"></a>Связанные ссылки

- [PinchGesture (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-pinchgesture)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PinchGestureRecognizer](xref:Xamarin.Forms.PinchGestureRecognizer)
