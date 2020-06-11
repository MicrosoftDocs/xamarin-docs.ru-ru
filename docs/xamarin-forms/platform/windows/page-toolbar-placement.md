---
Title: "размещение панели инструментов на странице в Windows" Описание: "особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Windows, которая изменяет размещение панели инструментов на странице.
MS. произв. Xamarin MS. AssetID: 99F29E95-0C36-4A3B-BDE8-7E9F119E844E MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 10/24/2018 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="page-toolbar-placement-on-windows"></a>Размещение панели инструментов страницы в Windows

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Этот универсальная платформа Windows зависящий от платформы, используется для изменения размещения панели инструментов в [`Page`](xref:Xamarin.Forms.Page) и используется в XAML путем присвоения [`Page.ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty) свойству присоединенного свойства значения [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) перечисления:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:Page.ToolbarPlacement="Bottom">
  ...
</TabbedPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetToolbarPlacement(ToolbarPlacement.Bottom);
```

`Page.On<Windows>`Метод указывает, что эта платформа будет запускаться только в Windows. [ `Page.SetToolbarPlacement` ] (Xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Page. Сеттулбарплацемент ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Page}, Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Тулбарплацемент)). в [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространстве имен используется для задания размещения панели инструментов, при этом [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) перечисление предоставляет три значения: [`Default`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default) , [`Top`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top) и [`Bottom`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom) .

В результате заданное размещение панели инструментов применяется к [`Page`](xref:Xamarin.Forms.Page) экземпляру:

[![](page-toolbar-placement-images/toolbar-placement.png "Toolbar Placement Platform-Specific")](page-toolbar-placement-images/toolbar-placement-large.png#lightbox "Toolbar Placement Platform-Specific")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ВиндовсспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
