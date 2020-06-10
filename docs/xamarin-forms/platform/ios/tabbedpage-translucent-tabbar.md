---
Title: "Таббедпаже полупрозрачная панель вкладок в" Описание "для iOS:" особенности платформы позволяют использовать функции, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать зависящую от платформы iOS, которая устанавливает режим полупрозрачность для панели вкладок в Таббедпаже ".
MS. произв. Xamarin MS. AssetID: 9581AE81-9557-47AD-8B07-25A1AC5F055B MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 01/16/2020 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="tabbedpage-translucent-tab-bar-on-ios"></a>Таббедпаже полупрозрачная панель вкладок в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS используется для установки режима полупрозрачность для панели вкладок в [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . Он используется в XAML путем установки `TabbedPage.TranslucencyMode` Свойства BIND в `TranslucencyMode` значение перечисления:

```xaml
<TabbedPage ...
            xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
            ios:TabbedPage.TranslucencyMode="Opaque">
    ...
</TabbedPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetTranslucencyMode(TranslucencyMode.Opaque);
```

`TabbedPage.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `TabbedPage.SetTranslucencyMode`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для установки режима полупрозрачность панели вкладок на, [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) указывая одно из следующих `TranslucencyMode` значений перечисления:

- `Default`, который задает для панели вкладок режим полупрозрачность по умолчанию. Это значение по умолчанию для свойства `TabbedPage.TranslucencyMode`.
- `Translucent`, который устанавливает прозрачность панели вкладок.
- `Opaque`, который задает непрозрачность панели вкладок.

Кроме того, `GetTranslucencyMode` метод можно использовать для получения текущего значения `TranslucencyMode` перечисления, применяемого к [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) .

Результат заключается в том, что режим полупрозрачность панели вкладок в [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) может быть установлен следующим образом:

![Снимок экрана с полупрозрачными и непрозрачными панелями табуляции в iOS](tabbedpage-translucent-tabbar-images/translucencymodes.png "Полупрозрачные и непрозрачные панели вкладок")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
