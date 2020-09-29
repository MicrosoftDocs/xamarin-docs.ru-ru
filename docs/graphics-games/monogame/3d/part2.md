---
title: Рисование трехмерной графики с вершинами в одноигре
description: В коигру поддерживается использование массивов вершин для определения способа отрисовки трехмерного объекта на уровне отдельных точек. Пользователи могут воспользоваться массивами вершин, чтобы создать динамическую геометрию, реализовать специальные эффекты и повысить эффективность их отрисовки посредством отбора.
ms.prod: xamarin
ms.assetid: 932AF5C2-884D-46E1-9455-4C359FD7C092
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: ccec89253c58d7d43cb09f33777c253a2f0da56d
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91431119"
---
# <a name="drawing-3d-graphics-with-vertices-in-monogame"></a>Рисование трехмерной графики с вершинами в одноигре

_В коигру поддерживается использование массивов вершин для определения способа отрисовки трехмерного объекта на уровне отдельных точек. Пользователи могут воспользоваться массивами вершин, чтобы создать динамическую геометрию, реализовать специальные эффекты и повысить эффективность их отрисовки посредством отбора._

Пользователи, прошедшие чтение [руководств по моделям отрисовки](~/graphics-games/monogame/3d/part1.md) , будут знакомы с отрисовкой трехмерной модели в виде «коигру». `Model`Класс является эффективным способом отрисовки трехмерной графики при работе с данными, определенными в файле (например, FBX), и при работе со статическими данными. Для некоторых игр необходимо, чтобы трехмерная геометрия была определена или обработана динамически во время выполнения. В этих случаях можно использовать массивы *вершин* для определения и отрисовки геометрии. Вершина — это общий термин для точки в трехмерном пространстве, которая является частью упорядоченного списка, используемого для определения геометрии. Обычно вершины упорядочиваются таким образом, чтобы определить ряд треугольников.

Чтобы помочь визуализировать, как вершины используются для создания трехмерных объектов, рассмотрим следующую сферу:

![Чтобы помочь визуализировать, как вершины используются для создания трехмерных объектов, рассмотрите эту сферу](part2-images/image1.png)

Как показано выше, сфера четко состоит из нескольких треугольников. Можно просмотреть каркас сферы, чтобы увидеть, как вершины соединяются с треугольниками формы:

![Просмотр каркаса сферы, чтобы увидеть, как вершины соединяются с формами треугольников](part2-images/image2.png)

В этом пошаговом руководстве будут рассмотрены следующие темы:

- Создание проекта
- Создание вершин
- Добавление кода рисования
- Отрисовка с помощью текстуры
- Изменение координат текстуры
- Отрисовка вершин с помощью моделей

Готовый проект будет содержать шашки, которые будут выводиться с помощью массива вершин.

![Готовый проект будет содержать шашки, которые будут выводиться с помощью массива вершин.](part2-images/image3.png)

## <a name="creating-a-project"></a>Создание проекта

Сначала мы скачиваем проект, который будет использоваться в качестве отправной точки. Мы будем использовать проект модели [, который можно найти здесь](/samples/xamarin/mobile-samples/modelrenderingmg/).

После скачивания и распаковки откройте и запустите проект. Мы планируем увидеть шесть моделей роботов, отображаемых на экране:

![Шесть моделей роботов, отображаемых на экране](part2-images/image4.png)

К концу этого проекта мы будем объединять собственную пользовательскую отрисовку вершин с помощью робота `Model` , поэтому мы не будем удалять код рендеринга робота. Вместо этого мы просто удалим вызов, `Game1.Draw` чтобы удалить изображение из 6 роботов. Для этого откройте файл **Game1.CS** и выберите `Draw` метод. Измените его, чтобы он содержал следующий код:

```csharp
protected override void Draw(GameTime gameTime)
{
  GraphicsDevice.Clear(Color.CornflowerBlue);
  base.Draw(gameTime);
}
```

