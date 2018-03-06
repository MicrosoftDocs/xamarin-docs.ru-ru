---
title: "Кэширование текстуры с помощью CCTextureCache"
description: "В CocosSharp CCTextureCache класс предоставляет стандартный способ упорядочения кэша и выгрузки содержимого. Это особенно полезно для больших игры, которые может не поместиться полностью в оперативную ПАМЯТЬ, упрощая процесс группирования и утилизация текстур."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1B5F3F85-9E68-42A7-B516-E90E54BA7102
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 365e343a55a208b63f4dc52999e8857b5f0ec1f4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="texture-caching-using-cctexturecache"></a>Кэширование текстуры с помощью CCTextureCache

_В CocosSharp CCTextureCache класс предоставляет стандартный способ упорядочения кэша и выгрузки содержимого. Это особенно полезно для больших игры, которые может не поместиться полностью в оперативную ПАМЯТЬ, упрощая процесс группирования и утилизация текстур._

`CCTextureCache` Класс является важной частью разработки игр CocosSharp. Большинство CocosSharp играми воспользуйтесь `CCTextureCache` объекта, даже если явно не любое число методов CocosSharp для внутреннего использования *общего* текстур кэша.

В настоящем руководстве описывается `CCTextureCache` и поэтому важно для разработки игр. В частности рассматривается:

 - Почему текстуры кэширования: вопросы и ответы
 - Время существования текстуры
 - С помощью SharedTextureCache
 - Отложенной загрузки и предварительную загрузку с AddImage
 - Освобождение текстуры


# <a name="why-texture-caching-matters"></a>Почему текстуры кэширования: вопросы и ответы

Кэширование текстуры является важным аспектом при разработке игры, загрузка текстуры длительную операцию и текстур требуется значительный объем оперативной памяти во время выполнения.

Как и любая операция файл загрузки текстур с диска может быть дорогостоящей операцией. Если загружаемый файл требуется обработка, например распаковать (как в случае для изображений png и jpg) текстуры загрузка может занять дополнительное время. Кэширование текстуры можно уменьшить количество раз, что приложение должно загрузить файлы с диска.

Как упоминалось выше, текстуры также занимают большой объем памяти среды выполнения. Например фонового изображения, подобранные по разрешение iPhone 6 (1344 x 750) должны занимать 4 МБ оперативной памяти, даже если PNG-файл имеет размер только несколько килобайт. Кэширование текстуры предоставляет способ совместного использования ссылок текстур в приложении, а также простой способ выгрузки все содержимое во время перехода между различными состояниями игры.


# <a name="texture-lifespan"></a>Время существования текстуры

Можно недолго или CocosSharp текстур может храниться в памяти для всей продолжительности выполнения приложения. Чтобы свести к минимуму памяти использования приложения следует освободить текстур, когда больше не нужны. Конечно это означает, что текстуры может удален и повторно загрузить в более позднее время, которое может увеличить время загрузки или приводят к снижению производительности во время загрузки. 

Текстуры загрузки часто требуется компромисс между временем использования и нагрузки памяти и производительности во время выполнения. Игры, которые используют небольшой объем памяти текстур можно сохранить все текстуры в памяти, чем требуется, но больше игры может потребоваться выгрузить текстур для освобождения места.

На следующей диаграмме показан простой игры, который загружает текстур, при необходимости и сохраняет их в памяти для всей продолжительности выполнения:

![](texture-cache-images/image1.png "На этой схеме показан простой игры, который загружает текстур, при необходимости и сохраняет их в памяти для всей продолжительности выполнения")

Первые две строки представляют текстур, которые нужны сразу же после выполнения игры. Следующие три строки представляют текстур для каждого уровня загрузки при необходимости.

