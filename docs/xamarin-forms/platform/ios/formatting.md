---
title: Добавление форматирования, относящегося к iOS
description: В этой статье объясняется, как задать внешний вид iOS без использования Xamarin.Forms пользовательского модуля подготовки отчетов.
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 062dd77f1dc7b82f16bfbc6d46e44d4adeb73350
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84564710"
---
# <a name="adding-ios-specific-formatting"></a>Добавление форматирования, относящегося к iOS

Одним из способов настройки форматирования для iOS является создание [пользовательского модуля подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) отчетов для элемента управления и установка стилей и цветов для каждой платформы.

Другие параметры для управления Xamarin.Forms внешним видом приложения iOS:

- Настройка параметров вывода в [ **info. plist**](#customizing-infoplist)
- Установка стилей элементов управления через [ `UIAppearance` API](#uiappearance-api)

Эти альтернативы обсуждаются ниже.

## <a name="customizing-infoplist"></a>Настройка info. plist

Файл **info. plist** позволяет настроить некоторые аспекты Рендереринг приложения iOS, например способ отображения строки состояния (и состояние).

Например, в [примере TODO](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo) используется следующий код для установки цвета и цвета текста панели навигации на всех платформах:

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

Результат показан в следующем фрагменте экрана. Обратите внимание, что элементы строки состояния являются черными (не могут быть заданы в пределах, Xamarin.Forms так как это функция для конкретной платформы).

![](theme-images/status-default-sml.png "iOS Theming")

В идеале строка состояния также будет содержать белый вид, что можно выполнить непосредственно в проекте iOS. Добавьте следующие записи в файле **info. plist** , чтобы строка состояния была белыми:

![](theme-images/info-plist.png "iOS Info.plist Entries")

или измените соответствующий файл **info. plist** непосредственно для включения:

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

Теперь при запуске приложения панель навигации отображается зеленым цветом, а ее текст — белым (из-за Xamarin.Forms форматирования) *, а* текст строки состояния также является белым благодаря конфигурации iOS:

![](theme-images/status-white-sml.png "iOS Theming")

## <a name="uiappearance-api"></a>API Уиаппеаранце

[ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) можно использовать для установки свойств визуального элемента во многих элементах управления iOS *без* необходимости создания [пользовательского модуля подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)отчетов.

Добавление одной строки кода в метод **AppDelegate.CS** `FinishedLaunching` может иметь стиль для всех элементов управления заданного типа, используя их `Appearance` свойство. Следующий код содержит два примера: глобально стилизацию строки табуляции и переключения управления.

**AppDelegate.CS** в проекте iOS

```csharp
public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
  // tab bar
    UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
  // switch
    UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
    // required Xamarin.Forms code
    Forms.Init ();
    LoadApplication (new App ());
    return base.FinishedLaunching (app, options);
}
```

### <a name="uitabbar"></a>уитаббар

По умолчанию выбранный значок панели вкладок в элементе[`TabbedPage`](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
будет синим:

![](theme-images/tabbar-default.png "Default iOS Tab Bar Icon in TabbedPage")

Чтобы изменить это поведение, задайте `UITabBar.Appearance` свойство:

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

В результате выбранная вкладка будет зеленым:

![](theme-images/tabbar-custom.png "Green iOS Tab Bar Icon in TabbedPage")

Использование этого API позволяет настроить внешний вид элементаXamarin.Forms
`TabbedPage`в iOS с очень небольшим кодом. Дополнительные сведения об использовании пользовательского модуля подготовки отчетов для установки определенного шрифта для вкладки см. в описании инструкции по [настройке вкладок](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs) .

### <a name="uiswitch"></a>UISwitch

`Switch`Элемент управления — это еще один пример, который можно легко присвоить стилю:

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Эти два снимка экрана показывают элемент управления по умолчанию `UISwitch` слева и настроенную версию (параметр `Appearance` ) справа в [примере TODO](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo):

![](theme-images/switch-default.png "Цвет Уисвитч по умолчанию") ![](theme-images/switch-custom.png "Настроенный цвет Уисвитч")

### <a name="other-controls"></a>Другие элементы управления

Многие элементы управления пользовательского интерфейса iOS могут иметь цвета по умолчанию и другие атрибуты, заданные с помощью [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md).

## <a name="related-links"></a>Связанные ссылки

- [уиаппеаранце](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [Настройка вкладок](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs)
