---
Title: "Быстрая прокрутка ListView" Описание: "особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Android, которая обеспечивает быструю прокрутку данных в ListView. "
MS. произв. Xamarin MS. AssetID: 37D95A2D-74AC-488A-B903-2BDD799EAA5C MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 07/10/2018 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="listview-fast-scrolling-on-android"></a>Быстрая прокрутка ListView в Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа для Android используется для быстрой прокрутки данных в [`ListView`](xref:Xamarin.Forms.ListView) . Он используется в XAML путем присвоения `ListView.IsFastScrollEnabled` свойству присоединенного свойства `boolean` значения:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        ...
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  GroupDisplayBinding="{Binding Key}"
                  IsGroupingEnabled="true"
                  android:ListView.IsFastScrollEnabled="true">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

`ListView.On<Android>`Метод указывает, что эта платформа будет запускаться только в Android. `ListView.SetIsFastScrollEnabled`Метод в [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространстве имен используется, чтобы обеспечить быструю прокрутку данных в [`ListView`](xref:Xamarin.Forms.ListView) . Кроме того, `SetIsFastScrollEnabled` метод можно использовать для переключения быстрой прокрутки путем вызова `IsFastScrollEnabled` метода для возврата, включена ли Быстрая прокрутка.

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

В результате можно включить быструю прокрутку данных в [`ListView`](xref:Xamarin.Forms.ListView) , что изменяет размер бегунка прокрутки.

[![](listview-fast-scrolling-images/fastscroll.png "ListView FastScroll Platform-Specific")](listview-fast-scrolling-images/fastscroll-large.png#lightbox "ListView FastScroll Platform-Specific")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
