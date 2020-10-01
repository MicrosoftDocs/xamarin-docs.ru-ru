---
title: Передача параметров эффекта в качестве присоединенных свойств
description: Присоединенные свойства можно использовать для определения параметров эффекта, реагирующих на изменения свойств времени выполнения. В этой статье описывается использование присоединенных свойств для передачи параметров эффекту и изменение параметра во время выполнения.
ms.prod: xamarin
ms.assetid: DFCDCB9F-17DD-4117-BD53-B4FB206BB387
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c014396f9670ece93295c2c8da1738f9dc7dae9e
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91555987"
---
# <a name="passing-effect-parameters-as-attached-properties"></a>Передача параметров эффекта в качестве присоединенных свойств

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffectruntimechange)

_Присоединенные свойства можно использовать для определения параметров эффекта, реагирующих на изменения свойств времени выполнения. В этой статье описывается использование присоединенных свойств для передачи параметров эффекту и изменение параметра во время выполнения._

Процесс создания параметров эффектов, которые реагируют на изменения свойств времени выполнения, состоит из указанных далее этапов.

1. Создание класса `static`, который содержит присоединенное свойство для каждого параметра, передаваемого эффекту.
1. Добавление дополнительного присоединенного свойства в класс, который будет использоваться для управления добавлением эффекта в элемент управления, к которому будет присоединен класс, или удалением эффекта из элемента с присоединенным классом. Это присоединенное свойство должно регистрировать делегат `propertyChanged`, который будет выполняться при изменении значения свойства.
1. Создание методов получения и задания `static` для каждого присоединенного свойства.
1. Реализация логики в делегате `propertyChanged` для добавления и удаления эффекта.
1. Реализация вложенного класса внутри класса `static` (с именем, совпадающим с именем эффекта), который делит класс `RoutingEffect` на подклассы. Вызов конструктора базового класса с передачей объединения имени группы разрешения и уникального идентификатора, который был указан для каждого зависящего от платформы класса эффекта.

Затем параметры можно передать эффекту, добавив присоединенные свойства и значения свойств в соответствующий элемент управления. Кроме того, параметры можно изменить во время выполнения, указав значение нового присоединенного свойства.

> [!NOTE]
> Присоединенное свойство — это особый тип привязываемого свойства, определяемого в одном классе, но присоединяемого к другим объектам, и распознаваемого в XAML как атрибуты, которые содержат класс и имя свойства, разделенные точкой. Дополнительные сведения см. в статье [Общие сведения о присоединенных свойствах](~/xamarin-forms/xaml/attached-properties.md).

В примере приложения показан `ShadowEffect`, который добавляет тень к тексту, отображаемому элементом управления [`Label`](xref:Xamarin.Forms.Label). Кроме того, цвет тени может быть изменен во время выполнения. На следующей схеме показаны обязанности каждого проекта в примере приложения, а также связи между ними:

![Задачи проекта для эффекта тени](attached-properties-images/shadow-effect.png)

Элемент управления [`Label`](xref:Xamarin.Forms.Label) в `HomePage` настраивается с помощью класса `LabelShadowEffect` в каждом проекте для конкретной платформы. Параметры передаются каждому классу `LabelShadowEffect` через присоединенные свойства в классе `ShadowEffect`. Каждый класс `LabelShadowEffect` является производным от класса `PlatformEffect` для каждой платформы. В результате тень добавляется к тексту, отображаемому элементом управления `Label`, как показано на следующих снимках экрана.

![Эффект тени на каждой платформе](attached-properties-images/screenshots.png)

## <a name="creating-effect-parameters"></a>Создание параметров эффекта

Необходимо создать класс `static` для представления параметров эффекта, как показано в следующем примере кода.

```csharp
public static class ShadowEffect
{
  public static readonly BindableProperty HasShadowProperty =
    BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false, propertyChanged: OnHasShadowChanged);
  public static readonly BindableProperty ColorProperty =
    BindableProperty.CreateAttached ("Color", typeof(Color), typeof(ShadowEffect), Color.Default);
  public static readonly BindableProperty RadiusProperty =
    BindableProperty.CreateAttached ("Radius", typeof(double), typeof(ShadowEffect), 1.0);
  public static readonly BindableProperty DistanceXProperty =
    BindableProperty.CreateAttached ("DistanceX", typeof(double), typeof(ShadowEffect), 0.0);
  public static readonly BindableProperty DistanceYProperty =
    BindableProperty.CreateAttached ("DistanceY", typeof(double), typeof(ShadowEffect), 0.0);

  public static bool GetHasShadow (BindableObject view)
  {
    return (bool)view.GetValue (HasShadowProperty);
  }

  public static void SetHasShadow (BindableObject view, bool value)
  {
    view.SetValue (HasShadowProperty, value);
  }
  ...

  static void OnHasShadowChanged (BindableObject bindable, object oldValue, object newValue)
  {
    var view = bindable as View;
    if (view == null) {
      return;
    }

    bool hasShadow = (bool)newValue;
    if (hasShadow) {
      view.Effects.Add (new LabelShadowEffect ());
    } else {
      var toRemove = view.Effects.FirstOrDefault (e => e is LabelShadowEffect);
      if (toRemove != null) {
        view.Effects.Remove (toRemove);
      }
    }
  }

  class LabelShadowEffect : RoutingEffect
  {
    public LabelShadowEffect () : base ("MyCompany.LabelShadowEffect")
    {
    }
  }
}
```

