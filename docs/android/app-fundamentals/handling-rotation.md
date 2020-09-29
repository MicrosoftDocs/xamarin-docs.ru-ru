---
title: Обработка поворота
description: В этом разделе описывается, как управлять изменением ориентации устройства в Xamarin. Android. В нем рассматривается работа с системой ресурсов Android для автоматической загрузки ресурсов для конкретной ориентации устройства, а также как программно Обрабатывайте изменения ориентации.
ms.prod: xamarin
ms.assetid: 6D33ADF7-ED81-0256-479D-D9E3787A76B0
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 944452160ed023f47fc2c648f092032c245350fb
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91455679"
---
# <a name="handling-rotation"></a>Обработка поворота

_В этом разделе описывается, как управлять изменением ориентации устройства в Xamarin. Android. В нем рассматривается работа с системой ресурсов Android для автоматической загрузки ресурсов для конкретной ориентации устройства, а также как программно Обрабатывайте изменения ориентации._

## <a name="overview"></a>Обзор

Так как мобильные устройства легко поворачиваются, встроенное вращение является стандартной функцией в мобильных ОС. Android предоставляет сложную платформу для работы с вращением в приложениях, независимо от того, создается ли пользовательский интерфейс декларативно в XML или программно в коде. При автоматической обработке изменений декларативного макета на повернутом устройстве приложение может воспользоваться преимуществами тесной интеграции с системой ресурсов Android. Для программного макета изменения должны обрабатываться вручную. Это обеспечивает более точное управление во время выполнения, но за счет большей работы для разработчика. Приложение может также отказаться от перезапуска действия и управлять изменением ориентации вручную.

В этом руководство рассматриваются следующие темы ориентации:

- Поворот декларативного **макета** &ndash; Использование системы ресурсов Android для создания приложений, поддерживающих ориентацию, включая загрузку макетов и драваблес для определенных ориентаций.

- **Программный поворот макета** &ndash; Как программно добавлять элементы управления, а также как управлять изменением ориентации вручную.

## <a name="handling-rotation-declaratively-with-layouts"></a>Декларативная обработка вращения с помощью макетов

Включив файлы в папки, которые следуют соглашениям об именовании, Android автоматически загружает соответствующие файлы при изменении ориентации.
Среди прочего, поддерживаются следующие шрифты:

- *Ресурсы макета* &ndash; Указание файлов макета, которые будут сведены для каждой ориентации.

- *Рисуемые ресурсы* &ndash; Указание, какие драваблес загружаются для каждой ориентации.

### <a name="layout-resources"></a>Ресурсы макета

По умолчанию файлы Android XML (AXML), содержащиеся в папке **Resources/Layout** , используются для отрисовки представлений для действия. Ресурсы этой папки используются как для книжной, так и для альбомной ориентации, если дополнительные ресурсы макета не предоставляются специально для альбомных. Рассмотрим структуру проекта, созданную шаблоном проекта по умолчанию:

[![Структура шаблона проекта по умолчанию](handling-rotation-images/00.png)](handling-rotation-images/00.png#lightbox)

Этот проект создает один файл **Main. axml** в папке **Resources/Layout** . При `OnCreate` вызове метода действия он расширяет представление, определенное в **Main. axml,** которое объявляет кнопку, как показано в приведенном ниже коде XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:orientation="vertical"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
<Button  
  android:id="@+id/myButton"
  android:layout_width="fill_parent" 
  android:layout_height="wrap_content" 
  android:text="@string/hello"/>
</LinearLayout>
```

Если устройство поворачивается в альбомную ориентацию, `OnCreate` метод действия вызывается снова, а тот же самый **главный файл axml** будет сведен, как показано на снимке экрана ниже:

[![Тот же экран, но в альбомной ориентации](handling-rotation-images/01-sml.png)](handling-rotation-images/01.png#lightbox)

#### <a name="orientation-specific-layouts"></a>Макеты, зависящие от ориентации

В дополнение к папке макета (которая по умолчанию имеет книжную ориентацию и также может быть явно названа *Макет-порт* путем включения папки с именем `layout-land` ), приложение может определять представления, необходимые в альбомной ориентации без каких-либо изменений кода.

Предположим, что файл **Main. axml** содержит следующий код XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
  <TextView
    android:text="This is portrait"
    android:layout_height="wrap_content"
    android:layout_width="fill_parent" />
</RelativeLayout>
```

Если папка с именем Layout-Land, содержащая дополнительный файл **Main. axml** , добавляется в проект, то при увеличении макета в альбомной ориентации на компьютер Android будет загружена только что добавленная **основная. axml.** Рассмотрим альбомную версию файла **Main. axml** , которая содержит следующий код (для простоты этот XML похож на книжную версию кода по умолчанию, но использует другую строку в `TextView` ):

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
  <TextView
    android:text="This is landscape"
    android:layout_height="wrap_content"
    android:layout_width="fill_parent" />
</RelativeLayout>
```

Выполнение этого кода и поворот устройства с книжной на альбомную демонстрирует новую загрузку XML, как показано ниже:

[![Книжная и альбомная снимки экрана печать в портретном режиме](handling-rotation-images/02.png)](handling-rotation-images/02.png#lightbox)

### <a name="drawable-resources"></a>Рисуемые ресурсы

Во время вращения Android рассматривает рисуемые ресурсы аналогично макетным ресурсам. В этом случае система получает драваблес из папок **Resources/Draw** , **Resources/Draw-Land** соответственно.

Например, предположим, что проект содержит изображение с именем Monkey.png в папке **Resources/Draw (ресурсы и нарисованные** ), где на изображении есть ссылка из `ImageView` XML-файла следующим образом:

```xml
<ImageView
  android:layout_height="wrap_content"
  android:layout_width="wrap_content"
  android:src="@drawable/monkey"
  android:layout_centerVertical="true"
  android:layout_centerHorizontal="true" />
```

Далее предполагается, что в разделе **ресурсы или нарисованная Land**включена другая версия **Monkey.png** . Как и в случае с файлами макета, при повороте устройства нарисованные изменения для данной ориентации, как показано ниже:

[![Разные версии Monkey.png отображаются в книжной и альбомной ориентации](handling-rotation-images/03.png)](handling-rotation-images/03.png#lightbox)

## <a name="handling-rotation-programmatically"></a>Обработка вращения программным способом

Иногда мы определяем макеты в коде. Это может происходить по различным причинам, включая технические ограничения, настройки разработчика и т. д. При добавлении элементов управления программным способом приложение должно вручную учитывать ориентацию устройства, которая автоматически обрабатывается при использовании ресурсов XML.

### <a name="adding-controls-in-code"></a>Добавление элементов управления в код

Для программного добавления элементов управления приложению необходимо выполнить следующие действия.

- Создайте макет.
- Задайте параметры макета.
- Создание элементов управления.
- Задайте параметры макета элемента управления.
- Добавьте элементы управления в макет.
- Установите макет в качестве представления содержимого.

Например, рассмотрим пользовательский интерфейс, состоящий из одного `TextView` элемента управления, добавленного в `RelativeLayout` , как показано в следующем коде.

```csharp
protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);
                        
  // create a layout
  var rl = new RelativeLayout (this);

  // set layout parameters
  var layoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.FillParent);
  rl.LayoutParameters = layoutParams;
        
  // create TextView control
  var tv = new TextView (this);

  // set TextView's LayoutParameters
  tv.LayoutParameters = layoutParams;
  tv.Text = "Programmatic layout";

  // add TextView to the layout
  rl.AddView (tv);
        
  // set the layout as the content view
  SetContentView (rl);
}
```

Этот код создает экземпляр `RelativeLayout` класса и задает его `LayoutParameters` свойство. `LayoutParams`Класс — это способ инкапсуляции элементов управления с возможностью повторного использования. После создания экземпляра макета можно создавать и добавлять в него элементы управления. В этом примере также есть элементы управления `LayoutParameters` , например `TextView` . После `TextView` создания добавьте его в `RelativeLayout` и установите в `RelativeLayout` качестве представления содержимого приложение, отображающее, `TextView` как показано ниже:

[![Кнопка приращения счетчика, отображаемая в режимах книжной и альбомной ориентации](handling-rotation-images/04.png)](handling-rotation-images/04.png#lightbox)

### <a name="detecting-orientation-in-code"></a>Определение ориентации в коде

Если приложение пытается загрузить другой пользовательский интерфейс для каждой ориентации `OnCreate` , когда вызывается (это происходит каждый раз при вращении устройства), он должен определить ориентацию, а затем загрузить нужный код пользовательского интерфейса. В Android имеется класс с именем `WindowManager` , который можно использовать для определения текущего вращения устройства через `WindowManager.DefaultDisplay.Rotation` свойство, как показано ниже:

```csharp
protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);
                        
  // create a layout
  var rl = new RelativeLayout (this);

  // set layout parameters
  var layoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.FillParent);
  rl.LayoutParameters = layoutParams;
                        
  // get the initial orientation
  var surfaceOrientation = WindowManager.DefaultDisplay.Rotation;
  // create layout based upon orientation
  RelativeLayout.LayoutParams tvLayoutParams;
                
  if (surfaceOrientation == SurfaceOrientation.Rotation0 || surfaceOrientation == SurfaceOrientation.Rotation180) {
    tvLayoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
  } else {
    tvLayoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
    tvLayoutParams.LeftMargin = 100;
    tvLayoutParams.TopMargin = 100;
  }
                        
  // create TextView control
  var tv = new TextView (this);
  tv.LayoutParameters = tvLayoutParams;
  tv.Text = "Programmatic layout";
        
  // add TextView to the layout
  rl.AddView (tv);
        
  // set the layout as the content view
  SetContentView (rl);
}
```

Этот код задает `TextView` положение с координатами 100 пикселей в верхнем левом углу экрана, автоматически анимировать в новом макете при повороте на альбомную, как показано ниже:

[![Состояние представления сохраняется в книжных и альбомных режимах.](handling-rotation-images/05.png)](handling-rotation-images/05.png#lightbox)

### <a name="preventing-activity-restart"></a>Предотвращение перезапуска действия

Помимо обработки всех `OnCreate` возможностей, приложение может препятствовать перезапуску действия при изменении ориентации, установив `ConfigurationChanges` в значение `ActivityAttribute` следующим образом:

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
```

