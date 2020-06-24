---
title: Пользовательские отрисовщики оболочки Xamarin.Forms
description: Приложения оболочки Xamarin.Forms имеют широкие возможности для настройки через свойства и методы, которые предоставляются в классах оболочки. Но вы можете создать собственный отрисовщик оболочки, если доступных возможностей настройки для конкретной платформы недостаточно.
ms.prod: xamarin
ms.assetid: 3B1A6AE8-1D1E-4C34-B9AB-48F4444FEF32
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 321539f877a86804245d27a2d76d1edeb1abd1e9
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137791"
---
# <a name="xamarinforms-shell-custom-renderers"></a>Пользовательские отрисовщики оболочки Xamarin.Forms

Одним из преимуществ приложений оболочки Xamarin.Forms является то, что их внешний вид и поведение подробно настраиваются через свойства и методы, которые предоставляются в классах оболочки. Но вы можете создать собственный отрисовщик оболочки, если доступных возможностей настройки для конкретной платформы недостаточно. Пользовательский отрисовщик оболочки, как и в других системах, можно добавить только в один проект платформы, чтобы настроить для нее внешний вид и поведение, сохраняя поведение по умолчанию для другой платформы. Вы также можете добавить разные пользовательские отрисовщики оболочки в каждый проект платформы, чтобы раздельно настраивать внешний вид и поведение для iOS и Android.

Приложения оболочки отображаются в iOS и Android через класс `ShellRenderer`. В iOS этот класс `ShellRenderer` находится в пространстве имен `Xamarin.Forms.Platform.iOS`. В Android класс `ShellRenderer` находится в пространстве имен `Xamarin.Forms.Platform.Android`.

Процесс создания пользовательского отрисовщика оболочки выглядит следующим образом.

1. Создайте подкласс класса `Shell`. Обычно в приложении оболочки эта задача уже выполнена.
1. Используйте подкласс класса `Shell`. Обычно в приложении оболочки эта задача уже выполнена.
1. Создайте на нужной платформе класс пользовательского отрисовщика, наследуемый от класса `ShellRenderer`.

## <a name="create-a-custom-renderer-class"></a>Создание класса пользовательского отрисовщика

Процесс создания класса пользовательского отрисовщика оболочки выглядит следующим образом.

1. Создайте подкласс класса `ShellRenderer`.
1. Переопределите методы для нужного изменения в поведении.
1. Добавьте `ExportRendererAttribute` в подкласс `ShellRenderer`, чтобы указать, что он будет использоваться для отрисовки приложения оболочки. Этот атрибут используется для регистрации пользовательского отрисовщика в Xamarin.Forms.

> [!NOTE]
> Предоставлять пользовательский отрисовщик оболочки в проекте для каждой платформы необязательно. Если пользовательский отрисовщик незарегистрирован, используется класс `ShellRenderer` по умолчанию.

Класс `ShellRenderer` предоставляет следующие переопределяемые методы.

| iOS | Android |
| --- | --- |
| `SetElementSize`<br />`CreateFlyoutRenderer`<br />`CreateNavBarAppearanceTracker`<br />`CreatePageRendererTracker`<br />`CreateShellFlyoutContentRenderer`<br />`CreateShellItemRenderer`<br />`CreateShellItemTransition`<br />`CreateShellSearchResultsRenderer`<br />`CreateShellSectionRenderer`<br />`CreateTabBarAppearanceTracker`<br />`Dispose`<br />`OnCurrentItemChanged`<br />`OnElementPropertyChanged`<br />`OnElementSet`<br />`UpdateBackgroundColor` | `CreateFragmentForPage`<br />`CreateShellFlyoutContentRenderer`<br />`CreateShellFlyoutRenderer`<br />`CreateShellItemRenderer`<br />`CreateShellSectionRenderer`<br />`CreateTrackerForToolbar`<br />`CreateToolbarAppearanceTracker`<br />`CreateTabLayoutAppearanceTracker`<br />`CreateBottomNavViewAppearanceTracker`<br />`OnElementPropertyChanged`<br />`OnElementSet`<br />`SwitchFragment`<br />`Dispose` |

