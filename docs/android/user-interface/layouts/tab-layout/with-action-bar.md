---
title: Макеты с вкладками с помощью Актионбар
description: В этом руководство рассказывается о том, как использовать API Актионбар для создания пользовательского интерфейса с вкладками в приложении Xamarin. Android.
ms.prod: xamarin
ms.assetid: B7E60AAF-BDA5-4305-9000-675F0438734D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: b1e6fadb52f6009e28d18143ac24c5180dae037c
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457046"
---
# <a name="tabbed-layouts-with-the-actionbar"></a>Макеты с вкладками с помощью Актионбар

_В этом руководство рассказывается о том, как использовать API Актионбар для создания пользовательского интерфейса с вкладками в приложении Xamarin. Android._

## <a name="overview"></a>Обзор

Панель действий — это шаблон пользовательского интерфейса Android, который используется для обеспечения согласованного пользовательского интерфейса для таких ключевых функций, как вкладки, удостоверение приложения, меню и поиск. В Android 3,0 (API уровня 11) в Google появились API Актионбар для платформы Android. Интерфейсы API Актионбар содержат темы пользовательского интерфейса, обеспечивающие единообразный внешний вид и классы, позволяющие использовать пользовательские интерфейсы с вкладками. В этом руководство объясняется, как добавить вкладки панели действий в приложение Xamarin. Android. Здесь также обсуждается использование библиотеки поддержки Android версии 7 для неактионбарных вкладок в приложениях Xamarin. Android, предназначенных для Android 2,1, на Android 2,3. 

Обратите внимание, что `Toolbar` является более новым и обобщенным компонентом панели операций, который следует использовать вместо `ActionBar` ( `Toolbar` был разработан для замены `ActionBar` ). Дополнительные сведения см. в разделе [панель инструментов](~/android/user-interface/controls/tool-bar/index.md). 

## <a name="requirements"></a>Требования

Любое приложение Xamarin. Android, предназначенное для API уровня 11 (Android 3,0) или более поздней версии, имеет доступ к интерфейсам API Актионбар в рамках собственных интерфейсов API Android. 

