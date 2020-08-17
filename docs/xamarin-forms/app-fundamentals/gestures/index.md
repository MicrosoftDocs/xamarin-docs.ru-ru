---
title: Жесты в Xamarin.Forms
description: В этом руководстве объясняется, как распознаватели жестов Xamarin.Forms можно использовать для определения взаимодействия пользователя с представлениями в приложении Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 0E197A51-2304-4C09-A710-C7FF24A89F15
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/04/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7528afd0971cf06eb69df4ed7c08c3fd6dcc9e22
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2020
ms.locfileid: "87917821"
---
# <a name="no-locxamarinforms-gestures"></a>Жесты в Xamarin.Forms

_Распознаватели жестов можно использовать для определения взаимодействия пользователя с представлениями в приложении Xamarin.Forms._

Класс Xamarin.Forms [`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer) поддерживает касание, сжатие, сдвиг и перетаскивание в экземплярах [`View`](xref:Xamarin.Forms.View).

## <a name="add-a-tap-gesture-recognizer"></a>[Добавление распознавателя жестов касания](tap.md)

Жест касания используется для обнаружения касания и распознается классом [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer).

## <a name="add-a-pinch-gesture-recognizer"></a>[Добавление распознавателя жестов сжатия](pinch.md)

Жест сжатия используется для интерактивного масштабирования и распознается классом [`PinchGestureRecognizer`](xref:Xamarin.Forms.PinchGestureRecognizer).

## <a name="add-a-pan-gesture-recognizer"></a>[Добавление распознавателя жестов сдвига](pan.md)

Жест сдвига используется для обнаружения движения пальцев по экрану и применения этого движения к содержимому и распознается классом [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer).

## <a name="add-a-swipe-gesture-recognizer"></a>[Добавление распознавателя жестов прокрутки](swipe.md)

Жест прокрутки происходит, когда палец перемещается по экрану в горизонтальном или вертикальном направлении. Он часто используется для перемещения по содержимому. Жесты прокрутки распознаются классом [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer).

## <a name="add-a-drag-and-drop-gesture-recognizer"></a>[Добавление распознавателя жестов перетаскивания](drag-and-drop.md)

Жест перетаскивания позволяет перетаскивать элементы и связанные с ними пакеты данных из одного расположения на экране в другое, используя непрерывный жест. Жесты перетаскивания распознаются с помощью класса `DragGestureRecognizer`, а жесты отпускания — с помощью класса `DropGestureRecognizer`.
