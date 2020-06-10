---
Title: "Описание высоты Навигатионпаже Bar на Android" Description: "особенности платформы позволяют использовать функции, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать зависящую от платформы Android платформу, устанавливающую высоту панели навигации в Навигатионпаже.
MS. произв. Xamarin MS. AssetID: C8A73B64-FE70-408A-A72E-8AF147F0C52C MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 07/10/2018 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="navigationpage-bar-height-on-android"></a>Высота Навигатионпаже Bar на Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа Android определяет высоту панели навигации на [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) . Он используется в XAML путем установки [`NavigationPage.BarHeight`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.BarHeightProperty) Свойства BIND в целочисленное значение:

```xaml
<NavigationPage ...
                xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
                android:NavigationPage.BarHeight="450">
    ...
</NavigationPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

public class AndroidNavigationPageCS : Xamarin.Forms.NavigationPage
{
    public AndroidNavigationPageCS()
    {
        On<Android>().SetBarHeight(450);
    }
}
```

`NavigationPage.On<Android>`Метод указывает, что эта платформа будет запускаться только на платформе с поддержкой совместимости с приложением Android. [ `NavigationPage.SetBarHeight` ] (Xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. AppCompat. Навигатионпаже. Сетбархеигхт ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Навигатионпаже}, System. Int32). в [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) пространстве имен используется для задания высоты панели навигации на [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) . Кроме того, [ `NavigationPage.GetBarHeight` ] (xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. AppCompat. Навигатионпаже. Жетбархеигхт ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Навигатионпаже})). можно использовать метод, чтобы вернуть высоту панели навигации в `NavigationPage` .

В результате высота панели навигации на [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) может быть задана следующим образом:

![](navigationpage-bar-height-images/navigationpage-barheight.png "NavigationPage navigation bar height")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
