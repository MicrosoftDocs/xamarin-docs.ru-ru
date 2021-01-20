---
title: Ориентация устройства
description: В этой статье объясняется, как разметка Xamarin.Forms приложений, которые выглядят великолепно в книжной и альбомной ориентации.
ms.prod: xamarin
ms.assetid: 11A1D327-2DF3-4F3B-810D-6C95B71D27B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 955cb8371e6363f617738dcf2fe4a6eb27dcb032
ms.sourcegitcommit: 63029dd7ea4edb707a53ea936ddbee684a926204
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2021
ms.locfileid: "98608862"
---
# <a name="device-orientation"></a>Ориентация устройства

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-responsivelayout)

Важно учитывать, как будет использоваться ваше приложение и как можно встроить альбомную ориентацию, чтобы улучшить взаимодействие с пользователем. Отдельные макеты можно разрабатывать для поддержки нескольких ориентаций и лучше использовать доступное пространство. На уровне приложения поворот может быть отключен или включен.

## <a name="controlling-orientation"></a>Управление ориентацией

При использовании Xamarin.Forms поддерживаемым методом управления ориентацией устройства является использование параметров каждого отдельного проекта.

### <a name="ios"></a>iOS

В iOS Ориентация устройства настраивается для приложений, использующих файл **info. plist** . Используйте параметры интегрированной среды разработки в верхней части этого документа, чтобы выбрать, какие инструкции вы хотите увидеть:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

В Visual Studio откройте проект iOS и откройте **info. plist**. Файл откроется на панели конфигурации, начиная со вкладки сведения о развертывании iPhone:

![Сведения о развертывании iPhone в Visual Studio](device-orientation-images/orientation-vs.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

В Visual Studio для Mac откройте проект iOS и откройте **info. plist**. На вкладке **приложение** будут доступны разделы для установки ориентации:

![Сведения о развертывании iPhone в Visual Studio для Mac](device-orientation-images/orientation-vsmac.png)

Если вы предпочитаете изменить значения с помощью интерфейса редактора "ключ — значение", выберите вкладку **исходный**> в нижней части экрана:

![Поддерживаемые ориентации устройств в Visual Studio для Mac](device-orientation-images/orientation-source-vsmac.png)

-----

### <a name="android"></a>Android

Чтобы управлять ориентацией на Android, откройте **MainActivity.CS** и задайте ориентацию с помощью атрибута, определяющего `MainActivity` класс:

```csharp
namespace MyRotatingApp.Droid
{
    [Activity (Label = "MyRotatingApp.Droid", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation, ScreenOrientation = ScreenOrientation.Landscape)] //This is what controls orientation
    public class MainActivity : FormsAppCompatActivity
    {
        protected override void OnCreate (Bundle bundle)
...
```

Xamarin. Android поддерживает несколько параметров для указания ориентации:

- **Альбомная ориентация** &ndash; заставляет приложение располагаться в альбомной ориентации независимо от данных датчика.
- **Книжная ориентация** &ndash; заставляет приложение располагаться в книжной ориентации независимо от данных датчика.
- **Пользователь** &ndash; приводит к тому, что приложение будет представлено с использованием предпочтительной ориентации пользователя.
- **Позади** &ndash; Указывает, что ориентация приложения совпадает с ориентацией [действия](xref:Android.App.Activity) .
- **Датчик** &ndash; приводит к тому, что ориентация приложения определяется датчиком, даже если пользователь отключил автоматический поворот.
- **Сенсорландскапе** &ndash; заставляет приложение использовать альбомную ориентацию при использовании данных датчика для изменения направления экрана (чтобы экран не отображался в виде переворачивания).
- **Сенсорпортраит** &ndash; заставляет приложение использовать книжную ориентацию при использовании данных датчика для изменения направления экрана (чтобы экран не отображался в виде переворачивания).
- **Реверселандскапе** &ndash; заставляет приложение использовать альбомную ориентацию, направленную в противоположное направление от обычного, чтобы она отображалась «сверху вниз».
- **Реверсепортраит** &ndash; заставляет приложение использовать книжную ориентацию, направленную на противоположное направление от обычного, чтобы она отображалась как "перевернутая".
- **Фуллсенсор** &ndash; заставляет приложение полагаться на данные датчика, чтобы выбрать правильную ориентацию (из возможных 4).
- **Фуллусер** &ndash; заставляет приложение использовать предпочтения ориентации пользователя. Если включен автоматический поворот, можно использовать все 4 ориентации.
- **Усерландскапе** &ndash; _\[ Не поддерживается \]_ , если в приложении используется альбомная ориентация, если пользователь не включил автоматический поворот, в этом случае он будет использовать датчик для определения ориентации. Этот параметр приведет к нарушению компиляции.
- **Усерпортраит** &ndash; _\[ Не поддерживается \]_ , если в приложении используется книжная ориентация, если только пользователь не включил автоматический поворот, в этом случае он будет использовать датчик для определения ориентации. Этот параметр приведет к нарушению компиляции.
- **Заблокировано** &ndash; _\[ Не поддерживается \]_ указывает, что приложение будет использовать ориентацию экрана, независимо от того, где он запускается, без реагирования на изменения физической ориентации устройства. Этот параметр приведет к нарушению компиляции.

Обратите внимание, что собственные API-интерфейсы Android обеспечивают множество возможностей управления ориентацией, в том числе параметры, явно противоречащие параметрам, заданным пользователем.

### <a name="universal-windows-platform"></a>Универсальная платформа Windows

В универсальная платформа Windows (UWP) поддерживаемые ориентации задаются в файле **Package. appxmanifest** . При открытии манифеста отображается панель конфигурации, где можно выбрать поддерживаемые ориентации.

## <a name="reacting-to-changes-in-orientation"></a>Реагирование на изменения ориентации

Xamarin.Forms не предоставляет собственные события для уведомления приложения об изменениях ориентации в общем коде. Однако [Xamarin.Essentials](~/essentials/index.md) содержит `DeviceDisplay` класс [], который предоставляет уведомления об изменении ориентации.

Чтобы обнаружить ориентации без Xamarin.Essentials , следите за `SizeChanged` событием `Page` , которое срабатывает при изменении ширины или высоты `Page` изменений. Если ширина превышает `Page` высоту, устройство находится в альбомном режиме. Дополнительные сведения см. в разделе [Отображение изображения на основе ориентации экрана](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation).

Кроме того, можно переопределить [`OnSizeAllocated`](xref:Xamarin.Forms.Page.OnSizeAllocated*) метод в `Page` , вставив в него любую логику изменения макета. `OnSizeAllocated`Метод вызывается каждый раз `Page` , когда выделяется новый размер, который происходит при каждом повороте устройства. Обратите внимание, что базовая реализация `OnSizeAllocated` выполняет важные функции макета, поэтому важно вызвать базовую реализацию в переопределении:

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
}
```

Если не выполнить этот шаг, это приведет к неработающей странице.

Обратите внимание, что `OnSizeAllocated` метод может вызываться много раз при вращении устройства. Каждый раз при изменении макета используется непроизводительна ресурсов, что может привести к мерцанию. Рекомендуется использовать переменную экземпляра на странице, чтобы определить ориентацию ориентации: альбомная или Книжная и только перерисовка при изменении.

```csharp
private double width = 0;
private double height = 0;

protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
    if (this.width != width || this.height != height)
    {
        this.width = width;
        this.height = height;
        //reconfigure layout
    }
}
```

После обнаружения изменения ориентации устройства может потребоваться добавить или удалить дополнительные представления в пользовательском интерфейсе, чтобы реагировать на изменения в доступном пространстве. Например, рассмотрим встроенный калькулятор для каждой платформы в книжной ориентации:

![Приложение калькулятора в книжной ориентации](device-orientation-images/calculator-portrait.png)

и альбомная ориентация:

![Приложение калькулятора в альбомной ориентации](device-orientation-images/calculator-landscape.png)

Обратите внимание, что приложения используют преимущества доступного пространства путем добавления дополнительных функций в альбомной ориентации.

## <a name="responsive-layout"></a>Макет с откликом

Можно разработать интерфейсы с помощью встроенных макетов, чтобы они были корректно перенесены при вращении устройства. При проектировании интерфейсов, которые останутся привлекательными при ответе на изменения ориентации, учитывайте следующие общие правила.

- **Обратить внимание на соотношение** &ndash; изменение ориентации может вызвать проблемы при определенных допущениях относительно соотношения. Например, представление, для которого в 1/3 вертикального пространства экрана в книжной ориентации может не уместиться в 1/3 по вертикали.
- **Будьте внимательны с абсолютными значениями** &ndash; абсолютные (пиксельные) значения, имеющие смысл в книжной ориентации, могут не иметь смысла в альбомной ориентации. Если требуются абсолютные значения, используйте вложенные макеты, чтобы изолировать их влияние. Например, разумно использовать абсолютные значения в, `TableView` `ItemTemplate` Если шаблон элемента имеет гарантированно одинаковую высоту.

Приведенные выше правила также применяются при реализации интерфейсов для нескольких размеров экрана и обычно считаются лучшим подходом. Оставшаяся часть этого руководств поясняет конкретные примеры откликов, используя каждый из основных макетов в Xamarin.Forms .

> [!NOTE]
> Для ясности в следующих разделах показано, как реализовать макеты с быстрым реагированием, используя только один тип `Layout` за раз. На практике часто бывает проще смешивать `Layout` s для достижения желаемого макета, используя более простой или интуитивно понятный `Layout` для каждого компонента.

### <a name="stacklayout"></a>StackLayout

Рассмотрим следующее приложение, отображаемое в книжной ориентации:

![На снимке экрана показано приложение Photo StackLayout в книжной ориентации.](device-orientation-images/photo-stack-portrait.png)

и альбомная ориентация:

![На снимке экрана показано приложение Photo StackLayout в альбомной ориентации.](device-orientation-images/photo-stack-landscape.png)

Это выполняется с помощью следующего кода XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.StackLayoutPageXaml"
Title="Stack Photo Editor - XAML">
    <ContentPage.Content>
        <StackLayout Spacing="10" Padding="5" Orientation="Vertical"
        x:Name="outerStack"> <!-- can change orientation to make responsive -->
            <ScrollView>
                <StackLayout Spacing="5" HorizontalOptions="FillAndExpand"
                    WidthRequest="1000">
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Name: " WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="deer.jpg"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Date: " WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="07/05/2015"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Tags:" WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="deer, tiger"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Button Text="Save" HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                </StackLayout>
            </ScrollView>
            <Image  Source="deer.jpg" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Для изменения ориентации `outerStack` на основе ориентации устройства используется несколько языков C#:

```csharp
protected override void OnSizeAllocated (double width, double height){
    base.OnSizeAllocated (width, height);
    if (width != this.width || height != this.height) {
        this.width = width;
        this.height = height;
        if (width > height) {
            outerStack.Orientation = StackOrientation.Horizontal;
        } else {
            outerStack.Orientation = StackOrientation.Vertical;
        }
    }
}
```

Следует отметить следующее.

- `outerStack` настраивается для представления изображения и элементов управления в виде горизонтального или вертикального стека в зависимости от ориентации, что позволяет лучше использовать доступное пространство.

### <a name="absolutelayout"></a>AbsoluteLayout

Рассмотрим следующее приложение, отображаемое в книжной ориентации:

![На снимке экрана показано приложение Photo Абсолутелайаут в книжной ориентации.](device-orientation-images/photo-abs-portrait.png)

и альбомная ориентация:

![На снимке экрана показано приложение Photo Абсолутелайаут в альбомной ориентации.](device-orientation-images/photo-abs-landscape.png)

Это выполняется с помощью следующего кода XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.AbsoluteLayoutPageXaml"
Title="AbsoluteLayout - XAML" BackgroundImageSource="deer.jpg">
    <ContentPage.Content>
        <AbsoluteLayout>
            <ScrollView AbsoluteLayout.LayoutBounds="0,0,1,1"
                AbsoluteLayout.LayoutFlags="PositionProportional,SizeProportional">
                <AbsoluteLayout>
                    <Image Source="deer.jpg"
                        AbsoluteLayout.LayoutBounds=".5,0,300,300"
                        AbsoluteLayout.LayoutFlags="PositionProportional" />
                    <BoxView Color="#CC1A7019" AbsoluteLayout.LayoutBounds=".5
                        300,.7,50" AbsoluteLayout.LayoutFlags="XProportional
                        WidthProportional" />
                    <Label Text="deer.jpg" AbsoluteLayout.LayoutBounds = ".5
                        310,1, 50" AbsoluteLayout.LayoutFlags="XProportional
                        WidthProportional" HorizontalTextAlignment="Center" TextColor="White" />
                </AbsoluteLayout>
            </ScrollView>
            <Button Text="Previous" AbsoluteLayout.LayoutBounds="0,1,.5,60"
                AbsoluteLayout.LayoutFlags="PositionProportional
                    WidthProportional"
                BackgroundColor="White" TextColor="Green" BorderRadius="0" />
            <Button Text="Next" AbsoluteLayout.LayoutBounds="1,1,.5,60"
                AbsoluteLayout.LayoutFlags="PositionProportional
                    WidthProportional" BackgroundColor="White"
                    TextColor="Green" BorderRadius="0" />
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

Следует отметить следующее.

- Из-за способа размещения страницы не требуется выполнять процедурный код, чтобы реализовать скорость реагирования.
- Используется `ScrollView` для того, чтобы метка была видимой, даже если высота экрана меньше, чем сумма фиксированной высоты кнопок и изображения.

### <a name="relativelayout"></a>RelativeLayout

Рассмотрим следующее приложение, отображаемое в книжной ориентации:

![На снимке экрана показано приложение Photo RelativeLayout в книжной ориентации.](device-orientation-images/photo-rel-portrait.png)

и альбомная ориентация:

![На снимке экрана показано приложение Photo RelativeLayout в альбомной ориентации.](device-orientation-images/photo-rel-landscape.png)

Это выполняется с помощью следующего кода XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.RelativeLayoutPageXaml"
Title="RelativeLayout - XAML"
BackgroundImageSource="deer.jpg">
    <ContentPage.Content>
        <RelativeLayout x:Name="outerLayout">
            <BoxView BackgroundColor="#AA1A7019"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=1}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=0,Constant=0}" />
            <ScrollView
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=1}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=0,Constant=0}">
                <RelativeLayout>
                    <Image Source="deer.jpg" x:Name="imageDeer"
                        RelativeLayout.WidthConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=.8}"
                        RelativeLayout.XConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=.1}"
                        RelativeLayout.YConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Height,Factor=0,Constant=10}" />
                    <Label Text="deer.jpg" HorizontalTextAlignment="Center"
                        RelativeLayout.WidthConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=1}"
                        RelativeLayout.HeightConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Height,Factor=0,Constant=75}"
                        RelativeLayout.XConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                        RelativeLayout.YConstraint="{ConstraintExpression
                            Type=RelativeToView,ElementName=imageDeer,Property=Height,Factor=1,Constant=20}" />
                </RelativeLayout>

            </ScrollView>

            <Button Text="Previous" BackgroundColor="White" TextColor="Green" BorderRadius="0"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=60}"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                 />
            <Button Text="Next" BackgroundColor="White" TextColor="Green" BorderRadius="0"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=60}"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                />
        </RelativeLayout>
    </ContentPage.Content>
</ContentPage>

```

