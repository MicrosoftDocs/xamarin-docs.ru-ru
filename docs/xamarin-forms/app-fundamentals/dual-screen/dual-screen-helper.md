---
title: Вспомогательные функции платформы для двух экранов Xamarin.Forms
description: В этом руководстве рассматривается использование класса DualScreenHelper из Xamarin.Forms для оптимизации интерфейса приложения на двухэкранных устройствах, таких как Surface Duo и Surface Neo.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d9daf5a24c0dcfd07d529955c411259f4c1359df
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138948"
---
# <a name="xamarinforms-dual-screen-platform-helpers"></a>Вспомогательные функции платформы для двух экранов Xamarin.Forms

![](~/media/shared/preview.png "This API is currently pre-release")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)

Класс `DualScreenHelper` позволяет определить, поддерживает ли устройство режим "картинка в картинке", и открыть `ContentPage` в окне, использующем такой режим. Если вы используете устройство Neo, то в режиме создания страница откроется на панели WonderBar.

## <a name="properties"></a>Свойства

- `HasCompactModeSupport` используется для проверки того, поддерживает ли платформа открытие `ContentPage` в режиме CompactMode.
- `OpenCompactMode` открывает `ContentPage` в режиме CompactMode, если поддерживается платформой.

## <a name="example"></a>Пример

```csharp
async void OpenCompactWindowClicked(object sender, EventArgs e)
{
    if (!DualScreenHelper.HasCompactModeSupport())
    {
        await DisplayAlert("Unsupported", "This platform doesn't support this feature", "Ok");
        return;
    }

    ContentPage page = new ContentPage() { BackgroundColor = Color.Purple };

    var button = new Button()
    {
        Text = "Close this Window"
    };

    var layout = new StackLayout()
    {
        Children =
        {
            new Label(){ Text = "Welcome to your Compact Mode Window" },
            button
        },
        BackgroundColor = Color.Yellow,
        HorizontalOptions = LayoutOptions.Fill,
        VerticalOptions = LayoutOptions.Fill
    };

    page.Content = new ScrollView() { Content = layout };
    var args = await DualScreenHelper.OpenCompactMode(page);

    button.Command = new Command(async () =>
    {
        await args.CloseAsync();
    });
}
```