Класс `ShadowEffect` содержит пять присоединенных свойств с методами получения и задания `static` для каждого присоединенного свойства. Четыре свойства представляют параметры, которые следует передать каждому классу `LabelShadowEffect` для конкретной платформы. Класс `ShadowEffect` также определяет присоединенное свойство `HasShadow`, которое используется для управления добавлением эффекта в элемент управления, к которому присоединен класс `ShadowEffect`, или удалением эффекта из элемента. Это присоединенное свойство регистрирует метод `OnHasShadowChanged`, который будет выполняться при изменении значения свойства. В зависимости от значения присоединенного свойства `HasShadow` этот метод добавляет или удаляет эффект.

Вложенный класс `LabelShadowEffect`, который делит класс [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) на подклассы, поддерживает операции добавления и удаления эффекта. Класс `RoutingEffect` представляет независимый от платформы эффект, являющийся оболочкой для внутреннего эффекта, который обычно специфичен для платформы. Это упрощает процесс удаления эффекта, так как отсутствует доступ времени компиляции к сведениям о типе для определяемого платформой эффекта. Конструктор `LabelShadowEffect` вызывает конструктор базового класса, передавая параметр, состоящий из объединения имени группы разрешения и уникального идентификатора, который был указан для каждого зависящего от платформы класса эффекта. Это позволяет добавлять эффект в метод `OnHasShadowChanged` или удалять его оттуда, как показано ниже.

- **Добавление эффекта** — в коллекцию [`Effects`](xref:Xamarin.Forms.Element.Effects) элемента управления добавляется новый экземпляр класса `LabelShadowEffect`. Это действие заменяет использование метода [`Effect.Resolve`](xref:Xamarin.Forms.Effect.Resolve(System.String)) для добавления эффекта.
- **Удаление эффекта** — из коллекции [`Effects`](xref:Xamarin.Forms.Element.Effects) элемента управления извлекается и удаляется первый экземпляр класса `LabelShadowEffect`.

## <a name="consuming-the-effect"></a>Использование эффекта

Каждый зависящий от платформы `LabelShadowEffect` можно использовать путем добавления присоединенных свойств в элемент управления [`Label`](xref:Xamarin.Forms.Label), как показано в следующем примере кода XAML.

```xaml
<Label Text="Label Shadow Effect" ...
       local:ShadowEffect.HasShadow="true" local:ShadowEffect.Radius="5"
       local:ShadowEffect.DistanceX="5" local:ShadowEffect.DistanceY="5">
  <local:ShadowEffect.Color>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="Black" />
        <On Platform="Android" Value="White" />
        <On Platform="UWP" Value="Red" />
    </OnPlatform>
  </local:ShadowEffect.Color>
</Label>
```

В следующем примере кода показан эквивалентный элемент управления [`Label`](xref:Xamarin.Forms.Label) на языке C#.

```csharp
var label = new Label {
  Text = "Label Shadow Effect",
  ...
};

Color color = Color.Default;
switch (Device.RuntimePlatform)
{
    case Device.iOS:
        color = Color.Black;
        break;
    case Device.Android:
        color = Color.White;
        break;
    case Device.UWP:
        color = Color.Red;
        break;
}

ShadowEffect.SetHasShadow (label, true);
ShadowEffect.SetRadius (label, 5);
ShadowEffect.SetDistanceX (label, 5);
ShadowEffect.SetDistanceY (label, 5);
ShadowEffect.SetColor (label, color));
```

Если присоединенному свойству `ShadowEffect.HasShadow` задать значение `true`, выполняется метод `ShadowEffect.OnHasShadowChanged`, который добавляет класс `LabelShadowEffect` в элемент управления [`Label`](xref:Xamarin.Forms.Label) или удаляет его из него. В обоих примерах кода присоединенное свойство `ShadowEffect.Color` предоставляет соответствующие платформе цветовые значения. Дополнительные сведения см. в разделе о [классе Device](~/xamarin-forms/platform/device.md).

Кроме того, класс [`Button`](xref:Xamarin.Forms.Button) позволяет изменять цвет тени во время выполнения. При нажатии `Button` следующий код изменяет цвет тени путем задания присоединенного свойства `ShadowEffect.Color`.

