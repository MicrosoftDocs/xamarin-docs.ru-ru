---
title: Настраиваемые значки документов в Xamarin. iOS
description: В этой статье рассматривается включение ресурса изображения и управление им в приложении Xamarin. iOS, которое будет использоваться в качестве пользовательского значка типа документа.
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/23/2017
ms.openlocfilehash: 3a74084db461271ca7fd440ab2c9779f949b30ff
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436838"
---
# <a name="custom-document-icons-in-xamarinios"></a>Настраиваемые значки документов в Xamarin. iOS

_В этой статье рассматривается включение ресурса изображения и управление им в приложении Xamarin. iOS, которое будет использоваться в качестве пользовательского значка типа документа._

Если приложение Xamarin. iOS поддерживает загрузку определенного типа документа, разработчик может предоставить значки, которые система будет использовать при обнаружении этого типа документа, например, когда пользователь удерживает вложение в *почтовом приложении* , как показано ниже:

 [![Пример значков типа документа](custom-document-types-images/17.png)](custom-document-types-images/17.png#lightbox)

Разработчик может добавить сведения о типе документа для формата файла, который приложение может открывать, включая записи словаря для `CFBundleTypeName` строки и `LSItemContentTypes` массива в приложении `Info.plist` . Значки для типа документа попадают в `CFBundleTypeIconFiles` массив. Если значок документа не указан, iOS будет наследоваться от значка приложения.
Значки можно указывать для нескольких размеров, оптимизированных для различных способов разрешения устройств. 

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Чтобы назначить эти значения в Visual Studio для Mac, используйте раздел **типы документов** на вкладке **Дополнительно** в `Info.plist` редакторе, чтобы добавить тип документа и назначить ему значки изображений. Например, ниже приведен снимок экрана, показывающий регистрацию для поддержки PDF:

 [![Раздел "типы документов" на вкладке "Дополнительно" редактора "info. plist"](custom-document-types-images/18.png)](custom-document-types-images/18.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Чтобы назначить эти значения в Visual Studio, используйте раздел **типы документов** на вкладке **Дополнительно** на странице `Info.plist` :

 ![Откройте раздел типы документов на вкладке Дополнительно.](custom-document-types-images/doc01w.png)

Нажмите кнопку **Добавить тип документа** и заполните обязательные поля:

![Форма «Добавление типа документа»](custom-document-types-images/doc02w.png)

-----

Дополнительные сведения о типах документов см. в разделах [Справочник по единообразным идентификаторам типов](https://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html) Apple и [разделы по программированию документов для iOS](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html).

## <a name="related-links"></a>Связанные ссылки

- [Работа с изображениями (пример)](/samples/xamarin/ios-samples/workingwithimages)
- [Привет, iPhone](~/ios/get-started/hello-ios/index.md)
- [Пользовательские рекомендации по созданию значков и изображений](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)