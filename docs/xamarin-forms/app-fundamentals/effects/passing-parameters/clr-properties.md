---
title: Передача параметров эффекта в качестве свойств среды CLR
description: Свойства общеязыковой среды выполнения (CLR) можно использовать для определения параметров эффекта, не реагирующих на изменения свойств во время выполнения. В этой статье демонстрируется использование свойств среды CLR для передачи параметров эффекту.
ms.prod: xamarin
ms.assetid: 4B50466C-5DBD-45DD-B1E6-BE9524C92F27
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 63c7eb799b92871d7e00b7302bbe86f8b959ec23
ms.sourcegitcommit: 05ba8ffb8b34ec881b89e442323f3edd8de18f2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/18/2020
ms.locfileid: "83546041"
---
# <a name="passing-effect-parameters-as-common-language-runtime-properties"></a>Передача параметров эффекта в качестве свойств среды CLR

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffect)

_Свойства общеязыковой среды выполнения (CLR) можно использовать для определения параметров эффекта, не реагирующих на изменения свойств во время выполнения. В этой статье демонстрируется использование свойств среды CLR для передачи параметров эффекту._

Процесс создания параметров эффектов, которые не реагируют на изменения свойств во время выполнения, состоит из указанных далее этапов.

1. Создайте класс `public`, являющийся подклассом класса [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect). Класс `RoutingEffect` представляет независимый от платформы эффект, являющийся оболочкой для внутреннего эффекта, который обычно специфичен для платформы.
1. Создайте конструктор, вызывающий конструктор базового класса с передачей объединения имени группы разрешения и уникального идентификатора, который был указан для каждого зависящего от платформы класса эффекта.
1. Добавьте в класс свойства для каждого параметра, передаваемого эффекту.

Затем параметры можно передать эффекту, указав значение каждого свойства при создании экземпляра эффекта.

В примере приложения показан эффект `ShadowEffect`, который добавляет тень к тексту, отображаемому элементом управления [`Label`](xref:Xamarin.Forms.Label). На следующей схеме показаны обязанности каждого проекта в примере приложения, а также связи между ними:

![](clr-properties-images/shadow-effect.png "Shadow Effect Project Responsibilities")

Элемент управления [`Label`](xref:Xamarin.Forms.Label) в `HomePage` настраивается с помощью класса `LabelShadowEffect` в каждом проекте для конкретной платформы. Параметры передаются каждому эффекту `LabelShadowEffect` через свойства в классе `ShadowEffect`. Каждый класс `LabelShadowEffect` является производным от класса `PlatformEffect` для каждой платформы. В результате тень добавляется к тексту, отображаемому элементом управления `Label`, как показано на следующих снимках экрана.

![](clr-properties-images/screenshots.png "Shadow Effect on each Platform")

## <a name="creating-effect-parameters"></a>Создание параметров эффекта

Необходимо создать класс `public`, являющийся подклассом класса [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect), для представления параметров эффекта, как показано в следующем примере кода:

```csharp
public class ShadowEffect : RoutingEffect
{
  public float Radius { get; set; }

  public Color Color { get; set; }

  public float DistanceX { get; set; }

  public float DistanceY { get; set; }

  public ShadowEffect () : base ("MyCompany.LabelShadowEffect")
  {            
  }
}
```

Эффект `ShadowEffect` содержит четыре свойства, представляющие параметры, которые следует передать каждому классу `LabelShadowEffect` для конкретной платформы. Конструктор класса вызывает конструктор базового класса, передавая параметр, состоящий из объединения имени группы разрешения и уникального идентификатора, который был указан для каждого зависящего от платформы класса эффекта. Поэтому при создании экземпляра `ShadowEffect` в коллекцию [`Effects`](xref:Xamarin.Forms.Element.Effects) элемента управления добавляется новый экземпляр `MyCompany.LabelShadowEffect`.

## <a name="consuming-the-effect"></a>Использование эффекта

В следующем примере кода XAML показан элемент управления [`Label`](xref:Xamarin.Forms.Label), к которому присоединяется `ShadowEffect`:

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Effects>
    <local:ShadowEffect Radius="5" DistanceX="5" DistanceY="5">
      <local:ShadowEffect.Color>
        <OnPlatform x:TypeArguments="Color">
            <On Platform="iOS" Value="Black" />
            <On Platform="Android" Value="White" />
            <On Platform="UWP" Value="Red" />
        </OnPlatform>
      </local:ShadowEffect.Color>
    </local:ShadowEffect>
  </Label.Effects>
</Label>
```

В следующем примере кода показан эквивалентный элемент управления [`Label`](xref:Xamarin.Forms.Label) на языке C#:

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

label.Effects.Add (new ShadowEffect {
  Radius = 5,
  Color = color,
  DistanceX = 5,
  DistanceY = 5
});
```

