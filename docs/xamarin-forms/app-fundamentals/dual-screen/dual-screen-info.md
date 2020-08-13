---
title: Возможности устройства с двумя экранами Xamarin.Forms
description: В этом руководстве рассматривается использование класса DualScreenInfo из Xamarin.Forms для оптимизации интерфейса приложения на двухэкранных устройствах, таких как Surface Duo и Surface Neo.
ms.prod: xamarin
ms.assetid: dd5eb074-f4cb-4ab4-b47d-76f862ac7cfa
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 05/19/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e1f0ee6b3c509fcdb15653789866a9ec2dc88791
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2020
ms.locfileid: "87918210"
---
# <a name="no-locxamarinforms-dualscreeninfo-helper-class"></a>Xamarin.Forms Вспомогательный класс DualScreenInfo

![Предварительный выпуск API](~/media/shared/preview.png)

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)

Класс `DualScreenInfo` позволяет определить, в какой области отображается ваше представление, сколько места оно занимает, в каком положении находится устройство, каков угол сгиба и т. д.

## <a name="configure-dualscreeninfo"></a>Настройка DualScreenInfo

Чтобы создать макет для двух экранов в приложении, выполните указанные ниже действия.

1. Выполните [начальные](index.md) инструкции, чтобы добавить NuGet и настроить класс Android `MainActivity`.
1. Добавьте `using Xamarin.Forms.DualScreen;` в файл класса.
1. Используйте класс `DualScreenInfo.Current` в приложении.

## <a name="properties"></a>Свойства

- При использовании двух экранов свойство `SpanningBounds` возвращает два прямоугольника, указывающих границы каждой видимой области. Если окно не занимает два экрана, возвращается пустой массив.
- `HingeBounds` указывает расположение сгиба на экране.
- `IsLandscape` указывает, находится ли устройство в альбомной ориентации. Это полезно, поскольку собственные API-интерфейсы устройства неверно сообщают ориентацию, когда приложение расширено на два экрана.
- `SpanMode` указывает режим макета: вертикальный, горизонтальный или однопанельный.

Кроме того, событие `PropertyChanged` срабатывает при изменении каких-либо свойств, а событие `HingeAngleChanged` срабатывает при изменении угла сгиба.

## <a name="poll-hinge-angle-on-android-and-uwp"></a>Запрос угла сгиба на Android и UWP

Следующий метод доступен при обращении к `DualScreenInfo` из проектов на платформе Android и UWP:

- `GetHingeAngleAsync` получает текущий угол сгиба устройства. При использовании симулятора свойство HingeAngle можно задать путем изменения значения датчика давления.

Этот метод можно вызвать из пользовательских отрисовщиков в Android и UWP. В следующем примере кода показан пользовательский отрисовщик для Android:

```csharp
public class HingeAngleLabelRenderer : Xamarin.Forms.Platform.Android.FastRenderers.LabelRenderer
{
    System.Timers.Timer _hingeTimer;
    public HingeAngleLabelRenderer(Context context) : base(context)
    {
    }

    async void OnTimerElapsed(object sender, System.Timers.ElapsedEventArgs e)
    {
        if (_hingeTimer == null)
            return;

        _hingeTimer.Stop();
        var hingeAngle = await DualScreenInfo.Current.GetHingeAngleAsync();

        Device.BeginInvokeOnMainThread(() =>
        {
            if (_hingeTimer != null)
                Element.Text = hingeAngle.ToString();
        });

        if (_hingeTimer != null)
            _hingeTimer.Start();
    }

    protected override void OnElementChanged(ElementChangedEventArgs<Label> e)
    {
        base.OnElementChanged(e);

        if (_hingeTimer == null)
        {
            _hingeTimer = new System.Timers.Timer(100);
            _hingeTimer.Elapsed += OnTimerElapsed;
            _hingeTimer.Start();
        }
    }

    protected override void Dispose(bool disposing)
    {
        if (_hingeTimer != null)
        {
            _hingeTimer.Elapsed -= OnTimerElapsed;
            _hingeTimer.Stop();
            _hingeTimer = null;
        }

        base.Dispose(disposing);
    }
}
```

