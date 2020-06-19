---
title: Реагирование на изменения системных тем в Xamarin.Forms приложениях
description: Xamarin.Formsприложения могут реагировать на изменения темы операционной системы с помощью типа Онаппсеме и расширения разметки DynamicResource.
ms.assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D
ms.prod: xamarin
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/17/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 86ad823466470033c458ad44a404e8ab667c1b95
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84903081"
---
# <a name="respond-to-system-theme-changes-in-xamarinforms-applications"></a>Реагирование на изменения системных тем в Xamarin.Forms приложениях

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)

Устройства обычно включают в себя светло-и темные темы, каждый из которых ссылается на широкий набор предпочтений внешнего вида, который можно задать на уровне операционной системы. Приложения должны учитывать эти системные темы и отвечать сразу после изменения темы системы.

Тема системы может измениться по ряду причин, в зависимости от конфигурации устройства. К ним относится пользовательская тема, измененная пользователем, она изменяется из-за времени суток и изменяется из-за таких факторов окружающей среды, как низкая интенсивность.

Xamarin.Formsприложения могут реагировать на изменения темы системы за счет использования ресурсов с `AppThemeBinding` расширением разметки, а также `SetAppThemeColor` `SetOnAppTheme<T>` методов расширения и.

> [!IMPORTANT]
> Реагирование на изменение системной темы в настоящее время экспериментально и может использоваться только путем установки `AppTheme_Experimental` флага. Дополнительные сведения см. в разделе [экспериментальные флаги](~/xamarin-forms/internals/experimental-flags.md).

Для Xamarin.Forms реагирования на изменение системной темы должны быть выполнены следующие требования.

- Xamarin.Forms4.6.0.967 или выше.
- iOS 13 или более поздняя.
- Android 10 (API 29) или более поздней версии.
- UWP сборки 14393 или более поздней версии.

На следующих снимках экрана показаны страницы с темами для светлых и темных системных тем в iOS и Android:

