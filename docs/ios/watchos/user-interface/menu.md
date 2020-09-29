---
title: Элемент управления Menu watchOS (Force Touch) в Xamarin
description: В этом документе описывается использование жеста принудительного касания watchOS в Xamarin. В нем рассматривается реакция на принудительное касание, Добавление меню и изменение пунктов меню.
ms.prod: xamarin
ms.assetid: 5A7F83FB-9BC4-4812-92C5-CEC8DAE8211E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 5ad45e1cc17a58875037f20996463f55f2b68685
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91431548"
---
# <a name="watchos-menu-control-force-touch-in-xamarin"></a>Элемент управления Menu watchOS (Force Touch) в Xamarin

Контрольный комплект предоставляет Force Touch жест, который запускает меню при реализации на экране контрольных приложений.

![Apple Watch отображения меню](menu-images/menu.png)
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="responding-to-force-touch"></a>Реагирование на Force Touch

Если объект был `Menu` реализован для контроллера интерфейса, то при выполнении пользователем Force Touch откроется меню. Если меню не было реализовано, экран кратко анимируется без каких-либо других действий.

Принудительное касание не связано с каким бы то ни было определенным элементом на экране; к контроллеру интерфейса можно присоединить только одно меню, которое будет отображаться независимо от того, где на экране появляется Force Touch нажатии.

Можно представить между одним и четырьмя параметрами меню.

## <a name="adding-a-menu"></a>Добавление меню

`Menu`Необходимо добавить в `InterfaceController` в раскадровке во время разработки. При перетаскивании элемента управления Menu на контроллер интерфейса отсутствует визуальное указание на просмотр раскадровки, но **меню** появляется на панели **структуры документа** :

![Изменение меню во время разработки](menu-images/menu-action.png)

В элемент управления Menu можно добавить до четырех пунктов меню. Их можно настроить на панели **свойств** . Можно задать следующие атрибуты:

- Заголовок и
- Пользовательский образ или
- Образ системы: принятие, добавление, блокировка, отклонение, информация, возможно, больше, Выкл., пауза, воспроизведение, повторение, возобновление, поделиться, передвинуть, докладчик, корзина.

Создайте `Action` , выбрав раздел **события** на панели **Свойства** и введя имя метода действия. Разделяемый метод будет создан в коде, который можно реализовать в классе контроллера интерфейса следующим образом:

```csharp
partial void MenuItemTapped ()
{
    Console.WriteLine ("A menu item was tapped.");
}
```

### <a name="custom-images"></a>Пользовательские образы

Как и в изображениях вкладок в iOS, для изображений пунктов меню требуется непрозрачный шаблон с альфа-каналом, позволяющий отображать фон.

Для лучшей производительности следует добавить изображения, используемые для меню, в проект Watch App (а не проект расширения приложения).

## <a name="changing-the-menu-items"></a>Изменение пунктов меню

<!--
### Design Time Items

Menu items added the storyboard can be shown and hidden programmatically.
-->

### <a name="adding-at-runtime"></a>Добавление во время выполнения

Добавление объекта в `Menu` Контроллер интерфейса во время выполнения невозможно, хотя коллекция `MenuItem` s *может* быть изменена программным способом.
Используйте `AddMenuItem` метод, как показано ниже.

```csharp
AddMenuItem (WKMenuItemIcon.Accept, "Yes", new ObjCRuntime.Selector ("tapped"));
```

API комплекта отслеживания Xamarin. iOS в настоящее время требует `selector` для `AdMenuItem` метода, который должен быть объявлен следующим образом:

```csharp
[Export("tapped")]
void MenuItemTapped ()
{
    Console.WriteLine ("The dynamically added 'Yes' menu item was tapped.");
}
```

### <a name="removing-at-runtime"></a>Удаление во время выполнения

`ClearAllMenuItems`Метод может быть вызван для удаления всех элементов меню, *добавленных программным способом* .

Не удается очистить элементы меню, настроенные в раскадровке.

## <a name="related-links"></a>Связанные ссылки

- [Ватчкиткаталог (пример)](/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Документ меню Apple](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Menus.html)