Это приведет к тому, что в игре отобразится пустой синий экран:

![Это приведет к тому, что игра отобразит пустой синий экран](part2-images/image5.png)

## <a name="creating-the-vertices"></a>Создание вершин

Мы создадим массив вершин для определения геометрии. В этом пошаговом руководстве мы создадим трехмерную плоскость (квадрат в трехмерном пространстве, а не самолете). Хотя наша плоскость содержит четыре стороны и четыре угла, она будет состоять из двух треугольников, каждая из которых требует трех вершин. Поэтому мы будем определять сумму из шести точек.

До сих пор мы говорили о вершинах в общем смысле, но в игре есть несколько стандартных структур, которые можно использовать для вершин:

- `Microsoft.Xna.Framework.Graphics.VertexPositionColor`
- `Microsoft.Xna.Framework.Graphics.VertexPositionColorTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionNormalTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionTexture`

Имя каждого типа указывает компоненты, которые он содержит. Например, `VertexPositionColor` содержит значения для параметров «расположение» и «цвет». Рассмотрим каждый из компонентов:

- "Расположение" — все типы вершин включают `Position` компонент. `Position`Значения определяют, где вершина расположена в трехмерном пространстве (X, Y и Z).
- Цвет — в вершинах при необходимости можно указать `Color` значение для выполнения настраиваемого оттенка.
- Обычная — нормали определяют, каким образом поверхность объекта направлена. Нормали необходимы, если отрисовка объекта с освещением с помощью освещения, направленной на поверхность поверхности, влияет на степень ее получения. Нормали обычно задаются как *векторы единиц измерения* — трехмерный вектор, длина которого равна 1.
- Текстура — текстура — это координаты текстуры, т. е. какая часть текстуры должна отображаться в определенной вершине. Значения текстур необходимы, если для отрисовки трехмерного объекта используется текстура. Координаты текстуры — это нормализованные координаты. Это означает, что значения будут находиться в диапазоне от 0 до 1. Далее в этом пошаговом окне мы рассмотрим координаты текстуры более подробно.

Наша плоскость выступает в качестве основания, и мы хотим применить текстуру при подготовке к просмотру, поэтому мы будем использовать этот `VertexPositionTexture` тип для определения вершин.

Сначала мы добавим член в наш класс Game1:

```csharp
VertexPositionTexture[] floorVerts;
```

Затем определите наши вершины в `Game1.Initialize` . Обратите внимание, что указанный шаблон, упомянутый ранее в этой статье `Game1.Initialize` , не содержит метод, поэтому нам нужно добавить весь метод в `Game1` :

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];
    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);
    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;
    // We’ll be assigning texture values later
    base.Initialize ();
}
```

Чтобы помочь визуализировать, как будут выглядеть наши вершины, рассмотрим следующую схему:

![Чтобы помочь визуализировать, как будут выглядеть вершины, рассмотрим эту схему.](part2-images/image6.png)

Нам нужно полагаться на нашу диаграмму, чтобы визуализировать вершины до завершения реализации нашего кода отрисовки.

## <a name="adding-drawing-code"></a>Добавление кода рисования

Теперь, когда у нас есть позиции для определенной геометрии, мы можем написать наш код отрисовки.

Во-первых, необходимо определить `BasicEffect` экземпляр, который будет содержать параметры для отрисовки, такие как расположение и освещение. Для этого добавьте `BasicEffect` элемент в `Game1` класс ниже, где `floorVerts` определено поле:

```csharp
...
VertexPositionTexture[] floorVerts;
// new code:
BasicEffect effect;
```

Затем измените метод, `Initialize` чтобы определить результат:

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;
    // new code:
    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

Теперь мы можем добавить код для выполнения рисования:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
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

    foreach (var pass in effect.CurrentTechnique.Passes)
    {
        pass.Apply ();

        graphics.GraphicsDevice.DrawUserPrimitives (
            // We’ll be rendering two trinalges
            PrimitiveType.TriangleList,
            // The array of verts that we want to render
            floorVerts,
            // The offset, which is 0 since we want to start
            // at the beginning of the floorVerts array
            0,
            // The number of triangles to draw
            2);
    }
}
```

