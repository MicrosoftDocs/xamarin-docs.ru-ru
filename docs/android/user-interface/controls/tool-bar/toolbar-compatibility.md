---
title: Совместимость панели инструментов
ms.prod: xamarin
ms.assetid: A0798CA1-2C7D-43B6-9E91-4435CC7B6683
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 8d5f5ff1cfe7876862371a9732f0ab8186bbeeba
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457640"
---
# <a name="toolbar-compatibility"></a>Совместимость панели инструментов

## <a name="overview"></a>Обзор

В этом разделе объясняется, как использовать `Toolbar` в версиях Android более ранние, чем android 5,0 без описания операций. Если приложение не поддерживает версии Android, предшествующие Android 5,0, этот раздел можно пропустить. 

Так как `Toolbar` является частью библиотеки поддержки Android версии 7, ее можно использовать на устройствах под управлением android 2,1 (API уровня 7) и более поздних версий. Однако [Библиотека поддержки Android версии 7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet должна быть установлена, а код изменен так, чтобы использовать `Toolbar` реализацию, предоставленную в этой библиотеке. В этом разделе объясняется, как установить этот NuGet и изменить приложение **тулбарфун** , [добавив вторую панель инструментов](~/android/user-interface/controls/tool-bar/adding-a-second-toolbar.md) , чтобы она выполнялась в версиях Android раньше, чем с параметром без описания 5,0.

Чтобы изменить приложение для использования версии на панели инструментов AppCompat, выполните следующие действия. 

1. Задайте минимальные и целевые версии Android для приложения.

2. Установите пакет NuGet для AppCompat.

3. Используйте тему AppCompat вместо встроенной темы Android.

4. Измените `MainActivity` таким образом, чтобы он подклассы, `AppCompatActivity` а не `Activity` . 

Каждый из этих шагов подробно описан в следующих разделах.

## <a name="set-the-minimum-and-target-android-version"></a>Установка минимальной и целевой версий Android

Целевая платформа приложения должна быть установлена на уровне API 21 или выше, иначе приложение не будет развернуто должным образом. Если при развертывании приложения обнаружена ошибка, например, **идентификатор ресурса не найден для атрибута "тилемодекс" в пакете "Android"** , это связано с тем, что для целевой платформы не задано значение **Android 5,0 (API уровня 21-интерфейса)** или больше. 

Задайте для целевого уровня платформы уровень API 21 или выше и задайте для параметров проекта уровня API Android минимальную версию Android, которую будет поддерживать приложение. Дополнительные сведения о настройке уровней API Android см. в разделе Общие сведения об [уровнях API Android](~/android/app-fundamentals/android-api-levels.md). В этом `ToolbarFun` примере для минимальной версии Android задано значение KitKat (API уровня 4,4). 

## <a name="install-the-appcompat-nuget-package"></a>Установка пакета AppCompat NuGet

Затем добавьте в проект пакет [AppCompat версии 7 для библиотеки поддержки Android](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) . В Visual Studio щелкните правой кнопкой мыши **ссылки** и выберите пункт **Управление пакетами NuGet...**. Нажмите кнопку **Обзор** и найдите **библиотеку поддержки Android версии 7 AppCompat**. Выберите **Xamarin. Android. support. версии 7. AppCompat** и нажмите кнопку **установить**. 

[![Снимок экрана пакета версии 7 AppCompat, выбранного в окне "Управление пакетами NuGet"](toolbar-compatibility-images/01-appcompat-nuget-sml.png)](toolbar-compatibility-images/01-appcompat-nuget.png#lightbox)

При установке этого NuGet также устанавливаются несколько других пакетов NuGet (например, **Xamarin. Android. support. анимированные. Vector. Рисующие**, Xamarin. Android **. support. v4**и **Xamarin. Android. support. Vector. Draw**). Дополнительные сведения об установке пакетов NuGet см. [в разделе Пошаговое руководство. Включение NuGet в проект](/visualstudio/mac/nuget-walkthrough). 

## <a name="use-an-appcompat-theme-and-toolbar"></a>Использование темы и панели инструментов AppCompat

Библиотека AppCompat поставляется с несколькими `Theme.AppCompat` темами, которые можно использовать в любой версии Android, поддерживаемой библиотекой AppCompat. `ToolbarFun`Тема примера приложения является производной от `Theme.Material.Light.DarkActionBar` , которая недоступна в версиях Android, предшествующих интерфейсу без описания операций. Поэтому `ToolbarFun` необходимо адаптироваться к использованию аналога AppCompat для этой темы `Theme.AppCompat.Light.DarkActionBar` . Кроме того, поскольку `Toolbar` недоступно в версиях Android, предшествующих интерфейсу без описания операций, необходимо использовать версию AppCompat `Toolbar` . Поэтому макеты должны использовать `android.support.v7.widget.Toolbar` вместо `Toolbar` . 

### <a name="update-layouts"></a>Обновление макетов

Измените **Resources/Layout/Main. axml** и замените `Toolbar` элемент следующим XML-кодом: 

```xml
<android.support.v7.widget.Toolbar
    android:id="@+id/edit_toolbar"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorAccent"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

Измените **Resources/Layout/toolbar.xml** и замените его содержимое следующим XML-кодом: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"/>
```

Обратите внимание, что `?attr` значения больше не начинаются с префикса `android:` (Помните, что `?` запись ссылается на ресурс в текущей теме). Если `?android:attr` все еще использовались здесь, Android будет ссылаться на значение атрибута из текущей платформы, а не из библиотеки AppCompat. Поскольку в этом примере используется объект, `actionBarSize` определенный библиотекой AppCompat, `android:` префикс удаляется. Аналогичным образом `@android:style` изменяется на, чтобы `@style` `android:theme` атрибут был задан в теме в библиотеке AppCompat, а &ndash; `ThemeOverlay.AppCompat.Dark.ActionBar` `ThemeOverlay.Material.Dark.ActionBar` не в. 

### <a name="update-the-style"></a>Обновление стиля

Измените **Resources/Values/styles.xml** и замените его содержимое следующим XML-кодом: 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="MyTheme" parent="MyTheme.Base"> </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="windowNoTitle">true</item>
    <item name="windowActionBar">false</item>
    <item name="colorPrimary">#5A8622</item>
    <item name="colorAccent">#A88F2D</item>
  </style>
</resources>
```

Имена элементов и родительские темы в этом примере больше не являются префиксами, `android:` так как мы используем библиотеку AppCompat. Кроме того, Родительская тема изменяется на версию AppCompat `Light.DarkActionBar` . 

### <a name="update-menus"></a>Обновить меню

Для поддержки более ранних версий Android библиотека AppCompat использует настраиваемые атрибуты, отражающие атрибуты `android:` пространства имен. Однако некоторые атрибуты (например, атрибут, `showAsAction` используемый в `<menu>` теге) не существуют в платформе Android на старых устройствах &ndash; `showAsAction` были введены в интерфейсе API Android 11, но недоступны в API Android 7. По этой причине для префикса всех атрибутов, определенных библиотекой поддержки, необходимо использовать пользовательское пространство имен. В файлах ресурсов меню `local` для префикса атрибута определено пространство имен с именем `showAsAction` . 

Измените **ресурсы/меню/top_menus.xml** и замените его содержимое следующим XML-кодом:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_edit"
       android:icon="@mipmap/ic_action_content_create"
       local:showAsAction="ifRoom"
       android:title="Edit" />
  <item
       android:id="@+id/menu_save"
       android:icon="@mipmap/ic_action_content_save"
       local:showAsAction="ifRoom"
       android:title="Save" />
  <item
       android:id="@+id/menu_preferences"
       local:showAsAction="never"
       android:title="Preferences" />
</menu>
```

`local`Пространство имен добавляется с этой строкой:

```xml
xmlns:local="http://schemas.android.com/apk/res-auto">
```

`showAsAction`Этот атрибут предшествует этому `local:` пространству имен, а не`android:` 

```csharp
local:showAsAction="ifRoom"
```

Аналогичным образом измените **Resources/Menu/edit_menus.xml** и замените его содержимое следующим XML-кодом:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_cut"
       android:icon="@mipmap/ic_menu_cut_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Cut" />
  <item
       android:id="@+id/menu_copy"
       android:icon="@mipmap/ic_menu_copy_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Copy" />
  <item
       android:id="@+id/menu_paste"
       android:icon="@mipmap/ic_menu_paste_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Paste" />
</menu>
```

Как этот переключатель пространства имен обеспечивает поддержку `showAsAction` атрибута в версиях Android до уровня API 11? Пользовательский атрибут `showAsAction` и все возможные значения включаются в приложение при установке AppCompat NuGet. 

## <a name="subclass-appcompatactivity"></a>Подкласс Аппкомпатактивити

Последним шагом в преобразовании является изменение `MainActivity` , чтобы он был подклассом `AppCompactActivity` . Измените **MainActivity.CS** и добавьте следующие `using` инструкции: 

```csharp
using Android.Support.V7.App;
using Toolbar = Android.Support.V7.Widget.Toolbar;
```

Это объявляется как `Toolbar` версия AppCompat `Toolbar` . Затем измените определение класса `MainActivity` : 

```csharp
public class MainActivity : AppCompatActivity
```

Чтобы задать панель действий для версии AppCompat `Toolbar` , замените вызов на `SetActionBar` `SetSupportActionBar` . В этом примере заголовок также изменяется, указывая, что используется версия AppCompat `Toolbar` :

```csharp
SetSupportActionBar (toolbar);
SupportActionBar.Title = "My AppCompat Toolbar";
```

Наконец, измените минимальный уровень Android на значение предварительной версии, которое должно поддерживаться (например, API 19). 

Выполните сборку приложения и запустите его на устройстве предварительной версии или эмуляторе Android. На следующем снимке экрана показана версия совместимости **тулбарфун** для хранилища 4 с KITKAT (API 19): 

[![Полный снимок экрана приложения, работающего на устройстве KitKat, отображаются обе панели инструментов](toolbar-compatibility-images/02-running-on-kitkat-sml.png)](toolbar-compatibility-images/02-running-on-kitkat.png#lightbox)

При использовании библиотеки AppCompat темы не нужно переключать на основе версии Android &ndash; . Библиотека AppCompat позволяет обеспечить согласованное взаимодействие с пользователем во всех поддерживаемых версиях Android. 

## <a name="related-links"></a>Связанные ссылки

- [Панель инструментов без описания операций (пример)](/samples/xamarin/monodroid-samples/android50-toolbar)
- [Панель инструментов AppCompat (пример)](/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)