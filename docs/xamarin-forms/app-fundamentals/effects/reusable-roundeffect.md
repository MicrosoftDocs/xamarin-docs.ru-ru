---
title: Многоразовый RoundEffect в Xamarin.Forms
description: RoundEffect — это многоразовый эффект, который можно применить к любому элементу управления, производному от VisualElement, для отрисовки этого элемента управления в виде круга.
ms.prod: xamarin
ms.assetid: B5DE7507-B565-4EE5-9897-27E5733FD173
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 10/27/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 74df5809576745903f978862ddc8a1e98aa1c973
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374268"
---
# <a name="xamarinforms-reusable-roundeffect"></a>Многоразовый RoundEffect в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/effects-roundeffect/)

> [!IMPORTANT]
> Больше не нужно использовать `RoundEffect` для отрисовки элемента управления в виде окружности. Последний рекомендуемый подход заключается в обрезке элемента управления с помощью `EllipseGeometry`. Дополнительные сведения см. в разделе [Обрезка с помощью класса Geometry](~/xamarin-forms/user-interface/shapes/geometries.md#clip-with-a-geometry).

RoundEffect упрощает отрисовку любого элемента управления, производного от `VisualElement`, в виде круга. Этот эффект можно использовать для создания круглых изображений, кнопок или других элементов управления:

[![Снимки экрана RoundEffect в iOS и Android](example-roundeffect-images/round-effect-cropped.png)](example-roundeffect-images/round-effect.png#lightbox)

## <a name="create-a-shared-routingeffect"></a>Создание общего RoutingEffect

Для создания кроссплатформенного эффекта нужно создать класс эффектов в общем проекте. Пример приложения создает пустой класс `RoundEffect`, производный от класса `RoutingEffect`:

```csharp
public class RoundEffect : RoutingEffect
{
    public RoundEffect() : base($"Xamarin.{nameof(RoundEffect)}")
    {
    }
}
```

Этот класс позволяет общему проекту разрешать ссылки на данный эффект в коде или XAML, но не предоставляет никаких функциональных возможностей. Эффект должен иметь реализации для каждой платформы.

## <a name="implement-the-android-effect"></a>Реализация эффекта для Android

Проект платформы Android определяет класс `RoundEffect`, производный от `PlatformEffect`. Этот класс помечен атрибутами `assembly`, которые позволяют Xamarin.Forms разрешать класс эффектов:

```csharp
[assembly: ResolutionGroupName("Xamarin")]
[assembly: ExportEffect(typeof(RoundEffectDemo.Droid.RoundEffect), nameof(RoundEffectDemo.Droid.RoundEffect))]
namespace RoundEffectDemo.Droid
{
    public class RoundEffect : PlatformEffect
    {
        // ...
    }
}
```

Платформа Android использует концепцию `OutlineProvider` для определения границ элемента управления. Пример проекта включает класс `CornerRadiusProvider`, производный от класса `ViewOutlineProvider`:

```csharp
class CornerRadiusOutlineProvider : ViewOutlineProvider
{
    Element element;

    public CornerRadiusOutlineProvider(Element formsElement)
    {
        element = formsElement;
    }

    public override void GetOutline(Android.Views.View view, Outline outline)
    {
        float scale = view.Resources.DisplayMetrics.Density;
        double width = (double)element.GetValue(VisualElement.WidthProperty) * scale;
        double height = (double)element.GetValue(VisualElement.HeightProperty) * scale;
        float minDimension = (float)Math.Min(height, width);
        float radius = minDimension / 2f;
        Rect rect = new Rect(0, 0, (int)width, (int)height);
        outline.SetRoundRect(rect, radius);
    }
}
```

Этот класс использует свойства `Width` и `Height` экземпляра `Element` Xamarin.Forms для вычисления радиуса, который равен половине самого короткого размера.

После того как поставщик структуры определен, класс `RoundEffect` может использовать его для реализации этого эффекта:

```csharp
public class RoundEffect : PlatformEffect
{
    ViewOutlineProvider originalProvider;
    Android.Views.View effectTarget;

    protected override void OnAttached()
    {
        try
        {
            effectTarget = Control ?? Container;
            originalProvider = effectTarget.OutlineProvider;
            effectTarget.OutlineProvider = new CornerRadiusOutlineProvider(Element);
            effectTarget.ClipToOutline = true;
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Failed to set corner radius: {ex.Message}");
        }
    }

    protected override void OnDetached()
    {
        if(effectTarget != null)
        {
            effectTarget.OutlineProvider = originalProvider;
            effectTarget.ClipToOutline = false;
        }
    }
}
```

Метод `OnAttached` вызывается, когда эффект присоединяется к элементу. Существующий объект `OutlineProvider` сохраняется, чтобы его можно было восстановить при отсоединении этого эффекта. Новый экземпляр `CornerRadiusOutlineProvider` используется в качестве `OutlineProvider`, а для `ClipToOutline` устанавливается значение true для обрезки выступающих элементов по границе структуры.

Метод `OnDetatched` вызывается, когда эффект удаляется из элемента и восстанавливает исходное значение `OutlineProvider`.

> [!NOTE]
> В зависимости от типа элемента свойство `Control` может быть или не быть равно null. Если свойство `Control` не равно null, скругленные углы можно применить непосредственно к элементу управления. Однако при значении null скругленные углы должны применяться к объекту `Container`. Поле `effectTarget` позволяет применять эффект к соответствующему объекту.

## <a name="implement-the-ios-effect"></a>Реализация эффекта для iOS

Проект платформы iOS определяет класс `RoundEffect`, производный от `PlatformEffect`. Этот класс помечен атрибутами `assembly`, которые позволяют Xamarin.Forms разрешать класс эффектов:

```csharp
[assembly: ResolutionGroupName("Xamarin")]
[assembly: ExportEffect(typeof(RoundEffectDemo.iOS.RoundEffect), nameof(RoundEffectDemo.iOS.RoundEffect))]
namespace RoundEffectDemo.iOS
{
    public class RoundEffect : PlatformEffect
    {
        // ...
    }
```

В iOS элементы управления имеют свойство `Layer`, имеющее свойство `CornerRadius`. Реализация класса `RoundEffect` в iOS вычисляет соответствующий радиус угла и обновляет свойство `CornerRadius` слоя:

```csharp
public class RoundEffect : PlatformEffect
{
    nfloat originalRadius;
    UIKit.UIView effectTarget;

    protected override void OnAttached()
    {
        try
        {
            effectTarget = Control ?? Container;
            originalRadius = effectTarget.Layer.CornerRadius;
            effectTarget.ClipsToBounds = true;
            effectTarget.Layer.CornerRadius = CalculateRadius();
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Failed to set corner radius: {ex.Message}");
        }
    }

    protected override void OnDetached()
    {
        if (effectTarget != null)
        {
            effectTarget.ClipsToBounds = false;
            if (effectTarget.Layer != null)
            {
                effectTarget.Layer.CornerRadius = originalRadius;
            }
        }
    }

    float CalculateRadius()
    {
        double width = (double)Element.GetValue(VisualElement.WidthRequestProperty);
        double height = (double)Element.GetValue(VisualElement.HeightRequestProperty);
        float minDimension = (float)Math.Min(height, width);
        float radius = minDimension / 2f;

        return radius;
    }
}
```

Метод `CalculateRadius` вычисляет радиус на основе минимального размера `Element` Xamarin.Forms. Метод `OnAttached` вызывается, когда эффект присоединяется к элементу управления и обновляет свойство `CornerRadius` слоя. Он задает для свойства `ClipToBounds` значение `true`, поэтому выступающие элементы обрезаются по границам элемента управления. Метод `OnDetatched` вызывается, когда эффект удаляется из элемента и резервирует эти изменения, восстанавливая первоначальный радиус угла.

> [!NOTE]
> В зависимости от типа элемента свойство `Control` может быть или не быть равно null. Если свойство `Control` не равно null, скругленные углы можно применить непосредственно к элементу управления. Однако при значении null скругленные углы должны применяться к объекту `Container`. Поле `effectTarget` позволяет применять эффект к соответствующему объекту.

## <a name="consume-the-effect"></a>Использование эффекта

После реализации этого эффекта на разных платформах он может использоваться элементами управления Xamarin.Forms. Обычное приложение `RoundEffect` делает объект `Image` круглым. В следующем коде XAML показано применение эффекта к экземпляру `Image`:

```xaml
<Image Source=outdoors"
       HeightRequest="100"
       WidthRequest="100">
    <Image.Effects>
        <local:RoundEffect />
    </Image.Effects>
</Image>
```

Этот эффект можно также применить в коде:

```csharp
var image = new Image
{
    Source = ImageSource.FromFile("outdoors"),
    HeightRequest = 100,
    WidthRequest = 100
};
image.Effects.Add(new RoundEffect());
```

Класс `RoundEffect` можно применить к любому элементу управления, производному от `VisualElement`.

> [!NOTE]
> Чтобы эффект вычислил правильный радиус, элемент управления, к которому он применяется, должен иметь явно заданный размер. То есть должны быть определены свойства `HeightRequest` и `WidthRequest`. Если затронутый элемент управления отображается в `StackLayout`, его свойство `HorizontalOptions` не должно использовать одно из значений **Expand**, например `LayoutOptions.CenterAndExpand`, или измерения будут неточными.

## <a name="related-links"></a>Связанные ссылки

- [Пример приложения RoundEffect](/samples/xamarin/xamarin-forms-samples/effects-roundeffect/)
- [Введение в эффекты](~/xamarin-forms/app-fundamentals/effects/introduction.md)
- [Создание эффекта](~/xamarin-forms/app-fundamentals/effects/creating.md)