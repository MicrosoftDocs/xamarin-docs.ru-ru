---
title: Особенности платформы
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать и создавать зависящие от платформы.
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/01/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ca78ee1c31e4f6e2089d5860543aabafe1a80c3a
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93366910"
---
# <a name="platform-specifics"></a>Особенности платформы

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

_Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов._

Процесс использования конкретной платформы через XAML или API кода Fluent выглядит следующим образом:

1. Добавьте `xmlns` объявление или `using` директиву для [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration) пространства имен.
1. Добавьте `xmlns` объявление или `using` директиву для пространства имен, содержащего функциональные возможности для конкретной платформы:
    1. В iOS это [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространство имен.
    1. В Android это [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространство имен. Для Android AppCompat это [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) пространство имен.
    1. На универсальная платформа Windows это [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространство имен.
1. Примените конкретную платформу на основе XAML или из кода с помощью `On<T>` API Fluent. Значением параметра `T` может быть [`iOS`](xref:Xamarin.Forms.PlatformConfiguration.iOS) [`Android`](xref:Xamarin.Forms.PlatformConfiguration.Android) типы, или [`Windows`](xref:Xamarin.Forms.PlatformConfiguration.Windows) из [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration) пространства имен.

> [!NOTE]
> Обратите внимание, что попытка использования конкретной платформы на платформе, где она недоступна, не приведет к ошибке. Вместо этого код будет выполняться без применения конкретной платформы.

Зависящие от платформы значения, используемые `On<T>` API кода Fluent, возвращающие [`IPlatformElementConfiguration`](xref:Xamarin.Forms.IPlatformElementConfiguration`2) объекты. Это позволяет вызывать несколько конкретных платформ для одного и того же объекта с помощью каскадного метода.

Дополнительные сведения о конкретных платформах, предоставляемых Xamarin.Forms , см. в [разделе особенности платформы iOS](~/xamarin-forms/platform/ios/index.md), [особенности платформы Android](~/xamarin-forms/platform/android/index.md)и [особенности платформы Windows](~/xamarin-forms/platform/windows/index.md).

## <a name="creating-platform-specifics"></a>Создание особенностей платформы

Поставщики могут создавать собственные платформы с применением эффектов. В результате обеспечивается конкретная функциональность, которая затем предоставляется через зависящую от платформы. Результатом является результат, который может быть проще в использовании XAML и через API кода Fluent.

Процесс создания конкретной платформы выглядит следующим образом:

1. Реализуйте конкретную функциональность в качестве результата. Дополнительные сведения см. [в разделе Создание влияния](~/xamarin-forms/app-fundamentals/effects/creating.md).
1. Создайте класс для конкретной платформы, который предоставит результат. Дополнительные сведения см. [в разделе Создание класса Platform-Specific](#creating-a-platform-specific-class).
1. В классе, ориентированном на платформу, реализуйте присоединенное свойство, чтобы разрешить использование платформы с использованием XAML. Дополнительные сведения см. в разделе [Добавление присоединенного свойства](#adding-an-attached-property).
1. В классе, ориентированном на платформу, реализуйте методы расширения, чтобы обеспечить использование платформы для использования через API кода Fluent. Дополнительные сведения см. в разделе [Добавление методов расширения](#adding-extension-methods).
1. Измените реализацию эффектов таким образом, чтобы результат был применен только в том случае, если на той же платформе была вызвана та же платформа, что и в результате. Дополнительные сведения см. [в разделе Создание результата](#creating-the-effect).

Результатом предоставления результата в качестве конкретной платформы является то, что этот результат можно легко использовать с помощью XAML и API кода Fluent.

> [!NOTE]
> Это предусмотрено, что поставщики будут использовать этот метод для создания собственных платформ для простоты потребления пользователями. Хотя пользователи могут создавать свои собственные платформы, следует отметить, что для этого требуется больше кода, чем создание и использование влияния.

[Пример приложения](/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific) демонстрирует `Shadow` конкретную платформу, которая добавляет тень к тексту, отображаемому [`Label`](xref:Xamarin.Forms.Label) элементом управления:

![Теневое Platform-Specific](images/screenshots.png)

В [примере приложения](/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific) `Shadow` для каждой платформы реализована конкретная платформа для простоты понимания. Тем не менее, помимо реализации влияния на конкретную платформу, реализация теневого класса в значительной степени идентична для каждой платформы. Поэтому в этом руководством рассматриваются реализация теневого класса и связанная с ней воздействие на одну платформу.

Дополнительные сведения о влиянии см. [в разделе Настройка элементов управления с помощью эффектов](~/xamarin-forms/app-fundamentals/effects/index.md).

### <a name="creating-a-platform-specific-class"></a>Создание класса, зависящего от платформы

Конкретная платформа создается как `public static` класс:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
  public static Shadow
  {
    ...
  }
}
```

В следующих разделах обсуждается реализация `Shadow` конкретного и связанного с платформой воздействия.

#### <a name="adding-an-attached-property"></a>Добавление присоединенного свойства

Присоединенное свойство необходимо добавить к `Shadow` конкретной платформе, чтобы разрешить использование с помощью XAML:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        const string EffectName = "MyCompany.LabelShadowEffect";

        public static readonly BindableProperty IsShadowedProperty =
            BindableProperty.CreateAttached("IsShadowed",
                                            typeof(bool),
                                            typeof(Shadow),
                                            false,
                                            propertyChanged: OnIsShadowedPropertyChanged);

        public static bool GetIsShadowed(BindableObject element)
        {
            return (bool)element.GetValue(IsShadowedProperty);
        }

        public static void SetIsShadowed(BindableObject element, bool value)
        {
            element.SetValue(IsShadowedProperty, value);
        }

        ...

        static void OnIsShadowedPropertyChanged(BindableObject element, object oldValue, object newValue)
        {
            if ((bool)newValue)
            {
                AttachEffect(element as FormsElement);
            }
            else
            {
                DetachEffect(element as FormsElement);
            }
        }

        static void AttachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || controller.EffectIsAttached(EffectName))
            {
                return;
            }
            element.Effects.Add(Effect.Resolve(EffectName));
        }

        static void DetachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || !controller.EffectIsAttached(EffectName))
            {
                return;
            }

            var toRemove = element.Effects.FirstOrDefault(e => e.ResolveId == Effect.Resolve(EffectName).ResolveId);
            if (toRemove != null)
            {
                element.Effects.Remove(toRemove);
            }
        }
    }
}
```

`IsShadowed`Присоединенное свойство используется для добавления `MyCompany.LabelShadowEffect` результата к элементу управления, к которому присоединен класс, и удаления его из него `Shadow` . Это присоединенное свойство регистрирует метод `OnIsShadowedPropertyChanged`, который будет выполняться при изменении значения свойства. В свою очередь, этот метод вызывает `AttachEffect` метод или, `DetachEffect` чтобы добавить или удалить результат на основе значения `IsShadowed` присоединенного свойства. Этот результат добавляется в элемент управления или удаляется из него путем изменения коллекции элемента управления [`Effects`](xref:Xamarin.Forms.Element.Effects) .

> [!NOTE]
> Обратите внимание, что этот результат разрешается путем указания значения, которое представляет собой объединение имени группы разрешения и уникального идентификатора, указанного в реализации действия. Дополнительные сведения см. [в разделе Создание влияния](~/xamarin-forms/app-fundamentals/effects/creating.md).

Дополнительные сведения о присоединенных свойствах см. в разделе [Присоединенные свойства](~/xamarin-forms/xaml/attached-properties.md).

#### <a name="adding-extension-methods"></a>Добавление методов расширения

Методы расширения должны быть добавлены к `Shadow` конкретной платформе, чтобы разрешить использование через API кода Fluent:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        ...
        public static bool IsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config)
        {
            return GetIsShadowed(config.Element);
        }

        public static IPlatformElementConfiguration<iOS, FormsElement> SetIsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config, bool value)
        {
            SetIsShadowed(config.Element, value);
            return config;
        }
        ...
    }
}
```