Классы `FlyoutItem` и `TabBar` являются псевдонимами для класса `ShellItem`, а класс `Tab` — для класса `ShellSection`. Таким образом, методы `CreateShellItemRenderer` и `CreateShellSectionRenderer` должны быть переопределены при создании пользовательского отрисовщика для объектов `FlyoutItem` и `Tab` соответственно.

> [!IMPORTANT]
> Для iOS и Android существуют дополнительные классы отрисовщика оболочки, такие как `ShellSectionRenderer` и `ShellItemRenderer`. Но эти дополнительные классы отрисовщика создаются путем переопределения класса `ShellRenderer`. Это означает, что любое поведение этих дополнительных классов отрисовщика можно переопределить, создавая для них подклассы и экземпляры этого подкласса в соответствующем переопределении производного класса `ShellRenderer`.

### <a name="ios-example"></a>Пример для iOS

Следующий пример кода демонстрирует производный класс `ShellRenderer` для iOS, который задает фоновое изображение для панели навигации приложения оболочки:

```csharp
using UIKit;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(Xaminals.AppShell), typeof(Xaminals.iOS.MyShellRenderer))]
namespace Xaminals.iOS
{
    public class MyShellRenderer : ShellRenderer
    {
        protected override IShellSectionRenderer CreateShellSectionRenderer(ShellSection shellSection)
        {
            var renderer = base.CreateShellSectionRenderer(shellSection);
            if (renderer != null)
            {
                (renderer as ShellSectionRenderer).NavigationBar.SetBackgroundImage(UIImage.FromFile("monkey.png"), UIBarMetrics.Default);
            }
            return renderer;
        }
    }
}
```

Класс `MyShellRenderer` переопределяет метод `CreateShellSectionRenderer` и извлекает отрисовщик, созданный в базовом классе. Затем он изменяет отрисовщик, задавая фоновое изображение для панели навигации, и возвращает измененный отрисовщик.

### <a name="android-example"></a>Пример для Android

Следующий пример кода демонстрирует производный класс `ShellRenderer` для Android, который задает фоновое изображение для панели навигации приложения оболочки:

```csharp
using Android.Content;
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(Xaminals.AppShell), typeof(Xaminals.Droid.MyShellRenderer))]
namespace Xaminals.Droid
{
    public class MyShellRenderer : ShellRenderer
    {
        public MyShellRenderer(Context context) : base(context)
        {
        }

        protected override IShellToolbarAppearanceTracker CreateToolbarAppearanceTracker()
        {
            return new MyShellToolbarAppearanceTracker(this);
        }
    }
}
```

Класс `MyShellRenderer` переопределяет метод `CreateToolbarAppearanceTracker` и возвращает экземпляр класса `MyShellToolbarAppearanceTracker`. Класс `MyShellToolbarAppearanceTracker`, который является производным от класса `ShellToolbarAppearanceTracker`, представлен в следующем примере:

```csharp
using Android.Support.V7.Widget;
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

namespace Xaminals.Droid
{
    public class MyShellToolbarAppearanceTracker : ShellToolbarAppearanceTracker
    {
        public MyShellToolbarAppearanceTracker(IShellContext context) : base(context)
        {
        }

        public override void SetAppearance(Toolbar toolbar, IShellToolbarTracker toolbarTracker, ShellAppearance appearance)
        {
            base.SetAppearance(toolbar, toolbarTracker, appearance);
            toolbar.SetBackgroundResource(Resource.Drawable.monkey);
        }
    }
}
```

Класс `MyShellToolbarAppearanceTracker` переопределяет метод `SetAppearance` и изменяет панель инструментов, задавая для нее фоновое изображение.

> [!IMPORTANT]
> Вам достаточно лишь добавить `ExportRendererAttribute` в пользовательский отрисовщик, наследуемый от класса `ShellRenderer`. Дополнительные производные классы отрисовщика оболочки создаются в подклассе класса `ShellRenderer`.

## <a name="related-links"></a>Связанные ссылки

- [Пользовательские отрисовщики Xamarin.Forms](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