## <a name="access-dualscreeninfo-in-your-application-window"></a>Доступ к DualScreenInfo в окне приложения

В следующем коде показано, как получить доступ к `DualScreenInfo` для окна приложения:

```csharp
DualScreenInfo currentWindow = DualScreenInfo.Current;

// Retrieve absolute position of the hinge on the screen
var hingeBounds = currentWindow.HingeBounds;

// check if app window is spanned across two screens
if(currentWindow.SpanMode == TwoPaneViewMode.SinglePane)
{
    // window is only on one screen
}
else if(currentWindow.SpanMode == TwoPaneViewMode.Tall)
{
    // window is spanned across two screens and oriented top-bottom
}
else if(currentWindow.SpanMode == TwoPaneViewMode.Wide)
{
    // window is spanned across two screens and oriented side-by-side
}

// Detect if any of the properties on DualScreenInfo change.
// This is useful to detect if the app window gets spanned
// across two screens or put on only one  
currentWindow.PropertyChanged += OnDualScreenInfoChanged;
```

## <a name="apply-dualscreeninfo-to-layouts"></a>Применение DualScreenInfo к макетам

Класс `DualScreenInfo` содержит конструктор, который может принять макет и предоставить сведения о его использовании на двух экранах устройств:

```xaml
<Grid x:Name="grid" ColumnSpacing="0">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="{Binding Column1Width}" />
        <ColumnDefinition Width="{Binding Column2Width}" />
        <ColumnDefinition Width="{Binding Column3Width}" />
    </Grid.ColumnDefinitions>
    <Label FontSize="Large"
           VerticalOptions="Center"
           HorizontalOptions="End"
           Text="I should be on the left side of the hinge" />
    <Label FontSize="Large"
           VerticalOptions="Center"
           HorizontalOptions="Start"
           Grid.Column="2"
           Text="I should be on the right side of the hinge" />
</Grid>
```

```csharp
public partial class GridUsingDualScreenInfo : ContentPage
{
    public DualScreenInfo DualScreenInfo { get; }
    public double Column1Width { get; set; }
    public double Column2Width { get; set; }
    public double Column3Width { get; set; }

    public GridUsingDualScreenInfo()
    {
        InitializeComponent();
        DualScreenInfo = new DualScreenInfo(grid);
        BindingContext = this;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        DualScreenInfo.PropertyChanged += OnInfoPropertyChanged;
        UpdateColumns();
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        DualScreenInfo.PropertyChanged -= OnInfoPropertyChanged;
    }

    void UpdateColumns()
    {
        // Check if grid is on two screens
        if (DualScreenInfo.SpanningBounds.Length > 0)
        {
            // set the width of the first column to the width of the layout
            // that's on the left screen
            Column1Width = DualScreenInfo.SpanningBounds[0].Width;

            // set the middle column to the width of the hinge
            Column2Width = DualScreenInfo.HingeBounds.Width;

            // set the width of the third column to the width of the layout
            // that's on the right screen
            Column3Width = DualScreenInfo.SpanningBounds[1].Width;
        }
        else
        {
            Column1Width = 100;
            Column2Width = 0;
            Column3Width = 100;
        }

        OnPropertyChanged(nameof(Column1Width));
        OnPropertyChanged(nameof(Column2Width));
        OnPropertyChanged(nameof(Column3Width));

    }

    void OnInfoPropertyChanged(object sender, System.ComponentModel.PropertyChangedEventArgs e)
    {
        UpdateColumns();
    }
}
```

На следующем снимке экрана показан получившийся макет:

![Размещение сетки на двух экранах](dual-screen-info-images/grid-on-two-screens.png)

## <a name="related-links"></a>Связанные ссылки

- [DualScreen (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)
