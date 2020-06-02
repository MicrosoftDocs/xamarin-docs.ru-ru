---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0ed11ccacec73dd3c5c220c41bffd090af6c5b27
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139390"
---
# <a name="creating-an-effect"></a>Создание эффекта

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-focuseffect)

_Эффекты упрощают настройку элемента управления. В этой статье показано, как создать эффект, который изменяет цвет фона элемента управления Entry, когда элемент управления получает фокус._

Процесс создания эффекта в каждом проекте для конкретной платформы выглядит следующим образом:

1. Создайте подкласс класса `PlatformEffect`.
1. Переопределите метод `OnAttached` и напишите логику для настройки элемента управления.
1. Переопределите метод `OnDetached` и напишите логику для очистки настройки элемента управления, если это необходимо.
1. Добавьте к классу эффекта атрибут [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute). Этот атрибут задает пространство имен для эффектов в рамках всей организации, что предотвращает конфликты с другими эффектами, имеющими такое же имя. Обратите внимание, что этот атрибут можно применить в проекте только один раз.
1. Добавьте к классу эффекта атрибут [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute). Этот атрибут регистрирует эффект с уникальным идентификатором, который Xamarin.Forms использует вместе с именем группы для поиска эффекта перед его применением к элементу управления. Атрибут принимает два параметра — имя типа эффекта и уникальную строку, которая будет использоваться для поиска эффекта перед его применением к элементу управления.

После этого эффект можно использовать посредством его присоединения к соответствующему элементу управления.

> [!NOTE]
> Предоставлять эффект в проекте каждой платформы необязательно. При попытке использовать эффект, когда он не зарегистрирован, возвращается значение, отличное от null, которое не выполняет никаких действий.

Этот пример приложения демонстрирует `FocusEffect`, изменяющий цвет фона элемента управления при получении фокуса. На следующей схеме показаны обязанности каждого проекта в примере приложения, а также связи между ними:

![](creating-images/focus-effect.png "Focus Effect Project Responsibilities")

Элемент управления [`Entry`](xref:Xamarin.Forms.Entry) в `HomePage` настраивается с помощью класса `FocusEffect` в каждом проекте для конкретной платформы. Каждый класс `FocusEffect` является производным от класса `PlatformEffect` для каждой платформы. Это приводит к тому, что элемент управления `Entry` отрисовывается с использованием цвета фона для конкретной платформы, который меняется при получении фокуса этим элементом управления, как показано на следующих снимках экрана:

![](creating-images/screenshots-1.png "Focus Effect on each Platform")
![](creating-images/screenshots-2.png "Focus Effect on each Platform")

## <a name="creating-the-effect-on-each-platform"></a>Создание эффекта на каждой платформе

В следующих разделах рассматривается реализация класса `FocusEffect` для каждой платформы.

## <a name="ios-project"></a>Проект на платформе iOS

В следующем примере кода показана реализация класса `FocusEffect` для проекта на платформе iOS:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(EffectsDemo.iOS.FocusEffect), nameof(EffectsDemo.iOS.FocusEffect))]
namespace EffectsDemo.iOS
{
    public class FocusEffect : PlatformEffect
    {
        UIColor backgroundColor;

