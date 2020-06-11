---
Title: "присоединенные свойства" Description: "в этой статье приводятся общие сведения о вложенных свойствах и демонстрируется их создание и использование".
MS. произв. Xamarin MS. AssetID: 6E9DCDC3-A0E4-46A6-BAA9-4FEB6DF8A5A8 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 06/02/2016 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="attached-properties"></a>Присоединенные свойства

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffect)


Присоединенные свойства позволяют объекту назначить значение для свойства, которое не определено его собственным классом. Например, дочерние элементы могут использовать присоединенные свойства для информирования своего родительского элемента о том, как они представлены в пользовательском интерфейсе. [`Grid`](xref:Xamarin.Forms.Grid)Элемент управления позволяет указать строку и столбец дочернего элемента, задав `Grid.Row` `Grid.Column` Свойства и. `Grid.Row`и `Grid.Column` являются присоединенными свойствами, так как они задаются для элементов, являющихся дочерними элементами объекта `Grid` , а не `Grid` самого самого.

Привязываемые свойства должны быть реализованы в виде вложенных свойств в следующих сценариях:

- Если есть необходимость иметь механизм настройки свойства для классов, отличных от определяющего класса.
- Если класс представляет службу, которая должна быть легко интегрирована с другими классами.

Дополнительные сведения о свойствах, допускающих привязку, см. в разделе [свойства, допускающие](~/xamarin-forms/xaml/bindable-properties.md)привязку.

## <a name="create-an-attached-property"></a>Создание присоединенного свойства

Процесс создания присоединенного свойства выглядит следующим образом:

1. Создайте [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) экземпляр с одной из [`CreateAttached`](xref:Xamarin.Forms.BindableProperty.CreateAttached*) перегрузок метода.
1. Укажите `static` `Get` методы *PropertyName* и `Set` *PropertyName* в качестве методов доступа к присоединенному свойству.

### <a name="create-a-property"></a>Создание свойства

При создании присоединенного свойства для использования в других типах класс, в котором создается свойство, не обязательно должен быть производным от [`BindableObject`](xref:Xamarin.Forms.BindableObject) . Однако свойство *Target* для методов доступа должно иметь значение или быть производным от класса, [`BindableObject`](xref:Xamarin.Forms.BindableObject) .

Вложенное свойство может быть создано путем объявления `public static readonly` свойства типа [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Свойству BIND должно быть присвоено возвращаемое значение одного из [ `BindableProperty.CreateAttached` ] (xref: Xamarin.Forms . Биндаблепроперти. Креатеаттачед (System. String, System. Type, System. Type, System. Object, Xamarin.Forms . BindingMode, Xamarin.Forms . Биндаблепроперти. Валидатевалуеделегате, Xamarin.Forms . Биндаблепроперти. Биндингпропертичанжедделегате, Xamarin.Forms . Биндаблепроперти. Биндингпропертичангингделегате, Xamarin.Forms . Биндаблепроперти. Коерцевалуеделегате, Xamarin.Forms . Биндаблепроперти. Креатедефаултвалуеделегате)) перегрузки методов. Объявление должно находиться в теле класса-владельца, но за пределами определений элементов.

В следующем коде показан пример присоединенного свойства:

```csharp
public static readonly BindableProperty HasShadowProperty =
  BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false);
```

При этом создается присоединенное свойство с именем `HasShadow` типа `bool` . Свойство принадлежит `ShadowEffect` классу и имеет значение по умолчанию `false` . Соглашение об именовании присоединенных свойств заключается в том, что идентификатор присоединенного свойства должен совпадать с именем свойства, указанным в `CreateAttached` методе, с добавлением к нему "Property". Таким образом, в приведенном выше примере идентификатор присоединенного свойства — `HasShadowProperty` .

Дополнительные сведения о создании связываемых свойств, включая параметры, которые могут быть указаны во время создания, см. [в разделе Создание связываемого](~/xamarin-forms/xaml/bindable-properties.md#consume-a-bindable-property)свойства.

### <a name="create-accessors"></a>Создание методов доступа

В `Get` *PropertyName* `Set` качестве методов доступа к присоединенному свойству требуются статические методы PropertyName и *PropertyName* . в противном случае система свойств не сможет использовать присоединенное свойство. `Get`Метод доступа *PropertyName* должен соответствовать следующей сигнатуре:

```csharp
public static valueType GetPropertyName(BindableObject target)
```

`Get`Метод доступа *PropertyName* должен возвращать значение, которое содержится в соответствующем `BindableProperty` поле для присоединенного свойства. Это можно сделать, вызвав [ `GetValue` ] (xref: Xamarin.Forms . BindableObject. GetValue ( Xamarin.Forms . Биндаблепроперти)), передавая идентификатор привязываемого свойства для получения значения, а затем приведя результирующее значение к требуемому типу.

`Set`Метод доступа *PropertyName* должен соответствовать следующей сигнатуре:

```csharp
public static void SetPropertyName(BindableObject target, valueType value)
```

`Set`Метод доступа *PropertyName* должен задавать значение соответствующего `BindableProperty` поля для присоединенного свойства. Это можно сделать, вызвав [ `SetValue` ] (xref: Xamarin.Forms . BindableObject. SetValue ( Xamarin.Forms . Биндаблепроперти, System. Object)), передавая идентификатор привязываемого свойства, для которого задается значение, и задаваемый значение.

Для обоих методов доступа *целевой* объект должен иметь значение или быть производным от класса [`BindableObject`](xref:Xamarin.Forms.BindableObject) .

В следующем примере кода показаны методы доступа для `HasShadow` присоединенного свойства:

```csharp
public static bool GetHasShadow (BindableObject view)
{
  return (bool)view.GetValue (HasShadowProperty);
}

public static void SetHasShadow (BindableObject view, bool value)
{
  view.SetValue (HasShadowProperty, value);
}
```

### <a name="consume-an-attached-property"></a>Использование присоединенного свойства

После создания присоединенного свойства его можно использовать из XAML или кода. В XAML это достигается путем объявления пространства имен с префиксом с помощью объявления пространства имен, указывающего имя пространства имен среды CLR, и, при необходимости, имени сборки. Дополнительные сведения см. в разделе [пространства имен XAML](~/xamarin-forms/xaml/namespaces.md).

В следующем примере кода показано пространство имен XAML для пользовательского типа, содержащего присоединенное свойство, которое определено в той же сборке, что и код приложения, ссылающийся на настраиваемый тип:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EffectsDemo" ...>
  ...
</ContentPage>
```

Объявление пространства имен затем используется при установке присоединенного свойства для конкретного элемента управления, как показано в следующем примере кода XAML:

```xaml
<Label Text="Label Shadow Effect" local:ShadowEffect.HasShadow="true" />
```

Эквивалентный код на языке C# показан в следующем примере:

```csharp
var label = new Label { Text = "Label Shadow Effect" };
ShadowEffect.SetHasShadow (label, true);
```

### <a name="consume-an-attached-property-with-a-style"></a>Использование присоединенного свойства со стилем

Вложенные свойства также можно добавлять в элемент управления с помощью стиля. В следующем примере кода XAML показан *явный* стиль, использующий `HasShadow` присоединенное свойство, которое можно применить к [`Label`](xref:Xamarin.Forms.Label) элементам управления:

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="true" />
  </Style.Setters>
</Style>
```

Чтобы применить класс [`Style`](xref:Xamarin.Forms.Style) к классу [`Label`](xref:Xamarin.Forms.Label), его свойству [`Style`](xref:Xamarin.Forms.NavigableElement.Style) следует задать значение экземпляра `Style` с помощью расширения разметки `StaticResource`, как показано в следующем примере кода.

```xaml
<Label Text="Label Shadow Effect" Style="{StaticResource ShadowEffectStyle}" />
```

Дополнительные сведения о стилях см. в статье [Стили](~/xamarin-forms/user-interface/styles/index.md).

## <a name="advanced-scenarios"></a>Сложные сценарии

При создании присоединенного свойства существует ряд необязательных параметров, которые можно задать для включения расширенных сценариев присоединенных свойств. Это включает обнаружение изменений свойств, проверку значений свойств и приведение значений свойств. Дополнительные сведения см. в разделе [Расширенные сценарии](~/xamarin-forms/xaml/bindable-properties.md#advanced-scenarios).

## <a name="related-links"></a>Связанные ссылки

- [Привязываемые свойства](~/xamarin-forms/xaml/bindable-properties.md)
- [Пространства имен языка XAML](~/xamarin-forms/xaml/namespaces.md)
- [Эффект тени (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffect)
- [API Биндаблепроперти](xref:Xamarin.Forms.BindableProperty)
- [API BindableObject](xref:Xamarin.Forms.BindableObject)
