---
title: Часть 2. Реализация Валкинггаме
description: В этом пошаговом руководстве показано, как добавить логику игры и содержимое в пустой проект, чтобы создать демонстрацию анимированного спрайта, перемещающегося с помощью сенсорного ввода.
ms.prod: xamarin
ms.assetid: F0622A01-DE7F-451A-A51F-129876AB6FFD
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 835beca6866f5d3f82ae48d1c99557ceafe360ba
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91429767"
---
# <a name="part-2--implementing-the-walkinggame"></a>Часть 2. Реализация Валкинггаме

_В этом пошаговом руководстве показано, как добавить логику игры и содержимое в пустой проект, чтобы создать демонстрацию анимированного спрайта, перемещающегося с помощью сенсорного ввода._

В предыдущих частях этого пошагового руководства было показано, как создавать пустые проекты с неигровыми проектами. Мы будем создавать эти предыдущие части, выполнив простую демонстрацию игры. В этом разделе содержатся следующие подразделы:

- Распаковка содержимого игры
- Общие сведения о классе
- Подготовка к просмотру нашего первого спрайта
- Создание Чарактерентити
- Добавление Чарактерентити в игру
- Создание класса анимации
- Добавление первой анимации в Чарактерентити
- Добавление перемещения к символу
- Сопоставление движения и анимации

## <a name="unzipping-our-game-content"></a>Распаковка содержимого игры

Прежде чем приступить к написанию кода, мы хотим распаковать наше *содержимое*игры. Разработчики игр часто используют термин *содержимое* для ссылки на файлы, не являющиеся файлами кода, которые обычно создаются визуальными исполнителями, проектировщиками игр или проектировщиками аудио. К распространенным типам содержимого относятся файлы, используемые для отображения визуальных элементов, воспроизведения звука или управления поведением искусственного интеллекта (ии). Из содержимого перспективы группы разработки игр обычно создаются не программистами.

Содержимое, используемое здесь, можно найти [на сайте GitHub](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true). Эти файлы будут скачаны в расположение, к которому мы будем обращаться далее в этом пошаговом руководстве.

## <a name="monogame-class-overview"></a>Общие сведения о классе

Для начала мы рассмотрим классы для занятий, используемые в базовой отрисовке:

- `SpriteBatch` — используется для рисования двухмерной графики на экране. *Спрайты* — это двумерные визуальные элементы, которые используются для отображения изображений на экране. `SpriteBatch`Объект может рисовать один спрайт за раз между его `Begin` `End` методами и, или несколько спрайтов могут быть сгруппированы или объединены в *пакет*.
- `Texture2D` — представляет объект Image во время выполнения. `Texture2D` экземпляры часто создаются из форматов файлов, таких как PNG или BMP, хотя их также можно создавать динамически во время выполнения. `Texture2D` экземпляры используются при подготовке к просмотру с помощью `SpriteBatch` экземпляров.
- `Vector2` — представляет позицию в двухмерной системе координат, которая часто используется для позиционирования визуальных объектов. Также включается `Vector3` и, `Vector4` но мы будем использовать только `Vector2` в этом пошаговом руководстве.
- `Rectangle` — 4-сторонняя область с положением, шириной и высотой. Мы будем использовать этот параметр, чтобы определить, какая часть нашего `Texture2D` будет отображаться при начале работы с анимациями.

Также следует обратить внимание на то, что эта возможность не поддерживается корпорацией Майкрософт, несмотря на ее пространство имен. `Microsoft.Xna`Пространство имен используется в игре, чтобы упростить перенос существующих проектов XNA в игру.

## <a name="rendering-our-first-sprite"></a>Подготовка к просмотру нашего первого спрайта

Далее мы нарисуем один спрайт на экран, чтобы продемонстрировать, как выполнить двухмерную отрисовку в виде «в игре».

### <a name="creating-a-texture2d"></a>Создание Texture2D

