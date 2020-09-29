---
title: Средство выбора времени
description: Выбор времени с помощью Тимепиккердиалог и Диалогфрагмент
ms.prod: xamarin
ms.assetid: EB4E8206-E8AD-9F04-AC1C-82AC9364A9DD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 0f6082476a7fe4ebd8a6a3f52c23951d58ac1383
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91453786"
---
# <a name="android-time-picker"></a>Средство выбора времени Android

Чтобы предоставить пользователю возможность выбора времени, можно использовать [TimePicker](xref:Android.Widget.TimePicker). Приложения Android обычно используют `TimePicker` с [тимепиккердиалог](xref:Android.App.TimePickerDialog) для выбора значения времени &ndash; . Это помогает обеспечить единообразный интерфейс для устройств и приложений. `TimePicker` позволяет пользователям выбрать время суток в 24-часовом или 12-часовом режиме AM/PM.
`TimePickerDialog` — Это вспомогательный класс, который инкапсулирует `TimePicker` в диалоговом окне.

[![Пример снимка экрана диалогового окна выбора времени в действии](time-picker-images/01-example-screen-sml.png)](time-picker-images/01-example-screen.png#lightbox)

## <a name="overview"></a>Обзор

Современные приложения Android отображают `TimePickerDialog` в [диалогфрагмент](xref:Android.App.DialogFragment). Это позволяет приложению отображать `TimePicker` диалоговое окно в виде всплывающего диалогового окна или внедрять его в действие. Кроме того, `DialogFragment` управляет жизненным циклом и отображением диалогового окна, уменьшая объем кода, который должен быть реализован.

В этом руководстве показано, как использовать объект `TimePickerDialog` , заключенный в `DialogFragment` . Пример приложения отображает `TimePickerDialog` как модальное диалоговое окно, когда пользователь нажимает кнопку на действии. Когда время задается пользователем, диалоговое окно завершает работу, а обработчик обновляет на `TextView` экране действия с выбранным временем.

## <a name="requirements"></a>Требования

Пример приложения для этого руководством предназначен для Android 4,1 (уровень API
16) или более поздней версии, но ее можно использовать с Android 3,0 (API уровня 11 или выше). Можно поддерживать более старые версии Android с добавлением в проект библиотеки поддержки Android версии 4 и некоторыми изменениями кода.

## <a name="using-the-timepicker"></a>Использование TimePicker

Этот пример расширяется `DialogFragment` ; Реализация подкласса `DialogFragment` (вызываемая `TimePickerFragment` ниже) группирует и отображает `TimePickerDialog` . При первом запуске примера приложения отображается кнопка **выбрать время** над тем `TextView` , которое будет использоваться для отображения выбранного времени:

[![Начальный экран примера приложения](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

При нажатии кнопки **выбрать время** в примере приложения запускается, `TimePickerDialog` как показано на снимке экрана:

[![Снимок экрана: диалоговое окно выбора времени по умолчанию, отображаемое приложением](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)

В `TimePickerDialog` , выбрав время и нажав кнопку **ОК** , вызывается `TimePickerDialog` метод [ионтимесетлистенер. OnTime](xref:Android.App.TimePickerDialog.IOnTimeSetListener.OnTimeSet*).
Этот интерфейс реализуется размещением `DialogFragment` ( `TimePickerFragment` , описанным ниже). Нажатие кнопки **Отмена** приводит к закрытию фрагмента и диалогового окна.

`DialogFragment` Возвращает выбранное время для ведущего действия одним из трех способов:

1. **Вызов метода или задание свойства** &ndash; Действие может предоставить свойство или метод, специально для задания этого значения.

2. **Создание события** &ndash; `DialogFragment` Может определять событие, которое будет вызываться при `OnTimeSet` вызове метода.

3. **С `Action` помощью** &ndash; `DialogFragment` Может вызвать метод, `Action<DateTime>` чтобы отобразить время действия. Действие предоставит `Action<DateTime` при создании экземпляра `DialogFragment` .

В этом примере используется третий метод, который требует, чтобы действие предложит `Action<DateTime>` обработчик в `DialogFragment` .

## <a name="start-an-app-project"></a>Запуск проекта приложения

Запуск нового проекта Android с именем **тимепиккердемо** (если вы не знакомы с созданием проектов Xamarin. Android, см. раздел [Hello, Android,](~/android/get-started/hello-android/hello-android-quickstart.md) чтобы узнать, как создать новый проект).

Измените **Resources/Layout/Main. axml** и замените его содержимое следующим XML-кодом:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_gravity="center_horizontal"
    android:padding="16dp">
    <Button
        android:id="@+id/select_button"
        android:paddingLeft="24dp"
        android:paddingRight="24dp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="PICK TIME"
        android:textSize="20dp" />
    <TextView
        android:id="@+id/time_display"
        android:layout_height="wrap_content"
        android:layout_width="match_parent"
        android:paddingTop="22dp"
        android:text="Picked time will be displayed here"
        android:textSize="24dp" />
</LinearLayout>
```

Это базовый [элемент LinearLayout](xref:Android.Widget.LinearLayout) с [TextView](xref:Android.Widget.TextView) , отображающий время и [кнопку](xref:Android.Widget.Button) , которая открывает `TimePickerDialog` . Обратите внимание, что этот макет использует жестко запрограммированные строки и измерения, чтобы упростить и легко понимать приложение в &ndash; рабочем приложении обычно использует ресурсы для этих значений (как можно увидеть в примере кода [DatePicker](https://github.com/xamarin/recipes/blob/master/Recipes/android/controls/datepicker/select_a_date/Resources/layout/Main.axml) ).

Измените **MainActivity.CS** и замените его содержимое следующим кодом:

```csharp
using Android.App;
using Android.Widget;
using Android.OS;
using System;
using Android.Util;
using Android.Text.Format;

namespace TimePickerDemo
{
    [Activity(Label = "TimePickerDemo", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        TextView timeDisplay;
        Button timeSelectButton;

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            SetContentView(Resource.Layout.Main);
            timeDisplay = FindViewById<TextView>(Resource.Id.time_display);
            timeSelectButton = FindViewById<Button>(Resource.Id.select_button);
        }
    }
}
```

При сборке и запуске этого примера вы увидите исходный экран, аналогичный следующему снимку экрана:

[![Начальный экран приложения](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Нажатие кнопки **выбрать время** не выполняет никаких действий, поскольку `DialogFragment` еще не реализовано для вывода `TimePicker` .
Следующим шагом является создание этого `DialogFragment` .

## <a name="extending-dialogfragment"></a>Расширение Диалогфрагмент

Чтобы расширить `DialogFragment` для использования с `TimePicker` , необходимо создать подкласс, производный от `DialogFragment` и реализующий `TimePickerDialog.IOnTimeSetListener` . Добавьте следующий класс в **MainActivity.CS**:

```csharp
public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
{
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };

    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }

    public override Dialog OnCreateDialog (Bundle savedInstanceState)
    {
        DateTime currentTime = DateTime.Now;
        bool is24HourFormat = DateFormat.Is24HourFormat(Activity);
        TimePickerDialog dialog = new TimePickerDialog
            (Activity, this, currentTime.Hour, currentTime.Minute, is24HourFormat);
        return dialog;
    }

    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
        Log.Debug(TAG, selectedTime.ToLongTimeString());
        timeSelectedHandler (selectedTime);
    }
}
```

Этот `TimePickerFragment` класс разбивается на более мелкие части и объясняется в следующем разделе.

### <a name="dialogfragment-implementation"></a>Реализация Диалогфрагмент

`TimePickerFragment` реализует несколько методов: фабричный метод, метод создания экземпляра диалогового окна и `OnTimeSet` метод обработчика, необходимый для `TimePickerDialog.IOnTimeSetListener` .

- `TimePickerFragment` является подклассом `DialogFragment` . Он также реализует `TimePickerDialog.IOnTimeSetListener` интерфейс (то есть он предоставляет необходимый `OnTimeSet` метод):

    ```csharp
    public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
    ```

- `TAG` инициализируется для целей ведения журнала (*митимепиккерфрагмент* можно изменить на любую строку, которую вы хотите использовать). `timeSelectedHandler`Для предотвращения исключений NULL-ссылок действие инициализируется пустым делегатом.

    ```csharp
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };
    ```

- `NewInstance`Метод Factory вызывается для создания нового экземпляра `TimePickerFragment` . Этот метод принимает `Action<DateTime>` обработчик, который вызывается, когда пользователь нажимает кнопку **ОК** в `TimePickerDialog` :

    ```csharp
    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }
    ```

- Когда фрагмент будет отображаться, Android вызывает `DialogFragment` метод [онкреатедиалог](xref:Android.App.DialogFragment.OnCreateDialog*).
    Этот метод создает новый `TimePickerDialog` объект и инициализирует его с помощью действия, объекта обратного вызова (текущего экземпляра `TimePickerFragment` ) и текущего времени:

    ```csharp
    public override Dialog OnCreateDialog (Bundle savedInstanceState)
    {
        DateTime currentTime = DateTime.Now;
        bool is24HourFormat = DateFormat.Is24HourFormat(Activity);
        TimePickerDialog dialog = new TimePickerDialog
            (Activity, this, currentTime.Hour, currentTime.Minute, is24HourFormat);
        return dialog;
    }
    ```

- Когда пользователь изменяет параметр времени в `TimePicker` диалоговом окне, `OnTimeSet` вызывается метод. `OnTimeSet` создает `DateTime` объект, используя текущую дату и слияния за время (час и минуту), выбранное пользователем:

    ```csharp
    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
    ```

- Этот `DateTime` объект передается в `timeSelectedHandler` объект, зарегистрированный в `TimePickerFragment` объекте во время создания. `OnTimeSet` вызывает этот обработчик для обновления экрана времени действия до выбранного времени (этот обработчик реализуется в следующем разделе):

    ```csharp
    timeSelectedHandler (selectedTime);
    ```

## <a name="displaying-the-timepickerfragment"></a>Отображение Тимепиккерфрагмент

Теперь, когда объект `DialogFragment` реализован, можно создать экземпляр `DialogFragment` с помощью `NewInstance` метода Factory и отобразить его, вызвав [диалогфрагмент. Показать](xref:Android.App.DialogFragment.Show*):

Добавьте следующий метод к `MainActivity`:

```csharp
void TimeSelectOnClick (object sender, EventArgs eventArgs)
{
    TimePickerFragment frag = TimePickerFragment.NewInstance (
        delegate (DateTime time)
        {
            timeDisplay.Text = time.ToShortTimeString();
        });

    frag.Show(FragmentManager, TimePickerFragment.TAG);
}
```

После `TimeSelectOnClick` создания экземпляра объект `TimePickerFragment` создает и передает делегат для анонимного метода, который обновляет отображение времени действия, используя переданное значение времени. Наконец, он запускает `TimePicker` фрагмент диалогового окна (через `DialogFragment.Show` ) для показа `TimePicker` пользователю.

В конце `OnCreate` метода добавьте следующую строку, чтобы присоединить обработчик событий к кнопке **Выбор времени** , запускающей диалоговое окно:

```csharp
timeSelectButton.Click += TimeSelectOnClick;
```

При нажатии кнопки **выбрать время** `TimeSelectOnClick` будет вызываться для вывода `TimePicker` фрагмента диалогового окна пользователю.

## <a name="try-it"></a>Попробуйте!

Выполните сборку и запустите приложение. При нажатии кнопки **выбрать время** `TimePickerDialog` отображается в формате времени по умолчанию для действия (в данном случае — в 12-часовом режиме AM/PM):

[![Диалоговое окно времени отображается в режиме AM/PM](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)
   
При нажатии кнопки **ОК** в `TimePicker` диалоговом окне обработчик обновляет действие `TextView` с выбранным временем, а затем завершает работу:

[![В действии TextView отображается время/M.](time-picker-images/04-after-time-dialog-sml.png)](time-picker-images/04-after-time-dialog.png#lightbox)

Затем добавьте следующую строку кода `OnCreateDialog` сразу после `is24HourFormat` объявления и инициализации:

```csharp
is24HourFormat = true;
```

Это изменение приводит к тому, что в конструктор передается флаг `TimePickerDialog` `true` , чтобы вместо формата времени действия размещения использовался 24-часовой режим. При повторной сборке и запуске приложения нажмите кнопку **выбрать время** , `TimePicker` после чего диалоговое окно отобразится в 24-часовом формате:

[![Диалоговое окно TimePicker в 24-часовом формате](time-picker-images/05-24hr-time-dialog-sml.png)](time-picker-images/05-24hr-time-dialog.png#lightbox)

Так как обработчик вызывает [DateTime. тошорттиместринг](xref:System.DateTime.ToShortDateString*) для вывода времени на действие `TextView` , время по-прежнему выводится в 12-часовом формате AM/PM по умолчанию.

## <a name="summary"></a>Сводка

В этой статье описано, как отобразить мини-приложение в `TimePicker` виде всплывающего модального диалогового окна действия Android. Он предоставил пример `DialogFragment` реализации и обсуждал `IOnTimeSetListener` интерфейс. В этом примере также показано, как `DialogFragment` может взаимодействовать с действием узла для отображения выбранного времени.

## <a name="related-links"></a>Связанные ссылки

- [диалогфрагмент](xref:Android.App.DialogFragment)
- [TimePicker](xref:Android.Widget.TimePicker)
- [тимепиккердиалог](xref:Android.App.TimePickerDialog)
- [Тимепиккердиалог. Ионтимесетлистенер](xref:Android.App.TimePickerDialog.IOnTimeSetListener)
- [Тимепиккердемо (пример)](/samples/xamarin/monodroid-samples/userinterface-timepickerdemo)