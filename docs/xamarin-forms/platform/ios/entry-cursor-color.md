---
Title: "Ввод цвета курсора в iOS" Описание: "особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать зависящую от платформы iOS, которая задает цвет курсора для записи. "
MS. произв. Xamarin MS. AssetID: 867D70BA-53F9-4434-8094-85D71DCECC2D MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 10/24/2018 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="entry-cursor-color-on-ios"></a>Цвет курсора ввода в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS задает цвет курсора [`Entry`](xref:Xamarin.Forms.Entry) для определенного цвета. Он используется в XAML путем установки [`Entry.CursorColor`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.CursorColorProperty) Свойства BIND в значение [`Color`](xref:Xamarin.Forms.Color) :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Entry ... ios:Entry.CursorColor="LimeGreen" />
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var entry = new Xamarin.Forms.Entry();
entry.On<iOS>().SetCursorColor(Color.LimeGreen);
```

`Entry.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. [ `Entry.SetCursorColor` ] (Xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Entry. Сеткурсорколор ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Entry}, Xamarin.Forms . Цвет). в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен в качестве цвета курсора задается указанный [`Color`](xref:Xamarin.Forms.Color) . Кроме того, [ `Entry.GetCursorColor` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Entry. Жеткурсорколор ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Entry})). для получения текущего цвета курсора можно использовать метод.

В результате цвет курсора в [`Entry`](xref:Xamarin.Forms.Entry) может быть установлен для конкретного [`Color`](xref:Xamarin.Forms.Color) :

![](entry-cursor-color-images/entry-cursorcolor.png "Entry Cursor Color")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
