---
title: Создание приложения оболочки Xamarin.Forms
description: Чтобы создать приложение оболочки Xamarin.Forms, нужно создать XAML-файл, в котором, в свою очередь, создается подкласс Shell, задаются свойства MainPage класса App приложения для подкласса объекта Shell и описывается визуальная иерархия приложения в подклассе Shell.
ms.prod: xamarin
ms.assetid: 2A51D78F-6CD5-4BC4-A62E-11CEFA799987
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/15/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f02deecbd372e7125fe13aaed37ad08f2cc4faf6
ms.sourcegitcommit: 1b542afc0f6f2f6adbced527ae47b9ac90eaa1de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101757626"
---
# <a name="create-a-xamarinforms-shell-application"></a>Создание приложения оболочки Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Чтобы создать приложение оболочки Xamarin.Forms, сделайте следующее:

1. Создайте приложение Xamarin.Forms или загрузите существующее приложение, которое вы можете преобразовать в приложение оболочки.
1. В проект с общим кодом добавьте XAML-файл, в котором создается производный класс [`Shell`](xref:Xamarin.Forms.Shell). Дополнительные сведения см. в разделе [Создание производного класса оболочки](#subclass-the-shell-class).
1. Задайте свойство [`MainPage`](xref:Xamarin.Forms.Application.MainPage) классу `App` приложения для подкласса объекта [`Shell`](xref:Xamarin.Forms.Shell). Дополнительные сведения см. в разделе [Начальная загрузка приложения оболочки](#bootstrap-the-shell-application).
1. Опишите визуальную иерархию приложения в подклассе [`Shell`](xref:Xamarin.Forms.Shell). Дополнительные сведения см. в разделе [Описание визуальной иерархии приложения](#describe-the-visual-hierarchy-of-the-application).

Пошаговое руководство по созданию приложения оболочки см. в разделе [Краткое руководство по созданию приложения Xamarin.Forms](~/get-started/quickstarts/app.md).

## <a name="subclass-the-shell-class"></a>Создание производного класса оболочки

Чтобы создать приложение оболочки Xamarin.Forms, нужно сначала добавить XAML-файл в проект с общим кодом, который создает производный класс [`Shell`](xref:Xamarin.Forms.Shell). Файл можно назвать произвольно, но рекомендуется использовать имя **AppShell**. В следующем примере кода показан только что созданный файл **AppShell.xaml**:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       x:Class="MyApp.AppShell">

</Shell>
```

В следующем примере показан файл кода программной части **AppShell.xaml.cs**:

```csharp
using Xamarin.Forms;

namespace MyApp
{
    public partial class AppShell : Shell
    {
        public AppShell()
        {
            InitializeComponent();
        }
    }
}
```

## <a name="bootstrap-the-shell-application"></a>Начальная загрузка приложения оболочки

После создания XAML-файла, в котором создается подкласс объекта [`Shell`](xref:Xamarin.Forms.Shell), необходимо задать свойство [`MainPage`](xref:Xamarin.Forms.Application.MainPage) класса `App` для подкласса объекта `Shell`:

```csharp
namespace MyApp
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new AppShell();
        }
        ...
    }
}
```

В этом примере класс `AppShell` — это XAML-файл, который является производным от класса [`Shell`](xref:Xamarin.Forms.Shell).

> [!WARNING]
> Будет создано пустое приложение оболочки. Попытка запустить его приведет к созданию исключения `InvalidOperationException`.

## <a name="describe-the-visual-hierarchy-of-the-application"></a>Описания визуальной иерархии приложения

Последним шагом при создании приложения оболочки Xamarin.Forms является описание визуальной иерархии приложения в подклассе [`Shell`](xref:Xamarin.Forms.Shell). Производный класс `Shell` состоит из трех основных объектов иерархии:

1. [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) или [`TabBar`](xref:Xamarin.Forms.TabBar). Объект `FlyoutItem` представляет один или несколько элементов всплывающего меню. Он требуется, если шаблон навигации требует использования всплывающего меню. Объект `TabBar` представляет нижнюю панель вкладок. Он требуется, если навигация в приложении начинается с нижней панели вкладок (когда всплывающее меню не требуется). Каждый объект `FlyoutItem` или `TabBar` является дочерним для объекта [`Shell`](xref:Xamarin.Forms.Shell).
1. [`Tab`](xref:Xamarin.Forms.Tab) представляет сгруппированное содержимое с навигацией по нижним вкладкам. Каждый объект `Tab` является дочерним для объекта [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) или [`TabBar`](xref:Xamarin.Forms.TabBar).
1. Объект [`ShellContent`](xref:Xamarin.Forms.ShellContent), представляющий объекты [`ContentPage`](xref:Xamarin.Forms.ContentPage) для каждой вкладки. Каждый объект `ShellContent` является дочерним для объекта [`Tab`](xref:Xamarin.Forms.Tab). Если `Tab` содержит более одного объекта `ShellContent`, перемещение по ним осуществляется с помощью верхней панели вкладок.

Эти объекты не представляют собой какие-либо элементы пользовательского интерфейса, они служат лишь для организации визуальной структуры приложения. На основе этих объектов оболочка создает пользовательский интерфейс навигации по содержимому.

Ниже представлен простой пример XAML для производного класса [`Shell`](xref:Xamarin.Forms.Shell):

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    ...
    <FlyoutItem FlyoutDisplayOptions="AsMultipleItems">
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png"
                          ContentTemplate="{DataTemplate views:CatsPage}" />
            <ShellContent Title="Dogs"
                          Icon="dog.png"
                          ContentTemplate="{DataTemplate views:DogsPage}" />
        </Tab>
        <!--
        Shell has implicit conversion operators that enable the Shell visual hierarchy to be simplified.
        This is possible because a subclassed Shell object can only ever contain a FlyoutItem object or a TabBar object,
        which can only ever contain Tab objects, which can only ever contain ShellContent objects.

        The implicit conversion automatically wraps the ShellContent objects below in Tab objects.
        -->
        <ShellContent Title="Monkeys"
                      Icon="monkey.png"
                      ContentTemplate="{DataTemplate views:MonkeysPage}" />
        <ShellContent Title="Elephants"
                      Icon="elephant.png"
                      ContentTemplate="{DataTemplate views:ElephantsPage}" />
        <ShellContent Title="Bears"
                      Icon="bear.png"
                      ContentTemplate="{DataTemplate views:BearsPage}" />
    </FlyoutItem>
    ...
</Shell>
```

