---
title: Пример базового RecyclerView
description: Пример приложения, демонстрирующий использование RecyclerView.
ms.prod: xamarin
ms.assetid: A50520D2-1214-40E1-9B27-B0891FE11584
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/30/2018
ms.openlocfilehash: d4a775d1e96fd6650623c2a151ae74b8b68ce0ac
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457085"
---
# <a name="a-basic-recyclerview-example"></a>Пример базового RecyclerView

Чтобы понять, как `RecyclerView` работает типичное приложение, в этом разделе рассматривается пример приложения [рециклервиевер](/samples/xamarin/monodroid-samples/android50-recyclerviewer) — простой пример кода, который используется `RecyclerView` для отображения большой коллекции фотографий: 

[![Два снимка экрана приложения RecyclerView, использующего Кардвиевс для показа фотографий](recyclerview-example-images/01-recyclerviewer-sml.png)](recyclerview-example-images/01-recyclerviewer.png#lightbox)

**Рециклервиевер** использует [кардвиев](~/android/user-interface/controls/card-view.md) для реализации каждого элемента фотографии в `RecyclerView` макете. Из `RecyclerView` -за преимуществ производительности этот пример приложения может быстро прокручивать большую коллекцию фотографий без заметных задержек.

### <a name="an-example-data-source"></a>Пример источника данных

В этом примере приложения источник данных «фотоальбом» (представленный `PhotoAlbum` классом) предоставляет `RecyclerView` содержимое элемента.
`PhotoAlbum` — Это коллекция фотографий с заголовками; При создании экземпляра вы получаете готовый набор из 32 фотографий:

```csharp
PhotoAlbum mPhotoAlbum = new PhotoAlbum ();
```

Каждый экземпляр Photo в `PhotoAlbum` предоставляет свойства, позволяющие СЧИТЫВАТЬ идентификатор ресурса изображения, `PhotoID` и строку заголовка `Caption` . Коллекция фотографий организована таким же путем, что индексатор может получить доступ к каждой фотографии. Например, следующие строки кода обращаются к ИДЕНТИФИКАТОРу ресурса изображения и заголовку для десятой фотографии в коллекции:

```csharp
int imageId = mPhotoAlbum[9].ImageId;
string caption = mPhotoAlbum[9].Caption;
```

`PhotoAlbum` также предоставляет `RandomSwap` метод, который можно вызвать для переключения первого фото в коллекции на случайное значение, которое еще не выбрано в коллекции.

```csharp
mPhotoAlbum.RandomSwap ();
```

Поскольку сведения о реализации `PhotoAlbum` не важны для понимания `RecyclerView` , `PhotoAlbum` Исходный код здесь не представлен. Исходный код `PhotoAlbum` доступен по адресу [PhotoAlbum.CS](https://github.com/xamarin/monodroid-samples/blob/master/android5.0/RecyclerViewer/RecyclerViewer/PhotoAlbum.cs) в примере приложения [рециклервиевер](/samples/xamarin/monodroid-samples/android50-recyclerviewer) .

### <a name="layout-and-initialization"></a>Макет и инициализация

Файл макета **Main. axml**состоит из одного элемента `RecyclerView` в `LinearLayout` :

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:scrollbars="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
</LinearLayout>
```

Обратите внимание, что необходимо использовать полное имя **Android. support. версии 7. widget. RecyclerView** , поскольку `RecyclerView` упаковано в библиотеку поддержки. `OnCreate`Метод `MainActivity` Инициализирует этот макет, создает экземпляр адаптера и подготавливает базовый источник данных:

```csharp
public class MainActivity : Activity
{
    RecyclerView mRecyclerView;
    RecyclerView.LayoutManager mLayoutManager;
    PhotoAlbumAdapter mAdapter;
    PhotoAlbum mPhotoAlbum;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Prepare the data source:
        mPhotoAlbum = new PhotoAlbum ();

        // Instantiate the adapter and pass in its data source:
        mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);

        // Set our view from the "main" layout resource:
        SetContentView (Resource.Layout.Main);

        // Get our RecyclerView layout:
        mRecyclerView = FindViewById<RecyclerView> (Resource.Id.recyclerView);

        // Plug the adapter into the RecyclerView:
        mRecyclerView.SetAdapter (mAdapter);
```

Этот код делает следующее:

1. Создает экземпляр `PhotoAlbum` источника данных.

2. Передает источник данных фотоальбома в конструктор адаптера `PhotoAlbumAdapter` (который определяется далее в этом пошаговом окне). 
   Обратите внимание, что рекомендуется передать источник данных в качестве параметра в конструктор адаптера. 

3. Возвращает объект `RecyclerView` из макета.

4. Подключает адаптер к `RecyclerView` экземпляру, вызвав метод, `RecyclerView` `SetAdapter` как показано выше.

### <a name="layout-manager"></a>Диспетчер макетов

Каждый элемент в `RecyclerView` состоит из элемента `CardView` , содержащего фотографию и подпись фотографии (подробные сведения см. в разделе «географические [заполнитель»](#view-holder) ). Предопределенный параметр `LinearLayoutManager` используется для размещения каждого `CardView` элемента в вертикальной прокрутке.

```csharp
mLayoutManager = new LinearLayoutManager (this);
mRecyclerView.SetLayoutManager (mLayoutManager);
```

Этот код находится в `OnCreate` методе основного действия. Конструктору диспетчера макетов требуется *контекст*, поэтому он `MainActivity` передается с помощью, `this` как показано выше.

Вместо использования предопределенного компонента `LinearLayoutManager` можно подключить пользовательский диспетчер макетов, отображающий два `CardView` элемента параллельно, реализуя эффект анимации для переворачивания страниц для прохода по коллекции фотографий. Далее в этом пошаговом окне вы увидите пример изменения макета, переключаясь в другой диспетчер макетов.

<a name="view-holder"></a>

### <a name="view-holder"></a>Просмотреть держатель

Вызывается класс-держатель представления `PhotoViewHolder` . Каждый `PhotoViewHolder` экземпляр содержит ссылки на `ImageView` и `TextView` связанного элемента строки, который располагается в `CardView` пояснены здесь:

[![Схема Кардвиев, содержащая Имажевиев и TextView](recyclerview-example-images/02-cardview-layout-sml.png)](recyclerview-example-images/02-cardview-layout.png#lightbox)

`PhotoViewHolder` является производным от `RecyclerView.ViewHolder` и содержит свойства для хранения ссылок на объект `ImageView` и, `TextView` показанный в приведенном выше макете.
`PhotoViewHolder` состоит из двух свойств и одного конструктора:

```csharp
public class PhotoViewHolder : RecyclerView.ViewHolder
{
    public ImageView Image { get; private set; }
    public TextView Caption { get; private set; }

    public PhotoViewHolder (View itemView) : base (itemView)
    {
        // Locate and cache view references:
        Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
        Caption = itemView.FindViewById<TextView> (Resource.Id.textView);
    }
}
```

В этом примере кода `PhotoViewHolder` конструктору передается ссылка на представление родительского элемента ( `CardView` ), которое является `PhotoViewHolder` оболочкой. Обратите внимание, что вы всегда пересылаете представление родительского элемента в базовый конструктор. `PhotoViewHolder`Конструктор вызывает `FindViewById` в представлении родительского элемента, чтобы нахождение всех ссылок на дочернее представление, `ImageView` и `TextView` , сохраняя результаты в `Image` `Caption` свойствах и соответственно. Адаптер позже извлекает ссылки на представления из этих свойств, когда обновляет эти `CardView` дочерние представления новыми данными.

Дополнительные сведения о `RecyclerView.ViewHolder` см. в [справочнике по классам RecyclerView. виевхолдер](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).

### <a name="adapter"></a>Адаптер

Адаптер загружает каждую `RecyclerView` строку с данными для конкретной фотографии. Например, для данной фотографии в позиции строки *p*адаптер находит связанные данные в позиции *p* в источнике данных и копирует эти данные в элемент строки в позиции *p* в `RecyclerView` коллекции. Адаптер использует держатель представления для поиска ссылок на `ImageView` и `TextView` в этой позиции, поэтому ему не нужно многократно вызывать `FindViewById` эти представления, так как пользователь прокручивает коллекцию фотографий и повторно использует представления.

В **рециклервиевер**класс адаптера является производным от класса `RecyclerView.Adapter` для создания `PhotoAlbumAdapter` :

```csharp
public class PhotoAlbumAdapter : RecyclerView.Adapter
{
    public PhotoAlbum mPhotoAlbum;

    public PhotoAlbumAdapter (PhotoAlbum photoAlbum)
    {
        mPhotoAlbum = photoAlbum;
    }
    ...
}
```

`mPhotoAlbum`Элемент содержит источник данных (фотоальбом), который передается в конструктор; конструктор копирует фотоальбом в эту переменную-член. Реализуются следующие необходимые `RecyclerView.Adapter` методы:

- **`OnCreateViewHolder`**&ndash;Создает экземпляр файла макета элемента и владельца представления.

- **`OnBindViewHolder`**&ndash;Загружает данные в указанной позиции в представления, ссылки на которые хранятся в заданном заполнителе представления.

- **`ItemCount`**&ndash;Возвращает число элементов в источнике данных.

Диспетчер макетов вызывает эти методы при размещении элементов в `RecyclerView` . Реализация этих методов рассматривается в следующих разделах.

#### <a name="oncreateviewholder"></a>онкреатевиевхолдер

Диспетчер макетов вызывает `OnCreateViewHolder` , когда `RecyclerView` для представления элемента требуется новый владелец представления. `OnCreateViewHolder` увеличивает представление элемента из файла макета представления и заключает представление в новый `PhotoViewHolder` экземпляр. `PhotoViewHolder`Конструктор находит и сохраняет ссылки на дочерние представления в макете, как описано ранее в [заполнителье представления](#view-holder).

Каждый элемент строки представлен объектом `CardView` , который содержит `ImageView` (для фотографии) и `TextView` (для заголовка). Этот макет находится в файле **фотокардвиев. axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:card_view="http://schemas.android.com/apk/res-auto"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    <android.support.v7.widget.CardView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        card_view:cardElevation="4dp"
        card_view:cardUseCompatPadding="true"
        card_view:cardCornerRadius="5dp">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:padding="8dp">
            <ImageView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:id="@+id/imageView"
                android:scaleType="centerCrop" />
            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:textAppearance="?android:attr/textAppearanceMedium"
                android:textColor="#333333"
                android:text="Caption"
                android:id="@+id/textView"
                android:layout_gravity="center_horizontal"
                android:layout_marginLeft="4dp" />
        </LinearLayout>
    </android.support.v7.widget.CardView>
</FrameLayout>
```

Этот макет представляет один элемент строки в `RecyclerView` . `OnBindViewHolder`Метод (описан ниже) копирует данные из источника данных в `ImageView` и в `TextView` этом макете.
`OnCreateViewHolder` увеличивает этот макет для заданного расположения фотографии в `RecyclerView` и создает новый `PhotoViewHolder` экземпляр (который находит и кэширует ссылки на `ImageView` и `TextView` дочерние представления в связанном `CardView` макете):

```csharp
public override RecyclerView.ViewHolder
    OnCreateViewHolder (ViewGroup parent, int viewType)
{
    // Inflate the CardView for the photo:
    View itemView = LayoutInflater.From (parent.Context).
                Inflate (Resource.Layout.PhotoCardView, parent, false);

    // Create a ViewHolder to hold view references inside the CardView:
    PhotoViewHolder vh = new PhotoViewHolder (itemView);
    return vh;
}

```

Полученный экземпляр заполнителя представления `vh` возвращается к вызывающему объекту (диспетчеру макетов).

#### <a name="onbindviewholder"></a>онбиндвиевхолдер

Когда Диспетчер макетов готов к отображению определенного представления в `RecyclerView` видимой области экрана, он вызывает `OnBindViewHolder` метод адаптера для заполнения элемента в указанной позиции строки содержимым из источника данных. `OnBindViewHolder` Получает сведения о фотографии для указанной позицией строки (ресурса изображения фотографии и строки для подписи фотографии) и копирует эти данные в связанные представления. Представления размещаются через ссылки, хранящиеся в объекте-владельцу представления (который передается через `holder` параметр):

```csharp
public override void
    OnBindViewHolder (RecyclerView.ViewHolder holder, int position)
{
    PhotoViewHolder vh = holder as PhotoViewHolder;

    // Load the photo image resource from the photo album:
    vh.Image.SetImageResource (mPhotoAlbum[position].PhotoID);

    // Load the photo caption from the photo album:
    vh.Caption.Text = mPhotoAlbum[position].Caption;
}
```

Переданный объект-владелец представления сначала должен быть приведен к производному типу заполнителя представления (в данном случае `PhotoViewHolder` ) перед его использованием.
Адаптер загружает ресурс изображения в представление, на которое ссылается свойство держателя представления `Image` , и копирует текст заголовка в представление, на которое ссылается свойство держателя представления `Caption` . Это *привязывает* связанное представление к его данным.

Обратите внимание, что `OnBindViewHolder` — это код, который напрямую работает со структурой данных. В этом случае `OnBindViewHolder` понимает, как сопоставить `RecyclerView` позицию элемента со связанным элементом данных в источнике данных. В этом случае сопоставление является простым, так как позиция может использоваться в качестве индекса массива в фотоальбоме. Однако более сложные источники данных могут потребовать дополнительного кода для установления такого сопоставления.

#### <a name="itemcount"></a>ItemCount

`ItemCount`Метод возвращает количество элементов в коллекции данных. В примере приложения Photo Viewer число элементов равно числу фотографий в фотоальбоме:

```csharp
public override int ItemCount
{
    get { return mPhotoAlbum.NumPhotos; }
}
```

Дополнительные сведения о `RecyclerView.Adapter` см. в [справочнике по классам RecyclerView. Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).

### <a name="putting-it-all-together"></a>Совместное размещение

Результирующая `RecyclerView` реализация в примере Photo App состоит из `MainActivity` кода, который создает источник данных, Диспетчер макетов и адаптер. `MainActivity` создает `mRecyclerView` экземпляр источника данных и адаптер и подключает его к диспетчеру макета и адаптеру:

```csharp
public class MainActivity : Activity
{
    RecyclerView mRecyclerView;
    RecyclerView.LayoutManager mLayoutManager;
    PhotoAlbumAdapter mAdapter;
    PhotoAlbum mPhotoAlbum;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        mPhotoAlbum = new PhotoAlbum();
        SetContentView (Resource.Layout.Main);
        mRecyclerView = FindViewById<RecyclerView> (Resource.Id.recyclerView);

        // Plug in the linear layout manager:
        mLayoutManager = new LinearLayoutManager (this);
        mRecyclerView.SetLayoutManager (mLayoutManager);

        // Plug in my adapter:
        mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
        mRecyclerView.SetAdapter (mAdapter);
    }
}

```

`PhotoViewHolder` находит и кэширует ссылки на представление:

```csharp
public class PhotoViewHolder : RecyclerView.ViewHolder
{
    public ImageView Image { get; private set; }
    public TextView Caption { get; private set; }

    public PhotoViewHolder (View itemView) : base (itemView)
    {
        // Locate and cache view references:
        Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
        Caption = itemView.FindViewById<TextView> (Resource.Id.textView);
    }
}
```

`PhotoAlbumAdapter` реализует три переопределенных метода.

```csharp
public class PhotoAlbumAdapter : RecyclerView.Adapter
{
    public PhotoAlbum mPhotoAlbum;
    public PhotoAlbumAdapter (PhotoAlbum photoAlbum)
    {
        mPhotoAlbum = photoAlbum;
    }

    public override RecyclerView.ViewHolder
        OnCreateViewHolder (ViewGroup parent, int viewType)
    {
        View itemView = LayoutInflater.From (parent.Context).
                    Inflate (Resource.Layout.PhotoCardView, parent, false);
        PhotoViewHolder vh = new PhotoViewHolder (itemView);
        return vh;
    }

    public override void
        OnBindViewHolder (RecyclerView.ViewHolder holder, int position)
    {
        PhotoViewHolder vh = holder as PhotoViewHolder;
        vh.Image.SetImageResource (mPhotoAlbum[position].PhotoID);
        vh.Caption.Text = mPhotoAlbum[position].Caption;
    }

    public override int ItemCount
    {
        get { return mPhotoAlbum.NumPhotos; }
    }
}
```

При компиляции и выполнении этого кода создается основное приложение для просмотра фотографий, как показано на следующих снимках экрана:

[![Два снимка экрана приложения для просмотра фотографий с вертикальной прокруткой карт фото](recyclerview-example-images/03-recyclerviewer-basic-sml.png)](recyclerview-example-images/03-recyclerviewer-basic.png#lightbox)

Если тени не рисуются (как показано на снимке экрана выше), измените **Свойства/AndroidManifest.xml** и добавьте следующий параметр атрибута в `<application>` элемент:

```xml
android:hardwareAccelerated="true"
```

Это базовое приложение поддерживает только обзор фотоальбома. Он не реагирует на события касания элемента и не обрабатывает изменения в базовых данных. Эта функция добавлена в [примере расширения RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md).

### <a name="changing-the-layoutmanager"></a>Изменение Лайаутманажер

Из-за `RecyclerView` гибкости можно легко изменить приложение, чтобы использовать другой диспетчер макетов. В следующем примере он изменяется для отображения фотоальбома с макетом сетки, который прокручивается горизонтально, а не с вертикальным линейным макетом. Для этого экземпляр диспетчера макетов изменяется для использования `GridLayoutManager` следующим образом:

```csharp
mLayoutManager = new GridLayoutManager(this, 2, GridLayoutManager.Horizontal, false);
```

Это изменение кода заменяет вертикальный на `LinearLayoutManager` `GridLayoutManager` , который представляет сетку, которая состоит из двух строк, прокрутхся по горизонтали. При повторной компиляции и запуске приложения вы увидите, что фотографии отображаются в сетке, а прокрутка — горизонтальная, а не вертикальная:

[![Пример снимка экрана приложения с горизонтальной прокруткой фотографий в сетке](recyclerview-example-images/04-gridlayoutmanager-sml.png)](recyclerview-example-images/04-gridlayoutmanager.png#lightbox)

Изменяя только одну строку кода, можно изменить приложение для просмотра фотографий, чтобы использовать другой макет с другим поведением.
Обратите внимание, что ни код адаптера, ни XML макета не были изменены для изменения стиля макета. 

В следующем разделе, посвященном расширению [примера RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md), этот базовый пример приложения расширяется для обработки событий щелчка элемента и обновления `RecyclerView` при изменении базового источника данных.

## <a name="related-links"></a>Связанные ссылки

- [Рециклервиевер (пример)](/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView части и функциональные возможности](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Расширение примера RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)