        protected override void OnAttached ()
        {
            try {
                Control.BackgroundColor = backgroundColor = UIColor.FromRGB (204, 153, 255);
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }

        protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged (args);

            try {
                if (args.PropertyName == "IsFocused") {
                    if (Control.BackgroundColor == backgroundColor) {
                        Control.BackgroundColor = UIColor.White;
                    } else {
                        Control.BackgroundColor = backgroundColor;
                    }
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

Метод `OnAttached` задает для свойства `BackgroundColor` элемента управления светло-сиреневый цвет с помощью метода `UIColor.FromRGB`, а также сохраняет этот цвет в поле. Если у элемента управления, к которому присоединен эффект, отсутствует свойство `BackgroundColor`, указанная выше функциональность заключается в блок `try`/`catch`. Так как очистка не требуется, метод `OnDetached` не предоставляет реализацию.

Переопределение `OnElementPropertyChanged` реагирует на изменения привязываемого свойства в элементе управления Xamarin.Forms. При изменении свойства [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused) значение свойства `BackgroundColor` элемента управления изменяется на белый цвет, если элемент управления находится в фокусе, в противном случае оно меняется на светло-сиреневый. Если у элемента управления, к которому присоединен эффект, отсутствует свойство `BackgroundColor`, указанная выше функциональность заключается в блок `try`/`catch`.

## <a name="android-project"></a>Проект на платформе Android

В следующем примере кода показана реализация класса `FocusEffect` для проекта на платформе Android:

```csharp
using System;
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(EffectsDemo.Droid.FocusEffect), nameof(EffectsDemo.Droid.FocusEffect))]
namespace EffectsDemo.Droid
{
    public class FocusEffect : PlatformEffect
    {
        Android.Graphics.Color originalBackgroundColor = new Android.Graphics.Color(0, 0, 0, 0);
        Android.Graphics.Color backgroundColor;

        protected override void OnAttached()
        {
            try
            {
                backgroundColor = Android.Graphics.Color.LightGreen;
                Control.SetBackgroundColor(backgroundColor);
            }
            catch (Exception ex)
            {
                Console.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached()
        {
        }

        protected override void OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);
            try
            {
                if (args.PropertyName == "IsFocused")
                {
                    if (((Android.Graphics.Drawables.ColorDrawable)Control.Background).Color == backgroundColor)
                    {
                        Control.SetBackgroundColor(originalBackgroundColor);
                    }
                    else
                    {
                        Control.SetBackgroundColor(backgroundColor);
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

Метод `OnAttached` вызывает метод `SetBackgroundColor`, чтобы задать цвет фона элемента управления на светло-зеленый, а также сохраняет этот цвет в поле. Если у элемента управления, к которому присоединен эффект, отсутствует свойство `SetBackgroundColor`, указанная выше функциональность заключается в блок `try`/`catch`. Так как очистка не требуется, метод `OnDetached` не предоставляет реализацию.

Переопределение `OnElementPropertyChanged` реагирует на изменения привязываемого свойства в элементе управления Xamarin.Forms. При изменении свойства [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused) цвет фона элемента управления изменяется на белый цвет, если элемент управления находится в фокусе, в противном случае он меняется на светло-зеленый. Если у элемента управления, к которому присоединен эффект, отсутствует свойство `BackgroundColor`, указанная выше функциональность заключается в блок `try`/`catch`.

## <a name="universal-windows-platform-projects"></a>Проекты для универсальной платформы Windows

В следующем коде показана реализация класса `FocusEffect` для проекта на универсальной платформе Windows (UWP):

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(EffectsDemo.UWP.FocusEffect), nameof(EffectsDemo.UWP.FocusEffect))]
namespace EffectsDemo.UWP
{
    public class FocusEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            try
            {
                (Control as Windows.UI.Xaml.Controls.Control).Background = new SolidColorBrush(Colors.Cyan);
                (Control as FormsTextBox).BackgroundFocusBrush = new SolidColorBrush(Colors.White);
            }
            catch (Exception ex)
            {
                Debug.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached()
        {
        }
    }
}
```

Метод `OnAttached` задает в качестве значения свойства `Background` элемента управления голубой цвет, а для свойства `BackgroundFocusBrush` — белый цвет. Если у элемента управления, к которому присоединен эффект, отсутствуют эти свойства, указанная выше функциональность заключается в блок `try`/`catch`. Так как очистка не требуется, метод `OnDetached` не предоставляет реализацию.

## <a name="consuming-the-effect"></a>Использование эффекта

Процесс использования эффекта из проекта библиотеки .NET Standard Xamarin.Forms или общей библиотеки выглядит следующим образом:

1. Объявите элемент управления, который будет настраиваться эффектом.
1. Присоедините эффект к элементу управления, добавив его в коллекцию [`Effects`](xref:Xamarin.Forms.Element.Effects) элемента управления.

> [!NOTE]
> Экземпляр эффекта можно присоединить только к одному элементу управления. Таким образом, чтобы использовать эффект в двух элементах управления, его нужно разрешить дважды.

## <a name="consuming-the-effect-in-xaml"></a>Использование эффекта в XAML

В следующем примере кода XAML показан элемент управления [`Entry`](xref:Xamarin.Forms.Entry), к которому присоединяется `FocusEffect`:

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

Класс `FocusEffect` в библиотеке .NET Standard поддерживает использование эффекта в XAML, как показано в следующем примере кода:

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ($"MyCompany.{nameof(FocusEffect)}")
    {
    }
}
```

Класс `FocusEffect` делит на подклассы класс [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect), который представляет независимый от платформы эффект, являющийся оболочкой для внутреннего эффекта, который обычно зависит от платформы. Класс `FocusEffect` вызывает конструктор базового класса, передавая параметр, состоящий из объединения имени группы разрешения (указанного с помощью атрибута [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) в классе эффекта) и уникального идентификатора, указанного с помощью атрибута [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) в классе эффекта. Таким образом, при инициализации [`Entry`](xref:Xamarin.Forms.Entry) во время выполнения в коллекцию [`Effects`](xref:Xamarin.Forms.Element.Effects) элемента управления добавляется новый экземпляр класса `MyCompany.FocusEffect`.

Эффекты также можно присоединить к элементам управления с помощью реакции на событие или присоединенных свойств. Дополнительные сведения о присоединении эффекта к элементу управления с помощью реакции на событие см. в разделе [Повторно используемый EffectBehavior](~/xamarin-forms/app-fundamentals/behaviors/reusable/effect-behavior.md). Дополнительные сведения о присоединении эффекта к элементу управления с помощью присоединенных свойств см. в разделе [Передача параметров эффекту](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md).

## <a name="consuming-the-effect-in-cnum"></a>Использование эффекта в C&num;

В следующем примере кода показан эквивалентный элемент управления [`Entry`](xref:Xamarin.Forms.Entry) на языке C#:

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

`FocusEffect` присоединяется к экземпляру `Entry` посредством добавления эффекта в коллекцию [`Effects`](xref:Xamarin.Forms.Element.Effects) элемента управления, как показано в следующем примере кода:

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ($"MyCompany.{nameof(FocusEffect)}"));
  ...
}
```

[`Effect.Resolve`](xref:Xamarin.Forms.Effect.Resolve(System.String)) возвращает [`Effect`](xref:Xamarin.Forms.Effect) для указанного имени, что является объединением имени группы разрешения (указанного с помощью атрибута [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) в классе эффекта) и уникального идентификатора, указанного с помощью атрибута [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) в классе эффекта. Если платформа не предоставляет эффект, метод `Effect.Resolve` возвращает значение, отличное от `null`.

## <a name="summary"></a>Сводка

В этой статье показано, как создать эффект, который изменяет цвет фона элемента управления [`Entry`](xref:Xamarin.Forms.Entry), когда элемент управления получает фокус.

## <a name="related-links"></a>Связанные ссылки

- [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Эффект](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [Эффект цвета фона (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-backgroundcoloreffect)
- [Эффект фокуса (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-focuseffect)