Этот XAML при выполнении отображает `CatsPage`, то есть первый элемент содержимого, объявленный в производном классе [`Shell`](xref:Xamarin.Forms.Shell):

[![Снимок экрана приложения оболочки для iOS и Android](create-images/cats.png)](create-images/cats-large.png#lightbox)

Если нажать значок из трех полосок или провести пальцем от левого края, отобразится всплывающий элемент:

[![Снимок экрана: всплывающий элемент оболочки в iOS и Android](create-images/flyout.png)](create-images/flyout-large.png#lightbox)

Во всплывающем окне отображается несколько элементов, так как для свойства [`FlyoutDisplayOptions`](xref:Xamarin.Forms.ShellGroupItem.FlyoutDisplayOptions) задано значение `AsMultipleItems`. Дополнительные сведения см. в разделе [Параметры всплывающего меню](flyout.md#flyout-display-options).

> [!IMPORTANT]
> В приложении оболочки страницы создаются по запросу в ответ на навигацию. Это достигается с помощью расширения разметки [`DataTemplate`](xref:Xamarin.Forms.Xaml.DataTemplateExtension) для задания свойства [`ContentTemplate`](xref:Xamarin.Forms.ShellContent.ContentTemplate) каждого объекта [`ShellContent`](xref:Xamarin.Forms.ShellContent) в соответствии с объектом [`ContentPage`](xref:Xamarin.Forms.ContentPage).

## <a name="related-links"></a>Связанные ссылки

- [Xaminals (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Краткое руководство по созданию приложения Xamarin.Forms](~/get-started/quickstarts/app.md)
