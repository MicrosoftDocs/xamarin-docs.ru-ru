---
title: Использование UrhoSharp вXamarin.Forms
description: В этой статье объясняется, как можно использовать UrhoSharp для добавления трехмерной графики в Xamarin.Forms приложение для расширенной визуализации.
ms.prod: xamarin
ms.assetid: 0646B98E-CC04-4537-9715-9F82338FD7FF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/11/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fd1893a91d9d8e5d2c2581a9f3f9b5ef8ee59f1f
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937677"
---
# <a name="using-urhosharp-in-xamarinforms"></a>Использование UrhoSharp вXamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/urho-samples/tree/master/FormsSample)

## <a name="what-is-urhosharp"></a>Что такое UrhoSharp?

[UrhoSharp](~/graphics-games/urhosharp/index.md) — это мощный трехмерный механизм для разработчиков Xamarin и .NET. [Введение познакомит](~/graphics-games/urhosharp/introduction.md) с библиотекой UrhoSharp, а в [этих заметках](~/graphics-games/urhosharp/using.md) описывается, как программировать сцены и действия.

UrhoSharp можно использовать для отрисовки графики в Xamarin.Forms приложениях.
В этом [образце](https://github.com/xamarin/urho-samples/tree/master/FormsSample) показано, как UrhoSharp можно использовать для создания интерактивной трехмерной диаграммы:

![Трехмерная интерактивная диаграмма UrhoSharp на устройстве iOS ](urhosharp-images/ios-animation.gif)
 ![ UrhoSharp 3D Interactive в Android](urhosharp-images/android-animation.gif)

## <a name="adding-the-urhosharp-nuget-packages"></a>Добавление пакетов NuGet UrhoSharp

Перед использованием UrhoSharp разработчикам необходимо добавить в свое решение пакет NuGet UrhoSharp. В этом учебнике предполагается наличие Xamarin.Forms проекта с проектом библиотеки iOS, Android и .NET Standard. Весь код будет записан в проект библиотеки .NET Standard; но UrhoSharp NuGet также необходимо добавить в проекты iOS и Android.

Пакет NuGet UrhoSharp. Forms содержит все объекты, необходимые для создания объектов UrhoSharp. Пакет NuGet UrhoSharp. Forms включает `UrhoSurface` класс, который используется для размещения UrhoSharp в Xamarin.Forms .
Чтобы начать, щелкните правой кнопкой мыши папку **packages** в проекте библиотеки .NET Standard и выберите пункт **Добавить пакеты...**. Введите условие поиска **UrhoSharp. Forms**, выберите **UrhoSharp для Xamarin.Forms **, а затем щелкните **Добавить пакет**.

[![Диалоговое окно добавления пакетов](urhosharp-images/add-package-sml.png)](urhosharp-images/add-package.png#lightbox "Диалоговое окно добавления пакетов")

Пакет NuGet UrhoSharp. Forms будет добавлен в проект:

![Папка Packages](urhosharp-images/packages.png)

Повторите описанные выше действия для проектов, связанных с платформой (например, iOS и Android).

## <a name="walkthrough-adding-urhosharp-to-a-xamarinforms-app"></a>Пошаговое руководство. Добавление UrhoSharp в Xamarin.Forms приложение

Эти шаги описывают код в Xamarin.Forms образце UrhoSharp:

1. [Создание страницы Xamarin Forms](#1-create-a-xamarin-forms-page)
2. [Добавление Урхосурфаце](#2-add-the-urhosurface)
3. [Создание приложения Урхо](#3-build-an-urho-application)
4. [Добавление класса Charts в Урхосурфаце](#4-add-the-charts-class-to-the-urhosurface)
5. [Взаимодействие с UrhoSharp](#5-interacting-with-urhosharp)

Обратите внимание, что в примере используются функции C# 6, которые могут не компилироваться в более старых версиях Visual Studio.

### <a name="1-create-a-xamarin-forms-page"></a>1. Создание страницы Xamarin Forms

В приведенном ниже коде показана Xamarin.Forms Страница `UrhoPage` до добавления кода, связанного с Урхо:

```csharp
public class UrhoPage : ContentPage
{
  Slider selectedBarSlider, rotationSlider;

  public UrhoPage()
  {
    // we'll add Urho later

    rotationSlider = new Slider(0, 500, 250);

    selectedBarSlider = new Slider(0, 5, 2.5);

    Title = " UrhoSharp + Xamarin.Forms";
    Content = new StackLayout {
      Padding = new Thickness (12, 12, 12, 40),
      VerticalOptions = LayoutOptions.FillAndExpand,
      Children = {
        rotationSlider,
        new Label { Text = "SELECTED VALUE:" },
        selectedBarSlider,
      }
    };
  }
```

### <a name="2-add-the-urhosurface"></a>2. Добавьте Урхосурфаце

UrhoSharp может размещаться в, `ContentPage` как и другие Xamarin.Forms элементы управления.
В следующем фрагменте кода показана `UrhoSurface` страница, добавленная на Xamarin.Forms страницу:

```csharp
using Urho;
using Urho.Forms;
...
public class UrhoPage : ContentPage
{
  UrhoSurface urhoSurface;

  public UrhoPage()
  {
    urhoSurface = new UrhoSurface();
    urhoSurface.VerticalOptions = LayoutOptions.FillAndExpand;
...
    Content = new StackLayout {
    Padding = new Thickness (12, 12, 12, 40),
    VerticalOptions = LayoutOptions.FillAndExpand,
    Children = {
      urhoSurface,  // added
      new Label { Text = "ROTATION:" },
      rotationSlider,
      new Label { Text = "SELECTED VALUE:" },
      selectedBarSlider,
    }
  };
```

### <a name="3-build-an-urho-application"></a>3. Создание приложения Урхо

`Charts`Сведения о реализации трехмерной графики Урхо, используемой в этом примере, см. в классе. Ниже показана базовая структура кода. Обратите внимание, что класс реализует, `Urho.Application` который отличается от `Xamarin.Forms.Application` класса, реализованного в **app.CS**.

```csharp
using Urho;
using Urho.Actions;
using Urho.Gui;
using Urho.Shapes;

namespace FormsSample
{
    public class Charts : Urho.Application
    {
    public Charts (ApplicationOptions options = null) : base(options) { }
    protected override void Start ()
    {
      ...
    }
    protected override void OnUpdate(float timeStep)
    {
      ...
    }
```

[Документация по UrhoSharp](~/graphics-games/urhosharp/index.md) содержит дополнительные сведения о создании трехмерных сцен и действий.

### <a name="4-add-the-charts-class-to-the-urhosurface"></a>4. Добавьте класс Charts в Урхосурфаце

Используйте `UrhoSurface.Show<T>` универсальный метод, чтобы добавить приложение Урхо на Xamarin.Forms страницу. В приведенном ниже фрагменте кода показан дополнительный код, необходимый для создания `Charts` класса:

```csharp
public class UrhoPage : ContentPage
{
  Charts urhoApp;
  ...
  protected override async void OnAppearing ()
  {
    urhoApp = await urhoSurface.Show<Charts> (new ApplicationOptions(assetsFolder: null)
      { Orientation = ApplicationOptions.OrientationType.Portrait });
  }
```

Примечание. `Show<T>` метод является асинхронным и должен вызываться с `await` ключевым словом.

### <a name="5-interacting-with-urhosharp"></a>5. взаимодействие с UrhoSharp

В этом примере можно выбрать и изменить гистограммы. `Charts`Класс предоставляет `Bars` и `SelectedBar` для включения этого взаимодействия.

Каждый "Bar" имеет обработчик событий выбора, добавленный после `Charts` подготовки класса, путем прохода по предоставленной `Bars` коллекции:

```csharp
protected override async void OnAppearing ()
{
  urhoApp = await urhoSurface.Show<Charts>(new ApplicationOptions(assetsFolder: null) { Orientation = ApplicationOptions.OrientationType.Portrait });
  foreach (var bar in urhoApp.Bars)
  {
    bar.Selected += OnBarSelection;
  }
}
```

Обработчик событий использует значение Xamarin.Forms `Slider` элемента управления для настройки высоты данной линейки:

```csharp
private void OnBarSelection(Bar bar)
{
  //reset value
  selectedBarSlider.ValueChanged -= OnValuesSliderValueChanged;
  selectedBarSlider.Value = bar.Value;
  selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
}

void OnValuesSliderValueChanged(object sender, ValueChangedEventArgs e)
{  // C# 6
  if (urhoApp?.SelectedBar != null)
  {
    urhoApp.SelectedBar.Value = (float)e.NewValue;
  }
}
```

Наконец, необходимо установить связь между двумя `Slider` элементами управления, чтобы при изменении их значения UrhoSharp на холсте. Первый ползунок поворачивает изображение трехмерной диаграммы, а второй ползунок корректирует высоту выбранной линии.

```csharp
rotationSlider = new Slider(0, 500, 250);
rotationSlider.ValueChanged += (s, e) => urhoApp?.Rotate((float)(e.NewValue - e.OldValue));

selectedBarSlider = new Slider(0, 5, 2.5);
selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
```

В анимации в [верхней части страницы](#what-is-urhosharp) показан пример выполнения.

## <a name="summary"></a>Сводка

На этой странице показано, как можно использовать UrhoSharp для добавления визуализации трехмерных данных в Xamarin.Forms . Дополнительные сведения [UrhoSharp documentation](~/graphics-games/urhosharp/index.md) о создании Урхо сцен, которые можно включить в Xamarin.Forms приложения с помощью метода, показанного выше, см. в документации по UrhoSharp.

## <a name="related-links"></a>Связанные ссылки

- [Пример диаграмм (C# 6)](https://github.com/xamarin/urho-samples/tree/master/FormsSample)
- [UrhoSharp](~/graphics-games/urhosharp/index.md)
