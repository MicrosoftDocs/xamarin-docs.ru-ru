---
Title: "Выбор элемента DatePicker в iOS" Описание: "особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать элементы управления для платформы iOS, которые используются для выбора элементов в DatePicker. "
MS. произв. Xamarin MS. AssetID: 847E69D1-6AE0-4E82-B9C8-919E009C2014 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 01/15/2020 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="datepicker-item-selection-on-ios"></a>Выбор элемента DatePicker в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Элементы управления для конкретных платформ iOS при выборе элементов в [`DatePicker`](xref:Xamarin.Forms.DatePicker) , позволяя пользователю указать, что выбор элементов выполняется при просмотре элементов в элементе управления, или только после нажатия кнопки **done (Готово** ). Он используется в XAML путем присвоения `DatePicker.UpdateMode` свойству присоединенного свойства значения `UpdateMode` перечисления:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
       <DatePicker MinimumDate="01/01/2020"
                   MaximumDate="12/31/2020"
                   ios:DatePicker.UpdateMode="WhenFinished" />
       ...
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

datePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`DatePicker.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `DatePicker.SetUpdateMode`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для управления тем, когда происходит выбор элементов, с `UpdateMode` перечислением, предоставляющим два возможных значения:

- `Immediately`— Выбор элементов происходит при просмотре пользователем элементов в [`DatePicker`](xref:Xamarin.Forms.DatePicker) . Это поведение по умолчанию в Xamarin.Forms .
- `WhenFinished`— Выбор элементов происходит только после нажатия пользователем кнопки **done (Готово** ) в [`DatePicker`](xref:Xamarin.Forms.DatePicker) .

Кроме того, `SetUpdateMode` метод можно использовать для переключения значений перечисления путем вызова `UpdateMode` метода, который возвращает текущий объект `UpdateMode` :

```csharp
switch (datePicker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        datePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        datePicker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

В результате заданный объект `UpdateMode` применяется к элементу [`DatePicker`](xref:Xamarin.Forms.DatePicker) управления, который управляет тем, когда происходит выбор элементов:

[![Снимок экрана режимов обновления DatePicker](datepicker-selection-images/datepicker-updatemode.png "DatePicker UpdateMode для конкретной платформы")](datepicker-selection-images/datepicker-updatemode-large.png#lightbox "DatePicker UpdateMode для конкретной платформы")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
