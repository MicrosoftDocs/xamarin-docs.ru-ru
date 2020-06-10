---
title: Альтернативные значки приложений в Xamarin. iOS
description: В этом документе описывается, как использовать альтернативные значки приложений в Xamarin. iOS. В нем описывается добавление этих значков в проект Xamarin. iOS, изменение файла INFO. plist и управление значком приложения программным способом.
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: e90ee165073dbbe792e4ca1916463517ad86255d
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84572303"
---
# <a name="alternate-app-icons-in-xamarinios"></a>Альтернативные значки приложений в Xamarin. iOS

_В этой статье описывается использование альтернативных значков приложений в Xamarin. iOS._

Компания Apple добавила несколько улучшений в iOS 10,3, которые позволяют приложению управлять своим значком:

- `ApplicationIconBadgeNumber`— Получает или задает эмблему значка приложения в Springboard Series.
- `SupportsAlternateIcons`— Если `true` приложение имеет альтернативный набор значков.
- `AlternateIconName`— Возвращает имя выбранного в данный момент альтернативного значка или `null` значение, если используется основной значок.
- `SetAlternameIconName`— Используйте этот метод, чтобы переключить значок приложения на заданный альтернативный значок.

![](alternate-app-icons-images/icons04.png "A sample alert when an app changes its icon")

<a name="Adding-Alternate-Icons"></a>

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>Добавление альтернативных значков в проект Xamarin. iOS

Чтобы разрешить приложению переключиться на альтернативный значок, необходимо включить в проект приложения Xamarin. iOS коллекцию изображений значков. Эти образы нельзя добавить в проект с помощью стандартного `Assets.xcassets` метода, они должны быть добавлены непосредственно в папку **Resources** .

Выполните следующие действия.

1. Выберите нужные изображения значков в папке, выберите все и перетащите их в папку **ресурсы** в **Обозреватель решений**:

    ![](alternate-app-icons-images/icons00.png "Select the icons images from a folder")

2. При появлении запроса выберите **Копировать**, **Используйте то же действие для всех выбранных файлов** и нажмите кнопку **ОК** :

    ![](alternate-app-icons-images/icons02.png "The Add File to Folder dialog box")

3. При завершении папка **Resources** должна выглядеть следующим образом:

    ![](alternate-app-icons-images/icons01.png "The Resources folder should look like this")

<a name="Modifying-the-Info.plist-File"></a>

## <a name="modifying-the-infoplist-file"></a>Изменение файла INFO. plist

Если в папку **Resources** добавлены необходимые образы, ключ [кфбундлеалтернатеиконс](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13) потребуется добавить в файл **info. plist** проекта. Этот ключ определяет имя нового значка и образы, составляющие его.

Выполните следующие действия.

1. В **Обозревателе решений** дважды щелкните файл **Info.plist**, чтобы открыть его для редактирования.
2. Переключитесь в представление **исходного кода** .
3. Добавьте раздел **значков пакета** и оставьте для **типа** значение **Dictionary**.
4. Добавьте `CFBundleAlternateIcons` ключ и задайте для **типа** значение **Dictionary**.
5. Добавьте `AppIcon2` ключ и задайте для **типа** значение **Dictionary**. Это будет имя нового альтернативного набора значков приложения.
6. Добавьте `CFBundleIconFiles` ключ и задайте для **типа** значение **Array** .
7. Добавьте в массив новую строку `CFBundleIconFiles` для каждого файла значка, исходящего из расширения, а `@2x` также `@3x` суффиксов, и т. д. (пример `100_icon` ). Повторите этот шаг для каждого файла, составляющего альтернативный набор значков.
8. Добавьте `UIPrerenderedIcon` ключ в `AppIcon2` словарь, установите **Тип** **Boolean** и значение **нет**.
9. Сохраните изменения, внесенные в файл.

Полученный файл **info. plist** должен выглядеть следующим образом после завершения:

![](alternate-app-icons-images/icons03.png "The completed Info.plist file")

Или, как и при открытии в текстовом редакторе:

```xml
<key>CFBundleIcons</key>
<dict>
    <key>CFBundleAlternateIcons</key>
    <dict>
        <key>AppIcon2</key>
        <dict>
            <key>CFBundleIconFiles</key>
            <array>
                <string>100_icon</string>
                <string>114_icon</string>
                <string>120_icon</string>
                <string>144_icon</string>
                <string>152_icon</string>
                <string>167_icon</string>
                <string>180_icon</string>
                <string>29_icon</string>
                <string>40_icon</string>
                <string>50_icon</string>
                <string>512_icon</string>
                <string>57_icon</string>
                <string>58_icon</string>
                <string>72_icon</string>
                <string>76_icon</string>
                <string>80_icon</string>
                <string>87_icon</string>
            </array>
            <key>UIPrerenderedIcon</key>
            <false/>
        </dict>
    </dict>
</dict>
```

<a name="Managing-the-Apps-Icon"></a>

## <a name="managing-the-apps-icon"></a>Управление значком приложения 

Используя изображения значков, включенные в проект Xamarin. iOS и правильно настроенный файл **info. plist** , разработчик может использовать одну из многих новых функций, добавленных в iOS 10,3 для управления значком приложения.

`SupportsAlternateIcons`Свойство `UIApplication` класса позволяет разработчику определить, поддерживает ли приложение альтернативные значки. Пример.

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

`ApplicationIconBadgeNumber`Свойство `UIApplication` класса позволяет разработчику получить или задать номер текущего значка приложения в Springboard Series. Значение по умолчанию равно нулю (0). Пример.

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

`AlternateIconName`Свойство `UIApplication` класса позволяет разработчику получить имя выбранного в данный момент альтернативного приложения или возвращает, `null` Если приложение использует основной значок. Пример.

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

`SetAlternameIconName`Свойство `UIApplication` класса позволяет разработчику изменить значок приложения. Передайте имя значка, чтобы выбрать или `null` вернуться к основному значку. Пример.

```csharp
partial void UsePrimaryIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName (null, (err) => {
        Console.WriteLine ("Set Primary Icon: {0}", err);
    });
}

partial void UseAlternateIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName ("AppIcon2", (err) => {
        Console.WriteLine ("Set Alternate Icon: {0}", err);
    });
}
```

Когда приложение запускается и пользователь выбирает альтернативный значок, отображается предупреждение следующего вида:

![](alternate-app-icons-images/icons04.png "A sample alert when an app changes its icon")

Если пользователь переключается на основной значок, будет выведено предупреждение следующего вида:

![](alternate-app-icons-images/icons05.png "A sample alert when an app changes to the primary icon")

<a name="Summary"></a>

## <a name="summary"></a>Сводка

В этой статье рассматривается добавление альтернативных значков приложений в проект Xamarin. iOS и их использование в приложении.

## <a name="related-links"></a>Связанные ссылки

- [Пример Иостенсри](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-iostenthree/)
