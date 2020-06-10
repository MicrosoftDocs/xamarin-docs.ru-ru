---
title: CardView
description: Мини-приложение Кардвиев — это компонент пользовательского интерфейса, который представляет содержимое текста и изображения в представлениях, напоминающих карты. В этом руководство объясняется, как использовать и настраивать Кардвиев в приложениях Xamarin. Android с сохранением обратной совместимости с более ранними версиями Android.
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 2684866d04f2b085c70c70a04ff2c828205f0f32
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571914"
---
# <a name="xamarinandroid-cardview"></a>Xamarin. Android Кардвиев

_Мини-приложение Кардвиев — это компонент пользовательского интерфейса, который представляет содержимое текста и изображения в представлениях, напоминающих карты. В этом руководство объясняется, как использовать и настраивать Кардвиев в приложениях Xamarin. Android с сохранением обратной совместимости с более ранними версиями Android._

## <a name="overview"></a>Обзор

`Cardview`Мини-приложение, представленное в Android 5,0 (без описания операций), представляет собой компонент пользовательского интерфейса, который представляет содержимое текста и изображения в представлениях, напоминающих карты. `CardView`реализуется как мини-приложение `FrameLayout` со скругленными углами и тенью. Как правило, `CardView` используется для представления одного элемента строки в `ListView` `GridView` группе представлений или. Например, на следующем снимке экрана показан пример приложения для резервирования командировок, в котором реализуются `CardView` карточки назначения для перемещения на основе прокрутки `ListView` :

![Пример приложения с использованием Кардвиев для каждого назначения в командировке](card-view-images/01-cardview-example.png)

В этом руководство объясняется, как добавить `CardView` пакет в проект Xamarin. Android, как добавить его `CardView` в макет и как настроить внешний вид `CardView` в приложении. Кроме того, в этом руководством содержится подробный список `CardView` атрибутов, которые можно изменить, включая атрибуты, которые помогут вам использовать `CardView` более ранние версии Android, чем Android 5,0.

<a name="requirements"></a>

## <a name="requirements"></a>Требования

Для использования новых функций Android 5,0 и более поздних версий (включая `CardView` ) в приложениях на основе Xamarin необходимо следующее:

- **Xamarin. Android** &ndash; xamarin. Android 4,20 или более поздней версии должен быть установлен и настроен с помощью Visual Studio или Visual Studio для Mac.

- **Пакет SDK для Android** &ndash; Android 5,0 (API 21) или более поздней версии должен быть установлен с помощью диспетчера пакет SDK для Android.

