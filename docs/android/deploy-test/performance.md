---
title: Производительность Xamarin.Android
description: Существует множество методов повышения производительности приложений, созданных с помощью Xamarin.Android. Вместе они могут значительно снизить загрузку ЦП и сократить объем памяти, используемой приложением. Эти методы описаны в данной статье.
ms.prod: xamarin
ms.assetid: dc2e27f2-7f71-4d57-9cf9-165528276613
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 63365ebc12089ced7de621b3a510996fa66119ce
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571965"
---
# <a name="xamarinandroid-performance"></a>Производительность Xamarin.Android

_Существует множество методов повышения производительности приложений, созданных с помощью Xamarin.Android. Вместе они могут значительно снизить загрузку ЦП и сократить объем памяти, используемой приложением. Все они описаны в этой статье._

## <a name="performance-overview"></a>Обзор производительности

Низкая производительность приложения проявляется по-разному. Из-за нее приложение может переставать отвечать на запросы, могут возникать задержки при прокрутке и сокращаться время работы батареи. Однако оптимизация производительности предусматривает не только правильную реализацию кода. Необходимо также учитывать эффективность работы пользователей. Например, чтобы повысить удобство работы, необходимо сделать так, чтобы выполнение операций не мешало пользователю выполнять другие действия.

Существует ряд методов повышения производительности приложений, созданных на платформе Xamarin.Android, в том числе воспринимаемой пользователем. К ним относятся следующие:

- [Оптимизация иерархий макетов](#optimizelayout)
- [Оптимизация представлений списков](#optimizelistviews)
- [Удаление обработчиков событий в действиях](#removeeventhandlers)
- [Ограничение продолжительности существования служб](#limitservices)
- [Освобождение ресурсов по уведомлению](#releaseresources)
- [Освобождение ресурсов при скрытии пользовательского интерфейса](#releaseresourcesuihidden)
- [Оптимизация графических ресурсов](#optimizeimages)
- [Ликвидация неиспользуемых графических ресурсов](#disposeimages)
- [Исключение арифметических операций с плавающей запятой](#avoidfloats)
- [Закрытие диалоговых окон](#dismissdialogs)

> [!NOTE]
> Прежде чем прочитать эту статью, ознакомьтесь со статьей [Кроссплатформенная производительность](~/cross-platform/deploy-test/memory-perf-best-practices.md), в которой описываются универсальные для всех платформ способы оптимизации использования памяти и повышения производительности приложений, построенных с помощью платформы Xamarin.

<a name="optimizelayout"></a>

## <a name="optimize-layout-hierarchies"></a>Оптимизация иерархий макетов

Для каждого добавляемого в приложение макета выполняются инициализация, разметка и отрисовка. При наличии вложенных экземпляров [`LinearLayout`](xref:Android.Widget.LinearLayout), использующих параметр `weight`, этап разметки может быть достаточно ресурсоемким, поскольку каждый дочерний объект измеряется два раза. Использование вложенных экземпляров `LinearLayout` может привести к появлению глубокой иерархии представлений, что повлечет снижение производительности для макетов, которые используются много раз, таких как [`ListView`](xref:Android.Widget.ListView). Таким образом, оптимизация макетов позволяет многократно повысить производительность.

Например, рассмотрим макет [`LinearLayout`](xref:Android.Widget.LinearLayout) для строки представления списка, который содержит значок, название и описание. `LinearLayout` будет содержать [`ImageView`](xref:Android.Widget.ImageView), а также вертикальный объект `LinearLayout`, содержащий два экземпляра [`TextView`](xref:Android.Widget.TextView):

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="?android:attr/listPreferredItemHeight"
    android:padding="5dip">
    <ImageView
        android:id="@+id/icon"
        android:layout_width="wrap_content"
        android:layout_height="fill_parent"
        android:layout_marginRight="5dip"
        android:src="@drawable/icon" />
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="0dip"
        android:layout_weight="1"
        android:layout_height="fill_parent">
        <TextView
            android:layout_width="fill_parent"
            android:layout_height="0dip"
            android:layout_weight="1"
            android:gravity="center_vertical"
            android:text="Mei tempor iuvaret ad." />
        <TextView
            android:layout_width="fill_parent"
            android:layout_height="0dip"
            android:layout_weight="1"
            android:singleLine="true"
            android:ellipsize="marquee"
            android:text="Lorem ipsum dolor sit amet." />
    </LinearLayout>
</LinearLayout>
```

Иерархия этого макета имеет 3 уровня, поэтому при его использовании для каждой строки [`ListView`](xref:Android.Widget.ListView) производительность заметно снижается. Чтобы оптимизировать этот макет, можно преобразовать его в плоскую структуру, как показано в следующем примере кода:

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="?android:attr/listPreferredItemHeight"
    android:padding="5dip">
    <ImageView
        android:id="@+id/icon"
        android:layout_width="wrap_content"
        android:layout_height="fill_parent"
        android:layout_alignParentTop="true"
        android:layout_alignParentBottom="true"
        android:layout_marginRight="5dip"
        android:src="@drawable/icon" />
    <TextView
        android:id="@+id/secondLine"
        android:layout_width="fill_parent"
        android:layout_height="25dip"
        android:layout_toRightOf="@id/icon"
        android:layout_alignParentBottom="true"
        android:layout_alignParentRight="true"
        android:singleLine="true"
        android:ellipsize="marquee"
        android:text="Lorem ipsum dolor sit amet." />
    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/icon"
        android:layout_alignParentRight="true"
        android:layout_alignParentTop="true"
        android:layout_above="@id/secondLine"
        android:layout_alignWithParentIfMissing="true"
        android:gravity="center_vertical"
        android:text="Mei tempor iuvaret ad." />
</RelativeLayout>
```

Количество уровней в иерархии уменьшено с 3 до 2, а один объект [`RelativeLayout`](xref:Android.Widget.RelativeLayout) заменил собой два экземпляра [`LinearLayout`](xref:Android.Widget.LinearLayout). Таким образом, при использовании макета для каждой строки [`ListView`](xref:Android.Widget.ListView) удастся добиться значительного повышения производительности.

<a name="optimizelistviews"></a>

## <a name="optimize-list-views"></a>Оптимизация представлений списков

Пользователи рассчитывают на быструю прокрутку и загрузку экземпляров [`ListView`](xref:Android.Widget.ListView). Если каждая строка в представлении списка будет содержать иерархии представлений с глубоким уровнем вложения или сложные макеты, производительность прокрутки может заметно снижаться. Тем не менее существует несколько способов повысить производительность `ListView`:

- Повторное использование представлений строк. Дополнительные сведения см. в разделе [Повторное использование представлений строк](#reuserowviews).
- Преобразование макетов в плоскую структуру, если это возможно.
- Кэширование содержимого строки, извлеченного из веб-службы.
- Предотвращение масштабирования изображений.

Совместное применение этих способов позволяет обеспечить плавную прокрутку экземпляров [`ListView`](xref:Android.Widget.ListView).

<a name="reuserowviews"></a>

### <a name="reuse-row-views"></a>Повторное использование представлений строк

Если в [`ListView`](xref:Android.Widget.ListView) отображаются сотни строк, на создание сотен объектов [`View`](xref:Android.Views.View) затрачивается большой объем памяти, хотя при этом на экране одновременно отображается лишь несколько из них. Вместо этого в память можно загружать только многократно используемые объекты `View`, которые отображаются в строках на экране, загружая в них необходимое **содержимое**. Это позволяет избежать создания сотен дополнительных объектов и значительно сэкономить время и ресурсы памяти.

Таким образом, если строка скрывается с экрана, ее представление может быть помещено в очередь для повторного использования, как показано в следующем примере кода:

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

По мере прокрутки списка [`ListView`](xref:Android.Widget.ListView) вызывает переопределение метода `GetView`, запрашивая отображение новых представлений. Если это возможно, неиспользуемое представление передается в параметре `convertView`. Если это значение равно `null`, код создает новый экземпляр [`View`](xref:Android.Views.View), иначе свойства `convertView` можно сбросить и использовать повторно.

Дополнительные сведения см. в разделе [Повторное использование представления строк](~/android/user-interface/layouts/list-view/populating.md#row-view-re-use) в статье [Заполнение объекта ListView данными](~/android/user-interface/layouts/list-view/populating.md).

<a name="removeeventhandlers"></a>

## <a name="remove-event-handlers-in-activities"></a>Удаление обработчиков событий в действиях

Действие, которое уничтожается в среде выполнения Android, по-прежнему может существовать в среде выполнения Mono. Таким образом, можно удалять обработчики событий для внешних объектов в методе `Activity.OnPause`, благодаря чему в среде выполнения не будет храниться ссылка на действие, которое было уничтожено.

В действии объявите обработчик или обработчики событий на уровне класса:

```csharp
EventHandler<UpdatingEventArgs> service1UpdateHandler;
```

Затем реализуйте обработчики в действии, например, как в `OnResume`:

```csharp
service1UpdateHandler = (object s, UpdatingEventArgs args) => {
    this.RunOnUiThread (() => {
        this.updateStatusText1.Text = args.Message;
    });
};
App.Current.Service1.Updated += service1UpdateHandler;
```

При выходе из состояния выполнения действия вызывается метод `OnPause`. В реализации `OnPause` удалите обработчики событий следующим образом:

```csharp
App.Current.Service1.Updated -= service1UpdateHandler;
```

<a name="limitservices"></a>

## <a name="limit-the-lifespan-of-services"></a>Ограничение продолжительности существования служб

При запуске службы Android выполняет соответствующий ей процесс. При этом выделяемая для процесса память не может использоваться где-то еще. Таким образом, если служба выполняется, но не используется, повышается риск снижения производительности приложения из-за нехватки памяти. Кроме того, в этом случае снижается эффективность переключения между приложениями, поскольку уменьшается число доступных для кэширования процессов в Android.

Время существования службы можно ограничить с помощью класса `IntentService`, который завершает собственную работу после обработки запустившего его намерения.

<a name="releaseresources"></a>

## <a name="release-resources-when-notified"></a>Освобождение ресурсов по уведомлению

В рамках жизненного цикла приложения обратный вызов метода [`OnTrimMemory`](xref:Android.App.Activity.OnTrimMemory*) позволяет получить уведомление о нехватке памяти устройства. Реализация этого обратного вызова позволяет прослушивать следующие уведомления о состоянии памяти:

- [`TrimMemoryRunningModerate`](xref:Android.Content.ComponentCallbacks2.TrimMemoryRunningModerate) — приложению *может* потребоваться высвобождение некоторых ненужных ресурсов.
- [`TrimMemoryRunningLow`](xref:Android.Content.ComponentCallbacks2.TrimMemoryRunningLow) — приложению *следует* освободить ненужные ресурсы.
- [`TrimMemoryRunningCritical`](xref:Android.Content.ComponentCallbacks2.TrimMemoryRunningCritical) — приложению *следует* освободить как можно больше некритических процессов.

Кроме того, при кэшировании процесса приложения обратный вызов [`OnTrimMemory`](xref:Android.App.Activity.OnTrimMemory*) может получать следующие уведомления о состоянии памяти:

- [`TrimMemoryBackground`](xref:Android.Content.ComponentCallbacks2.TrimMemoryBackground) — следует освободить ресурсы, которые можно эффективно и быстро перестроить в случае возвращения пользователя в приложение.
- [`TrimMemoryModerate`](xref:Android.Content.ComponentCallbacks2.TrimMemoryModerate) — высвобождение ресурсов может обеспечить кэширование других процессов в системе для повышения общей производительности.
- [`TrimMemoryComplete`](xref:Android.Content.ComponentCallbacks2.TrimMemoryComplete) — если объем доступной памяти не увеличится, процесс приложения в скором времени будет завершен.

В ответ на полученное уведомление должны быть высвобождены ресурсы, соответствующие его уровню.

<a name="releaseresourcesuihidden"></a>

## <a name="release-resources-when-the-user-interface-is-hidden"></a>Освобождение ресурсов при скрытии пользовательского интерфейса

Когда пользователь переходит в другое приложение, следует высвободить любые ресурсы, используемые интерфейсом текущего приложения. Это позволяет значительно повысить производительность кэширования процессов в среде Android и общую эффективность.

Чтобы получать уведомление, когда пользователь выходит из пользовательского интерфейса, реализуйте обратный вызов [`OnTrimMemory`](xref:Android.App.Activity.OnTrimMemory*) в классах `Activity` и прослушивание на уровне [`TrimMemoryUiHidden`](xref:Android.Content.ComponentCallbacks2.TrimMemoryUiHidden), указывающем на скрытие пользовательского интерфейса в представлении. Это уведомление будет получено только в том случае, если *все* компоненты пользовательского интерфейса приложения скрываются. Освобождение ресурсов пользовательского интерфейса при получении уведомления позволяет гарантировать быстрое возобновление доступа к ним при возвращении пользователя в приложение после выполнения других действий.

<a name="optimizeimages"></a>

## <a name="optimize-image-resources"></a>Оптимизация графических ресурсов

Изображения — это один из самых ресурсоемких ресурсов в приложениях. Они часто хранятся в высоком разрешении. При выводе изображения на экран используется разрешение, соответствующее характеристикам экрана. Если разрешение изображения превышает возможности экрана, следует уменьшить его масштаб.

Дополнительные сведения см. в разделе [Оптимизация графических ресурсов](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages) в руководстве [Кроссплатформенная производительность](~/cross-platform/deploy-test/memory-perf-best-practices.md).

<a name="disposeimages"></a>

## <a name="dispose-of-unused-image-resources"></a>Ликвидация неиспользуемых графических ресурсов

Чтобы оптимизировать использование памяти, рекомендуется ликвидировать неиспользуемые ресурсы больших изображений. Тем не менее важно обеспечить надлежащее удаление изображений. Вместо явного вызова `.Dispose()` вы можете использовать преимущества инструкций [using](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/using-statement) для эффективной работы с объектами `IDisposable`. 

Например, класс [Bitmap](xref:Android.Graphics.Bitmap) реализует интерфейс `IDisposable`. Создавая оболочку при создании экземпляра объекта `BitMap` в блоке `using`, можно гарантировать, что при выходе из блока он будет ликвидирован надлежащим образом:

```csharp
using (Bitmap smallPic = BitmapFactory.DecodeByteArray(smallImageByte, 0, smallImageByte.Length))
{
    // Use the smallPic bit map here
}
```

Дополнительные сведения об освобождении высвобождаемых ресурсов см. в разделе [Освобождение ресурсов IDisposable](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable).  

<a name="avoidfloats"></a>

## <a name="avoid-floating-point-arithmetic"></a>Исключение арифметических операций с плавающей запятой

На устройствах Android арифметические операции с плавающей запятой выполняются примерно в два раза медленнее операций с целыми числами. Соответственно, следует по возможности использовать операции с целыми числами. Тем не менее на современном оборудовании разница между арифметическими операциями со значениями типов `float` и `double` незаметна.

> [!NOTE]
> Даже при выполнении целочисленных арифметических операций некоторым процессорам не хватает ресурсов для деления. В связи с этим целочисленные операции деления и получения модуля часто выполняются в программном обеспечении.

<a name="dismissdialogs"></a>

## <a name="dismiss-dialogs"></a>Закрытие диалоговых окон

При использовании класса [`ProgressDialog`](xref:Android.App.ProgressDialog) (а также любого диалогового окна или оповещения) вместо вызова метода [`Hide`](xref:Android.App.Dialog.Hide*) по завершении работы с диалоговым окном следует вызывать метод [`Dismiss`](xref:Android.App.Dialog.Dismiss*). В противном случае диалоговое окно по-прежнему будет существовать и создавать утечку памяти за счет хранения ссылки на действие.

## <a name="summary"></a>Сводка

В этой статье были рассмотрены методы повышения производительности приложений, созданных на платформе Xamarin.Android. Вместе они могут значительно снизить загрузку ЦП и сократить объем памяти, используемой приложением.

## <a name="related-links"></a>Связанные ссылки

- [Кроссплатформенная производительность](~/cross-platform/deploy-test/memory-perf-best-practices.md)