Следует отметить следующее.

- Из-за способа размещения страницы не требуется выполнять процедурный код, чтобы реализовать скорость реагирования.
- Используется `ScrollView` для того, чтобы метка была видимой, даже если высота экрана меньше, чем сумма фиксированной высоты кнопок и изображения.

### <a name="grid"></a>Макет Grid

Рассмотрим следующее приложение, отображаемое в книжной ориентации:

![На снимке экрана показана книжная сетка приложения.](device-orientation-images/photo-grid-portrait.png)

и альбомная ориентация:

![На снимке экрана показана альбомная сетка приложения.](device-orientation-images/photo-grid-landscape.png)

Это выполняется с помощью следующего кода XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.GridPageXaml"
Title="Grid - XAML">
    <ContentPage.Content>
        <Grid x:Name="outerGrid">
            <Grid.RowDefinitions>
                <RowDefinition Height="*" />
                <RowDefinition Height="60" />
            </Grid.RowDefinitions>
            <Grid x:Name="innerGrid" Grid.Row="0" Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="*" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Image Source="deer.jpg" Grid.Row="0" Grid.Column="0" HeightRequest="300" WidthRequest="300" />
                <Grid x:Name="controlsGrid" Grid.Row="0" Grid.Column="1" >
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="*" />
                    </Grid.ColumnDefinitions>
                    <Label Text="Name:" Grid.Row="0" Grid.Column="0" />
                    <Label Text="Date:" Grid.Row="1" Grid.Column="0" />
                    <Label Text="Tags:" Grid.Row="2" Grid.Column="0" />
                    <Entry Grid.Row="0" Grid.Column="1" />
                    <Entry Grid.Row="1" Grid.Column="1" />
                    <Entry Grid.Row="2" Grid.Column="1" />
                </Grid>
            </Grid>
            <Grid x:Name="buttonsGrid" Grid.Row="1">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Previous" Grid.Column="0" />
                <Button Text="Save" Grid.Column="1" />
                <Button Text="Next" Grid.Column="2" />
            </Grid>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

