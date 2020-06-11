---
Title: "ListView SelectionMode в Windows" Description: "особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Windows, которая определяет, могут ли элементы в ListView реагировать на жесты касания.
MS. произв. Xamarin MS. AssetID: 57EF3A7F-1407-4B31-AE21-D149293D4228 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 10/24/2018 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="listview-selectionmode-on-windows"></a>ListView SelectionMode в Windows

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

На универсальная платформа Windows по умолчанию Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) использует собственное `ItemClick` событие для реагирования на взаимодействие, а не на собственное `Tapped` событие. Это обеспечивает специальные возможности, чтобы экранный диктор и клавиатура Windows могли взаимодействовать с `ListView` . Однако он также визуализирует любые жесты касания в `ListView` неработоспособном виде.

Это универсальная платформа Windows зависящие от платформы элементы управления, могут ли элементы в элементе, [`ListView`](xref:Xamarin.Forms.ListView) реагировать на жесты касания, и, следовательно, инициирует ли машинное `ListView` `ItemClick` `Tapped` событие или. Он используется в XAML путем присвоения [`ListView.SelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty) свойству присоединенного свойства значения [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) перечисления:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <ListView ... windows:ListView.SelectionMode="Inaccessible">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

`ListView.On<Windows>`Метод указывает, что данная платформа будет запускаться только на универсальная платформа Windows. [ `ListView.SetSelectionMode` ] (Xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. ListView. Сетселектионмоде ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . ListView}, Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Листвиевселектионмоде)). в [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространстве имен используется для контроля над тем, могут ли элементы в элементе управления [`ListView`](xref:Xamarin.Forms.ListView) реагировать на жесты касания, с [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) перечислением, предоставляющим два возможных значения:

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible)— Указывает, что `ListView` компонент запустит собственное `ItemClick` событие для обработки взаимодействия и, таким образом, предоставит специальные возможности. Таким образом, Экранный диктор и клавиатура Windows могут взаимодействовать с `ListView` . Однако элементы в `ListView` не могут реагировать на жесты касания. Это поведение по умолчанию для `ListView` экземпляров в универсальная платформа Windows.
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible)— Указывает, что `ListView` будет срабатывать собственное `Tapped` событие для работы с взаимодействием. Поэтому элементы в `ListView` могут реагировать на жесты касания. Однако нет специальных возможностей, поэтому экранный диктор Windows и клавиатура не могут взаимодействовать с компонентом `ListView` .

> [!NOTE]
> `Accessible` `Inaccessible` Режимы выбора и являются взаимоисключающими, и необходимо выбрать доступ [`ListView`](xref:Xamarin.Forms.ListView) или `ListView` , который может реагировать на жесты касания.

Кроме того, [ `GetSelectionMode` ] (xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. ListView. Жетселектионмоде ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . ListView})). можно использовать метод для возврата текущего [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) .

В результате заданный объект [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) применяется к элементу [`ListView`](xref:Xamarin.Forms.ListView) , который определяет, могут ли элементы в элементе управления `ListView` реагировать на жесты касания, и, следовательно, активирует ли машинное `ListView` `ItemClick` `Tapped` событие или.

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ВиндовсспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
