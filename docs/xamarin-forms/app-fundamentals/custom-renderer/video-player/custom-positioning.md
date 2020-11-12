---
title: Настраиваемая строка позиционирования видео
description: В этой статье объясняется, как реализовать настраиваемую строку позиционирования в приложении видеопроигрывателя с помощью Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 6D792264-30FF-46F7-8C1B-2FEF9D277DF4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 15529520668f4d6353fe99c13ddb2e6cc2801500
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93367248"
---
# <a name="custom-video-positioning"></a>Настраиваемая строка позиционирования видео

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

На каждой платформе реализуются элементы управления транспортировкой, в том числе строка позиционирования. Эта строка похожа на ползунок или полосу прокрутки и отображает общую продолжительность видео, а также текущую позицию его воспроизведения. Кроме того, с помощью строки позиционирования пользователь может перемещаться вперед или назад в другую позицию воспроизведения.

В этой статье описывается реализация собственной настраиваемой строки позиционирования.

## <a name="the-duration-property"></a>Свойство Duration

Для работы с настраиваемой строкой позиционирования объекту `VideoPlayer` требуются сведения о продолжительности видео. Объект `VideoPlayer` определяет доступное только для чтения свойство `Duration` типа `TimeSpan`:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Duration read-only property
        private static readonly BindablePropertyKey DurationPropertyKey =
            BindableProperty.CreateReadOnly(nameof(Duration), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan(),
                propertyChanged: (bindable, oldValue, newValue) => ((VideoPlayer)bindable).SetTimeToEnd());

        public static readonly BindableProperty DurationProperty = DurationPropertyKey.BindableProperty;

        public TimeSpan Duration
        {
            get { return (TimeSpan)GetValue(DurationProperty); }
        }

        TimeSpan IVideoPlayerController.Duration
        {
            set { SetValue(DurationPropertyKey, value); }
            get { return Duration; }
        }
        ···
    }
}
```

Как и свойство `Status`, описываемое в [предыдущей статье](custom-transport.md), свойство `Duration` доступно только для чтения. Оно определяется с использованием закрытого объекта `BindablePropertyKey` и может задаваться только посредством ссылки на интерфейс `IVideoPlayerController`, который содержит свойство `Duration`:

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPlayerController
    {
        VideoStatus Status { set; get; }

        TimeSpan Duration { set; get; }
    }
}
```

Также необходимо обратить внимание на изменяемый свойством обработчик, вызывающий метод `SetTimeToEnd`, который описывается далее в этой статье.

Продолжительность видео *не будет* доступна сразу же после задания свойства `Source` объекта `VideoPlayer`. Базовый видеопроигрыватель сможет определить продолжительность видеофайла только после его частичного скачивания.

Отрисовщики каждой из платформ получают сведения о продолжительности видео следующим образом:

### <a name="video-duration-in-ios"></a>Продолжительность видео в iOS

В iOS сведения о продолжительности видео извлекаются из свойства `Duration` объекта `AVPlayerItem`, не сразу же после создания объекта `AVPlayerItem`. Вы можете задать наблюдатель iOS для свойства `Duration`, однако объект `VideoPlayerRenderer` получает сведения о продолжительности в методе `UpdateStatus`, который вызывается 10 раз в секунду:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            if (playerItem != null)
            {
                ((IVideoPlayerController)Element).Duration = ConvertTime(playerItem.Duration);
                ···
            }
        }

        TimeSpan ConvertTime(CMTime cmTime)
        {
            return TimeSpan.FromSeconds(Double.IsNaN(cmTime.Seconds) ? 0 : cmTime.Seconds);

        }
        ···
    }
}
```

Метод `ConvertTime` преобразует объект `CMTime` в значение `TimeSpan`.

### <a name="video-duration-in-android"></a>Продолжительность видео в Android

Свойство `Duration` объекта `VideoView` в Android содержит действительную продолжительность видео в миллисекундах в том случае, если вызвано событие `Prepared` объекта `VideoView`. Класс `VideoPlayerRenderer` в Android использует этот обработчик для получения свойства `Duration`:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            ···
            ((IVideoPlayerController)Element).Duration = TimeSpan.FromMilliseconds(videoView.Duration);
        }
        ···
    }
}
```

### <a name="video-duration-in-uwp"></a>Продолжительность видео на универсальной платформе Windows (UWP)

