---
title: Заполнение ListView Xamarin. Android данными
ms.prod: xamarin
ms.assetid: AC4F95C8-EC3F-D960-7D44-8D55D0E4F1B6
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/21/2017
ms.openlocfilehash: 6c22905e576b37d892d83a7b22799e4afdc2262f
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457163"
---
# <a name="populating-a-xamarinandroid-listview-with-data"></a>Заполнение ListView Xamarin. Android данными

Чтобы добавить строки в, необходимо `ListView` добавить его в макет и реализовать `IListAdapter` методы с методами, которые выполняют `ListView` вызовы для заполнения самого себя. В состав Android входят встроенные `ListActivity` `ArrayAdapter` классы и, которые можно использовать без определения ПОЛЬЗОВАТЕЛЬСКОГО макета XML или кода. `ListActivity`Класс автоматически создает `ListView` и предоставляет `ListAdapter` свойство для предоставления представлений строк, отображаемых через адаптер.

Встроенные адаптеры получают идентификатор ресурса представления в качестве параметра, который используется для каждой строки. Вы можете использовать встроенные ресурсы, например, в `Android.Resource.Layout` , чтобы вам не нужно было писать собственный.

## <a name="using-listactivity-and-arrayadapterltstringgt"></a>Использование строки Листактивити и ArrayAdapter &lt;&gt;

В примере **басиктабле/хомескрин. CS** демонстрируется использование этих классов для отображения всего `ListView` нескольких строк кода:

```csharp
[Activity(Label = "BasicTable", MainLauncher = true, Icon = "@drawable/icon")]
public class HomeScreen : ListActivity {
   string[] items;
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       items = new string[] { "Vegetables","Fruits","Flower Buds","Legumes","Bulbs","Tubers" };
       ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItem1, items);
   }
}
```

### <a name="handling-row-clicks"></a>Обработка щелчков по строкам

Обычно `ListView` также позволяет пользователю коснуться строки, чтобы выполнить какое-либо действие (например, воспроизведение песни или вызов контакта или отображение другого экрана). Для реагирования на пользовательское касание должен быть еще один метод, реализованный следующим `ListActivity` &ndash; `OnListItemClick` &ndash; образом:

[![Снимок экрана Симплелиститем](populating-images/simplelistitem1.png)](populating-images/simplelistitem1.png#lightbox)

```csharp
protected override void OnListItemClick(ListView l, View v, int position, long id)
{
   var t = items[position];
   Android.Widget.Toast.MakeText(this, t, Android.Widget.ToastLength.Short).Show();
}
```

Теперь пользователь может коснуться строки, и `Toast` появится предупреждение:

[![Снимок экрана с всплывающим окном, отображаемым при затронутой строке](populating-images/basictable2.png)](populating-images/basictable2.png#lightbox)

## <a name="implementing-a-listadapter"></a>Реализация Листадаптер

`ArrayAdapter<string>` является великолепной из-за простоты, но чрезвычайно ограничена. Однако часто возникает коллекция бизнес-сущностей, а не только строк, которые необходимо привязать.
Например, если данные состоят из коллекции классов Employee, может потребоваться, чтобы в списке отображались только названия сотрудников. Чтобы настроить поведение элемента `ListView` для управления отображаемыми данными, необходимо реализовать подкласс `BaseAdapter` переопределения следующих четырех элементов:

- **Количество** &ndash; Чтобы сообщить элементу управления, сколько строк находится в данных.

- **Просмотр** &ndash; Для возвращения представления для каждой строки, заполненной данными.
    Этот метод имеет параметр для `ListView` передачи существующей неиспользуемой строки для повторного использования.

- **Жетитемид** &ndash; Возвращает идентификатор строки (обычно это номер строки, хотя это может быть любое длинное значение).

- **Этот индексатор [int]** &ndash; возвращает данные, связанные с определенным номером строки.

В примере кода в **басиктаблеадаптер/хомескринадаптер. CS** показано, как создать подкласс `BaseAdapter` :

```csharp
public class HomeScreenAdapter : BaseAdapter<string> {
   string[] items;
   Activity context;
   public HomeScreenAdapter(Activity context, string[] items) : base() {
       this.context = context;
       this.items = items;
   }
   public override long GetItemId(int position)
  {
       return position;
   }
   public override string this[int position] {  
       get { return items[position]; }
   }
   public override int Count {
       get { return items.Length; }
   }
   public override View GetView(int position, View convertView, ViewGroup parent)
   {
       View view = convertView; // re-use an existing view, if one is available
      if (view == null) // otherwise create a new one
           view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
       view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
       return view;
   }
}
```

### <a name="using-a-custom-adapter"></a>Использование пользовательского адаптера

Использование пользовательского адаптера аналогично встроенному `ArrayAdapter` , передавая `context` `string[]` значения и для отображаемых значений:

```csharp
ListAdapter = new HomeScreenAdapter(this, items);
```

Поскольку в этом примере используется тот же макет строки ( `SimpleListItem1` ), полученное приложение будет выглядеть аналогично предыдущему примеру.

### <a name="row-view-re-use"></a>Повторное использование представления строк

В этом примере только шесть элементов. Так как экран может соответствовать восьми, повторное использование строк не требуется. Однако при отображении сотен или тысяч строк это приведет к недостаточной памяти для создания сотен или тысяч `View` объектов, когда только восемь помещаются на экран за раз. Чтобы избежать такой ситуации, когда строка исчезает с экрана, ее представление помещается в очередь для повторного использования. По мере прокрутки пользователя `ListView` вызовы `GetView` для запроса новых представлений отображаются, &ndash; если они доступны, если они передают неиспользуемое представление в `convertView` параметре. Если это значение равно null, то код должен создать новый экземпляр представления. в противном случае можно повторно задать свойства этого объекта и повторно использовать его.

`GetView`Метод должен следовать этому шаблону для повторного использования представлений строк:

```csharp
public override View GetView(int position, View convertView, ViewGroup parent)
{
   View view = convertView; // re-use an existing view, if one is supplied
   if (view == null) // otherwise create a new one
       view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
   // set view properties to reflect data for the given row
   view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
   // return the view, populated with data, for display
   return view;
}
```

Пользовательские реализации адаптеров *всегда* должны повторно использовать `convertView` объект перед созданием новых представлений, чтобы избежать нехватки памяти при отображении длинных списков.

Некоторые реализации адаптеров (например, `CursorAdapter` ) не имеют `GetView` метода, а используют два разных метода `NewView` и `BindView` принудительное повторное использование строк путем разделения обязанностей `GetView` на два метода. `CursorAdapter`Ниже приведен пример в документе.

## <a name="enabling-fast-scrolling"></a>Включение быстрой прокрутки

Быстрая прокрутка позволяет пользователю прокручивать длинные списки, предоставляя дополнительный маркер "Handle", который выступает в качестве полосы прокрутки для прямого доступа к части списка. На этом снимке экрана показан маркер быстрой прокрутки:

[![Снимок экрана с быстрой прокруткой с помощью маркера прокрутки](populating-images/fastscroll.png)](populating-images/fastscroll.png#lightbox)

Причиной появления маркера быстрой прокрутки является то, что установка `FastScrollEnabled` свойства равным `true` :

```csharp
ListView.FastScrollEnabled = true;
```

### <a name="adding-a-section-index"></a>Добавление индекса раздела

Индекс раздела предоставляет дополнительные отзывы для пользователей при быстрой прокрутке по длинному списку &ndash; , в котором отображается раздел с прокруткой. Чтобы вызвать отображение индекса раздела, подкласс адаптера должен реализовать `ISectionIndexer` интерфейс для предоставления текста индекса в зависимости от отображаемых строк:

[![Снимок экрана с отображаемым выше разделом H, начинающимся с H](populating-images/sectionindex.png)](populating-images/sectionindex.png#lightbox)

Для реализации необходимо `ISectionIndexer` Добавить в адаптер три метода:

- **Части** &ndash; Содержит полный список заголовков индексов разделов, которые могут быть отображены. Для этого метода требуется массив объектов Java, поэтому код должен создать объект `Java.Lang.Object[]` из коллекции .NET. В нашем примере возвращается список начальных символов в списке в виде `Java.Lang.String` .

- **Жетпоситионфорсектион** &ndash; Возвращает позиции первой строки для данного индекса раздела.

- **Жетсектионфорпоситион** &ndash; Возвращает индекс раздела, который будет отображаться для данной строки.

В примере `SectionIndex/HomeScreenAdapter.cs` файла реализуются эти методы и дополнительный код в конструкторе. Конструктор строит индекс раздела путем прохода по каждой строке и извлечения первого символа заголовка (элементы уже должны быть отсортированы для работы).

```csharp
alphaIndex = new Dictionary<string, int>();
for (int i = 0; i < items.Length; i++) { // loop through items
   var key = items[i][0].ToString();
   if (!alphaIndex.ContainsKey(key))
       alphaIndex.Add(key, i); // add each 'new' letter to the index
}
sections = new string[alphaIndex.Keys.Count];
alphaIndex.Keys.CopyTo(sections, 0); // convert letters list to string[]

// Interface requires a Java.Lang.Object[], so we create one here
sectionsObjects = new Java.Lang.Object[sections.Length];
for (int i = 0; i < sections.Length; i++) {
   sectionsObjects[i] = new Java.Lang.String(sections[i]);
}
```

При создании структур данных `ISectionIndexer` методы очень просты:

```csharp
public Java.Lang.Object[] GetSections()
{
   return sectionsObjects;
}
public int GetPositionForSection(int section)
{
   return alphaIndexer[sections[section]];
}
public int GetSectionForPosition(int position)
{   // this method isn't called in this example, but code is provided for completeness
    int prevSection = 0;
    for (int i = 0; i < sections.Length; i++)
    {
        if (GetPositionForSection(i) > position)
        {
            break;
        }
        prevSection = i;
    }
    return prevSection;
}
```

Названиям индексов разделов не нужно сопоставлять 1:1 с реальными разделами. Именно поэтому `GetPositionForSection` метод существует.
`GetPositionForSection` предоставляет возможность сопоставлять любые индексы в списке индексов с любыми разделами в представлении списка. Например, в индексе может быть знак "z", но для каждой буквы может отсутствовать раздел таблицы, поэтому вместо "z" в 26 может сопоставлено значение 25 или 24, либо любой индекс раздела "z" должен быть сопоставлен с.

## <a name="related-links"></a>Связанные ссылки

- [Басиктаблеандроид (пример)](/samples/xamarin/monodroid-samples/basictableandroid)
- [Басиктаблеадаптер (пример)](/samples/xamarin/monodroid-samples/basictableadapter)
- [Фастскролл (пример)](/samples/xamarin/monodroid-samples/fastscroll)