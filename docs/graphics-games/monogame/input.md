---
title: Справочник по одноигровому игровому планшету
description: В этом документе описывается игровой планшет, кросс-платформенный класс для доступа к входным устройствам в одноигре. В нем рассматривается чтение входных данных с игрового планшета и примеры кода.
ms.prod: xamarin
ms.assetid: 1F71F3E8-2397-4C6A-8163-6731ECFB7E03
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 11b1cfda435e97b09f9d1eded22f11f912d1783d
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86934034"
---
# <a name="monogame-gamepad-reference"></a>Справочник по одноигровому игровому планшету

_Игровой планшет — это стандартный, кросс-платформенный класс для доступа к устройствам ввода в пропускной игре._

`GamePad`может использоваться для чтения входных данных с устройств ввода на нескольких высокоигровых платформах. В этом руководство показано, как работать с классом игрового планшета. Так как каждое устройство ввода различается в разметке и числе предоставляемых им кнопок, в этом руководством содержатся схемы, отображающие различные сопоставления устройств.

## <a name="gamepad-as-a-replacement-for-xbox360gamepad"></a>Игровой планшет в качестве замены для Xbox360GamePad

Исходный API XNA предоставил `Xbox360GamePad` класс для чтения входных данных из игрового устройства на Xbox 360 или ПК. Эта игра заменена `GamePad` классом, так как контроллеры Xbox 360 не могут использоваться на большинстве высококлассных платформ (таких как iOS или Xbox One). Несмотря на изменение имени, использование `GamePad` класса аналогично `Xbox360GamePad` классу.

## <a name="reading-input-from-gamepad"></a>Чтение входных данных с игрового планшета

`GamePad`Класс предоставляет стандартизированный способ чтения входных данных на любой односторонней платформе. Он предоставляет информацию двумя способами:

- `GetState`— Возвращает текущее состояние кнопок контроллера, аналоговые устройства и d-Pad.
- `GetCapabilities`— Возвращает сведения о возможностях оборудования, например о наличии определенных кнопок в контроллере или о поддержке вибрации.

### <a name="example-moving-a-character"></a>Пример. перемещение символа

В следующем коде показано, как можно использовать левый элемент Thumb для перемещения символа путем установки его `XVelocity` `YVelocity` свойств и. В этом коде предполагается, что `characterInstance` является экземпляром объекта со `XVelocity` `YVelocity` свойствами и:

```csharp
// In Update, or some code called every frame:
var gamePadState = GamePad.GetState(PlayerIndex.One);
// Use gamePadState to move the character
characterInstance.XVelocity = gamePadState.ThumbSticks.Left.X * characterInstance.MaxSpeed;
characterInstance.YVelocity = gamePadState.ThumbSticks.Left.Y * characterInstance.MaxSpeed;
```

### <a name="example-detecting-pushes"></a>Пример. Обнаружение push-уведомлений