Необходимо создать `Texture2D` экземпляр, который будет использоваться при подготовке к просмотру спрайта. Все содержимое игры в конечном итоге содержится в папке с именем **Content,** расположенной в проекте для конкретной платформы. Совместно используемые проекты не могут содержать содержимое, так как содержимое должно использовать действия сборки, относящиеся к платформе. Папку Content можно найти в проекте iOS и в папке Assets в проекте Android.

Чтобы добавить содержимое нашей игры, щелкните правой кнопкой мыши папку **содержимое** и выберите **Добавить > добавить файлы...** Перейдите в расположение, куда был извлечен файл content.zip, и выберите **charactersheet.png** файл. При появлении запроса о том, как добавить файл в папку, следует выбрать параметр **копирования** :

![При появлении запроса о том, как добавить файл в папку, выберите параметр копирования.](part2-images/image1.png)

Папка Content теперь содержит файл charactersheet.png:

![Папка Content теперь содержит файл charactersheet.png](part2-images/image2.png)

Далее мы добавим код для загрузки файла charactersheet.png и создадим `Texture2D` . Для этого откройте `Game1.cs` файл и добавьте в класс Game1.CS следующее поле:

```csharp
Texture2D characterSheetTexture;
```

Далее мы создадим `characterSheetTexture` в `LoadContent` методе. Прежде чем все `LoadContent` методы модификаций выглядят следующим образом:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    // TODO: use this.Content to load your game content here
}
```

Следует отметить, что проект по умолчанию уже создает `spriteBatch` экземпляр для нас. Мы будем использовать это позднее, но мы не будем добавлять дополнительный код для подготовки к `spriteBatch` использованию. С другой стороны, наша `spriteSheetTexture` не требует инициализации, поэтому мы инициализируем его после `spriteBatch` создания:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
    {
        characterSheetTexture = Texture2D.FromStream (this.GraphicsDevice, stream);

    }
}
```

Теперь, когда у нас есть `SpriteBatch` экземпляр и `Texture2D` экземпляр, мы можем добавить наш код отрисовки в `Game1.Draw` метод:

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    spriteBatch.Begin ();

    Vector2 topLeftOfSprite = new Vector2 (50, 50);
    Color tintColor = Color.White;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);

    spriteBatch.End ();

    base.Draw(gameTime);
}
```

Теперь при запуске игры будет показан один спрайт, отображающий текстуру, созданную из charactersheet.png:

![Теперь при запуске игры будет показан один спрайт, отображающий текстуру, созданную из charactersheet.png](part2-images/image3.png)

## <a name="creating-the-characterentity"></a>Создание Чарактерентити

До сих пор мы добавили код для отрисовки одного спрайта на экран; Однако если бы нам пришлось разработать полную игру без создания каких-либо других классов, мы бы работали с проблемами организации кода.

### <a name="what-is-an-entity"></a>Что такое сущность?

Распространенным шаблоном организации кода игры является создание нового класса для каждого типа *сущности* игры. Сущность в разработке игр — это объект, который может содержать некоторые из следующих характеристик (не все являются обязательными):

- Визуальный элемент, такой как спрайт, текст или трехмерная модель
- Физика или каждое поведение кадра, например единица, патроллинг заданный путь или символ проигрывателя, отвечающий на ввод
- Может создаваться и уничтожаться динамически, например при включении и сборе с помощью проигрывателя.

Системы Организации могут быть сложными, и многие игровые модули предлагают классы, помогающие управлять сущностями. Мы будем реализовывать очень простую систему сущностей, поэтому стоит отметить, что для всех игр, как правило, требуется больше Организации, чем часть разработчика.

### <a name="defining-the-characterentity"></a>Определение Чарактерентити

Наша сущность, которую мы будем называть `CharacterEntity` , будет иметь следующие характеристики:

- Возможность загрузки собственного `Texture2D`
- Возможность самостоятельной отрисовки, включая, в которой содержатся вызывающие методы для обновления проходной анимации
- `X` и свойства Y для управления положением символа.
- Возможность обновления, в частности, для считывания значений с сенсорного экрана и настройки позиционирования соответствующим образом.

Чтобы добавить в `CharacterEntity` игру, щелкните правой кнопкой мыши проект **валкинггаме** и выберите команду **Добавить > новый файл...**. Выберите параметр **пустой класс** и назовите новый файл **чарактерентити**, а затем нажмите кнопку **создать**.

Сначала мы добавим возможность `CharacterEntity` загрузки, `Texture2D` а также для рисования самого себя. Будет изменен только что добавленный `CharacterEntity.cs` файл следующим образом:

```csharp
using System;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Input.Touch;

