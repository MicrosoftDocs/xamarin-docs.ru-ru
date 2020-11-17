---
title: Сводная информация о Главе 21. Transform
description: Создание мобильных приложений с помощью Xamarin.Forms. Сводная информация о Главе 21. Transform
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3642F112-C7FA-4A74-9000-F9087BA89AD9
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cb822c7ac2a05dc9f0d51f51e9737add4395b84d
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374034"
---
# <a name="summary-of-chapter-21-transforms"></a>Сводная информация о Главе 21. Transform

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)

> [!NOTE]
> Эта книга была опубликована весной 2016 года и с тех пор не обновлялась. Многое в этой книге остается ценным, но некоторые материалы устарели, а некоторые разделы перестали быть полностью верными или полными.

Представление Xamarin.Forms появляется на экране в том месте и с тем размером, которые определяются его родительским элементом, обычно производном от `Layout` или `Layout<View>`. Возможность *преобразования* в Xamarin.Forms позволяет изменить расположение, размер и даже ориентацию элемента.

Xamarin.Forms поддерживает три основных типа преобразований:

- *перенос* &mdash; сдвиг объекта по горизонтали или по вертикали;
- *масштабирование* &mdash; изменение размера элемента;
- *вращение* &mdash; поворот элемента вокруг некоторой точки или оси.

В Xamarin.Forms масштабирование выполняется изотропно, то есть в равной мере применяется к высоте и ширине. Поддерживается вращение как в пределах двумерной плоскости экрана, так и в трехмерном пространстве. Преобразование со сдвигом (искажением) не поддерживается, также не предлагается обобщенного матричного преобразования.

Преобразования поддерживаются для восьми свойств типа `double`, которые определены классом `VisualElement`:

- [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)
- [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)
- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)
- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)
- [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)
- [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)
- [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)
- [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)

