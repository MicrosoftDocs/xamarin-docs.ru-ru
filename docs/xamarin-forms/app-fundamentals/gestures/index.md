---
title: "Жесты в Xamarin.Forms" description: "В этом руководстве объясняется, как распознаватели жестов Xamarin.Forms можно использовать для определения взаимодействия пользователя с представлениями в приложении Xamarin.Forms".
ms.prod: xamarin ms.assetid: 0E197A51-2304-4C09-A710-C7FF24A89F15 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 07/25/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinforms-gestures"></a>Жесты в Xamarin.Forms

_Распознаватели жестов можно использовать для определения взаимодействия пользователя с представлениями в приложении Xamarin.Forms._

Класс Xamarin.Forms [`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer) поддерживает касание, сжатие, сдвиг и прокрутку в экземплярах [`View`](xref:Xamarin.Forms.View).

## <a name="adding-a-tap-gesture-recognizer"></a>[Добавление распознавателя жестов касания](tap.md)

Жест касания используется для обнаружения касания и распознается классом [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer).

## <a name="adding-a-pinch-gesture-recognizer"></a>[Добавление распознавателя жестов сжатия](pinch.md)

Жест сжатия используется для интерактивного масштабирования и распознается классом [`PinchGestureRecognizer`](xref:Xamarin.Forms.PinchGestureRecognizer).

## <a name="adding-a-pan-gesture-recognizer"></a>[Добавление распознавателя жестов сдвига](pan.md)

Жест сдвига используется для обнаружения движения пальцев по экрану и применения этого движения к содержимому и распознается классом [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer).

## <a name="adding-a-swipe-gesture-recognizer"></a>[Добавление распознавателя жестов прокрутки](swipe.md)

Жест прокрутки происходит, когда палец перемещается по экрану в горизонтальном или вертикальном направлении. Он часто используется для перемещения по содержимому. Жесты прокрутки распознаются классом [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer).