```csharp
ShadowEffect.SetColor (label, Color.Teal);
```

### <a name="consuming-the-effect-with-a-style"></a>Использование эффекта со стилем

Эффекты, которые можно использовать путем добавления присоединенных свойств в элемент управления, также могут использоваться в стиле. В следующем примере кода XAML показан *явный* стиль для эффекта тени, который может применяться к элементам управления [`Label`](xref:Xamarin.Forms.Label).

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="True" />
    <Setter Property="local:ShadowEffect.Radius" Value="5" />
    <Setter Property="local:ShadowEffect.DistanceX" Value="5" />
    <Setter Property="local:ShadowEffect.DistanceY" Value="5" />
  </Style.Setters>
</Style>
```

Чтобы применить класс [`Style`](xref:Xamarin.Forms.Style) к классу [`Label`](xref:Xamarin.Forms.Label), его свойству [`Style`](xref:Xamarin.Forms.NavigableElement.Style) следует задать значение экземпляра `Style` с помощью расширения разметки `StaticResource`, как показано в следующем примере кода.

```xaml
<Label Text="Label Shadow Effect" ... Style="{StaticResource ShadowEffectStyle}" />
```

Дополнительные сведения о стилях см. в статье [Стили](~/xamarin-forms/user-interface/styles/index.md).

## <a name="creating-the-effect-on-each-platform"></a>Создание эффекта на каждой платформе

В следующих разделах рассматривается реализация класса `LabelShadowEffect` для каждой платформы.

### <a name="ios-project"></a>Проект на платформе iOS

В следующем примере кода показана реализация класса `LabelShadowEffect` для проекта на платформе iOS.

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached ()
        {
            try {
                UpdateRadius ();
                UpdateColor ();
                UpdateOffset ();
                Control.Layer.ShadowOpacity = 1.0f;
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateRadius ()
        {
            Control.Layer.ShadowRadius = (nfloat)ShadowEffect.GetRadius (Element);
        }

        void UpdateColor ()
        {
            Control.Layer.ShadowColor = ShadowEffect.GetColor (Element).ToCGColor ();
        }

        void UpdateOffset ()
        {
            Control.Layer.ShadowOffset = new CGSize (
                (double)ShadowEffect.GetDistanceX (Element),
                (double)ShadowEffect.GetDistanceY (Element));
        }
    }
```

Метод `OnAttached` вызывает методы, которые получают значения присоединенного свойства с помощью методов получения `ShadowEffect` и задают для свойств `Control.Layer` соответствующие значения для создания тени. Ели у элемента управления, к которому присоединен эффект, отсутствуют свойства `Control.Layer`, указанная выше функциональность заключается в блок `try`/`catch`. Поскольку очистка не требуется, метод `OnDetached` не предоставляет реализацию.

#### <a name="responding-to-property-changes"></a>Реагирование на изменения свойств

Если во время выполнения происходит изменение значений присоединенных свойств `ShadowEffect`, эффект должен отреагировать путем отображения изменений. Переопределенная версия метода `OnElementPropertyChanged` в классе эффекта, зависящем от платформы, отвечает на изменения привязываемого свойства, как показано в следующем примере кода.

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.RadiusProperty.PropertyName) {
      UpdateRadius ();
    } else if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
               args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
    }
  }
  ...
}
```

Метод `OnElementPropertyChanged` обновляет радиус распространения, цвет и смещение тени при условии, что изменилось соответствующее значение присоединенного свойства `ShadowEffect`. Следует всегда проверять наличие измененного свойства, так как это переопределение может вызываться многократно.

### <a name="android-project"></a>Проект на платформе Android

В следующем примере кода показана реализация класса `LabelShadowEffect` для проекта на платформе Android.

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.Droid
{
    public class LabelShadowEffect : PlatformEffect
    {
        Android.Widget.TextView control;
        Android.Graphics.Color color;
        float radius, distanceX, distanceY;

        protected override void OnAttached ()
        {
            try {
                control = Control as Android.Widget.TextView;
                UpdateRadius ();
                UpdateColor ();
                UpdateOffset ();
                UpdateControl ();
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateControl ()
        {
            if (control != null) {
                control.SetShadowLayer (radius, distanceX, distanceY, color);
            }
        }

        void UpdateRadius ()
        {
            radius = (float)ShadowEffect.GetRadius (Element);
        }

        void UpdateColor ()
        {
            color = ShadowEffect.GetColor (Element).ToAndroid ();
        }

        void UpdateOffset ()
        {
            distanceX = (float)ShadowEffect.GetDistanceX (Element);
            distanceY = (float)ShadowEffect.GetDistanceY (Element);
        }
    }
```

