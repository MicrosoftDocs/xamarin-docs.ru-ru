---
title: Использование класса Model
description: Класс модели значительно упрощает визуализацию сложных трехмерных объектов по сравнению с традиционным методом отрисовки трехмерной графики. Объекты модели создаются из файлов содержимого, что позволяет легко интегрировать содержимое без пользовательского кода.
ms.prod: xamarin
ms.assetid: AD0A7971-51B1-4E38-B412-7907CE43CDDF
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 06d585e976ce41080682f531b9661ff06cedaebe
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91437017"
---
# <a name="using-the-model-class"></a>Использование класса Model

_Класс модели значительно упрощает визуализацию сложных трехмерных объектов по сравнению с традиционным методом отрисовки трехмерной графики. Объекты модели создаются из файлов содержимого, что позволяет легко интегрировать содержимое без пользовательского кода._

Этот API включает `Model` класс, который можно использовать для хранения данных, загруженных из файла содержимого, и для выполнения отрисовки. Файлы модели могут быть очень простыми (например, сплошной цветной треугольник) или могут включать сведения для сложной отрисовки, включая текстурирование и освещение.

В этом пошаговом руководстве используется [трехмерная модель робота](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) и рассматриваются следующие темы:

- Запуск нового игрового проекта
- Создание Кснбс для модели и ее текстуры
- Включение Кснбс в проект игры
- Рисование трехмерной модели
- Рисование нескольких моделей

По завершении наш проект будет выглядеть следующим образом:

![Пример завершения примера, демонстрирующий шесть роботов](part1-images/image1.png)

## <a name="creating-an-empty-game-project"></a>Создание пустого игрового проекта

Необходимо сначала настроить игровой проект с именем MonoGame3D. Сведения о создании проекта с неигровыми проектами см. [в этом пошаговом руководстве по созданию проекта кросс-платформенной игры](~/graphics-games/monogame/introduction/part1.md).

Прежде чем переходить к нам, необходимо убедиться, что проект открыт и правильно развернут. После развертывания должен отобразиться пустой синий экран:

![Пустой синий экран игры](part1-images/image2.png)

## <a name="including-the-xnbs-in-the-game-project"></a>Включение Кснбс в проект игры

