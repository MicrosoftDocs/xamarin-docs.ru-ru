---
title: Трехмерные координаты в коигру
description: Понимание трехмерной системы координат является важным шагом при разработке трехмерных игр. В игре предусмотрено несколько классов для позиционирования, ориентации и масштабирования объектов в трехмерном пространстве.
ms.prod: xamarin
ms.assetid: A4130995-48FD-4E2E-9C2B-ADCEFF35BE3A
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 54a4c6e32059b6ff32b3a93abf5fd30c65f16b5f
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936634"
---
# <a name="3d-coordinates-in-monogame"></a>Трехмерные координаты в коигру

_Понимание трехмерной системы координат является важным шагом при разработке трехмерных игр. В игре предусмотрено несколько классов для позиционирования, ориентации и масштабирования объектов в трехмерном пространстве._

Для разработки трехмерных игр необходимо понимать, как работать с объектами в трехмерной системе координат. В этом пошаговом руководстве рассматривается обработка визуальных объектов (в частности, модели). Мы создадим концепцию управления моделью для создания класса 3D Camera.

Представленные концепции берутся из линейной перебора, но мы будем использовать практический подход, чтобы любой пользователь без сложных математических вычислений мог применить эти концепции в своих играх.

Мы будем обсуждать следующие темы:

- Создание проекта
- Создание сущности робота
- Перемещение объекта робота
- умножение матриц
- Создание сущности камеры
- Перемещение камеры с входными данными

По завершении у нас будет проект с роботом, перемещающийся на окружность и камеру, которыми можно управлять с помощью сенсорного ввода:

![По завершении приложение будет включать проект с роботом, перемещающимся на окружность и камеру, которыми можно управлять с помощью сенсорного ввода.](part3-images/image1.gif)

## <a name="creating-a-project"></a>Создание проекта

