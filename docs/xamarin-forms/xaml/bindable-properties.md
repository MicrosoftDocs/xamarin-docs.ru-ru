---
Title: " Xamarin.Forms описание связываемых свойств" Description: "в этой статье содержатся общие сведения о связываемых свойствах и демонстрируется их создание и использование".
MS. произв. Xamarin MS. AssetID: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 01/16/2020 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-bindable-properties"></a>Xamarin.FormsПривязываемые свойства

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)

Привязываемые свойства расширяют функциональные возможности свойства CLR путем резервного копирования свойства с [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) типом, а не для резервного копирования свойства с полем. Назначение свойств, допускающих привязку, заключается в предоставлении системы свойств, поддерживающей привязку данных, стили, шаблоны и значения, заданные с помощью связей типа «родители-потомки». Кроме того, привязываемые свойства могут предоставлять значения по умолчанию, проверку значений свойств и обратные вызовы, которые отслеживают изменения свойств.

Свойства должны быть реализованы в виде связываемых свойств для поддержки одной или нескольких из следующих функций:

- Действует как допустимое *целевое* свойство для привязки данных.
- Задание свойства с помощью [стиля](~/xamarin-forms/user-interface/styles/index.md).
- Предоставление значения свойства по умолчанию, отличного от используемого по умолчанию для типа свойства.
- Проверка значения свойства.
- Наблюдение за изменениями свойств.

Примеры Xamarin.Forms связываемых свойств включают [`Label.Text`](xref:Xamarin.Forms.Label.Text) , [`Button.BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius) и [`StackLayout.Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) . Каждое связываемое свойство имеет соответствующее `public static readonly` поле типа, которое [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) представлено в том же классе и является идентификатором привязываемого свойства. Например, соответствующий идентификатор свойства привязки для `Label.Text` свойства — [`Label.TextProperty`](xref:Xamarin.Forms.Label.TextProperty) .

## <a name="create-a-bindable-property"></a>Создание привязываемого свойства

Процесс создания привязываемого свойства выглядит следующим образом:

1. Создайте [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) экземпляр с одной из [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create*) перегрузок метода.
1. Определите методы доступа к свойствам для [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) экземпляра.

Все [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) экземпляры должны быть созданы в потоке пользовательского интерфейса. Это означает, что только код, выполняемый в потоке пользовательского интерфейса, может получить или задать значение привязываемого свойства. Однако `BindableProperty` доступ к экземплярам можно осуществлять из других потоков путем маршалирования в поток пользовательского интерфейса с помощью [`Device.BeginInvokeOnMainThread`](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) метода.

### <a name="create-a-property"></a>Создание свойства

Чтобы создать `BindableProperty` экземпляр, содержащий класс должен быть производным от [`BindableObject`](xref:Xamarin.Forms.BindableObject) класса. Однако `BindableObject` класс имеет высокий уровень в иерархии классов, поэтому большинство классов, используемых для функциональности пользовательского интерфейса, поддерживают связываемые свойства.