namespace WalkingGame
{
    public class CharacterEntity
    {
        static Texture2D characterSheetTexture;

        public float X
        {
            get;
            set;
        }

        public float Y
        {
            get;
            set;
        }

        public CharacterEntity (GraphicsDevice graphicsDevice)
        {
            if (characterSheetTexture == null)
            {
                using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
                {
                    characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
                }
            }
        }

        public void Draw(SpriteBatch spriteBatch)
        {
            Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
            Color tintColor = Color.White;
            spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);
        }
    }
}
```

Приведенный выше код добавляет ответственность за размещение, отрисовку и загрузку содержимого в `CharacterEntity` . Давайте рассмотрим некоторые соображения, которые потребовались в приведенном выше коде.

### <a name="why-are-x-and-y-floats"></a>Почему используются плавающие числа X и Y?

Разработчики, не знакомые с программированием игр, могут узнать, почему `float` используется тип, а не `int` или `double` . Причина заключается в том, что 32-разрядное значение чаще всего используется для позиционирования кода отрисовки нижнего уровня. Тип Double занимает 64 бит для точности, что редко требуется для позиционирования объектов. Хотя 32-разрядная разница может показаться незначительной, многие современные игры включают графические изображения, для которых требуется обработка десятков тысяч значений позиций каждый кадр (30 или 60 раз в секунду). Вырезание объема памяти, который должен перемещаться по графическому конвейеру на половину, может оказать значительное влияние на производительность игры.

`int`Тип данных может быть соответствующей единицей измерения для позиционирования, но может полагать ограничения на способ позиционирования сущностей. Например, использование целочисленных значений затрудняет реализацию плавного перемещения для игр, которые поддерживают масштаб или трехмерные камеры (которые разрешены `SpriteBatch` ).

Наконец, мы видим, что логика, которая перемещает наш символ вокруг экрана, будет делать это с помощью значений времени игры. Результат умножения скорости на количество времени, переданного в определенный кадр, редко приведет к целому числу, поэтому необходимо использовать `float` для `X` и `Y` .

### <a name="why-is-charactersheettexture-static"></a>Зачем Чарактершиттекстуре статический?

`characterSheetTexture` `Texture2D` Экземпляр представляет собой представление среды выполнения файла charactersheet.png. Иными словами, он содержит значения цвета для каждого пикселя, извлеченного из исходного файла **charactersheet.png** . Если наша игра создавала два `CharacterEntity` экземпляра, для каждой из них потребуется доступ к информации из charactersheet.png. В этом случае мы не хотим тратить затраты на загрузку charactersheet.png дважды, а также не хотим, чтобы в ОЗУ сохранялась копия памяти текстуры. Использование `static` поля позволяет нам совместно использовать одни и те же объекты `Texture2D` во всех `CharacterEntity` экземплярах.

Использование `static` членов для представления содержимого среды выполнения — упрощенное решение, которое не позволяет устранить проблемы, возникающие в больших играх, таких как выгрузка содержимого при переходе с одного уровня на другой. Более сложные решения, которые выходят за рамки данного пошагового руководства, включают использование конвейера содержимого или создание пользовательской системы управления содержимым.

### <a name="why-is-spritebatch-passed-as-an-argument-instead-of-instantiated-by-the-entity"></a>Почему SpriteBatch передается как аргумент, а не создается с помощью сущности?

`Draw`Метод, реализованный выше, принимает `SpriteBatch` аргумент, но, напротив, `characterSheetTexture` создается экземпляром `CharacterEntity` .

Причина этого заключается в том, что наиболее эффективная визуализация возможна, когда один и тот же `SpriteBatch` экземпляр используется для всех `Draw` вызовов, и когда все `Draw` вызовы выполняются между одним набором `Begin` вызовов и `End` . Разумеется, наша игра будет включать только один экземпляр сущности, но более сложные игры будут использовать преимущества шаблона, который позволяет нескольким сущностям применять один и тот же `SpriteBatch` экземпляр.

## <a name="adding-characterentity-to-the-game"></a>Добавление Чарактерентити в игру

Теперь, когда мы добавили наш `CharacterEntity` код для самостоятельной отрисовки, мы можем заменить код в `Game1.cs` для использования экземпляра этой новой сущности. Для этого замените `Texture2D` поле полем `CharacterEntity` в `Game1` :

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;
    SpriteBatch spriteBatch;
    // New code:
    CharacterEntity character;

    public Game1()
    {
    ...
```

