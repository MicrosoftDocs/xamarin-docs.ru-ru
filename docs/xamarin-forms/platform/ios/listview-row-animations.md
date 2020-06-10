---
Title: "анимация строк ListView в iOS" Описание: "особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать особенности платформы iOS, которые определяют, отключаются ли анимации строк при обновлении коллекции элементов ListView. "
MS. произв. Xamarin MS. AssetID: E8F5103F-4D8E-4A5A-A16C-7FA14EE786AC MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 02/21/2019 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="listview-row-animations-on-ios"></a>Анимация строк ListView в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS определяет, отключается ли анимация строк при [`ListView`](xref:Xamarin.Forms.ListView) обновлении коллекции Items. Он используется в XAML путем установки `ListView.RowAnimationsEnabled` Свойства BIND в значение `false` :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.RowAnimationsEnabled="false">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetRowAnimationsEnabled(false);
```

`ListView.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `ListView.SetRowAnimationsEnabled`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для управления тем, отключается ли анимация строк при [`ListView`](xref:Xamarin.Forms.ListView) обновлении коллекции Items. Кроме того, `ListView.GetRowAnimationsEnabled` метод можно использовать для возврата к отключению анимации строк в `ListView` .

> [!NOTE]
> [`ListView`](xref:Xamarin.Forms.ListView)анимация по строкам включена по умолчанию. Таким образом, анимация возникает при вставке новой строки в `ListView` .

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
