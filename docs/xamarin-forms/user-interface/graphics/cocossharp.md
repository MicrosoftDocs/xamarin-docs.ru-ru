---
title: Использование CocosSharp вXamarin.Forms
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
ms.openlocfilehash: cb2303eb91fe2aa332ed35131baa7f6dd3cfeff5
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84129523"
---
# <a name="using-cocossharp-in-xamarinforms"></a>Использование CocosSharp вXamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/CocosSharpForms)

_CocosSharp можно использовать для добавления точных визуализаций форм, изображений и текста в приложение для расширенной визуализации._

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**Развитие 2016: кокосовые # вXamarin.Forms**

## <a name="overview"></a>Обзор

CocosSharp — это гибкая, мощная технология для отображения графики, чтения сенсорного ввода, воспроизведения звука и управления содержимым. В этом руководство объясняется, как добавить CocosSharp в Xamarin.Forms приложение. В нем рассматриваются следующие вопросы:

- [Что такое CocosSharp?](#what)
- [Добавление пакетов NuGet CocosSharp](#nuget)
- [Пошаговое руководство. Добавление CocosSharp в Xamarin.Forms приложение](#add)

<a name="what" />

## <a name="what-is-cocossharp"></a>Что такое CocosSharp?

[CocosSharp](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md) — это модуль игры с открытым исходным кодом, доступный на платформе Xamarin.
CocosSharp — это эффективная Библиотека, которая включает в себя следующие функции:

- Визуализация изображений с помощью `CCSprite` класса
- Отрисовка фигур с помощью `CCDrawNode` класса
- Логика каждого фрейма с использованием `CCNode.Schedule` класса
- Управление содержимым (Загрузка и выгрузка ресурсов, таких как PNG-файлы) с помощью`CCTextureCache`
- Анимации с помощью `CCAction` класса

Основной задачей CocosSharp является упрощение создания кросс-платформенных двумерных игр. Однако это также может быть отличным дополнением к приложениям Xamarin Forms. Так как для игр обычно требуется эффективная визуализация и точный контроль над визуальными элементами, CocosSharp можно использовать для добавления мощных визуализаций и эффектов к неигровым приложениям.

Xamarin.Formsпостроена на основе собственных, зависящих от платформы систем пользовательского интерфейса. Например, в iOS и Android [ `Button` могут использоваться разные версии, и](xref:Xamarin.Forms.Button) даже может различаться версия операционной системы. Напротив, CocosSharp не использует никаких визуальных объектов, связанных с платформой, поэтому все визуальные объекты выглядят одинаково на всех платформах. Конечно, разрешение и пропорции различаются между устройствами, и это может повлиять на то, как CocosSharp визуализирует свои визуальные элементы. Эти сведения будут обсуждаться далее в этом пошаговом окне.

Более подробные сведения можно найти в [разделе CocosSharp](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md).

<a name="nuget" />

## <a name="adding-the-cocossharp-nuget-packages"></a>Добавление пакетов NuGet CocosSharp

Прежде чем использовать CocosSharp, разработчикам необходимо внести несколько дополнений в свой Xamarin.Forms проект.
В этом учебнике предполагается наличие Xamarin.Forms проекта с проектом библиотеки iOS, Android и .NET Standard.
Весь код будет записан в проект библиотеки .NET Standard; Однако библиотеки необходимо добавить в проекты iOS и Android.

Пакет NuGet CocosSharp содержит все объекты, необходимые для создания объектов CocosSharp.
Пакет NuGet CocosSharp. Forms включает `CocosSharpView` класс, который используется для размещения CocosSharp в Xamarin.Forms .
Добавьте **CocosSharp. Forms** NuGet и **CocosSharp** будет автоматически добавляться.
Для этого щелкните правой кнопкой мыши папку **packages** в проекте библиотеки .NET Standard и выберите **Добавить пакеты...**. Введите условие поиска **CocosSharp. Forms**, выберите **CocosSharp для Xamarin.Forms **, а затем щелкните **Добавить пакет**.

![](cocossharp-images/image1.png "Add Packages Dialog")

В проект будут добавлены пакеты NuGet **CocosSharp** и **CocosSharp. Forms** :

![](cocossharp-images/image2.png "Packages Folder")

Повторите описанные выше действия для проектов, связанных с платформой (например, iOS и Android).

<a name="add" />

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>Пошаговое руководство. Добавление CocosSharp в Xamarin.Forms приложение

Чтобы добавить в приложение простое представление CocosSharp, выполните следующие действия Xamarin.Forms :

1. [Создание страницы Xamarin Forms](#1)
1. [Добавление Кокосшарпвиев](#2)
1. [Создание Гамесцене](#3)
1. [Добавление круга](#4)
1. [Взаимодействие с CocosSharp](#5)

После успешного добавления CocosSharp представления в Xamarin.Forms приложение обратитесь к [документации по CocosSharp](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md) , чтобы узнать больше о создании содержимого с помощью CocosSharp.

<a name="1" />

### <a name="1-creating-a-xamarin-forms-page"></a>1. Создание страницы Xamarin Forms

CocosSharp может размещаться в любом Xamarin.Forms контейнере. В этом примере для этой страницы используется страница с именем `HomePage` . `HomePage`разбивается пополам на, `Grid` чтобы продемонстрировать, как Xamarin.Forms и CocosSharp могут быть отображены одновременно на одной странице.

Сначала настройте страницу, чтобы она содержала `Grid` и два `Button` экземпляра:

```csharp
public class HomePage : ContentPage
{
public HomePage ()
    {
        // This is the top-level grid, which will split our page in half
        var grid = new Grid ();
        this.Content = grid;
        grid.RowDefinitions = new RowDefinitionCollection {
            // Each half will be the same size:
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
        };
        CreateTopHalf (grid);
        CreateBottomHalf (grid);
    }
    void CreateTopHalf(Grid grid)
    {
        // We'll be adding our CocosSharpView here:
    }
    void CreateBottomHalf(Grid grid)
    {
        // We'll use a StackLayout to organize our buttons
        var stackLayout = new StackLayout();
        // The first button will move the circle to the left when it is clicked:
        var moveLeftButton = new Button {
            Text = "Move Circle Left"
        };
        stackLayout.Children.Add (moveLeftButton);

        // The second button will move the circle to the right when clicked:
        var moveCircleRight = new Button {
            Text = "Move Circle Right"
        };
        stackLayout.Children.Add (moveCircleRight);
        // The stack layout will be in the bottom half (row 1):

        grid.Children.Add (stackLayout, 0, 1);
    }
}
```

В iOS отображается, `HomePage` как показано на следующем рисунке:

![](cocossharp-images/image3.png "HomePage Screenshot")

<a name="2" />

### <a name="2-adding-a-cocossharpview"></a>2. Добавление Кокосшарпвиев

`CocosSharpView`Класс используется для внедрения CocosSharp в Xamarin.Forms приложение. Поскольку `CocosSharpView` наследуется от класса [ Xamarin.Forms . Класс представления](xref:Xamarin.Forms.View) предоставляет знакомый интерфейс для макета, который можно использовать в контейнерах макета, таких как [ Xamarin.Forms . Сетка](xref:Xamarin.Forms.Grid). Добавьте новый `CocosSharpView` в проект, выполнив `CreateTopHalf` метод:

```csharp
void CreateTopHalf(Grid grid)
{
    // This hosts our game view.
    var gameView = new CocosSharpView () {
        // Notice it has the same properties as other XamarinForms Views
        HorizontalOptions = LayoutOptions.FillAndExpand,
        VerticalOptions = LayoutOptions.FillAndExpand,
        // This gets called after CocosSharp starts up:
        ViewCreated = HandleViewCreated
    };
    // We'll add it to the top half (row 0)
    grid.Children.Add (gameView, 0, 0);
}
```

Инициализация CocosSharp не выполняется немедленно, поэтому Зарегистрируйте событие для `CocosSharpView` завершения его создания. Сделайте это в `HandleViewCreated` методе:

```csharp
void HandleViewCreated (object sender, EventArgs e)
{
    var gameView = sender as CCGameView;
    if (gameView != null)
    {
        // This sets the game "world" resolution to 100x100:
        gameView.DesignResolution = new CCSizeI (100, 100);
        // GameScene is the root of the CocosSharp rendering hierarchy:
        gameScene = new GameScene (gameView);
        // Starts CocosSharp:
        gameView.RunWithScene (gameScene);
    }
}
```

Этот `HandleViewCreated` метод имеет две важные сведения, которые мы будем искать. Первый — это `GameScene` класс, который будет создан в следующем разделе. Важно отметить, что приложение не будет компилироваться до `GameScene` создания и `gameScene` разрешения ссылки на экземпляр.

Вторым важным описанием является `DesignResolution` свойство, определяющее видимую область игры для объектов CocosSharp. `DesignResolution`Свойство будет рассматриваться после создания `GameScene` .

<a name="3" />

### <a name="3-creating-the-gamescene"></a>3. Создание Гамесцене

`GameScene`Класс наследует от CocosSharp `CCScene` . `GameScene`— Это первая точка, в которой мы работаем исключительно с CocosSharp. Код, содержащийся в `GameScene` , будет работать в любом приложении CocosSharp, независимо от того, размещено ли оно в Xamarin.Forms проекте.

`CCScene`Класс является корнем визуального элемента для всех CocosSharp отрисовки. Любой видимый объект CocosSharp должен содержаться в `CCScene` . В частности, визуальные объекты должны быть добавлены в `CCLayer` экземпляры, и эти `CCLayer` экземпляры должны быть добавлены в `CCScene` .

Следующая диаграмма поможет визуализировать типичную CocosSharp иерархию:

![](cocossharp-images/image4.png "Typical CocosSharp Hierarchy")

`CCScene`В один момент времени может быть активным только один. Большинство игр используют несколько `CCLayer` экземпляров для сортировки содержимого, но наше приложение использует только одно. Точно так же большинство игр используют несколько визуальных объектов, но в нашем приложении мы будем использовать только один. Более подробное обсуждение визуальной иерархии CocosSharp можно найти в [разделе Пошаговое руководство по баунЦинггаме](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/bouncing-game.md).

Изначально `GameScene` класс будет почти пустым — мы просто создадим его для удовлетворения ссылки в `HomePage` . Добавьте новый класс в проект библиотеки .NET Standard с именем `GameScene` . Он должен наследовать от `CCScene` класса следующим образом:

```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

Теперь, когда `GameScene` определен, можно вернуться к `HomePage` и добавить поле:

```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

Теперь можно скомпилировать наш проект и запустить его, чтобы увидеть CocosSharp. Мы не добавили в наш объект ничего, `GameScene,` поэтому верхняя половина нашей страницы является черным — цвет по умолчанию для CocosSharp сцены:

![](cocossharp-images/image5.png "Blank GameScene")

<a name="4" />

### <a name="4-adding-a-circle"></a>4. Добавление круга

В настоящее время приложение имеет выполняющийся экземпляр ядра CocosSharp, в котором отображается пустое значение `CCScene` . Далее мы добавим визуальный объект: круг. `CCDrawNode`Класс можно использовать для рисования различных геометрических фигур, как описано в разделе [Рисование геометрии с помощью ккдравноде Guide](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/ccdrawnode.md).

Добавьте в наш класс окружность `GameScene` и создайте его экземпляр в конструкторе, как показано в следующем коде:

```csharp
public class GameScene : CCScene
{
    CCDrawNode circle;
    public GameScene (CCGameView gameView) : base(gameView)
    {
        var layer = new CCLayer ();
        this.AddLayer (layer);
        circle = new CCDrawNode ();
        layer.AddChild (circle);
        circle.DrawCircle (
            // The center to use when drawing the circle,
            // relative to the CCDrawNode:
            new CCPoint (0, 0),
            radius:15,
            color:CCColor4B.White);
        circle.PositionX = 20;
        circle.PositionY = 50;
    }
}
```

Теперь при запуске приложения в левой части области отображения CocosSharp отображается окружность:

![](cocossharp-images/image6.png "Circle in GameScene")

#### <a name="understanding-designresolution"></a>Основные сведения о Десигнресолутион

Теперь, когда отображается объект визуального объекта CocosSharp, можно исследовать `DesignResolution` свойство.

`DesignResolution`Представляет ширину и высоту области CocosSharp для размещения и изменения размеров объектов. Фактическое разрешение области измеряется в *пикселях* , а `DesignResolution` измеряется в *единицах*мира. На следующей схеме показано разрешение различных частей представления, отображаемых на iPhone 5 с разрешением экрана 640x1136 пикселей.

![](cocossharp-images/image7.png "iPhone 5s Design Resolution")

На схеме выше отображаются размеры пикселя за пределами экрана в виде черного текста. Единицы отображаются в виде белого текста на диаграмме внутри диаграммы. Ниже приведены некоторые важные сведения.

- Источник CocosSharp отображается внизу слева. При переходе вправо увеличивается значение X, а при движении увеличивается значение Y. Обратите внимание, что значение Y инвертировано по сравнению с некоторыми другими механизмами 2D-макета, где (0, 0) — левый верхний край холста.
- Поведение CocosSharp по умолчанию заключается в сохранении пропорций его представления. Поскольку первая строка в сетке шире, чем высота, CocosSharp не заполняет всю ширину ячейки, как показано пунктирным белым прямоугольником. Это поведение можно изменить, как описано в разделе [Обработка нескольких разрешений в CocosSharp Guide](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/resolutions.md).
- В этом примере CocosSharp будет поддерживать область дисплея 100 единиц в ширину и высоту, независимо от размера или пропорций устройства. Это означает, что в коде можно предположить, что X = 100 соответствует дальней границе CocosSharp дисплея, сохраняя единообразное расположение на всех устройствах.

#### <a name="ccdrawnode-details"></a>Сведения о Ккдравноде

В нашем простом приложении `CCDrawNode` для рисования окружности используется класс. Этот класс может быть очень полезен для бизнес-приложений, так как он обеспечивает рендеринг геометрических объектов, отсутствующий в Xamarin.Forms . Помимо кругов, `CCDrawNode` класс можно использовать для рисования прямоугольников, линий, линий и пользовательских многоугольников. `CCDrawNode`также легко использовать, так как не требует использования файлов изображений (например, PNG). Более подробное обсуждение Ккдравноде можно найти в разделе [Рисование геометрии с помощью ккдравноде Guide](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/ccdrawnode.md).

<a name="5" />

### <a name="5-interacting-with-cocossharp"></a>5. взаимодействие с CocosSharp

Визуальные элементы CocosSharp (например, `CCDrawNode` ) наследуются от `CCNode` класса. `CCNode`предоставляет два свойства, которые можно использовать для размещения объекта относительно его родительского элемента: `PositionX` и `PositionY` . В настоящее время наш код использует эти два свойства для размещения центра окружности, как показано в следующем фрагменте кода:

```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

Важно отметить, что объекты CocosSharp располагаются по явному значению расположения, а не к большинству Xamarin.Forms представлений, которые автоматически размещаются в соответствии с поведением их родительских элементов управления макетом.

Мы добавим код, позволяющий пользователю щелкнуть одну из двух кнопок для перемещения окружности влево или вправо на 10 единиц (не в пикселях), так как круг рисуется в пространстве CocosSharp мира. Сначала мы создадим два открытых метода в `GameScene` классе:

```csharp
public void MoveCircleLeft()
{
    circle.PositionX -= 10;
}

public void MoveCircleRight()
{
    circle.PositionX += 10;
}
```

Далее мы добавим обработчики для двух кнопок в `HomePage` , чтобы реагировать на нажатия. По завершении наш `CreateBottomHalf` метод содержит следующий код:

```csharp
void CreateBottomHalf(Grid grid)
{
    // We'll use a StackLayout to organize our buttons
    var stackLayout = new StackLayout();

    // The first button will move the circle to the left when it is clicked:
    var moveLeftButton = new Button {
        Text = "Move Circle Left"
    };
    moveLeftButton.Clicked += (sender, e) => gameScene.MoveCircleLeft ();
    stackLayout.Children.Add (moveLeftButton);

    // The second button will move the circle to the right when clicked:
    var moveCircleRight = new Button {
        Text = "Move Circle Right"
    };
    moveCircleRight.Clicked += (sender, e) => gameScene.MoveCircleRight ();
    stackLayout.Children.Add (moveCircleRight);

    // The stack layout will be in the bottom half (row 1):
    grid.Children.Add (stackLayout, 0, 1);
}
```

Теперь CocosSharp окружность перемещается в ответ на нажатия. Также можно четко увидеть границы холста CocosSharp, переместив круг достаточного края влево или вправо:

![](cocossharp-images/image8.png "GameScene with Moving Circle")

## <a name="summary"></a>Сводка

В этом руководство показано, как добавить CocosSharp в существующий Xamarin.Forms проект, как создать взаимодействие между Xamarin.Forms и CocosSharp и обсудить различные моменты при создании макетов в CocosSharp.

CocosSharp Game Engine предлагает множество функциональных возможностей и глубину, поэтому в этом пошаговом руководством только некоторые возможности CocosSharp. Разработчики, желающие ознакомиться с дополнительными сведениями о CocosSharp, могут найти множество статей в [архиве CocosSharp](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/).

## <a name="related-links"></a>Связанные ссылки

- [Кокосшарпформс (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/master/CocosSharpForms)