`IsShadowed` `SetIsShadowed` Методы расширения и вызывают метод доступа get и Set для `IsShadowed` присоединенного свойства соответственно. Каждый метод расширения работает с `IPlatformElementConfiguration<iOS, FormsElement>` типом, который указывает, что конкретная платформа может вызываться для [`Label`](xref:Xamarin.Forms.Label) экземпляров из iOS.

#### <a name="creating-the-effect"></a>Создание результата

`Shadow`Зависящая от платформы добавляет в `MyCompany.LabelShadowEffect` [`Label`](xref:Xamarin.Forms.Label) и удаляет ее. В следующем примере кода показана реализация класса `LabelShadowEffect` для проекта на платформе iOS:

```csharp
[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace ShadowPlatformSpecific.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            UpdateShadow();
        }

        protected override void OnDetached()
        {
        }

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (args.PropertyName == Shadow.IsShadowedProperty.PropertyName)
            {
                UpdateShadow();
            }
        }

        void UpdateShadow()
        {
            try
            {
                if (((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.CornerRadius = 5;
                    Control.Layer.ShadowColor = UIColor.Black.CGColor;
                    Control.Layer.ShadowOffset = new CGSize(5, 5);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
                else if (!((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.ShadowOpacity = 0;
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

`UpdateShadow`Метод задает `Control.Layer` свойства для создания тени, при условии, что для `IsShadowed` присоединенного свойства задано значение `true` , и при условии, что `Shadow` конкретная платформа была вызвана на той же платформе, для которой реализуется эффект. Эта проверка выполняется с помощью `OnThisPlatform` метода.

Если `Shadow.IsShadowed` значение присоединенного свойства изменяется во время выполнения, то эффект должен ответить, удалив тень. Поэтому переопределенная версия `OnElementPropertyChanged` метода используется для реагирования на изменение свойства, доступного для привязки, путем вызова `UpdateShadow` метода.

Дополнительные сведения о создании эффектов см. в разделе [Создание эффектов](~/xamarin-forms/app-fundamentals/effects/creating.md) и [Передача параметров эффектов в качестве вложенных свойств](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md).

### <a name="consuming-the-platform-specific"></a>Использование конкретной платформы

`Shadow`Конкретная платформа используется в XAML путем установки `Shadow.IsShadowed` для присоединенного свойства `boolean` значения:

```xaml
<ContentPage xmlns:ios="clr-namespace:MyCompany.Forms.PlatformConfiguration.iOS" ...>
  ...
  <Label Text="Label Shadow Effect" ios:Shadow.IsShadowed="true" ... />
  ...
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using MyCompany.Forms.PlatformConfiguration.iOS;

...

shadowLabel.On<iOS>().SetIsShadowed(true);
```

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [ШадовплатформспеЦифик (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific)
- [Особенности платформы iOS](~/xamarin-forms/platform/ios/index.md)
- [Особенности платформы Android](~/xamarin-forms/platform/android/index.md)
- [Особенности платформы Windows](~/xamarin-forms/platform/windows/index.md)
- [Настройка элементов управления с помощью эффектов](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Вложенные свойства](~/xamarin-forms/xaml/attached-properties.md)
- [API Платформконфигуратион](xref:Xamarin.Forms.PlatformConfiguration)