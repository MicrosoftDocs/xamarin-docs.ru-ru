---
title: Несколько окон для iPad в Xamarin. iOS
description: Добавление поддержки нескольких окон в приложения iPad.
ms.prod: xamarin
ms.assetid: 524b6f2e-dbdf-11e9-8a34-2a2ae2dbcce4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/20/2019
ms.openlocfilehash: ce7df59d41efdd2d151fd2ea73cf26b40ee7fa10
ms.sourcegitcommit: 834466c9d9cf35e9659e467ce0123e5f5ade6138
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/22/2020
ms.locfileid: "85129913"
---
# <a name="multiple-windows-for-ipad"></a>Использование нескольких окон для iPad

iOS 13 теперь поддерживает параллельные окна для одного приложения на iPad. Это позволяет создавать новые возможности и перетаскивать взаимодействия между окнами. В этом документе показано, как настроить приложение для поддержки этой функции, а также появились новые функции. 

## <a name="project-configuration"></a>Конфигурация проекта

Чтобы настроить проект для нескольких окон, измените атрибут, `info.plist` указав, что `NSUserActivityTypes` приложение iOS будет выполнять действия с этим типом, а также `UIApplicationSceneManifest` включить `UIApplicationSupportsMultipleScenes` для нескольких окон и `UISceneConfigurations` связать сцену с раскадровкой.

```xml
<key>NSUserActivityTypes</key>
<array>
    <string>com.xamarin.Gallery.openDetail</string>
</array>
<key>UIApplicationSceneManifest</key>
<dict>
    <key>UIApplicationSupportsMultipleScenes</key>
    <true/>
    <key>UISceneConfigurations</key>
    <dict>
        <key>UIWindowSceneSessionRoleApplication</key>
        <array>
            <dict>
                <key>UISceneConfigurationName</key>
                <string>Default Configuration</string>
                <key>UISceneDelegateClassName</key>
                <string>SceneDelegate</string>
                <key>UISceneStoryboardFile</key>
                <string>Main</string>
            </dict>
        </array>
    </dict>
</dict>
```

## <a name="opening-multiple-windows"></a>Открытие нескольких окон

Когда приложение открыто и выполняется на iPad, существует несколько способов открыть несколько окон этого приложения, по умолчанию обрабатывающих iOS.

- **Раскрыть приложение** . Коснитесь значка приложения на закрепления, чтобы перейти в этот режим, пока приложение открыто.
- Перетащите значок приложения с закрепления **на начало** работающего приложения, чтобы получить плавающее окно.
- **Разделить экран** . Перетащите значок приложения с стыковочного узла на край экрана iPad, чтобы создать новое параллельное окно.

Дополнительные взаимодействия для входа в режим с несколькими окнами доступны в приложении.

**Перетащите приложение из приложения** . Используйте взаимодействие перетаскивания в приложении, чтобы начать новый, `NSUserActivity` так же, как при перетаскивании значка приложения в предыдущих примерах.

При использовании [взаимодействия с перетаскиванием][0]вы создаете `NSUserActivity` и связываете данные, которые необходимо передать, в новое окно, которое вы запрашиваете для открытия iOS.

```csharp
public UIDragItem [] GetItemsForBeginningDragSession (UICollectionView collectionView, IUIDragSession session, NSIndexPath indexPath)
{
    var selectedPhoto = GetPhoto (indexPath);

    var userActivity = selectedPhoto.OpenDetailUserActivity ();
    var itemProvider = new NSItemProvider (UIImage.FromFile (selectedPhoto.Name));
    itemProvider.RegisterObject (userActivity, NSItemProviderRepresentationVisibility.All);

    var dragItem = new UIDragItem (itemProvider) {
        LocalObject = selectedPhoto
    };

    return new [] { dragItem };
}
```

В приведенном выше коде `selectedPhoto` объект модели имеет метод, возвращающий `NSUserActivity` вызванный `OpenDetailUserActivity()` . По завершении жеста перетаскивания, объект `UIDragItem` с представляется с `userActivity` помощью `NSItemProvider` .

**Явные действия** . пользовательские жесты на кнопках или ссылках предоставляют возможность открывать новое окно.

В можно `UIApplication` начать новый `UISceneSession` с помощью вызова `RequestSceneSessionActivation` . Если существующая сцена уже существует, следует использовать ее. По умолчанию будет создана новая сцена.

```csharp
public void ItemSelected(UICollectionView collectionView, NSIndexPath indexPath)
{
    var userActivity = selectedPhoto.OpenDetailUserActivity ();

    UIApplication.SharedApplication.RequestSceneSessionActivation(
        sceneSession: null,
        userActivity: userActivity,
        options: null,
        errorHandler: null
    );
}
```

В этом примере `userActivity` компонент является единственным значением, передаваемым в `RequestSceneSessionActivation` метод, чтобы открыть новое окно приложения на основе явного действия пользователя. в данном случае это `ItemSelected` обработчик объекта `UICollectionView` .

## <a name="related-links"></a>Связанные ссылки

- [Перетаскивание в Xamarin. iOS][0]

[0]: ~/ios/platform/introduction-to-ios11/drag-and-drop.md
