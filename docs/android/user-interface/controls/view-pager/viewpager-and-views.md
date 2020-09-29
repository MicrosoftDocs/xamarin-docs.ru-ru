---
title: ViewPager с представлениями
description: ViewPager — это Диспетчер макетов, позволяющий реализовать навигацию жестурал. Жестуралная навигация позволяет пользователю прокручивать влево и вправо для пошагового просмотра страниц данных. В этом руководстве объясняется, как реализовать прокрутку пользовательского интерфейса с помощью ViewPager и Пажертабстрип, используя представления в качестве страниц данных (в следующем руководстве описывается использование фрагментов для страниц).
ms.prod: xamarin
ms.assetid: 42E5379F-B0F4-4B87-A314-BF3DE405B0C8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: e4f4243c06d98eac6f3501c41b48508f260d4633
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91456994"
---
# <a name="viewpager-with-views"></a>ViewPager с представлениями

_ViewPager — это Диспетчер макетов, позволяющий реализовать навигацию жестурал. Жестуралная навигация позволяет пользователю прокручивать влево и вправо для пошагового просмотра страниц данных. В этом руководстве объясняется, как реализовать прокрутку пользовательского интерфейса с помощью ViewPager и Пажертабстрип, используя представления в качестве страниц данных (в следующем руководстве описывается использование фрагментов для страниц)._

## <a name="overview"></a>Обзор

Это руководство содержит пошаговую демонстрацию того, как использовать `ViewPager` для реализации коллекции изображений листопадное и популярная деревьев. В этом приложении пользователь просматривает левое и правое по «каталогу дерева» для просмотра изображений дерева. В верхней части каждой страницы каталога имя дерева отображается в `PagerTabStrip` , а изображение дерева — в виде объекта `ImageView` . Адаптер используется для взаимодействия с `ViewPager` базовой моделью данных. Это приложение реализует адаптер, производный от `PagerAdapter` . 

Хотя `ViewPager` приложения, основанные на службах, часто реализуются с помощью `Fragment` s, существуют некоторые сравнительно простые случаи использования, в которых не нужно использовать дополнительную сложность `Fragment` . Например, в базовом приложении Коллекция образов, показанном в этом пошаговом руководстве, не требуется использование `Fragment` s. Так как содержимое является статическим и пользователь просматривает только между разными образами, реализацию можно упростить, используя стандартные представления и макеты Android. 

## <a name="start-an-app-project"></a>Запуск проекта приложения

Создайте проект Android с именем **трипажер** (Дополнительные сведения о создании проектов Android см. в статье [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) ). Затем запустите диспетчер пакетов NuGet. (Дополнительные сведения об установке пакетов NuGet см. [в разделе Пошаговое руководство. Включение NuGet в проект](/visualstudio/mac/nuget-walkthrough)). Найдите и установите **библиотеку поддержки Android v4**: 

