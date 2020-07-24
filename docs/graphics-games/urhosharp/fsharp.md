---
title: Программирование UrhoSharp с помощью F#
description: 'В этом документе описывается создание простого приложения Hello World UrhoSharp с помощью F # в Visual Studio для Mac.'
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: af9619ace957a47282cbf9fdefea4e81e7eace13
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86940014"
---
# <a name="programming-urhosharp-with-f"></a>Программирование UrhoSharp с помощью F\#

UrhoSharp можно программировать с помощью F #, используя те же самые библиотеки и концепции, которые используются программистами на C#. В статье [Использование UrhoSharp](~/graphics-games/urhosharp/using.md) приводятся общие сведения о подсистеме UrhoSharp, которые необходимо прочитать до этой статьи.

Как и многие библиотеки, созданные в мире C++, многие функции UrhoSharp возвращают логические значения или целые числа, указывающие на успех или неудачу. Следует использовать `|> ignore` для игнорирования этих значений.

[Пример программы](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) — это «Hello World» для UrhoSharp из F #.

## <a name="creating-an-empty-project"></a>Создание пустого проекта

Шаблоны F # для UrhoSharp пока недоступны, поэтому для создания собственного проекта UrhoSharp можно либо начать с [примера](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) , либо выполнить следующие действия.

1. В Visual Studio для Mac создайте новое **решение**. Выберите **iOS > приложение > приложение с одним представлением** и выберите **F #** в качестве языка реализации. 
1. Удалите файл **Main. Storyboard** . Откройте файл **info. plist** и в области **сведений о развертывании iPhone/iPod** удалите `Main` строку в раскрывающемся списке **главного интерфейса** .
1. Удалите также файл **ViewController. FS** .

## <a name="building-hello-world-in-urho"></a>Создание Hello World в Урхо

Теперь можно приступать к определению классов игры. Как минимум потребуется определить подкласс `Urho.Application` и переопределить его `Start` метод. Чтобы создать этот файл, щелкните правой кнопкой мыши проект F #, выберите **Добавить новый файл...** и добавьте в проект пустой класс F #. Новый файл будет добавлен в конец списка файлов в проекте, но его необходимо перетащить, чтобы он появился *перед* использованием в **AppDelegate. FS**.

1. Добавьте ссылку на пакет NuGet Урхо.
1. В существующем проекте Урхо скопируйте (крупные) каталоги **CoreData/** и **Data/** в **ресурсы или** каталог проекта. В проекте F # щелкните правой кнопкой мыши папку **Resources** и используйте команду **Добавить или добавить существующую папку** , чтобы добавить все эти файлы в проект.

Теперь структура проекта должна выглядеть следующим образом:

![Теперь структура проекта должна выглядеть следующим образом:](fsharp-images/solutionpane.png)

Определите созданный вами класс как подтип `Urho.Application` и переопределите его `Start` метод:

```fsharp
namespace HelloWorldUrho1

open Urho
open Urho.Gui
open Urho.iOS

type HelloWorld(o : ApplicationOptions) =
    inherit Urho.Application(o) 

override this.Start() = 
        let cache = this.ResourceCache
        let helloText = new Text()

        helloText.Value <- "Hello World from Urho3D, Mono, and F#"
        helloText.HorizontalAlignment <- HorizontalAlignment.Center
        helloText.VerticalAlignment <- VerticalAlignment.Center

        helloText.SetColor (new Color(0.f, 1.f, 0.f))
        let f = cache.GetFont("Fonts/Anonymous Pro.ttf")

        helloText.SetFont(f, 30) |> ignore
                  
        this.UI.Root.AddChild(helloText)
            
```

Код очень прост. Он использует `Urho.Gui.Text` класс для вывода выровненной по центру строки с определенным шрифтом и размером цвета. 

Однако перед выполнением этого кода необходимо инициализировать UrhoSharp. 

Откройте файл AppDelegate. FS и измените `FinishedLaunching` метод следующим образом:

```fsharp
namespace HelloWorldUrho1

open System
open UIKit
open Foundation
open Urho
open Urho.iOS

[<Register ("AppDelegate")>]
type AppDelegate () =
    inherit UIApplicationDelegate ()

    override this.FinishedLaunching (app, options) =
        let o = ApplicationOptions.Default
     
        let g = new HelloWorld(o)
        g.Run() |> ignore
       
        true
```

`ApplicationOptions.Default`Предоставляет параметры по умолчанию для приложения с альбомным режимом. Передайте их `ApplicationOptions` в конструктор по умолчанию для своего `Application` подкласса (Обратите внимание, что при определении `HelloWorld` класса строка `inherit Application(o)` вызывает конструктор базового класса).

`Run`Метод `Application` запускает программу. Он определяется как возвращающий объект `int` , который можно передать `ignore` .

Итоговая программа должна выглядеть, как на следующем снимке экрана:

![Снимок экрана итоговой программы](fsharp-images/helloworldfsharp.png)

## <a name="related-links"></a>Связанные ссылки

- [Обзор в GitHub (пример)](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
