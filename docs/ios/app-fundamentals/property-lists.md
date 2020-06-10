---
title: Работа со списками свойств в Xamarin. iOS
description: В этом документе Visual Studio для Mac представлен графический и расширенный редактор списка свойств (plist) для работы с info. plist и назначениями. plist. Здесь показано, как задать значки и изображения для запуска приложений iOS в Visual Studio для Mac.
ms.prod: xamarin
ms.assetid: 5E687043-0443-377C-9A12-9C5A05958646
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 6c2b5869f647f65b932b6ec92f359f8a79402c8f
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84569300"
---
# <a name="working-with-property-lists-in-xamarinios"></a>Работа со списками свойств в Xamarin. iOS

_В этом документе Visual Studio для Mac представлен графический и расширенный редактор списка свойств (plist) для работы с info. plist и назначениями. plist. Здесь показано, как задать значки и изображения для запуска приложений iOS в Visual Studio для Mac._

Visual Studio для Mac содержит графический редактор plist, который упрощает редактирование свойств и возможностей приложения. Visual Studio для Mac имеет два. плистс — `Info.plist` для редактирования свойств и значков приложения, а `Entitlements.plist` для управления возможностями приложения. В этом руководством содержатся сведения о работе с ними в Visual Studio для Mac. Сведения о правах на plist см. в разделе [Работа с](~/ios/deploy-test/provisioning/entitlements.md) назначениями.

## <a name="infoplist"></a>Info.plist

