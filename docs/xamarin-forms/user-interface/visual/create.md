---
Title: "Создание Xamarin.Forms визуального модуля подготовки отчетов" Описание: "Создание Xamarin.Forms визуальных элементов для выборочного применения к объектам висуалелемент без необходимости создания подклассов для Xamarin.Forms представлений".
MS. произв. Xamarin MS. AssetID: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 03/12/2019 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="create-a-xamarinforms-visual-renderer"></a>Создание Xamarin.Forms визуального модуля подготовки отчетов

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

Xamarin.FormsВизуальный элемент позволяет создавать модули подготовки отчетов и выборочно применять их к [`VisualElement`](xref:Xamarin.Forms.VisualElement) объектам, не прибегая к Xamarin.Forms представлениям подкласс. Модуль подготовки отчетов, указывающий `IVisual` тип (как часть его `ExportRendererAttribute` ), будет использоваться для визуализации входящих в представления, а не для модуля подготовки к просмотру по умолчанию. Во время выбора модуля визуализации `Visual` свойство представления проверяется и включается в процесс выбора модуля подготовки отчетов.

> [!IMPORTANT]
> В настоящее время [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) свойство не может быть изменено после подготовки представления, но оно будет изменено в следующем выпуске.

Процесс создания и использования Xamarin.Forms визуального модуля визуализации:

