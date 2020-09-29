---
title: Настройка вида ListView
ms.prod: xamarin
ms.assetid: B09AD282-2C4F-D71E-6806-9B1EF05C2CD4
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/26/2018
ms.openlocfilehash: 41b4bb25aa170abeb58a92058f4bfb8b78af35ad
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457241"
---
# <a name="customizing-a-listviews-appearance-with-xamarinandroid"></a>Настройка внешнего вида ListView с помощью Xamarin. Android

Внешний вид ListView определяется макетом отображаемых строк. Чтобы изменить внешний вид `ListView` , используйте другой макет строки.

## <a name="built-in-row-views"></a>Встроенные представления строк

Существует 12 встроенных представлений, на которые можно ссылаться с помощью **Android. resource. Layout**:

- **Тестлиститем** &ndash; Одна строка текста с минимальным форматированием.

- **SimpleListItem1** &ndash; Одна строка текста.

- **SimpleListItem2** &ndash; Две строки текста.

- **Симплеселектаблелиститем** &ndash; Однострочный текст, который поддерживает выбор одного или нескольких элементов (добавляется на уровне API 11).

- **SimpleListItemActivated1** &ndash; Аналогично SimpleListItem1, но цвет фона показывает, что строка выбрана (добавляется на уровне API 11).

- **SimpleListItemActivated2** &ndash; Аналогично SimpleListItem2, но цвет фона показывает, что строка выбрана (добавляется на уровне API 11).

- **Симплелиститемчеккед** &ndash; Отображает флажки, указывающие на выбор.

- **Симплелиститеммултиплечоице** &ndash; Отображает флажки, указывающие выбор нескольких вариантов.

- **Симплелиститемсинглечоице** &ndash; Отображает переключатели для обозначения взаимно исключающего выбора.

- **Тволинелиститем** &ndash; Две строки текста.

- **Активитилиститем** &ndash; Одна строка текста с изображением.

- **Симпликспандаблелиститем** &ndash; Группирует строки по категориям, и каждая группа может быть развернута или свернута.

Каждое встроенное представление строки имеет связанный с ним встроенный стиль. На этих снимках экрана показано, как выглядит каждое представление:

[![Снимки экрана Тестлиститем, Симплеселектаблелиститем, SimpleListitem1 и SimpleListItem2](customizing-appearance-images/builtinviews.png)](customizing-appearance-images/builtinviews.png#lightbox)

[![Снимки экрана SimpleListItemActivated1, SimpleListItemActivated2, Симплелиститемчеккед и Симплелиститеммултиплечеккед](customizing-appearance-images/builtinviews-2.png)](customizing-appearance-images/builtinviews-2.png#lightbox)

[![Снимки экрана Симплелиститемсинглечоице, Тволинелиститем, Активитилиститем и Симпликспандаблелиститем](customizing-appearance-images/builtinviews-3.png)](customizing-appearance-images/builtinviews-3.png#lightbox)

Пример файла **буилтинвиевс/хомескринадаптер. CS** (в решении **буилтинвиевс** ) содержит код для создания экранов нерасширяемого элемента списка. Представление задается в `GetView` методе следующим образом:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
```

Затем можно задать свойства представления, ссылаясь на стандартные идентификаторы элементов управления `Text1` `Text2` и `Icon` в разделе `Android.Resource.Id` (не задавать свойства, которые не содержит представление, или будет создано исключение):

```csharp
view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = item.Heading;
view.FindViewById<TextView>(Android.Resource.Id.Text2).Text = item.SubHeading;
view.FindViewById<ImageView>(Android.Resource.Id.Icon).SetImageResource(item.ImageResourceId); // only use with ActivityListItem
```

Пример файла **буилтинекспандаблевиевс/експандаблескринадаптер. CS** (в решении **буилтинвиевс** ) содержит код для создания экрана симпликспандаблелиститем. Представление группы задается в `GetGroupView` методе следующим образом:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem1, null);
```

Дочернее представление задается в `GetChildView` методе следующим образом:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem2, null);
```

Затем можно задать свойства представления группы и дочернего представления, ссылаясь на стандартные `Text1` `Text2` идентификаторы и элементы управления, как показано выше. Снимок экрана Симпликспандаблелиститем (показанный выше) содержит пример однострочного представления группы (SimpleExpandableListItem1) и двустороннего дочернего представления (SimpleExpandableListItem2). Кроме того, представление группы может быть настроено для двух строк (SimpleExpandableListItem2), а дочернее представление может быть настроено для одной строки (SimpleExpandableListItem1) либо как представление группы, так и дочернее представление могут иметь одинаковое число строк. 

## <a name="accessories"></a>Accessories

Строки могут содержать аксессуары, добавленные в область справа от представления, чтобы указать состояние выбора:

- **Симплелиститемчеккед** &ndash; Создает список с одним выбором с галочкой в качестве индикатора.

- **Симплелиститемсинглечоице** &ndash; Создает списки переключателей, в которых возможен только один вариант.

- **Симплелиститеммултиплечоице** &ndash; Создает списки типов CheckBox, где возможны несколько вариантов.

Вышеупомянутые аксессуары показаны на следующих экранах в соответствующем порядке:

[![Снимки экрана Симплелиститемчеккед, Симплелиститемсинглечоице и Симплелиститеммултиплечоице с аксессуарами](customizing-appearance-images/accessories.png)](customizing-appearance-images/accessories.png#lightbox)

Чтобы отобразить одно из этих аксессуаров, передайте требуемый идентификатор ресурса макета адаптеру, а затем вручную задайте для них состояние выбора. В этой строке кода показано, как создать и назначить `Adapter` с помощью одного из следующих макетов:

```csharp
ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItemChecked, items);
```

`ListView`Сам по себе поддерживает различные режимы выбора, независимо от отображаемой принадлежности. Чтобы избежать путаницы, используйте `Single` режим выделения с `SingleChoice` аксессуарами `Checked` и `Multiple` режимом или с использованием `MultipleChoice` стиля. Режим выбора управляется `ChoiceMode` свойством объекта `ListView` .

### <a name="handling-api-level"></a>Обработка уровня API

Более ранние версии Xamarin. Android реализовали перечисления как целочисленные свойства. Последняя версия предоставила правильные типы перечисления .NET, что значительно упрощает обнаружение возможных вариантов.

В зависимости от уровня API, на который вы нацелены, `ChoiceMode` это либо целое число, либо перечисление. Пример файла **акцессоривиевс/хомескрин. CS** содержит блок комментария, если вы хотите ориентироваться на API Gingerbread:

```csharp
// For targeting Gingerbread the ChoiceMode is an int, otherwise it is an
// enumeration.

lv.ChoiceMode = Android.Widget.ChoiceMode.Single; // 1
//lv.ChoiceMode = Android.Widget.ChoiceMode.Multiple; // 2
//lv.ChoiceMode = Android.Widget.ChoiceMode.None; // 0

// Use this block if targeting Gingerbread or lower
/*
lv.ChoiceMode = 1; // Single
//lv.ChoiceMode = 0; // none
//lv.ChoiceMode = 2; // Multiple
//lv.ChoiceMode = 3; // MultipleModal
*/
```

### <a name="selecting-items-programmatically"></a>Выбор элементов программным способом

Ручная установка выбранных элементов выполняется с помощью `SetItemChecked` метода (его можно вызывать несколько раз для множественного выбора):

```csharp
// Set the initially checked row ("Fruits")
lv.SetItemChecked(1, true);
```

Код также должен определять один выбор по-разному из нескольких вариантов выбора. Чтобы определить, какая строка была выбрана в `Single` режиме, используйте `CheckedItemPosition` свойство целое число:

```csharp
FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPosition
```

Чтобы определить, какие строки были выбраны в `Multiple` режиме, необходимо перебрать `CheckedItemPositions` `SparseBooleanArray` . Разреженный массив подобен словарю, который содержит только записи, в которых значение было изменено, поэтому необходимо пройти весь массив для поиска значений, `true` чтобы узнать, что было выбрано в списке, как показано в следующем фрагменте кода:

```csharp
var sparseArray = FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPositions;
for (var i = 0; i < sparseArray.Size(); i++ )
{
   Console.Write(sparseArray.KeyAt(i) + "=" + sparseArray.ValueAt(i) + ",");
}
Console.WriteLine();
```

## <a name="creating-custom-row-layouts"></a>Создание пользовательских макетов строк

Четыре встроенных представления строк очень просты. Чтобы отобразить более сложные макеты (например, список сообщений электронной почты, твитов или контактных сведений), требуется пользовательское представление. Пользовательские представления обычно объявляются в виде файлов AXML в каталоге **ресурсов или макета** , а затем загружаются с помощью настраиваемого адаптера с использованием идентификатора ресурса. Представление может содержать любое количество классов отображения (например, Текствиевс, Имажевиевс и других элементов управления) с пользовательскими цветами, шрифтами и макетом.

Этот пример отличается от предыдущих примеров несколькими способами.

- Наследует от `Activity` , а не от `ListActivity` . Можно настроить строки для любого `ListView` , однако другие элементы управления также можно включать в `Activity` Макет (например, в заголовок, кнопки или другие элементы пользовательского интерфейса). В этом примере для иллюстрации добавляется заголовок выше `ListView` .

- Требуется файл макета AXML для экрана; в предыдущих примерах `ListActivity` файл макета не требуется. Этот AXML содержит `ListView` объявление элемента управления.

- Для отображения каждой строки требуется файл макета AXML. Этот файл AXML содержит элементы управления "текст" и "изображение" с пользовательскими настройками шрифта и цвета.

- Использует необязательный XML-файл настраиваемого селектора для установки внешнего вида строки, если она выбрана.

- `Adapter`Реализация возвращает пользовательский макет из `GetView` переопределения.

- `ItemClick` должен быть объявлен по-другому (обработчик событий присоединяется к, `ListView.ItemClick` а не переопределяется  `OnListItemClick` в `ListActivity` ).

Эти изменения подробно описаны ниже, начиная с создания представления действия и настраиваемого представления строк, а затем покрывающие изменения адаптера и действия для их подготовки к просмотру.

### <a name="adding-a-listview-to-an-activity-layout"></a>Добавление ListView к макету действия

Поскольку `HomeScreen` больше не наследуется от `ListActivity` него, он не имеет представления по умолчанию, поэтому для представления хомескрин должен быть создан файл макета AXML. В этом примере представление будет иметь заголовок (с использованием `TextView` ) и `ListView` для отображения данных. Макет определяется в файле **Resources/Layout/хомескрин. axml** , который показан здесь:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   android:orientation="vertical"
   android:layout_width="fill_parent"
   android:layout_height="fill_parent">
    <TextView android:id="@+id/Heading"
        android:text="Vegetable Groups"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:background="#00000000"
        android:textSize="30dp"
        android:textColor="#FF267F00"
        android:textStyle="bold"
        android:padding="5dp"
    />
    <ListView android:id="@+id/List"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:cacheColorHint="#FFDAFF7F"
    />
</LinearLayout>
```

Преимущество использования `Activity` с пользовательским макетом (вместо `ListActivity` ) заключается в возможности добавлять на экран дополнительные элементы управления, такие как заголовок `TextView` в этом примере.

### <a name="creating-a-custom-row-layout"></a>Создание пользовательского макета строки

Другой файл макета AXML должен содержать пользовательский макет для каждой строки, которая будет отображаться в представлении списка. В этом примере строка будет иметь зеленый фон, коричневый текст и изображение, выходящее по правому краю. XML-разметка Android для объявления этого макета описана в разделе **Resources/Layout/кустомвиев. axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout  xmlns:android="http://schemas.android.com/apk/res/android"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
   android:background="#FFDAFF7F"
   android:padding="8dp">
    <LinearLayout android:id="@+id/Text"
       android:orientation="vertical"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:paddingLeft="10dip">
        <TextView
         android:id="@+id/Text1"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:textColor="#FF7F3300"
         android:textSize="20dip"
         android:textStyle="italic"
         />
        <TextView
         android:id="@+id/Text2"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:textSize="14dip"
         android:textColor="#FF267F00"
         android:paddingLeft="100dip"
         />
    </LinearLayout>
    <ImageView
        android:id="@+id/Image"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:padding="5dp"
        android:src="@drawable/icon"
        android:layout_alignParentRight="true" />
</RelativeLayout >
```

Хотя пользовательский макет строки может содержать множество различных элементов управления, производительность прокрутки может зависеть от сложных макетов и использования изображений (особенно если они должны загружаться по сети). Дополнительные сведения об устранении проблем с производительностью см. в статье Google.

### <a name="referencing-a-custom-row-view"></a>Ссылка на представление настраиваемой строки

Пример пользовательского адаптера реализован в `HomeScreenAdapter.cs` . Ключевой метод — это `GetView` место, где он загружает пользовательский AXML с помощью идентификатора ресурса `Resource.Layout.CustomView` , а затем задает свойства каждого элемента управления в представлении перед его возвратом. Ниже показан полный класс адаптера.

```csharp
public class HomeScreenAdapter : BaseAdapter<TableItem> {
   List<TableItem> items;
   Activity context;
   public HomeScreenAdapter(Activity context, List<TableItem> items)
       : base()
   {
       this.context = context;
       this.items = items;
   }
   public override long GetItemId(int position)
   {
       return position;
   }
   public override TableItem this[int position]
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
           view = context.LayoutInflater.Inflate(Resource.Layout.CustomView, null);
       view.FindViewById<TextView>(Resource.Id.Text1).Text = item.Heading;
       view.FindViewById<TextView>(Resource.Id.Text2).Text = item.SubHeading;
       view.FindViewById<ImageView>(Resource.Id.Image).SetImageResource(item.ImageResourceId);
       return view;
   }
}
```

### <a name="referencing-the-custom-listview-in-the-activity"></a>Ссылка на пользовательский элемент управления ListView в действии

Поскольку `HomeScreen` класс теперь наследует от `Activity` , `ListView` поле объявляется в классе для хранения ссылки на элемент управления, объявленный в AXML:

```csharp
ListView listView;
```

Затем класс должен загрузить пользовательский макет AXML действия с помощью `SetContentView` метода. Затем он может найти `ListView` элемент управления в макете, создает и назначает адаптер и назначает обработчик щелчка. Код для метода OnCreate показан здесь:

```csharp
SetContentView(Resource.Layout.HomeScreen); // loads the HomeScreen.axml as this activity's view
listView = FindViewById<ListView>(Resource.Id.List); // get reference to the ListView in the layout