Некоторые API Актионбар были перенесены на уровень API 7 (Android 2,1) и доступны через [библиотеку AppCompat версии 7](https://developer.android.com/tools/support-library/features.html#v7-appcompat), которая доступна для приложений Xamarin. Android через [библиотеку поддержки Xamarin Android Library-версии 7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) .

## <a name="introducing-tabs-in-the-actionbar"></a>Знакомство с вкладками в Актионбар

Панель действий пытается отобразить все вкладки одновременно и сделать все вкладки размером в зависимости от ширины самой широкой метки табуляции. Это показано на следующем снимке экрана: 

![Пример снимка экрана Актионбар со всеми вкладками одинакового размера](with-action-bar-images/image1.png)

Если Актионбар не может отобразить все вкладки, он настроит вкладки в представлении с горизонтальной прокруткой. Пользователь может прокрутить влево или вправо для просмотра оставшихся вкладок. На этом снимке экрана из Google Play показан пример. 

![Пример снимка экрана вкладок в прокручиваемом представлении с горизонтальным прокруткой](with-action-bar-images/image2.png)

Каждая вкладка на панели действий должна быть связана с [*фрагментом*](~/android/platform/fragments/index.md). Когда пользователь выбирает вкладку, в приложении отображается фрагмент, связанный с вкладкой. Актионбар не несет ответственности за отображение соответствующего фрагмента для пользователя. Вместо этого Актионбар будет уведомлять приложение об изменениях состояния на вкладке через класс, реализующий интерфейс Актионбар. Итаблистенер. Этот интерфейс предоставляет три метода обратного вызова, которые будут вызываться Android при изменении состояния вкладки: 

- **Онтабселектед** — этот метод вызывается, когда пользователь выбирает вкладку. Он должен отображать фрагмент.

- **Онтабреселектед** — этот метод вызывается, когда вкладка уже выбрана, но снова выбирается пользователем. Этот обратный вызов обычно используется для обновления и обновления отображаемого фрагмента.

- **Онтабунселектед** — этот метод вызывается, когда пользователь выбирает другую вкладку. Этот обратный вызов используется для сохранения состояния в отображаемом фрагменте до его исчезновения.

Xamarin. Android заключает в оболочку `ActionBar.ITabListener` события с `ActionBar.Tab` классом. Приложения могут назначать обработчики событий одному или нескольким из этих событий. Существует три события (по одному для каждого метода в `ActionBar.ITabListener` ), которые будет вызывать вкладка панели действий: 

- табселектед
- табреселектед
- табунселектед

### <a name="adding-tabs-to-the-actionbar"></a>Добавление вкладок в Актионбар

Актионбар является собственным интерфейсом Android 3,0 (API уровня 11) и более поздней версии и доступен для любого приложения Xamarin. Android, которое нацелено на этот API как минимум. 

Ниже показано, как добавить Актионбар вкладки в действие Android. 

1. В `OnCreate` методе действия &ndash; *перед инициализацией любых мини-приложений пользовательского интерфейса* &ndash; приложение должно установить в значение, `NavigationMode` `ActionBar` `ActionBar.NavigationModeTabs` как показано в следующем фрагменте кода:

   ```csharp
   ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
   SetContentView(Resource.Layout.Main);
   ```

2. Создайте новую вкладку с помощью `ActionBar.NewTab()` .

3. Назначьте обработчики событий или предоставьте пользовательскую `ActionBar.ITabListener` реализацию, которая будет отвечать на события, возникающие, когда пользователь взаимодействует со вкладками актионбар.

4. Добавьте вкладку, созданную на предыдущем шаге, в `ActionBar` .

Следующий код является одним из примеров использования этих шагов для добавления вкладок в приложение, которое использует обработчики событий для реагирования на изменения состояния: 

```csharp
protected override void OnCreate(Bundle bundle)
{
    ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
    SetContentView(Resource.Layout.Main);

    ActionBar.Tab tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab1_text));
    tab.SetIcon(Resource.Drawable.tab1_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       };
    ActionBar.AddTab(tab);

    tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab2_text));
    tab.SetIcon(Resource.Drawable.tab2_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       };
    ActionBar.AddTab(tab);
}
```

#### <a name="event-handlers-vs-actionbaritablistener"></a>Обработчики событий VS Актионбар. Итаблистенер

Приложения должны использовать обработчики событий и `ActionBar.ITabListener` для различных сценариев. Обработчики событий предлагают определенный объем синтаксического удобства; они сохраняют у вас возможность создавать и реализовывать класс `ActionBar.ITabListener` . Это удобство достигается за счет &ndash; Xamarin. Android выполняет это преобразование, создавая один класс и реализуя `ActionBar.ITabListener` его. Это нормально, если в приложении имеется ограниченное количество вкладок. 

При работе со многими вкладками или совместном использовании общих функциональных возможностей на вкладках Актионбар может быть более эффективным в плане памяти и производительности для создания пользовательского класса, реализующего интерфейс `ActionBar.ITabListener` и совместно использующего один экземпляр класса. Это позволит сократить число Греф, которые использует приложение Xamarin. Android. 

### <a name="backwards-compatibility-for-older-devices"></a>Обратная совместимость для старых устройств

[Библиотека поддержки Android версии 7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) порты Back Ports Актионбар на Android 2,1 (API уровня 7). Вкладки доступны в приложении Xamarin. Android после добавления этого компонента в проект.

Чтобы использовать Актионбар, действие должно создать подкласс `ActionBarActivity` и использовать тему AppCompat, как показано в следующем фрагменте кода:

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity: ActionBarActivity
```

Действие может получить ссылку на его Актионбар из `ActionBarActivity.SupportingActionBar` Свойства. В следующем фрагменте кода показан пример настройки Актионбар в действии:

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity : ActionBarActivity, ActionBar.ITabListener
{
    static readonly string Tag = "ActionBarTabsSupport";

    public void OnTabReselected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Optionally refresh/update the displayed tab.
        Log.Debug(Tag, "The tab {0} was re-selected.", tab.Text);
    }

    public void OnTabSelected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Display the fragment the user should see
        Log.Debug(Tag, "The tab {0} has been selected.", tab.Text);
    }

    public void OnTabUnselected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Save any state in the displayed fragment.
        Log.Debug(Tag, "The tab {0} as been unselected.", tab.Text);
    }

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SupportActionBar.NavigationMode = ActionBar.NavigationModeTabs;
        SetContentView(Resource.Layout.Main);
    }

    void AddTabToActionBar(int labelResourceId, int iconResourceId)
    {
        ActionBar.Tab tab = SupportActionBar.NewTab()
                                            .SetText(labelResourceId)
                                            .SetIcon(iconResourceId)
                                            .SetTabListener(this);
        SupportActionBar.AddTab(tab);
    }
}
```

## <a name="summary"></a>Сводка

В этом руководство мы рассмотрели создание пользовательского интерфейса с вкладками в Xamarin. Android с помощью Актионбар. Мы рассмотрели, как добавлять вкладки в Актионбар и как действие может взаимодействовать с событиями табуляции через `ActionBar.ITabListener` интерфейс. Кроме того, мы увидели, как в библиотеке поддержки Android пакет AppCompat версии 7 поддерживает вкладки Актионбар с более старыми версиями Android. 

## <a name="related-links"></a>Связанные ссылки

- [Актионбартабс (пример)](/samples/xamarin/monodroid-samples/userinterface-actionbartabs)
- [Панель инструментов](~/android/user-interface/controls/tool-bar/index.md)
- [Фрагменты](~/android/platform/fragments/index.md)
- [ActionBar](https://developer.android.com/guide/topics/ui/actionbar.html)
- [актионбарактивити](https://developer.android.com/reference/android/support/v7/app/ActionBarActivity.html)
- [Шаблон панели действий](https://developer.android.com/design/patterns/actionbar.html)
- [Android версии 7 AppCompat](https://developer.android.com/tools/support-library/features.html#v7-appcompat)
- [Пакет NuGet библиотеки поддержки Xamarin. Android версии 7 для AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)