Метод `OnAttached` вызывает методы, которые получают значения присоединенного свойства с помощью методов получения `ShadowEffect`, и вызывает метод, который вызывает метод [`TextView.SetShadowLayer`](xref:Android.Widget.TextView.SetShadowLayer*) для создания тени с использованием значений свойств. Если у элемента управления, к которому присоединен эффект, отсутствуют свойства `Control.Layer`, указанная выше функциональность заключается в блок `try`/`catch`. Поскольку очистка не требуется, метод `OnDetached` не предоставляет реализацию.

#### <a name="responding-to-property-changes"></a>Реагирование на изменения свойств

Если во время выполнения происходит изменение значений присоединенных свойств `ShadowEffect`, эффект должен отреагировать путем отображения изменений. Переопределенная версия метода `OnElementPropertyChanged` в классе эффекта, зависящем от платформы, отвечает на изменения привязываемого свойства, как показано в следующем примере кода.

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.RadiusProperty.PropertyName) {
      UpdateRadius ();
      UpdateControl ();
    } else if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
      UpdateControl ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
               args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
      UpdateControl ();
    }
  }
  ...
}
```

Метод `OnElementPropertyChanged` обновляет радиус распространения, цвет и смещение тени при условии, что изменилось соответствующее значение присоединенного свойства `ShadowEffect`. Следует всегда проверять наличие измененного свойства, так как это переопределение может вызываться многократно.

### <a name="universal-windows-platform-project"></a>Проект на универсальной платформе Windows

В следующем коде показана реализация класса `LabelShadowEffect` для проекта на универсальной платформе Windows (UWP):

```csharp
[assembly: ResolutionGroupName ("MyCompany")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.UWP
{
    public class LabelShadowEffect : PlatformEffect
    {
        Label shadowLabel;
        bool shadowAdded = false;

        protected override void OnAttached ()
        {
            try {
                if (!shadowAdded) {
                    var textBlock = Control as Windows.UI.Xaml.Controls.TextBlock;

                    shadowLabel = new Label ();
                    shadowLabel.Text = textBlock.Text;
                    shadowLabel.FontAttributes = FontAttributes.Bold;
                    shadowLabel.HorizontalOptions = LayoutOptions.Center;
                    shadowLabel.VerticalOptions = LayoutOptions.CenterAndExpand;

                    UpdateColor ();
                    UpdateOffset ();

                    ((Grid)Element.Parent).Children.Insert (0, shadowLabel);
                    shadowAdded = true;
                }
            } catch (Exception ex) {
                Debug.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateColor ()
        {
            shadowLabel.TextColor = ShadowEffect.GetColor (Element);
        }

        void UpdateOffset ()
        {
            shadowLabel.TranslationX = ShadowEffect.GetDistanceX (Element);
            shadowLabel.TranslationY = ShadowEffect.GetDistanceY (Element);
        }
    }
}
```

На платформе UWP эффект тени отсутствует, поэтому реализация `LabelShadowEffect` на обеих платформах имитирует его, добавляя второй класс смещения [`Label`](xref:Xamarin.Forms.Label) за основной экземпляр `Label`. Метод `OnAttached` создает экземпляр `Label` и задает для `Label` определенные свойства макета. Затем он вызывает методы, которые получают значения присоединенных свойств с помощью методов получения `ShadowEffect`, и создает тень, задавая свойства [`TextColor`](xref:Xamarin.Forms.Label.TextColor), [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) и [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) для управления цветом и расположением `Label`. `shadowLabel` является смещением, вставленным за основной экземпляр `Label`. Если у элемента управления, к которому присоединен эффект, отсутствуют свойства `Control.Layer`, указанная выше функциональность заключается в блок `try`/`catch`. Поскольку очистка не требуется, метод `OnDetached` не предоставляет реализацию.

#### <a name="responding-to-property-changes"></a>Реагирование на изменения свойств

Если во время выполнения происходит изменение значений присоединенных свойств `ShadowEffect`, эффект должен отреагировать путем отображения изменений. Переопределенная версия метода `OnElementPropertyChanged` в классе эффекта, зависящем от платформы, отвечает на изменения привязываемого свойства, как показано в следующем примере кода.

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
                      args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
    }
  }
  ...
}
```

Метод `OnElementPropertyChanged` обновляет цвет или смещение тени при условии, что изменилось соответствующее значение вложенного свойства `ShadowEffect`. Следует всегда проверять наличие измененного свойства, так как это переопределение может вызываться многократно.

## <a name="summary"></a>Сводка

В этой статье описывалось использование присоединенных свойств для передачи параметров эффекту и изменение параметра во время выполнения. Присоединенные свойства можно использовать для определения параметров эффекта, реагирующих на изменения свойств времени выполнения.

## <a name="related-links"></a>Связанные ссылки

- [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Эффект](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [Эффект тени (пример)](/samples/xamarin/xamarin-forms-samples/effects-shadoweffectruntimechange)