Свойство `NaturalDuration` объекта `MediaElement` имеет значение типа `TimeSpan` и становится действительным, когда объект `MediaElement` вызывает событие `MediaOpened`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        void OnMediaElementMediaOpened(object sender, RoutedEventArgs args)
        {
            ((IVideoPlayerController)Element).Duration = Control.NaturalDuration.TimeSpan;
        }
        ···
    }
}
```

## <a name="the-position-property"></a>Свойство Position

Объекту `VideoPlayer` также требуется свойство `Position`, значение которого увеличивается от нуля до `Duration` по мере воспроизведения видео. Объект `VideoPlayer` реализует это свойство как свойство `Position` в объекте `MediaElement` универсальной платформы Windows, которое является обычным привязываемым свойством с открытыми методами доступа `set` и `get`:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Position property
        public static readonly BindableProperty PositionProperty =
            BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan(),
                propertyChanged: (bindable, oldValue, newValue) => ((VideoPlayer)bindable).SetTimeToEnd());

        public TimeSpan Position
        {
            set { SetValue(PositionProperty, value); }
            get { return (TimeSpan)GetValue(PositionProperty); }
        }
        ···
    }
}
```

Метод доступа `get` возвращает текущую позицию во время воспроизведения видео, а метод доступа `set` обеспечивает реакцию на действия пользователя, который изменяет позицию воспроизведения с помощью строки позиционирования.

В iOS и Android у свойства, позволяющего получить текущую позицию, есть только метод доступа `get`, а для выполнения второй задачи используется метод `Seek`. Если взглянуть глубже, использование метода `Seek` выглядит более разумным способом по сравнению с применением одного свойства `Position`. При работе с одним свойством `Position` возникает проблема, связанная с наследованием: по мере воспроизведения видео значение свойства `Position` должно непрерывно обновляться в соответствии с новой позицией. Однако в большинстве случаев изменения свойства `Position` не должны приводить к переходу видеопроигрывателя в новую позицию воспроизведения. В таких случаях видеопроигрыватель осуществлял бы поиск по последнему значению свойства `Position`, в результате чего воспроизведение видео не продвигалось бы.

Несмотря на сложности с реализацией свойства `Position` с методами доступа `set` и `get`, этот подход был выбран из-за его согласованности с объектом `MediaElement` UWP. Кроме того, он дает существенные преимущества в контексте привязки данных: свойство `Position` объекта `VideoPlayer` можно привязать к ползунку, который используется как для отображения текущей позиции, так и для поиска новой. Тем не менее, чтобы избежать циклов с обратной связью, при реализации такого свойства `Position` необходимо соблюдать ряд мер предосторожности.

### <a name="setting-and-getting-ios-position"></a>Задание и получение позиции в iOS

В iOS свойство `CurrentTime` объекта `AVPlayerItem` указывает на текущую позицию во время воспроизведения видео. Объект `VideoPlayerRenderer` в iOS задает свойство `Position` в обработчике `UpdateStatus` одновременно с заданием свойства `Duration`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            if (playerItem != null)
            {
                ···
                ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, ConvertTime(playerItem.CurrentTime));
            }
        }
        ···
    }
}
```

Отрисовщик обнаруживает, что свойство `Position`, заданное из объекта `VideoPlayer`, было изменено в переопределении `OnElementPropertyChanged` и использует новое значение для вызова метода `Seek` для объекта `AVPlayer`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                TimeSpan controlPosition = ConvertTime(player.CurrentTime);

                if (Math.Abs((controlPosition - Element.Position).TotalSeconds) > 1)
                {
                    player.Seek(CMTime.FromSeconds(Element.Position.TotalSeconds, 1));
                }
            }
        }
        ···
    }
}
```

Обратите внимание, что каждый раз, когда свойство `Position` в объекте `VideoPlayer` задается из обработчика `OnUpdateStatus`, свойство `Position` вызывает событие `PropertyChanged`, которое обнаруживается в переопределении `OnElementPropertyChanged`. В большинстве случаев при таких изменениях метод `OnElementPropertyChanged` не должен выполнять никакие действия. В противном случае при каждом изменении позиции видео осуществлялся бы переход в только что достигнутую позицию!

Чтобы избежать таких циклов обратной связи, метод `OnElementPropertyChanged` вызывает метод `Seek` только в том случае, если свойство `Position` отличается от текущей позиции объекта `AVPlayer` более чем на одну секунду.

### <a name="setting-and-getting-android-position"></a>Задание и получение позиции в Android