1. Создайте модули подготовки платформы для требуемого представления. Дополнительные сведения см. в разделе [Создание модулей подготовки](#create-platform-renderers)отчетов.
1. Создайте тип, производный от `IVisual` . Дополнительные сведения см. в разделе [Создание типа IVisual](#create-an-ivisual-type).
1. Зарегистрируйте `IVisual` тип как часть объекта `ExportRendererAttribute` , который будет оформлять модули подготовки отчетов. Дополнительные сведения см. [в статье регистрация типа IVisual](#register-the-ivisual-type).
1. Использовать визуальный модуль визуализации, задав [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) для свойства представления `IVisual` имя. Дополнительные сведения см. [в разделе использование визуального модуля подготовки](#consume-the-visual-renderer)отчетов.
1. используемых Зарегистрируйте имя для `IVisual` типа. Дополнительные сведения см. [в статье регистрация имени для типа IVisual](#register-a-name-for-the-ivisual-type).

## <a name="create-platform-renderers"></a>Создание отрисовщиков для платформ

Дополнительные сведения о создании класса модуля подготовки отчетов см. в разделе [пользовательские модули подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)отчетов. Однако обратите внимание, что Xamarin.Forms визуальный модуль визуализации применяется к представлению без необходимости создавать подкласс представления.

Перечисленные здесь классы модуля подготовки отчетов реализуют пользовательский интерфейс [`Button`](xref:Xamarin.Forms.Button) , который отображает текст с тенью.

### <a name="ios"></a>iOS

В следующем примере кода показано средство визуализации кнопок для iOS:

```csharp
public class CustomButtonRenderer : ButtonRenderer
{
    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        base.OnElementChanged(e);

        if (e.OldElement != null)
        {
            // Cleanup
        }

        if (e.NewElement != null)
        {
            Control.TitleShadowOffset = new CoreGraphics.CGSize(1, 1);
            Control.SetTitleShadowColor(Color.Black.ToUIColor(), UIKit.UIControlState.Normal);
        }
    }
}
```

### <a name="android"></a>Android

В следующем примере кода показано средство визуализации кнопок для Android:

```csharp
public class CustomButtonRenderer : Xamarin.Forms.Platform.Android.AppCompat.ButtonRenderer
{
    public CustomButtonRenderer(Context context) : base(context)
    {
    }

    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        base.OnElementChanged(e);

        if (e.OldElement != null)
        {
            // Cleanup
        }

        if (e.NewElement != null)
        {
            Control.SetShadowLayer(5, 3, 3, Color.Black.ToAndroid());
        }
    }
}
```

## <a name="create-an-ivisual-type"></a>Создание типа IVisual

В межплатформенной библиотеке Создайте тип, производный от `IVisual` :

```csharp
public class CustomVisual : IVisual
{
}
```

`CustomVisual`Затем тип можно зарегистрировать в классах модуля подготовки отчетов, [`Button`](xref:Xamarin.Forms.Button) позволяя объектам использовать модули подготовки отчетов.

## <a name="register-the-ivisual-type"></a>Регистрация типа IVisual

В проектах платформы добавьте на `ExportRendererAttribute` уровне сборки:

```csharp
[assembly: ExportRenderer(typeof(Xamarin.Forms.Button), typeof(CustomButtonRenderer), new[] { typeof(CustomVisual) })]
namespace VisualDemos.iOS
{
    public class CustomButtonRenderer : ButtonRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
        {
            // ...
        }
    }
}
```

В этом примере для проекта платформы iOS параметр `ExportRendererAttribute` указывает, что `CustomButtonRenderer` класс будет использоваться для визуализации используемых [`Button`](xref:Xamarin.Forms.Button) объектов с `IVisual` типом, зарегистрированным в качестве третьего аргумента. Модуль подготовки отчетов, указывающий `IVisual` тип (как часть его `ExportRendererAttribute` ), будет использоваться для визуализации входящих в представления, а не для модуля подготовки к просмотру по умолчанию.

## <a name="consume-the-visual-renderer"></a>Использование визуального модуля подготовки отчетов

[`Button`](xref:Xamarin.Forms.Button)Объект может использовать классы модуля подготовки отчетов, установив для его свойства значение [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) `Custom` :

```xaml
<Button Visual="Custom"
        Text="CUSTOM BUTTON"
        BackgroundColor="{StaticResource PrimaryColor}"
        TextColor="{StaticResource SecondaryTextColor}"
        HorizontalOptions="FillAndExpand" />
```

> [!NOTE]
> В XAML преобразователь типов устраняет необходимость включения "визуального" суффикса в [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) значение свойства. Однако можно также указать полное имя типа.

Эквивалентный код на C# выглядит так:

```csharp
Button button = new Button { Text = "CUSTOM BUTTON", ... };
button.Visual = new CustomVisual();
```

Во время выбора модуля визуализации [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) свойство объекта [`Button`](xref:Xamarin.Forms.Button) проверяется и включается в процесс выбора модуля подготовки отчетов. Если модуль подготовки отчетов не найден, Xamarin.Forms будет использоваться модуль подготовки отчетов по умолчанию.

На следующих снимках экрана показан отображаемый объект [`Button`](xref:Xamarin.Forms.Button) , который отображает текст с тенью:

[![Снимок экрана пользовательской кнопки с теневым текстом в iOS и Android](material-visual-images/custom-button.png "Кнопка с тенью текста")](material-visual-images/custom-button-large.png#lightbox)

## <a name="register-a-name-for-the-ivisual-type"></a>Регистрация имени для типа IVisual

[`VisualAttribute`](xref:Xamarin.Forms.VisualAttribute)Можно использовать для необязательной регистрации другого имени для `IVisual` типа. Этот подход можно использовать для разрешения конфликтов имен между различными визуальными библиотеками или в ситуациях, когда нужно просто ссылаться на визуальный элемент по имени, отличному от имени его типа.

[`VisualAttribute`](xref:Xamarin.Forms.VisualAttribute)Должен быть определен на уровне сборки в межплатформенной библиотеке или в проекте платформы:

```csharp
[assembly: Visual("MyVisual", typeof(CustomVisual))]
```

`IVisual`Затем тип можно использовать с помощью зарегистрированного имени:

```xaml
<Button Visual="MyVisual"
        ... />
```

> [!NOTE]
> При использовании визуального элемента с помощью его зарегистрированного имени необходимо добавить любой "визуальный" суффикс.

## <a name="related-links"></a>Связанные ссылки

- [Визуальный элемент "материал" (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)
- [Визуальный элемент материала Xamarin.Forms](material-visual.md)
- [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
