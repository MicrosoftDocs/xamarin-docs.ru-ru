---
title: 'Ошибка Ибтул: не удалось завершить операцию.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: ddeffd2dd01553b5beb8bca7827603a416c3955b
ms.sourcegitcommit: 4bbf54d2bc1df96af69814e2e5dae47be12e0474
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2021
ms.locfileid: "102603026"
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>Ошибка Ибтул: не удалось завершить операцию.

## <a name="fixed-in-xcode-611"></a>Исправлено в Xcode 6.1.1

Компания Apple [устранила](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1) эту `ibtool` ошибку в Xcode 6.1.1, поэтому обновление до Xcode 6.1.1 или более поздней версии является самым простым исправлением.

* * *

## <a name="description-of-the-problem"></a>Описание проблемы

В `ibtool` команде Xcode 6,0 возникла ошибка в OS X 10,10 Yosemite. Xamarin. iOS использует Xcode `ibtool` для компиляции раскадровки и `XIB` файлов.

Дополнительные сведения об ошибке в Xcode можно найти в следующей Stack Overflow записи: [https://stackoverflow.com/questions/25754763/cant-open-storyboard](https://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>Сообщение об ошибке

> Не удалось открыть документ "файл mainstoryboard. Storyboard". не удалось завершить операцию. (ошибка com. Apple. Интерфацебуилдер-1.)

## <a name="workarounds-for-xcode-60"></a>Обходные пути (для Xcode 6,0)

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>Вариант 1. Управление всеми `UIImageView.Image` свойствами в коде

Вместо того чтобы задавать `Image` свойство объекта `UIImageView` в раскадровке или `.xib` файле, можно задать свойство в одном из методов переопределения жизненного цикла представления в контроллере представления (например, в `ViewDidLoad()` ). Советы по использованию Vs см. в разделе также [Работа с образами](~/ios/app-fundamentals/images-icons/index.md) `UIImage.FromBundle()` . `UIImage.FromFile()`

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>Вариант 2. Перемещение всех ресурсов образа в папку верхнего уровня `Resources`

После перемещения образов в папку верхнего уровня потребуется `Resources` Обновить раскадровку и `.xib` файлы, чтобы использовать новые пути к изображениям.

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>Вариант 3. Задайте `LogicalName` для всех проблемных ресурсов изображений, чтобы они были скопированы на верхний уровень `.app` пакета.

Например, предположим, что исходный `.csproj` файл содержит следующую запись:

`<BundleResource Include="Resources\Images\image.png" />`

Этот элемент можно изменить и добавить, `LogicalName` чтобы изображение было скопировано на верхний уровень `.app` пакета:

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

В Visual Studio для Mac `LogicalName` можно также задать, используя `Resource ID` поле для изображения в разделе **View > Pad > Properties**. (См. также: [https://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545](https://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545) )

После этого изменения необходимо будет обновить раскадровку и файлы, `.xib` чтобы использовать новые пути к изображениям верхнего уровня. В Visual Studio и Visual Studio для Mac необходимо изменить путь для `Image` свойства вручную.

### <a name="next-steps"></a>Следующие шаги

Чтобы получить дополнительную помощь, или если проблема остается даже после использования приведенных выше сведений, ознакомьтесь с возможностями [поддержки Xamarin?](~/cross-platform/troubleshooting/support-options.md) для получения сведений о вариантах контактов, предложениях, а также о том, как при необходимости создать новую ошибку. 