[![Снимок экрана: поддержка NuGet версии 4 выбрана в диспетчере пакетов NuGet](viewpager-and-views-images/01-install-support-lib-sml.png)](viewpager-and-views-images/01-install-support-lib.png#lightbox)

При этом также будут установлены дополнительные пакеты, реакуиред с помощью **библиотеки поддержки Android версии 4**.

## <a name="add-an-example-data-source"></a>Добавление примера источника данных

В этом примере источник данных каталога дерева (представленный `TreeCatalog` классом) предоставляет `ViewPager` содержимое с содержимым элемента. 
`TreeCatalog` содержит готовый набор изображений в виде дерева и заголовков деревьев, которые адаптер будет использовать для создания `View` s. `TreeCatalog`Конструктор не требует аргументов:

```csharp
TreeCatalog treeCatalog = new TreeCatalog();
```

Коллекция изображений в `TreeCatalog` организована таким образом, что индексатор может получить доступ к каждому изображению. Например, следующая строка кода извлекает идентификатор ресурса изображения для третьего изображения в коллекции: 

```csharp
int imageId = treeCatalog[2].imageId;
```

Поскольку сведения о реализации `TreeCatalog` не важны для понимания `ViewPager` , `TreeCatalog` код не указан здесь. Исходный код `TreeCatalog` доступен по адресу [TreeCatalog.CS](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/TreePager/TreeCatalog.cs). Скачайте этот исходный файл (или скопируйте и вставьте код в новый файл **TreeCatalog.CS** ) и добавьте его в проект. Кроме того, скачайте и распакуйте [файлы образа](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/Resources/tree-images.zip?raw=true) в папку **Resources/Draw** и включите их в проект. 

## <a name="create-a-viewpager-layout"></a>Создание макета ViewPager

Откройте файл **Resources/Layout/Main. axml** и замените его содержимое следующим XML-кодом:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

</android.support.v4.view.ViewPager>
```

Этот XML-код определяет `ViewPager` , который занимает весь экран. Обратите внимание, что необходимо использовать полное имя **Android. support. v4. View. ViewPager** , так как `ViewPager` оно упаковано в библиотеку поддержки. `ViewPager` доступен только в [библиотеке поддержки Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/); он недоступен в пакет SDK для Android. 

## <a name="set-up-viewpager"></a>Настройка ViewPager

Измените **MainActivity.CS** и добавьте следующий `using` оператор:

```csharp
using Android.Support.V4.View;
```

Замените метод `OnCreate` следующим кодом:

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    TreeCatalog treeCatalog = new TreeCatalog();
}
```

Этот код делает следующее:

1. Задает представление из ресурса макета **Main. axml** .

2. Извлекает ссылку на `ViewPager` из макета.

3. Создает новый объект в `TreeCatalog` качестве источника данных.

При сборке и запуске этого кода вы увидите экран, который напоминает следующий снимок экрана: 

[![Снимок экрана: приложение, отображающее пустой ViewPager](viewpager-and-views-images/02-initial-screen-sml.png)](viewpager-and-views-images/02-initial-screen.png#lightbox)

На этом этапе объект `ViewPager` пуст, так как в нем отсутствует адаптер для доступа к содержимому в **трикаталог**. В следующем разделе создается **пажерадаптер** для подключения к `ViewPager` **трикаталог**. 

## <a name="create-the-adapter"></a>Создание адаптера

`ViewPager` использует объект контроллера адаптера, расположенный между объектом `ViewPager` и источником данных (см. иллюстрацию в [адаптере](~/android/user-interface/controls/view-pager/index.md#adapter)). Для доступа к этим данным `ViewPager` необходимо предоставить пользовательский адаптер, производный от `PagerAdapter` . Этот адаптер заполняет каждую `ViewPager` страницу содержимым из источника данных. Поскольку этот источник данных зависит от конкретного приложения, Пользовательский адаптер является кодом, который понимает, как получить доступ к данным. По мере того, как пользователь просматривает страницы `ViewPager` , адаптер извлекает данные из источника данных и загружает их на страницы для `ViewPager` отображаемого. 

При реализации класса `PagerAdapter` необходимо переопределить следующее:

- **Инстантиатеитем** &ndash; Создает страницу ( `View` ) для данной должности и добавляет ее в `ViewPager` коллекцию представлений. 

- **Дестройитем** &ndash; Удаляет страницу из заданной должности.

- **Количество** &ndash; Свойство только для чтения, возвращающее число доступных представлений (страниц). 

- **Исвиевфромобжект** &ndash; Определяет, связана ли страница с конкретным объектом ключа. (Этот объект создается `InstantiateItem` методом.) В этом примере объект ключа является `TreeCatalog` объектом данных.

Добавьте новый файл с именем **TreePagerAdapter.CS** и замените его содержимое следующим кодом: 

```csharp
using System;
using Android.App;
using Android.Runtime;
using Android.Content;
using Android.Views;
using Android.Widget;
using Android.Support.V4.View;
using Java.Lang;

namespace TreePager
{
    class TreePagerAdapter : PagerAdapter
    {
        public override int Count
        {
            get { throw new NotImplementedException(); }
        }

        public override bool IsViewFromObject(View view, Java.Lang.Object obj)
        {
            throw new NotImplementedException();
        }

        public override Java.Lang.Object InstantiateItem (View container, int position)
        {
            throw new NotImplementedException();
        }

        public override void DestroyItem(View container, int position, Java.Lang.Object view)
        {
            throw new NotImplementedException();
        }
    }
}
```

Этот код является заглушкой для основной `PagerAdapter` реализации. В следующих разделах каждый из этих методов заменяется рабочим кодом. 

### <a name="implement-the-constructor"></a>Реализация конструктора

Когда приложение создает экземпляр `TreePagerAdapter` , оно предоставляет контекст ( `MainActivity` ) и экземпляр `TreeCatalog` . Добавьте следующие переменные и конструктор членов `TreePagerAdapter` в начало класса в **TreePagerAdapter.CS**: 

```csharp
Context context;
TreeCatalog treeCatalog;

public TreePagerAdapter (Context context, TreeCatalog treeCatalog)
{
    this.context = context;
    this.treeCatalog = treeCatalog;
}
```

Этот конструктор предназначен для хранения контекста и `TreeCatalog` экземпляра, который `TreePagerAdapter` будет использоваться. 

### <a name="implement-count"></a>Число реализаций

`Count`Реализация относительно проста: она возвращает количество деревьев в каталоге дерева. Замените `Count` следующим кодом:

```csharp
public override int Count
{
    get { return treeCatalog.NumTrees; }
}
```

`NumTrees`Свойство объекта `TreeCatalog` возвращает количество деревьев (число страниц) в наборе данных.

### <a name="implement-instantiateitem"></a>Реализация Инстантиатеитем

`InstantiateItem`Метод создает страницу для заданной должности. Он также должен добавить вновь созданное представление в `ViewPager` коллекцию представлений. Чтобы сделать это возможным, `ViewPager` передается в качестве параметра контейнера. 

Замените метод `InstantiateItem` следующим кодом:

```csharp
public override Java.Lang.Object InstantiateItem (View container, int position)
{
    var imageView = new ImageView (context);
    imageView.SetImageResource (treeCatalog[position].imageId);
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.AddView (imageView);
    return imageView;
}
```

Этот код делает следующее:

1. Создает новый объект `ImageView` для вывода изображения дерева в указанной позиции. Приложение `MainActivity` является контекстом, который будет передан `ImageView` конструктору.

2. Задает `ImageView` ресурсу `TreeCatalog` идентификатор ресурса изображения в указанной позиции.

3. Приводит переданный контейнер `View` к `ViewPager` ссылке.
    Обратите внимание, что необходимо использовать `JavaCast<ViewPager>()` для правильного выполнения этого приведения (это необходимо для того, чтобы Android выполнял преобразование типов, проверяемое во время выполнения).

4. Добавляет экземпляр, созданный в `ImageView` , `ViewPager` и возвращает в `ImageView` вызывающий объект.

Когда `ViewPager` отображает изображение в `position` , оно отображается `ImageView` . Изначально `InstantiateItem` вызывается дважды для заполнения первых двух страниц с представлениями. По мере прокрутки пользователь вызывается снова, чтобы поддерживать представления сразу за пределами текущего отображаемого элемента. 

### <a name="implement-destroyitem"></a>Реализация Дестройитем

`DestroyItem`Метод удаляет страницу из заданной должности. В приложениях, в которых представление в любой заданной позиции может измениться, `ViewPager` необходимо удалить устаревшее представление в этой позиции, прежде чем заменить его новым представлением. В этом `TreeCatalog` примере представление в каждой позиции не изменяется, поэтому представление, удаленное с помощью, `DestroyItem` будет просто повторно Добавлено при `InstantiateItem` вызове для этой позиции. (Для повышения эффективности можно реализовать пул для перезапуска `View` , который будет повторно отображаться в той же позиции.) 

Замените метод `DestroyItem` следующим кодом: 

```csharp
public override void DestroyItem(View container, int position, Java.Lang.Object view)
{
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.RemoveView(view as View);
}
```

Этот код делает следующее:

1. Приводит переданный контейнер `View` в `ViewPager` ссылку.

2. Приводит переданный объект Java ( `view` ) к C# `View` ( `view as View` );

3. Удаляет представление из `ViewPager` . 

### <a name="implement-isviewfromobject"></a>Реализация Исвиевфромобжект

По мере того как пользователь просматривает слайды влево и вправо по страницам содержимого, `ViewPager` вызывает метод, `IsViewFromObject` чтобы убедиться, что дочерний `View` объект в данной позиции связан с объектом адаптера для той же позиции (следовательно, объект адаптера называется *ключом объекта*). Для относительно простых приложений Ассоциация — это одно из идентификаторов, которое &ndash; ключ объекта адаптера на этом экземпляре является представлением, которое ранее было возвращено в `ViewPager` Via `InstantiateItem` . Однако для других приложений ключ объекта может быть другим экземпляром класса адаптера, связанным с (но не аналогичным) дочерним представлением, которое `ViewPager` отображается в этой позиции. Только адаптер знает, связаны ли переданные представления и ключ объекта. 

`IsViewFromObject` для правильной работы должен быть реализован `PagerAdapter` . Если `IsViewFromObject` возвращает `false` для данной позиции, `ViewPager` представление в этой позиции не отображается. В `TreePager` приложении ключ объекта, возвращаемый, `InstantiateItem` *является* страницей `View` дерева, поэтому код должен проверять только удостоверение (т. е. ключ объекта и представление являются одним и тем же). Замените `IsViewFromObject` следующим кодом: 

```csharp
public override bool IsViewFromObject(View view, Java.Lang.Object obj)
{
    return view == obj;
}
```

## <a name="add-the-adapter-to-the-viewpager"></a>Добавление адаптера в ViewPager

Теперь, когда `TreePagerAdapter` реализуется, пришло время добавить его в `ViewPager` . В **MainActivity.CS**добавьте следующую строку кода в конец `OnCreate` метода:

```csharp
viewPager.Adapter = new TreePagerAdapter(this, treeCatalog);
```

Этот код создает экземпляр `TreePagerAdapter` , передавая в `MainActivity` качестве контекста ( `this` ). Экземпляр `TreeCatalog` передается в второй аргумент конструктора. `ViewPager` `Adapter` Свойству присвоено значение, установленное в экземпляре `TreePagerAdapter` объекта; в этот объект подключается к `TreePagerAdapter` `ViewPager` . 

Основная реализация теперь завершает &ndash; сборку и запускает приложение. На экране появится первое изображение каталога дерева, как показано на рисунке слева на следующем снимке экрана. Проведите влево, чтобы увидеть другие представления в виде дерева, а затем проведите вправо для перемещения назад по каталогу дерева: 

[![Снимки экрана Трипажер прокрутки приложений с помощью изображений в виде дерева](viewpager-and-views-images/03-example-views-sml.png)](viewpager-and-views-images/03-example-views.png#lightbox)

## <a name="add-a-pager-indicator"></a>Добавление индикатора страничного навигатора

Эта минимальная `ViewPager` Реализация отображает изображения каталога дерева, но не указывает, где находится пользователь в каталоге. Следующим шагом является добавление `PagerTabStrip` . `PagerTabStrip`Информирует пользователя о том, какая страница отображается, и предоставляет контекст навигации, отображая указание предыдущей и следующей страниц. `PagerTabStrip` предназначен для использования в качестве индикатора текущей страницы a `ViewPager` ; прокручивается и обновляется по мере того, как пользователь просматривает каждую страницу. 

Откройте **ресурсы/макет/Main. axml** и добавьте в `PagerTabStrip` Макет:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    <android.support.v4.view.PagerTabStrip
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:layout_gravity="top"
          android:paddingBottom="10dp"
          android:paddingTop="10dp"
          android:textColor="#fff" />

</android.support.v4.view.ViewPager>
```

`ViewPager` и `PagerTabStrip` предназначены для совместной работы. При объявлении `PagerTabStrip` внутри макета компонент `ViewPager` `ViewPager` автоматически находит `PagerTabStrip` и подключает его к адаптеру. При сборке и запуске приложения в верхней части каждого экрана должно отобразиться пустое поле `PagerTabStrip` : 

[![Снимок экрана крупный план пустого Пажертабстрип](viewpager-and-views-images/04-empty-pagetabstrip-cap-sml.png)](viewpager-and-views-images/04-empty-pagetabstrip-cap.png#lightbox)

### <a name="display-a-title"></a>Отображение заголовка

Чтобы добавить заголовок на каждую вкладку страницы, реализуйте `GetPageTitleFormatted` метод в классе, `PagerAdapter` производном от. `ViewPager` вызывает метод `GetPageTitleFormatted` (Если реализован), чтобы получить строку заголовка, описывающую страницу в указанной позиции. Добавьте следующий метод в `TreePagerAdapter` класс в **TreePagerAdapter.CS**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String(treeCatalog[position].caption);
}
```

Этот код извлекает строку заголовка дерева из указанной страницы (расположения) в каталоге дерева, преобразует ее в Java `String` и возвращает в `ViewPager` . При запуске приложения с помощью этого нового метода на каждой странице отображается заголовок дерева в `PagerTabStrip` . Вы увидите имя дерева в верхней части экрана без подчеркивания: 

[![Снимки экрана страниц с заполненными текстом Пажертабстрип табуляции](viewpager-and-views-images/05-final-pagetabstrip-sml.png)](viewpager-and-views-images/05-final-pagetabstrip.png#lightbox)

Для просмотра каждого изображения в виде дерева в каталоге можно прокрутить вперед и назад. 

### <a name="pagertitlestrip-variation"></a>Пажертитлестрип

`PagerTitleStrip` очень похож на `PagerTabStrip` , за исключением того, что `PagerTabStrip` добавляет подчеркивание для текущей выбранной вкладки. Вы можете заменить `PagerTabStrip` на `PagerTitleStrip` в приведенном выше макете и запустить приложение еще раз, чтобы увидеть, как оно выглядит `PagerTitleStrip` : 

[![Пажертитлестрип с подчеркиванием, удаленным из текста](viewpager-and-views-images/06-pagetitlestrip-example-sml.png)](viewpager-and-views-images/06-pagetitlestrip-example.png#lightbox)

Обратите внимание, что подчеркивание удаляется при преобразовании в `PagerTitleStrip` . 

## <a name="summary"></a>Сводка

В этом пошаговом руководстве представлен пошаговый пример создания базового `ViewPager` приложения без использования `Fragment` s. В нем представлен пример источника данных, содержащий изображения и строки заголовков, `ViewPager` макет для отображения изображений и `PagerAdapter` подкласс, который подключается `ViewPager` к источнику данных. Чтобы помочь пользователю перемещаться по набору данных, были добавлены инструкции, объясняющие, как добавить `PagerTabStrip` или `PagerTitleStrip` отобразить заголовок изображения в верхней части каждой страницы. 

## <a name="related-links"></a>Связанные ссылки

- [Трипажер (пример)](/samples/xamarin/monodroid-samples/userinterface-treepager)