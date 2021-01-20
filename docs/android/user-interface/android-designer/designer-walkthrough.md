---
title: Использование Xamarin. Android Designer
description: Эта статья представляет собой пошаговое руководство по Xamarin. Android Designer. Здесь показано, как создать пользовательский интерфейс для небольшого приложения браузера цвета. Этот пользовательский интерфейс полностью создан в конструкторе.
ms.prod: xamarin
ms.assetid: 70FF2F9A-71BD-317E-C881-A44D82DF1BD8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/25/2018
ms.openlocfilehash: 1eed9691684bffbc9367c9b411a83876a52cf72f
ms.sourcegitcommit: 63029dd7ea4edb707a53ea936ddbee684a926204
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2021
ms.locfileid: "98609771"
---
# <a name="using-the-xamarinandroid-designer"></a>Использование Xamarin. Android Designer

_Эта статья представляет собой пошаговое руководство по Xamarin. Android Designer. Здесь показано, как создать пользовательский интерфейс для небольшого приложения браузера цвета. Этот пользовательский интерфейс полностью создан в конструкторе._

## <a name="overview"></a>Обзор

Пользовательские интерфейсы Android можно создавать декларативно с помощью XML-файлов или программно, путем написания кода. Xamarin. Android Designer позволяет разработчикам создавать и изменять декларативные макеты визуально, не требуя ручного редактирования XML-файлов. Конструктор также предоставляет отзыв в режиме реального времени, позволяющий разработчикам оценивать изменения пользовательского интерфейса без необходимости повторного развертывания приложения на устройстве или в эмуляторе. Эти функции конструктора могут ускорить разработку пользовательского интерфейса Android невероятно.
В этой статье показано, как использовать Xamarin. Android Designer для визуального создания пользовательского интерфейса.

> [!TIP]
> Новые выпуски Visual Studio поддерживают открытие XML-файлов в Android Designer.
>
> Android Designer поддерживает как файлы AXML, так и XML.

## <a name="walkthrough"></a>Пошаговое руководство

Цель этого пошагового руководства — использовать Android Designer для создания пользовательского интерфейса для примера приложения браузера цветов. Приложение браузера цветов отображает список цветов, их имена и значения RGB. Вы узнаете, как добавлять мини-приложения в **область конструктора** , а также как визуально размещать эти мини-приложения. После этого вы узнаете, как изменить мини-приложения в интерактивном режиме на **область конструктора** или с помощью панели **свойств** конструктора. Наконец, вы увидите, как выглядит дизайн при запуске приложения на устройстве или в эмуляторе.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

### <a name="creating-a-new-project"></a>Создание нового проекта

Первым шагом является создание проекта Xamarin. Android. Запустите Visual Studio, щелкните **создать проект...** и выберите шаблон **Visual C \# > Android > Android (Xamarin)** .
Назовите новое приложение **десигнервалксраугх** и нажмите кнопку **ОК**.

