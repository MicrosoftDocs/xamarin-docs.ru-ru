---
title: Фрагменты Пошаговое руководство. Часть 2
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: 444A894D-5197-4726-934F-79BA80A71CB0
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/26/2018
ms.openlocfilehash: c0d4b0d1a31be43d16fb69eba18c07815631f496
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91453939"
---
# <a name="fragments-walkthrough-ndash-landscape"></a>Фрагменты Пошаговое руководство &ndash; альбомная ориентация

В [фрагментах пошагового руководства &ndash;, части 1](./walkthrough.md), показано, как создавать и использовать фрагменты в приложении Android, предназначенном для телефонов с небольшими экранами. Следующим шагом в этом пошаговом руководстве является изменение приложения для использования преимуществ в виде дополнительного горизонтального пространства на планшете &ndash;, на котором будет одно действие, которое всегда будет списком воспроизведения (`TitlesFragment`), а `PlayQuoteFragment` будет динамически добавляться к действию в ответ на выбор, сделанный пользователем:

[![Приложение, работающее на планшете](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

На телефонах, работающих в альбомном режиме, также будет выгодно это улучшение:

[![Приложение, работающее на телефоне Android в альбомной ориентации](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>Обновление приложения для работы в альбомной ориентации

Следующие изменения будут основываться на работе, которая была проделана в статье [Фрагменты Пошаговое руководство — телефон](./walkthrough.md).

1. Создайте альтернативный макет для отображения как `TitlesFragment`, так и `PlayQuoteFragment`.
1. Обновите `TitlesFragment`, чтобы определить, отображает ли устройство одновременно оба фрагмента и соответственно изменить поведение.
1. Обновите `PlayQuoteActivity`, чтобы закрыть устройство, если оно в альбомной ориентации.

## <a name="1-create-an-alternate-layout"></a>1. Создание альтернативного макета

Когда основное действие создается на устройстве Android, Android выбирает макет для загрузки в зависимости от ориентации устройства. По умолчанию Android предоставит файл макета **Resources/layout/activity_main.axml**. Для устройств, загружаемых в альбомной ориентации, Android предоставит файл макета **Resources/layout-land/activity_main.axml**. Руководство по [ресурсах Android](../../../app-fundamentals/resources-in-android/index.md) содержит более подробную информацию о способе принятия Android решений о том, какие файлы ресурсов загружать для приложения.

Создайте альтернативный макет, предназначенный для **альбомной** ориентации, выполнив действия, описанные в руководстве [Альтернативные представления макета](../../../user-interface/android-designer/alternative-layout-views.md). При этом в проект необходимо добавить новый файл ресурсов макета, **Resources/layout/activity_main.axml**:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Альтернативный макет в обозревателе решений](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Альтернативный макет в Панели решения](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

-----

После создания альтернативного макета измените источник файла **Resources/layout-land/activity_main.axml** так, чтобы он соответствовал этому XML-документу:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/two_fragments_layout"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <fragment android:name="FragmentSample.TitlesFragment"
        android:id="@+id/titles"
        android:layout_weight="1"
        android:layout_width="0px"
        android:layout_height="match_parent" />

    <FrameLayout android:id="@+id/playquote_container"
            android:layout_weight="1"
            android:layout_width="0px"
            android:layout_height="match_parent"
             />
</LinearLayout>
```

Корневое представление действия получает идентификатор ресурса `two_fragments_layout` и имеет два подпредставления, `fragment` и `FrameLayout`. При статической загрузке `fragment`, `FrameLayout` выступает в качестве заполнителя,который во время выполнения будет заменен на `PlayQuoteFragment`. Каждый раз, когда в `TitlesFragment`выбрано новое воспроизведение, `playquote_container` будет обновлен новым экземпляром `PlayQuoteFragment`.

Каждое из подпредставлений будет занимать полную высоту их родительского элемента. Ширина каждого подпредставления определяется атрибутами `android:layout_weight` и `android:layout_width`. В этом примере каждое подпредставление будет занимать 50 % ширины, предоставляемой родительским элементом. Дополнительные сведения о _весе макета_ см. в статье [LinearLayout](https://developer.android.com/guide/topics/ui/layout/linear.html).

## <a name="2-changes-to-titlesfragment"></a>2. Изменения в TitlesFragment

После создания альтернативного макета необходимо обновить `TitlesFragment`. Когда приложение отображает два фрагмента одного действия, `TitlesFragment` должен загружать `PlayQuoteFragment` в Действии родительского элемента. В противном случае `TitlesFragment` должен запустить `PlayQuoteActivity`, на котором размещается `PlayQuoteFragment`. Логический флаг поможет `TitlesFragment` определить, какое поведение следует использовать. Этот флаг будет инициализирован в методе `OnActivityCreated`.

Сначала добавьте переменную экземпляра в начало класса `TitlesFragment`:

```csharp
bool showingTwoFragments;
```

Затем добавьте следующий фрагмент кода, чтобы `OnActivityCreated` инициализировать переменную: 

```csharp
var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
showingTwoFragments = quoteContainer != null &&
                      quoteContainer.Visibility == ViewStates.Visible;
if (showingTwoFragments)
{
    ListView.ChoiceMode = ChoiceMode.Single;
    ShowPlayQuote(selectedPlayId);
}
```

Если устройство работает в альбомной ориентации, на экране будет отображаться `FrameLayout` с идентификатором ресурса `playquote_container`, поэтому `showingTwoFragments` будет инициализирован в `true`. Если устройство работает в портретном режиме, `playquote_container` не будет отображаться на экране, поэтому `showingTwoFragments` будет `false`.

Методу `ShowPlayQuote` потребуется изменить способ отображения цитаты &ndash; либо в фрагменте, либо при запуске нового действия.  Обновите метод `ShowPlayQuote`, чтобы загрузить фрагмент при отображении двух фрагментов, в противном случае он должен запустить действие:

```csharp
void ShowPlayQuote(int playId)
{
    selectedPlayId = playId;
    if (showingTwoFragments)
    {
        ListView.SetItemChecked(selectedPlayId, true);

        var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

        if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
        {
            var container = Activity.FindViewById(Resource.Id.playquote_container);
            var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

            FragmentTransaction ft = FragmentManager.BeginTransaction();
            ft.Replace(Resource.Id.playquote_container, quoteFrag);
            ft.Commit();
        }
    }
    else
    {
        var intent = new Intent(Activity, typeof(PlayQuoteActivity));
        intent.PutExtra("current_play_id", playId);
        StartActivity(intent);
    }
}
```

Если пользователь выбрал воспроизведение, отличающееся от отображаемого в данный момент в `PlayQuoteFragment`, создается новый `PlayQuoteFragment`, который заменит содержимое `playquote_container` в контексте `FragmentTransaction`.

### <a name="complete-code-for-titlesfragment"></a>Полный код для TitlesFragment

После завершения всех предыдущих изменений в `TitlesFragment`полный класс должен соответствовать этому коду:

```csharp
public class TitlesFragment : ListFragment
{
    int selectedPlayId;
    bool showingTwoFragments;

    public override void OnActivityCreated(Bundle savedInstanceState)
    {
        base.OnActivityCreated(savedInstanceState);
        ListAdapter = new ArrayAdapter<string>(Activity, Android.Resource.Layout.SimpleListItemActivated1, Shakespeare.Titles);

        if (savedInstanceState != null)
        {
            selectedPlayId = savedInstanceState.GetInt("current_play_id", 0);
        }

        var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
        showingTwoFragments = quoteContainer != null &&
                                quoteContainer.Visibility == ViewStates.Visible;
        if (showingTwoFragments)
        {
            ListView.ChoiceMode = ChoiceMode.Single;
            ShowPlayQuote(selectedPlayId);
        }
    }

    public override void OnSaveInstanceState(Bundle outState)
    {
        base.OnSaveInstanceState(outState);
        outState.PutInt("current_play_id", selectedPlayId);
    }

    public override void OnListItemClick(ListView l, View v, int position, long id)
    {
        ShowPlayQuote(position);
    }

    void ShowPlayQuote(int playId)
    {
        selectedPlayId = playId;
        if (showingTwoFragments)
        {
            ListView.SetItemChecked(selectedPlayId, true);

            var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

            if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
            {
                var container = Activity.FindViewById(Resource.Id.playquote_container);
                var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

                FragmentTransaction ft = FragmentManager.BeginTransaction();
                ft.Replace(Resource.Id.playquote_container, quoteFrag);
                ft.AddToBackStack(null);
                ft.SetTransition(FragmentTransit.FragmentFade);
                ft.Commit();
            }
        }
        else
        {
            var intent = new Intent(Activity, typeof(PlayQuoteActivity));
            intent.PutExtra("current_play_id", playId);
            StartActivity(intent);
        }
    }
}
```

## <a name="3-changes-to-playquoteactivity"></a>3. Изменения в PlayQuoteActivity

Есть одна последняя деталь, о которой нужно позаботиться: `PlayQuoteActivity` не требуется, если устройство находится в альбомной ориентации. Если устройство находится в альбомном режиме, `PlayQuoteActivity` не должно быть видимым. Обновите метод `OnCreate` из `PlayQuoteActivity` так, чтобы он закрылся. Этот код является окончательной версией `PlayQuoteActivity.OnCreate`:

```csharp
protected override void OnCreate(Bundle savedInstanceState)
{
    base.OnCreate(savedInstanceState);

    if (Resources.Configuration.Orientation == Android.Content.Res.Orientation.Landscape)
    {
        Finish();
    }

    var playId = Intent.Extras.GetInt("current_play_id", 0);
    var playQuoteFrag = PlayQuoteFragment.NewInstance(playId);
    FragmentManager.BeginTransaction()
                    .Add(Android.Resource.Id.Content, playQuoteFrag)
                    .Commit();
}
```

Это изменение добавляет проверку ориентации устройства. Если он находится в альбомной ориентации, `PlayQuoteActivity` закроется.

## <a name="4-run-the-application"></a>4. Запуск приложения

После завершения этих изменений запустите приложение, переворачивайте устройство в альбомный режим (при необходимости), а затем выберите воспроизведение. Цитата должна отображаться на том же экране, что и список воспроизведения:

[![Приложение, работающее на телефоне Android в альбомной ориентации](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[![Приложение, работающее на планшете Android](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)