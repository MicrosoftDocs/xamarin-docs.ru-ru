---
title: Введение в iOS 14
description: В этом документе представлено общее описание некоторых интерфейсов API iOS 14, для которых Xamarin предоставляет привязки C#.
ms.prod: xamarin
ms.assetid: 4953216e-472b-4484-9c1e-7263ac537f21
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/17/2020
ms.openlocfilehash: e9793617c76813fb68a57213edd8b48529f19ac7
ms.sourcegitcommit: 0c45e3f810947e3d43223aa01bf3e43a0defca65
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/21/2020
ms.locfileid: "90843615"
---
# <a name="introduction-to-ios-14"></a>Введение в iOS 14

Чтобы приступить к работе, следуйте этим [инструкциям](~/ios/platform/ios14/get-started.md) .

## <a name="new-control-uicolorwell"></a>Новый элемент управления: Уиколорвелл

[`UIColorWell`](https://developer.apple.com/documentation/uikit/uicolorwell) — новый элемент управления UIKit для выбора цветов из набора образцов, использования-сбрасыватель или ввода значений вручную. Элемент управления отображает круглую кнопку цвета, которая запускает модальную форму при касании.

![уиколорвелл](ios14-images/colorwell.png)

```xaml
<ios:UIColorWell
    SelectedColor="{x:Static ios:UIColor.Red}"
    ValueChanged="OnColorChanged" />
```

```csharp
private void OnColorChanged(object sender, EventArgs e)
{
    var colorWell = (UIColorWell)sender; 
    Debug.WriteLine(colorWell.SelectedColor);
}
```

## <a name="modified-controls"></a>Измененные элементы управления

Несколько элементов управления получили обновления, что особенно важно:

- [Уибарбуттонитем](https://developer.apple.com/documentation/uikit/uibarbuttonitem) теперь может добавить уимену, который будет отображаться как контекстном меню Action.
- [Уидатепиккер](https://developer.apple.com/documentation/uikit/uidatepicker) теперь поддерживает несколько стилей: Automatic (по умолчанию), Compact, inline и Wheel.
- [Уисплитвиевконтроллер](https://developer.apple.com/documentation/uikit/uisplitviewcontroller) теперь поддерживает три столбца: первичный, вторичный и добавочный.
 
![Предварительный выпуск API](~/media/shared/preview.png)

## <a name="embedded-widgetkit-support"></a>Поддержка Embedded Виджеткит

В этом выпуске пакета SDK добавлена поддержка внедрения расширений Виджеткит, написанных в SWIFT, в главное приложение Xamarin. iOS. Это позволяет создавать приложения с поддержкой мини-приложений на сегодняшний день.

С помощью этого метода вы создадите "гибридное" приложение, состроив расширение мини-приложения с помощью Свифтуи и внедрив его в приложение Xamarin. iOS.

Для использования поддержки Виджеткит потребуется внести несколько изменений в файл проекта вручную.

Добавьте следующий раздел в проект:

```xml
<AdditionalAppExtensions Include="$(MSBuildProjectDirectory)/../../native">
     <Name>NativeTodayExtension</Name>
     <BuildOutput Condition="'$(Platform)' == 'iPhone'">build/Debug-iphoneos</BuildOutput>
     <BuildOutput Condition="'$(Platform)' == 'iPhoneSimulator'">build/Debug-iphonesimulator</BuildOutput>
</AdditionalAppExtensions>
```

Измените путь, включенный в первую ссылку, чтобы он указывал на каталог сборки расширения пользовательского интерфейса SWIFT.

Может быть полезно включить относительное расположение выходных данных проекта в проекте Xcode (файл > Параметры проекта), чтобы получить более простой путь для поиска:

![Параметры Xcode](ios14-images/xcode-settings.png)

Этот [пример приложения](https://github.com/chamons/xamarin-ios-swift-extension/blob/master/App/TestApplication/TestApplication.csproj#L143) использует сериализацию JSON для перемещения данных из приложения Xamarin. iOS в образец мини-приложений для показа.

Те, кто заинтересован в Виджеткит, приглашают предоставить свои [Отзывы](https://github.com/xamarin/xamarin-macios/issues/8933).

## <a name="related-links"></a>Связанные ссылки

- [Заметки о выпуске Xamarin. iOS 14](/xamarin/ios/release-notes/14/14.0)
- [Документация по Уиколорвелл](https://developer.apple.com/documentation/uikit/uicolorwell)