Как и в случае с отрисовщиком iOS, объект `VideoPlayerRenderer` в Android задает новое значение для свойства `Position` в обработчике `OnUpdateStatus`. Свойство `CurrentPosition` объекта `VideoView` содержит значение новой позиции в миллисекундах:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            TimeSpan timeSpan = TimeSpan.FromMilliseconds(videoView.CurrentPosition);
            ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, timeSpan);
        }
        ···
    }
}
```

Кроме того, как и в случае с отрисовщиком iOS, отрисовщик Android вызывает метод `SeekTo` объекта `VideoView` в том случае, если свойство `Position` изменилось и при этом его новое значение отличается от значения `CurrentPosition` объекта `VideoView` более чем на одну секунду:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                if (Math.Abs(videoView.CurrentPosition - Element.Position.TotalMilliseconds) > 1000)
                {
                    videoView.SeekTo((int)Element.Position.TotalMilliseconds);
                }
            }
        }
        ···
    }
}
```

### <a name="setting-and-getting-uwp-position"></a>Задание и получение позиции на универсальной платформе Windows

Объект `VideoPlayerRenderer` универсальной платформы Windows обрабатывает свойство `Position` тем же способом, что и отрисовщики iOS и Android. Тем не менее, поскольку свойство `Position` объекта `MediaElement` UWP также имеет значение типа `TimeSpan`, преобразование при этом не требуется:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                if (Math.Abs((Control.Position - Element.Position).TotalSeconds) > 1)
                {
                    Control.Position = Element.Position;
                }
            }
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, Control.Position);
        }
        ···
    }
}
```

## <a name="calculating-a-timetoend-property"></a>Вычисление значения свойства TimeToEnd

В некоторых случаях видеопроигрыватель отображает оставшееся время воспроизведения видео. Это значение в начале видео равно его продолжительности, а в конце уменьшается до нуля.

Объект `VideoPlayer` содержит доступное только для чтения свойство `TimeToEnd`, которое полностью обрабатывается в рамках класса на основании изменений значений свойств `Duration` и `Position`:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        private static readonly BindablePropertyKey TimeToEndPropertyKey =
            BindableProperty.CreateReadOnly(nameof(TimeToEnd), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan());

        public static readonly BindableProperty TimeToEndProperty = TimeToEndPropertyKey.BindableProperty;

        public TimeSpan TimeToEnd
        {
            private set { SetValue(TimeToEndPropertyKey, value); }
            get { return (TimeSpan)GetValue(TimeToEndProperty); }
        }

        void SetTimeToEnd()
        {
            TimeToEnd = Duration - Position;
        }
        ···
    }
}
```

Метод `SetTimeToEnd` вызывается из изменяемых свойством обработчиков объектов `Duration` и `Position`.

## <a name="a-custom-slider-for-video"></a>Настраиваемый ползунок для видео

Вы можете написать собственный элемент управления для строки позиционирования, а также использовать объект Xamarin.Forms `Slider` или класс, производный от `Slider`, например следующий класс `PositionSlider`. Этот класс определяет свойства `Duration` и `Position` типа `TimeSpan` которые привязываются к данным двух одноименных свойств объекта `VideoPlayer`. Обратите внимание, что по умолчанию для свойства `Position` применяется двусторонняя привязка данных:

```csharp
namespace FormsVideoLibrary
{
    public class PositionSlider : Slider
    {
        public static readonly BindableProperty DurationProperty =
            BindableProperty.Create(nameof(Duration), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(1),
                                    propertyChanged: (bindable, oldValue, newValue) =>
                                    {
                                        double seconds = ((TimeSpan)newValue).TotalSeconds;
                                        ((PositionSlider)bindable).Maximum = seconds <= 0 ? 1 : seconds;
                                    });

        public TimeSpan Duration
        {
            set { SetValue(DurationProperty, value); }
            get { return (TimeSpan)GetValue(DurationProperty); }
        }

        public static readonly BindableProperty PositionProperty =
            BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(0),
                                    defaultBindingMode: BindingMode.TwoWay,
                                    propertyChanged: (bindable, oldValue, newValue) =>
                                    {
                                        double seconds = ((TimeSpan)newValue).TotalSeconds;
                                        ((PositionSlider)bindable).Value = seconds;
                                    });

        public TimeSpan Position
        {
            set { SetValue(PositionProperty, value); }
            get { return (TimeSpan)GetValue(PositionProperty); }
        }

        public PositionSlider()
        {
            PropertyChanged += (sender, args) =>
            {
                if (args.PropertyName == "Value")
                {
                    TimeSpan newPosition = TimeSpan.FromSeconds(Value);

                    if (Math.Abs(newPosition.TotalSeconds - Position.TotalSeconds) / Duration.TotalSeconds > 0.01)
                    {
                        Position = newPosition;
                    }
                }
            };
        }
    }
}
```

