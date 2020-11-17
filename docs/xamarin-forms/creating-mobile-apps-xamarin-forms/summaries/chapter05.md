---
title: Сводка по главе 5. Работа с размерами
description: Создание мобильных приложений с помощью Xamarin.Forms. Сводка по главе 5. Работа с размерами
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 486800E9-C09F-4B95-9AC2-C0F8FE563BCF
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8cd68fdc3d7e94b6ae12ce6296dc4d698bc0ebd2
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93370264"
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>Сводка по главе 5. Работа с размерами

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)

> [!NOTE]
> Эта книга была опубликована весной 2016 года и с тех пор не обновлялась. Многое в этой книге остается ценным, но некоторые материалы устарели, а некоторые разделы перестали быть полностью верными или полными.

На данный момент в Xamarin.Forms используются следующие размеры:

- Высота строки состояния iOS равна 20.
- Ширина и высота `BoxView` по умолчанию — 40.
- Значение `Padding` в `Frame` по умолчанию — 20.
- Значение `Spacing` в `StackLayout` по умолчанию — 6.
- Метод `Device.GetNamedSize` возвращает размер шрифта в виде числа.

Это не размеры в пикселях. Это аппаратно-независимые единицы, которые распознаются на любой платформе.

## <a name="pixels-points-dps-dips-and-dius"></a>Пиксели, точки, единицы dps, DIP и DIU

В начале истории Apple Mac и Microsoft Windows программисты использовали в качестве единиц пиксели. Однако с появлением дисплеев с более высоким разрешением возникла необходимость в более виртуализированном и абстрактном подходе к экранным координатам. Программисты, работающие с Mac, использовали в качестве единиц *точки*, традиционно равные 1/72 дюйма, а разработчики Windows — *аппаратно-независимые единицы* (DIU), которые были равны 1/96 дюйма.

Однако мобильные устройства, как правило, располагаются гораздо ближе к лицу и имеют более высокое разрешение, чем экраны настольных компьютеров, а это означает, что допускается более высокая плотность пикселей.

Программисты, разрабатывающие решения для устройств Apple iPhone и iPad, продолжают использовать в качестве единиц *точки*, но теперь количество таких точек на дюйм равно 160. В зависимости от устройства точка может содержать 1, 2 или 3 пикселя.

Ситуация с Android аналогична. Программисты используют в качестве единиц *независимые от плотности пиксели* (dps), а соотношение между dps и пикселями основано на количестве 160 dps на дюйм.

На телефонах и мобильных устройствах с Windows также установлены коэффициенты масштабирования, которые примерно равны 160 аппаратно-независимым единицам на дюйм.

> [!NOTE]
> В Xamarin.Forms больше не поддерживаются телефоны и мобильные устройства под управлением Windows.

В общем, программист, разрабатывающий с помощью Xamarin.Forms решения для телефонов и планшетов, может предполагать, что все единицы измерения основаны на следующем условии:

- 160 единиц на дюйм, что эквивалентно
- 64 единицам на сантиметр.

Свойства [`Width`](xref:Xamarin.Forms.VisualElement.Width) и [`Height`](xref:Xamarin.Forms.VisualElement.Height) только для чтения, определяемые с помощью `VisualElement`, по умолчанию имеют фиктивное значение &ndash;1. Эти свойства будут отражать фактический размер элемента в аппаратно-независимых единицах, только если размер элемента изменен в соответствии с макетом. Этот размер включает любое значение `Padding`, установленное для элемента, но не `Margin`.

Визуальный элемент вызывает событие [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged) при изменении `Width` или `Height`. В примере [**WhatSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize) это событие используется для отображения размера экрана программы.

## <a name="metrical-sizes"></a>Размеры в метрических единицах

В [**MetricalBoxView**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView) [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) и [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) используются для отображения `BoxView` высотой в один дюйм и шириной в один сантиметр.

## <a name="estimated-font-sizes"></a>Приблизительные размеры шрифта

В примере [**FontSizes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes) показано, как использовать правило "160 единиц на дюйм" для указания размеров шрифта в точках. Визуальная согласованность между платформами, использующими этот прием, лучше, чем у `Device.GetNamedSize`.

## <a name="fitting-text-to-available-size"></a>Подгонка текста к размеру экрана

Блок текста можно вписать в определенный прямоугольник, вычислив `FontSize` для `Label` с помощью следующих условий:

- междустрочный интервал равен 120 % от размера шрифта (на платформах Windows — 130 %);
- средняя ширина символа составляет 50 % от размера шрифта.

Этот метод демонстрируется в примере [**EstimatedFontSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize). Эта программа была написана до того, как стало доступно свойство [`Margin`](xref:Xamarin.Forms.View.Margin), поэтому в ней для имитации поля используется [`ContentView`](xref:Xamarin.Forms.ContentView) с параметром [`Padding`](xref:Xamarin.Forms.Layout.Padding).

[![Тройной снимок экрана с приблизительным размером шрифта](images/ch05fg07-small.png "Текст, подогнанный к размеру экрана")](images/ch05fg07-large.png#lightbox "Текст, подогнанный к размеру экрана")

## <a name="a-fit-to-size-clock"></a>Подбор размера шрифта при оповещении об обновлении часов

В примере [**FitToSizeClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock) демонстрируется использование [`Device.StartTimer`](xref:Xamarin.Forms.Device.StartTimer(System.TimeSpan,System.Func{System.Boolean})) для запуска таймера, который периодически оповещает приложение о том, что пора обновить часы. Размер шрифта устанавливается равным одной шестой ширины страницы, чтобы размер оповещения был максимально большим.

## <a name="accessibility-issues"></a>Проблемы со специальными возможностями

Программы **EstimatedFontSize** и **FitToSizeClock** содержат небольшой недостаток. Если пользователь изменяет параметры специальных возможностей телефона на устройствах Android или Windows 10 Mobile, программа больше не может оценить размер отображаемого текста в зависимости от размера шрифта. Эта проблема продемонстрирована в примере [**AccessibilityTest**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest).

## <a name="empirically-fitting-text"></a>Эмпирическая подгонка текста

Еще один способ вписать текст в прямоугольник — эмпирически вычислить размер отображаемого текста и скорректировать (уменьшить или увеличить) его размер. Программа, описанная в книге, вызывает [`GetSizeRequest`](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double)) в визуальном элементе, чтобы получить нужный размер элемента. Этот метод не рекомендуется к использованию. Вместо этого программы должны вызывать [`Measure`](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)).

Для `Label` первым аргументом должна быть ширина контейнера (чтобы разрешить перенос), а для второго аргумента необходимо задать значение `Double.PositiveInfinity`, чтобы высота не была ограничена. Этот метод демонстрируется в примере [**EmpiricalFontSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize).

## <a name="related-links"></a>Связанные ссылки

- [Глава 5, полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [Примеры для главы 5](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [Примеры F# для главы 5](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