- **Java JDK 1,8** &ndash; JDK 1,7 можно использовать, если вы специально нацелены на API уровня 23 и более ранних версий. JDK 1,8 доступен в [Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Приложение должно также включать `Xamarin.Android.Support.v7.CardView` пакет. Чтобы добавить `Xamarin.Android.Support.v7.CardView` пакет в Visual Studio для Mac, выполните следующие действия.

1. Откройте проект, щелкните правой кнопкой мыши **пакеты** и выберите команду **Добавить пакеты**.

2. В диалоговом окне **Добавление пакетов** выполните поиск по запросу **кардвиев**.

3. Выберите **Библиотека поддержки Xamarin версии 7 кардвиев**, а затем щелкните **Добавить пакет**.

Чтобы добавить `Xamarin.Android.Support.v7.CardView` пакет в Visual Studio, выполните следующие действия.

1. Откройте проект, щелкните правой кнопкой мыши узел **ссылки** (в области **Обозреватель решений** ) и выберите пункт **Управление пакетами NuGet...**.

2. Когда откроется диалоговое окно **Управление пакетами NuGet** , введите **кардвиев** в поле поиска.

3. Когда появится **Библиотека поддержки Xamarin версии 7 кардвиев** , нажмите кнопку **установить**.

Сведения о настройке проекта приложения Android 5,0 см. в разделе [Настройка проекта android 5,0](~/android/platform/lollipop.md).
Дополнительные сведения об установке пакетов NuGet см. [в разделе Пошаговое руководство. Включение NuGet в проект](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

## <a name="introducing-cardview"></a>Знакомство с Кардвиев

Значение по умолчанию `CardView` напоминает белую карту с минимально закругленными углами и небольшой тенью. В следующем примере макета **Main. axml** отображается одно мини-приложение `CardView` , содержащее `TextView` :

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:gravity="center_horizontal"
    android:padding="5dp">
    <android.support.v7.widget.CardView
        android:layout_width="fill_parent"
        android:layout_height="245dp"
        android:layout_gravity="center_horizontal">
        <TextView
            android:text="Basic CardView"
            android:layout_marginTop="0dp"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:gravity="center"
            android:layout_centerVertical="true"
            android:layout_alignParentRight="true"
            android:layout_alignParentEnd="true" />
    </android.support.v7.widget.CardView>
</LinearLayout>
```

Если вы используете этот XML-код для замены существующего содержимого **Main. axml**, не забудьте закомментировать любой код в **MainActivity.CS** , который ссылается на ресурсы в предыдущем XML.

Этот пример макета создает значение по умолчанию `CardView` с одной строкой текста, как показано на следующем снимке экрана:

[![Снимок экрана Кардвиев с белым фоном и строкой текста](card-view-images/02-basic-cardview-sml.png)](card-view-images/02-basic-cardview.png#lightbox)

В этом примере для стиля приложения задана тема светлого материала (), `Theme.Material.Light` чтобы `CardView` упростить просмотр теней и границ. Дополнительные сведения о приложениях Android 5,0 см. в разделе « [Тема материалов](~/android/user-interface/material-theme.md)». В следующем разделе мы расскажем, как настроить `CardView` приложение.

## <a name="customizing-cardview"></a>Настройка Кардвиев

Основные атрибуты можно изменить, `CardView` чтобы настроить внешний вид в `CardView` приложении. Например, повышение уровня `CardView` можно увеличить, чтобы привести к более крупной теневой копии (что делает карту плавающей выше, чем у фона). Кроме того, радиус угла можно увеличить, чтобы сделать углы карты более скругленными.

В следующем примере макета настраиваемый `CardView` используется для создания имитации печатной фотографии ("моментальный снимок"). `ImageView`Добавляется в `CardView` для отображения изображения, а элемент `TextView` размещается под полем `ImageView` для отображения заголовка изображения.
В этом примере макет `CardView` имеет следующие настройки:

- `cardElevation`Увеличивается до 4dp для приведения более крупной тени.
- `cardCornerRadius`Увеличивается до 5dp, чтобы сделать углы более скругленными.

Поскольку `CardView` предоставляется библиотекой поддержки Android версии 7, ее атрибуты недоступны из `android:` пространства имен. Поэтому необходимо определить собственное пространство имен XML и использовать это пространство имен в качестве `CardView` префикса атрибута. В приведенном ниже примере макета мы будем использовать эту строку для определения пространства имен с именем `cardview` :

```xml
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
```

Это пространство имен можно вызвать `card_view` или даже `myapp` при выборе (оно доступно только в области этого файла). Независимо от выбранного для вызова этого пространства имен необходимо использовать его для префикса `CardView` атрибута, который требуется изменить. В этом примере макета `cardview` пространство имен является префиксом для `cardElevation` и `cardCornerRadius` :

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:gravity="center_horizontal"
    android:padding="5dp">
    <android.support.v7.widget.CardView
        android:layout_width="fill_parent"
        android:layout_height="245dp"
        android:layout_gravity="center_horizontal"
        cardview:cardElevation="4dp"
        cardview:cardCornerRadius="5dp">
        <LinearLayout
            android:layout_width="fill_parent"
            android:layout_height="240dp"
            android:orientation="vertical"
            android:padding="8dp">
            <ImageView
                android:layout_width="fill_parent"
                android:layout_height="190dp"
                android:id="@+id/imageView"
                android:scaleType="centerCrop" />
            <TextView
                android:layout_width="fill_parent"
                android:layout_height="wrap_content"
                android:textAppearance="?android:attr/textAppearanceMedium"
                android:textColor="#333333"
                android:text="Photo Title"
                android:id="@+id/textView"
                android:layout_gravity="center_horizontal"
                android:layout_marginLeft="5dp" />
        </LinearLayout>
    </android.support.v7.widget.CardView>
</LinearLayout>
```

Если этот пример макета используется для отображения изображения в приложении для просмотра фотографий, то `CardView` имеет внешний вид моментального снимка фотографии, как показано на следующем снимке экрана:

[![Кардвиев с изображением и заголовком под изображением](card-view-images/03-photo-cardview-sml.png)](card-view-images/03-photo-cardview.png#lightbox)

Этот снимок экрана взят из примера приложения [рециклервиевер](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer) , который использует мини-приложение `RecyclerView` для представления прокручиваемого списка `CardView` изображений для просмотра фотографий. Дополнительные сведения о `RecyclerView` см. в разделе [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) Guide.

Обратите внимание, что `CardView` в области содержимого может отображаться более одного дочернего представления. Например, в приведенном выше примере приложения для просмотра фотографий область содержимого состоит из объекта `ListView` , который содержит `ImageView` и `TextView` . Хотя `CardView` экземпляры часто располагаются вертикально, их также можно упорядочить по горизонтали (см. раздел [Создание пользовательского стиля представления](~/android/user-interface/material-theme.md#customview) для примера снимка экрана).

### <a name="cardview-layout-options"></a>Параметры макета Кардвиев

`CardView`макеты можно настраивать, настроив один или несколько атрибутов, влияющих на его заполнение, повышение прав, радиус угла и цвет фона.

[![Схема атрибутов Кардвиев](card-view-images/04-attributes-sml.png)](card-view-images/04-attributes.png#lightbox)

Каждый атрибут можно также изменить динамически, вызвав аналогичный `CardView` метод (Дополнительные сведения о `CardView` методах см. в [справочнике по классу кардвиев](https://developer.android.com/reference/android/support/v7/widget/CardView.html)).
Обратите внимание, что эти атрибуты (за исключением цвета фона) принимают значение измерения, которое представляет собой десятичное число, за которым следует единица измерения. Например, `11.5dp` задает 11,5 пикселей, не зависящих от плотности.

#### <a name="padding"></a>Заполнение

`CardView`предлагает пять атрибутов заполнения для размещения содержимого в карточке. Их можно задать в XML макета или вызвать аналогичные методы в коде:

[![Схема атрибутов заполнения Кардвиев](card-view-images/05-padding-sml.png)](card-view-images/05-padding.png#lightbox)

Атрибуты заполнения объясняются следующим образом.

- `contentPadding`&ndash;Внутреннее заполнение между дочерними представлениями `CardView` и всеми краями карты.

- `contentPaddingBottom`&ndash;Внутреннее заполнение между дочерними представлениями `CardView` и нижним краями карточки.

- `contentPaddingLeft`&ndash;Внутреннее заполнение между дочерними представлениями `CardView` и левым краями карточки.

- `contentPaddingRight`&ndash;Внутреннее заполнение между дочерними представлениями `CardView` и правым ребром карты.

- `contentPaddingTop`&ndash;Внутреннее заполнение между дочерними представлениями `CardView` и верхним краями карточки.

Атрибуты заполнения содержимого зависят от границы области содержимого, а не от какого-либо заданного мини-приложения, расположенного в области содержимого.
Например, если `contentPadding` в приложении для просмотра фотографий было достаточное увеличение, то `CardView` будет обрезать изображение и текст, отображаемый на карточке.

#### <a name="elevation"></a>Elevation

`CardView`предлагает два атрибута повышения прав для управления его повышением прав и, в результате, размер его тени:

[![Схема атрибутов повышения прав Кардвиев](card-view-images/06-elevation-sml.png)](card-view-images/06-elevation.png#lightbox)

Атрибуты повышения прав описаны ниже.

- `cardElevation`&ndash;Повышение уровня `CardView` (представляет свою ось Z).

- `cardMaxElevation`&ndash;Максимальное значение `CardView` повышения прав.

Большие значения `cardElevation` увеличивают размер тени, чтобы сделать его `CardView` более подходящим, чем верх фона. `cardElevation`Атрибут также определяет порядок рисования перекрывающихся представлений, то есть `CardView` будет отображаться под другим перекрывающимся представлением с более высоким уровнем повышения прав и над всеми перекрывающимися представлениями с более низким значением повышения прав.
Этот `cardMaxElevation` параметр полезен, если приложение динамически изменяет повышение прав &ndash; . Это предотвращает расширение тени за пределами, заданными с помощью этого параметра.

#### <a name="corner-radius-and-background-color"></a>Радиус и цвет фона угла

`CardView`предоставляет атрибуты, которые можно использовать для управления радиусом его угла и его цветом фона. Эти два свойства позволяют изменить общий стиль `CardView` :

[![Схема Кардвиев угла радиаус и атрибутов цвета фона](card-view-images/07-radius-bgcolor-sml.png)](card-view-images/07-radius-bgcolor.png#lightbox)

Эти атрибуты объясняются следующим образом.

- `cardCornerRadius`&ndash;Радиус угла для всех углов `CardView` .

- `cardBackgroundColor`&ndash;Цвет фона объекта `CardView` .

На этой диаграмме `cardCornerRadius` для задано более скругленное 10dp, а `cardBackgroundColor` для параметра задано значение `"#FFFFCC"` (светло-желтый).

## <a name="compatibility"></a>Совместимость

Вы можете использовать `CardView` более ранние версии Android, чем android 5,0 без описания операций. Поскольку `CardView` является частью библиотеки поддержки Android версии 7, вы можете использовать `CardView` с Android 2,1 (API уровня 7) и более поздней версии.
Однако пакет необходимо установить, `Xamarin.Android.Support.v7.CardView` как описано в разделе [требования](#requirements)выше.

`CardView`При работе с устройствами происходит немного другое поведение (уровень API 21):

- `CardView`использует программную реализацию теневой копии, которая добавляет дополнительное дополнение.

- `CardView`не обрезает дочерние представления, пересекающие `CardView` скругленные углы.

Чтобы упростить управление этими различиями в совместимости, `CardView` предоставляет несколько дополнительных атрибутов, которые можно настроить в макете:

- `cardPreventCornerOverlap`&ndash;Задайте для этого атрибута значение `true` , чтобы добавить заполнение при выполнении приложения в более ранних версиях Android (уровень API 20 и более ранние версии). Этот параметр предотвращает `CardView` пересечение содержимого со `CardView` скругленными углами.

- `cardUseCompatPadding`&ndash;Задайте для этого атрибута значение `true` , чтобы добавить заполнение при запуске приложения в версиях Android с уровнем API 21 или выше. Если вы хотите использовать на устройствах с предварительно заданными версиями `CardView` и они выглядят одинаково в параметре без описания операций (или более поздней версии), присвойте этому атрибуту значение `true` . Если этот атрибут включен, `CardView` добавляет дополнительное дополнение для рисования теней при запуске на устройствах с предварительной обначальным интерфейсом. Это помогает преодолеть различия в заполнении, которые появились, когда вступают в действие предварительные реализации программных теневых теней.

Дополнительные сведения о поддержке совместимости с более ранними версиями Android см. в разделе [поддержание совместимости](https://developer.android.com/training/material/compatibility.html).

## <a name="summary"></a>Сводка

В этом руководством появился новый мини-приложение, `CardView` входящее в Android 5,0 (без описания операций). Он демонстрирует внешний вид по умолчанию `CardView` и объясняет, как настраивать `CardView` его, изменяя его повышение прав, Скругление углов, заполнение содержимого и цвет фона. В нем перечислены `CardView` атрибуты макета (со схемами-образцами) и описано использование `CardView` на устройствах Android, более ранних, чем Android 5,0 без описания операций. Дополнительные сведения о `CardView` см. в [справочнике по классам кардвиев](https://developer.android.com/reference/android/support/v7/widget/CardView.html).

## <a name="related-links"></a>Связанные ссылки

- [RecyclerView (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [Введение в интерфейс без описания операций](~/android/platform/lollipop.md)
- [Справочник по классам Кардвиев](https://developer.android.com/reference/android/support/v7/widget/CardView.html)