В обоих примерах кода создается экземпляр класса `ShadowEffect`. Перед его добавлением в коллекцию [`Effects`](xref:Xamarin.Forms.Element.Effects) элемента управления задаются значения для каждого его свойства. Обратите внимание на то, что свойство `ShadowEffect.Color` использует значения цвета для конкретной платформы. Дополнительные сведения см. в статье о [классе Device](~/xamarin-forms/platform/device.md).

## <a name="creating-the-effect-on-each-platform"></a>Создание эффекта на каждой платформе

В следующих разделах рассматривается реализация класса `LabelShadowEffect` для каждой платформы.

### <a name="ios-project"></a>Проект на платформе iOS

В следующем примере кода показана реализация класса `LabelShadowEffect` для проекта на платформе iOS:

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
                var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                if (effect != null) {
                    Control.Layer.ShadowRadius = effect.Radius;
                    Control.Layer.ShadowColor = effect.Color.ToCGColor ();
                    Control.Layer.ShadowOffset = new CGSize (effect.DistanceX, effect.DistanceY);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

Метод `OnAttached` получает экземпляр `ShadowEffect` и задает для свойств `Control.Layer` соответствующие значения для создания тени. Если у элемента управления, к которому присоединен эффект, отсутствуют свойства `Control.Layer`, указанная выше функциональность заключается в блок `try`/`catch`. Так как очистка не требуется, метод `OnDetached` не предоставляет реализацию.

### <a name="android-project"></a>Проект на платформе Android

В следующем примере кода показана реализация класса `LabelShadowEffect` для проекта на платформе Android:

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.Droid
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached ()
        {
            try {
                var control = Control as Android.Widget.TextView;
                var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                if (effect != null) {
                    float radius = effect.Radius;
                    float distanceX = effect.DistanceX;
                    float distanceY = effect.DistanceY;
                    Android.Graphics.Color color = effect.Color.ToAndroid ();
                    control.SetShadowLayer (radius, distanceX, distanceY, color);
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

Метод `OnAttached` получает экземпляр `ShadowEffect` и вызывает метод [`TextView.SetShadowLayer`](xref:Android.Widget.TextView.SetShadowLayer*) для создания тени с использованием указанных значений свойств. Если у элемента управления, к которому присоединен эффект, отсутствуют свойства `Control.Layer`, указанная выше функциональность заключается в блок `try`/`catch`. Так как очистка не требуется, метод `OnDetached` не предоставляет реализацию.

### <a name="universal-windows-platform-project"></a>Проект на универсальной платформе Windows

В следующем коде показана реализация класса `LabelShadowEffect` для проекта на универсальной платформе Windows (UWP):

```csharp
[assembly: ResolutionGroupName ("Xamarin")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.UWP
{
    public class LabelShadowEffect : PlatformEffect
    {
        bool shadowAdded = false;

        protected override void OnAttached ()
        {
            try {
                if (!shadowAdded) {
                    var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                    if (effect != null) {
                        var textBlock = Control as Windows.UI.Xaml.Controls.TextBlock;
                        var shadowLabel = new Label ();
                        shadowLabel.Text = textBlock.Text;
                        shadowLabel.FontAttributes = FontAttributes.Bold;
                        shadowLabel.HorizontalOptions = LayoutOptions.Center;
                        shadowLabel.VerticalOptions = LayoutOptions.CenterAndExpand;
                        shadowLabel.TextColor = effect.Color;
                        shadowLabel.TranslationX = effect.DistanceX;
                        shadowLabel.TranslationY = effect.DistanceY;

                        ((Grid)Element.Parent).Children.Insert (0, shadowLabel);
                        shadowAdded = true;
                    }
                }
            } catch (Exception ex) {
                Debug.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

На платформе UWP эффект тени отсутствует, поэтому реализация `LabelShadowEffect` на обеих платформах имитирует его, добавляя второй класс смещения [`Label`](xref:Xamarin.Forms.Label) после основного экземпляра `Label`. Метод `OnAttached` получает экземпляр `ShadowEffect`, создает элемент управления `Label` и задает для `Label` определенные свойства макета. Затем он создает тень, задавая свойства [`TextColor`](xref:Xamarin.Forms.Label.TextColor), [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) и [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) для управления цветом и расположением `Label`. После этого `shadowLabel` вставляется со смещением после основного экземпляра `Label`. Если у элемента управления, к которому присоединен эффект, отсутствуют свойства `Control.Layer`, указанная выше функциональность заключается в блок `try`/`catch`. Так как очистка не требуется, метод `OnDetached` не предоставляет реализацию.

## <a name="summary"></a>Сводка

В этой статье было продемонстрировано использование свойств среды CLR для передачи параметров эффекту. Свойства среды CLR можно использовать для определения параметров эффекта, не реагирующих на изменения свойств во время выполнения.

## <a name="related-links"></a>Связанные ссылки

- [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Эффект](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [Эффект тени (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffect)