Если игры велико достаточно его в конечном итоге загружает достаточно текстур для заполнения всех ОЗУ, предоставляемых устройства и операционной системы. Чтобы устранить эту проблему, игры может выгрузки данных текстуры, когда он больше не нужен. Например в примере ниже показан игры, который выгружает Level1Texture, когда он больше не нужен, а затем загружает Level2Texture для следующего уровня. Конечным результатом является то, что только трех текстур хранятся в памяти в любой момент времени: 

![](texture-cache-images/image2.png "Конечным результатом является то, что только трех текстур хранятся в памяти в любой момент времени")


Приведенной выше схеме указывает, использование памяти текстуры может быть уменьшена путем выгрузки, но для этого может потребоваться время дополнительной загрузки, если решает проигрыватель для воспроизведения уровень. Также стоит отметить, что текстуры UITexture и MainCharacter загружаются и никогда не выгружаются. Это означает, что эти текстур требуются на всех уровнях, всегда хранятся в памяти. 


# <a name="using-sharedtexturecache"></a>С помощью SharedTextureCache

CocosSharp автоматически кэширует текстур при загрузке их с помощью `CCSprite` конструктор. Например следующий код создает только один экземпляр текстуры:


```csharp
for (int i = 0; i < 100; i++)
{
    CCSprite starSprite = new CCSprite ("star.png");
    starSprite.PositionX = i * 32;
    this.AddChild (starSprite);
} 
```

Автоматически кэширует CocosSharp `star.png` текстуры, чтобы избежать ресурсоемких вместо создания многочисленные идентичные `CCTexture2D` экземпляров. Это достигается путем `AddImage` вызывается на общий `CCTextureCache` экземпляра, в частности `CCTextureCache.SharedTextureCache.Shared`. Чтобы понять, как `SharedTextureCache` используется рассмотрим следующий код, который функционально идентичен вызова `CCSprite` конструктор с параметром строки:


```

CCSprite starSprite = new CCSprite ();
 starSprite.Texture = CCTextureCache.SharedTextureCache.AddImage ("star.png");
```

`AddImage` проверяет, если файл аргументов (в данном случае `star.png`) уже загружен. В этом случае возвращается кэшированного экземпляра. Если затем не загружается из файловой системы и ссылку на текстуры сохраняется для последующих `AddImage` вызовов. Другими словами `star.png` образ загружается только один раз, и последующие вызовы требуют нет доступа к дополнительный диск или память дополнительные текстуры.


# <a name="lazy-loading-vs-pre-loading-with-addimage"></a>Отложенная загрузка vs. Предварительная загрузка с AddImage

`AddImage` позволяет коду записываться же запрошенную текстуру уже загружена ли или нет. Это означает, что содержимое не будет загружен, если она нужна; Тем не менее это может вызвать проблемы с производительностью во время выполнения из-за непредсказуемым содержимое при загрузке.

Например, рассмотрим игру, где можно обновить оружие игрока. При обновлении оружие и projectiles наглядно изменится, приведет к новой текстуры используется. Если содержимое отложенной загрузки затем текстур, связанных с обновленной оружие не будет загружен изначально, а лишь через некоторое время, когда проигрыватель запрашивает обновления. 

Эта загрузка игрового опубликованную в середине процесса может привести к игры для *pop*, который является запрет короткое, но заметно при выполнении. Чтобы избежать этого, код может предсказать, какие текстуры могут потребоваться заранее и предварительно загрузить их. Например следующие может использоваться для предварительной загрузки текстур:


```csharp
void PreLoadImages()
{
    var cache = CCTextureCache.SharedTextureCache;

    cache.AddImage ("powerup1.png");
    cache.AddImage ("powerup2.png");
    cache.AddImage ("powerup3.png");

    cache.AddImage ("enemy1.png");
    cache.AddImage ("enemy2.png");
    cache.AddImage ("enemy3.png");

    // pre-load any additional content here to 
    // prevent pops at runtime
} 
```