А также приведенный ниже процедурный код для управления изменениями вращения.

```csharp
private double width;
private double height;

protected override void OnSizeAllocated (double width, double height){
    base.OnSizeAllocated (width, height);
    if (width != this.width || height != this.height) {
        this.width = width;
        this.height = height;
        if (width > height) {
            innerGrid.RowDefinitions.Clear();
            innerGrid.ColumnDefinitions.Clear ();
            innerGrid.RowDefinitions.Add (new RowDefinition{ Height = new GridLength (1, GridUnitType.Star) });
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.Children.Remove (controlsGrid);
            innerGrid.Children.Add (controlsGrid, 1, 0);
        } else {
            innerGrid.RowDefinitions.Clear();
            innerGrid.ColumnDefinitions.Clear ();
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition{ Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Auto) });
            innerGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
            innerGrid.Children.Remove (controlsGrid);
            innerGrid.Children.Add (controlsGrid, 0, 1);
        }
    }
}
```

Следует отметить следующее.

- Из-за способа размещения страницы существует метод изменения расположения элементов управления в сетке.

## <a name="related-links"></a>Связанные ссылки

- [Макет (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [Пример Бусинесстумбле (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
- [Макет с откликом (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-responsivelayout)
- [Отображение изображения на основе ориентации экрана](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation)