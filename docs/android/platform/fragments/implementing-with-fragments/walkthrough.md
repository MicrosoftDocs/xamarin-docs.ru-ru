---
title: Пошаговое руководство по фрагментам в Xamarin.Android. Часть 1
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: ED368FA9-A34E-DC39-D535-5C34C32B9761
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/21/2018
ms.openlocfilehash: 56c9fc307166ec9c72ab6cb1f7a726c4cd12cb3e
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997037"
---
# <a name="fragments-walkthrough-ndash-phone"></a>Пошаговое руководство по фрагментам для смартфонов &ndash;

Это первая часть пошагового руководства, в рамках которого будет создано приложение Xamarin.Android, предназначенное для устройства Android в книжной ориентации. В этом пошаговом руководстве рассказывается, как создавать фрагменты в Xamarin.Android и как добавлять их в пример.

[![Снимок экрана приложения с пошаговыми инструкциями](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

Для этого приложения будут созданы следующие классы:

1. `PlayQuoteFragment` &nbsp; — этот фрагмент позволяет отобразить цитату из пьесы Уильяма Шекспира. Он размещается в `PlayQuoteActivity`.
1. `Shakespeare` &nbsp; — этот класс будет содержать два жестко зафиксированных массива в качестве свойств.
1. `TitlesFragment` &nbsp; — этот фрагмент позволяет отобразить список названий пьес, написанных Уильямом Шекспиром. Он размещается в `MainActivity`.
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` запускает `PlayQuoteActivity` в ответ на выбор пьесы пользователем в `TitlesFragment`.

## <a name="1-create-the-android-project"></a>1. Создание проекта Android

Создайте проект Xamarin.Android с именем **FragmentSample**.
# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Создание проекта Xamarin.Android](./walkthrough-images/01-newproject.w157-sml.png)](./walkthrough-images/01-newproject.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Создание проекта Xamarin.Android](./walkthrough-images/01-newproject.m742-sml.png)](./walkthrough-images/01-newproject.m742.png#lightbox)

Для работы с этим пошаговым руководством рекомендуем выбрать **современную разработку**.

После создания проекта переименуйте файл **layout/Main.axml** в **layout/activity_main.axml**.

-----

## <a name="2-add-the-data"></a>2. Добавление данных

Данные для этого приложения будут храниться в двух жестко зафиксированных строковых массивах, являющихся свойствами класса с именем `Shakespeare`:

* `Shakespeare.Titles` &nbsp; — этот массив будет содержать список пьес Уильяма Шекспира. Это источник данных для `TitlesFragment`.
* `Shakespeare.Dialogue` &nbsp; — этот массив будет содержать список цитат из одной из пьес, указанных в `Shakespeare.Titles`. Это источник данных для `PlayQuoteFragment`.

Добавьте новый класс C# в проект **FragmentSample** и присвойте ему имя **Shakespeare.cs**. В этом файле создайте новый класс C# с именем `Shakespeare` и следующим содержимым:

```csharp
class Shakespeare
{
    public static string[] Titles = {
                                      "Henry IV (1)",
                                      "Henry V",
                                      "Henry VIII",
                                      "Richard II",
                                      "Richard III",
                                      "Merchant of Venice",
                                      "Othello",
                                      "King Lear"
                                    };

    public static string[] Dialogue = {
                                        "So shaken as we are, so wan with care, Find we a time for frighted peace to pant, And breathe short-winded accents of new broils To be commenced in strands afar remote. No more the thirsty entrance of this soil Shall daub her lips with her own children's blood; Nor more shall trenching war channel her fields, Nor bruise her flowerets with the armed hoofs Of hostile paces: those opposed eyes, Which, like the meteors of a troubled heaven, All of one nature, of one substance bred, Did lately meet in the intestine shock And furious close of civil butchery Shall now, in mutual well-beseeming ranks, March all one way and be no more opposed Against acquaintance, kindred and allies: The edge of war, like an ill-sheathed knife, No more shall cut his master. Therefore, friends, As far as to the sepulchre of Christ, Whose soldier now, under whose blessed cross We are impressed and engaged to fight, Forthwith a power of English shall we levy; Whose arms were moulded in their mothers' womb To chase these pagans in those holy fields Over whose acres walk'd those blessed feet Which fourteen hundred years ago were nail'd For our advantage on the bitter cross. But this our purpose now is twelve month old, And bootless 'tis to tell you we will go: Therefore we meet not now. Then let me hear Of you, my gentle cousin Westmoreland, What yesternight our council did decree In forwarding this dear expedience.",
                                        "Hear him but reason in divinity, And all-admiring with an inward wish You would desire the king were made a prelate: Hear him debate of commonwealth affairs, You would say it hath been all in all his study: List his discourse of war, and you shall hear A fearful battle render'd you in music: Turn him to any cause of policy, The Gordian knot of it he will unloose, Familiar as his garter: that, when he speaks, The air, a charter'd libertine, is still, And the mute wonder lurketh in men's ears, To steal his sweet and honey'd sentences; So that the art and practic part of life Must be the mistress to this theoric: Which is a wonder how his grace should glean it, Since his addiction was to courses vain, His companies unletter'd, rude and shallow, His hours fill'd up with riots, banquets, sports, And never noted in him any study, Any retirement, any sequestration From open haunts and popularity.",
                                        "I come no more to make you laugh: things now, That bear a weighty and a serious brow, Sad, high, and working, full of state and woe, Such noble scenes as draw the eye to flow, We now present. Those that can pity, here May, if they think it well, let fall a tear; The subject will deserve it. Such as give Their money out of hope they may believe, May here find truth too. Those that come to see Only a show or two, and so agree The play may pass, if they be still and willing, I'll undertake may see away their shilling Richly in two short hours. Only they That come to hear a merry bawdy play, A noise of targets, or to see a fellow In a long motley coat guarded with yellow, Will be deceived; for, gentle hearers, know, To rank our chosen truth with such a show As fool and fight is, beside forfeiting Our own brains, and the opinion that we bring, To make that only true we now intend, Will leave us never an understanding friend. Therefore, for goodness' sake, and as you are known The first and happiest hearers of the town, Be sad, as we would make ye: think ye see The very persons of our noble story As they were living; think you see them great, And follow'd with the general throng and sweat Of thousand friends; then in a moment, see How soon this mightiness meets misery: And, if you can be merry then, I'll say A man may weep upon his wedding-day.",
                                        "First, heaven be the record to my speech! In the devotion of a subject's love, Tendering the precious safety of my prince, And free from other misbegotten hate, Come I appellant to this princely presence. Now, Thomas Mowbray, do I turn to thee, And mark my greeting well; for what I speak My body shall make good upon this earth, Or my divine soul answer it in heaven. Thou art a traitor and a miscreant, Too good to be so and too bad to live, Since the more fair and crystal is the sky, The uglier seem the clouds that in it fly. Once more, the more to aggravate the note, With a foul traitor's name stuff I thy throat; And wish, so please my sovereign, ere I move, What my tongue speaks my right drawn sword may prove.",
                                        "Now is the winter of our discontent Made glorious summer by this sun of York; And all the clouds that lour'd upon our house In the deep bosom of the ocean buried. Now are our brows bound with victorious wreaths; Our bruised arms hung up for monuments; Our stern alarums changed to merry meetings, Our dreadful marches to delightful measures. Grim-visaged war hath smooth'd his wrinkled front; And now, instead of mounting barded steeds To fright the souls of fearful adversaries, He capers nimbly in a lady's chamber To the lascivious pleasing of a lute. But I, that am not shaped for sportive tricks, Nor made to court an amorous looking-glass; I, that am rudely stamp'd, and want love's majesty To strut before a wanton ambling nymph; I, that am curtail'd of this fair proportion, Cheated of feature by dissembling nature, Deformed, unfinish'd, sent before my time Into this breathing world, scarce half made up, And that so lamely and unfashionable That dogs bark at me as I halt by them; Why, I, in this weak piping time of peace, Have no delight to pass away the time, Unless to spy my shadow in the sun And descant on mine own deformity: And therefore, since I cannot prove a lover, To entertain these fair well-spoken days, I am determined to prove a villain And hate the idle pleasures of these days. Plots have I laid, inductions dangerous, By drunken prophecies, libels and dreams, To set my brother Clarence and the king In deadly hate the one against the other: And if King Edward be as true and just As I am subtle, false and treacherous, This day should Clarence closely be mew'd up, About a prophecy, which says that 'G' Of Edward's heirs the murderer shall be. Dive, thoughts, down to my soul: here Clarence comes.",
                                        "To bait fish withal: if it will feed nothing else, it will feed my revenge. He hath disgraced me, and hindered me half a million; laughed at my losses, mocked at my gains, scorned my nation, thwarted my bargains, cooled my friends, heated mine enemies; and what's his reason? I am a Jew. Hath not a Jew eyes? hath not a Jew hands, organs, dimensions, senses, affections, passions? fed with the same food, hurt with the same weapons, subject to the same diseases, healed by the same means, warmed and cooled by the same winter and summer, as a Christian is? If you prick us, do we not bleed? if you tickle us, do we not laugh? if you poison us, do we not die? and if you wrong us, shall we not revenge? If we are like you in the rest, we will resemble you in that. If a Jew wrong a Christian, what is his humility? Revenge. If a Christian wrong a Jew, what should his sufferance be by Christian example? Why, revenge. The villany you teach me, I will execute, and it shall go hard but I will better the instruction.",
                                        "Virtue! a fig! 'tis in ourselves that we are thus or thus. Our bodies are our gardens, to the which our wills are gardeners: so that if we will plant nettles, or sow lettuce, set hyssop and weed up thyme, supply it with one gender of herbs, or distract it with many, either to have it sterile with idleness, or manured with industry, why, the power and corrigible authority of this lies in our wills. If the balance of our lives had not one scale of reason to poise another of sensuality, the blood and baseness of our natures would conduct us to most preposterous conclusions: but we have reason to cool our raging motions, our carnal stings, our unbitted lusts, whereof I take this that you call love to be a sect or scion.",
                                        "Blow, winds, and crack your cheeks! rage! blow! You cataracts and hurricanoes, spout Till you have drench'd our steeples, drown'd the cocks! You sulphurous and thought-executing fires, Vaunt-couriers to oak-cleaving thunderbolts, Singe my white head! And thou, all-shaking thunder, Smite flat the thick rotundity o' the world! Crack nature's moulds, an germens spill at once, That make ingrateful man!"
                                    };
}
```

## <a name="3-create-the-playquotefragment"></a>3. Создание PlayQuoteFragment

С помощью фрагмента Android `PlayQuoteFragment` будет отображаться цитата из пьесы Шекспира, выбранной пользователем ранее в приложении. Этот фрагмент не будет использовать файл макета Android, а будет динамически создавать свой пользовательский интерфейс. Добавьте в проект новый класс `Fragment` с именем `PlayQuoteFragment`.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Добавление нового класса C#](./walkthrough-images/04-addfragment.w157-sml.png)](./walkthrough-images/02-addclass.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Добавление нового класса C#](./walkthrough-images/04-addfragment.m742-sml.png)](./walkthrough-images/02-addclass.m742.png#lightbox)

-----

Затем измените код этого фрагмента таким образом, чтобы он соответствовал следующему фрагменту кода:

```csharp
public class PlayQuoteFragment : Fragment
{
    public int PlayId => Arguments.GetInt("current_play_id", 0);

    public static PlayQuoteFragment NewInstance(int playId)
    {
        var bundle = new Bundle();
        bundle.PutInt("current_play_id", playId);
        return new PlayQuoteFragment {Arguments = bundle};
    }

    public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
    {
        if (container == null)
        {
            return null;
        }

        var textView = new TextView(Activity);
        var padding = Convert.ToInt32(TypedValue.ApplyDimension(ComplexUnitType.Dip, 4, Activity.Resources.DisplayMetrics));
        textView.SetPadding(padding, padding, padding, padding);
        textView.TextSize = 24;
        textView.Text = Shakespeare.Dialogue[PlayId];

        var scroller = new ScrollView(Activity);
        scroller.AddView(textView);

        return scroller;
    }
}
```

В приложениях Android часто используется методика предоставления фабричного метода, с помощью которого будет создаваться экземпляр фрагмента. Это гарантирует, что фрагмент будет создан с необходимыми параметрами для правильной работы. В этом пошаговом руководстве предполагается, что приложение применит метод `PlayQuoteFragment.NewInstance` для создания нового фрагмента при каждом выборе цитаты. Метод `NewInstance` принимает один параметр &ndash;, который обозначает индекс отображаемой цитаты.

Android будет вызывать метод `OnCreateView` для отображения фрагмента на экране. Метод возвращает объект Android `View`, который является фрагментом. Этот фрагмент не использует файл макета для создания представления. Вместо этого он будет программными средствами создавать представление, создавая экземпляр **TextView** для хранения цитаты, и отображать мини-приложение в **ScrollView**.

> [!NOTE]
> Подклассы фрагмента должны использовать открытый конструктор по умолчанию без параметров.

## <a name="4-create-the-playquoteactivity"></a>4. Создание PlayQuoteActivity

Фрагменты должны размещаться внутри действия. Поэтому для этого приложения нужно создать действие, в котором будет размещаться `PlayQuoteFragment`. Действие позволит динамически добавить этот фрагмент в макет во время выполнения. Добавьте в приложение новое действие и присвойте ему имя `PlayQuoteActivity`.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Добавление действия Android в проект](./walkthrough-images/03-addactivity.w157-sml.png)](./walkthrough-images/03-addactivity.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Добавление действия Android в проект](./walkthrough-images/03-addactivity.m742-sml.png)](./walkthrough-images/03-addactivity.m742.png#lightbox)

-----

Измените код в `PlayQuoteActivity` следующим образом:

```csharp
[Activity(Label = "PlayQuoteActivity")]
public class PlayQuoteActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        var playId = Intent.Extras.GetInt("current_play_id", 0);

        var detailsFrag = PlayQuoteFragment.NewInstance(playId);
        FragmentManager.BeginTransaction()
                        .Add(Android.Resource.Id.Content, detailsFrag)
                        .Commit();
    }
}
```

При создании `PlayQuoteActivity` он будет создавать экземпляр нового фрагмента `PlayQuoteFragment` и загружать его в корневое представление в контексте `FragmentTransaction`. Обратите внимание, что при этом действии файл макета Android не загружается для пользовательского интерфейса. Вместо этого в корневое представление приложения добавляется новый фрагмент `PlayQuoteFragment`. Идентификатор ресурса `Android.Resource.Id.Content` используется для ссылки на корневое представление действия без указания его идентификатора.

## <a name="5-create-titlesfragment"></a>5. Создание TitlesFragment

`TitlesFragment` позволяет создать подкласс специализированного фрагмента, известного как `ListFragment`, который содержит логику для отображения `ListView` в нашем фрагменте. `ListFragment` предоставляет свойство `ListAdapter` (которое `ListView` использует для отображения содержимого) и обработчик событий с именем `OnListItemClick`, что позволяет фрагменту реагировать на нажатия строки, отображаемой `ListView`.

Чтобы приступить к работе, добавьте в проект новый фрагмент и присвойте ему имя **TitlesFragment**:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Добавление фрагмента Android в проект](./walkthrough-images/04-addfragment.w157-sml.png)](./walkthrough-images/04-addfragment.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Добавление фрагмента Android в проект](./walkthrough-images/04-addfragment.m742-sml.png)](./walkthrough-images/04-addfragment.m742.png#lightbox)

-----

Измените код внутри фрагмента следующим образом:

```csharp
public class TitlesFragment : ListFragment
{
    int selectedPlayId;

    public TitlesFragment()
    {
        // Being explicit about the requirement for a default constructor.
    }

    public override void OnActivityCreated(Bundle savedInstanceState)
    {
        base.OnActivityCreated(savedInstanceState);
        ListAdapter = new ArrayAdapter<String>(Activity, Android.Resource.Layout.SimpleListItemActivated1, Shakespeare.Titles);

        if (savedInstanceState != null)
        {
            selectedPlayId = savedInstanceState.GetInt("current_play_id", 0);
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
        var intent = new Intent(Activity, typeof(PlayQuoteActivity));
        intent.PutExtra("current_play_id", playId);
        StartActivity(intent);
    }
}
```

При создании действия Android вызовет метод `OnActivityCreated` из фрагмента. На этом этапе создается адаптер списка для `ListView`.  Метод `ShowQuoteFromPlay` запустит экземпляр `PlayQuoteActivity`, чтобы отобразить цитату из выбранной пьесы.

## <a name="display-titlesfragment-in-mainactivity"></a>Отображение TitlesFragment в MainActivity

И наконец, нам нужно отобразить `TitlesFragment` внутри `MainActivity`. При действии фрагмент не загружается динамически. Вместо этого фрагмент будет статически загружен путем объявления его в элементе `fragment` файла макета для этого действия. Загружаемый фрагмент определяется путем назначения атрибута `android:name` классу фрагмента (включая пространство имен для типа). Например, для использования `TitlesFragment` свойству `android:name` нужно присвоить значение `FragmentSample.TitlesFragment`.

Измените файл макета **activity_main.axml**, заменив существующий XML следующим:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <fragment
        android:name="FragmentSample.TitlesFragment"
        android:id="@+id/titles"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

> [!NOTE]
> Атрибут `class` — допустимая замена для `android:name`. Нет официальных рекомендаций по выбору того или иного варианта. Во многих базах кода `class` и `android:name` применяются наравне друг с другом.

В действие MainActivity не нужно вносить никаких изменений кода. Код в этом классе теперь быть выглядеть примерно так:

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        SetContentView(Resource.Layout.activity_main);
    }
}
```

## <a name="run-the-app"></a>Запуск приложения

Теперь, когда весь код готов, запустите приложение на устройстве, чтобы увидеть его в работе.

[![Снимки экрана приложения, работающего на телефоне.](./walkthrough-images/05-app-screenshots-sml.png)](./walkthrough-images/05-app-screenshots.png#lightbox)

В рамках [части 2 этого пошагового руководства](./walkthrough-landscape.md) вы оптимизируете это приложение для устройств, работающих в альбомном режиме.