// populate the listview with data
listView.Adapter = new HomeScreenAdapter(this, tableItems);
listView.ItemClick += OnListItemClick;  // to be defined
```

Наконец, `ItemClick` необходимо определить обработчик; в этом случае просто отобразится `Toast` сообщение:

```csharp
void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
   var listView = sender as ListView;
   var t = tableItems[e.Position];
   Android.Widget.Toast.MakeText(this, t.Heading, Android.Widget.ToastLength.Short).Show();
}
```

Полученный экран выглядит следующим образом:

[![Снимок экрана итогового Кустомроввиев](customizing-appearance-images/customrowview.png)](customizing-appearance-images/customrowview.png#lightbox)

### <a name="customizing-the-row-selector-color"></a>Настройка цвета селектора строк

Когда строка затронута, ее необходимо выделить для отзывов пользователей. Если пользовательское представление определяет цвет фона как **кустомвиев. axml** , он также переопределяет выделенный фрагмент. Эта строка кода в **кустомвиев. axml** задает светло-зеленый фон, но это также означает отсутствие визуального индикатора при внесении строки:

```xml
android:background="#FFDAFF7F"
```

Чтобы повторно включить поведение выделения, а также настроить используемый цвет, задайте для атрибута Background значение настраиваемый селектор. Селектор будет объявлять как цвет фона по умолчанию, так и цвет выделения. Файловые ресурсы, которые вы **Нарисуйте/CustomSelector.xml** , содержат следующее объявление:

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
<item android:state_pressed="false"
  android:state_selected="false"
  android:drawable="@color/cellback" />
<item android:state_pressed="true" >
  <shape>
     <gradient
      android:startColor="#E77A26"
        android:endColor="#E77A26"
        android:angle="270" />
  </shape>
</item>
<item android:state_selected="true"
  android:state_pressed="false"
  android:drawable="@color/cellback" />
</selector>
```

