---
Title: "TimePicker выбор элементов в iOS" Description: "особенности платформы позволяют использовать функции, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать элементы управления для платформы iOS, которые используются при выборе элементов в TimePicker.
MS. произв. Xamarin MS. AssetID: 554AC877-8698-4B8C-A676-80DD64305A06 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 01/15/2020 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="timepicker-item-selection-on-ios"></a>Выбор элемента TimePicker в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Элементы управления для конкретных платформ iOS при выборе элементов в [`TimePicker`](xref:Xamarin.Forms.TimePicker) , позволяя пользователю указать, что выбор элементов выполняется при просмотре элементов в элементе управления, или только после нажатия кнопки **done (Готово** ). Он используется в XAML путем присвоения `TimePicker.UpdateMode` свойству присоединенного свойства значения `UpdateMode` перечисления:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
       <TimePicker Time="14:00:00"
                   ios:TimePicker.UpdateMode="WhenFinished" />
       ...
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

timePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`TimePicker.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `TimePicker.SetUpdateMode`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для управления тем, когда происходит выбор элементов, с `UpdateMode` перечислением, предоставляющим два возможных значения:

- `Immediately`— Выбор элементов происходит при просмотре пользователем элементов в [`TimePicker`](xref:Xamarin.Forms.TimePicker) . Это поведение по умолчанию в Xamarin.Forms .
- `WhenFinished`— Выбор элементов происходит только после нажатия пользователем кнопки **done (Готово** ) в [`TimePicker`](xref:Xamarin.Forms.TimePicker) .

Кроме того, `SetUpdateMode` метод можно использовать для переключения значений перечисления путем вызова `UpdateMode` метода, который возвращает текущий объект `UpdateMode` :

```csharp
switch (timePicker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        timePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        timePicker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

В результате заданный объект `UpdateMode` применяется к элементу [`TimePicker`](xref:Xamarin.Forms.TimePicker) управления, который управляет тем, когда происходит выбор элементов:

[![Снимок экрана: режимы обновления TimePicker](timepicker-selection-images/timepicker-updatemode.png "TimePicker UpdateMode для конкретной платформы")](timepicker-selection-images/timepicker-updatemode-large.png#lightbox "TimePicker UpdateMode для конкретной платформы")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
