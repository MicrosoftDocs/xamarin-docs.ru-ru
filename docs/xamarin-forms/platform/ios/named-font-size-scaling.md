---
Title: "масштабирование специальных возможностей для именованных размеров шрифтов в iOS" Description: "особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая отключает масштабирование специальных возможностей для именованных размеров шрифтов.
MS. произв. Xamarin MS. AssetID: B443BAF6-E6F6-4A0E-80B5-CAACE6B550EF MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 06/28/2019 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="accessibility-scaling-for-named-font-sizes-on-ios"></a>Масштабирование специальных возможностей для именованных размеров шрифтов в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS отключает масштабирование доступа для именованных размеров шрифтов. Он используется в XAML путем установки `Application.EnableAccessibilityScalingForNamedFontSizes` Свойства BIND в значение `false` :

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.EnableAccessibilityScalingForNamedFontSizes="false">
    ...
</Application>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetEnableAccessibilityScalingForNamedFontSizes(false);
```

`Application.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `Application.SetEnableAccessibilityScalingForNamedFontSizes`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для отключения именованных размеров шрифтов, масштабируемых с помощью параметров специальных возможностей iOS. Кроме того, `Application.GetEnableAccessibilityScalingForNamedFontSizes` метод можно использовать для того, чтобы получить возможность масштабирования именованных размеров шрифтов с помощью параметров специальных возможностей iOS.

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