Нам нужно будет позвонить `DrawGround` в `Game1.Draw` :

```csharp
protected override void Draw (GameTime gameTime)
{
    GraphicsDevice.Clear (Color.CornflowerBlue);

    DrawGround ();

    base.Draw (gameTime);
}
```

При выполнении приложение отобразит следующее:

![Приложение будет отображать это при выполнении](part2-images/image7.png)

Рассмотрим некоторые сведения в приведенном выше коде.

### <a name="view-and-projection-properties"></a>Свойства представления и проекции

`View`Свойства и `Projection` управляют отображением сцены. Этот код будет изменен позже при повторном добавлении кода визуализации модели. В частности, `View` управляет расположением и ориентацией камеры и `Projection` управляет *полем представления* (которое может использоваться для масштабирования камеры).

### <a name="techniques-and-passes"></a>Методы и проходы

После назначения свойств по нашему результату можно выполнить фактическую отрисовку.

Мы не будем менять `CurrentTechnique` свойство в этом пошаговом руководстве, но более сложные игры могут иметь один результат, который может выполнять рисование различными способами (например, применение значения цвета). Каждый из этих режимов подготовки можно представить в виде метода, который можно назначить перед отрисовкой. Кроме того, для правильного отображения каждого метода может потребоваться несколько проходов. Эффекты могут потребовать нескольких проходов при отрисовке сложных визуальных элементов, таких как свечение или шерсть.

Важно помнить, что `foreach` цикл позволяет использовать один и тот же код C# для отображения любого воздействия независимо от сложности базового класса `BasicEffect` .

### <a name="drawuserprimitives"></a>дравусерпримитивес

`DrawUserPrimitives` , где отображаются вершины. Первый параметр указывает методу, как мы организовали вершины. Мы разстроили их так, чтобы каждый треугольник определялся тремя упорядоченными вершинами, поэтому мы используем `PrimitiveType.TriangleList` значение.

Вторым параметром является массив вершин, которые мы определили ранее.

Третий параметр задает первый индекс для рисования. Так как мы хотим, чтобы весь массив вершин был визуализирован, мы передаем значение 0.

Наконец, мы указываем число отображаемых треугольников. Наш массив вершин содержит два треугольника, поэтому передайте значение 2.

## <a name="rendering-with-a-texture"></a>Отрисовка с помощью текстуры

На этом этапе наше приложение визуализирует белую плоскость (в перспективе). Далее мы добавим в наш проект текстуру, которая будет использоваться при подготовке к просмотру плоскости.

Для простоты мы добавим PNG-файлы непосредственно в наш проект, а не с помощью инструмента для конвейера «непрерывной игры». Для этого Скачайте PNG- [файл](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true) на компьютер. После скачивания щелкните правой кнопкой мыши папку **содержимое** на панели решения и выберите **Добавить>добавить файлы...** . При работе с Android эта папка будет находиться в папке **Assets** в проекте, относящемся к Android. Если в iOS, то эта папка будет находиться в корне проекта iOS. Перейдите в расположение, где сохранен **checkerboard.png** , и выберите этот файл. Выберите, чтобы скопировать файл в каталог.

Далее мы добавим код для создания нашего `Texture2D` экземпляра. Сначала добавьте в `Texture2D` качестве члена в `Game1` `BasicEffect` экземпляре.

```csharp
...
BasicEffect effect;
// new code:
Texture2D checkerboardTexture;
```

Измените `Game1.LoadContent` следующим образом:

```csharp
protected override void LoadContent()
{
    // Notice that loading a model is very similar
    // to loading any other XNB (like a Texture2D).
    // The only difference is the generic type.
    model = Content.Load<Model> ("robot");

    // We aren't using the content pipeline, so we need
    // to access the stream directly:
    using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
    {
        checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
    }
}
```

