---
title: Модульное тестирование приложений Xamarin.iOS
description: Этот документ содержит общие сведения о модульном тестировании приложения Xamarin.iOS. Он описывает, как создать проект модульного тестирования, а также писать и запускать тесты.
ms.prod: xamarin
ms.assetid: BD959779-3239-79B6-5289-3A9ECDFBD973
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 98855bb51552edcd567e3b99740f0be04034e388
ms.sourcegitcommit: e27e29c14b783263e063baaa65d4eecb8dd31f57
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98628973"
---
# <a name="unit-testing-xamarinios-apps"></a>Модульное тестирование приложений Xamarin.iOS

В этом документе описывается создание модульных тестов для проектов Xamarin.iOS.
Модульное тестирование с помощью Xamarin.iOS осуществляется с помощью платформы Touch.Unit, в состав которой входит средство запуска тестов iOS и измененная версия платформы NUnit, которая называется [Touch.Unit](https://github.com/xamarin/Touch.Unit) и предоставляет знакомый набор API-интерфейсов для написания модульных тестов.

## <a name="setting-up-a-test-project-in-visual-studio-for-mac"></a>Настройка тестового проекта в Visual Studio для Mac

Чтобы настроить платформу модульного тестирования для проекта, необходимо лишь добавить в решение проект типа **Проект модульных тестов iOS**. Для этого щелкните решение правой кнопкой мыши и выберите **Добавить > Добавить новый проект**. В списке выберите **iOS > Тесты > Unified API > Проект модульных тестов iOS** (можно выбрать язык C# или F#).

![Выбор языка C# или F#](touch.unit-images/00.png)

После этого будет создан базовый проект, содержащий основное средство выполнения тестов и ссылающийся на новую сборку MonoTouch.NUnitLite. Проект будет выглядеть следующим образом:

![Проект в обозревателе решений](touch.unit-images/01.png)

Класс `AppDelegate.cs` содержит средство выполнения тестов и выглядит следующим образом:

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
    UIWindow window;
    TouchRunner runner;

    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        // create a new window instance based on the screen size
        window = new UIWindow (UIScreen.MainScreen.Bounds);
        runner = new TouchRunner (window);

        // register every tests included in the main application/assembly
        runner.Add (System.Reflection.Assembly.GetExecutingAssembly ());

        window.RootViewController = new UINavigationController (runner.GetViewController ());

        // make the window visible
        window.MakeKeyAndVisible ();

        return true;
    }
}
```

> [!NOTE]
> Тип проекта модульного теста iOS не доступен в Visual Studio 2019 или Visual Studio 2017 в Windows.

## <a name="writing-some-tests"></a>Написание некоторых тестов

Теперь, когда имеется базовая оболочка, следует написать первый набор тестов.

Тесты написаны путем создания классов с примененным к ним атрибутом `[TestFixture]`. В каждом классе TestFixture атрибут `[Test]` следует применить к каждому методу, который должен вызывать средство выполнения тестов. Фактические средства тестирования могут существовать в любом файле в проекте тестирования.

Чтобы быстро приступить к работе, выберите **Добавить/Добавить новый файл** и выберите **UnitTests** в группе Xamarin.iOS. После этого будет добавлен структурный файл, содержащий один пройденный тест, один непройденный тест и один пропущенный тест. Файл выглядит следующим образом:

```csharp
using System;
using NUnit.Framework;

namespace Fixtures {

    [TestFixture]
    public class Tests {

        [Test]
        public void Pass ()
        {
                Assert.True (true);
        }

        [Test]
        public void Fail ()
        {
                Assert.False (true);
        }

        [Test]
        [Ignore ("another time")]
        public void Ignore ()
        {
                Assert.True (false);
        }
    }
}
```

## <a name="running-your-tests"></a>Выполнение тестов

Для запуска проекта в решении щелкните его правой кнопкой мыши и выберите пункт **Отладка элемента** или **Запустить элемент**.

Средство выполнения тестов позволяет просмотреть, какие тесты регистрируются, и по отдельности выбрать, какие тесты могут выполняться.

[![Список зарегистрированных тестов](touch.unit-images/02-sml.png)](touch.unit-images/02.png#lightbox) 
[![Отдельный текст](touch.unit-images/03-sml.png)](touch.unit-images/03.png#lightbox) 

[![Результаты выполнения](touch.unit-images/04-sml.png)](touch.unit-images/04.png#lightbox)

Можно выполнить отдельное средство тестирования, выбрав его из вложенных представлений, или выполнить все тесты, выбрав "Выполнить все". При выполнении теста по умолчанию он должен содержать один пройденный тест, один непройденный тест и один пропущенный тест. Отчет будет выглядеть следующим образом. Можно перейти непосредственно к непройденному тесту и изучить дополнительные сведения о сбое:

[![Снимок экрана: пример отчета, экран Test Runner.](touch.unit-images/05-sml.png)](touch.unit-images/05.png#lightbox) [![Снимок экрана: пример отчета, экран "Тесты".](touch.unit-images/06-sml.png)](touch.unit-images/06.png#lightbox) [![Снимок экрана: пример отчета, экран "Тесты" с данными о состоянии теста](touch.unit-images/07-sml.png)](touch.unit-images/07.png#lightbox).

Можно также обратиться к окну выходных данных приложения в интегрированной среде разработки, чтобы увидеть, какие тесты выполняются, и их текущее состояние.

## <a name="writing-new-tests"></a>Написание новых тестов

NUnitLite является модификацией NUnit, которая называется проектом [Touch.Unit](https://github.com/xamarin/Touch.Unit). Это облегченная платформа тестирования для .NET, созданная на базе [NUnit](https://nunit.com/) и содержащая поднабор ее компонентов.
Она использует минимальный объем ресурсов и подходит для платформ с ограниченными ресурсами, таким, как те, которые используются при разработке внедряемых решений и решений для мобильных устройств. API-интерфейс NUnitLite доступен в Xamarin.iOS. С помощью базовой структуры, которая содержится в шаблоне модульного теста, вы получаете методы [класса Assert](xref:NUnit.Framework.Assert), которые являются основной точкой входа.

Помимо методов класса assert, функциональные возможности модульного тестирования разбиты на следующие пространства имен, которые являются частью NUnitLite:

- [NUnit.Framework](xref:NUnit.Framework)
- [NUnit.Constraints](xref:NUnit.Framework.Constraints)
- [NUniteLite.Runner](xref:NUnitLite.Runner)