Изменяемый свойством обработчик для свойства `Duration` присваивает свойству `Maximum` базового объекта `Slider` свойство `TotalSeconds` значения `TimeSpan`. Аналогичным образом, изменяемый свойством обработчик свойства `Position` задает свойство `Value` объекта `Slider`. Таким образом, базовый объект `Slider` отслеживает позицию объекта `PositionSlider`.

Объект `PositionSlider` обновляется на основе базового объекта `Slider` только в одном случае: когда пользователь выполняет какие-либо действия с объектом `Slider`, указывая на необходимость перехода в новую позицию воспроизведения видео. Такую ситуацию обнаруживает обработчик `PropertyChanged` в конструкторе `PositionSlider`. Обработчик проверяет наличие изменений в свойстве `Value`. Если оно отличается от свойства `Position`, то свойству `Position` присваивается значение свойства `Value`.

Теоретически, внутренняя инструкция `if` может быть записана следующим образом:

```csharp
if (newPosition.Seconds != Position.Seconds)
{
    Position = newPosition;
}
```

Тем не менее реализация объекта `Slider` в Android имеет всего 1000 дискретных шагов, независимо от значений параметров `Minimum` и `Maximum`. Если видео имеет продолжительность более 1000 секунд, два разных значения `Position` будут соответствовать одному и тому же значению параметра `Value` объекта `Slider`. Соответственно, такая инструкция `if` приведет к ложному положительному определению действий, выполняемых пользователем с объектом `Slider`. Вместо этого более безопасным будет убедиться, что новая и текущая позиция отличаются более чем на одну сотую долю от общей продолжительности видео.

## <a name="using-the-positionslider"></a>Использование объекта PositionSlider

В документации по объекту [`MediaElement`](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/) UWP приводится предупреждение о привязке к свойству `Position`, которое часто обновляется. В документации рекомендуется использовать таймер при запросе свойства `Position`.

Это хорошая рекомендация, однако в трех классах `VideoPlayerRenderer` при обновлении свойства `Position` таймер уже используется косвенным образом. Свойство `Position` изменяется в обработчике события `UpdateStatus`, которое вызывается только 10 раз в секунду.

Таким образом, свойство `Position` объекта `VideoPlayer` может быть привязано к свойству `Position` объекта `PositionSlider` без риска для производительности, что продемонстрировано на странице **Настраиваемая строка позиционирования** :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.CustomPositionBarPage"
             Title="Custom Position Bar">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           AreTransportControlsEnabled="False"
                           Source="{StaticResource ElephantsDream}" />

        ···

        <StackLayout Grid.Row="1"
                     Orientation="Horizontal"
                     Margin="10, 0"
                     BindingContext="{x:Reference videoPlayer}">

            <Label Text="{Binding Path=Position,
                                  StringFormat='{0:hh\\:mm\\:ss}'}"
                   VerticalOptions="Center"/>

            ···

            <Label Text="{Binding Path=TimeToEnd,
                                  StringFormat='{0:hh\\:mm\\:ss}'}"
                   VerticalOptions="Center" />
        </StackLayout>

        <video:PositionSlider Grid.Row="2"
                              Margin="10, 0, 10, 10"
                              BindingContext="{x:Reference videoPlayer}"
                              Duration="{Binding Duration}"           
                              Position="{Binding Position}">
            <video:PositionSlider.Triggers>
                <DataTrigger TargetType="video:PositionSlider"
                             Binding="{Binding Status}"
                             Value="{x:Static video:VideoStatus.NotReady}">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </video:PositionSlider.Triggers>
        </video:PositionSlider>
    </Grid>
</ContentPage>
```

Первая кнопка с многоточием (···) скрывает объект `ActivityIndicator`. Ее действие аналогично описываемому на предыдущей странице **Настраиваемая транспортировка**. Обратите внимание на два элемента `Label`, на которых отображаются свойства `Position` и `TimeToEnd`. Кнопка с многоточием между этими двумя элементами `Label` скрывает два элемента `Button`, показанные на странице **Настраиваемая транспортировка** для воспроизведения, приостановки и остановки. Логика кода программной части также аналогична странице **Настраиваемая транспортировка**.

[![Настраиваемое размещение](custom-positioning-images/custompositioning-small.png "Настраиваемое размещение")](custom-positioning-images/custompositioning-large.png#lightbox "Настраиваемое размещение")

Это заключительная статья, посвященная объекту `VideoPlayer`.

## <a name="related-links"></a>Связанные ссылки

- [Демоверсии видеопроигрывателя (пример)](/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)