Формат файла. XNB-— это стандартное расширение для сборки содержимого (содержимое, созданное [средством «конвейерная Pipeline»](http://www.monogame.net/documentation/?page=Pipeline)). Все построенное содержимое имеет исходный файл (FBX-файл в случае нашей модели) и файл назначения (XNB--файл). Формат. FBX — это общий формат трехмерной модели, который может создаваться такими приложениями, как [Maya](https://www.autodesk.com/products/maya/overview) и [Blend](https://www.blender.org/). 

`Model`Класс может быть создан путем загрузки XNB--файла с диска, который содержит данные трехмерной геометрии.   Этот XNB--файл создается с помощью проекта содержимого. Шаблоны с использованием проектов автоматически включают в себя проект содержимого (с расширением мгкп) в папке содержимого. Подробное обсуждение этого средства см. в статье [Путеводитель по конвейеру содержимого](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/content-pipeline/introduction.md).

В этом пошаговом окне мы пропустим использование инструмента «одноигровой конвейер» и будем использовать. Файлы XNB-, содержащиеся здесь. Обратите внимание, что. XNB-файлы различаются для каждой платформы, поэтому обязательно используйте правильный набор файлов XNB-для любой платформы, с которой вы работаете.

Мы распакуйте [ файлContent.zip](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) , чтобы мы могли использовать содержащиеся в нашей игре файлы XNB-. При работе с проектом Android щелкните правой кнопкой мыши папку **Assets** в проекте **валкинггаме. Android** . При работе с проектом iOS щелкните правой кнопкой мыши проект **валкинггаме. iOS** . Выберите **Добавить->добавить файлы...** и выберите оба файла. XNB-в папке для платформы, над которой вы работаете.

Теперь оба файла должны быть частью нашего проекта:

![Папка содержимого обозревателя решений с файлами XNB-](part1-images/xnbsinxs.png)

Visual Studio для Mac не может автоматически устанавливать действие сборки для вновь добавленных Кснбс. Для iOS щелкните правой кнопкой мыши каждый из файлов и выберите **действие сборки — >BundleResource**. Для Android щелкните правой кнопкой мыши каждый из файлов и выберите **действие сборки — >AndroidAsset**.

## <a name="rendering-a-3d-model"></a>Отрисовка трехмерной модели

Последний шаг, необходимый для просмотра модели на экране, — Добавление кода загрузки и рисования. В частности, мы будем делать следующее:

- Определение `Model` экземпляра в нашем `Game1` классе
- Загрузка `Model` экземпляра в `Game1.LoadContent`
- Рисование `Model` экземпляра в `Game1.Draw`

Замените `Game1.cs` файл кода (который находится в **валкинггаме** PCL) следующим кодом:

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;

    // This is the model instance that we'll load
    // our XNB into:
    Model model;

    public Game1()
    {
        graphics = new GraphicsDeviceManager(this);
        graphics.IsFullScreen = true;

        Content.RootDirectory = "Content";
    }
    protected override void LoadContent()
    {
        // Notice that loading a model is very similar
        // to loading any other XNB (like a Texture2D).
        // The only difference is the generic type.
        model = Content.Load<Model> ("robot");
    }

    protected override void Update(GameTime gameTime)
    {
        base.Update(gameTime);
    }

    protected override void Draw(GameTime gameTime)
    {
        GraphicsDevice.Clear(Color.CornflowerBlue);

        // A model is composed of "Meshes" which are
        // parts of the model which can be positioned
        // independently, which can use different textures,
        // and which can have different rendering states
        // such as lighting applied.
        foreach (var mesh in model.Meshes)
        {
            // "Effect" refers to a shader. Each mesh may
            // have multiple shaders applied to it for more
            // advanced visuals. 
            foreach (BasicEffect effect in mesh.Effects)
            {
                // We could set up custom lights, but this
                // is the quickest way to get somethign on screen:
                effect.EnableDefaultLighting ();
                // This makes lighting look more realistic on
                // round surfaces, but at a slight performance cost:
                effect.PreferPerPixelLighting = true;

                // The world matrix can be used to position, rotate
                // or resize (scale) the model. Identity means that
                // the model is unrotated, drawn at the origin, and
                // its size is unchanged from the loaded content file.
                effect.World = Matrix.Identity;

                // Move the camera 8 units away from the origin:
                var cameraPosition = new Vector3 (0, 8, 0);
                // Tell the camera to look at the origin:
                var cameraLookAtVector = Vector3.Zero;
                // Tell the camera that positive Z is up
                var cameraUpVector = Vector3.UnitZ;

                effect.View = Matrix.CreateLookAt (
                    cameraPosition, cameraLookAtVector, cameraUpVector);

                // We want the aspect ratio of our display to match
                // the entire screen's aspect ratio:
                float aspectRatio = 
                    graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
                // Field of view measures how wide of a view our camera has.
                // Increasing this value means it has a wider view, making everything
                // on screen smaller. This is conceptually the same as "zooming out".
                // It also 
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                // Anything closer than this will not be drawn (will be clipped)
                float nearClipPlane = 1;
                // Anything further than this will not be drawn (will be clipped)
                float farClipPlane = 200;

                effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

            }

            // Now that we've assigned our properties on the effects we can
            // draw the entire mesh
            mesh.Draw ();
        }
        base.Draw(gameTime);
    }
}
```

Если запустить этот код, отобразится модель на экране:

![Модель, отображаемая на экране](part1-images/image8.png "Если этот код выполняется, модель будет отображаться на экране")

### <a name="model-class"></a>Класс Model

`Model`Класс является основным классом для выполнения трехмерной отрисовки из файлов содержимого (таких как FBX-файлы). Он содержит все сведения, необходимые для отрисовки, включая трехмерную геометрию, ссылки на текстуры и `BasicEffect` экземпляры, которые управляют размещением, освещением и значениями камеры.

`Model`Сам класс не имеет переменных для позиционирования напрямую, так как один экземпляр модели может быть визуализирован в нескольких местах, как будет показано далее в этом пошаговом окне.

Каждый `Model` из них состоит из одного или нескольких `ModelMesh` экземпляров, которые доступны через `Meshes` свойство. Хотя мы можем рассматривать `Model` как один объект игры (например, робот или автомобиль), каждый из них `ModelMesh` может быть нарисован различными `BasicEffect` значениями. Например, отдельные части сетки могут представлять конечное положение робота или колеса на автомобиле, и мы можем присвоить значения, `BasicEffect` чтобы сделать колеса прокруткой или перемещением. 

### <a name="basiceffect-class"></a>Класс Басицеффект

`BasicEffect`Класс предоставляет свойства для управления параметрами отрисовки. Первое изменение, которое мы сделаем в, `BasicEffect` — вызов `EnableDefaultLighting` метода. Как следует из названия, это позволяет включить освещение по умолчанию, что очень удобно для проверки того, что `Model` отображается в игре, как ожидалось. Если закомментировать `EnableDefaultLighting` вызов, мы увидим, что модель отображается только с текстурой, но без заливки или отраженного свечения:

```csharp
//effect.EnableDefaultLighting ();
```

![Модель, отображаемая только с текстурой, но без заливки или отраженного свечения](part1-images/image9.png "Модель, отображаемая только с текстурой, но без заливки или отраженного свечения")

`World`Свойство можно использовать для корректировки расположения, вращения и масштаба модели. В приведенном выше коде используется значение, означающее, `Matrix.Identity` что `Model` компонент будет отображаться в игре точно так же, как указано в FBX-файле. В [части 3](~/graphics-games/monogame/3d/part3.md)мы будем рассматривать матрицы и трехмерные координаты более подробно, но в качестве примера можно изменить расположение объекта `Model` , изменив `World` свойство следующим образом:

```csharp
// Z is up, so changing Z to 3 moves the object up 3 units:
var modelPosition = new Vector3 (0, 0, 3);
effect.World = Matrix.CreateTranslation (modelPosition); 
```

Этот код приводит к перемещению объекта вверх на 3 единицы мира:

![Этот код приводит к перемещению объекта вверх на 3 единицы мира](part1-images/image10.png "Этот код приводит к перемещению объекта вверх на 3 единицы мира")

Последние два свойства, назначенные в, `BasicEffect` — `View` и `Projection` . Мы будем рассматривать трехмерные камеры в [части 3](~/graphics-games/monogame/3d/part3.md), но в качестве примера можно изменить расположение камеры, изменив локальную `cameraPosition` переменную:

```csharp
// The 8 has been changed to a 30 to move the Camera further back
var cameraPosition = new Vector3 (0, 30, 0);
```

Мы можем увидеть, что Камера перешла назад, что приведет к `Model` уменьшению масштаба из-за перспективы:

![Камера перешла назад, в результате чего модель будет отображаться меньше из-за перспективы](part1-images/image11.png "Камера перешла назад, в результате чего модель будет отображаться меньше из-за перспективы")

## <a name="rendering-multiple-models"></a>Визуализация нескольких моделей

Как упоминалось выше, один объект `Model` может быть нарисован несколько раз. Чтобы упростить эту задачу, мы перемещая `Model` код рисования в собственный метод, который принимает требуемую `Model` точку в качестве параметра. По завершении `Draw` методы и `DrawModel` будут выглядеть следующим образом:

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);
    DrawModel (new Vector3 (-4, 0, 0));
    DrawModel (new Vector3 ( 0, 0, 0));
    DrawModel (new Vector3 ( 4, 0, 0));
    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));
    base.Draw(gameTime);
}
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            effect.World = Matrix.CreateTranslation (modelPosition);
            var cameraPosition = new Vector3 (0, 10, 0);
            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;
            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);
            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;
            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
        }
        // Now that we've assigned our properties on the effects we can
        // draw the entire mesh
        mesh.Draw ();
    }
}
```

В результате модель робота будет отображаться шесть раз:

![В результате модель робота будет отображаться шесть раз.](part1-images/image1.png "В результате модель робота будет отображаться шесть раз.")

## <a name="summary"></a>Сводка

В этом пошаговом руководстве появился класс с коигру `Model` . В нем рассматривается преобразование FBX-файла в XNB-, который может быть включен в `Model` класс. В нем также показано, как изменения в `BasicEffect` экземпляре могут повлиять на `Model` Рисование.

## <a name="related-links"></a>Связанные ссылки

- [Справочник по модели с неигровыми моделями](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Graphics_Model)
- [Content.zip](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)
- [Завершенный проект (пример)](/samples/xamarin/mobile-samples/modelrenderingmg/)