Затем измените `DrawGround` метод. Единственное изменение необходимо присвоить `effect.TextureEnabled` `true` параметру и установить для значения `effect.Texture` `checkerboardTexture` :

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
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

    // new code:
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
```

Наконец, необходимо изменить `Game1.Initialize` метод, чтобы назначить координаты текстуры в вершинах:

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;

    // New code:
    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, 1);
    floorVerts [2].TextureCoordinate = new Vector2 (1, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (1, 1);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

Если мы выполним код, мы видим, что в нашей плоскости теперь отображается шаблон шахматной доски:

![Теперь плоскость отображает шаблон шахматной доски](part2-images/image8.png)

## <a name="modifying-texture-coordinates"></a>Изменение координат текстуры

В игре используются нормализованные координаты текстуры, которые являются координатами между 0 и 1, а не между 0 и шириной или высотой текстуры. Следующая диаграмма поможет визуализировать нормализованные координаты:

![Эта диаграмма поможет визуализировать нормализованные координаты](part2-images/image9.png)

Нормализованные координаты текстуры позволяют изменять размер текстуры без необходимости переписывать код или повторно создавать модели (например, FBX-файлы). Это возможно, так как нормализованные координаты представляют отношение, а не конкретные Пиксели. Например, (1, 1) всегда будет представлять правый нижний угол независимо от размера текстуры.

Можно изменить назначение координат текстуры, чтобы использовать одну переменную для числа повторений:

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

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

    base.Initialize ();
}
```

В результате текстура повторяется 20 раз:

![Это приводит к повторению текстуры в 20 раз](part2-images/image10.png)

## <a name="rendering-vertices-with-models"></a>Отрисовка вершин с помощью моделей

Теперь, когда плоскость отображается правильно, мы можем повторно добавить модели, чтобы просмотреть все вместе. Сначала мы повторно добавим код модели в наш `Game1.Draw` метод (с измененными положениями):

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    DrawGround ();

    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));

    DrawModel (new Vector3 (-4, 4, 3));
    DrawModel (new Vector3 ( 0, 4, 3));
    DrawModel (new Vector3 ( 4, 4, 3));

    base.Draw(gameTime);
}
```

Кроме того, мы создадим `Vector3` в `Game1` для представления расположения камеры. Мы добавим поле в нашем `checkerboardTexture` объявлении:

```csharp
...
Texture2D checkerboardTexture;
// new code:
Vector3 cameraPosition = new Vector3(0, 10, 10);
```

Затем удалите локальную `cameraPosition` переменную из `DrawModel` метода:

```csharp
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = Matrix.CreateTranslation (modelPosition);

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);
            ...
```

Аналогичным образом удалите локальную `cameraPosition` переменную из `DrawGround` метода:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);
    ...
```

Теперь, если мы выполним код, можно увидеть обе модели и земля в одно и то же время:

![Одновременно отображаются обе модели и земля](part2-images/image11.png)

Если изменить расположение камеры (например, увеличив значение X, которое в данном случае перемещает камеру влево), можно увидеть, что значение влияет как на заземление, так и на модели:

```csharp
Vector3 cameraPosition = new Vector3(15, 10, 10);
```

Этот код приводит к следующему результату:

![Этот код приводит к последующему представлению](part2-images/image3.png)

## <a name="summary"></a>Сводка

В этом пошаговом руководстве показано, как использовать массив вершин для выполнения пользовательской отрисовки. В этом случае мы создали шашку, которая объединяет нашу визуализацию на основе вершин с текстурой `BasicEffect` , но представленный здесь код служит основанием для любой трехмерной отрисовки. Мы также показали, что отрисовка на основе вершин может быть смешана с моделями в одной сцене.

## <a name="related-links"></a>Связанные ссылки

- [Файл шахматной доски (пример)](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true)
- [Завершенный проект (пример)](/samples/xamarin/mobile-samples/modelsandvertsmg/)