Далее мы добавим экземпляр этой сущности в `Game1.Initialize` :

```csharp
protected override void Initialize()
{
    character = new CharacterEntity (this.GraphicsDevice);

    base.Initialize();
}
```

Также необходимо удалить `Texture2D` Создание из `LoadContent` , так как теперь оно обрабатывается внутри `CharacterEntity` . `Game1.LoadContent` должен выглядеть следующим образом:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
}
```

Стоит отметить, что несмотря на свое имя, `LoadContent` метод не является единственным местом, где можно загрузить содержимое. Многие игры реализуют загрузку содержимого в своем методе Initialize или даже в коде обновления как содержимое, может не понадобиться, пока игрок не достигнет определенной точки игры.

Наконец, мы можем изменить метод Draw следующим образом:

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    // We'll start all of our drawing here:
    spriteBatch.Begin ();

    // Now we can do any entity rendering:
    character.Draw(spriteBatch);
    // End renders all sprites to the screen:
    spriteBatch.End ();

    base.Draw(gameTime);
}
```

Если запустить игру, будет выведен символ. Поскольку X и Y по умолчанию имеют значение 0, то символ располагается в левом верхнем углу экрана:

![Поскольку X и Y по умолчанию имеют значение 0, то символ располагается в левом верхнем углу экрана.](part2-images/image4.png)

## <a name="creating-the-animation-class"></a>Создание класса анимации

В настоящее время в нашем примере `CharacterEntity` отображается полный файл **charactersheet.png** . Такое расположение нескольких образов в одном файле называется *листом спрайта*. Как правило, спрайт будет визуализировать только часть листа спрайтов. Мы изменим, `CharacterEntity` чтобы визуализировать часть этого **charactersheet.png**, и эта часть со временем изменится, чтобы отобразить анимацию прохода.

Мы создадим `Animation` класс для управления логикой и состоянием анимации чарактерентити. Класс анимации будет общим классом, который можно использовать для любой сущности, а не только для `CharacterEntity` анимации. Ultimate класс предоставит, `Animation` `Rectangle` который `CharacterEntity` будет использоваться при рисовании самого себя. Мы также создадим `AnimationFrame` класс, который будет использоваться для определения анимации.

### <a name="defining-animationframe"></a>Определение Аниматионфраме

`AnimationFrame` не будет содержать логику, связанную с анимацией. Мы будем использовать его только для хранения данных. Чтобы добавить `AnimationFrame` класс, щелкните правой кнопкой мыши общий проект **валкинггаме** и выберите команду **Добавить > новый файл....** Введите имя **аниматионфраме** и нажмите кнопку **New (создать** ). Мы изменим `AnimationFrame.cs` файл, чтобы он содержал следующий код:

```csharp
using System;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class AnimationFrame
    {
        public Rectangle SourceRectangle { get; set; }
        public TimeSpan Duration { get; set; }
    }
}
```

`AnimationFrame`Класс содержит два фрагмента информации:

- `SourceRectangle` — Определяет область, `Texture2D` которая будет отображаться объектом `AnimationFrame` . Это значение измеряется в пикселях с верхним левым значением (0, 0).
- `Duration` — Определяет, как долго `AnimationFrame` отображается при использовании в `Animation` .

### <a name="defining-animation"></a>Определение анимации

`Animation`Класс будет содержать объект, а `List<AnimationFrame>` также логику для переключения того, какой кадр в данный момент отображается в соответствии с количеством времени.

Чтобы добавить `Animation` класс, щелкните правой кнопкой мыши общий проект **валкинггаме** и выберите команду **Добавить > новый файл...**. Введите имя **анимации** и нажмите кнопку **создать** . Мы изменим `Animation.cs` файл, чтобы он содержал следующий код:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class Animation
    {
        List<AnimationFrame> frames = new List<AnimationFrame>();
        TimeSpan timeIntoAnimation;

        TimeSpan Duration
        {
            get
            {
                double totalSeconds = 0;
                foreach (var frame in frames)
                {
                    totalSeconds += frame.Duration.TotalSeconds;
                }

                return TimeSpan.FromSeconds (totalSeconds);
            }
        }

        public void AddFrame(Rectangle rectangle, TimeSpan duration)
        {
            AnimationFrame newFrame = new AnimationFrame()
            {
                SourceRectangle = rectangle,
                Duration = duration
            };

            frames.Add(newFrame);
        }

        public void Update(GameTime gameTime)
        {
            double secondsIntoAnimation = 
                timeIntoAnimation.TotalSeconds + gameTime.ElapsedGameTime.TotalSeconds;

            double remainder = secondsIntoAnimation % Duration.TotalSeconds;

            timeIntoAnimation = TimeSpan.FromSeconds (remainder);
        }
    }
}
```

Рассмотрим некоторые сведения из `Animation` класса.

### <a name="frames-list"></a>Список кадров

`frames`Элемент сохраняет данные для анимации. Код, который создает экземпляр анимации, будет добавлять `AnimationFrame` экземпляры в `frames` список с помощью `AddFrame` метода. Более полная реализация может предоставлять `public` методы или свойства для изменения `frames` , но мы будем ограничивать функциональные возможности добавления кадров в этом пошаговом руководстве.

### <a name="duration"></a>Длительность

Duration возвращает общую длительность, `Animation,` полученную путем добавления длительности всех содержащихся в нем `AnimationFrame` экземпляров. Это значение может быть кэшировано `AnimationFrame` , если было неизменяемым объектом, но, поскольку мы реализовали аниматионфраме в качестве класса, который можно изменить после добавления в анимацию, необходимо вычислить это значение при обращении к свойству.

### <a name="update"></a>Update

`Update`Метод должен вызываться каждый кадр (то есть каждый раз, когда обновляется вся игра). Его целью является увеличение элемента, `timeIntoAnimation` который используется для возврата текущего отображаемого кадра. Логика в `Update` не `timeIntoAnimation` всегда превышает значение длительности всей анимации.

Так как мы будем использовать `Animation` класс для вывода анимированной анимации, мы хотим, чтобы эта анимация повторялась при достижении конца. Это можно сделать, проверив, превышает ли `timeIntoAnimation` `Duration` значение. Если да, то он вернется к началу и сохранит остаток, что приведет к циклической анимации.

### <a name="obtaining-the-current-frames-rectangle"></a>Получение прямоугольника текущего кадра

Цель `Animation` класса — предоставить объект, `Rectangle` который можно использовать при рисовании спрайта. В настоящее время `Animation` класс содержит логику для изменения `timeIntoAnimation` элемента, который будет использоваться для получения `Rectangle` отображаемых элементов. Для этого нужно создать `CurrentRectangle` свойство в `Animation` классе. Скопируйте это свойство в `Animation` класс:

```csharp
public Rectangle CurrentRectangle
{
    get
    {
        AnimationFrame currentFrame = null;

        // See if we can find the frame
        TimeSpan accumulatedTime = new TimeSpan();
        foreach(var frame in frames)
        {
            if (accumulatedTime + frame.Duration >= timeIntoAnimation)
            {
                currentFrame = frame;
                break;
            }
            else
            {
                accumulatedTime += frame.Duration;
            }
        }

        // If no frame was found, then try the last frame, 
        // just in case timeIntoAnimation somehow exceeds Duration
        if (currentFrame == null)
        {
            currentFrame = frames.LastOrDefault ();
        }

        // If we found a frame, return its rectangle, otherwise
        // return an empty rectangle (one with no width or height)
        if (currentFrame != null)
        {
            return currentFrame.SourceRectangle;
        }
        else
        {
            return Rectangle.Empty;
        }
    }
}
```

## <a name="adding-the-first-animation-to-characterentity"></a>Добавление первой анимации в Чарактерентити

Объект `CharacterEntity` будет содержать анимацию для пошагового и пошагового просмотра, а также ссылку на текущее `Animation` Отображаемое значение.

Во-первых, мы добавим наш первый, `Animation,` который мы будем использовать для тестирования функциональности, как написано на данный момент. Давайте добавим следующие члены в класс Чарактерентити:

```csharp
Animation walkDown;
Animation currentAnimation;
```

Теперь определим `walkDown` анимацию. Для этого измените `CharacterEntity` конструктор следующим образом:

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

Как упоминалось ранее, нам нужно вызвать `Animation.Update` для воспроизведения анимации на основе времени. Также необходимо назначить `currentAnimation` . Теперь мы присваиваем ему значение `currentAnimation` `walkDown` , но мы будем заменяем этот код позже, когда мы реализуем логику перемещения. Мы добавим `Update` метод `CharacterEntity` следующим образом:

```csharp
public void Update(GameTime gameTime)
{
    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

Теперь, когда у нас есть `currentAnimation` назначение и обновление, мы можем использовать его для выполнения рисования. Мы выполним изменение `CharacterEntity.Draw` следующим образом:

```csharp
public void Draw(SpriteBatch spriteBatch)
{
    Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
    Color tintColor = Color.White;
    var sourceRectangle = currentAnimation.CurrentRectangle;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, sourceRectangle, Color.White);
}
```

Последним шагом для получения `CharacterEntity` анимации является вызов вновь добавленного `Update` метода из `Game1` . Измените `Game1.Update` следующим образом:

```csharp
protected override void Update(GameTime gameTime)
{
    character.Update (gameTime);
    base.Update(gameTime);
}
```

Теперь `CharacterEntity` будет воспроизведен `walkDown` анимация:

![Теперь Чарактерентити будет играть свою Валкдовную анимацию](part2-images/image5.gif)

## <a name="adding-movement-to-the-character"></a>Добавление перемещения к символу

Далее мы добавим перемещение к нашему символу с помощью сенсорных элементов управления. Когда пользователь касается экрана, символ перемещается в точку, где размещается экран. Если штрихи не обнаружены, то символ будет находиться на месте.

### <a name="defining-getdesiredvelocityfrominput"></a>Определение ЖетдесиредвелоЦитифроминпут

Мы будем использовать класс для игр `TouchPanel` , который предоставляет сведения о текущем состоянии сенсорного экрана. Добавим метод, который будет проверять `TouchPanel` и возвращать требуемую скорость персонажа:

```csharp
Vector2 GetDesiredVelocityFromInput()
{
    Vector2 desiredVelocity = new Vector2 ();

    TouchCollection touchCollection = TouchPanel.GetState();

    if (touchCollection.Count > 0)
    {
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;

        if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)
        {
            desiredVelocity.Normalize();
            const float desiredSpeed = 200;
            desiredVelocity *= desiredSpeed;
        }
    }

    return desiredVelocity;
}
```

`TouchPanel.GetState`Метод возвращает объект, `TouchCollection` который содержит сведения о том, где пользователь касается экрана. Если пользователь не касается экрана, то `TouchCollection` будет пустым, в этом случае нам не нужно перемещать символ.

Если пользователь касается экрана, то этот символ будет перемещен к первому касанию, иными словами, с `TouchLocation` индексом 0. Изначально мы установим желаемую скорость, равную разнице между расположением символа и первым местом касания:

```csharp
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;
```

Ниже приведено несколько математических операций, которые позволят перемещать символы с той же скоростью. Чтобы объяснить, почему это важно, рассмотрим ситуацию, когда пользователь касается экрана, на котором 500 пикселей, от места расположения символа. В первой строке `desiredVelocity.X` , где задано значение, назначается 500. Тем не менее, если пользователь касался экрана на расстоянии только 100 единиц из символа, то значение будет `desiredVelocity.X` равно 100. В результате скорость перемещения символа будет реагировать на расстояние от символа до точки касания. Так как мы хотим, чтобы символ всегда был перемещен с той же скоростью, необходимо изменить ДесиредвелоЦити.

`if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)`Оператор проверяет, имеет ли скорость ненулевое значение, иными словами, проверка того, что пользователь не касается той же точки, что и текущая позиции символа. В противном случае необходимо задать скорость символа как постоянную, независимо от того, насколько далеко коснуться. Это достигается путем нормализации вектора скорости, который, в своюмся, приводит к длине 1. Вектор скорости 1 означает, что символ будет перемещен в 1 пиксель в секунду. Мы будем ускорять это, умножив значение на желаемую скорость 200.

### <a name="applying-velocity-to-position"></a>Применение скорости к положению

Скорость, возвращенная из, `GetDesiredVelocityFromInput` должна быть применена к `X` `Y` значению и значениям для любого воздействия во время выполнения. Мы изменим `Update` метод следующим образом:

```csharp
public void Update(GameTime gameTime)
{

    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;

    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

Реализованный здесь метод называется движением *на основе времени* (в отличие от перемещения на *основе кадров* ). Перемещение на основе времени умножает значение скорости (в нашем случае значения, хранящиеся в `velocity` переменной) на количество времени, прошедшее с момента последнего обновления, хранящегося в `gameTime.ElapsedGameTime.TotalSeconds` . Если игра выполняется в меньшем количестве кадров в секунду, то время, прошедшее между кадрами, становится итоговым результатом, так как объекты, использующие время перемещения на основе времени, всегда будут перемещаться по той же скорости, независимо от частоты кадров.

Если мы выполним игру сейчас, мы увидим, что символ перемещается к сенсорному месту:

![Символ перемещается к сенсорному месту](part2-images/image6.gif)

## <a name="matching-movement-and-animation"></a>Сопоставление движения и анимации

После перемещения и воспроизведения символа в одной анимации можно определить оставшуюся часть анимации, а затем использовать их для отражения перемещения символа. По завершении в итоге у нас будет восемь анимаций:

- Анимация для прохода вверх, вниз, влево и вправо
- Анимация для зафиксирования, вниз, влево и вправо

### <a name="defining-the-rest-of-the-animations"></a>Определение оставшейся части анимации

Сначала мы добавим `Animation` экземпляры в `CharacterEntity` класс для всех анимаций в том же месте, где мы добавили `walkDown` . После этого в `CharacterEntity` будут сосуществовать следующие `Animation` члены:

```csharp
Animation walkDown;
Animation walkUp;
Animation walkLeft;
Animation walkRight;

Animation standDown;
Animation standUp;
Animation standLeft;
Animation standRight;

Animation currentAnimation;
```

Теперь мы определим анимацию в `CharacterEntity` конструкторе следующим образом:

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkUp = new Animation ();
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (160, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (176, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkLeft = new Animation ();
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (64, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (80, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkRight = new Animation ();
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (112, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (128, 0, 16, 16), TimeSpan.FromSeconds (.25));

    // Standing animations only have a single frame of animation:
    standDown = new Animation ();
    standDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standUp = new Animation ();
    standUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standLeft = new Animation ();
    standLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standRight = new Animation ();
    standRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

Следует отметить, что приведенный выше код был добавлен в `CharacterEntity` конструктор, чтобы сократить это пошаговое руководство. Обычно игры разделяют определение анимации символов в собственные классы или загружают эту информацию из формата данных, такого как XML или JSON.

Далее предстоит настроить логику для использования анимации в соответствии с направлением перемещения символа или в соответствии с последней анимацией, если символ был только что остановлен. Для этого мы изменим `Update` метод:

```csharp
public void Update(GameTime gameTime)
{
    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;

    if (velocity != Vector2.Zero)
    {
        bool movingHorizontally = Math.Abs (velocity.X) > Math.Abs (velocity.Y);
        if (movingHorizontally)
        {
            if (velocity.X > 0)
            {
                currentAnimation = walkRight;
            }
            else
            {
                currentAnimation = walkLeft;
            }
        }
        else
        {
            if (velocity.Y > 0)
            {
                currentAnimation = walkDown;
            }
            else
            {
                currentAnimation = walkUp;
            }
        }
    }
    else
    {
        // If the character was walking, we can set the standing animation
        // according to the walking animation that is playing:
        if (currentAnimation == walkRight)
        {
            currentAnimation = standRight;
        }
        else if (currentAnimation == walkLeft)
        {
            currentAnimation = standLeft;
        }
        else if (currentAnimation == walkUp)
        {
            currentAnimation = standUp;
        }
        else if (currentAnimation == walkDown)
        {
            currentAnimation = standDown;
        }
        else if (currentAnimation == null)
        {
            currentAnimation = standDown;
        }

        // if none of the above code hit then the character
        // is already standing, so no need to change the animation.
    }

    currentAnimation.Update (gameTime);
}
```

Код для переключения анимации разбивается на два блока. Первая проверяет, не равно ли скорость, `Vector2.Zero` — указывает, что перемещается символ. Если символ перемещается, можно просмотреть `velocity.X` значения и, `velocity.Y` чтобы определить, какая анимация будет воспроизводиться.

Если символ не перемещается, мы хотим присвоить символу фиксированную `currentAnimation` анимацию, но это делается только в том случае, если `currentAnimation` является анимированной анимацией или если анимация не была задана. Если `currentAnimation` не является одной из четырех пошаговых анимаций, то символ уже помещается, поэтому изменять его не нужно `currentAnimation` .

Результатом этого кода является то, что символ будет правильно анимироваться при прохождении, а затем в последнем направлении, когда оно было остановлено:

![Результатом этого кода является то, что символ будет правильно анимироваться при прохождении, а затем в последнем направлении, когда он был остановлен](part2-images/image7.gif)

## <a name="summary"></a>Сводка

В этом пошаговом руководстве показано, как работать с коигру для создания кросс-платформенной игры с помощью спрайтов, перемещения объектов, обнаружения входных данных и анимации. В нем рассматривается создание класса анимации общего назначения. Также показано, как создать сущность символов для организации логики кода.

## <a name="related-links"></a>Связанные ссылки

- [Ресурс образа Чарактершит (пример)](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)
- [Проход по завершении игры (пример)](/samples/xamarin/mobile-samples/walkinggamemg/)