[![Снимок экрана: Главная страница приложения с темой, в iOS и Android](system-theme-changes-images/main-page-both-themes.png "Главная страница приложения с темой")](system-theme-changes-images/main-page-both-themes-large.png#lightbox "Главная страница приложения с темой") 
 [ ![Снимок экрана со страницей сведений о приложении с темой в iOS и Android](system-theme-changes-images/detail-page-both-themes.png "Страница сведений о приложении с темой")](system-theme-changes-images/detail-page-both-themes-large.png#lightbox "Страница сведений о приложении с темой")

## <a name="define-and-consume-theme-resources"></a>Определение и использование ресурсов тем

Ресурсы для светлой и темной темы можно использовать с `AppThemeBinding` расширением разметки, а также с помощью `SetAppThemeColor` `SetOnAppTheme<T>` методов расширения и. При использовании этих подходов ресурсы автоматически применяются в зависимости от значения текущей системной темы. Кроме того, объекты, использующие эти ресурсы, автоматически обновляются при изменении темы системы во время работы приложения.

### <a name="appthemebinding-markup-extension"></a>Расширение разметки Аппсемебиндинг

`AppThemeBinding`Расширение разметки позволяет использовать ресурс, например изображение или цвет, на основе текущей системной темы:

```xaml
<ContentPage ...>
    <StackLayout Margin="20">
        <Label Text="This text is green in light mode, and red in dark mode."
               TextColor="{AppThemeBinding Light=Green, Dark=Red}" />
        <Image Source="{AppThemeBinding Light=lightlogo.png, Dark=darklogo.png}" />
    </StackLayout>
</ContentPage>
```

В этом примере цвет текста первого объекта задается [`Label`](xref:Xamarin.Forms.Label) зеленым цветом, когда устройство использует его светлую тему, и задается красным, если устройство использует его темную тему. Аналогичным образом, [`Image`](xref:Xamarin.Forms.Image) отображает другой файл изображения на основе текущей системной темы.

Кроме того, ресурсы, определенные в, [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) можно использовать с `StaticResource` расширением разметки:

```xaml
<ContentPage ...>
    <ContentPage.Resources>

        <!-- Light colors -->
        <Color x:Key="LightPrimaryColor">WhiteSmoke</Color>
        <Color x:Key="LightSecondaryColor">Black</Color>

        <!-- Dark colors -->
        <Color x:Key="DarkPrimaryColor">Teal</Color>
        <Color x:Key="DarkSecondaryColor">White</Color>

        <Style x:Key="ButtonStyle"
               TargetType="Button">
            <Setter Property="BackgroundColor"
                    Value="{AppThemeBinding Light={StaticResource LightPrimaryColor}, Dark={StaticResource DarkPrimaryColor}}" />
            <Setter Property="TextColor"
                    Value="{AppThemeBinding Light={StaticResource LightSecondaryColor}, Dark={StaticResource DarkSecondaryColor}}" />
        </Style>

    </ContentPage.Resources>

    <Grid BackgroundColor="{AppThemeBinding Light={StaticResource LightPrimaryColor}, Dark={StaticResource DarkPrimaryColor}}">
      <Button Text="MORE INFO"
              Style="{StaticResource ButtonStyle}" />
    </Grid>    
</ContentPage>    
```

В этом примере цвет фона [`Grid`](xref:Xamarin.Forms.Grid) и [`Button`](xref:Xamarin.Forms.Button) стиля изменяется в зависимости от того, используется ли на устройстве светлая или темная тема.

Дополнительные сведения о `AppThemeBinding` расширении разметки см. в разделе [расширение разметки аппсемебиндинг](~/xamarin-forms/xaml/markup-extensions/consuming.md#appthemebinding-markup-extension).

### <a name="extension-methods"></a>Методы расширения

Xamarin.Formsвключает `SetAppThemeColor` и `SetOnAppTheme<T>` методы расширения, позволяющие [`VisualElement`](xref:Xamarin.Forms.VisualElement) объектам реагировать на изменения темы системы.

`SetAppThemeColor`Метод позволяет [`Color`](xref:Xamarin.Forms.Color) указать объекты, которые будут установлены для целевого свойства на основе текущей системной темы:

```csharp
Label label = new Label();
label.SetAppThemeColor(Label.TextColorProperty, Color.Green, Color.Red);
```

В этом примере цвет текста для [`Label`](xref:Xamarin.Forms.Label) имеет значение зеленый, если устройство использует светло-тему, а для устройства используется темная тема.

`SetOnAppTheme<T>`Метод позволяет указать объекты типа `T` , которые будут установлены для целевого свойства на основе текущей системной темы:

```csharp
Image image = new Image();
image.SetOnAppTheme<FileImageSource>(Image.SourceProperty, "lightlogo.png", "darklogo.png");
```

В этом примере отображается, [`Image`](xref:Xamarin.Forms.Image) `lightlogo.png` когда устройство использует светло-тему и `darklogo.png` когда устройство использует его темную тему.

## <a name="detect-the-current-system-theme"></a>Обнаружение текущей системной темы

Текущая тема системы может быть обнаружена путем получения значения `Application.RequestedTheme` Свойства:

```csharp
OSAppTheme currentTheme = Application.Current.RequestedTheme;
```

`RequestedTheme`Свойство возвращает `OSAppTheme` элемент перечисления. Перечисление `OSAppTheme` определяет следующие члены:

- `Unspecified`, что означает, что устройство использует неуказанную тему.
- `Light`, что означает, что устройство использует светло-тему.
- `Dark`, что означает, что устройство использует его темную тему.

## <a name="set-the-current-user-theme"></a>Задание текущей темы пользователя

Тема, используемая приложением, может быть задана с помощью `Application.UserTheme` свойства, имеющего тип `OSAppTheme` :

```csharp
Application.Current.UserAppTheme = OSAppTheme.Dark;
```

В этом примере приложение настроено для использования темы, определенной для темного режима системы.

## <a name="react-to-theme-changes"></a>Реагирование на изменения темы

Тема системы на устройстве может измениться по ряду причин в зависимости от настройки устройства. Xamarin.Formsприложения могут получать уведомления при изменении темы системы, обрабатывая `Application.RequestedThemeChanged` событие:

```csharp
Application.Current.RequestedThemeChanged += (s, a) =>
{
    // Respond to the theme change
};
```

`AppThemeChangedEventArgs`Объект, который прилагается к `RequestedThemeChanged` событию, имеет одно свойство с именем `RequestedTheme` типа `OSAppTheme` . Это свойство можно проверить для обнаружения требуемой системной темы.

## <a name="related-links"></a>Связанные ссылки

- [Системсемес (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)
- [Расширение разметки Аппсемебиндинг](~/xamarin-forms/xaml/markup-extensions/consuming.md#appthemebinding-markup-extension)
- [Словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Задание стиля приложений Xamarin.Forms с помощью стилей XAML](~/xamarin-forms/user-interface/styles/xaml/index.md)