Свойство, доступное для привязки, может быть создано путем объявления `public static readonly` свойства типа [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Свойству BIND должно быть присвоено возвращаемое значение одного из [ `BindableProperty.Create` ] (xref: Xamarin.Forms . Биндаблепроперти. Create (System. String, System. Type, System. Type, System. Object, Xamarin.Forms . BindingMode, Xamarin.Forms . Биндаблепроперти. Валидатевалуеделегате, Xamarin.Forms . Биндаблепроперти. Биндингпропертичанжедделегате, Xamarin.Forms . Биндаблепроперти. Биндингпропертичангингделегате, Xamarin.Forms . Биндаблепроперти. Коерцевалуеделегате, Xamarin.Forms . Биндаблепроперти. Креатедефаултвалуеделегате)) перегрузки методов. Объявление должно находиться в теле [`BindableObject`](xref:Xamarin.Forms.BindableObject) производного класса, но за пределами определений элементов.

При создании необходимо указать как минимум идентификатор [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) , а также следующие параметры:

- Имя [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) .
- Тип свойства.
- Тип объекта-владельца.
- Значение по умолчанию для свойства. Это гарантирует, что свойство всегда возвращает определенное значение по умолчанию, если оно не задано, и может отличаться от значения по умолчанию для типа свойства. Значение по умолчанию будет восстановлено при [ `ClearValue` ] (xref: Xamarin.Forms . BindableObject. ClearValue ( Xamarin.Forms . Биндаблепроперти)) вызывается метод для свойства, доступного для привязки.

В следующем коде показан пример привязываемого свойства с идентификатором и значениями для четырех обязательных параметров:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

При этом создается [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) экземпляр с именем `EventName` типа `string` . Свойство принадлежит `EventToCommandBehavior` классу и имеет значение по умолчанию `null` . Соглашение об именовании для свойств, допускающих привязку, заключается в том, что идентификатор связываемого свойства должен совпадать с именем свойства, указанным в `Create` методе, с добавлением к нему "Property". Таким образом, в приведенном выше примере идентификатор свойства, допускающий привязку, — `EventNameProperty` .

При необходимости при создании [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) экземпляра можно указать следующие параметры.

- Режим привязки. Используется для указания направления, в котором изменения значения свойства будут распространены. В режиме привязки по умолчанию изменения будут распространяться от *источника* к *целевому объекту*.
- Делегат проверки, который будет вызываться, если задано значение свойства. Дополнительные сведения см. в разделе [обратные вызовы проверки](#validation-callbacks).
- Измененное свойство делегат, который будет вызываться при изменении значения свойства. Дополнительные сведения см. в разделе [Обнаружение изменений свойств](#detect-property-changes).
- Изменяемое свойство делегата, которое будет вызываться при изменении значения свойства. Этот делегат имеет ту же сигнатуру, что и делегат, измененный свойством.
- Приведенный делегат значения, который будет вызываться при изменении значения свойства. Дополнительные сведения см. в разделе [приведение обратных вызовов к значению](#coerce-value-callbacks).
- Объект `Func` , используемый для инициализации значения свойства по умолчанию. Дополнительные сведения см. в разделе [Создание значения по умолчанию с помощью Func](#create-a-default-value-with-a-func).

### <a name="create-accessors"></a>Создание методов доступа

Для доступа к связываемому свойству методы доступа к свойствам должны использовать синтаксис свойства. `Get`Метод доступа должен возвращать значение, содержащееся в соответствующем связываемом свойстве. Это можно сделать, вызвав [ `GetValue` ] (xref: Xamarin.Forms . BindableObject. GetValue ( Xamarin.Forms . Биндаблепроперти)), передав идентификатор привязываемого свойства для получения значения, а затем приведя результат к требуемому типу. `Set`Метод доступа должен задавать значение соответствующего привязываемого свойства. Это можно сделать, вызвав [ `SetValue` ] (xref: Xamarin.Forms . BindableObject. SetValue ( Xamarin.Forms . Биндаблепроперти, System. Object)), передавая идентификатор привязываемого свойства, для которого задается значение, и задаваемый значение.

В следующем примере кода показаны методы доступа для `EventName` привязываемого свойства:

```csharp
public string EventName
{
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

## <a name="consume-a-bindable-property"></a>Использование привязываемого свойства

После создания свойства, доступного для привязки, его можно использовать из XAML или кода. В XAML это достигается путем объявления пространства имен с префиксом с помощью объявления пространства имен, указывающего имя пространства имен CLR, и, при необходимости, имени сборки. Дополнительные сведения см. в разделе [пространства имен XAML](~/xamarin-forms/xaml/namespaces.md).

В следующем примере кода показано пространство имен XAML для пользовательского типа, который содержит свойство, доступное для привязки, которое определено в той же сборке, что и код приложения, ссылающийся на пользовательский тип:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

Объявление пространства имен используется при задании свойства, доступного для `EventName` привязки, как показано в следующем примере кода XAML:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

Эквивалентный код на языке C# показан в следующем примере:

```csharp
var listView = new ListView ();
listView.Behaviors.Add (new EventToCommandBehavior
{
  EventName = "ItemSelected",
  ...
});
```

## <a name="advanced-scenarios"></a>Сложные сценарии

При создании [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) экземпляра существует ряд необязательных параметров, которые можно задать для включения расширенных сценариев свойств с возможностью привязки. В этом разделе рассматриваются эти сценарии.

### <a name="detect-property-changes"></a>Обнаружение изменений свойств

`static`Метод обратного вызова, измененный свойством, можно зарегистрировать с помощью свойства, доступного для привязки, указав `propertyChanged` параметр для [ `BindableProperty.Create` ] (xref: Xamarin.Forms . Биндаблепроперти. Create (System. String, System. Type, System. Type, System. Object, Xamarin.Forms . BindingMode, Xamarin.Forms . Биндаблепроперти. Валидатевалуеделегате, Xamarin.Forms . Биндаблепроперти. Биндингпропертичанжедделегате, Xamarin.Forms . Биндаблепроперти. Биндингпропертичангингделегате, Xamarin.Forms . Биндаблепроперти. Коерцевалуеделегате, Xamarin.Forms . Биндаблепроперти. Креатедефаултвалуеделегате)). Указанный метод обратного вызова будет вызываться при изменении значения свойства, доступного для привязки.

В следующем примере кода показано, как `EventName` свойство, поддерживающее привязку, регистрирует `OnEventNameChanged` метод в виде метода обратного вызова, измененного свойством:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create (
    "EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
...

static void OnEventNameChanged (BindableObject bindable, object oldValue, object newValue)
{
  // Property changed implementation goes here
}
```

В методе обратного вызова, измененном свойством, [`BindableObject`](xref:Xamarin.Forms.BindableObject) параметр используется для обозначения того, какой экземпляр класса-владельца сообщил об изменении, а значения двух `object` параметров представляют старое и новое значения привязываемого свойства.

### <a name="validation-callbacks"></a>Обратные вызовы проверки

`static`Метод обратного вызова проверки можно зарегистрировать с помощью свойства, доступного для привязки, указав `validateValue` параметр для [ `BindableProperty.Create` ] (xref: Xamarin.Forms . Биндаблепроперти. Create (System. String, System. Type, System. Type, System. Object, Xamarin.Forms . BindingMode, Xamarin.Forms . Биндаблепроперти. Валидатевалуеделегате, Xamarin.Forms . Биндаблепроперти. Биндингпропертичанжедделегате, Xamarin.Forms . Биндаблепроперти. Биндингпропертичангингделегате, Xamarin.Forms . Биндаблепроперти. Коерцевалуеделегате, Xamarin.Forms . Биндаблепроперти. Креатедефаултвалуеделегате)). Указанный метод обратного вызова будет вызываться, если задано значение свойства BIND.

В следующем примере кода показано, как `Angle` свойство с возможностью привязки регистрирует `IsValidValue` метод в качестве метода обратного вызова проверки:

```csharp
public static readonly BindableProperty AngleProperty =
  BindableProperty.Create ("Angle", typeof(double), typeof(HomePage), 0.0, validateValue: IsValidValue);
...

static bool IsValidValue (BindableObject view, object value)
{
  double result;
  bool isDouble = double.TryParse (value.ToString (), out result);
  return (result >= 0 && result <= 360);
}
```

Обратные вызовы проверки предоставляются со значением и должны возвращать значение `true` , если значение является допустимым для свойства, в противном случае — `false` . Если обратный вызов проверки возвращает значение `false` , которое должно обрабатываться разработчиком, возникнет исключение. Типичное использование метода обратного вызова проверки ограничивает значения целых чисел или Double, если задано свойство, доступное для привязки. Например, `IsValidValue` метод проверяет, что значение свойства находится в диапазоне от `double` 0 до 360.

### <a name="coerce-value-callbacks"></a>Приведение обратных вызовов значения

`static`Приведенный ниже метод обратного вызова значения можно зарегистрировать с помощью свойства, доступного для привязки, указав `coerceValue` параметр для [ `BindableProperty.Create` ] (xref: Xamarin.Forms . Биндаблепроперти. Create (System. String, System. Type, System. Type, System. Object, Xamarin.Forms . BindingMode, Xamarin.Forms . Биндаблепроперти. Валидатевалуеделегате, Xamarin.Forms . Биндаблепроперти. Биндингпропертичанжедделегате, Xamarin.Forms . Биндаблепроперти. Биндингпропертичангингделегате, Xamarin.Forms . Биндаблепроперти. Коерцевалуеделегате, Xamarin.Forms . Биндаблепроперти. Креатедефаултвалуеделегате)). Указанный метод обратного вызова будет вызываться при изменении значения свойства, доступного для привязки.

> [!IMPORTANT]
> `BindableObject`Тип имеет `CoerceValue` метод, который может быть вызван для принудительной повторной оценки значения его `BindableProperty` аргумента путем вызова обратного вызова приводимого значения.

Обратные вызовы приводимых значений используются для принудительного переоценки привязываемого свойства при изменении значения свойства. Например, можно использовать обратный вызов приводимого значения, чтобы убедиться, что значение одного связываемого свойства не больше значения другого привязываемого свойства.

В следующем примере кода показано, как `Angle` свойство с возможностью привязки регистрирует `CoerceAngle` метод в качестве метода обратного вызова для приведения значения:

```csharp
public static readonly BindableProperty AngleProperty = BindableProperty.Create (
  "Angle", typeof(double), typeof(HomePage), 0.0, coerceValue: CoerceAngle);
public static readonly BindableProperty MaximumAngleProperty = BindableProperty.Create (
  "MaximumAngle", typeof(double), typeof(HomePage), 360.0, propertyChanged: ForceCoerceValue);
...

static object CoerceAngle (BindableObject bindable, object value)
{
  var homePage = bindable as HomePage;
  double input = (double)value;

  if (input > homePage.MaximumAngle)
  {
    input = homePage.MaximumAngle;
  }
  return input;
}

static void ForceCoerceValue(BindableObject bindable, object oldValue, object newValue)
{
  bindable.CoerceValue(AngleProperty);
}
```

`CoerceAngle`Метод проверяет значение `MaximumAngle` свойства, и если `Angle` значение свойства больше, оно приводит значение к `MaximumAngle` значению свойства. Кроме того, при `MaximumAngle` изменении свойства функция обратного вызова приводимого значения вызывается для `Angle` свойства путем вызова `CoerceValue` метода.

### <a name="create-a-default-value-with-a-func"></a>Создание значения по умолчанию с помощью Func

`Func`Можно использовать для инициализации значения по умолчанию привязываемого свойства, как показано в следующем примере кода:

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

Параметру присваивается значение `defaultValueCreator` `Func` , вызывающее [ `Device.GetNamedSize` ] (xref: Xamarin.Forms . Device. Жетнамедсизе ( Xamarin.Forms . Намедсизе, System. Type)) `double` , который возвращает, представляющий именованный размер шрифта, используемого на [`Label`](xref:Xamarin.Forms.Label) собственной платформе.

## <a name="related-links"></a>Связанные ссылки

- [Пространства имен языка XAML](~/xamarin-forms/xaml/namespaces.md)
- [Поведение события для команды (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)
- [Обратный вызов проверки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-validationcallback)
- [Обратный вызов значения приведения (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-coercevaluecallback)
- [API Биндаблепроперти](xref:Xamarin.Forms.BindableProperty)
- [API BindableObject](xref:Xamarin.Forms.BindableObject)
