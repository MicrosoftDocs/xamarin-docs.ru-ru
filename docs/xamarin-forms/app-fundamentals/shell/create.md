---
title: Создание приложения оболочки Xamarin.Forms
description: Чтобы создать приложение оболочки Xamarin.Forms, нужно создать XAML-файл, в котором, в свою очередь, создается подкласс Shell, задаются свойства MainPage класса App приложения для подкласса объекта Shell и описывается визуальная иерархия приложения в подклассе Shell.
ms.prod: xamarin
ms.assetid: 2A51D78F-6CD5-4BC4-A62E-11CEFA799987
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/24/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 62c323dd73f656d5b4f4e06c672fd952845efb22
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137804"
---
# <a name="create-a-xamarinforms-shell-application"></a>Создание приложения оболочки Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Чтобы создать приложение оболочки Xamarin.Forms, сделайте следующее:

1. Создайте приложение Xamarin.Forms или загрузите существующее приложение, которое вы можете преобразовать в приложение оболочки.
1. В проект с общим кодом добавьте XAML-файл, в котором создается производный класс `Shell`. Дополнительные сведения см. в разделе [Создание производного класса оболочки](#subclass-the-shell-class).
1. Задайте свойство [`MainPage`](xref:Xamarin.Forms.Application.MainPage) классу `App` приложения для подкласса объекта `Shell`. Дополнительные сведения см. в разделе [Начальная загрузка приложения оболочки](#bootstrap-the-shell-application).
1. Опишите визуальную иерархию приложения в подклассе `Shell`. Дополнительные сведения см. в разделе [Описание визуальной иерархии приложения](#describe-the-visual-hierarchy-of-the-application).

## <a name="subclass-the-shell-class"></a>Создание производного класса оболочки

Чтобы создать приложение оболочки Xamarin.Forms, нужно сначала добавить XAML-файл в проект с общим кодом, который создает производный класс `Shell`. Файл можно назвать произвольно, но рекомендуется использовать имя **AppShell**. В следующем примере кода показан только что созданный файл **AppShell.xaml**:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       x:Class="Xaminals.AppShell">

</Shell>
```

В следующем примере показан файл кода программной части **AppShell.xaml.cs**:

```csharp
using Xamarin.Forms;

namespace Xaminals
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

После создания XAML-файла, в котором создается подкласс объекта `Shell`, необходимо задать свойство [`MainPage`](xref:Xamarin.Forms.Application.MainPage) класса `App` для подкласса объекта `Shell`:

```csharp
namespace Xaminals
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

В этом примере класс `AppShell` — это XAML-файл, который является производным от класса `Shell`.

> [!NOTE]
> Итак, мы создали пустое приложение оболочки. Если его запустить, будет возвращено `InvalidOperationException`.

## <a name="describe-the-visual-hierarchy-of-the-application"></a>Описания визуальной иерархии приложения

Последним шагом при создании приложения оболочки Xamarin.Forms является описание визуальной иерархии приложения в подклассе `Shell`. Производный класс `Shell` состоит из трех основных объектов иерархии:

- `FlyoutItem` или `TabBar`. Объект `FlyoutItem` представляет один или несколько элементов всплывающего меню. Он требуется, если шаблон навигации предусматривает использование всплывающего меню. Объект `TabBar` представляет нижнюю панель вкладок. Он требуется, если навигация в приложении начинается с нижней панели вкладок. Каждый объект `FlyoutItem` или `TabBar` является дочерним для объекта `Shell`.
- `Tab` представляет сгруппированное содержимое с навигацией по нижним вкладкам. Каждый объект `Tab` является дочерним для объекта `FlyoutItem` или `TabBar`.
- `ShellContent` представляет объекты `ContentPage` в приложении. Каждый объект `ShellContent` является дочерним для объекта `Tab`. Если `Tab` содержит более одного объекта `ShellContent`, перемещение по ним осуществляется с помощью верхней панели вкладок.

Ни один из этих объектов не представляет какую-либо часть пользовательского интерфейса, они служат лишь для организации визуальной структуры приложения. На основе этих объектов оболочка создает пользовательский интерфейс навигации по содержимому.

Ниже представлен простой пример XAML для производного класса `Shell`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    ...
    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png">
                <views:CatsPage />
            </ShellContent>
            <ShellContent Title="Dogs"
                          Icon="dog.png">
                <views:DogsPage />
            </ShellContent>
        </Tab>
        <ShellContent Title="Monkeys"
                      Icon="monkey.png">
            <views:MonkeysPage />
        </ShellContent>
        <ShellContent Title="Elephants"
                      Icon="elephant.png">  
            <views:ElephantsPage />
        </ShellContent>
        <ShellContent Title="Bears"
                      Icon="bear.png">
            <views:BearsPage />
        </ShellContent>
    </FlyoutItem>
    ...
</Shell>
```

Этот XAML при выполнении отображает `CatsPage`, то есть первый элемент содержимого, объявленный в производном классе `Shell`:

[![Снимок экрана приложения оболочки для iOS и Android](create-images/cats.png "Приложение оболочки")](create-images/cats-large.png#lightbox "Приложение оболочки")

Если нажать значок из трех полосок или провести пальцем от левого края, отобразится всплывающий элемент:

[![Снимок экрана: всплывающий элемент оболочки в iOS и Android](create-images/flyout-reduced.png "Всплывающий элемент оболочки")](create-images/flyout-reduced-large.png#lightbox "Всплывающий элемент оболочки")

> [!IMPORTANT]
> В приложении оболочки каждый [`ContentPage`](xref:Xamarin.Forms.ContentPage), который является дочерним элементом объекта `ShellContent`, создается во время запуска приложения. Такой метод добавления объектов `ShellContent` приводит к созданию дополнительных страниц во время запуска приложения, что может замедлять запуск. Но оболочка может создавать страницы и по требованию, реагируя на переходы. Дополнительные сведения см. в разделе об [эффективности загрузки страниц](tabs.md#efficient-page-loading) в руководстве по [вкладкам оболочки Xamarin.Forms](tabs.md).

## <a name="related-links"></a>Связанные ссылки

- [Xaminals (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