`GamePadState`предоставляет сведения о текущем состоянии контроллера, например, была ли нажата определенная кнопка. Для некоторых действий, например для перехода к символам, требуется проверка того, была ли нажата кнопка (не находится в последнем кадре, но она находится в состоянии "находится в прошлом)" или "освобождена" (в последнем фрейме, но не в этом фрейме).

Для выполнения логики такого типа необходимо создать локальные переменные, в которых хранится предыдущий кадр, `GamePadState` и текущий кадр `GamePadState` . В следующем примере показано, как сохранить и использовать предыдущий кадр `GamePadState` для реализации перехода:

```csharp
// At class scope:
// Store the last frame's and this frame's GamePadStates.
// "new" them so that code doesn't have to perform null checks:
GamePadState lastFrameGamePadState = new GamePadState();
GamePadState currentGamePadState = new GamePadState();
protected override void Update(GameTime gameTime)
{
    // store off the last state before reading the new one:
    lastFrameGamePadState = currentGamePadState;
    currentGamePadState = GamePad.GetState(PlayerIndex.One);
    bool wasAButtonPushed =
currentGamePadState.Buttons.A == ButtonState.Pressed
        && lastFrameGamePadState.Buttons.A == ButtonState.Released;
    if(wasAButtonPushed)
    {
        MakeCharacterJump();
    }
...
}
```

### <a name="example-checking-for-buttons"></a>Пример: Проверка кнопок

`GetCapabilities`может использоваться для проверки наличия в контроллере определенного оборудования, такого как конкретная кнопка или аналоговая записка. В следующем коде показано, как проверить кнопки B и Y на контроллере в игре, которая требует наличия обеих кнопок:

```csharp
var capabilities = GamePad.GetCapabilities(PlayerIndex.One);
bool hasBButton = capabilities.HasBButton;
bool hasXButton = capabilities.HasXButton;
if(!hasBButton || !hasXButton)
{
    NotifyUserOfMissingButtons();
}
```

## <a name="ios"></a>iOS

приложения iOS поддерживают входные видеоконтроллеры беспроводной связи.

> [!IMPORTANT]
> Пакеты NuGet для 3,5 не поддерживают Беспроводные игровые контроллеры. Для использования класса "игровой планшет" в iOS требуется сборка 3,5 из источника или использование двоичных файлов с помощью одноигровой 3,6 NuGet.

### <a name="ios-game-controller"></a>Игровой контроллер iOS

`GamePad`Класс возвращает свойства, считанные с беспроводных контроллеров. Свойства в `GamePad` обеспечивают хорошее покрытие для стандартного контроллера iOS, как показано на следующей схеме:

![Свойства на игровом устройстве обеспечивают хорошее покрытие для стандартного контроллера iOS, как показано на этой схеме.](input-images/image1.png)

## <a name="apple-tv"></a>Apple TV

Игры Apple TV могут использовать Siri удаленные или беспроводные игровые контроллеры для ввода данных.

### <a name="siri-remote"></a>Siri удаленный

*Siri Remote* — это собственное устройство ввода для Apple TV. Хотя значения Siri Remote могут быть прочитаны с помощью событий (как показано в разделе [Siri Remote and Bluetooth Controllers Guide](~/ios/tvos/platform/remote-bluetooth.md)), `GamePad` класс может возвращать значения из Siri Remote.

Обратите внимание, что `GamePad` может только считывать входные данные с кнопки воспроизведение и сенсорную поверхность:

![Обратите внимание, что игровой планшет может считывать данные только с кнопки воспроизведения и с сенсорной поверхности.](input-images/image2.png)

Так как перемещение сенсорной панели считывается с помощью `DPad` свойства, значения перемещения выводятся с помощью `ButtonState` класса. Иными словами, значения доступны только в виде `ButtonState.Pressed` или `ButtonState.Released` , в отличие от числовых значений или жестов.

### <a name="apple-tv-game-controller"></a>Игровой контроллер Apple TV

Игровые контроллеры для Apple TV ведут себя так же, как и игровые контроллеры для приложений iOS. Дополнительные сведения см. в разделе [игровой контроллер iOS](#ios-game-controller) . 

## <a name="xbox-one"></a>Xbox One

Консоль Xbox One поддерживает чтение входных данных с устройства Xbox One.

### <a name="xbox-one-game-controller"></a>Игровой контроллер Xbox One

Игровой контроллер Xbox One — это наиболее распространенное устройство ввода для Xbox One. `GamePad`Класс предоставляет входные значения от оборудования игрового контроллера.

![Класс игровой планшет предоставляет входные значения от оборудования игрового контроллера](input-images/image3.png)

## <a name="summary"></a>Итоги

В этом руководство были представлены общие сведения о `GamePad` классе с поддержкой «в игре», реализации логики считывания входных данных и схемах распространенных `GamePad` реализаций.

## <a name="related-links"></a>Связанные ссылки

- [Игровой планшет](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_GamePad)
