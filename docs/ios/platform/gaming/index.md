---
title: Игровые API iOS в Xamarin. iOS
description: В этой статье рассматриваются новые возможности игр, предоставляемые iOS 9, которые можно использовать для улучшения функций графики и звука в игре Xamarin. iOS.
ms.prod: xamarin
ms.assetid: 958D38FD-9240-482E-9A42-D6671ED8F2B0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: b21e72cc072b614c2e900d7c7b3aa04c9f0812cb
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939780"
---
# <a name="ios-gaming-apis-in-xamarinios"></a>Игровые API iOS в Xamarin. iOS

_В этой статье рассматриваются новые возможности игр, предоставляемые iOS 9, которые можно использовать для улучшения функций графики и звука в игре Xamarin. iOS._

Компания Apple внесла несколько усовершенствований в игровые API в iOS 9, которые упрощают внедрение игр и аудио в приложение Xamarin. iOS.
Они включают в себя простоту разработки через высокоуровневые платформы и использование возможностей GPU устройства iOS для повышения скорости и возможностей работы с графикой.

[![Пример приложения, в котором работает флоккинг](images/flocking01.png)](images/flocking01.png#lightbox)

Сюда входят Гамеплайкит, Реплайкит, модели ввода-вывода, Металкит и металлические шейдеры производительности, а также новые улучшенные функции металлов, SceneKit и SpriteKit.

В этой статье представлены все способы улучшения игры Xamarin. iOS с помощью новых игр iOS 9:

## <a name="introducing-gameplaykit"></a>Знакомство с Гамеплайкит

Новая Гамеплайкит платформа Apple предоставляет набор технологий, которые позволяют легко создавать игры для устройств iOS, уменьшая количество повторяющихся, общий код, необходимый для реализации. Гамеплайкит предоставляет средства для разработки механики игр, которые затем можно легко сочетать с графическим механизмом (например, SceneKit или SpriteKit) для быстрой доставки готовой игры.

Гамеплайкит включает в себя несколько распространенных алгоритмов воспроизведения игр, таких как:

- На основе поведения — моделирование агента, которое позволяет определять перемещения и цели, которые будут автоматически повлиять на AI.
- Minmax искусственный интеллект для воспроизведения игр.
- Система правил для логики игры, управляемой данными, с нечеткими причинами для обеспечения кризиса поведения.

Кроме того, Гамеплайкит принимает стандартный блок для разработки игр с помощью модульной архитектуры, которая предоставляет следующие возможности:

- Конечный автомат для обработки сложных, процедурных систем на основе кода в игре.
- Средства для предоставления случайного воспроизведения игр и непредсказуемости без возникновения проблем с отладкой.
- Многократно используемая, компонентная архитектура на основе сущностей.

Дополнительные сведения о Гамеплайкит см. в [руководстве по программированию Гамеплайкит](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/GameplayKit_Guide/index.html#//apple_ref/doc/uid/TP40015172) Apple и [справочнике по гамеплайкит Framework](https://developer.apple.com/library/prerelease/ios/documentation/GameplayKit/Reference/GameplayKit_Framework/index.html#//apple_ref/doc/uid/TP40015199).

## <a name="gameplaykit-examples"></a>Примеры Гамеплайкит

Давайте кратко рассмотрим реализацию некоторых простых механизмов игры игр в приложении Xamarin. iOS с помощью комплекта Game Play.

### <a name="pathfinding"></a>пасфиндинг

Пасфиндинг — это способность элемента AI в игре находить свой путь вокруг игровой доски.
Например, 2D-врагов находит свой путь с помощью лабиринта или трехмерного символа через кораблей мира ландшафта.

Рассмотрим следующую карту:

[![Пример пасфиндинг Map](images/gkpathfindpath.png)](images/gkpathfindpath.png#lightbox)

С помощью пасфиндинг этот код C# может найти способ через карту:

```csharp
var a = GKGraphNode2D.FromPoint (new Vector2 (0, 5));
var b = GKGraphNode2D.FromPoint (new Vector2 (3, 0));
var c = GKGraphNode2D.FromPoint (new Vector2 (2, 6));
var d = GKGraphNode2D.FromPoint (new Vector2 (4, 6));
var e = GKGraphNode2D.FromPoint (new Vector2 (6, 5));
var f = GKGraphNode2D.FromPoint (new Vector2 (6, 0));

a.AddConnections (new [] { b, c }, false);
b.AddConnections (new [] { e, f }, false);
c.AddConnections (new [] { d }, false);
d.AddConnections (new [] { e, f }, false);

var graph = GKGraph.FromNodes(new [] { a, b, c, d, e, f });

var a2e = graph.FindPath (a, e); // [ a, c, d, e ]
var a2f = graph.FindPath (a, f); // [ a, b, f ]

Console.WriteLine(String.Join ("->", (object[]) a2e));
Console.WriteLine(String.Join ("->", (object[]) a2f));
```

### <a name="classical-expert-system"></a>Классическая экспертная система

В следующем фрагменте кода C# показано, как можно использовать Гамеплайкит для реализации классической экспертной системы:

```csharp
string output = "";
bool reset = false;
int input = 15;

public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    /*
    If reset is true, clear the output and set reset to false
    */
    var clearRule = GKRule.FromPredicate ((rules) => reset, rules => {
        output = "";
        reset = false;
    });
    clearRule.Salience = 1;

    var fizzRule = GKRule.FromPredicate (mod (3), rules => {
        output += "fizz";
    });
    fizzRule.Salience = 2;

    var buzzRule = GKRule.FromPredicate (mod (5), rules => {
        output += "buzz";
    });
    buzzRule.Salience = 2;

    /*
    This *always* evaluates to true, but is higher Salience, so evaluates after lower-salience items
    (which is counter-intuitive). Print the output, and reset (thus triggering "ResetRule" next time)
    */
    var outputRule = GKRule.FromPredicate (rules => true, rules => {
        System.Console.WriteLine(output == "" ? input.ToString() : output);
        reset = true;
    });
    outputRule.Salience = 3;

    var rs = new GKRuleSystem ();
    rs.AddRules (new [] {
        clearRule,
        fizzRule,
        buzzRule,
        outputRule
    });

    for (input = 1; input < 16; input++) {
        rs.Evaluate ();
        rs.Reset ();
    }
}

protected Func<GKRuleSystem, bool> mod(int m)
{
    Func<GKRuleSystem,bool> partiallyApplied = (rs) => input % m == 0;
    return partiallyApplied;
}
```

На основе заданного набора правил ( `GKRule` ) и известного набора входов экспертная система ( `GKRuleSystem` ) создаст прогнозируемый результат ( `fizzbuzz` для нашего примера выше).

### <a name="flocking"></a>флоккинг

Флоккинг позволяет группе сущностей, управляемых искусственным интеллектом, вести себя как флокк, где группа реагирует на перемещения и действия ведущего объекта, например флокк птиц в полете или школьный Swimming.

В следующем фрагменте кода C# реализуется поведение флоккинг с помощью Гамеплайкит и SpriteKit для графического представления:

```csharp
using System;
using SpriteKit;
using CoreGraphics;
using UIKit;
using GameplayKit;
using Foundation;
using System.Collections.Generic;
using System.Linq;
using OpenTK;

namespace FieldBehaviorExplorer
{
    public static class FlockRandom
    {
        private static GKARC4RandomSource rand = new GKARC4RandomSource ();

        static FlockRandom ()
        {
            rand.DropValues (769);
        }

        public static float NextUniform ()
        {
            return rand.GetNextUniform ();
        }
    }

    public class FlockingScene : SKScene
    {
        List<Boid> boids = new List<Boid> ();
        GKComponentSystem componentSystem;
        GKAgent2D trackingAgent; //Tracks finger on screen
        double lastUpdateTime = Double.NaN;
        //Hold on to behavior so it doesn't get GC'ed
        static GKBehavior flockingBehavior;
        static GKGoal seekGoal;

        public FlockingScene (CGSize size) : base (size)
        {
            AddRandomBoids (20);

            var scale = 0.4f;
            //Flocking system
            componentSystem = new GKComponentSystem (typeof(GKAgent2D));
            var behavior = DefineFlockingBehavior (boids.Select (boid => boid.Agent).ToArray<GKAgent2D>(), scale);
            boids.ForEach (boid => {
                boid.Agent.Behavior = behavior;
                componentSystem.AddComponent(boid.Agent);
            });

            trackingAgent = new GKAgent2D ();
            trackingAgent.Position = new Vector2 ((float) size.Width / 2.0f, (float) size.Height / 2.0f);
            seekGoal = GKGoal.GetGoalToSeekAgent (trackingAgent);
        }

        public override void TouchesBegan (NSSet touches, UIEvent evt)
        {
            boids.ForEach(boid => boid.Agent.Behavior.SetWeight(1.0f, seekGoal));
        }

        public override void TouchesEnded (NSSet touches, UIEvent evt)
        {
            boids.ForEach (boid => boid.Agent.Behavior.SetWeight (0.0f, seekGoal));
        }

        public override void TouchesMoved (NSSet touches, UIEvent evt)
        {
            var touch = (UITouch) touches.First();
            var loc = touch.LocationInNode (this);
            trackingAgent.Position = new Vector2((float) loc.X, (float) loc.Y);
        }

        private void AddRandomBoids (int count)
        {
            var scale = 0.4f;
            for (var i = 0; i < count; i++) {
                var b = new Boid (UIColor.Red, this.Size, scale);
                boids.Add (b);
                this.AddChild (b);
            }
        }

        internal static GKBehavior DefineFlockingBehavior(GKAgent2D[] boidBrains, float scale)
        {
            if (flockingBehavior == null) {
                var flockingGoals = new GKGoal[3];
                flockingGoals [0] = GKGoal.GetGoalToSeparate (boidBrains, 100.0f * scale, (float)Math.PI * 8.0f);
                flockingGoals [1] = GKGoal.GetGoalToAlign (boidBrains, 40.0f * scale, (float)Math.PI * 8.0f);
                flockingGoals [2] = GKGoal.GetGoalToCohere (boidBrains, 40.0f * scale, (float)Math.PI * 8.0f);

                flockingBehavior = new GKBehavior ();
                flockingBehavior.SetWeight (25.0f, flockingGoals [0]);
                flockingBehavior.SetWeight (10.0f, flockingGoals [1]);
                flockingBehavior.SetWeight (10.0f, flockingGoals [2]);
            }
            return flockingBehavior;
        }

        public override void Update (double currentTime)
        {
            base.Update (currentTime);
            if (Double.IsNaN(lastUpdateTime)) {
                lastUpdateTime = currentTime;
            }
            var delta = currentTime - lastUpdateTime;
            componentSystem.Update (delta);
        }
    }

    public class Boid : SKNode, IGKAgentDelegate
    {
        public GKAgent2D Agent { get { return brains; } }
        public SKShapeNode Sprite { get { return sprite; } }

        class BoidSprite : SKShapeNode
        {
            public BoidSprite (UIColor color, float scale)
            {
                var rot = CGAffineTransform.MakeRotation((float) (Math.PI / 2.0f));
                var path = new CGPath ();
                path.MoveToPoint (rot, new CGPoint (10.0, 0.0));
                path.AddLineToPoint (rot, new CGPoint (0.0, 30.0));
                path.AddLineToPoint (rot, new CGPoint (10.0, 20.0));
                path.AddLineToPoint (rot, new CGPoint (20.0, 30.0));
                path.AddLineToPoint (rot, new CGPoint (10.0, 0.0));
                path.CloseSubpath ();

                this.SetScale (scale);
                this.Path = path;
                this.FillColor = color;
                this.StrokeColor = UIColor.White;

            }
        }

        private GKAgent2D brains;
        private BoidSprite sprite;
        private static int boidId = 0;

        public Boid (UIColor color, CGSize size, float scale)
        {
            brains = BoidBrains (size, scale);
            sprite = new BoidSprite (color, scale);
            sprite.Position = new CGPoint(brains.Position.X, brains.Position.Y);
            sprite.ZRotation = brains.Rotation;
            sprite.Name = boidId++.ToString ();

            brains.Delegate = this;

            this.AddChild (sprite);
        }

        private GKAgent2D BoidBrains(CGSize size, float scale)
        {
            var brains = new GKAgent2D ();
            var x = (float) (FlockRandom.NextUniform () * size.Width);
            var y = (float) (FlockRandom.NextUniform () * size.Height);
            brains.Position = new Vector2 (x, y);

            brains.Rotation = (float)(FlockRandom.NextUniform () * Math.PI * 2.0);
            brains.Radius = 30.0f * scale;
            brains.MaxSpeed = 0.5f;
            return brains;
        }

        [Export ("agentDidUpdate:")]
        public void AgentDidUpdate (GameplayKit.GKAgent agent)
        {
        }

        [Export ("agentWillUpdate:")]
        public void AgentWillUpdate (GameplayKit.GKAgent agent)
        {
            var brainsIn = (GKAgent2D) agent;
            sprite.Position = new CGPoint(brainsIn.Position.X, brainsIn.Position.Y);
            sprite.ZRotation = brainsIn.Rotation;
            Console.WriteLine ($"{sprite.Name} -> [{sprite.Position}], {sprite.ZRotation}");
        }
    }
}
```

Затем реализуйте эту сцену в контроллере представления:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
        // Perform any additional setup after loading the view, typically from a nib.
        this.View = new SKView {
        ShowsFPS = true,
        ShowsNodeCount = true,
        ShowsDrawCount = true
    };
}

public override void ViewWillLayoutSubviews ()
{
    base.ViewWillLayoutSubviews ();

    var v = (SKView)View;
    if (v.Scene == null) {
        var scene = new FlockingScene (View.Bounds.Size);
        scene.ScaleMode = SKSceneScaleMode.AspectFill;
        v.PresentScene (scene);
    }
}
```

При запуске небольшие анимированные _«боидс»_ будут флокк вокруг отвода пальца:

[![Небольшой анимированный Боидс будет флокк вокруг отвода пальца](images/flocking01.png)](images/flocking01.png#lightbox)

### <a name="other-apple-examples"></a>Другие примеры Apple

В дополнение к приведенным выше примерам компания Apple предоставила следующие примеры приложений, которые можно перекодировать в C# и Xamarin. iOS:

- [Фауринаров: использование Гамеплайкит Minmax специалист по стратегиям для соперников ии](https://developer.apple.com/library/prerelease/ios/samplecode/FourInARow/Introduction/Intro.html#//apple_ref/doc/uid/TP40016142)
- [Ажентскаталог: использование системы агентов в Гамеплайкит](https://developer.apple.com/library/prerelease/ios/samplecode/AgentsCatalog/Introduction/Intro.html#//apple_ref/doc/uid/TP40016141)
- [Демоботс: создание кросс-платформенной игры с помощью SpriteKit и Гамеплайкит](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)

## <a name="metal"></a>Металлические

В iOS 9 Компания Apple предоставила несколько изменений и дополнений к металлической нагрузке, чтобы обеспечить недорогой доступ к GPU. С помощью металла вы можете максимально увеличить графику и вычислить потенциальные возможности приложений iOS.

Среда металла включает следующие новые функции:

- Новые частные и геоцветовые текстуры трафаретов для OS X.
- Улучшенное качество тени с глубиной глубины и отдельные значения переднего и заднего наборов элементов.
- Усовершенствованный язык заливки и улучшенная стандартная библиотека.
- Вычислительные шейдеры поддерживают более широкий диапазон форматов пикселей.

### <a name="the-metalkit-framework"></a>Платформа Металкит

Платформа Металкит предоставляет набор вспомогательных классов и функций, которые уменьшают объем работы, необходимой для использования металла в приложении iOS. Металкит обеспечивает поддержку в трех ключевых областях:

1. Асинхронная загрузка текстур из различных источников, включая такие общие форматы, как PNG, JPEG, КТКС и водо.
2. Простой доступ к ресурсам на основе ввода-вывода модели для обработки модели с учетом особенностей. Эти функции были оптимизированы для обеспечения эффективной передачи данных между сетками ввода-вывода модели и металлическими буферами.
3. Стандартные металлические представления и Управление представлениями, которые значительно снижают объем кода, необходимый для отображения графических изображений в приложении iOS.

Дополнительные сведения о Металкит см. в справочнике по [платформе металкит](https://developer.apple.com/library/prerelease/ios/documentation/MetalKit/Reference/MTKFrameworkReference/index.html#//apple_ref/doc/uid/TP40015356)компании Apple, [руководстве по программированию металлического программирования](https://developer.apple.com/library/prerelease/ios/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221), лабораторной [платформе и справочнике](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalFrameworkReference/index.html#//apple_ref/doc/uid/TP40014161) [по языку для работы с металлической заливкой](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364).

### <a name="metal-performance-shaders-framework"></a>Платформа шейдеров производительности металла

Платформа шейдеров для металлической производительности предоставляет высокооптимизированный набор графических и вычислительных шейдеров для использования в приложениях iOS на основе металла. Каждый шейдер в среде шейдера металлической производительности был специально настроен для обеспечения высокой производительности на металлических поддерживаемых графических процессорах iOS.

Используя классы шейдеров производительности, можно добиться максимальной производительности для каждого конкретного GPU iOS, не настраивая и не сохраняя отдельные базы кода. Построители текстур производительности можно использовать с любыми металлическими ресурсами, такими как текстуры и буферы.

Платформа шейдера производительности металла предоставляет набор общих шейдеров, таких как:

- **Размытие по Гауссу** ( `MPSImageGaussianBlur` )
- **Обнаружение Собел ребра** ( `MPSImageSobel` )
- **Гистограмма изображений** ( `MPSImageHistogram` )

Дополнительные сведения см. в статье [по языку "металлическая штриховка](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364)" Apple.

## <a name="introducing-model-io"></a>Введение в модель ввода-вывода

Платформа ввода-вывода модели Apple обеспечивает глубокое понимание трехмерных ресурсов (например, моделей и связанных с ними ресурсов). Модель ввода-вывода предоставляет игры для iOS с физическими материалами, моделями и освещением, которые можно использовать с Гамеплайкит, металлами и SceneKit.

С помощью модели ввода-вывода можно поддерживать следующие типы задач:

- Импорт освещения, материалов, данных сетки, параметров камеры и других данных на основе сцены из различных популярных форматов программного обеспечения и игровых модулей.
- Обработка или создание данных на основе сцены, например создание ДОМЕС или внедрить освещения в сетке.
- Работает с Металкит, SceneKit и Глкит для эффективной загрузки игровых ресурсов в буфер GPU для подготовки к просмотру.
- Экспорт данных на основе сцен в разнообразные популярные форматы программного обеспечения и игровых модулей.

Дополнительные сведения о вводе-выводе в модели см. в [справочнике по платформе модели ввода-вывода](https://developer.apple.com/library/prerelease/ios/documentation/ModelIO/Reference/ModelIO_Framework/index.html#//apple_ref/doc/uid/TP40015421) Apple.

## <a name="introducing-replaykit"></a>Знакомство с Реплайкит

Новая Реплайкит платформа Apple позволяет легко добавлять записи игр в свою игру iOS и позволять пользователю быстро и легко редактировать и предоставлять общий доступ к этому видео в приложении.

Дополнительные сведения см. в статье Apple to [реплайкит and Game Center Video](https://developer.apple.com/videos/wwdc/2015/?id=605) and [Демоботс: создание кросс-платформенной игры с помощью SpriteKit и гамеплайкит](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) Sample App.

## <a name="scenekit"></a>SceneKit

Набор сцен — это API графа трехмерной сцены, который упрощает работу с трехмерной графикой. Он впервые появился в OS X 10,8 и теперь поставляется с iOS 8. С помощью набора сцен создание впечатляющих трехмерных визуализаций и необычных трехмерных игр не требует опыта в OpenGL. Основываясь на общих концепциях графов сцены, набор сцен абстрагирует сложности OpenGL и OpenGL ES, что упрощает добавление трехмерного содержимого в приложение. Тем не менее, если вы являетесь экспертом по стандарту OpenGL, в наборе сцен есть отличная поддержка привязки непосредственно с OpenGL. Он также включает множество функций, дополняющих трехмерную графику, например "физика", и хорошо интегрируется с несколькими другими платформами Apple, такими как основная анимация, основной образ и набор спрайтов.

Дополнительные сведения см. в нашей документации по [SceneKit](~/ios/platform/gaming/scenekit.md) .

### <a name="scenekit-changes"></a>SceneKit изменения

Компания Apple добавила следующие новые функции в SceneKit для iOS 9:

- Xcode теперь предоставляет редактор сцены, позволяющий быстро создавать игры и интерактивные трехмерные приложения путем редактирования сцен непосредственно из Xcode.
- `SCNView`Классы и `SCNSceneRenderer` можно использовать для включения визуализации металла (на поддерживаемых устройствах iOS).
- `SCNAudioPlayer`Классы и `SCNNode` можно использовать для добавления пространственных звуковых эффектов, которые автоматически отслеживанием расположения проигрывателя в приложении iOS.

Дополнительные сведения см. в [документации по SceneKit](~/ios/platform/introduction-to-ios8.md#scenekit) и [справочнике по SceneKit Framework](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SceneKit_Framework/index.html#//apple_ref/doc/uid/TP40012283) Apple и [Fox: создание игры SceneKit с помощью примера проекта Xcode сцены](https://developer.apple.com/library/prerelease/ios/samplecode/Fox/Introduction/Intro.html#//apple_ref/doc/uid/TP40016154) .

## <a name="spritekit"></a>SpriteKit

В комплекте спрайтов, двумерной игровой платформе компании Apple, есть некоторые интересные новые функции в iOS 8 и OS X Yosemite. К ним относятся интеграция с набором сцен, поддержка шейдера, освещение, тени, ограничения, нормальное создание карт и усовершенствования физических ресурсов. В частности, новые физические функции позволяют легко добавлять реалистичные эффекты в игру.

Дополнительные сведения см. в нашей документации по [SpriteKit](~/ios/platform/gaming/spritekit.md) .

### <a name="spritekit-changes"></a>SpriteKit изменения

Компания Apple добавила следующие новые функции в SpriteKit для iOS 9:

- Пространственный звуковой эффекты, который автоматически следит за положением проигрывателя с `SKAudioNode` классом.
- Xcode теперь содержит редактор сцены и редактор действий для простой двухмерной игры и создания приложений.
- Поддержка быстрой прокрутки игр с новыми объектами камеры ( `SKCameraNode` ).
- На устройствах iOS, поддерживающих металла, SpriteKit будет автоматически использовать его для подготовки к просмотру, даже если вы уже использовали пользовательские шейдеры OpenGL ES.

Дополнительные сведения см. в [документации по SpriteKit](~/ios/platform/introduction-to-ios8.md#spritekit) [Framework Apple SpriteKit](https://developer.apple.com/library/prerelease/ios/documentation/SpriteKit/Reference/SpriteKitFramework_Ref/index.html#//apple_ref/doc/uid/TP40013041) Platform и их [Демоботс: создание кросс-платформенной игры с помощью SpriteKit и гамеплайкит](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) Sample App.

## <a name="summary"></a>Итоги

В этой статье были рассмотрены новые игровые функции, предоставляемые iOS 9 для приложений Xamarin. iOS.
В нем появился Гамеплайкит и модель ввода-вывода. Основные усовершенствования для металла; и новые возможности SceneKit и SpriteKit.

## <a name="related-links"></a>Связанные ссылки

- [Примеры для iOS 9](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [iOS 9 для разработчиков](https://developer.apple.com/ios/pre-release/)
- [iOS 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