Список информационных свойств ( `Info.plist` ) — это обязательный файл iOS, который предоставляет сведения о конфигурации приложения в системе. `Info.plist`В пользовательском редакторе Visual Studio для Mac три панели, управляемые вкладками в нижней левой части окна редактора:

 [![](property-lists-images/tabs.png "The Info.plist editor tabs at the bottom left of the editor window")](property-lists-images/tabs.png#lightbox)

Каждая панель управляет различными свойствами, как описано ниже.

- **Панель приложений** — графический интерфейс для задания общих свойств приложения, а также значков и изображений запуска; Укажите режим интеграции карт и режимы фонового режима.
- **Расширенная панель** . на панели «Дополнительно» можно указать поддерживаемые типы документов, UTI и URL-адреса.
- **Панель исходного кода** — панель исходного кода управляет менее распространенными свойствами, а также пользовательскими свойствами приложения.

Следующие три раздела подробно изучите функции каждой панели.

## <a name="application-panel"></a>Панель приложения

Visual Studio для Mac содержит графический интерфейс для изменения общих `Info.plist` записей приложения.

1. Свойства приложения
1. Поддерживаемые типы устройств
1. Поддержка ориентации для каждого типа устройства
1. Стиль и цвет строки состояния
1. Значки и экраны запуска
1. Карты и фоновые режимы

Они описаны более подробно в следующих разделах.

 <a name="iOS_Application_Target"></a>

### <a name="ios-application-target"></a>Целевая версия приложения iOS

В этом разделе содержатся важные сведения, описывающие ваше приложение.
Сохраненный здесь **идентификатор** должен соответствовать идентификатору пакета, введенному в iTunes Connect (для приложений магазина приложений), а также в списке идентификаторов приложений портала подготовки iOS и сертификатах разработки и распространения.

 [![](property-lists-images/image24.png "iOS Application Target")](property-lists-images/image24.png#lightbox)

### <a name="device-deployment"></a>Развертывание устройства

 [![](property-lists-images/deployment.png "Device Deployment")](property-lists-images/deployment.png#lightbox)

Разделы сведений о **развертывании** устройств отображаются выборочно, в зависимости от выбора в раскрывающемся списке **устройства** в разделе **цель приложения** выше. Раскрывающийся список **главного интерфейса** установлен в **файл mainstoryboard** в приложениях, управляемых раскадровкой. Если пользовательский интерфейс полностью написан в коде, его можно оставить пустым.

### <a name="supported-device-orientations"></a>Поддерживаемые ориентации устройств

 **Поддерживаемые ориентации устройств** определяет, как приложение реагирует на вращение устройства. Приложения для iPhone и iPad очень часто поддерживают только **книжную ориентацию**или все, но не более **ногами**. Как правило, все приложения iPad, кроме игр, должны поддерживать все ориентации.

### <a name="status-bar-styles"></a>Стили строки состояния

В разделе **стили строки состояния** представлен графический интерфейс для редактирования приложения `UIStatusBarStyle` .

 [![](property-lists-images/status.png "Status Bar Styles")](property-lists-images/status.png#lightbox)

 <a name="Icons"></a>

### <a name="icons-launch-images-and-itunes-artwork"></a>Значки, изображения для запуска и иллюстрации iTunes

Сведения об использовании значков, изображений и иллюстраций в файле info. plist можно найти в разделе [Working with Images Guide (работа с образами](~/ios/app-fundamentals/images-icons/index.md) ).

### <a name="maps-integration-and-background-modes"></a>Интеграция карт и фоновые режимы

`Info.plist`Содержит специальные разделы для указания интеграции карт и режимов фонового режима. При выборе параметров, которые будут поддерживаться, в приложение будут добавлены необходимые свойства.

 [![](property-lists-images/maps.png "Maps Integration")](property-lists-images/maps.png#lightbox)

Дополнительные сведения о работе с картами см. в руководстве по использованию Xamarin [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md) .

 [![](property-lists-images/bging.png "Background Modes")](property-lists-images/bging.png#lightbox)

Дополнительные сведения о фоновых режимах см. в статье о фоновом развертывании Xamarin [в iOS](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md) .

## <a name="advanced-panel"></a>Расширенная панель

Панель «дополнительно» управляет типами документов и схемами URL-адресов, которые поддерживает приложение.

 [![](property-lists-images/image34.png "Advanced Panel")](property-lists-images/image34.png#lightbox)

 <a name="Document_Types"></a>

## <a name="document-types"></a>Типы документов

Для приложений, поддерживающих открытие файлов конкретных типов, в iOS имеется `CFBundleDocumentTypes` ключ. Если нам требуется, чтобы наше приложение поддерживало определенные известные типы файлов, например PDF-файлы, мы будем добавлять в ключ значение PDF. Этот раздел предоставляет удобный способ ввода данных, которые будут храниться в `CFBundleDocumentTypes` ключе `Info.plist` файла.

Дополнительные сведения о настройке этих значений см. в документации по [регистрации типов файлов, поддерживаемых приложением](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html) .

## <a name="utis"></a>UTI

Иногда приложению требуется поддержка открытия пользовательского типа файлов. Например, может потребоваться открыть файлы изображений с пользовательским расширением *ксам*. Чтобы указать пользовательский тип файла, мы создадим пользовательский UTI-универсальный идентификатор типа с помощью `UIExportedTypeDeclarations` ключа. На следующем снимке экрана показано, как создать пользовательский UTI для расширения. ксам:

 [![](property-lists-images/uti.png "UTIs Editor")](property-lists-images/uti.png#lightbox)

Как и экспортированный тип UTI укажите пользовательские UTI, относящиеся к вашему приложению, *импортированный тип UTI* ( `UIImportedTypeDeclarations` ключ) указывает, что пользовательские типы поддерживаются, но не принадлежат вашему приложению.

Дополнительные сведения об использовании пользовательских UTI см. в статье [Регистрация типов файлов, поддерживаемых приложением](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204-SW1) , в Apple.

## <a name="custom-urls"></a>Настраиваемый URL-адрес

Имя схемы URL-адреса (также называемое Protocol) является первой частью URL-адреса. Например, `http://` и `https://` являются общими схемами URL-адресов. Вы можете создать настраиваемую схему URL-адресов для приложения. Пользовательские схемы URL-адресов используются для обмена данными и их отправки с другими приложениями. На следующем снимке экрана показано, как создать настраиваемую схему URL-адресов с именем `monkeys://` :

 [![](property-lists-images/url.png "Custom URLs")](property-lists-images/url.png#lightbox)

Дополнительные сведения о реализации пользовательских схем URL-адресов см. в [разделе Реализация пользовательских схем URL-адресов в этом руководстве](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html) .

## <a name="source-panel"></a>Панель источника

На вкладке **источник** `Info.plist` файла можно добавлять или изменять пользовательские значения. Visual Studio для Mac предоставляет список наиболее распространенных свойств:

 [![](property-lists-images/image31.png "Adding a new property from a dropdown")](property-lists-images/image31.png#lightbox)

Для известных свойств Visual Studio для Mac будет иметь список допустимых значений, как показано на следующем снимке экрана:

 [![](property-lists-images/image32.png "Select a value from a know value list")](property-lists-images/image32.png#lightbox)

Visual Studio для Mac также определяет тип свойства, как показано ниже.

 [![](property-lists-images/image33.png "The available property types")](property-lists-images/image33.png#lightbox)

Дополнительные сведения о дополнительных свойствах см. в ссылках на [ресурсы, связанные с приложением](https://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html) Apple.

 <a name="Entitlements"></a>

## <a name="summary"></a>Сводка

В этой статье показано, как использовать графические и Расширенные редакторы plist для изменения стандартных конфигураций приложений, а также для указания значков и образов запуска. Он также появился `Entitlements.plist` для добавления возможностей приложения и управления ими.

## <a name="related-links"></a>Связанные ссылки

- [IDE](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide)
- [Ресурсы, связанные с приложениями](https://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)
- [Регистрация типов файлов, поддерживаемых приложением](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)
- [Реализация пользовательских схем URL-адресов](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)
- [Справочник по формату каталога активов](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_ref-Asset_Catalog_Format/index.html#//apple_ref/doc/uid/TP40015170-CH18-SW1)
