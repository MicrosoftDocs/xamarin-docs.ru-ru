---
title: Раскадровки в Xamarin. Mac — Быстрое начало
description: В этом документе приведены краткие сведения о создании пользовательских интерфейсов macOS с помощью раскадровок в Xamarin. Mac. Здесь описывается создание перехода и создание окна параметров.
ms.prod: xamarin
ms.assetid: 20719B5D-8147-4E8A-A23C-8D575C7ACCEE
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 05/02/2017
ms.openlocfilehash: 007d835b71c16a51cf5f6aca3c9fe1edd5be0034
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84572394"
---
# <a name="storyboards-in-xamarinmac-quick-start"></a>Раскадровки в Xamarin. Mac — Быстрое начало

В качестве краткого знакомства с использованием раскадровок для определения пользовательского интерфейса приложения Xamarin. Mac начнем новый проект Xamarin. Mac. Выберите приложение **Mac**  >  **приложение**  >  **Cocoa** и нажмите кнопку **Далее** :

[![](quickstart-images/qs01.png "Adding a new Cocoa App")](quickstart-images/qs01.png#lightbox)

Используйте **имя приложения** `MacStoryboard` и нажмите кнопку **Далее** :

[![](quickstart-images/qs02.png "Setting the App Name")](quickstart-images/qs02.png#lightbox)

Используйте **имя проекта** по умолчанию и **имя решения** , а затем нажмите кнопку **создать** :

[![](quickstart-images/qs03.png "The project and solution names")](quickstart-images/qs03.png#lightbox)

В **Обозреватель решений**дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в Interface Builder Xcode:

[![](quickstart-images/qs04.png "Editing the storyboard in Xcode")](quickstart-images/qs04.png#lightbox)

Как видно выше, раскадровка по умолчанию определяет как строку меню приложения, так и ее главное окно с контроллером и представлением ИТ. Для нашего примера приложения мы будем создавать пользовательский интерфейс с основным _представлением содержимого_ на одной стороне и _представлением инспектора_ во втором.

Для этого сначала необходимо удалить контроллер представления по умолчанию и представление, поставляемое с раскадровкой, выбрав его в Interface Builder и нажав клавишу **Delete** :

[![](quickstart-images/qs05.png "Removing the default view controller")](quickstart-images/qs05.png#lightbox)

Затем введите `split` в область **фильтра** , выберите контроллер представления с вертикальным разделением и перетащите его на _область конструктора_:

[![](quickstart-images/qs06.png "Searching for the split view controller")](quickstart-images/qs06.png#lightbox)

Обратите внимание, что контроллер автоматически включал два дочерних контроллера представления (и связанные с ними представления), которые просматриваются с левой и правой сторон разделенного представления. Чтобы связать представление с разделением с родительским окном, нажмите клавишу **CTRL** , щелкните контроллер окна (синий кружок в кадре контроллера окна) и перетащите линию в контроллер разделенного представления. В всплывающем **окне выберите содержимое окна** :

[![](quickstart-images/qs07.png "Setting the windows content view")](quickstart-images/qs07.png#lightbox)

Это позволит связать два элемента интерфейса вместе с помощью перехода:

[![](quickstart-images/qs08.png "The Segue between the window and the content")](quickstart-images/qs08.png#lightbox)

Необходимо разместить текстовое представление в левой части разделенного представления и автоматически заполнять доступную область при изменении размера окна или разделенного представления. Перетащите текстовое представление в верхний контроллер представления, присоединенный к разделенному представлению, и щелкните **закрепить** автоматическое ограничение макета (второй значок справа в нижней части область конструктора).

[![](quickstart-images/qs09.png "Configuring the constraints")](quickstart-images/qs09.png#lightbox)

Здесь мы будем щелкать все четыре значка **I-образного указателя** вокруг ограничивающего прямоугольника в верхней части ограничений контекстном меню Action и нажимайте кнопку **Добавить 4 ограничения** внизу, чтобы добавить необходимые ограничения.

Если мы вернемся к Visual Studio для Mac и запустим проект, обратите внимание, что текстовое представление автоматически изменяет размер, чтобы заполнить левую часть разделенного представления по мере изменения размера окна или разбиения на части.

[![](quickstart-images/qs10.png "An example of the app running")](quickstart-images/qs10.png#lightbox)

Так как мы будем использовать правую часть разделенного представления в качестве области инспектора, нам нужно уменьшить размер и разрешить его сворачивание. Вернитесь в Xcode и измените представление для правой части, выбрав его в область конструктора и щелкнув **Инспектор размеров**. Здесь введите **ширину** `250` :

[![](quickstart-images/qs11.png "Setting the width")](quickstart-images/qs11.png#lightbox)

Затем выберите разделенный элемент, представляющий правую сторону, задайте более высокий **приоритет** и щелкните флажок **пользователь может свернуть** .

[![](quickstart-images/qs12.png "Editing the holding priority")](quickstart-images/qs12.png#lightbox)

Если вернуться к Visual Studio для Mac и запустить проект сейчас, обратите внимание, что в правой части сохраняется меньшее значение размера, а размер окна изменяется.

[![](quickstart-images/qs13.png "An example of the app running")](quickstart-images/qs13.png#lightbox)

<a name="Defining-a-Presentation-Segue"></a>

## <a name="defining-a-presentation-segue"></a>Определение презентации перехода

Мы создадим макет правой части представления с разделением, чтобы он действовал в качестве инспектора для свойств выбранного текста. Некоторые элементы управления будут перетаскиваться в нижний вид для разметки пользовательского интерфейса инспектора. Для последнего элемента управления мы хотим отобразить контекстном меню Action, позволяющий пользователю выбрать один из четырех предустановленных стилей символов.

В область конструктора будет добавлена кнопка для инспектора и контроллера представления. Мы изменим размер контроллера представления так, чтобы наши контекстном меню actionи и добавили четыре кнопки. Далее мы будем **управлять** ключами. Щелкните кнопку в представлении инспектора и перетащите к контроллеру представления, который будет представлять наш контекстном меню Action:

[![](quickstart-images/qs14.png "Dragging to create a new segue")](quickstart-images/qs14.png#lightbox)

В контекстном меню мы выберем **контекстном меню Action**: 

[![](quickstart-images/qs15.png "Selecting the segue type")](quickstart-images/qs15.png#lightbox)

Наконец, мы выберем перехода в область конструктора и настроили **Предпочитаемый ребро** **влево**. Затем перетащите линию из **представления привязки** к кнопке, к которой нужно присоединить контекстном меню Action:

[![](quickstart-images/qs16.png "Dragging to create a new segue")](quickstart-images/qs16.png#lightbox)

Если мы вернемся к Visual Studio для Mac, запустите приложение и нажмите кнопку **None (нет** ) в инспекторе, появится контекстном меню Action:

[![](quickstart-images/qs17.png "An example of the segue running")](quickstart-images/qs17.png#lightbox)

<a name="Creating-App-Preferences"></a>

## <a name="creating-app-preferences"></a>Создание настроек приложения

Большинство стандартных приложений macOS предоставляют _диалоговое окно предпочтения_ , позволяющее пользователю определить несколько параметров, управляющих различными аспектами приложения, например, внешний вид или учетные записи пользователей.

Чтобы определить диалоговое окно стандартного предпочтения, сначала перетащите контроллер представления вкладок на область конструктора:

[![](quickstart-images/qs18.png "Editing the storyboard in Xcode")](quickstart-images/qs18.png#lightbox)

Опять же, это автоматически получит два подключенных контроллера дочернего представления. Например, мы добавим метку к каждому представлению, которое будет центрироваться внутри нее:

[![](quickstart-images/qs19.png "Setting the constraints")](quickstart-images/qs19.png#lightbox)

Затем нужно отобразить окно настройки, когда пользователь нажимает пункт меню **предпочтения...** . В строке меню выберите пункт меню "Параметры" **, щелкните "ключ —** щелчок" и перетащите строку в наш контроллер представления вкладок:

[![](quickstart-images/qs20.png "Dragging to create a segue")](quickstart-images/qs20.png#lightbox)

Во всплывающем окне мы выберем **модальное** окно, чтобы отобразить его как модальное диалоговое окно:

[![](quickstart-images/qs21.png "Selecting the segue type")](quickstart-images/qs21.png#lightbox)

Если мы сохраняем изменения, вернитесь к Visual Studio для Mac, запустите приложение и выберите пункт меню **предпочтения...** в открывшемся диалоговом окне предпочтения.

[![](quickstart-images/qs22.png "An example of the segue running")](quickstart-images/qs22.png#lightbox)

Вы можете заметить, что это не похоже на стандартное диалоговое окно настроек macOS приложения. Чтобы устранить эту проблему, включите два файла изображений в папку приложения Xamarin. Mac `Resources` в **Обозреватель решений** и вернитесь в Interface Builder Xcode.

Выберите контроллер представления вкладок и переключите его **стиль** на **панель инструментов**: 

[![](quickstart-images/qs23.png "Setting the tab bar style")](quickstart-images/qs23.png#lightbox)

Выберите каждую вкладку и присвойте ей **метку** и выберите один из изображений, чтобы его представить:

[![](quickstart-images/qs24.png "Configuring each tab in Xcode")](quickstart-images/qs24.png#lightbox)

Если мы сохраняем изменения, вернитесь к Visual Studio для Mac, запустите приложение и выберите пункт меню **предпочтения...** , после чего диалоговое окно отобразится как стандартное приложение macOS:

[![](quickstart-images/qs25.png "An example of the running preferences window")](quickstart-images/qs25.png#lightbox)

Дополнительные сведения см. в документации по [работе с изображениями](~/mac/app-fundamentals/image.md), [меню](~/mac/user-interface/menu.md), [окнами](~/mac/user-interface/window.md) и документацией по [диалоговым окнам](~/mac/user-interface/dialog.md) .

## <a name="related-links"></a>Связанные ссылки

- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Работа с Windows](~/mac/user-interface/window.md)
- [Рекомендации по созданию пользовательских интерфейсов в macOS](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)
- [Введение в Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
