---
title: Принципы разработки API Xamarin. Android
ms.prod: xamarin
ms.assetid: 3E52D815-D95D-5510-0D8F-77DAC7E62EDE
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: bb9dadd388a514f18abdaf6e6db6d76f0be99f86
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91454251"
---
# <a name="xamarinandroid-api-design-principles"></a>Принципы разработки API Xamarin. Android

В дополнение к основным библиотекам базовых классов, которые являются частью Mono, Xamarin. Android поставляется с привязками для различных интерфейсов API Android, позволяя разработчикам создавать собственные приложения Android с Mono.

В ядре Xamarin. Android есть механизм взаимодействия, который связывает мир C# с языком Java и предоставляет разработчикам доступ к API-интерфейсам Java из C# или других языков .NET.

## <a name="design-principles"></a>Принципы проектирования

Ниже приведены некоторые принципы разработки для привязки Xamarin. Android.

- Соответствие  [рекомендациям по проектированию .NET Framework](/dotnet/standard/design-guidelines/).

- Разрешить разработчикам подкласс классов Java.

- Подкласс должен работать с конструкциями C# Standard.

- Наследование от существующего класса.

- Вызовите базовый конструктор для создания цепочки.

- Переопределяющие методы должны выполняться с помощью системы переопределения C#.

- Сделайте распространенные задачи Java простыми и сложные задачи Java.

- Предоставьте свойства Жавабеан в виде свойств C#.

- Предоставление строго типизированного API:

  - Повышение безопасности типа.

  - Сократите количество ошибок времени выполнения.

  - Получите IntelliSense интегрированной среды разработки для возвращаемых типов.

  - Разрешает всплывающую документацию интегрированной среды разработки.

- Поощряйте изучение API в интегрированной среде разработки:

  - Используйте альтернативные платформы, чтобы снизить уязвимость Java Класслиб.

  - Предоставляйте делегаты C# (лямбда-выражения, анонимные методы и System. Delegate) вместо интерфейсов с одним методом, если это уместно и применимо.

  - Предоставьте механизм для вызова произвольных библиотек Java ( [Android. Runtime. жниенв](xref:Android.Runtime.JNIEnv)).

## <a name="assemblies"></a>Сборки

Xamarin. Android включает несколько сборок, которые составляют профиль для *Бесмобильности*. Дополнительные сведения см. на странице [Сборки](~/cross-platform/internals/available-assemblies.md).

Привязки к платформе Android содержатся в `Mono.Android.dll` сборке. Эта сборка содержит всю привязку для использования интерфейсов API Android и взаимодействия с виртуальной машиной среды выполнения Android.

## <a name="binding-design"></a>Разработка привязки

### <a name="collections"></a>Коллекции

API-интерфейсы Android широко используют коллекции Java. util для предоставления списков, наборов и сопоставлений. Мы предоставляем эти элементы с помощью интерфейсов [System. Collections. Generic](xref:System.Collections.Generic) в нашей привязке. Основные сопоставления:

- [Java. util. Set \<E> ](https://developer.android.com/reference/java/util/Set.html) сопоставляется с системным типом [ICollection \<T> ](xref:System.Collections.Generic.ICollection`1), вспомогательным классом [ \<T> Android. Runtime. Java](xref:Android.Runtime.JavaSet`1).

- [Java. util. List \<E> ](https://developer.android.com/reference/java/util/List.html) сопоставляется с системным типом [IList \<T> ](xref:System.Collections.Generic.IList`1), вспомогательным классом [Android \<T> . Runtime. жавалист](xref:Android.Runtime.JavaList`1).

- [Java. util. Map<K, V>](https://developer.android.com/reference/java/util/Map.html) сопоставляется с типом системы [IDictionary<TKey, TValue>](xref:System.Collections.Generic.IDictionary`2), вспомогательный класс [Android. Runtime. жавадиктионари<K, V>](xref:Android.Runtime.JavaDictionary`2).

- [Java. util. Collection \<E> ](https://developer.android.com/reference/java/util/Collection.html) сопоставляется с системным типом [ICollection \<T> ](xref:System.Collections.Generic.ICollection`1), вспомогательным классом [Android \<T> . Runtime. жаваколлектион](xref:Android.Runtime.JavaCollection`1).

Мы предоставили вспомогательные классы для упрощения безкопированного маршалирования этих типов. По возможности рекомендуется использовать предоставленные коллекции вместо реализации, предоставляемой платформой, например [`List<T>`](xref:System.Collections.Generic.List`1) или [`Dictionary<TKey, TValue>`](xref:System.Collections.Generic.Dictionary`2) . Реализации [Android. Runtime](xref:Android.Runtime) используют собственную коллекцию Java и поэтому не нуждаются в копировании в собственную коллекцию и из нее при передаче в член API Android.

Можно передать любую реализацию интерфейса в метод Android, принимающий этот интерфейс, например передать в `List<int>` конструктор [ArrayAdapter &lt; int &gt; (Context, int, IList &lt; int &gt; )](xref:Android.Widget.ArrayAdapter`1) . *Однако*для всех реализаций, *за исключением* реализаций Android. Runtime, это включает *копирование* списка из виртуальной машины Mono в виртуальную машину среды выполнения Android. Значение, если список позже изменялся в среде выполнения Android (например, путем вызова [ArrayAdapter &lt; T &gt; . Метод Add (T)](xref:Android.Widget.ArrayAdapter`1.Add*) ), эти изменения *не будут* видны в управляемом коде. Если `JavaList<int>` использовались, эти изменения будут видимы.

Перезаписанные, реализации интерфейса коллекций, которые *не* являются одним из приведенного выше списка **вспомогательных классов**, только для упаковки [in]:

```csharp
// This fails:
var badSource  = new List<int> { 1, 2, 3 };
var badAdapter = new ArrayAdapter<int>(context, textViewResourceId, badSource);
badAdapter.Add (4);
if (badSource.Count != 4) // true
    throw new InvalidOperationException ("this is thrown");

// this works:
var goodSource  = new JavaList<int> { 1, 2, 3 };
var goodAdapter = new ArrayAdapter<int> (context, textViewResourceId, goodSource);
goodAdapter.Add (4);
if (goodSource.Count != 4) // false
    throw new InvalidOperationException ("should not be reached.");
```

### <a name="properties"></a>Свойства

При необходимости методы Java преобразуются в свойства.

- Пара методов Java и преобразуется `T getFoo()` `void setFoo(T)` в `Foo` свойство. Пример: [Activity. намерения](xref:Android.App.Activity.Intent).

- Метод Java преобразуется `getFoo()` в доступное только для чтения свойство foo. Пример: [context. PackageName](xref:Android.Content.Context.PackageName).

- Свойства, доступные только для установки, не создаются.

- Свойства *не* создаются, если тип свойства будет массивом.

### <a name="events-and-listeners"></a>События и прослушиватели

API-интерфейсы Android основаны на Java, и его компоненты соответствуют шаблону Java для подключения прослушивателей событий. Этот шаблон является громоздким, так как требует от пользователя создания анонимного класса и объявления методов для переопределения, например, вот как это делается в Android с помощью Java:

```csharp
final android.widget.Button button = new android.widget.Button(context);

button.setText(this.count + " clicks!");
button.setOnClickListener (new View.OnClickListener() {
    public void onClick (View v) {
        button.setText(++this.count + " clicks!");
    }
});
```

Эквивалентный код в C#, использующий события, будет выглядеть так:

```csharp
var button = new Android.Widget.Button (context) {
    Text = string.Format ("{0} clicks!", this.count),
};
button.Click += (sender, e) => {
    button.Text = string.Format ("{0} clicks!", ++this.count);
};
```

Обратите внимание, что оба приведенных выше механизма доступны в Xamarin. Android. Можно реализовать интерфейс прослушивателя и присоединить его к View. Сетонкликклистенер или присоединить делегат, созданный с помощью любой из обычных парадигм C#, к событию Click.

Когда метод обратного вызова прослушивателя возвращает значение void, мы создаем элементы API на основе делегата [EventHandler &lt; тевентаргс &gt; ](xref:System.EventHandler`1) . Для этих типов прослушивателей создается событие, аналогичное приведенному выше примеру. Однако если обратный вызов прослушивателя возвращает значение, отличное от void, и не является **логическим** , события и евенсандлерс не используются. Вместо этого мы создадим конкретный делегат для сигнатуры обратного вызова и добавим свойства вместо событий. Причина заключается в том, чтобы справиться с порядком вызова делегата и обработкой возврата. Этот подход отражает, что делается с помощью API Xamarin. iOS.

События или свойства C# создаются автоматически только в том случае, если метод регистрации событий Android:

1. Имеет `set` префикс, например [ *Set*онкликклистенер](xref:Android.Views.View.SetOnClickListener*).

1. Имеет `void` тип возвращаемого значения.

1. Принимает только один параметр, тип параметра является интерфейсом, интерфейс имеет только один метод, а имя интерфейса заканчивается на `Listener` , например, в [ *прослушивателе*View. OnClick](xref:Android.Views.View.IOnClickListener).

Более того, если метод интерфейса прослушивателя имеет тип возвращаемого **значения Boolean** , а не **void**, то созданный подкласс *EventArgs* будет содержать *обработанное* свойство. Значение *обрабатываемого* свойства используется в качестве возвращаемого значения для метода *прослушивателя* , а по умолчанию — значение `true` .

Например, метод Android [View. сетонкэйлистенер ()](xref:Android.Views.View.SetOnKeyListener*) принимает интерфейс [View. онкэйлистенер](xref:Android.Views.View.IOnKeyListener) , а метод [View. онкэйлистенер. онкэй (View, int, кэйевент)](xref:Android.Views.View.IOnKeyListener.OnKey*) имеет логический тип возвращаемого значения. Xamarin. Android создает соответствующее событие [View. KeyPress](xref:Android.Views.View.KeyPress) , которое является [ &lt; представлением EventHandler. KeyEventArgs &gt; ](xref:Android.Views.View.KeyEventArgs).
Класс *KeyEventArgs* , в свою очередь, имеет свойство [View. KeyEventArgs. Handled](xref:Android.Views.View.KeyEventArgs.Handled) , которое используется в качестве возвращаемого значения для метода *View. онкэйлистенер. онкэй ()* .

Мы планируем добавить перегрузки для других методов и ctor для предоставления подключения на основе делегата. Кроме того, для прослушивателей с несколькими обратными вызовами требуется дополнительная проверка, чтобы определить, разумны ли реализации отдельных обратных вызовов, так что мы преобразуем их по мере их идентификации. Если соответствующего события нет, прослушиватели должны использоваться в C#, но при этом все, что вы думаете, может использовать делегирование для нашего внимания. Мы также выполнили некоторые преобразования интерфейсов без суффикса "Listener", когда было ясно, что это помогло бы воспользоваться альтернативой делегата.

Все интерфейсы прослушивателей реализуют [`Android.Runtime.IJavaObject`](xref:Android.Runtime.IJavaObject)
из-за сведений о реализации привязки, поэтому Классы прослушивателей должны реализовывать этот интерфейс. Это можно сделать, реализовав интерфейс прослушивателя для подкласса [Java. lang. Object](xref:Java.Lang.Object) или любого другого упакованного объекта Java, например действия Android.

### <a name="runnables"></a>руннаблес

Java использует интерфейс [Java. lang. готов к запуску](xref:Java.Lang.Runnable) для предоставления механизма делегирования. Класс [Java. lang. Thread](xref:Java.Lang.Thread) является важным потребителем этого интерфейса. Android также использует интерфейс в API.
[Действия. рунонуисреад ()](xref:Android.App.Activity.RunOnUiThread*) и [View.POST ()](xref:Android.Views.View.Post*) представляют собой важные примеры.

`Runnable`Интерфейс содержит один метод void, [Run ()](xref:Java.Lang.Runnable.Run). Таким образом, он самостоятельно привязывается в C# как делегат [System. Action](xref:System.Action) . Мы предоставили перегрузки в привязке, которые принимают `Action` параметр для всех элементов API, которые используют `Runnable` в собственном API, например [Activity. рунонуисреад ()](xref:Android.App.Activity.RunOnUiThread*) и [View.POST ()](xref:Android.Views.View.Post*).

Мы оставили перегрузки [интерфейс irunnable](xref:Java.Lang.IRunnable) вместо того, чтобы заменять их, так как несколько типов реализуют интерфейс и поэтому могут передаваться как руннаблес напрямую.

### <a name="inner-classes"></a>Внутренние классы

В Java есть два разных типа [вложенных классов](https://download.oracle.com/javase/tutorial/java/javaOO/nested.html): статические вложенные классы и нестатические классы.

Статические вложенные классы Java идентичны вложенным типам C#.

Нестатические вложенные классы, также называемые *внутренними классами*, значительно отличаются. Они содержат неявную ссылку на экземпляр своего включающего типа и не могут содержать статические члены (помимо других различий за пределами области этого обзора).

Когда речь идет о привязке и использовании C#, статические вложенные классы обрабатываются как обычные вложенные типы. В то же время внутренние классы имеют два существенных отличия:

1. Неявная ссылка на вмещающий тип должна быть явно указана в качестве параметра конструктора.

1. При наследовании от внутреннего класса внутренний класс *должен* быть вложен в тип, который наследуется от вмещающего типа базового внутреннего класса, а производный тип должен предоставлять конструктор того же типа, что и тип C#, содержащий.

Например, рассмотрим внутренний класс [Android. Service. обоев. валлпаперсервице. Engine](xref:Android.Service.Wallpaper.WallpaperService.Engine) . Поскольку это внутренний класс, [конструктор валлпаперсервице. Engine ()](xref:Android.Service.Wallpaper.WallpaperService.Engine#ctor) принимает ссылку на экземпляр [валлпаперсервице](xref:Android.Service.Wallpaper.WallpaperService) (Сравните и сравните с конструктором Java валлпаперсервице. Engine (), который не принимает параметров).

Примером наследования внутреннего класса является Кубеваллпапер. Кубингине:

```csharp
class CubeWallpaper : WallpaperService {
    public override WallpaperService.Engine OnCreateEngine ()
    {
        return new CubeEngine (this);
    }

    class CubeEngine : WallpaperService.Engine {
        public CubeEngine (CubeWallpaper s)
                : base (s)
        {
        }
    }
}
```

Обратите внимание `CubeWallpaper.CubeEngine` , как вложен в `CubeWallpaper` , `CubeWallpaper` наследует от класса, содержащего класс `WallpaperService.Engine` , и `CubeWallpaper.CubeEngine` имеет конструктор, принимающий объявляющий тип, `CubeWallpaper` в данном случае — все, как указано выше.

### <a name="interfaces"></a>Интерфейсы

Интерфейсы Java могут содержать три набора элементов, два из которых вызывают проблемы с C#:

1. Методы

1. Типы

1. Поля

Интерфейсы Java преобразуются в два типа:

1. Интерфейс (необязательный), содержащий объявления методов. Этот интерфейс имеет то же имя, что и интерфейс Java, *за исключением* того, что у него также есть префикс " *I* ".

1. Статический класс (необязательный), содержащий любые поля, объявленные в интерфейсе Java.

Вложенные типы "перемещены", которые являются одноуровневыми элементами включающего интерфейса, а не вложенными типами, при этом имя интерфейса заключается в качестве префикса.

Например, рассмотрим интерфейс [Android. OS. для загрузки](xref:Android.OS.Parcelable) .
Переданный *интерфейс* содержит методы, вложенные типы и константы. Методы, *Доступные для* загрузки, помещаются в интерфейс [Android. OS. ипарцелабле](xref:Android.OS.IParcelable) .
Постоянные *интерфейсные* константы помещаются в тип [Android. OS. парцелаблеконстс](xref:Android.OS.ParcelableConsts) . Вложенные типы с [Android. \<T> OS. класслоадеркреатор](https://developer.android.com/reference/android/os/Parcelable.ClassLoaderCreator.html) и Android. [OS. \<T> ](https://developer.android.com/reference/android/os/Parcelable.Creator.html) Supported. Creator в настоящее время не связаны из-за ограничений в поддержке универсальных шаблонов; если они поддерживались, они будут представлены в качестве интерфейсов *Android. OS. ипарцелаблекласслоадеркреатор* и *Android. OS. ипарцелаблекреатор* . Например, вложенный интерфейс [Android. OS. IBinder. деасреЦипиент](https://developer.android.com/reference/android/os/IBinder.DeathRecipient.html) привязан как интерфейс [Android. OS. ибиндердеасреЦипиент](xref:Android.OS.IBinderDeathRecipient) .

> [!NOTE]
> Начиная с Xamarin. Android 1,9, константы интерфейса Java _дублируются_ в целях упрощения переноса кода Java. Это помогает улучшить перенос кода Java, основанного на константах интерфейса [поставщика Android](https://developer.android.com/reference/android/provider/package-summary.html) .

Помимо перечисленных выше типов, существуют четыре дальнейших изменения:

1. Тип с тем же именем, что и у интерфейса Java, создается для хранения констант.

1. Типы, содержащие константы интерфейса, также содержат все константы, полученные из реализованных интерфейсов Java.

1. Все классы, реализующие интерфейс Java, содержащий константы, получают новый вложенный тип Интерфацеконстс, который содержит константы из всех реализованных интерфейсов.

1. Тип *констант* теперь устарел.

Для интерфейса *Android. OS. для загрузки* это означает, что теперь будет использоваться тип [*Android. OS.*](xref:Android.OS.Parcelable) для хранения констант. Например, [подCONTENTS_FILE_DESCRIPTORная](https://developer.android.com/reference/android/os/Parcelable.html#CONTENTS_FILE_DESCRIPTOR) константа будет привязана как константа для [*подконтентсфиледескриптора*](xref:Android.OS.Parcelable.ContentsFileDescriptor) , а не как константа *парцелаблеконстс. контентсфиледескриптор* .

Для интерфейсов, содержащих константы, которые реализуют другие интерфейсы, содержащие еще больше констант, теперь создается объединение всех констант. Например, интерфейс [Android. Provider. медиасторе. Video. видеоколумнс](https://developer.android.com/reference/android/provider/MediaStore.Video.VideoColumns.html) реализует интерфейс [Android. Provider. медиасторе. медиаколумнс](xref:Android.Provider.MediaStore.MediaColumns) . Однако до 1,9 тип [Android. Provider. медиасторе. Video. видеоколумнсконстс](xref:Android.Provider.MediaStore.Video.VideoColumnsConsts) не имеет способа доступа к константам, объявленным в [Android. Provider. медиасторе. медиаколумнсконстс](xref:Android.Provider.MediaStore.MediaColumnsConsts).
В результате выражение Java *медиасторе. Video. видеоколумнс. Title* должно быть привязано к выражению C# *медиасторе. Video. медиаколумнсконстс. Title* , которое трудно обнаружить, не читая много документации по Java. В 1,9 эквивалентным выражением C# будет [*медиасторе. Video. видеоколумнс. Title*](xref:Android.Provider.MediaStore.Video.VideoColumns.Title).

Кроме того, рассмотрим тип [Android. OS. пучок](xref:Android.OS.Bundle) , который *реализует интерфейс,* поддерживающий Java. Поскольку он реализует интерфейс, все константы в этом интерфейсе доступны "через" тип пакета, например, *пакет. CONTENTS_FILE_DESCRIPTOR* является идеально допустимым выражением Java.
Ранее для переноса этого выражения на C# необходимо просмотреть все интерфейсы, которые реализуются для просмотра, из какого типа *CONTENTS_FILE_DESCRIPTOR* поступил. Начиная с Xamarin. Android 1,9, классы, реализующие интерфейсы Java, которые содержат константы, будут иметь вложенный тип *интерфацеконстс* , который будет содержать все наследуемые константы интерфейса. Это позволит выполнить трансляцию *пакета. CONTENTS_FILE_DESCRIPTOR* в [*пакет. интерфацеконстс. контентсфиледескриптор*](xref:Android.OS.Bundle.InterfaceConsts.ContentsFileDescriptor).

Наконец, типы с суффиксом *const* , например *Android. OS. Парцелаблеконстс* , теперь устарели, кроме вновь появившихся вложенных типов интерфацеконстс. Они будут удалены в Xamarin. Android 3,0.

## <a name="resources"></a>Ресурсы

Изображения, описания макетов, двоичные BLOB-объекты и словари строк можно включать в приложение в качестве [файлов ресурсов](https://developer.android.com/guide/topics/resources/providing-resources.html).
Различные API-интерфейсы Android предназначены для [работы с идентификаторами ресурсов](https://developer.android.com/guide/topics/resources/accessing-resources.html) вместо работы с изображениями, строками или двоичными BLOB-объектами напрямую.

Например, пример приложения Android, содержащего макет пользовательского интерфейса ( `main.axml` ), строка таблицы интернационализации ( `strings.xml` ) и некоторые значки ( `drawable-*/icon.png` ) сохранит свои ресурсы в каталоге "Resources" приложения:

```
Resources/
    drawable-hdpi/
        icon.png

    drawable-ldpi/
        icon.png

    drawable-mdpi/
        icon.png

    layout/
        main.axml

    values/
        strings.xml
```

Собственные API-интерфейсы Android не работают непосредственно с именами файлов, а работают с идентификаторами ресурсов. При компиляции приложения Android, использующего ресурсы, система сборки упаковывает ресурсы для распространения и создает класс `Resource` с именем, который содержит маркеры для каждого из включаемых ресурсов. Например, для макета перечисленных выше ресурсов это то, что будет предоставлять класс R:

```csharp
public class Resource {
    public class Drawable {
        public const int icon = 0x123;
    }

    public class Layout {
        public const int main = 0x456;
    }

    public class String {
        public const int first_string = 0xabc;
        public const int second_string = 0xbcd;
    }
}
```

Затем следует использовать `Resource.Drawable.icon` для ссылки на `drawable/icon.png` файл или `Resource.Layout.main` для ссылки на `layout/main.xml` файл или `Resource.String.first_string` для ссылки на первую строку в файле словаря `values/strings.xml` .

## <a name="constants-and-enumerations"></a>Константы и перечисления

Собственные API-интерфейсы Android имеют много методов, которые принимают или возвращают int, который должен быть сопоставлен с постоянным полем, чтобы определить, что означает int. Чтобы использовать эти методы, пользователю необходимо обратиться к документации, чтобы узнать, какие константы соответствуют значениям, что меньше идеально.

Например, рассмотрим [Activity. рекуествиндовфеатуре (int феатуреид)](https://developer.android.com/reference/android/app/Activity.html#requestWindowFeature(int)).

В таких случаях мы представили группирование связанных констант в перечисление .NET и переназначьте метод для получения перечисления.
Таким образом, мы можем предложить выборку возможных значений в IntelliSense.

Приведенный выше пример превращается в: [Activity. рекуествиндовфеатуре (Виндовфеатурес феатуреид)](xref:Android.App.Activity.RequestWindowFeature*).

Обратите внимание, что этот процесс выполняется вручную, чтобы определить, какие константы относятся к друг другу, и какие интерфейсы API используют эти константы. Ошибки в файлах для любых констант, используемых в API, которые лучше выразить как перечисление.