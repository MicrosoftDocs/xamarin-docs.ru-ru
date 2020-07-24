---
title: Общие сведения о Опентк в Xamarin. Mac
description: В этой статье приводятся общие сведения о работе с Опентк в приложении Xamarin. Mac. Здесь рассматривается создание и обслуживание игрового окна, Визуализация простого объекта и отображение объекта пользователю.
ms.prod: xamarin
ms.assetid: BDE05645-7273-49D3-809B-8642347678D2
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: fbd6e4a3b1c5cdb45a057f1ed664a0d5e71e224d
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937895"
---
# <a name="introduction-to-opentk-in-xamarinmac"></a>Общие сведения о Опентк в Xamarin. Mac

Опентк (Open Toolkit) — это расширенная, низкоуровневого библиотека C#, которая упрощает работу с OpenGL, OpenCL и открытым. Опентк можно использовать для игр, научных приложений или других проектов, требующих трехмерной графики, аудио или вычислительной функциональности. В этой статье приводятся краткие сведения об использовании Опентк в приложении Xamarin. Mac.

[![Пример выполнения приложения](opentk-images/intro01.png)](opentk-images/intro01.png#lightbox)

В этой статье рассматриваются основы Опентк в приложении Xamarin. Mac. Мы настоятельно рекомендуем сначала ознакомиться со статьей [Hello, Mac](~/mac/get-started/hello-mac.md) , в частности [Знакомство с Xcode и Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) , а также с разделом "возможности [и действия](~/mac/get-started/hello-mac.md#outlets-and-actions) ", так как в нем рассматриваются основные понятия и методы, которые мы будем использовать в этой статье.

Возможно, вы захотите ознакомиться с [предоставлением классов и методов C# для цели-c](~/mac/internals/how-it-works.md) в документе о внутренних компонентах [Xamarin. Mac](~/mac/internals/how-it-works.md) . здесь также объясняются `Register` команды и, `Export` используемые для подключения классов c# к объектам и элементам пользовательского интерфейса на языке c.

<a name="About_OpenTK"></a>

## <a name="about-opentk"></a>О Опентк

Как уже говорилось выше, Опентк (открытый набор средств) — это расширенная, низкоуровневое библиотека C#, которая упрощает работу с OpenGL, OpenCL и открытым. Использование Опентк в приложении Xamarin. Mac предоставляет следующие возможности:

- **Быстрая разработка** — опентк предоставляет надежные типы данных и встроенную документацию для более простого и быстрого перехвата потока кода и выявления ошибок.
- **Простая интеграция** — опентк была разработана для простой интеграции с приложениями .NET.
- **Разрешительное License** -опентк распространяется по лицензиям MIT/X11 и полностью бесплатный.
- **Широкие** строго типизированные привязки — опентк поддерживают новейшие версии OpenGL, OpenGL | ES, Open и OpenCL с автоматической загрузкой расширений, проверкой ошибок и встроенной документацией.
- **Гибкие параметры графического пользовательского интерфейса** — опентк предоставляет собственное, высокопроизводительное окно игры, разработанное специально для игр и Xamarin. Mac.
- **Полностью управляемый CLS-совместимый код** — опентк поддерживает 32-разрядные и 64-разрядные версии macOS без неуправляемых библиотек.
- **набор средств для 3D-математических вычислений** Опентк предоставляет `Vector` `Matrix` структуры, `Quaternion` и с `Bezier` помощью набора трехмерных математических инструментов.

Опентк можно использовать для игр, научных приложений или других проектов, требующих трехмерной графики, аудио или вычислительной функциональности.

Дополнительные сведения см. на веб-сайте [Open Toolkit](https://opentk.net) .

<a name="OpenTK_Quickstart"></a>

## <a name="opentk-quickstart"></a>Краткое руководство по Опентк

В качестве краткого знакомства с использованием Опентк в приложении Xamarin. Mac мы создадим простое приложение, которое открывает игровое представление, визуализирует простой треугольник в этом представлении и присоединяет представление игры к главному окну приложения Mac для отображения треугольника пользователю.

<a name="Starting_a_New_Project"></a>

### <a name="starting-a-new-project"></a>Запуск нового проекта

Запустите Visual Studio для Mac и создайте новое решение Xamarin. Mac. Выберите **Mac**  >  **приложение**Mac  >  **Общие**  >  **Cocoa приложение**:

[![Добавление нового приложения Cocoa](opentk-images/sample01.png)](opentk-images/sample01.png#lightbox)

Введите `MacOpenTK` для **имени проекта**:

[![Задание имени проекта](opentk-images/sample02.png)](opentk-images/sample02.png#lightbox)

Нажмите кнопку **создать** , чтобы создать новый проект.

<a name="Including_OpenTK"></a>

### <a name="including-opentk"></a>Включение Опентк

Прежде чем можно будет использовать Open TK в приложении Xamarin. Mac, необходимо включить ссылку на сборку Опентк. В **Обозреватель решений**щелкните правой кнопкой мыши папку **ссылки** и выберите **изменить ссылки.**...

Установите флажок `OpenTK` и нажмите кнопку **ОК** :

[![Изменение ссылок на проекты](opentk-images/sample03.png)](opentk-images/sample03.png#lightbox)

<a name="Using_OpenTK"></a>

### <a name="using-opentk"></a>Использование Опентк

После создания нового проекта дважды щелкните `MainWindow.cs` файл в **Обозреватель решений** , чтобы открыть его для редактирования. Сделайте класс похожим `MainWindow` на следующий:

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using AppKit;
using CoreGraphics;

namespace MacOpenTK
{
    public partial class MainWindow : NSWindow
    {
        #region Computed Properties
        public MonoMacGameView Game { get; set; }
        #endregion

        #region Constructors
        public MainWindow (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindow (NSCoder coder) : base (coder)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create new Game View and replace the window content with it
            Game = new MonoMacGameView(ContentView.Frame);
            ContentView = Game;

            // Wire-up any required Game events
            Game.Load += (sender, e) =>
            {
                // TODO: Initialize settings, load textures and sounds here
            };

            Game.Resize += (sender, e) =>
            {
                // Adjust the GL view to be the same size as the window
                GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
            };

            Game.UpdateFrame += (sender, e) =>
            {
                // TODO: Add any game logic or physics
            };

            Game.RenderFrame += (sender, e) =>
            {
                // Setup buffer
                GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
                GL.MatrixMode(MatrixMode.Projection);

                // Draw a simple triangle
                GL.LoadIdentity();
                GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
                GL.Begin(BeginMode.Triangles);
                GL.Color3(Color.MidnightBlue);
                GL.Vertex2(-1.0f, 1.0f);
                GL.Color3(Color.SpringGreen);
                GL.Vertex2(0.0f, -1.0f);
                GL.Color3(Color.Ivory);
                GL.Vertex2(1.0f, 1.0f);
                GL.End();

            };

            // Run the game at 60 updates per second
            Game.Run(60.0);
        }
        #endregion
    }
}
```

Давайте подробно рассмотрим этот код.

<a name="Required_APIs"></a>

### <a name="required-apis"></a>Требуемые API

Для использования Опентк в классе Xamarin. Mac требуется несколько ссылок. В начале определения мы включили следующие `using` инструкции:

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using CoreGraphics;
```

Этот минимальный набор будет необходим для любого класса, использующего Опентк.

<a name="Adding_the_Game_View"></a>

### <a name="adding-the-game-view"></a>Добавление представления игры

Далее необходимо создать представление игры, которое будет содержать все взаимодействия с Опентк и отобразить результаты. Мы использовали следующий код:

```csharp
public MonoMacGameView Game { get; set; }
...

// Create new Game View and replace the window content with it
Game = new MonoMacGameView(ContentView.Frame);
ContentView = Game;
```

Мы предоставили игровое представление того же размера, что и главное окно Mac, и заменили представление содержимого окна новым `MonoMacGameView` . Так как мы заменили существующее содержимое окна, при изменении размера основного окна размер представления будет изменяться автоматически.

<a name="Responding_to_Events"></a>

### <a name="responding-to-events"></a>Реагирование на события

Существует несколько событий по умолчанию, на которые должно реагировать каждое представление игры. В этом разделе будут рассмотрены основные события, необходимые для.

<a name="The_Load_Event"></a>

### <a name="the-load-event"></a>Событие Load

`Load`Событие — это место для загрузки ресурсов с диска, например изображений, текстур или музыки. Для нашего простого тестового приложения мы не используем `Load` событие, но включили его для справки:

```csharp
Game.Load += (sender, e) =>
{
    // TODO: Initialize settings, load textures and sounds here
};
```

<a name="The_Resize_Event"></a>

### <a name="the-resize-event"></a>Событие изменения размера

`Resize`Событие должно вызываться при каждом изменении размера представления игры. Для нашего примера приложения мы создаем окно представления GL того же размера, что и наше представление игры (автоматическое изменение размера в главном окне Mac), с помощью следующего кода:

```csharp
Game.Resize += (sender, e) =>
{
    // Adjust the GL view to be the same size as the window
    GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
};
```

<a name="The_UpdateFrame_Event"></a>

### <a name="the-updateframe-event"></a>Событие Упдатефраме

`UpdateFrame`Событие используется для обработки вводимых пользователем данных, обновления позиций объектов, выполнения вычислений физикы или искусственного интеллекта. Для нашего простого тестового приложения мы не используем `UpdateFrame` событие, но включили его для справки:

```csharp
Game.UpdateFrame += (sender, e) =>
{
    // TODO: Add any game logic or physics
};
```

> [!IMPORTANT]
> Реализация Опентк в Xamarin. Mac не включает `Input API` , поэтому для добавления поддержки клавиатуры и мыши потребуется использовать API, предоставляемые компанией Apple. При необходимости можно создать пользовательский экземпляр класса `MonoMacGameView` и переопределить `KeyDown` `KeyUp` методы и.

<a name="The_RenderFrame_Event"></a>

### <a name="the-renderframe-event"></a>Событие Рендерфраме

`RenderFrame`Событие содержит код, используемый для отрисовки (рисования) графики. Для нашего примера приложения мы заполняем представление игры простым треугольником:

```csharp
Game.RenderFrame += (sender, e) =>
{
    // Setup buffer
    GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
    GL.MatrixMode(MatrixMode.Projection);

    // Draw a simple triangle
    GL.LoadIdentity();
    GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
    GL.Begin(BeginMode.Triangles);
    GL.Color3(Color.MidnightBlue);
    GL.Vertex2(-1.0f, 1.0f);
    GL.Color3(Color.SpringGreen);
    GL.Vertex2(0.0f, -1.0f);
    GL.Color3(Color.Ivory);
    GL.Vertex2(1.0f, 1.0f);
    GL.End();

};
```

Как правило, код прорисовки будет `GL.Clear` содержать вызов для удаления всех существующих элементов до вывода новых элементов.

> [!IMPORTANT]
> Для версии Опентк Xamarin. Mac **не** вызывайте `SwapBuffers` метод `MonoMacGameView` экземпляра в конце кода отрисовки. Это приведет к быстрому воспроизведению игры, а не к отображенному представлению.

<a name="Running_the_Game_View"></a>

### <a name="running-the-game-view"></a>Запуск представления игры

После определения всех необходимых событий и представления игры, присоединенного к главному окну Mac нашего приложения, мы читаем, чтобы запустить представление игры и отобразить нашу графику. Используйте следующий код:

```csharp
// Run the game at 60 updates per second
Game.Run(60.0);
```

Мы передаем желаемую частоту кадров, которую мы хотим обновить в игровом представлении. в нашем примере мы выбрали `60` кадры в секунду (одинаковую частоту обновления для обычного телевизора).

Давайте запустим приложение и увидите выходные данные:

[![Пример выходных данных приложений](opentk-images/intro01.png)](opentk-images/intro01.png#lightbox)

Если изменить размер окна, представление игры также будет располагаться, а размер треугольника также будет изменен и обновлен в режиме реального времени.

<a name="Where_to_Next"></a>

### <a name="where-to-next"></a>Далее?

Используя основы работы с Опентк в приложении Xamarin. Mac, ниже приведены некоторые рекомендации по выполнению следующих действий.

- Попробуйте изменить цвет треугольника и цвет фона для представления игры в `Load` `RenderFrame` событиях и.
- Измените цвет треугольника, когда пользователь нажмет клавишу в `UpdateFrame` событиях и, `RenderFrame` или создайте собственный пользовательский `MonoMacGameView` класс и переопределите `KeyUp` методы и `KeyDown` .
- Сделайте треугольник передвигаться по экрану с помощью подзнающих ключей в `UpdateFrame` событии. Указание. Используйте `Matrix4.CreateTranslation` метод, чтобы создать матрицу перевода и вызвать `GL.LoadMatrix` метод, чтобы загрузить его в `RenderFrame` событие.
- Используйте `for` цикл для отрисовки нескольких треугольников в `RenderFrame` событии.
- Поверните камеру, чтобы дать другое представление треугольника в трехмерном пространстве. Указание. Используйте `Matrix4.CreateTranslation` метод, чтобы создать матрицу перевода и вызвать `GL.LoadMatrix` метод для его загрузки. `Vector2` `Vector3` Для манипуляций с камерой можно также `Vector4` использовать `Matrix4` классы, и.

Дополнительные примеры см. в репозитории [Опентк Samples GitHub](https://github.com/opentk/opentk/tree/master/Source/Examples) . Он содержит официальный список примеров использования Опентк. Вам потребуется адаптировать эти примеры для использования с версией Опентк для Xamarin. Mac.

Более сложный пример реализации Опентк в Xamarin. Mac см. в нашем примере [мономакгамевиев](https://docs.microsoft.com/samples/xamarin/mac-samples/monomacgamewindow) .

<a name="Summary"></a>

## <a name="summary"></a>Сводка

В этой статье приведен краткий обзор работы с Опентк в приложении Xamarin. Mac. Мы увидели, как создать игровое окно, как подключить игровое окно к окну Mac и как визуализировать простую фигуру в окне игры.

## <a name="related-links"></a>Связанные ссылки

- [Макопентк (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/macopentk)
- [Мономакгамевиев (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/monomacgamewindow)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Работа с Windows](~/mac/user-interface/window.md)
- [Открытый набор средств](https://opentk.net)
- [Рекомендации по работе с человеческим интерфейсом OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Введение в Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
