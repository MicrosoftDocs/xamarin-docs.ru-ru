---
title: основы конструктора iOS
description: В этом руководством описывается Xamarin Designer для iOS. В нем демонстрируется использование конструктора iOS для визуального размещения элементов управления, получения доступа к этим элементам управления в коде и изменения свойств.
ms.prod: xamarin
ms.assetid: E7045E41-0DEF-416B-BCDB-52502350F61C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 01/31/2018
ms.openlocfilehash: 422b84c456e6fd9e1efb798bbf53959bd8423d07
ms.sourcegitcommit: 4bbf54d2bc1df96af69814e2e5dae47be12e0474
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2021
ms.locfileid: "102603143"
---
# <a name="ios-designer-basics"></a>основы конструктора iOS

_В этом руководством описывается Xamarin Designer для iOS. В нем демонстрируется использование конструктора iOS для визуального размещения элементов управления, получения доступа к этим элементам управления в коде и изменения свойств._

> [!WARNING]
> Конструктор iOS устарел в Visual Studio 2019 версии 16,8 и Visual Studio 2019 для Mac версии 8,8 и удален в Visual Studio 2019 версии 16,9 и Visual Studio для Mac версии 8,9.
> Рекомендуемый способ создания пользовательских интерфейсов iOS — непосредственно на компьютере Mac с Xcode. Дополнительные сведения см. в разделе [Разработка пользовательских интерфейсов с помощью Xcode](../storyboards/index.md). 

Xamarin Designer для iOS является конструктором визуальных интерфейсов, похожим на Interface Builder и Android Designer Xcode. Некоторые из его многих функций включают в себя простую интеграцию с Visual Studio для Windows и Mac, редактирование с помощью перетаскивания, интерфейс для настройки обработчиков событий, а также возможность отрисовки пользовательских элементов управления.

## <a name="requirements"></a>Требования

Конструктор iOS доступен в Visual Studio для Mac и Visual Studio 2017 и более поздних версий в Windows. В Visual Studio для Windows конструктор iOS требует подключения к правильно настроенному узлу сборки Mac, хотя Xcode не требуется.

В этом руководстве предполагается, что вы знакомы с содержанием, изложенным в [руководствах по начало работы](~/ios/get-started/index.md).

<a name="how-it-works"></a>

## <a name="how-the-ios-designer-works"></a>Как работает конструктор iOS

В этом разделе описывается, как конструктор iOS упрощает создание пользовательского интерфейса и его подключение к коду.

Конструктор iOS позволяет разработчикам визуально проектировать пользовательский интерфейс приложения. Как описано в разделе [Общие сведения о раскадровках](~/ios/user-interface/storyboards/index.md) , раскадровка Описывает экраны (контроллеры представлений), которые составляют приложение, элементы интерфейса (представления), размещенные на этих контроллерах представления, и общий поток навигации приложения. 

Контроллер представления состоит из двух частей: визуального представления в конструкторе iOS и связанного класса C#:

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Контроллер представления в конструкторе iOS](introduction-images/1-storyboardwithviewcontroller-vsmac.png "Контроллер представления в конструкторе iOS")](introduction-images/1-storyboardwithviewcontroller-vsmac-large.png#lightbox)

[![Код для контроллера представления](introduction-images/2-viewcontrollercode-vsmac.png "Код для контроллера представления")](introduction-images/2-viewcontrollercode-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Контроллер представления в конструкторе iOS](introduction-images/1-storyboardwithviewcontroller-vs.png "Контроллер представления в конструкторе iOS")](introduction-images/1-storyboardwithviewcontroller-vs-large.png#lightbox)

[![Код для контроллера представления](introduction-images/2-viewcontrollercode-vs.png "Код для контроллера представления")](introduction-images/2-viewcontrollercode-vs-large.png#lightbox)

-----

В состоянии по умолчанию контроллер представления не предоставляет никаких функциональных возможностей. Он должен быть заполнен элементами управления. Эти элементы управления помещаются в представление контроллера представления — прямоугольную область, содержащую все содержимое экрана. Большинство контроллеров представления содержат общие элементы управления, такие как кнопки, метки и текстовые поля, как показано на следующем снимке экрана, на котором показан контроллер представления, содержащий кнопку: 

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Контроллер представления, содержащий кнопку](introduction-images/3-viewcontrollerwithbutton-vsmac.png "Контроллер представления, содержащий кнопку")](introduction-images/3-viewcontrollerwithbutton-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Контроллер представления, содержащий кнопку](introduction-images/3-viewcontrollerwithbutton-vs.png "Контроллер представления, содержащий кнопку")](introduction-images/3-viewcontrollerwithbutton-vs-large.png#lightbox)

-----

Некоторые элементы управления, например метки, содержащие статический текст, можно добавить в контроллер представления и только слева. Однако чаще всего элементы управления должны быть настроены программно. Например, кнопка, добавленная выше, должна делать что-то при касании, поэтому обработчик событий должен быть добавлен в код.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Чтобы получить доступ к кнопке в коде и управлять ей, она должна иметь уникальный идентификатор. Укажите уникальный идентификатор, нажав кнопку, открыв **панель свойств** и задав в поле **имя** значение, например "SubmitButton":

[![Задание имени кнопки в Панель свойств](introduction-images/4-settingbuttonname-vsmac.png "Задание имени кнопки в Панель свойств")](introduction-images/4-settingbuttonname-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Чтобы получить доступ к кнопке в коде и управлять ей, она должна иметь уникальный идентификатор. Укажите уникальный идентификатор, нажав кнопку, открыв окно " **Свойства**" и задав в поле " **имя** " значение, например "SubmitButton":

[![Задание имени кнопки в окне "Свойства"](introduction-images/4-settingbuttonname-vs.png "Задание имени кнопки в окне "Свойства"")](introduction-images/4-settingbuttonname-vs-large.png#lightbox)

-----

Теперь, когда у кнопки есть имя, доступ к ней можно получить в коде. Но как это работает?

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

В **панель решения** перейдите к **ViewController.CS** и щелкните индикатор раскрытия, чтобы определить, что определение класса контроллера представления `ViewController` охватывает два файла, каждый из которых содержит определение [разделяемого класса](/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) :

[![Два файла, составляющие класс ViewController: ViewController.cs и ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vsmac.png "Два файла, составляющие класс ViewController: ViewController.cs и ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

В **Обозреватель решений** перейдите к **ViewController.CS** и щелкните индикатор раскрытия, чтобы определить, что определение класса контроллера представления `ViewController` охватывает два файла, каждый из которых содержит определение [разделяемого класса](/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) :

[![Два файла, составляющие класс ViewController: ViewController.cs и ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vs.png "Два файла, составляющие класс ViewController: ViewController.cs и ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vs-large.png#lightbox)

-----

- **ViewController.CS** должен быть заполнен пользовательским кодом, связанным с `ViewController` классом. В этом файле `ViewController` класс может отвечать на различные методы жизненного цикла контроллера представления iOS, настраивать пользовательский интерфейс и отвечать на вводимые пользователем данные, такие как нажатия кнопки.

- **ViewController.Designer.CS** — это созданный файл, созданный конструктором iOS для преобразования визуально сконструированного интерфейса в код. Так как изменения в этом файле будут перезаписаны, его не следует изменять. Объявления свойств в этом файле позволяют коду в `ViewController` классе получать доступ по **имени**, элементам управления, настроенным в конструкторе iOS. При открытии **ViewController.Designer.CS** открывается следующий код:

```csharp
namespace Designer
{
    [Register ("ViewController")]
    partial class ViewController
    {
        [Outlet]
        [GeneratedCode ("iOS Designer", "1.0")]
        UIKit.UIButton SubmitButton { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (SubmitButton != null) {
                SubmitButton.Dispose ();
                SubmitButton = null;
            }
        }
    }
}
```

`SubmitButton`Объявление свойства соединяет весь класс, а `ViewController` не только файл **ViewController.Designer.CS** — с кнопкой, определенной в раскадровке. Поскольку **ViewController.CS** определяет часть `ViewController` класса, у него есть доступ к `SubmitButton` .

На следующем снимке экрана показано, что IntelliSense теперь распознает `SubmitButton` ссылку в **ViewController.CS**:

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![IntelliSense, распознающий ссылку на SubmitButton](introduction-images/6-submitbuttonintellisense-vsmac.png "IntelliSense, распознающий ссылку на SubmitButton")](introduction-images/6-submitbuttonintellisense-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![IntelliSense, распознающий ссылку на SubmitButton](introduction-images/6-submitbuttonintellisense-vs.png "IntelliSense, распознающий ссылку на SubmitButton")](introduction-images/6-submitbuttonintellisense-vs-large.png#lightbox)

-----

В этом разделе показано, как создать кнопку в конструкторе iOS и получить доступ к этой кнопке в коде.

В оставшейся части этого документа представлен более новый обзор конструктора iOS.

## <a name="ios-designer-basics"></a>основы конструктора iOS

В этом разделе представлены части конструктора iOS и представлен обзор его возможностей.

### <a name="launching-the-ios-designer"></a>Запуск конструктора iOS

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Проекты Xamarin. iOS, созданные с помощью Visual Studio для Mac, включают раскадровку. Чтобы просмотреть содержимое раскадровки, дважды щелкните файл. Storyboard в **панель решения**:

[![Открытие раскадровки в конструкторе iOS](introduction-images/7-storyboardopen-vsmac.png "Открытие раскадровки в конструкторе iOS")](introduction-images/7-storyboardopen-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Большинство проектов Xamarin. iOS, созданных с помощью Visual Studio, включают раскадровку. Чтобы просмотреть содержимое раскадровки, дважды щелкните файл. Storyboard в **Обозреватель решений**:

[![Открытие раскадровки в конструкторе iOS](introduction-images/7-storyboardopen-vs.png "Открытие раскадровки в конструкторе iOS")](introduction-images/7-storyboardopen-vs-large.png#lightbox)

-----

<a name="iOS_Designer_features"></a>

### <a name="ios-designer-features"></a>функции конструктора iOS

В конструкторе iOS есть шесть основных разделов:

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Разделы конструктора iOS](introduction-images/8-sixpartsofiosdesigner-vsmac.png "Разделы конструктора iOS")](introduction-images/8-sixpartsofiosdesigner-vsmac-large.png#lightbox)

1. **Область конструктора** — Основная рабочая область конструктора iOS. В области документа он включает визуальное построение пользовательских интерфейсов.
2. **Панель инструментов "ограничения** " — позволяет переключаться между режимом редактирования кадров и режимом редактирования ограничений, двумя разными способами размещения элементов в пользовательском интерфейсе.
3. **Панель элементов** — перечисляет контроллеры, объекты, элементы управления, представления данных, средства распознавания жестов, окна и панели, которые можно перетащить в область конструктора и добавить в пользовательский интерфейс.
4. **Панель свойств** — показывает свойства выбранного элемента управления, включая удостоверение, стили визуальных элементов, доступность, макет и поведение.
5. **Структура документа** — показывает дерево элементов управления, образующих макет для редактируемого интерфейса. Щелкнув элемент в дереве, вы выделяете его в конструкторе iOS и отображает его свойства в  **панель свойств**. Это удобно для выбора определенного элемента управления в глубоко вложенном пользовательском интерфейсе.
6. **Нижняя панель инструментов** — содержит параметры для изменения способа отображения файла. Storyboard или. XIB в конструкторе iOS, включая устройство, ориентацию и масштаб.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Разделы конструктора iOS](introduction-images/8-sixpartsofiosdesigner-vs.png "Разделы конструктора iOS")](introduction-images/8-sixpartsofiosdesigner-vs-large.png#lightbox)

1. **Область конструктора** — Основная рабочая область конструктора iOS. В области документа он включает визуальное построение пользовательских интерфейсов.
2. **Панель инструментов "ограничения** " — позволяет переключаться между режимом редактирования кадров и режимом редактирования ограничений, двумя разными способами размещения элементов в пользовательском интерфейсе.
3. **Панель элементов** — перечисляет контроллеры, объекты, элементы управления, представления данных, средства распознавания жестов, окна и панели, которые можно перетащить в область конструктора и добавить в пользовательский интерфейс.
4. **Окно "свойства** " — отображаются свойства выбранного элемента управления, включая удостоверение, стили визуальных элементов, доступность, макет и поведение.
5. **Структура документа** — показывает дерево элементов управления, образующих макет для редактируемого интерфейса. Щелкнув элемент в дереве, вы выделяете его в конструкторе iOS и отображает его свойства в  **окне Свойства**. Это удобно для выбора определенного элемента управления в глубоко вложенном пользовательском интерфейсе.
6. **Нижняя панель инструментов** — содержит параметры для изменения способа отображения файла. Storyboard или. XIB в конструкторе iOS, включая устройство, ориентацию и масштаб.

-----

### <a name="design-workflow"></a>Проектирование рабочего процесса

#### <a name="adding-a-control-to-the-interface"></a>Добавление элемента управления в интерфейс

Чтобы добавить элемент управления в интерфейс, перетащите его с **панели элементов** и поместите в область конструктора. При добавлении или размещении элемента управления вертикальные и горизонтальные направляющие выделяют часто используемые положения макета, такие как Вертикальное центрирование, горизонтальное центрирование и поля:

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

![В области конструктора направляющие выделяют часто используемые положения макета](introduction-images/9-layoutguides-vsmac.png "В области конструктора направляющие выделяют часто используемые положения макета")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![В области конструктора направляющие выделяют часто используемые положения макета](introduction-images/9-layoutguides-vs.png "В области конструктора направляющие выделяют часто используемые положения макета")

-----

Синяя пунктирная линия в приведенном выше примере предоставляет горизонтальное руководство по выравниванию по центру, помогающее в размещении кнопок.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

#### <a name="context-menu-commands"></a>Команды контекстного меню

Контекстное меню доступно как в области конструктора, так и в **структуре документа**. Это меню содержит команды для выбранного элемента управления и его родителя, что полезно при работе с представлениями во вложенной иерархии.

[![Контекстное меню в области конструктора](introduction-images/10-contextmenudesignsurface-vsmac.png "Контекстное меню в области конструктора")](introduction-images/10-contextmenudesignsurface-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

-----

### <a name="constraints-toolbar"></a>Панель инструментов "ограничения"

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Панель инструментов очередностью](introduction-images/11-constraintstoolbar-vsmac.png "Панель инструментов "ограничения"")](introduction-images/11-constraintstoolbar-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Панель инструментов очередностью](introduction-images/11-constraintstoolbar-vs.png "Панель инструментов "ограничения"")](introduction-images/11-constraintstoolbar-vs-large.png#lightbox)

-----

Панель инструментов "ограничения" обновлена и теперь состоит из двух элементов управления: режим редактирования кадров/режим редактирования ограничений и кнопка Обновить ограничения/обновить кадры.

#### <a name="frame-editing-mode--constraint-editing-mode-toggle"></a>Переключение режима редактирования кадров или режима редактирования ограничений

В предыдущих версиях конструктора iOS, щелкнув уже выбранное представление в области конструктора, переключитесь между режимом редактирования кадров и режимом изменения ограничений. Теперь выключатель на панели инструментов ограничения переключается между этими режимами редактирования.

- Режим редактирования кадров:

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

![Кнопка режима редактирования кадров](introduction-images/12a-frameeditingmode-vsmac.png "Кнопка режима редактирования кадров")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Кнопка режима редактирования кадров](introduction-images/12a-frameeditingmode-vs.png "Кнопка режима редактирования кадров")

-----

- Режим редактирования ограничений:

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

![Кнопка режима изменения ограничений](introduction-images/12b-constrainteditingmode-vsmac.png "Кнопка режима изменения ограничений")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Кнопка режима изменения ограничений](introduction-images/12b-constrainteditingmode-vs.png "Кнопка режима изменения ограничений")

-----

#### <a name="update-constraints--update-frames-button"></a>Кнопка обновления ограничений/обновить кадры

Кнопка Обновить ограничения/обновить кадры располагается справа от переключателя режим редактирования кадров/режим редактирования ограничений.

- В режиме редактирования кадров нажатие этой кнопки настраивает кадры всех выбранных элементов в соответствии с их ограничениями.
- В режиме редактирования ограничений нажатие этой кнопки настраивает ограничения для всех выбранных элементов в соответствии с их кадрами.

### <a name="bottom-toolbar"></a>Нижняя панель инструментов

Нижняя панель инструментов предоставляет способ выбора устройства, ориентации и масштаба, используемого для просмотра раскадровки или файла XIB в конструкторе iOS:

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Нижняя панель инструментов, используемая для выбора устройства и ориентации для области конструктора.](introduction-images/13-bottomtoolbar-vsmac.png "Нижняя панель инструментов, используемая для выбора устройства и ориентации для области конструктора.")](introduction-images/13-bottomtoolbar-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Нижняя панель инструментов, используемая для выбора устройства и ориентации для области конструктора.](introduction-images/13-bottomtoolbar-vs.png "Нижняя панель инструментов, используемая для выбора устройства и ориентации для области конструктора.")](introduction-images/13-bottomtoolbar-vs-large.png#lightbox)

-----

#### <a name="device-and-orientation"></a>Устройство и ориентация

При развертывании Нижняя панель инструментов отображает все устройства, ориентации и (или) адаптации, применимые к текущему документу. Щелкнув их, вы измените представление, отображаемое в области конструктора. 

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Нижняя панель инструментов, развернутая для отображения устройств и ориентаций](introduction-images/14-bottomtoolbarexpanded-vsmac.png "Нижняя панель инструментов, развернутая для отображения устройств и ориентаций")](introduction-images/14-bottomtoolbarexpanded-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Нижняя панель инструментов, развернутая для отображения устройств и ориентаций](introduction-images/14-bottomtoolbarexpanded-vs.png "Нижняя панель инструментов, развернутая для отображения устройств и ориентаций")](introduction-images/14-bottomtoolbarexpanded-vs-large.png#lightbox)

-----

Обратите внимание, что при выборе устройства и ориентации изменяется только то, как конструктор iOS предварительно просматривает проект. Независимо от текущего выбора, новые добавленные ограничения применяются ко всем устройствам и ориентациям, если только кнопка **изменить признаки** не использовалась для указания другого значения.

Если [включены](~/ios/user-interface/storyboards/unified-storyboards.md#enabling-size-classes) [классы размера](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) , кнопка **изменить признаки** появится в развернутой нижней панели инструментов.  При нажатии кнопки " **изменить признаки** " отображаются параметры для создания варианта интерфейса на основе класса Size, представленного выбранным устройством и ориентацией. Рассмотрим следующие примеры:

- Если выбрано значение **iPhone SE**  /  **Книжная**, контекстном меню Action предоставит параметры для создания варианта интерфейса для компактной ширины, класса обычного размера высоты. 
- Если выбран режим "альбомный полный экран **" iPad Pro 9,7 "**  /    /   , контекстном меню Action предоставит варианты для создания варианта интерфейса для обычной ширины, класса обычного размера высоты.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Нижняя панель инструментов, используемая для изменения интерфейса по классу size](introduction-images/15-edittraitsbutton-vsmac.png "Нижняя панель инструментов, используемая для изменения интерфейса по классу size")](introduction-images/15-edittraitsbutton-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Нижняя панель инструментов, используемая для изменения интерфейса по классу size](introduction-images/15-edittraitsbutton-vs.png "Нижняя панель инструментов, используемая для изменения интерфейса по классу size")](introduction-images/15-edittraitsbutton-vs-large.png#lightbox)

-----

#### <a name="zoom-controls"></a>Элементы управления масштабом

Область конструктора поддерживает масштаб несколькими элементами управления:

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

![Элементы управления масштабом на нижней панели инструментов](introduction-images/16-zoomcontrols-vsmac.png "Элементы управления масштабом на нижней панели инструментов")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Элементы управления масштабом на нижней панели инструментов](introduction-images/16-zoomcontrols-vs.png "Элементы управления масштабом на нижней панели инструментов")

-----

К элементам управления относятся следующие.

1. Масштаб по размеру
2. Уменьшение масштаба.
3. Увеличение масштаба.
4. Фактический размер (размер 1:1 пикселей)

Эти элементы управления настраивают масштаб в области конструктора. Они не влияют на пользовательский интерфейс приложения во время выполнения.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

### <a name="properties-pad"></a>Панель свойств

Используйте **панель свойств** для изменения идентификатора, визуальных стилей, специальных возможностей и поведения элемента управления. На следующем снимке экрана показаны параметры **панель свойств** для кнопки:

[![Панель свойств для кнопки](introduction-images/17-buttonpropertiespad-vsmac.png "Панель свойств для кнопки")](introduction-images/17-buttonpropertiespad-vsmac-large.png#lightbox)
#### <a name="properties-pad-sections"></a>Панель свойств разделы

**Панель свойств** содержит три раздела:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

### <a name="properties-window"></a>Окно «Свойства»

Используйте **окно Свойства** для изменения идентификатора, визуальных стилей, специальных возможностей и поведения элемента управления. На следующем снимке экрана показаны параметры **окна свойств** для кнопки:

[![Окно "Свойства" для кнопки](introduction-images/17-buttonpropertieswindow-vs.png "Окно "Свойства" для кнопки")](introduction-images/17-buttonpropertieswindow-vs-large.png#lightbox)

#### <a name="properties-window-sections"></a>Разделы окна "Свойства"

**Окно Свойства** содержит три раздела:

-----

1. **Мини** -приложение — основные свойства элемента управления, такие как имя, класс, свойства стиля и т. д. Здесь обычно размещаются свойства для управления содержимым элемента управления.
2. **Макет** — свойства, которые отслеживают положение и размер элемента управления, включая ограничения и фреймы, перечислены здесь.
3. **События** — события и обработчики событий указываются здесь. Полезно для обработки таких событий, как касание, касание, перетаскивание и т. д. События также могут обрабатываться непосредственно в коде.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

#### <a name="editing-properties-in-the-properties-pad"></a>Изменение свойств в Панель свойств

В дополнение к визуальному редактированию в области конструктора, конструктор iOS поддерживает изменение свойств в **панель свойств**. Доступные свойства изменяются на основе выбранного элемента управления, как показано на снимках экрана ниже:

[![Свойства кнопки](introduction-images/18a-buttonpropertiespad-vsmac.png "Свойства кнопки")](introduction-images/18a-buttonpropertiespad-vsmac-large.png#lightbox)

[![Просмотр свойств контроллера](introduction-images/18b-viewcontrollerpropertiespad-vsmac.png "Просмотр свойств контроллера")](introduction-images/18b-viewcontrollerpropertiespad-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

#### <a name="editing-properties-in-the-properties-window"></a>Изменение свойств в окне "Свойства"

В дополнение к визуальному редактированию в области конструктора, конструктор iOS поддерживает изменение свойств в **окне Свойства**. Доступные свойства изменяются на основе выбранного элемента управления, как показано на снимках экрана ниже:

[![Свойства кнопки](introduction-images/18a-buttonpropertieswindow-vs.png "Свойства кнопки")](introduction-images/18a-buttonpropertieswindow-vs-large.png#lightbox)

[![Просмотр свойств контроллера](introduction-images/18b-viewcontrollerpropertieswindow-vs.png "Просмотр свойств контроллера")](introduction-images/18b-viewcontrollerpropertieswindow-vs-large.png#lightbox)

-----

> [!IMPORTANT]
> В разделе Identity (удостоверение) Панель свойств теперь отображается поле **Module (модуль** ). Этот раздел необходимо заполнять только при взаимодействии с классами SWIFT. Используйте его для ввода имени модуля для классов SWIFT, которые являются пространствами имен.

#### <a name="default-values"></a>Значения по умолчанию

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Многие свойства в **панель свойств** не отображают значение или значение по умолчанию. Однако код приложения по-прежнему может изменять эти значения. В **панель свойств** не отображаются значения, заданные в коде.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Многие свойства в **окне Свойства** не отображают значение или значение по умолчанию. Однако код приложения по-прежнему может изменять эти значения. В **окне Свойства** не отображаются значения, заданные в коде.

-----

#### <a name="event-handlers"></a>Обработчики событий

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Чтобы указать пользовательские обработчики событий для различных событий, используйте вкладку **события** **панель свойств**. Например, на следующем снимке экрана `HandleClick` метод обрабатывает **касание** кнопки в событии:

[![Панель свойств с набором обработчика событий для кнопки](introduction-images/19-buttonpropertiespadevents-vsmac.png "Панель свойств с набором обработчика событий для кнопки")](introduction-images/19-buttonpropertiespadevents-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Чтобы указать пользовательские обработчики событий для различных событий, используйте вкладку **события** **окна свойства**. Например, на следующем снимке экрана `HandleClick` метод обрабатывает **касание** кнопки в событии:

[![Окно свойств с набором обработчиков событий для кнопки](introduction-images/19-buttonpropertieswindowevents-vs.png "Окно свойств с набором обработчиков событий для кнопки")](introduction-images/19-buttonpropertieswindowevents-vs-large.png#lightbox)

-----

После указания обработчика событий необходимо добавить метод с таким же именем в соответствующий класс контроллера представления. В противном случае `unrecognized selector` при нажатии кнопки возникнет исключение:

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Нераспознанное исключение селектора](introduction-images/20-unrecognizedselector-vsmac.png "Нераспознанное исключение селектора")](introduction-images/20-unrecognizedselector-vsmac-large.png#lightbox)

Обратите внимание, что после указания обработчика событий в **панель свойств** конструктор iOS немедленно откроет соответствующий файл кода и предложит вставить объявление метода. 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Нераспознанное исключение селектора](introduction-images/20-unrecognizedselector-vs.png "Нераспознанное исключение селектора")](introduction-images/20-unrecognizedselector-vs-large.png#lightbox)

-----

Пример использования пользовательских обработчиков событий см. в [руководстве по Hello, iOS начало работы](~/ios/get-started/hello-ios/index.md).

### <a name="outline-view"></a>представление "Структура"

Конструктор iOS также может отображать иерархию интерфейса элементов управления в виде структуры. Чтобы получить доступ к структуре, перейдите на вкладку **Структура документа** , как показано ниже.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Структура документа](introduction-images/21-buttonoutlineview-vsmac.png "Структура документа")](introduction-images/21-buttonoutlineview-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Структура документа](introduction-images/21-buttonoutlineview-vs.png "Структура документа")](introduction-images/21-buttonoutlineview-vs-large.png#lightbox)

-----

Выбранный элемент управления в режиме структуры остается синхронизированным с выбранным элементом управления в области конструктора.  Эта функция полезна для выбора элемента из иерархии интерфейсов с глубиной вложенности.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

## <a name="revert-to-xcode"></a>Вернуться к Xcode

Можно использовать конструктор iOS и Xcode Interface Builder взаимозаменяемыми. Чтобы открыть файл Storyboard или XIB в Xcode Interface Builder, щелкните его правой кнопкой мыши и выберите **Открыть с помощью > Xcode Interface Builder**, как показано на снимке экрана ниже:

[![Открытие раскадровки в Xcode Interface Builder](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "Открытие раскадровки в Xcode Interface Builder")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png#lightbox)

После внесения изменений в Xcode Interface Builder сохраните файл и вернитесь в Visual Studio для Mac. Изменения будут синхронизированы с проектом Xamarin. iOS.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="revert-to-xcode"></a>Вернуться к Xcode

Можно использовать конструктор iOS и Xcode Interface Builder взаимозаменяемыми, однако Xcode Interface Builder доступны только на компьютерах Mac. Чтобы открыть файл Storyboard или XIB в Xcode Interface Builder на компьютере Mac, откройте решение, содержащее проект Xamarin. iOS в [Visual Studio для Mac](/visualstudio/mac/), щелкните правой кнопкой мыши файл и выберите **открыть с помощью > Xcode Interface Builder**, как показано на снимке экрана ниже:

[![Открытие раскадровки в Xcode Interface Builder](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "Открытие раскадровки в Xcode Interface Builder")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png#lightbox)

После внесения изменений в Xcode Interface Builder сохраните файл и вернитесь в Visual Studio для Mac. Изменения будут синхронизированы с проектом Xamarin. iOS.

-----

## <a name="xib-support"></a>. XIB, поддержка

Конструктор iOS поддерживает создание, изменение и управление файлами XIB. Это XML-файлы, представлять замкнутую отдельные пользовательские представления, которые можно добавить в иерархию представления приложения. XIB-файл обычно представляет интерфейс для одного представления или экрана в приложении, в то время как Раскадровка представляет множество экранов и переходы между ними.

Существует множество мнений о том, какое решение — файлы XIB, раскадровки или код — лучше всего подходит для создания и обслуживания пользовательского интерфейса. На самом деле нет идеального решения, и всегда стоит рассмотреть лучшее средство для работы. С другой стороны, файлы. XIB, как правило, являются наиболее мощными при использовании для представления пользовательского представления, необходимого в нескольких местах в приложении, например в пользовательской ячейке табличного представления. 

Дополнительную документацию по использованию файлов. XIB можно найти в следующих рецептах:

- [Использование шаблона View. XIB](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/using_the_ios_view_xib_template)
- [Создание пользовательского Таблевиевцелл с помощью. XIB](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/tables/custom-tableviewcell)
- [Создание экрана запуска с помощью. XIB](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/launchscreen-xib)

Дополнительные сведения об использовании раскадровки см. в статье [Введение в раскадровки](~/ios/user-interface/storyboards/index.md).

Это и другие руководства, связанные с конструктором iOS, относятся к использованию раскадровок в качестве стандартного подхода к созданию пользовательских интерфейсов, так как большинство новых шаблонов проектов Xamarin. iOS по умолчанию предоставляют раскадровку.

## <a name="summary"></a>Итоги

В этом учебнике приводятся общие сведения о конструкторе iOS, описание его возможностей и структурировании средств, предоставляемых ИТ для проектирования привлекательных пользовательских интерфейсов.

## <a name="related-links"></a>Связанные ссылки

- [Введение в раскадровку](~/ios/user-interface/storyboards/index.md)
- [Пошаговое руководство по разработке элементов управления для iOS](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Привет, iOS](~/ios/get-started/hello-ios/index.md)
- [Привет, iOS (несколько экранов)](~/ios/get-started/hello-ios-multiscreen/index.md)
- [Обзор Android Designer](~/android/user-interface/android-designer/index.md)
- [Разделяемые классы и методы](/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)
- [Погружение в Xamarin Designer для iOS — Развивайте 2014 (видео)](https://www.youtube.com/watch?v=W4H9uLjoEjM)
- [Использование конструктора iOS для создания экрана запуска (видео)](https://university.xamarin.com/lightninglectures/using-the-ios-designer-to-create-a-launch-screen)