После смены устройства действие не перезапускается. Чтобы вручную обойти изменение ориентации в этом случае, действие может переопределить `OnConfigurationChanged` метод и определить ориентацию от `Configuration` передаваемого объекта, как в новой реализации действия ниже:

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
public class CodeLayoutActivity : Activity
{
  TextView _tv;
  RelativeLayout.LayoutParams _layoutParamsPortrait;
  RelativeLayout.LayoutParams _layoutParamsLandscape;
                
  protected override void OnCreate (Bundle bundle)
  {
    // create a layout
    // set layout parameters
    // get the initial orientation

    // create portrait and landscape layout for the TextView
    _layoutParamsPortrait = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
                
    _layoutParamsLandscape = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
    _layoutParamsLandscape.LeftMargin = 100;
    _layoutParamsLandscape.TopMargin = 100;
                        
    _tv = new TextView (this);
                        
    if (surfaceOrientation == SurfaceOrientation.Rotation0 || surfaceOrientation == SurfaceOrientation.Rotation180) {
      _tv.LayoutParameters = _layoutParamsPortrait;
    } else {
      _tv.LayoutParameters = _layoutParamsLandscape;
    }
                        
    _tv.Text = "Programmatic layout";
    rl.AddView (_tv);
    SetContentView (rl);
  }
                
  public override void OnConfigurationChanged (Android.Content.Res.Configuration newConfig)
  {
    base.OnConfigurationChanged (newConfig);
                        
    if (newConfig.Orientation == Android.Content.Res.Orientation.Portrait) {
      _tv.LayoutParameters = _layoutParamsPortrait;
      _tv.Text = "Changed to portrait";
    } else if (newConfig.Orientation == Android.Content.Res.Orientation.Landscape) {
      _tv.LayoutParameters = _layoutParamsLandscape;
      _tv.Text = "Changed to landscape";
    }
  }
}
```

Здесь `TextView's` Параметры макета инициализируются как для альбомной, так и для книжной. Переменные класса содержат параметры вместе с `TextView` самим собой, так как действие не будет повторно создано при изменении ориентации. Код по-прежнему использует `surfaceOrientartion` в `OnCreate` для установки начального макета для `TextView` . После этого `OnConfigurationChanged` обрабатывает все последующие изменения макета.

При запуске приложения Android загружает изменения пользовательского интерфейса во время смены устройства и не перезапускает действие.

## <a name="preventing-activity-restart-for-declarative-layouts"></a>Предотвращение перезапуска действия для декларативных макетов

Перезапуски действий, вызванные поворотом устройства, также можно предотвратить, если определить макет в XML. Например, этот подход можно использовать, если мы хотим предотвратить перезагрузку действия (из соображений производительности, возможно), и нам не нужно загружать новые ресурсы для различных ориентаций.

Для этого мы используем ту же процедуру, которая используется в программном макете. Просто установите `ConfigurationChanges` в `ActivityAttribute` , как мы делали в `CodeLayoutActivity` предыдущем. Любой код, который необходимо выполнить для изменения ориентации, может быть реализован в `OnConfigurationChanged` методе.

## <a name="maintaining-state-during-orientation-changes"></a>Поддержание состояния во время изменения ориентации

Независимо от того, обрабатывается ли вращение декларативно или программно, все приложения Android должны реализовывать те же методы для управления состоянием при изменении ориентации устройства. Управление состоянием важно, так как система перезапускает выполняемое действие при вращении устройства Android. Android делает это для упрощения загрузки альтернативных ресурсов, таких как макеты и драваблес, разработанных специально для определенной ориентации. При перезапуске действие теряет любое временное состояние, которое может храниться в локальных переменных класса. Таким образом, если действие зависит от состояния, оно должно сохранять свое состояние на уровне приложения. Приложение должно поддерживать сохранение и восстановление состояния любого приложения, которое должно сохраняться при изменении ориентации.

Дополнительные сведения о сохранении состояния в Android см. в руководстве по [жизненному циклу действий](~/android/app-fundamentals/activity-lifecycle/index.md) .

## <a name="summary"></a>Сводка

В этой статье описано, как использовать встроенные возможности Android для работы с вращением. Во первых, было объяснено, как использовать систему ресурсов Android для создания приложений, поддерживающих ориентацию. Затем показано, как добавлять элементы управления в код, а также как управлять изменением ориентации вручную.

## <a name="related-links"></a>Связанные ссылки

- [Демонстрация вращения (пример)](/samples/xamarin/monodroid-samples/applicationfundamentals-rotationdemo)
- [Жизненный цикл действия](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Обработка изменений среды выполнения](https://developer.android.com/guide/topics/resources/runtime-changes.html)
- [Быстрое изменение ориентации экрана](https://android-developers.blogspot.com/2009/02/faster-screen-orientation-change.html)