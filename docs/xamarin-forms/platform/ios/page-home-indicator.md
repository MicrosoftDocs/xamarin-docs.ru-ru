---
Title: "видимость индикатора" в iOS "Описание:" особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая задает видимость индикатора Home на странице.
MS. произв. Xamarin MS. AssetID: F81022E0-3C6C-49C0-A000-FAF6574D3FB7 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 05/09/2019 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="home-indicator-visibility-on-ios"></a>Видимость индикатора дома в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS задает видимость индикатора Home на [`Page`](xref:Xamarin.Forms.Page) . Он используется в XAML путем установки [`Page.PrefersHomeIndicatorAutoHidden`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.PrefersHomeIndicatorAutoHiddenProperty) Свойства BIND в значение `boolean` :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersHomeIndicatorAutoHidden="true">
    ...
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersHomeIndicatorAutoHidden(true);
```

`Page.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. [ `Page.SetPrefersHomeIndicatorAutoHidden` ] (Xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Page. Сетпрефершомеиндикатораутохидден ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Page}, System. Boolean)) в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен управляет видимостью индикатора "начало". Кроме того, [ `Page.PrefersHomeIndicatorAutoHidden` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Page. Префершомеиндикатораутохидден ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Page})). можно использовать метод, чтобы получить видимость индикатора "начало".

В результате можно контролировать видимость индикатора Home на элементе [`Page`](xref:Xamarin.Forms.Page) управления:

![Снимок экрана с отображением индикатора "домой" на странице iOS](page-home-indicator-images/home-indicator-visibility.png "Видимость индикатора домашней страницы")

> [!NOTE]
> Эта конкретная платформа может применяться к [`ContentPage`](xref:Xamarin.Forms.ContentPage) объектам, [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) , [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) и [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) .

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
