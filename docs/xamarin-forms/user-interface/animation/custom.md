---
title: Пользовательские анимации в Xamarin.Forms
description: В этой статье показано, как использовать класс анимации Xamarin. Forms для создания и отмены анимации, синхронизации нескольких анимаций и создания пользовательских анимаций, которые анимировать свойства, которые не анимированы с помощью существующих методов анимации.
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4831ed148e186783667046afd49bf490c666b87b
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93367079"
---
# <a name="custom-animations-in-no-locxamarinforms"></a>Пользовательские анимации в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)

_Класс анимации — это стандартный блок всех Xamarin.Forms анимаций с методами расширения в классе виевекстенсионс, создающими один или несколько объектов анимации. В этой статье показано, как использовать класс анимации для создания и отмены анимации, синхронизации нескольких анимаций и создания пользовательских анимаций, которые анимировать свойства, которые не анимированы с помощью существующих методов анимации._

При создании объекта необходимо указать ряд параметров `Animation` , включая начальное и конечное значения анимируемого свойства, а также обратный вызов, который изменяет значение свойства. `Animation`Объект также может поддерживать коллекцию дочерних анимаций, которые могут быть запущены и синхронизированы. Дополнительные сведения см. в разделе [дочерние анимации](#child-animations).

Выполнение анимации, созданной с помощью [`Animation`](xref:Xamarin.Forms.Animation) класса, который может или не может включать дочерние анимации, достигается путем вызова [ `Commit` ] (xref: Xamarin.Forms . Animation. Commit ( Xamarin.Forms . IAnimatable, System. String, System. UInt32, System. UInt32, Xamarin.Forms . Метод замедления, System. Action {System. Double, System. Boolean}, System. Func {System. Boolean})). Этот метод задает длительность анимации и, среди прочих элементов, обратный вызов, который определяет, следует ли повторять анимацию.

Кроме того, [`Animation`](xref:Xamarin.Forms.Animation) класс имеет `IsEnabled` свойство, которое можно проверить, чтобы определить, была ли анимация отключена операционной системой, например, когда включен режим энергосбережения.

## <a name="create-an-animation"></a>Создание анимации

При создании [`Animation`](xref:Xamarin.Forms.Animation) объекта, как правило, требуется не менее трех параметров, как показано в следующем примере кода:

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

Этот код определяет анимацию [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) свойства [`Image`](xref:Xamarin.Forms.Image) экземпляра по значению 1 до значения 2. Анимированное значение, производное от Xamarin.Forms , передается обратному вызову, указанному в качестве первого аргумента, где он используется для изменения значения `Scale` Свойства.

Анимация начинается с вызова [ `Commit` ] (xref: Xamarin.Forms . Animation. Commit ( Xamarin.Forms . IAnimatable, System. String, System. UInt32, System. UInt32, Xamarin.Forms . Метод замедления, System. Action {System. Double, System. Boolean}, System. Func {System. Boolean})), как показано в следующем примере кода:

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

Обратите внимание, что [ `Commit` ] (xref: Xamarin.Forms . Animation. Commit ( Xamarin.Forms . IAnimatable, System. String, System. UInt32, System. UInt32, Xamarin.Forms . Метод "замедление", System. Action {System. Double, System. Boolean}, System. Func {System. Boolean})) не возвращает `Task` объект. Вместо этого уведомления предоставляются через методы обратного вызова.

В методе указаны следующие аргументы `Commit` :

- Первый аргумент ( *owner* ) определяет владельца анимации. Это может быть визуальный элемент, к которому применяется анимация, или другой визуальный элемент, такой как страница.
- Второй аргумент ( *Name* ) определяет анимацию с именем. Имя объединяется с владельцем для уникальной идентификации анимации. После этого можно использовать эту уникальную идентификацию, чтобы определить, запущена ли анимация ([ `AnimationIsRunning` ] (xref: Xamarin.Forms . AnimationExtensions. Аниматионисруннинг ( Xamarin.Forms . IAnimatable, System. String)) или, чтобы отменить его ([ `AbortAnimation` ] (xref: Xamarin.Forms . AnimationExtensions. Абортаниматион ( Xamarin.Forms . IAnimatable, System. String)).
- Третий аргумент ( *Rate* ) указывает количество миллисекунд между вызовами метода обратного вызова, определенного в [`Animation`](xref:Xamarin.Forms.Animation) конструкторе.
- Четвертый аргумент ( *Длина* ) указывает длительность анимации в миллисекундах.
- Пятый аргумент ( *замедление* ) определяет функцию плавности, используемую в анимации. Кроме того, функция плавности может быть указана в качестве аргумента для [`Animation`](xref:Xamarin.Forms.Animation) конструктора. Дополнительные сведения о функциях плавности см. в разделе [функции плавности](~/xamarin-forms/user-interface/animation/easing.md).
- Шестой аргумент ( *Finished* ) — это обратный вызов, который будет выполнен после завершения анимации. Этот обратный вызов принимает два аргумента с первым аргументом, указывающим конечное значение, а второй аргумент — значением `bool` , `true` если анимация была отменена. Кроме того, *завершенный* обратный вызов можно указать в качестве аргумента для [`Animation`](xref:Xamarin.Forms.Animation) конструктора. Однако при использовании одной анимации *, если* обратные вызовы задаются как в `Animation` конструкторе, так и в `Commit` методе, будет выполнен только обратный вызов, указанный в `Commit` методе.
- Седьмой аргумент ( *Repeat* ) — это обратный вызов, позволяющий повторять анимацию. Он вызывается в конце анимации и возвращает `true` значение, указывающее, что анимация должна повторяться.

Общий эффект заключается в создании анимации, которая увеличивает [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) свойство [`Image`](xref:Xamarin.Forms.Image) от 1 до 2, более 2 секунд (2000 миллисекунд) с помощью [`Linear`](xref:Xamarin.Forms.Easing.Linear) функции плавности. При каждом завершении анимации ее `Scale` свойство сбрасывается в 1, а анимация повторяется.

> [!NOTE]
> Параллельная анимация, которая выполняется независимо друг от друга, может быть создана путем создания `Animation` объекта для каждой анимации, а затем вызова `Commit` метода для каждой анимации.

### <a name="child-animations"></a>Дочерние анимации

[`Animation`](xref:Xamarin.Forms.Animation)Класс также поддерживает дочерние анимации, включающие создание `Animation` объекта, к которому `Animation` добавляются другие объекты. Это позволяет выполнять и синхронизировать ряд анимаций. В следующем примере кода показано создание и выполнение дочерних анимаций:

```csharp
var parentAnimation = new Animation ();
var scaleUpAnimation = new Animation (v => image.Scale = v, 1, 2, Easing.SpringIn);
var rotateAnimation = new Animation (v => image.Rotation = v, 0, 360);
var scaleDownAnimation = new Animation (v => image.Scale = v, 2, 1, Easing.SpringOut);

parentAnimation.Add (0, 0.5, scaleUpAnimation);
parentAnimation.Add (0, 1, rotateAnimation);
parentAnimation.Add (0.5, 1, scaleDownAnimation);

parentAnimation.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

Кроме того, пример кода можно написать более кратко, как показано в следующем примере кода:

```csharp
new Animation {
    { 0, 0.5, new Animation (v => image.Scale = v, 1, 2) },
    { 0, 1, new Animation (v => image.Rotation = v, 0, 360) },
    { 0.5, 1, new Animation (v => image.Scale = v, 2, 1) }
    }.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

В обоих примерах кода создается родительский [`Animation`](xref:Xamarin.Forms.Animation) объект, в который `Animation` затем добавляются дополнительные объекты. Первые два аргумента для [ `Add` ] (xref: Xamarin.Forms . Анимация. Add (System. Double, System. Double, Xamarin.Forms . Анимация). метод указывает, когда следует начинать и завершать дочерние анимации. Значения аргумента должны находиться в диапазоне от 0 до 1 и представлять относительный период в родительской анимации, которая будет активной для указанной дочерней анимации. Таким образом, в этом примере `scaleUpAnimation` для первой половины анимации будет активен, `scaleDownAnimation` а для второй половины анимации будет активным, а в течение `rotateAnimation` всей длительности будет активным.

Общий эффект заключается в том, что анимация выполняется более 4 секунд (4000 миллисекунд). Объект `scaleUpAnimation` анимируется [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) свойство от 1 до 2, более 2 секунд. `scaleDownAnimation`Затем объект анимируется `Scale` свойство от 2 до 1, в течение 2 секунд. Во время анимации по шкале происходит `rotateAnimation` анимация [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) свойства от 0 до 360 в течение 4 секунд. Обратите внимание, что анимации масштабирования также используют функции плавности. [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn)Функция плавности приводит к тому, что [`Image`](xref:Xamarin.Forms.Image) изначально сжимается до увеличения размера, а [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) функция плавности приводит к тому, что объект `Image` становится меньше, чем его фактический размер в конце всей анимации.

Существует ряд различий между [`Animation`](xref:Xamarin.Forms.Animation) объектом, использующим дочерние анимации, и одним из которых не является:

- При использовании дочерних анимаций функция *завершения* обратного вызова в дочерней анимации показывает, что дочерний элемент завершен, *а обратный вызов,* переданный в `Commit` метод, указывает, что вся анимация завершена.
- При использовании дочерних анимаций возврат `true` из обратного вызова метода *Repeat* в `Commit` методе не приведет к повторению анимации, но анимация продолжит выполняться без новых значений.
- Если включить функцию плавности в `Commit` метод, а функция плавности возвращает значение больше 1, анимация будет прервана. Если функция плавности возвращает значение меньше 0, значение задается в виде фиксации в 0. Чтобы использовать функцию плавности, которая возвращает значение меньше 0 или больше 1, оно должно быть указано в одной из дочерних анимаций, а не в `Commit` методе.

[`Animation`](xref:Xamarin.Forms.Animation)Класс также включает [ `WithConcurrent` ] (xref: Xamarin.Forms . Animation. Висконкуррент ( Xamarin.Forms . Анимации, System. Double, System. Double)), которые можно использовать для добавления дочерних анимаций в родительский `Animation` объект. Однако значения аргументов *Begin* и *Finish* не ограничиваются 0 и 1, но только эта часть дочерней анимации, соответствующая диапазону от 0 до 1, будет активной. Например, если в `WithConcurrent` вызове метода определена дочерняя анимация, нацеленная на [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) свойство от 1 до 6, но со значениями *Begin* и *Finish* , равными-2 и 3, *Начальное* значение-2 соответствует `Scale` значению 1, а значение *окончания* 3 соответствует `Scale` значению 6. Поскольку значения за пределами диапазона 0 и 1 не играют никакой части анимации, `Scale` свойство будет анимироваться только от 3 до 6.

## <a name="cancel-an-animation"></a>Отмена анимации

Приложение может отменить анимацию с помощью вызова [ `AbortAnimation` ] (xref: Xamarin.Forms . AnimationExtensions. Абортаниматион ( Xamarin.Forms . Метод расширения IAnimatable, System. String), как показано в следующем примере кода:

```csharp
this.AbortAnimation ("SimpleAnimation");
```

Обратите внимание, что анимации однозначно определяются сочетанием владельца анимации и имени анимации. Поэтому необходимо указать владельца и имя, указанные при запуске анимации, чтобы отменить анимацию. Поэтому в примере кода немедленно отменяется анимация с именем `SimpleAnimation` , принадлежащая странице.

## <a name="create-a-custom-animation"></a>Создание пользовательской анимации

Приведенные здесь примеры демонстрируют анимацию, которая может быть так же достигнута с помощью методов в [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) классе. Однако преимущество класса заключается в том [`Animation`](xref:Xamarin.Forms.Animation) , что он имеет доступ к методу обратного вызова, который выполняется при изменении анимированного значения. Это позволяет обратному вызову реализовать любую нужную анимацию. Например, следующий пример кода анимирует [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) свойство страницы, присвоив ему [`Color`](xref:Xamarin.Forms.Color) значения, создаваемые [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) методом, со значениями оттенка от 0 до 1:

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

Итоговая анимация обеспечивает внешний вид фона страницы через цвета Радуга.

Дополнительные примеры создания сложных анимаций, включая анимацию кривой Безье, см. в [главе 22 статьи](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf) [Создание мобильных приложений Xamarin.Forms с помощью ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="create-a-custom-animation-extension-method"></a>Создание пользовательского метода расширения анимации

Методы расширения в [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) классе позволяют анимировать свойство из текущего значения до указанного значения. Это затрудняет создание, например, `ColorTo` метод анимации, который можно использовать для анимации цвета из одного значения на другое, поскольку:

- Единственным [`Color`](xref:Xamarin.Forms.Color) свойством [`VisualElement`](xref:Xamarin.Forms.VisualElement) , определенным классом [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) , является, которое не всегда является требуемым `Color` свойством для анимации.
- Часто текущее значение [`Color`](xref:Xamarin.Forms.Color) свойства — [`Color.Default`](xref:Xamarin.Forms.Color.Default) , которое не является реальным цветом и не может использоваться в вычислениях интерполяции.

Решение этой проблемы заключается в том, что метод не `ColorTo` предназначен для конкретного [`Color`](xref:Xamarin.Forms.Color) Свойства. Вместо этого он может быть написан с помощью метода обратного вызова, который передает интерполяцию `Color` значения обратно вызывающему объекту. Кроме того, метод принимает начальные и конечные `Color` аргументы.

`ColorTo`Метод можно реализовать как метод расширения, который использует [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) метод в [`AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions) классе для предоставления его функциональных возможностей. Это обусловлено тем, что `Animate` метод можно использовать для назначения свойств, которые не принадлежат типу `double` , как показано в следующем примере кода:

```csharp
public static class ViewExtensions
{
  public static Task<bool> ColorTo(this VisualElement self, Color fromColor, Color toColor, Action<Color> callback, uint length = 250, Easing easing = null)
  {
    Func<double, Color> transform = (t) =>
      Color.FromRgba(fromColor.R + t * (toColor.R - fromColor.R),
                     fromColor.G + t * (toColor.G - fromColor.G),
                     fromColor.B + t * (toColor.B - fromColor.B),
                     fromColor.A + t * (toColor.A - fromColor.A));
    return ColorAnimation(self, "ColorTo", transform, callback, length, easing);
  }

  public static void CancelAnimation(this VisualElement self)
  {
    self.AbortAnimation("ColorTo");
  }

  static Task<bool> ColorAnimation(VisualElement element, string name, Func<double, Color> transform, Action<Color> callback, uint length, Easing easing)
  {
    easing = easing ?? Easing.Linear;
    var taskCompletionSource = new TaskCompletionSource<bool>();

    element.Animate<Color>(name, transform, callback, 16, length, easing, (v, c) => taskCompletionSource.SetResult(c));
    return taskCompletionSource.Task;
  }
}
```

[`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*)Метод требует аргумент *преобразования* , который является методом обратного вызова. Входные данные для этого обратного вызова всегда находятся в `double` диапазоне от 0 до 1. Таким образом, `ColorTo` метод определяет собственное преобразование `Func` , которое принимает от `double` 0 до 1 и возвращает [`Color`](xref:Xamarin.Forms.Color) значение, соответствующее этому значению. `Color`Значение вычисляется путем интерполяции [`R`](xref:Xamarin.Forms.Color.R) [`G`](xref:Xamarin.Forms.Color.G) значений,, [`B`](xref:Xamarin.Forms.Color.B) и [`A`](xref:Xamarin.Forms.Color.A) двух предоставленных `Color` аргументов. `Color`Затем значение передается в метод обратного вызова для приложения в определенное свойство.

Такой подход позволяет `ColorTo` методу анимировать любое [`Color`](xref:Xamarin.Forms.Color) свойство, как показано в следующем примере кода:

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

В этом примере кода метод выполняет `ColorTo` анимацию [`TextColor`](xref:Xamarin.Forms.Label.TextColor) свойств и свойства [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) [`Label`](xref:Xamarin.Forms.Label) , `BackgroundColor` Свойства страницы и [`Color`](xref:Xamarin.Forms.BoxView.Color) свойства объекта [`BoxView`](xref:Xamarin.Forms.BoxView) .

## <a name="related-links"></a>Связанные ссылки

- [Пользовательские анимации (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)
- [API анимации](xref:Xamarin.Forms.Animation)
- [API AnimationExtensions](xref:Xamarin.Forms.AnimationExtensions)