Чтобы сослаться на пользовательский селектор, измените атрибут Background в **кустомвиев. axml** на:

```xml
android:background="@drawable/CustomSelector"
```

Выбранная строка и соответствующее `Toast` сообщение теперь выглядят следующим образом:

[![Выбранная строка оранжевый, с отображаемым всплывающим сообщением именем выбранной строки](customizing-appearance-images/customselectcolor.png)](customizing-appearance-images/customselectcolor.png#lightbox)

### <a name="preventing-flickering-on-custom-layouts"></a>Предотвращение мерцания пользовательских макетов

Android пытается повысить производительность `ListView` прокрутки путем кэширования сведений о макете. Если у вас есть длинные прокручиваемые списки данных, необходимо также задать `android:cacheColorHint` свойство для `ListView` объявления в определении AXML действия (для того же значения цвета, что и для фона пользовательского макета строки). Если не включить эту подсказку, это может привести к «мерцанию» при прокрутке пользователем списка с настраиваемыми цветами фона строк.

## <a name="related-links"></a>Связанные ссылки

- [Буилтинвиевс (пример)](/samples/xamarin/monodroid-samples/builtinviews)
- [Акцессоривиевс (пример)](/samples/xamarin/monodroid-samples/accessoryviews)
- [Кустомроввиев (пример)](/samples/xamarin/monodroid-samples/customrowview)