Все эти свойства подкреплены привязываемыми свойствами. Их можно использовать для привязки данных и применения стилей. [**Глава 22. Анимация**](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter22.md) покажет вам, как применять анимацию для этих свойств, но уже в этой главе вы найдете несколько примеров анимации с применением [таймера](~/xamarin-forms/platform/device.md#devicestarttimer) Xamarin.Forms.

Свойства преобразования влияют только на отображение элемента, но *не влияют* на размещение элемента в макете.

## <a name="the-translation-transform"></a>Преобразование переносом

Отличные от нуля значения свойств [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) и [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) приводят к сдвигу объекта по горизонтали или по вертикали.

Программа [**TranslationDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TranslationDemo) дает возможность поэкспериментировать с этими свойствами на примере двух элементов `Slider`, которые управляют свойствами `TranslationX` и `TranslationY` объекта `Frame`. Преобразование также затрагивает все дочерние элементы этого `Frame`.

### <a name="text-effects"></a>Эффекты текста

Одно из самых распространенных применений для преобразования сдвигом — небольшое смещение отображаемого текста. Это показано в примере [**TextOffsets**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TextOffsets).

[![Снимок экрана с тремя изображениями смещения текста](images/ch21fg03-small.png "Смещение текста")](images/ch21fg03-large.png#lightbox "Смещение текста")

Еще один полезный эффект — создание нескольких копий `Label` для получения трехмерного блока, как показано в примере [**BlockText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BlockText).

### <a name="jumps-and-animations"></a>Прыжки и анимации

Пример [**ButtonJump**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonJump) использует сдвиг для переноса `Button` в ту точку, на которую нажимает пользователь, но основное его назначение — продемонстрировать получение в `Button` пользовательского ввода в том месте, где отображается кнопка.

Пример [**ButtonGlide**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonGlide) работает почти так же, но с таймером для анимации перемещения `Button` в другую точку.

## <a name="the-scale-transform"></a>Преобразование масштабированием

Преобразование [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) может увеличивать или уменьшать отображаемый размер элемента. Значение по умолчанию — 1. Значение 0 приводит к тому, что элемент становится невидимым. При отрицательных значениях элемент отображается развернутым на 180 градусов. Свойство `Scale` не влияет на свойства `Width` и (или) `Height` элемента. Эти значения остаются неизменными.

Вы можете поэкспериментировать со свойством `Scale` в примере [**SimpleScaleDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/SimpleScaleDemo).

Пример [**ButtonScaler**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonScaler) демонстрирует различия между анимированием свойства `Scale` в `Button` и анимированием свойства `FontSize`. Свойство `FontSize` влияет на то, как `Button` воспринимается в макете, а `Scale` — нет.

Пример [**ScaleToSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ScaleToSize) вычислят свойство `Scale`, которое применяется к элементу `Label` для максимально возможного увеличения его размера без выхода за пределы страницы.

### <a name="anchoring-the-scale"></a>Привязка масштаба

Элемент, к которому мы применяли масштабирование в трех предыдущих примера, всегда увеличивался или уменьшался относительно центра этого элемента. Другими словами, он увеличивал и уменьшал размер равномерно во всех направлениях. Центральная точка элемента является единственной, которая сохраняет свое положение при масштабировании.

Вы можете изменить центр масштабирования присвоив значения свойствам [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) и [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY). Эти свойства определяются относительно самого элемента. Например, значение 0 для параметра `AnchorX` означает левый край соответствующего элемента, а значение 1 — его правый край. Аналогично для `AnchorY`, 0 обозначает верхний край, а 1— нижний. Оба этих свойства имеют значение по умолчанию 0,5, что и обозначает центр элемента.

Пример [**AnchoredScaleDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/AnchoredScaleDemo) позволяет поэкспериментировать со свойствами `AnchorX` и `AnchorY`, а также со свойством `Scale`.

В iOS использование значений, отличных от значения по умолчанию, для свойств `AnchorX` и `AnchorY` обычно несовместимо с изменениями ориентации экрана.

## <a name="the-rotation-transform"></a>Преобразование поворотом

Свойство [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) указывается в градусах и обозначает поворот по часовой стрелке вокруг той точки элемента, которая определена значениями `AnchorX` и `AnchorY`. Пример [**PlaneRotationDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/PlaneRotationDemo) позволяет поэкспериментировать с этими тремя свойствами.

### <a name="rotated-text-effects"></a>Эффекты текста с поворотом

Пример [**BoxViewCircle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewCircle) демонстрирует математическую основу, которая позволяет нарисовать круг и 64 элементов `BoxView` с поворотами на небольшие смещения.

Пример [**RotatedText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/RotatedText) демонстрирует несколько элементов `Label` с одинаковой строкой текста, повернутых для создания эффекта "лучей".

Пример [**CircularText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/CircularText) демонстрирует строку текста, которая заворачивается по кругу.

### <a name="an-analog-clock"></a>Аналоговые часы

Библиотека [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) содержит класс [`AnalogClockViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnalogClockViewModel.cs), который вычисляет углы для положения стрелок часов. Чтобы избежать зависимостей от платформы в ViewModel, этот класс использует `Task.Delay` вместо таймера для получения нового значения `DateTime`.

Также в **Xamarin.FormsBook.Toolkit** включен класс [`SecondTickConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondTickConverter.cs) с реализацией `IValueConverter`, который округляет положение секундной стрелки до ближайшей секунды.

Пример [**MinimalBoxViewClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/MinimalBoxViewClock) использует три элемента `BoxView` и преобразование вращением для отрисовки аналоговых часов.

[**BoxViewClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewClock) использует `BoxView` для более сложной графики, включая отметки на циферблате и вращение стрелок вокруг точки, немного отстоящей от их конца.

[![Снимок экрана с тремя изображениями часов BoxView](images/ch21fg17-small.png "Циферблат аналоговых часов")](images/ch21fg17-large.png#lightbox "Циферблат аналоговых часов")

Кроме того, класс [`SecondBackEaseConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondBackEaseConverter.cs) из **Xamarin.FormsBook.Toolkit** отображает секундную стрелку так, что она немного сдвигается назад перед прыжком вперед, а затем опять смещается назад в нужное положение.

### <a name="vertical-sliders"></a>Вертикальные ползунки?

Пример [**VerticalSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/VerticalSliders) демонстрирует, что элементы `Slider` будут нормально функционировать, если их повернуть на 90 градусов. Но вам будет сложно правильно разместить такие элементы `Slider`, поскольку в макете они всегда остаются горизонтальными.

## <a name="3d-ish-rotations"></a>Вращения в трехмерном пространстве

Свойство [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX) действует так, как если бы элемент вращался в трехмерном пространстве вокруг своей горизонтальной оси, то есть верхняя и нижняя части элемента приближаются к зрителю или удаляются от него. Аналогичным образом, свойство [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY) вращает элемент в трехмерном пространстве вокруг его вертикальной оси, то есть левая и правая части элемента приближаются к зрителю или удаляются от него.

Свойство `AnchorX` влияет на `RotationY`, но не на `RotationX`. Свойство `AnchorY` влияет на `RotationX`, но не на `RotationY`. Вы можете поэкспериментировать с примером [**ThreeDeeRotationDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ThreeDeeRotationDemo), чтобы изучить взаимодействие этих трех свойств.

Трехмерная координатная система в Xamarin.Forms является левосторонней. Если направить указательный палец левой руки в направлении увеличения координат X (вправо по экрану), а средний — в направлении увеличения координат Y (вниз), то большой палец будет направлен в сторону увеличения координат Z (из экрана к наблюдателю).

Кроме того, для всех трех осей действует такое правило: если вы направите большой палец в сторону увеличения координат, изгиб остальных пальцев демонстрирует направление вращения на положительный угол.

## <a name="related-links"></a>Связанные ссылки

- [Глава 21, полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch21-Apr2016.pdf)
- [Примеры для Главы 21](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)
