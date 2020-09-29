---
title: Material Theme
description: Создание темы для приложения Xamarin. Android с помощью темы материалов
ms.prod: xamarin
ms.assetid: DC4CDBD0-3DF9-4B7E-B876-29128985E2A7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 22549bc7410b62e04cdb34c5c102a2fed18b0cf9
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457098"
---
# <a name="material-theme"></a>Material Theme

*Тема «материал* » — это стиль пользовательского интерфейса, который определяет внешний вид представлений и действий, начиная с Android 5,0 (без описания операций). Материалная тема встроена в Android 5,0, поэтому она используется системным ИНТЕРФЕЙСом и приложениями. Тема «материал» не является «темой» в том смысле, что в масштабе всей системы пользователь может динамически выбирать из меню параметров. Вместо этого тему материалов можно рассматривать как набор связанных встроенных базовых стилей, которые можно использовать для настройки внешнего вида и поведения приложения.

Android предоставляет три разновидности материала:

- `Theme.Material`&ndash;Темная версия темы материала; это разновидность по умолчанию в Android 5,0.

- `Theme.Material.Light`&ndash;Легкая версия темы материала.

- `Theme.Material.Light.DarkActionBar`&ndash;Светлая версия темы материала с темной панелью действий.

Ниже показаны примеры этих разновидностей темы материалов:

[![Пример снимка экрана темной темы, светлой темы и темной темы панели действий](material-theme-images/three-flavors-sml.png)](material-theme-images/three-flavors.png#lightbox)

Вы можете получить от темы материала, чтобы создать собственную тему, переопределив некоторые или все атрибуты цвета. Например, можно создать тему, производную от `Theme.Material.Light` , но переопределит цвет панели приложения в соответствии с цветом торговой марки. Можно также отформатировать отдельные представления. Например, можно создать стиль для [кардвиев](~/android/user-interface/controls/card-view.md) с более скругленными углами и использовать более темный цвет фона.

Можно использовать одну тему для всего приложения или использовать разные темы для различных экранов (действий) в приложении. На приведенных выше снимках экрана, например, одно приложение использует отдельную тему для каждого действия, чтобы продемонстрировать встроенные цветовые схемы. Переключатели переключают приложение в различные действия и в результате отображают разные темы.

Так как тема материалов поддерживается только в Android 5,0 и более поздних версиях, ее нельзя использовать (или пользовательскую тему, производную от темы материала) для подготовки приложения к работе в более ранних версиях Android. Однако можно настроить приложение для использования темы материалов на устройствах Android 5,0 и корректно перейти к более ранней теме при работе в более ранних версиях Android (Дополнительные сведения см. в разделе о [совместимости](#compatibility) этой статьи).

## <a name="requirements"></a>Требования

Для использования новых функций для материалов Android 5,0 в приложениях на основе Xamarin необходимо следующее:

- **Xamarin. Android** &ndash; xamarin. Android 4,20 или более поздней версии должен быть установлен и настроен с помощью Visual Studio или Visual Studio для Mac. 

- **Пакет SDK для Android** &ndash; Android 5,0 (API 21) или более поздней версии должен быть установлен с помощью диспетчера пакет SDK для Android.

- **Java JDK 1,8** &ndash; JDK 1,7 можно использовать, если вы специально нацелены на API уровня 23 и более ранних версий. JDK 1,8 доступен в [Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Сведения о настройке проекта приложения Android 5,0 см. в разделе [Настройка проекта android 5,0](~/android/platform/lollipop.md).

## <a name="using-the-built-in-themes"></a>Использование встроенных тем

Самый простой способ использовать тему материалов — настроить приложение для использования встроенной темы без настройки. Если вы не хотите явно настраивать тему, приложение по умолчанию будет иметь значение `Theme.Material` (темная тема). Если у приложения есть только одно действие, можно настроить тему на уровне действия. Если приложение имеет несколько действий, можно настроить тему на уровне приложения таким образом, чтобы она использовала одну и ту же тему для всех действий, или можно назначить разные темы для разных действий. В следующих разделах объясняется, как настроить темы на уровне приложения и на уровне действия.

### <a name="theming-an-application"></a>Приложения

Чтобы настроить все приложение для использования разновидности материала, задайте `android:theme` атрибуту узла приложения в **AndroidManifest.xml** одно из следующих:

- `@android:style/Theme.Material`&ndash;Темная тема.

- `@android:style/Theme.Material.Light`&ndash;Светло-тема.

- `@android:style/Theme.Material.Light.DarkActionBar`&ndash;Светлая тема с темной панелью действий.

В следующем примере приложение *MyApp* настраивается на использование светлой темы:

```xml
<application android:label="MyApp" 
             android:theme="@android:style/Theme.Material.Light">
</application>
```

Кроме того, можно задать `Theme` атрибут приложения в **AssemblyInfo.CS** (или **Properties.CS**). Пример:

```C#
[assembly: Application(Theme="@android:style/Theme.Material.Light")]
```

Если тема приложения имеет значение `@android:style/Theme.Material.Light` , каждое действие в *MyApp* будет отображаться с помощью `Theme.Material.Light` .

### <a name="theming-an-activity"></a>Действия

Чтобы создать тему действия, добавьте `Theme` параметр к `[Activity]` атрибуту над объявлением действия и назначьте `Theme` желаемую разновидность темы материала. В следующем примере для действия темы задано `Theme.Material.Light` следующее:

```C#
[Activity(Theme = "@android:style/Theme.Material.Light",
          Label = "MyApp", MainLauncher = true, Icon = "@drawable/icon")]  
```

Другие действия в этом приложении будут использовать `Theme.Material` цветовую схему по умолчанию (или, если настроено, параметр темы приложения).

<a name="customtheme"></a>

## <a name="using-custom-themes"></a>Использование пользовательских тем

Вы можете улучшить свою торговую марку, создав пользовательскую тему, которая будет использоваться для стиля приложения с цветами фирменной символики &rsquo; . Чтобы создать пользовательскую тему, необходимо определить новый стиль, производный от встроенной разновидности материала, переопределяя атрибуты цвета, которые требуется изменить. Например, можно определить пользовательскую тему, производную от, `Theme.Material.Light.DarkActionBar` и изменит цвет фона экрана на бежевый вместо белого.

Тема "материал" предоставляет следующие атрибуты макета для настройки:

- `colorPrimary`&ndash;Цвет панели приложения.

- `colorPrimaryDark`&ndash;Цвет строки состояния и контекстных панелей приложений; обычно это темная версия `colorPrimary` .

- `colorAccent`&ndash;Цвет элементов управления пользовательского интерфейса, таких как флажки, переключатели и поля редактирования.

- `windowBackground`&ndash;Цвет фона экрана.

- `textColorPrimary`&ndash;Цвет текста пользовательского интерфейса на панели приложения.

- `statusBarColor`&ndash;Цвет строки состояния.

- `navigationBarColor`&ndash;Цвет панели навигации.

Эти области экрана помечены на схеме ниже.

[![Схема атрибутов и связанных с ними областей экрана](material-theme-images/screen-attributes-sml.png)](material-theme-images/screen-attributes.png#lightbox)

По умолчанию `statusBarColor` для задано значение `colorPrimaryDark` . Можно задать `statusBarColor` сплошной цвет или установить для него значение, `@android:color/transparent` чтобы сделать строку состояния прозрачной. Панель навигации также можно сделать прозрачной, задав `navigationBarColor` для значение `@android:color/transparent` .

### <a name="creating-a-custom-app-theme"></a>Создание настраиваемой темы приложения

Вы можете создать пользовательскую тему приложения, создав и изменив файлы в папке **ресурсов** проекта приложения. Чтобы настроить стиль приложения с помощью пользовательской темы, выполните следующие действия.

- Создание файла **colors.xml** в **Resources/Values** &mdash; этот файл используется для определения пользовательских цветов темы. Например, можно вставить следующий код в **colors.xml** , чтобы помочь вам приступить к работе:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <color name="my_blue">#3498DB</color>
    <color name="my_green">#77D065</color>
    <color name="my_purple">#B455B6</color>
    <color name="my_gray">#738182</color>
</resources>
```

- Измените этот пример файла, чтобы определить имена и цветовые коды для цветовых ресурсов, которые будут использоваться в пользовательской теме.

- Создайте папку **Resources/Values-V21** . В этой папке создайте файл **styles.xml** :

    [![Расположение styles.xml в папке resources/values-21.xml](material-theme-images/values-v21-sml.png)](material-theme-images/values-v21.png#lightbox)

    Обратите внимание, что **Resources/Values-V21** относится к Android 5,0 &ndash; . более старые версии Android не будут читать файлы в этой папке.

- Добавьте `resources` узел в **styles.xml** и определите `style` узел с именем пользовательской темы. Например, ниже приведен файл **styles.xml** , определяющий *микустомсеме* (производный от встроенного `Theme.Material.Light` стиля темы):

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Customizations go here -->
    </style>
</resources>
```

- На этом этапе приложение, использующее *микустомсеме* , будет отображать тему акции `Theme.Material.Light` без настроек:

    [![Пользовательский внешний вид темы перед настройкой](material-theme-images/custom-theme-before-sml.png)](material-theme-images/custom-theme-before.png#lightbox)

- Добавьте настройки цвета в **styles.xml** , определив цвета атрибутов макета, которые необходимо изменить. Например, чтобы изменить цвет панели приложения на `my_blue` и изменить цвет элементов управления пользовательского интерфейса на `my_purple` , добавьте переопределение цветов в **styles.xml** , которые ссылаются на цветовые ресурсы, настроенные в **colors.xml**:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Override the app bar color -->
        <item name="android:colorPrimary">@color/my_blue</item>
        <!-- Override the color of UI controls -->
        <item name="android:colorAccent">@color/my_purple</item>
    </style>
</resources>
```

После внесения этих изменений приложение, использующее *микустомсеме* , будет отображать цвет панели приложения в `my_blue` и элементы управления пользовательского интерфейса в `my_purple` , но использовать `Theme.Material.Light` цветовую схему везде, где это необходимо:

[![Пользовательский внешний вид темы после настроек](material-theme-images/custom-theme-after-sml.png)](material-theme-images/custom-theme-after.png#lightbox)

В этом примере *микустомсеме* позаимствованы цвета `Theme.Material.Light` для цвета фона, строки состояния и цветов текста, но изменяет цвет панели приложения на `my_blue` и устанавливает цвет переключателя в значение `my_purple` .

<a name="customview"></a>

### <a name="creating-a-custom-view-style"></a>Создание пользовательского стиля представления

Android 5,0 также дает возможность стильировать отдельное представление. После создания **colors.xml** и **styles.xml** (как описано в предыдущем разделе) можно добавить стиль представления к **styles.xml**.
Чтобы применить стиль к отдельному представлению, выполните следующие действия.

- Измените **Resources/Values-V21/styles.xml** и добавьте `style` узел с именем пользовательского стиля представления. Задайте атрибуты пользовательского цвета для представления в этом `style` узле. Например, чтобы создать пользовательский стиль [кардвиев](~/android/user-interface/controls/card-view.md) с более скругленными углами и использовать `my_blue` его в качестве цвета фона карты, добавьте `style` узел **styles.xml** (внутри `resources` узла) и настройте цвет фона и радиус угла.

```xml
<!-- Theme an individual view: -->
<style name="CardView.MyBlue">

    <!-- Change the background color to Xamarin blue: -->
    <item name="cardBackgroundColor">@color/my_blue</item>

    <!-- Make the corners very round: -->
    <item name="cardCornerRadius">18dp</item>
</style>
```

- В макете задайте для `style` этого представления атрибут, соответствующий имени пользовательского стиля, выбранному на предыдущем шаге. Пример:

```xml
<android.support.v7.widget.CardView
    style="@style/CardView.MyBlue"
    android:layout_width="200dp"
    android:layout_height="100dp"
    android:layout_gravity="center_horizontal">
```

На следующем снимке экрана показан пример значения по умолчанию `CardView` (показано слева) по сравнению с тем, для которого задана `CardView` Пользовательская `CardView.MyBlue` тема (показана справа):

[![Примеры Кардвиев по умолчанию и пользовательского Кардвиев](material-theme-images/custom-cardview-sml.png)](material-theme-images/custom-cardview.png#lightbox)

В этом примере настраиваемый объект `CardView` отображается с цветом фона `my_blue` и радиусом 18dp угла.

## <a name="compatibility"></a>Совместимость

Чтобы присвоить приложению стиль, чтобы оно использовало тему материалов на Android 5,0, но автоматически перейдет к стилю, совместимому с предыдущими версиями Android, выполните следующие действия.

- Определите пользовательскую тему в окне " **ресурсы и значения" — V21/styles.xml** , которые являются производными от стиля темы материала. Пример:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

- Определите пользовательскую тему в списке **ресурсов/значений/styles.xml** , которая является производной от более старой темы, но использует то же имя темы, что и выше. Пример:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Holo.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

- В **AndroidManifest.xml**настройте приложение с помощью имени пользовательской темы. 
    Пример:

```xml
<application android:label="MyApp" 
             android:theme="@style/MyCustomTheme">
</application>
```

- Кроме того, можно применить стиль к конкретному действию с помощью пользовательской темы:

```C#
[Activity(Label = "MyActivity", Theme = "@style/MyCustomTheme")]
```

Если в вашей теме используются цвета, определенные в **colors.xml** файле, обязательно поместите этот файл в **Resources/Values** (а не **Resources/Values-V21**), чтобы обе версии пользовательской темы могли получить доступ к вашим определениям цветов.

Когда приложение выполняется на устройстве Android 5,0, оно будет использовать определение темы, указанное в **Resources/Values-V21/styles.xml**. Когда это приложение работает на старых устройствах Android, оно автоматически перейдет к определению темы, указанному в списке **ресурсов/значений/styles.xml**.

Дополнительные сведения о совместимости тем с более старыми версиями Android см. в разделе [альтернативные ресурсы](~/android/app-fundamentals/resources-in-android/alternate-resources.md).

## <a name="summary"></a>Сводка

В этой статье представлен новый стиль пользовательского интерфейса темы материалов, включенный в Android 5,0 (без описания операций). В нем описаны три встроенные разновидности материала, которые можно использовать для создания стиля приложения, а также объясняется, как создать пользовательскую тему для фирменной символики вашего приложения, а также как описать тему отдельного представления. Наконец, в этой статье объясняется, как использовать тему материалов в приложении, сохраняя при этом совместимость с более старыми версиями Android.

## <a name="related-links"></a>Связанные ссылки

- [Семесвитчер (пример)](/samples/xamarin/monodroid-samples/android50-themeswitcher)
- [Введение в интерфейс без описания операций](../platform/lollipop.md)
- [CardView](controls/card-view.md)
- [Альтернативные ресурсы](../app-fundamentals/resources-in-android/alternate-resources.md)
- [Интерфейс устройства Android](https://developer.android.com/about/versions/lollipop)
- [Разработчик круговой диаграммы Android](https://developer.android.com/about/versions/pie/)
- [Проектирование материалов](https://developer.android.com/guide/topics/ui/look-and-feel/)
- [Принципы проектирования материалов](https://material.io/design/)
- [Поддержание совместимости](https://developer.android.com/training/backward-compatible-ui/)