[![Пустое приложение Android](designer-walkthrough-images/vs/01-android-app-w158-sml.png)](designer-walkthrough-images/vs/01-android-app-w158.png#lightbox)

В диалоговом окне **новое приложение Android** выберите **пустое приложение** и нажмите кнопку **ОК**.

[![Выбор шаблона Пустое приложение Android](designer-walkthrough-images/vs/02-blank-app-w158-sml.png)](designer-walkthrough-images/vs/02-blank-app-w158.png#lightbox)

### <a name="adding-a-layout"></a>Добавление макета

Следующим шагом является создание **элемент LinearLayout** , который будет содержать элементы пользовательского интерфейса. Щелкните правой кнопкой мыши **ресурсы и макет** в **Обозреватель решений** и выберите **Добавить > новый элемент...**. В диалоговом окне **Добавление нового элемента** выберите **Макет Android**. Присвойте файлу имя **list_item** и нажмите кнопку **Добавить**:

[![Новый макет](designer-walkthrough-images/vs/03-new-layout-w158-sml.png)](designer-walkthrough-images/vs/03-new-layout-w158.png#lightbox)

В конструкторе отобразится новый макет **list_item** . Обратите внимание, что на левой панели отображаются две панели, &ndash; *область конструктора* для **list_item** видимой в области слева, в то время как источник XML отображается на правой панели. Можно поменять местами **область конструктора** и **исходных** областей, щелкнув значок **подкачки панелей** , расположенный между двумя панелями:

[![Представление конструктора](designer-walkthrough-images/vs/04-designer-view-w158-sml.png)](designer-walkthrough-images/vs/04-designer-view-w158.png#lightbox)

В меню **вид** выберите пункт **другие окна > структура документа** , чтобы открыть **структуру документа**. В **структуре документа** показано, что в настоящее время макет содержит одно мини-приложение **элемент LinearLayout** :

[![Структура документа](designer-walkthrough-images/vs/06-document-outline-w158-sml.png)](designer-walkthrough-images/vs/06-document-outline-w158.png#lightbox)

Следующим шагом является создание пользовательского интерфейса для приложения браузера цветов в рамках этого `LinearLayout` .

### <a name="creating-the-list-item-user-interface"></a>Создание пользовательского интерфейса элемента списка

Если панель **элементов** не отображается, щелкните вкладку **область элементов** слева. В **области элементов** прокрутите вниз до раздела **Images & Media (изображения** ) и прокрутите вниз, пока не найдете `ImageView` :

[![Обнаружение Имажевиев](designer-walkthrough-images/vs/07-locate-imageview-w158-sml.png)](designer-walkthrough-images/vs/07-locate-imageview-w158.png#lightbox)

Кроме того, можно ввести *имажевиев* в строку поиска, чтобы найти `ImageView` :

[![Поиск Имажевиев](designer-walkthrough-images/vs/08-imageview-search-w158-sml.png)](designer-walkthrough-images/vs/08-imageview-search-w158.png#lightbox)

Перетащите его `ImageView` на область конструктора ( `ImageView` будет использоваться для вывода образца цвета в приложении браузера цветов):

[![Имажевиев на холсте](designer-walkthrough-images/vs/09-imageview-on-canvas-w158-sml.png)](designer-walkthrough-images/vs/09-imageview-on-canvas-w158.png#lightbox)

Затем перетащите мини-приложение `LinearLayout (Vertical)` из **области элементов** в конструктор. Обратите внимание, что синий контур указывает границы добавленного объекта `LinearLayout` . **Структура документа** показывает, что он является дочерним элементом `LinearLayout` , расположенным в папке `imageView1 (ImageView)` :

[![Синий контур](designer-walkthrough-images/vs/10-blue-outline-w158-sml.png)](designer-walkthrough-images/vs/10-blue-outline-w158.png#lightbox)

При выборе `ImageView` в конструкторе синяя структура перемещается вокруг `ImageView` . Кроме того, выделение перемещается в `imageView1 (ImageView)` **структуру документа**:

[![Выбор Имажевиев](designer-walkthrough-images/vs/11-select-imageview-w158-sml.png)](designer-walkthrough-images/vs/11-select-imageview-w158.png#lightbox)

Затем перетащите мини-приложение `Text (Large)` из **панели элементов** в только что добавленное `LinearLayout` . Обратите внимание, что в конструкторе используются зеленые выделения, указывающие, куда будет вставлен новый мини-приложение:

[![Зеленые выделения](designer-walkthrough-images/vs/12-green-highlight-w158-sml.png)](designer-walkthrough-images/vs/12-green-highlight-w158.png#lightbox)

Затем добавьте мини-приложение `Text (Small)` под `Text (Large)` графическим элементом:

[![Добавить мини-приложение для малого текста](designer-walkthrough-images/vs/13-add-small-text-w158-sml.png)](designer-walkthrough-images/vs/13-add-small-text-w158.png#lightbox)

На этом этапе область конструктора должна выглядеть как на следующем снимке экрана:

[![На снимке экрана показана область конструктора с панелью элементов, структурой документа и областью макета с небольшим текстом.](designer-walkthrough-images/vs/14-raw-layout-w158-sml.png)](designer-walkthrough-images/vs/14-raw-layout-w158.png#lightbox)

Если эти два `textView` мини-приложения не находятся внутри `linearLayout1` , их можно перетащить `linearLayout1` в **структуру документа** и разместить их так, чтобы они отображались, как показано на предыдущем снимке экрана (с отступом в разделе `linearLayout1` ).

### <a name="arranging-the-user-interface"></a>Упорядочение пользовательского интерфейса

Следующим шагом является изменение пользовательского интерфейса для показа `ImageView` слева, при этом два `TextView` мини-приложения располагаются справа от `ImageView` .

1. Выберите `ImageView`.

2. В **окно свойств** введите *Width* в поле поиска и найдите **ширину макета**.

3. Измените значение **ширины макета** на `wrap_content` :

![Задать содержимое для переноса](designer-walkthrough-images/vs/15-wrap-content-w158.png)

Другой способ изменить этот `Width` параметр — щелкнуть треугольник в правой части мини-приложения, чтобы переключить его ширину на `wrap_content` :

![Перетащите, чтобы задать ширину](designer-walkthrough-images/vs/15b-width-arrow-w158.png)

Если щелкнуть треугольник еще раз, `Width` параметр будет возвращен в значение `match_parent` . Затем перейдите на панель **Структура документа** и выберите корень `LinearLayout` :

[![Выбор корневого элемент LinearLayout](designer-walkthrough-images/vs/16-root-linearlayout-w158-sml.png)](designer-walkthrough-images/vs/16-root-linearlayout-w158.png#lightbox)

Выбрав корневой элемент `LinearLayout` , вернитесь на панель **свойств** , введите *Orientation* в поле поиска и найдите параметр **ориентации** . Изменить **ориентацию** на `horizontal` :

![Выбрать ориентацию по горизонтали](designer-walkthrough-images/vs/17-horizontal-orientation-w158.png)

На этом этапе область конструктора должна выглядеть примерно так, как показано на снимке экрана ниже.
Обратите внимание, что `TextView` мини-приложения были перемещены справа от `ImageView` :

[![На снимке экрана показана область конструктора с панелью элементов, структурой документа и областью макета.](designer-walkthrough-images/vs/18-designer-layout-w158-sml.png)](designer-walkthrough-images/vs/18-designer-layout-w158.png#lightbox)

### <a name="modifying-the-spacing"></a>Изменение расстояния

Следующим шагом является изменение параметров заполнения и полей в пользовательском интерфейсе, чтобы предоставить больше пространства между мини-приложениями. Выберите элемент в `ImageView` области конструктора. В области **Свойства** введите `min` в поле поиска. Введите `70dp` для **минимальной высоты** и `50dp` **минимальной ширины**:

[![Настройка высоты и ширины](designer-walkthrough-images/vs/18b-set-height-width-sml.png)](designer-walkthrough-images/vs/18b-set-height-width.png#lightbox)

В области **Свойства** введите `padding` в поле поиска и введите `10dp` для **заполнения**. Эти `minHeight` `minWidth` Параметры и `padding` настройки добавляют отступ вокруг всех сторон элемента `ImageView` и елонгате его по вертикали. Обратите внимание, что XML макета изменяется при вводе этих значений:

[![Задать заполнение](designer-walkthrough-images/vs/19-padding-widths-w158-sml.png)](designer-walkthrough-images/vs/19-padding-widths-w158.png#lightbox)

Параметры "снизу", "слева", "справа" и "верхнее заполнение" можно задавать независимо, вводя значения в поля " **отступ снизу**", " **Отступ слева**", " **справа**" и " **Заполнение" верхних** полей соответственно.
Например, задайте для поля **левое** поле значение `5dp` , а для полей Отступ **снизу**, **Отступ справа** и **Заполнение верхнего** поля `10dp` —:

[![Пользовательские параметры заполнения](designer-walkthrough-images/vs/20-custom-padding-w158-sml.png)](designer-walkthrough-images/vs/20-custom-padding-w158.png#lightbox)

Затем измените расположение мини-приложения `LinearLayout` , которое содержит два мини-приложения `TextView` . В **структуре документа** выберите `linearLayout1` . В окне **Свойства** введите `margin` в поле поиска. Задать **расположение поля макета снизу**, левое **поле макета** и **поле макета сверху** `5dp` . Задайте для **поля макета право** `0dp` :

[![Задать поля](designer-walkthrough-images/vs/21-margins-w158-sml.png)](designer-walkthrough-images/vs/21-margins-w158.png#lightbox)

### <a name="removing-the-default-image"></a>Удаление изображения по умолчанию

Так как используется `ImageView` для показа цветов (а не изображений), следующим шагом является удаление источника изображения по умолчанию, добавленного шаблоном.

1. Выберите элемент в `ImageView` **области конструктора**.

2. В окне **Свойства** введите *src* в поле поиска.

3. Щелкните маленький квадрат справа от параметра свойства **src** и выберите **сбросить**.

[![Очистка параметра src Имажевиев](designer-walkthrough-images/vs/22-clear-img-src-w158-sml.png)](designer-walkthrough-images/vs/22-clear-img-src-w158.png#lightbox)

Это приведет `android:src="@android:drawable/ic_menu_gallery"` к удалению из исходного XML-файла `ImageView` .

### <a name="adding-a-listview-container"></a>Добавление контейнера ListView

Теперь, когда макет **list_item** определен, следующим шагом является добавление в `ListView` основной макет. Он `ListView` будет содержать список **list_item**. 

В **Обозреватель решений** откройте **ресурсы/макет/activity_main. axml**. На **панели элементов** выберите мини- `ListView` приложение и перетащите его на **область конструктора**. `ListView`В конструкторе будет пустым, за исключением синих линий, в которых отображается граница, если она выбрана. Можно просмотреть **структуру документа** , чтобы убедиться, что **ListView** добавлен правильно:

[![Новый ListView](designer-walkthrough-images/vs/23-new-listview-w158-sml.png)](designer-walkthrough-images/vs/23-new-listview-w158.png#lightbox)

По умолчанию параметру присваивается `ListView` `Id` значение `@+id/listView1` .
Пока `listView1` все еще выбрано в **структуре документа**, откройте панель **Свойства** , щелкните **упорядочение по** и выберите **Категория**.
Откройте **главное**, выберите свойство **ID** и измените его значение на `@+id/myListView` :

[![Переименование идентификатора в Милиствиев](designer-walkthrough-images/vs/24-change-id-w158-sml.png)](designer-walkthrough-images/vs/24-change-id-w158.png#lightbox)

На этом этапе пользовательский интерфейс готов к использованию.

### <a name="running-the-application"></a>Запуск приложения

Откройте **MainActivity.CS** и замените его код следующим:

```csharp
using Android.App;
using Android.Widget;
using Android.Views;
using Android.OS;
using Android.Support.V7.App;
using System.Collections.Generic;

namespace DesignerWalkthrough
{
    [Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
    public class MainActivity : AppCompatActivity
    {
        List<ColorItem> colorItems = new List<ColorItem>();
        ListView listView;

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set our view from the "main" layout resource
            SetContentView(Resource.Layout.activity_main);
            listView = FindViewById<ListView>(Resource.Id.myListView);

            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.DarkRed,
                ColorName = "Dark Red",
                Code = "8B0000"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.SlateBlue,
                ColorName = "Slate Blue",
                Code = "6A5ACD"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.ForestGreen,
                ColorName = "Forest Green",
                Code = "228B22"
            });

            listView.Adapter = new ColorAdapter(this, colorItems);
        }
    }

    public class ColorAdapter : BaseAdapter<ColorItem>
    {
        List<ColorItem> items;
        Activity context;
        public ColorAdapter(Activity context, List<ColorItem> items)
            : base()
        {
            this.context = context;
            this.items = items;
        }
        public override long GetItemId(int position)
        {
            return position;
        }
        public override ColorItem this[int position]
        {
            get { return items[position]; }
        }
        public override int Count
        {
            get { return items.Count; }
        }
        public override View GetView(int position, View convertView, ViewGroup parent)
        {
            var item = items[position];

            View view = convertView;
            if (view == null) // no view to re-use, create new
                view = context.LayoutInflater.Inflate(Resource.Layout.list_item, null);
            view.FindViewById<TextView>(Resource.Id.textView1).Text = item.ColorName;
            view.FindViewById<TextView>(Resource.Id.textView2).Text = item.Code;
            view.FindViewById<ImageView>(Resource.Id.imageView1).SetBackgroundColor(item.Color);

            return view;
        }
    }

    public class ColorItem
    {
        public string ColorName { get; set; }
        public string Code { get; set; }
        public Android.Graphics.Color Color { get; set; }
    }
}

```

Этот код использует пользовательский `ListView` адаптер для загрузки сведений о цвете и для вывода этих данных в пользовательском интерфейсе, который был только что создан. Чтобы не усложнять этот пример, сведения о цвете жестко запрограммированы в списке, но адаптер можно изменить, чтобы извлечь сведения о цвете из источника данных или вычислить его на лету. Дополнительные сведения о `ListView` адаптерах см. в разделе [ListView](~/android/user-interface/layouts/list-view/index.md).

Создайте и запустите приложение. На следующем снимке экрана представлен пример того, как приложение отображается при запуске на устройстве.

[![Окончательный снимок экрана](designer-walkthrough-images/vs/25-final-screenshot-sml.png)](designer-walkthrough-images/vs/25-final-screenshot.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

### <a name="creating-a-new-project"></a>Создание нового проекта

Первым шагом является создание проекта Xamarin. Android.

Запустите Visual Studio для Mac и нажмите кнопку **создать проект...**. Выберите шаблон **приложения Android** и нажмите кнопку **Далее**.

[![Пустое приложение Android](designer-walkthrough-images/xs/01-android-app-m75-sml.png)](designer-walkthrough-images/xs/01-android-app-m75.png#lightbox)

Назовите новое приложение **десигнервалксраугх**. В разделе **целевые платформы** выберите **Последняя и наибольшая** и нажмите кнопку **Далее**.

[![Имя приложения](designer-walkthrough-images/xs/02-designer-walkthrough-m75-sml.png)](designer-walkthrough-images/xs/02-designer-walkthrough-m75.png#lightbox)

На следующем экране диалогового окна нажмите кнопку **создать**.

### <a name="adding-a-layout"></a>Добавление макета

Следующим шагом является создание **элемент LinearLayout** , который будет содержать элементы пользовательского интерфейса.

В Visual Studio для Mac щелкните правой кнопкой мыши **ресурсы и макет** на панели **решения** и выберите **Добавить > новый файл...**. В диалоговом окне **новый файл** выберите **> макет Android**. Присвойте файлу имя **list_item** и нажмите кнопку **создать**.

[![Новый макет](designer-walkthrough-images/xs/03-new-layout-m75-sml.png)](designer-walkthrough-images/xs/03-new-layout-m75.png#lightbox)

После добавления этого файла на **область конструктора** отображается новый макет **list_item** (если отображается сообщение, *этот проект содержит ресурсы, которые не были успешно скомпилированы, может быть затронуто подготовка к просмотру*, щелкните **Сборка > собрать все** для построения проекта):

[![Представление конструктора](designer-walkthrough-images/xs/04-designer-view-m75-sml.png)](designer-walkthrough-images/xs/04-designer-view-m75.png#lightbox)

Щелкните вкладку **источник** в нижней части конструктора, чтобы просмотреть источник XML для этого макета. Если щелкнуть вкладку **Структура документа** справа, будет показано, что в данный момент макет содержит одно мини-приложение **элемент LinearLayout** :

[![XML-код конструктора](designer-walkthrough-images/xs/05-designer-xml-m75-sml.png)](designer-walkthrough-images/xs/05-designer-xml-m75.png#lightbox)

Следующим шагом является создание пользовательского интерфейса для приложения браузера цветов.

### <a name="creating-the-list-item-user-interface"></a>Создание пользовательского интерфейса элемента списка

Щелкните вкладку **конструктор** в нижней части экрана, чтобы вернуться в **область конструктора**. В области **панель элементов** справа перейдите к разделу **Images & Media (изображения** ) и найдите `ImageView` :

[![Обнаружение Имажевиев](designer-walkthrough-images/xs/06-locate-imageview-m75-sml.png)](designer-walkthrough-images/xs/06-locate-imageview-m75.png#lightbox)

Кроме того, можно ввести *имажевиев* в строку поиска, чтобы найти `ImageView` :

[![Поиск Имажевиев](designer-walkthrough-images/xs/07-imageview-search-m75-sml.png)](designer-walkthrough-images/xs/07-imageview-search-m75.png#lightbox)

Перетащите его `ImageView` на **область конструктора** ( `ImageView` будет использоваться для вывода образца цвета в приложении браузера цветов):

[![Имажевиев на холсте](designer-walkthrough-images/xs/08-imageview-on-canvas-m75-sml.png)](designer-walkthrough-images/xs/08-imageview-on-canvas-m75.png#lightbox)

Затем перетащите мини-приложение `LinearLayout (Vertical)` из **панели элементов** в **область конструктора**. Обратите внимание, что синий контур указывает границы добавленного объекта `LinearLayout` . **Структура документа** показывает, что он является дочерним элементом `LinearLayout` , расположенным ниже `imageView1 (ImageView)` :

[![Синий контур](designer-walkthrough-images/xs/10-blue-outline-m75-sml.png)](designer-walkthrough-images/xs/10-blue-outline-m75.png#lightbox)

При выборе `ImageView` в конструкторе синяя структура перемещается вокруг `ImageView` . Кроме того, выделение перемещается в `imageView1 (ImageView)` **структуру документа**:

[![Выбор Имажевиев](designer-walkthrough-images/xs/11-select-imageview-m75-sml.png)](designer-walkthrough-images/xs/11-select-imageview-m75.png#lightbox)

Затем перетащите мини-приложение `Text (Large)` из **панели элементов** в только что добавленное `LinearLayout` . Обратите внимание, что при перетаскивании указателя мыши на **область конструктора** он выделяет, куда будет вставлено новое мини-приложение.
Мини-приложение `Text (Large)` должно находиться внутри `linearLayout1` , как показано здесь:

[![Добавить мини-приложение крупного текста](designer-walkthrough-images/xs/12-green-highlight-m75-sml.png)](designer-walkthrough-images/xs/12-green-highlight-m75.png#lightbox)

Затем добавьте мини-приложение `Text (Small)` под `Text (Large)` графическим приложением. На этом этапе **область конструктора** должен выглядеть примерно следующим снимком экрана:

[![Добавить мини-приложение для малого текста](designer-walkthrough-images/xs/13-add-small-text-m75-sml.png)](designer-walkthrough-images/xs/13-add-small-text-m75.png#lightbox)

Если эти два `textView` мини-приложения не находятся внутри `linearLayout1` , их можно перетащить `linearLayout1` в **структуру документа** , чтобы они отображались так, как показано на предыдущем снимке экрана (с отступом в разделе `linearLayout1` ).

### <a name="arranging-the-user-interface"></a>Упорядочение пользовательского интерфейса

Следующим шагом является изменение пользовательского интерфейса для показа `ImageView` слева, при этом два `TextView` мини-приложения располагаются справа от `ImageView` .

1. Выбрав нужный `ImageView` пункт, перейдите на вкладку **свойства** .

2. Сразу под вкладкой **Свойства** нажмите кнопку **Макет**.

3. Прокрутите вниз до пункта **виевграуп** и измените `Width` значение параметра на  `wrap_content` :

[![Задать содержимое для переноса](designer-walkthrough-images/xs/15-wrap-content-m75-sml.png)](designer-walkthrough-images/xs/15-wrap-content-m75.png#lightbox)

Другой способ изменить этот `Width` параметр — щелкнуть треугольник в правой части мини-приложения, чтобы переключить его ширину на `wrap_content` :

[![Перетащите, чтобы задать ширину](designer-walkthrough-images/xs/16-width-arrow-m75-sml.png)](designer-walkthrough-images/xs/16-width-arrow-m75.png#lightbox)

Если щелкнуть треугольник еще раз, `Width` параметр будет возвращен в значение `match_parent` . Затем перейдите на панель **Структура документа** и выберите корень `LinearLayout` :

[![Выбор корневого элемент LinearLayout](designer-walkthrough-images/xs/17-root-linearlayout-m75-sml.png)](designer-walkthrough-images/xs/17-root-linearlayout-m75.png#lightbox)

Выбрав корень `LinearLayout` , вернитесь на вкладку **Свойства** и щелкните **мини**-приложение. Измените `Orientation` значение параметра на `horizontal` , как показано ниже. На этом этапе **область конструктора** должен выглядеть примерно так, как показано на снимке экрана ниже. Обратите внимание, что `TextView` мини-приложения были перемещены справа от `ImageView` :

[![Выбрать ориентацию по горизонтали](designer-walkthrough-images/xs/18-horizontal-orientation-m75-sml.png)](designer-walkthrough-images/xs/18-horizontal-orientation-m75.png#lightbox)

### <a name="modifying-the-spacing"></a>Изменение расстояния

Следующим шагом является изменение настроек заполнения и полей в пользовательском интерфейсе, чтобы предоставить больше пространства между мини-приложениями. Выберите `ImageView` и перейдите на вкладку **Макет** в разделе **Свойства**. Измените значение `Min Width` на `50dp` , на `Min Height` `70dp` , и на `Padding` `10dp` .
Это применяет заполнение вокруг всех сторон `ImageView` и елонгатес его по вертикали:

[![Задать заполнение](designer-walkthrough-images/xs/20-padding-widths-m75-sml.png)](designer-walkthrough-images/xs/20-padding-widths-m75.png#lightbox)

Параметры верхнего, правого, нижнего и левого отступов могут быть заданы независимо путем ввода значений в `Top` поля,, `Right` `Bottom` и `Left` заполнения соответственно. Например, задайте для `Left` параметра Padding значение, `5dp` а для `Top` `Right` `Bottom` заполнения значений — `10dp` . Обратите внимание, что `Padding` параметр изменяется на список следующих значений с разделителями-запятыми:

[![Пользовательские параметры заполнения](designer-walkthrough-images/xs/21-custom-padding-m75-sml.png)](designer-walkthrough-images/xs/21-custom-padding-m75.png#lightbox)

Затем измените расположение мини-приложения `LinearLayout` , которое содержит два мини-приложения `TextView` . В **структуре документа** выберите `linearLayout1` . На панели **Свойства** перейдите на вкладку **Макет** . Прокрутите вниз до раздела **виевграуп** и задайте `Left` `Top` для полей,, `Right` и значение `Bottom` `5dp` , `5dp` , `0dp` и `5dp` соответственно:

[![Задать поля](designer-walkthrough-images/xs/22-margins-m75-sml.png)](designer-walkthrough-images/xs/22-margins-m75.png#lightbox)

### <a name="removing-the-default-image"></a>Удаление изображения по умолчанию

Так как используется `ImageView` для показа цветов (а не изображений), следующим шагом является удаление источника изображения по умолчанию, добавленного шаблоном.

1. Выберите `ImageView`.

2. Перейдите на вкладку **мини** -приложение в разделе **свойства**.

3. Очистите `Src` параметр, чтобы он был пустым:

[![Очистка параметра src Имажевиев](designer-walkthrough-images/xs/23-clear-src-m75-sml.png)](designer-walkthrough-images/xs/23-clear-src-m75.png#lightbox)

Это приведет `android:src="@android:drawable/ic_menu_gallery"` к удалению из исходного XML-файла `ImageView` .

### <a name="adding-a-listview-container"></a>Добавление контейнера ListView

Теперь, когда макет **list_item** определен, следующим шагом является добавление в `ListView` основной макет. Он `ListView` будет содержать список **list_item**. 

В **Обозреватель решений** откройте **ресурсы/макет/Main. axml**.
Щелкните `Button` мини-приложение (если оно есть) и удалите его. На **панели элементов** выберите мини- `ListView` приложение и перетащите его на **область конструктора**.
`ListView`В конструкторе будет пустым, за исключением синих линий, в которых отображается граница, если она выбрана. Можно просмотреть **структуру документа** , чтобы убедиться, что **ListView** добавлен правильно:

[![Новый ListView](designer-walkthrough-images/xs/24-new-listview-m75-sml.png)](designer-walkthrough-images/xs/24-new-listview-m75.png#lightbox)

По умолчанию параметру присваивается `ListView` `Id` значение `@+id/listView1` .
Пока `listView1` все еще выбрано в **структуре документа**, откройте панель **Свойства** , щелкните **упорядочение по** и выберите **Категория**.
Откройте **главное**, выберите свойство **ID** и измените его значение на `@+id/myListView` :

[![Переименование идентификатора в Милиствиев](designer-walkthrough-images/xs/25-change-id-m75-sml.png)](designer-walkthrough-images/xs/25-change-id-m75.png#lightbox)

На этом этапе пользовательский интерфейс готов к использованию.

### <a name="running-the-application"></a>Запуск приложения

Откройте **MainActivity.CS** и замените его код следующим:

```csharp
using Android.App;
using Android.Widget;
using Android.Views;
using Android.OS;
using System.Collections.Generic;

namespace DesignerWalkthrough
{
    [Activity(Label = "@string/app_name", MainLauncher = true)]
    public class MainActivity : Activity
    {
        List<ColorItem> colorItems = new List<ColorItem>();
        ListView listView;

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set our view from the "main" layout resource
            SetContentView(Resource.Layout.Main);
            listView = FindViewById<ListView>(Resource.Id.myListView);

            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.DarkRed,
                ColorName = "Dark Red",
                Code = "8B0000"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.SlateBlue,
                ColorName = "Slate Blue",
                Code = "6A5ACD"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.ForestGreen,
                ColorName = "Forest Green",
                Code = "228B22"
            });

            listView.Adapter = new ColorAdapter(this, colorItems);
        }
    }

    public class ColorAdapter : BaseAdapter<ColorItem>
    {
        List<ColorItem> items;
        Activity context;
        public ColorAdapter(Activity context, List<ColorItem> items)
            : base()
        {
            this.context = context;
            this.items = items;
        }
        public override long GetItemId(int position)
        {
            return position;
        }
        public override ColorItem this[int position]
        {
            get { return items[position]; }
        }
        public override int Count
        {
            get { return items.Count; }
        }
        public override View GetView(int position, View convertView, ViewGroup parent)
        {
            var item = items[position];

            View view = convertView;
            if (view == null) // no view to re-use, create new
                view = context.LayoutInflater.Inflate(Resource.Layout.list_item, null);
            view.FindViewById<TextView>(Resource.Id.textView1).Text = item.ColorName;
            view.FindViewById<TextView>(Resource.Id.textView2).Text = item.Code;
            view.FindViewById<ImageView>(Resource.Id.imageView1).SetBackgroundColor(item.Color);

            return view;
        }
    }

    public class ColorItem
    {
        public string ColorName { get; set; }
        public string Code { get; set; }
        public Android.Graphics.Color Color { get; set; }
    }
}
```

Этот код использует пользовательский `ListView` адаптер для загрузки сведений о цвете и для вывода этих данных в пользовательском интерфейсе, который был только что создан. Чтобы не усложнять этот пример, сведения о цвете жестко запрограммированы в списке, но адаптер можно изменить, чтобы извлечь сведения о цвете из источника данных или вычислить его на лету. Дополнительные сведения о `ListView` адаптерах см. в разделе [ListView](~/android/user-interface/layouts/list-view/index.md).

Создайте и запустите приложение. На следующем снимке экрана представлен пример того, как приложение отображается при запуске на устройстве.

[![Окончательный снимок экрана](designer-walkthrough-images/xs/26-final-screenshot-sml.png)](designer-walkthrough-images/xs/26-final-screenshot.png#lightbox)

-----

## <a name="summary"></a>Итоги

В этой статье описывается процесс использования Xamarin. Android Designer в Visual Studio для создания пользовательского интерфейса для базового приложения.
Было продемонстрировано, как создать интерфейс для одного элемента в списке и продемонстрировать, как добавлять мини-приложения и размещать их визуально.
Также было объяснено, как назначать ресурсы, а затем задавать различные свойства для этих мини-приложений.