В этом пошаговом руководстве рассматривается перемещение объектов в трехмерном пространстве. Начнем с проекта для подготовки к просмотру моделей и массивов вершин [, которые можно найти здесь](https://docs.microsoft.com/samples/xamarin/mobile-samples/modelsandvertsmg/). После скачивания распакуйте и откройте проект, чтобы убедиться, что он выполняется, и мы должны увидеть следующее:

![После скачивания распакуйте и откройте проект, чтобы убедиться, что он выполняется, и это представление должно отображаться.](part3-images/image2.png)

## <a name="creating-a-robot-entity"></a>Создание сущности робота

Прежде чем приступить к перемещению нашего робота, мы создадим `Robot` класс, который будет содержать логику для рисования и перемещения. Разработчики игр обращаются к этой инкапсуляции логики и данных в виде *сущности*.

Добавьте новый пустой файл класса в переносимую библиотеку классов **MonoGame3D** (не зависящую от платформы Моделандвертс. Android). Назовите его **робота** и нажмите кнопку **создать**:

![Назовите его робота и нажмите кнопку Создать.](part3-images/image3.png)

Измените `Robot` класс следующим образом:

```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Content;

namespace MonoGame3D
{
    public class Robot
    {
        Model model;

        public void Initialize(ContentManager contentManager)
        {
            model = contentManager.Load<Model> ("robot");

        }

        // For now we'll take these values in, eventually we'll
        // take a Camera object
        public void Draw(Vector3 cameraPosition, float aspectRatio)
        {
            foreach (var mesh in model.Meshes)
            {
                foreach (BasicEffect effect in mesh.Effects)
                {
                    effect.EnableDefaultLighting ();
                    effect.PreferPerPixelLighting = true;

                    effect.World = Matrix.Identity; 
                    var cameraLookAtVector = Vector3.Zero;
                    var cameraUpVector = Vector3.UnitZ;

                    effect.View = Matrix.CreateLookAt (
                        cameraPosition, cameraLookAtVector, cameraUpVector);

                    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                    float nearClipPlane = 1;
                    float farClipPlane = 200;

                    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

                }

                // Now that we've assigned our properties on the effects we can
                // draw the entire mesh
                mesh.Draw ();
            }
        }
    }
}
```

`Robot`Код по сути является тем же кодом в `Game1` для рисования `Model` . Дополнительные сведения о `Model` загрузке и рисовании см. [в этом документе в этом разделе о работе с моделями](~/graphics-games/monogame/3d/part1.md). Теперь можно удалить весь `Model` код загрузки и отрисовки из и `Game1` заменить его `Robot` экземпляром:

```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        Vector3 cameraPosition = new Vector3(15, 10, 10);

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;

            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

            floorVerts [0].Position = new Vector3 (-20, -20, 0);
            floorVerts [1].Position = new Vector3 (-20,  20, 0);
            floorVerts [2].Position = new Vector3 ( 20, -20, 0);

            floorVerts [3].Position = floorVerts[1].Position;
            floorVerts [4].Position = new Vector3 ( 20,  20, 0);
            floorVerts [5].Position = floorVerts[2].Position;

            int repetitions = 20;

            floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
            floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
            floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

            floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
            floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
            floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

            effect = new BasicEffect (graphics.GraphicsDevice);

            robot = new Robot ();
            robot.Initialize (Content);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            robot.Draw (cameraPosition, aspectRatio);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);

            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;

            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

            effect.TextureEnabled = true;
            effect.Texture = checkerboardTexture;

            foreach (var pass in effect.CurrentTechnique.Passes)
            {
                pass.Apply ();

                graphics.GraphicsDevice.DrawUserPrimitives (
                            PrimitiveType.TriangleList,
                    floorVerts,
                    0,
                    2);
            }
        }
    }
}
```

Если мы выполним код, то у нас будет сцена с одним роботом, который рисуется главным образом под пол:

![Если код выполняется сейчас, приложение отобразит сцену с одним роботом, который рисуется главным образом под пол](part3-images/image4.png)

## <a name="moving-the-robot"></a>Перемещение робота

Теперь, когда у нас есть `Robot` класс, мы можем добавить логику перемещения в робот. В этом случае мы просто перейдем робота в круге в соответствии со временем игры. Это непрактичная реализация для реальной игры, так как символ обычно может реагировать на входные или искусственные аналитики, но он предоставляет среду для изучения трехмерного позиционирования и вращения.

Единственными сведениями, которые потребуются за пределами `Robot` класса, являются текущее время игры. Мы добавим `Update` метод, который примет `GameTime` параметр. Этот `GameTime` параметр будет использоваться для увеличения переменной угла, которая будет использоваться для определения конечной должности робота.

Сначала мы добавим поле Angle в `Robot` класс в `model` поле:

```csharp
public class Robot
{
    public Model model;

    // new code:
    float angle;
    ...
```

 Теперь можно увеличить это значение в `Update` функции:

```csharp
public void Update(GameTime gameTime)
{
    // TotalSeconds is a double so we need to cast to float
    angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
}
```

Нам нужно убедиться, что `Update` метод вызывается из `Game1.Update` :

```csharp
protected override void Update(GameTime gameTime)
{
    robot.Update (gameTime);
    base.Update(gameTime);
}
```

Конечно, на этом этапе поле угла не выполняет никаких действий — необходимо написать код для его использования. Мы изменим `Draw` метод, чтобы мы могли вычислить мир `Matrix` в выделенном методе: 

```csharp
public void Draw(Vector3 cameraPosition, float aspectRatio)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            // We’ll be doing our calculations here...
            effect.World = GetWorldMatrix();

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);

            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;

            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
        }

        mesh.Draw ();
    }
}
```

Далее мы реализуем `GetWorldMatrix` метод в `Robot` классе:

```csharp
Matrix GetWorldMatrix()
{
    const float circleRadius = 8;
    const float heightOffGround = 3;

    // this matrix moves the model "out" from the origin
    Matrix translationMatrix = Matrix.CreateTranslation (
        circleRadius, 0, heightOffGround);

    // this matrix rotates everything around the origin
    Matrix rotationMatrix = Matrix.CreateRotationZ (angle);

    // We combine the two to have the model move in a circle:
    Matrix combined = translationMatrix * rotationMatrix;

    return combined;
}
```

Результат выполнения этого кода приводит к тому, что робот перемещается по окружности:

![Выполнение этого кода приводит к перемещению робота в окружность](part3-images/image5.gif)

## <a name="matrix-multiplication"></a>умножение матриц

Приведенный выше код поворачивает робот, создавая `Matrix` в `GetWorldMatrix` методе. `Matrix`Структура содержит 16 значений float, которые можно использовать для перевода (задания расположения), вращения и масштабирования (Задание размера). При назначении `effect.World` свойства мы сообщаем базовой системе отрисовки, как разместить, изменить размер и ориентироваться на прорисовку (a `Model` или геометрию из вершин). 

К счастью, `Matrix` Структура включает ряд методов, упрощающих создание общих типов матриц. Первый использованный в приведенном выше коде — `Matrix.CreateTranslation` . *Преобразование* математического термина относится к операции, которая приводит к перемещению точки (или в нашем случае модели) из одного места в другое без каких-либо других изменений (например, при повороте или изменении размера). Функция принимает значение X, Y и Z для перевода. Если просмотреть сцену сверху вниз, наш `CreateTranslation` метод (в изоляции) выполняет следующие действия:

![Метод Креатетранслатион в режиме изоляции выполняет это действие](part3-images/image6.png)

Вторая создаваемая матрица — это матрица вращения, использующая `CreateRotationZ` матрицу. Это один из трех методов, которые можно использовать для создания вращения:

- `CreateRotationX`
- `CreateRoationY`
- `CreateRotationZ`

Каждый метод создает матрицу вращения путем поворота по заданной оси. В нашем случае мы вращающимися по оси Z, которая указывает на «up». Ниже приведены сведения о том, как работает вращение на основе осей.

![Это поможет визуализировать принципы работы вращения на основе осей.](part3-images/image7.png)

Мы также используем `CreateRotationZ` метод с полем угла, которое увеличивается с течением времени из-за вызова нашего `Update` метода. Результатом является то, что `CreateRotationZ` метод заставляет наш робот по орбите вокруг источника по мере прохода времени.

Последняя строка кода объединяет две матрицы в одну:

```csharp
Matrix combined = translationMatrix * rotationMatrix;
```

Это называется умножением матрицы, которое работает немного иначе, чем регулярное умножение. *Свойство коммутативной для умножения* указывает, что порядок чисел в операции умножения не изменяет результат. То есть 3 \* 4 эквивалентны 4 \* 3. Умножение матрицы отличается тем, что оно не коммутативной. Таким образом, приведенная выше строка может быть считана как «Apply Транслатионматрикс To Move a Model, а затем вращать все, применяя Ротатионматрикс». Можно визуализировать, как следующая строка влияет на расположение и поворот следующим образом:

![Общая PF. как указанная выше строка влияет на расположение и поворот](part3-images/image8.png)

Чтобы понять, как порядок умножения матрицы может повлиять на результат, рассмотрите следующее, где перемножение матрицы будет инвертировано:

```csharp
Matrix combined = rotationMatrix * translationMatrix;
```

Приведенный выше код сначала поместит модель на месте, а затем преобразует ее:

![Приведенный выше код сначала поместит модель на месте, а затем преобразует ее.](part3-images/image9.png)

При выполнении кода с инвертированным умножением мы заметите, что поскольку поворот применяется первыми, он влияет только на ориентацию модели, и положение модели остается неизменным. Иными словами, модель поворачивается на месте:

![Модель поворачивается на месте](part3-images/image10.gif)

## <a name="creating-the-camera-entity"></a>Создание сущности камеры

`Camera`Сущность будет содержать всю логику, необходимую для выполнения перемещения на основе ввода и предоставления свойств для назначения свойств `BasicEffect` классу.

Во-первых, мы реализуем статическую камеру (без перемещения на основе ввода) и свяжем ее с существующим проектом. Добавьте новый класс в переносимую библиотеку классов **MonoGame3D** (тот же проект с `Robot.cs` ) и назовите ее **Camera**. Замените содержимое файла на код, приведенный ниже.

```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Camera
    {
        // We need this to calculate the aspectRatio
        // in the ProjectionMatrix property.
        GraphicsDevice graphicsDevice;

        Vector3 position = new Vector3(15, 10, 10);

        public Matrix ViewMatrix
        {
            get
            {
                var lookAtVector = Vector3.Zero;
                var upVector = Vector3.UnitZ;

                return Matrix.CreateLookAt (
                    position, lookAtVector, upVector);
            }
        }

        public Matrix ProjectionMatrix
        {
            get
            {
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                float nearClipPlane = 1;
                float farClipPlane = 200;
                float aspectRatio = graphicsDevice.Viewport.Width / (float)graphicsDevice.Viewport.Height;

                return Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
            }
        }

        public Camera(GraphicsDevice graphicsDevice)
        {
            this.graphicsDevice = graphicsDevice;
        }

        public void Update(GameTime gameTime)
        {
            // We'll be doing some input-based movement here
        }
    }
}
```

Приведенный выше код очень похож на код из `Game1` и `Robot` , которому присваиваются матрицы `BasicEffect` . 

Теперь мы можем интегрировать новый `Camera` класс в существующие проекты. Во-первых, мы изменим `Robot` класс, чтобы взять `Camera` экземпляр в своем `Draw` методе, что приведет к исключению большого объема повторяющегося кода. Замените `Robot.Draw` метод следующим:

```csharp
public void Draw(Camera camera)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = GetWorldMatrix();
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;
        }

        mesh.Draw ();
    }
}
```

Затем измените `Game1.cs` файл:

```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        // New camera code
        Camera camera;

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;

            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

            floorVerts [0].Position = new Vector3 (-20, -20, 0);
            floorVerts [1].Position = new Vector3 (-20,  20, 0);
            floorVerts [2].Position = new Vector3 ( 20, -20, 0);

            floorVerts [3].Position = floorVerts[1].Position;
            floorVerts [4].Position = new Vector3 ( 20,  20, 0);
            floorVerts [5].Position = floorVerts[2].Position;

            int repetitions = 20;

            floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
            floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
            floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

            floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
            floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
            floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

            effect = new BasicEffect (graphics.GraphicsDevice);

            robot = new Robot ();
            robot.Initialize (Content);

            // New camera code
            camera = new Camera (graphics.GraphicsDevice);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            robot.Update (gameTime);
            // New camera code
            camera.Update (gameTime);
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            // New camera code
            robot.Draw (camera);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
            // New camera code
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;

            effect.TextureEnabled = true;
            effect.Texture = checkerboardTexture;

            foreach (var pass in effect.CurrentTechnique.Passes)
            {
                pass.Apply ();

                graphics.GraphicsDevice.DrawUserPrimitives (
                            PrimitiveType.TriangleList,
                    floorVerts,
                    0,
                    2);
            }
        }
    }
}
```

Изменения `Game1` из предыдущей версии (которая определяется с помощью `// New camera code` ):

- `Camera`поле в`Game1`
- `Camera`Создание экземпляра`Game1.Initialize`
- `Camera.Update`вызов в`Game1.Update`
- `Robot.Draw`Теперь принимает `Camera` параметр
- `Game1.Draw`Теперь использует `Camera.ViewMatrix` и`Camera.ProjectionMatrix`

## <a name="moving-the-camera-with-input"></a>Перемещение камеры с входными данными

До сих пор мы добавили `Camera` сущность, но не сделали ничего с ней, чтобы изменить поведение во время выполнения. Мы добавим поведение, которое позволит пользователю:

- Коснитесь левой стороны экрана, чтобы превратить камеру в сторону левого
- Коснитесь правой части экрана, чтобы повернуть камеру вправо
- Коснитесь центра экрана, чтобы переместить камеру вперед

### <a name="making-lookat-relative"></a>Создание относительных Лукат

Сначала мы изменим `Camera` класс, включив в него `angle` поле, которое будет использоваться для задания направления, направленного на сторону `Camera` . В настоящее время наш `Camera` способ определяет направление, с которым он проходит через локальную точку `lookAtVector` , назначенную `Vector3.Zero` . Другими словами, наш `Camera` всегда смотрит на источник. Если камера перемещается, будет также изменен угол, на который направлена камера.

![Если камера перемещается, будет также изменен угол, на который направлена камера.](part3-images/image11.gif)

Мы хотим, `Camera` чтобы объект был направлен в одно и то же направление независимо от его позиции — по крайней мере до реализации логики для поворота `Camera` с использованием входных данных. Первое изменение заключается в настройке `lookAtVector` переменной, которая будет основываться на текущем месте, вместо того, чтобы искать абсолютное положение:

```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    // Let's start at X = 0 so we're looking at things head-on
    Vector3 position = new Vector3(0, 20, 10);

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    }
    ...
```

Это приводит к `Camera` прямому просмотру мира. Обратите внимание, что начальное `position` значение было изменено `(0, 20, 10)` таким образом, чтобы `Camera` оно было центрировано по оси X. При запуске игры отображается следующее:

![При запуске игры отображается это представление](part3-images/image12.png)

### <a name="creating-an-angle-variable"></a>Создание переменной угла

`lookAtVector`Переменная определяет угол, на котором просматривается камера. В настоящее время она исправлена для просмотра отрицательной оси Y и немного наклонена вниз (из `-.5f` значения Z). Мы создадим `angle` переменную, которая будет использоваться для настройки `lookAtVector` Свойства. 

В предыдущих разделах этого пошагового руководства мы показали, что матрицы можно использовать для поворота способа рисования объектов. Можно также использовать матрицы для вращения векторов, например `lookAtVector` с помощью `Vector3.Transform` метода. 

Добавьте `angle` поле и измените `ViewMatrix` свойство следующим образом:

```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    Vector3 position = new Vector3(0, 20, 10);

    float angle;

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            // We'll create a rotation matrix using our angle
            var rotationMatrix = Matrix.CreateRotationZ (angle);
            // Then we'll modify the vector using this matrix:
            lookAtVector = Vector3.Transform (lookAtVector, rotationMatrix);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    }
    ...
```

### <a name="reading-input"></a>Чтение входных данных

`Camera`Теперь наша сущность может полностью контролироваться с помощью ее переменных "расположение" и "угол" — нам просто нужно изменить их в соответствии с вводом.

Во-первых, мы получаем `TouchPanel` состояние, чтобы найти место, в котором пользователь касается экрана. Дополнительные сведения об использовании `TouchPanel` класса см. [в справочнике по API таучпанел](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_Touch_TouchPanel).

Если пользователь касается левого угла, то мы изменим `angle` значение таким образом, чтобы `Camera` повернуть влево, а если пользователь касается правой трети, мы будем переворачивать другой способ. Если пользователь касается в среднем третьем углу экрана, мы перейдем `Camera` вперед.

Сначала добавьте оператор using для уточнения `TouchPanel` `TouchCollection` классов и в `Camera.cs` :

```csharp
using Microsoft.Xna.Framework.Input.Touch; 
```

Затем измените метод, `Update` чтобы прочесть сенсорную панель и `angle` `position` соответствующим образом настроить переменные и.

```csharp
public void Update(GameTime gameTime)
{
    TouchCollection touchCollection = TouchPanel.GetState();

    bool isTouchingScreen = touchCollection.Count > 0;
    if (isTouchingScreen)
    {
        var xPosition = touchCollection [0].Position.X;

        float xRatio = xPosition / (float)graphicsDevice.Viewport.Width;

        if (xRatio < 1 / 3.0f)
        {
            angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
        else if (xRatio < 2 / 3.0f)
        {
            var forwardVector = new Vector3 (0, -1, 0);

            var rotationMatrix = Matrix.CreateRotationZ (angle);
            forwardVector = Vector3.Transform (forwardVector, rotationMatrix);

            const float unitsPerSecond = 3;

            this.position += forwardVector * unitsPerSecond *
                (float)gameTime.ElapsedGameTime.TotalSeconds ;
        }
        else
        {
            angle -= (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
    }
}
```

Теперь `Camera` ответ будет отвечать на сенсорный ввод:

![Теперь камера реагирует на сенсорный ввод](part3-images/image1.gif)

Метод Update начинается с вызова метода `TouchPanel.GetState` , который возвращает коллекцию касаний. Хотя `TouchPanel.GetState` может возвращать несколько точек касания, для простоты мы будем беспокоиться только о первой.

Если пользователь касается экрана, код проверяет, находится ли первое касание в левой, средней или правой третьей части экрана. Левая и правая третья стороны поворачивают камеру, увеличивая или уменьшая `angle` переменную в соответствии со `TotalSeconds` значением (так что игра ведет себя одинаково независимо от частоты кадров).

Если пользователь касается Центральной трети экрана, то Камера перейдет вперед. Сначала это достигается путем получения прямого вектора, который изначально определен как указатель на отрицательную ось Y, затем поворачивается матрицей, созданной с помощью `Matrix.CreateRotationZ` , и `angle` значением. Наконец, `forwardVector` применяется к `position` использованию `unitsPerSecond` коэффициента.

## <a name="summary"></a>Сводка

В этом пошаговом руководстве описывается перемещение и поворот `Models` трехмерного пространства с помощью `Matrices` и `BasicEffect.World` Свойства. Такая форма перемещения предоставляет базу для перемещения объектов в трехмерных играх. В этом пошаговом руководстве также рассматривается реализация `Camera` сущности для просмотра мира из любой точки и угла.

## <a name="related-links"></a>Связанные ссылки

- [Ссылка на API-интерфейс на коигру](http://www.monogame.net/documentation/?page=api)
- [Завершенный проект (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/monogame3dcamera)
