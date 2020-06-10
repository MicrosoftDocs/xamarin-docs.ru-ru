---
Title: "действия контекста ViewCell в Android" Description: "особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Android, которая включает устаревший режим действия контекста ViewCell. "
MS. произв. Xamarin MS. AssetID: 8BD71B10-5037-443F-9975-B941CE393E5A MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч MS. Дата: 09/24/2019 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="viewcell-context-actions-on-android"></a>Контекстные действия ViewCell в Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

По умолчанию с Xamarin.Forms 4,3, когда [`ViewCell`](xref:Xamarin.Forms.ViewCell) в приложении Android определяются контекстные действия для каждого элемента в [`ListView`](xref:Xamarin.Forms.ListView) , меню контекстных действий обновляется при изменении выбранного элемента `ListView` . Однако в предыдущих версиях Xamarin.Forms меню контекстные действия не обновлялось, и это поведение называется `ViewCell` устаревшим режимом. Этот устаревший режим может привести к неправильному поведению, если `ListView` компонент использует, [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) чтобы установить его `ItemTemplate` из [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) объектов, определяющих различные контекстные действия.

Эта платформа для Android включает [`ViewCell`](xref:Xamarin.Forms.ViewCell) режим предыдущих версий меню контекстных действий для обеспечения обратной совместимости, чтобы меню контекстных действий не обновлялось при изменении выбранного элемента [`ListView`](xref:Xamarin.Forms.ListView) . Он используется в XAML путем установки `ViewCell.IsContextActionsLegacyModeEnabled` Свойства BIND в значение `true` :

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ItemsSource="{Binding Items}">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell android:ViewCell.IsContextActionsLegacyModeEnabled="true">
                        <ViewCell.ContextActions>
                            <MenuItem Text="{Binding Item1Text}" />
                            <MenuItem Text="{Binding Item2Text}" />
                        </ViewCell.ContextActions>
                        <Label Text="{Binding Text}" />
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

viewCell.On<Android>().SetIsContextActionsLegacyModeEnabled(true);
```

`ViewCell.On<Android>`Метод указывает, что эта платформа будет запускаться только в Android. `ViewCell.SetIsContextActionsLegacyModeEnabled`Метод в [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространстве имен используется для включения [`ViewCell`](xref:Xamarin.Forms.ViewCell) устаревшего режима меню контекстных действий, чтобы меню контекстных действий не обновлялось при изменении выбранного элемента [`ListView`](xref:Xamarin.Forms.ListView) . Кроме того, `ViewCell.GetIsContextActionsLegacyModeEnabled` метод можно использовать, чтобы возвращать, включен ли режим прежних контекстных действий.

На следующих снимках экрана [`ViewCell`](xref:Xamarin.Forms.ViewCell) включен режим устаревших действий контекста:

![Снимок экрана: включен устаревший режим ViewCell на Android](viewcell-context-actions-images/legacy-mode-enabled.png "Включен устаревший режим ViewCell")

В этом режиме отображаемые элементы меню контекстных действий идентичны для ячейки 1 и ячейки 2, несмотря на то, что для ячейки 2 определены разные элементы контекстного меню.

На следующих снимках экрана показано [`ViewCell`](xref:Xamarin.Forms.ViewCell) действие контекстного режима "отключено", что является поведением по умолчанию Xamarin.Forms :

![Снимок экрана: устаревший режим ViewCell отключен на Android](viewcell-context-actions-images/legacy-mode-disabled.png "Устаревший режим ViewCell отключен")

В этом режиме для ячейки 1 и ячейки 2 отображаются правильные элементы контекстного меню действий.

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
