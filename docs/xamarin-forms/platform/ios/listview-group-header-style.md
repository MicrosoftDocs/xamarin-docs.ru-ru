---
Title: "стиль заголовка группы ListView для iOS" Описание: "особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая определяет, будут ли ячейки заголовка ListView перемещаться во время прокрутки. "
MS. произв. Xamarin MS. AssetID: 099B2C7F-727F-4BCF-903B-87E728108C24 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 10/24/2018 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="listview-group-header-style-on-ios"></a>Стиль заголовка группы ListView в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS определяет, будут ли [`ListView`](xref:Xamarin.Forms.ListView) ячейки заголовка перемещаться во время прокрутки. Он используется в XAML путем задания `ListView.GroupHeaderStyle` свойству BIND значения `GroupHeaderStyle` перечисления.

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.GroupHeaderStyle="Grouped">
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

listView.On<iOS>().SetGroupHeaderStyle(GroupHeaderStyle.Grouped);
```

`ListView.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `ListView.SetGroupHeaderStyle`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для управления тем, будут ли [`ListView`](xref:Xamarin.Forms.ListView) ячейки заголовка перемещаться во время прокрутки. `GroupHeaderStyle`Перечисление предоставляет два возможных значения:

- `Plain`— Указывает, что ячейки заголовка имеют плавающее значение при [`ListView`](xref:Xamarin.Forms.ListView) прокрутке (по умолчанию).
- `Grouped`— Указывает, что ячейки заголовка не перемещаются при [`ListView`](xref:Xamarin.Forms.ListView) прокрутке.

Кроме того, `ListView.GetGroupHeaderStyle` метод можно использовать для возврата объекта `GroupHeaderStyle` , который применяется к [`ListView`](xref:Xamarin.Forms.ListView) .

Результатом является то, что указанное `GroupHeaderStyle` значение применяется к элементу [`ListView`](xref:Xamarin.Forms.ListView) , который определяет, будут ли ячейки заголовка перемещаться во время прокрутки:

[![Снимок экрана с плавающими и неплавающими ячейками заголовка ListView в iOS](listview-group-header-style-images/group-header-styles.png "ListView с плавающими и не плавающими ячейками заголовка")](listview-group-header-style-images/group-header-styles-large.png#lightbox "ListView с плавающими и не плавающими ячейками заголовка")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
