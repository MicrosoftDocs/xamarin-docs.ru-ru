---
title: Добавление AppCompat и дизайна материалов
description: В этой статье объясняется, как преобразовать существующие Xamarin.Forms приложения Android для использования AppCompat и дизайна материалов.
ms.prod: xamarin
ms.assetid: 045FBCDF-4D45-48BB-9911-BD3938C87D58
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 24206f6d6764c73f13a4b06fb44fa746f9d353af
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84135711"
---
# <a name="adding-appcompat-and-material-design"></a>Добавление AppCompat и дизайна материалов

_Выполните следующие действия, чтобы преобразовать существующие Xamarin.Forms приложения Android для использования AppCompat и дизайна материалов._

<!-- source https://gist.github.com/jassmith/a3b2a543f99126782936
https://blog.xamarin.com/material-design-for-your-xamarin-forms-android-apps/ -->

## <a name="overview"></a>Обзор

В этих инструкциях объясняется, как обновить существующие Xamarin.Forms приложения Android для использования библиотеки AppCompat и включить проектирование материалов в версии приложения для Android Xamarin.Forms .

### <a name="1-update-xamarinforms"></a>1. обновлениеXamarin.Forms

Убедитесь, что в решении используется Xamarin.Forms 2,0 или более поздней версии. ОбновитеXamarin.Forms
  Пакет NuGet для 2,0, если это необходимо.

### <a name="2-check-android-version"></a>2. Проверка версии Android

Убедитесь, что Целевая платформа проекта Android — Android 6,0 (Marshmallow). Проверьте **проект Android > параметры > сборки > общие** параметры, чтобы убедиться, что выбрана платформа Коррент:

 ![](appcompat-images/target-android-6-sml.png "Android General Build Configuration")

### <a name="3-add-new-themes-to-support-material-design"></a>3. Добавление новых тем для поддержки дизайна материалов

Создайте следующие три файла в проекте Android и вставьте содержимое ниже. Google предоставляет [рекомендации по стилю](https://www.google.com/design/spec/style/color.html#color-color-palette) и [генератор цветовой палитры](https://www.materialpalette.com/) , которые помогают выбрать альтернативную цветовую схему для указанной.

**Ресурсы/значения/colors.xml**

```xml
<resources>
  <color name="primary">#2196F3</color>
  <color name="primaryDark">#1976D2</color>
  <color name="accent">#FFC107</color>
  <color name="window_background">#F5F5F5</color>
</resources>
```

**Ресурсы/значения/style.xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
  </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.NoActionBar">
    <item name="colorPrimary">@color/primary</item>
    <item name="colorPrimaryDark">@color/primaryDark</item>
    <item name="colorAccent">@color/accent</item>
    <item name="android:windowBackground">@color/window_background</item>
    <item name="windowActionModeOverlay">true</item>
  </style>
</resources>
```

Дополнительный стиль должен быть включен в папку **Values-V21** для применения конкретных свойств при запуске в комплекте программного интерфейса Android и более поздних версиях.

**Resources/Values-V21/style.xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
    <!--If you are using MasterDetailPage you will want to set these, else you can leave them out-->
    <!--<item name="android:windowDrawsSystemBarBackgrounds">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>-->
  </style>
</resources>
```

### <a name="4-update-androidmanifestxml"></a>4. обновление AndroidManifest.xml

Чтобы обеспечить использование новых сведений о теме, задайте тему в файле **AndroidManifest** , добавив `android:theme="@style/MyTheme"` (ОСТАВЬТЕ остальной XML-файл).

**Свойства/AndroidManifest.xml**

```xml
...
<application android:label="AppName" android:icon="@drawable/icon"
  android:theme="@style/MyTheme">
...
```

### <a name="5-provide-toolbar-and-tab-layouts"></a>5. предоставление макетов панелей инструментов и вкладок

Создайте файлы **таббар. axml** и **Toolbar. axml** в каталоге **Resources/Layout** и вставьте их содержимое из следующего списка:

**RESOURCES/Layout/таббар. axml**

```xml
<android.support.design.widget.TabLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/sliding_tabs"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:tabIndicatorColor="@android:color/white"
    app:tabGravity="fill"
    app:tabMode="fixed" />
```

Для вкладок задано несколько свойств, включая значение параметра "сила притяжения" `fill` и "режим" `fixed` .
Если у вас много вкладок, вы можете переключиться на эту страницу, прочтите [документацию](https://developer.android.com/reference/android/support/design/widget/TabLayout.html) по Android таблайаут, чтобы узнать больше.

**Ресурсы, макеты и панели инструментов. axml**

```xml
<android.support.v7.widget.Toolbar
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
    app:layout_scrollFlags="scroll|enterAlways" />
```

В этих файлах мы создаем конкретную тему для панели инструментов, которая может различаться для вашего приложения.
Дополнительные сведения см. в записи блога о [панели инструментов Hello](https://blog.xamarin.com/android-tips-hello-toolbar-goodbye-action-bar/) .

### <a name="6-update-the-mainactivity"></a>6. Обновите`MainActivity`

В существующих Xamarin.Forms приложениях класс **MainActivity.CS** будет наследовать от `FormsApplicationActivity` . Его необходимо заменить на, `FormsAppCompatActivity` чтобы включить новые функции.

**MainActivity.cs**

```csharp
public class MainActivity : FormsAppCompatActivity  // was FormsApplicationActivity
```

Наконец, «привязать новые макеты из шага 5 в `OnCreate` методе, как показано ниже:

```csharp
protected override void OnCreate(Bundle bundle)
{
  // set the layout resources first
  FormsAppCompatActivity.ToolbarResource = Resource.Layout.Toolbar;
  FormsAppCompatActivity.TabLayoutResource = Resource.Layout.Tabbar;

  // then call base.OnCreate and the Xamarin.Forms methods
  base.OnCreate(bundle);
  Forms.Init(this, bundle);
  LoadApplication(new App());
}
```