Эта предварительная загрузка может привести к неиспользуемым памяти и может увеличить время запуска. Например, проигрыватель может получить фактически не питания представленный `powerup3.png` текстур, поэтому он будет загружаться без необходимости. Конечно, это может быть необходимости затрат для оплаты во избежание потенциальных pop в игрового процесса, поэтому, как правило, лучше предварительной загрузки содержимого помещается в оперативной памяти.


# <a name="disposing-textures"></a>Освобождение текстуры

Если игры не требуется больше памяти текстур, чем доступно на минимальное характеристик устройства, то текстур, не должны быть удалены. С другой стороны больше игры может потребоваться освободить память текстуры, чтобы освободить место для нового содержимого. Например, игра может использовать большой объем памяти, текстуры для среды хранения. Если среде используется только в определенном уровне, его должно быть выгружен при завершении уровень.


## <a name="disposing-a-single-texture"></a>Удаление одну текстуру

Сначала удалить одну текстуру требует вызова `Dispose` метода, а затем вручную удалить из `CCTextureCache`.

Ниже показано, как полностью удалить вместе с текстура спрайт фона:


```csharp
void DisposeBackground()
{
    // Assuming this is called from a CCLayer:
    this.RemoveChild (backgroundSprite);

    CCTextureCache.SharedTextureCache.RemoveTexture (backgroundsprite.Texture);

    backgroundSprite.Texture.Dispose ();
} 
```

Непосредственно disposing текстуры можно эффективно при работе с небольшим числом текстур, но это может стать ошибкам при работе с большими наборами текстуры.

Текстуры могут быть сгруппированы в настраиваемый (без общего доступа) `CCTextureCache` экземпляров для упрощения очистки текстуры.

Например, рассмотрим пример, где содержимое предварительно загружается с помощью определенного уровня `CCTextureCache` экземпляра. `CCTextureCache` Экземпляр может определяться в классе, определяющем уровень (который может быть `CCLayer` или `CCScene`):


```csharp
CCTextureCache levelTextures; 
```

`levelTextures` Экземпляр может использоваться для предварительной загрузки текстур определенного уровня: 


```

void PreloadLevelTextures(CCApplication application)
{
    levelTextures = new CCTextureCache (application);

    levelTextures.AddImage ("Background.png");
    levelTextures.AddImage ("Foreground.png");
    levelTextures.AddImage ("Enemy1.png");
    levelTextures.AddImage ("Enemy2.png");
    levelTextures.AddImage ("Enemy3.png");

    levelTextures.AddImage ("Powerups.png");
    levelTextures.AddImage ("Particles.png");
} 
```

Наконец по окончании уровень текстуры могут находиться все удаляется сразу через `CCTextureCache`:


```csharp
void EndLevel()
{
    levelTextures.Dispose ();
    // Perform any other end-level cleanup
} 
```

Метод Dispose удалит все внутренние текстуры, очистки памяти, используемой этими текстуры. Объединение `CCTextureCache.Shared` с уровня или игровой режим определенного `CCTextureCache` экземпляр приводит некоторые текстуры, сохранение через всю игру и некоторые выгружается, как выйти из уровней, аналогична схеме, представленного в начале этого руководства: 

![](texture-cache-images/image2.png "Объединение CCTextureCache.Shared игры или уровня относящиеся к режиму CCTextureCache экземпляра приводит к получению некоторых текстур, сохранение через всю игру и некоторые выгружается, как выйти из уровней, аналогична схеме, представленного в начале этого руководства")




# <a name="summary"></a>Сводка

В этом руководстве показано, как использовать `CCTextureCache` класса баланс производительности памяти использования и среды выполнения. `CCTexturCache.SharedTextureCache` можно явно или неявно используется для загрузки и кэширования текстур в течение жизненного цикла приложения, а `CCTextureCache` экземпляров может использоваться для выгрузки текстур, чтобы сократить объем используемой памяти.

## <a name="related-links"></a>Связанные ссылки

- [https://github.com/mono/CocosSharp](https://github.com/mono/CocosSharp)
- [/api/type/CocosSharp.CCTextureCache/](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)