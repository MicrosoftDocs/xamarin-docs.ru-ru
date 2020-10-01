---
title: Обработка речи в Android
description: В этой статье рассматриваются принципы работы с мощным пространством имен Android.Speech. С момента своего создания платформа Android умеет распознавать речь и выводить ее в виде текста. Этот процесс относительно прост. Но обратное преобразование текста в речь влечет больше сложностей, так как в нем нужно учитывать не только подсистему обработки речи, но и доступные языки, установленные из системы преобразования текста в речь (TTS).
ms.prod: xamarin
ms.assetid: FA3B8EC4-34D2-47E3-ACEA-BD34B28115B9
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/02/2018
ms.openlocfilehash: ce94179fe0ae7f3df1f2fa5ead20ec09fd6f8f5e
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457917"
---
# <a name="android-speech"></a>Обработка речи в Android

_В этой статье рассматриваются принципы работы с мощным пространством имен Android.Speech. С момента своего создания платформа Android умеет распознавать речь и выводить ее в виде текста. Этот процесс относительно прост. Но обратное преобразование текста в речь влечет больше сложностей, так как в нем нужно учитывать не только подсистему обработки речи, но и доступные языки, установленные из системы преобразования текста в речь (TTS)._

## <a name="speech-overview"></a>Обзор системы обработки речи

Наличие системы, которая "понимает" человеческий голос и проговаривает вводимые данные (преобразование речи в текст и текста в речь) — это активно развивающееся направление в разработке мобильных приложений, так как потребность в естественном взаимодействии с устройствами постоянно растет. Существует множество ситуаций, в которых функция преобразования текста в речь или речи в текст станет очень полезным инструментом для внедрения в приложение для Android.

Например, после запрета использования мобильных телефонов за рулем пользователи ищут способы управлять устройствами без использования рук. Множество разных форм-факторов устройств Android, включая Android Wear, и постоянно растущее разнообразие устройств с Android (например, планшеты и блокноты) создают значительный интерес к хорошим приложениям с системой TTS.

Google предоставляет разработчикам через пространство имен Android.Speech богатый набор API-интерфейсов, которые позволяют устройствам "понимать речь" в большинстве сценариев (например, программное обеспечение для слепых людей).  Это пространство имен содержит возможности для преобразования текста в речь через `Android.Speech.Tts` и управления подсистемой преобразования, а также несколько классов `RecognizerIntent` для обратного преобразования речи в текст.

Но даже при наличии возможностей для работы с речью нельзя забывать об аппаратных ограничениях. Не стоит ожидать, что устройство сможет успешно интерпретировать любую речь на любом из существующих языков.

## <a name="requirements"></a>Requirements (Требования)

Для работы с этим руководством нет особых требований. Вам потребуется только устройство с микрофоном и динамиком.

Основной системы интерпретации речи на устройствах Android является метод `Intent` с соответствующим объектом `OnActivityResult`.
Но здесь важно понимать, что этот процесс не подразумевает понимание речи, а только преобразование речи в текст. Это очень важное различие.

### <a name="the-difference-between-understanding-and-interpreting"></a>Разница между пониманием и преобразованием

Если по-простому, то понимание — это возможность понять реальное значение сказанного с учетом тона и контекста. Преобразование же сводится к выделению слов и выводу их в другой форме.

Рассмотрим простой пример из нашей ежедневной жизни.

<kbd>Привет! Как дела?</kbd>

Без выделения интонацией отдельных слов или их частей это будет простой вопрос. Но если произнести эту фразу медленно, человек быстро почувствует, что спрашивающий находится в плохом расположении духа, нуждается в поддержке или даже болен. Если слово "дела" выделяется интонационно, это обычно означает более высокую заинтересованность в ответе.

Без довольно мощных возможностей обработки звука, позволяющих анализировать интонацию, и искусственного интеллекта для анализа контекста программное обеспечение даже не приблизится к пониманию смысла — обычный смартфон может лишь преобразовать речь в текст.

## <a name="setting-up"></a>Настройка

Перед началом работы проверьте наличие микрофона на устройстве. Нет никакого смысла запускать приложение на устройстве Kindle или Google, на котором нет микрофона.

Следующий пример кода демонстрирует запрос, который проверяет наличие микрофона и, если микрофона нет, создает предупреждение. Если микрофон на этом этапе не обнаружен, вы можете завершить текущее действие или отключить функцию записи речи.

```csharp
string rec = Android.Content.PM.PackageManager.FeatureMicrophone;
if (rec != "android.hardware.microphone")
{
    var alert = new AlertDialog.Builder(recButton.Context);
    alert.SetTitle("You don't seem to have a microphone to record with");
    alert.SetPositiveButton("OK", (sender, e) =>
    {
        return;
    });
    alert.Show();
}
```

### <a name="creating-the-intent"></a>Создание намерения

Система обработки речи использует определенный тип намерения, которое называется `RecognizerIntent`. Это намерение управляет большим числом параметров, в том числе временем ожидания в тишине до завершения записи, возможностью распознавать и выводить текст на других языках, выводом дополнительных инструкций в модальном диалоге `Intent` и т. д. В этом фрагменте кода `VOICE` представляет собой намерение `readonly int`, которое используется в `OnActivityResult` для распознавания речи.

```csharp
var voiceIntent = new Intent(RecognizerIntent.ActionRecognizeSpeech);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguageModel, RecognizerIntent.LanguageModelFreeForm);
voiceIntent.PutExtra(RecognizerIntent.ExtraPrompt, Application.Context.GetString(Resource.String.messageSpeakNow));
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputPossiblyCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputMinimumLengthMillis, 15000);
voiceIntent.PutExtra(RecognizerIntent.ExtraMaxResults, 1);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguage, Java.Util.Locale.Default);
StartActivityForResult(voiceIntent, VOICE);
```

### <a name="conversion-of-the-speech"></a>Преобразование речи

Текст, полученный на основе речи, предоставляется в намерение `Intent`, которое возвращается после завершения действия, а доступ к нему осуществляется через `GetStringArrayListExtra(RecognizerIntent.ExtraResults)`. Здесь возвращается список `IList<string>`, для которого можно применять и отображать индекс в зависимости от числа языков, запрошенных в вызове намерения (и указанных в `RecognizerIntent.ExtraMaxResults`). Но как и с любым другим списком, есть смысл проверить наличие данных для отображения.

При ожидании возвращаемого значения `StartActivityForResult` нужно предоставить метод `OnActivityResult`.

В следующем примере `textBox` представляет собой `TextBox` для вывода диктуемого текста. Его можно с тем же успехом применять для передачи текста в какое-то средство интерпретации, а полученные результаты сравнить с примерами текста и передать в другую часть приложения.

```csharp
protected override void OnActivityResult(int requestCode, Result resultVal, Intent data)
{
    if (requestCode == VOICE)
    {
        if (resultVal == Result.Ok)
        {
            var matches = data.GetStringArrayListExtra(RecognizerIntent.ExtraResults);
            if (matches.Count != 0)
            {
                string textInput = textBox.Text + matches[0];
                textBox.Text = textInput;
                switch (matches[0].Substring(0, 5).ToLower())
                {
                    case "north":
                        MovePlayer(0);
                        break;
                    case "south":
                        MovePlayer(1);
                        break;
                }
            }
            else
            {
                textBox.Text = "No speech was recognised";
            }
        }
        base.OnActivityResult(requestCode, resultVal, data);
    }
}
```

## <a name="text-to-speech"></a>Преобразование текста в речь

Преобразование текста в речь нельзя рассматривать просто как обратную функцию к преобразованию речи в текст, так как эта функция зависит от двух важнейших компонентов: установленной на устройстве подсистемы преобразования и установленного в системе языка.

В основном устройства Android поставляются с установленной стандартной службой TTS от Google и хотя бы одним языком. Эти параметры определяются при первой настройке устройства и зависят от текущего расположения устройства на тот момент (например, для настраиваемого в Германии телефона будет установлен немецкий язык, а в США — американский английский).

### <a name="step-1---instantiating-texttospeech"></a>Шаг 1. Создание экземпляра TextToSpeech

`TextToSpeech` принимает до трех параметров, `AppContext`, `IOnInitListener` и `engine`, из которых первые два — обязательные. Прослушиватель (listener) используется для привязки к службе и проверки на наличие ошибок, а подсистемой (engine) может быть любое число доступных служб Android подсистем преобразования текста в речь. Как минимум, на устройстве будет подсистема от Google.

### <a name="step-2---finding-the-languages-available"></a>Шаг 2. Поиск доступных языков

Класс `Java.Util.Locale` содержит полезный метод с именем `GetAvailableLocales()`. Теперь можно сопоставить список языков, поддерживаемых подсистемой обработки речи, и языков, установленных в системе.

Из них очевидным образом выводится список "понимаемых" языков. В этом списке всегда будет присутствовать язык по умолчанию (тот, на котором пользователь впервые настраивал это устройство). Таким образом в нашем примере для `List<string>` первым параметром указывается значение Default, а остальные элементы списка заполняются по результатам выполнения `textToSpeech.IsLanguageAvailable(locale)`.

```csharp
var langAvailable = new List<string>{ "Default" };
var localesAvailable = Java.Util.Locale.GetAvailableLocales().ToList();
foreach (var locale in localesAvailable)
{
    var res = textToSpeech.IsLanguageAvailable(locale);
    switch (res)
    {
        case LanguageAvailableResult.Available:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryVarAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
    }
}
langAvailable = langAvailable.OrderBy(t => t).Distinct().ToList();
```

Этот код вызывает [TextToSpeech.IsLanguageAvailable](xref:Android.Speech.Tts.TextToSpeech.IsLanguageAvailable*), чтобы проверить наличие на устройстве языкового пакета для определенного языкового стандарта.
Этот метод возвращает [LanguageAvailableResult](xref:Android.Speech.Tts.LanguageAvailableResult), что позволяет определить, есть ли в системе язык для полученного языкового стандарта. Если `LanguageAvailableResult` имеет значение `NotSupported`, это означает, что для этого языка нет доступного (даже для скачивания) голосового пакета. Если `LanguageAvailableResult` имеет значение `MissingData`, это означает, что можно скачать новый языковой пакет (см. шаг 4).

### <a name="step-3---setting-the-speed-and-pitch"></a>Шаг 3. Настройка скорости и тона речи

Android позволяет пользователям изменять звучание речи через параметры `SpeechRate` и `Pitch` (скорость и тон речи соответственно). Эти параметры принимают значения от 0 до 1, где "обычной" речи соответствует значение 1.

### <a name="step-4---testing-and-loading-new-languages"></a>Шаг 4. Проверка и скачивание новых языков

Для скачивания нового языка используется намерение `Intent`. Результат этого намерения приводит к вызову метода [OnActivityResult](xref:Android.App.Activity.OnActivityResult*). В отличие от примера с преобразованием речи в текст (где [RecognizerIntent](xref:Android.Speech.RecognizerIntent) передается в `Intent` как параметр `PutExtra`), намерения `Intent` для проверки и скачивания основаны на действиях `Action`.

- [TextToSpeech.Engine.ActionCheckTtsData](xref:Android.Speech.Tts.TextToSpeech.Engine.ActionCheckTtsData) &ndash; запускает действие из платформенной подсистемы `TextToSpeech`, чтобы проверить правильность установки и доступность языковых ресурсов на устройстве.

- [TextToSpeech.Engine.ActionInstallTtsData](xref:Android.Speech.Tts.TextToSpeech.Engine.ActionInstallTtsData) &ndash; запускает действие, которое предлагает пользователю скачать нужные языки.

Следующий пример кода демонстрирует применение этих действий для языковых ресурсов и для скачивания нового языка.

```csharp
var checkTTSIntent = new Intent();
checkTTSIntent.SetAction(TextToSpeech.Engine.ActionCheckTtsData);
StartActivityForResult(checkTTSIntent, NeedLang);
//
protected override void OnActivityResult(int req, Result res, Intent data)
{
    if (req == NeedLang)
    {
        var installTTS = new Intent();
        installTTS.SetAction(TextToSpeech.Engine.ActionInstallTtsData);
        StartActivity(installTTS);
    }
}
```

`TextToSpeech.Engine.ActionCheckTtsData` проверяет доступность языковых ресурсов. После завершения этой проверки вызывается `OnActivityResult`. Если нужно скачать языковые ресурсы, `OnActivityResult` запускает действие `TextToSpeech.Engine.ActionInstallTtsData`, которое позволит пользователю скачать нужные языки. Обратите внимание, что реализация `OnActivityResult` не проверяет код `Result`, поскольку в нашем упрощенном примере решение о необходимости скачать языковой пакет уже принято.

Действие `TextToSpeech.Engine.ActionInstallTtsData` предоставляет пользователю интерфейс **голосовых данных для TTS от Google** для выбора скачиваемых языков.

![Выбор голосовых данных для TTS от Google](speech-images/01-google-tts-voice-data.png)

Предположим, что пользователь выбрал французский язык и щелкнул значок скачивания, чтобы получить голосовые данные для французского языка.

![Пример скачивания французского языка](speech-images/02-selecting-french.png)

После завершения скачивания данные будут установлены автоматически.

### <a name="step-5---the-ioninitlistener"></a>Шаг 5. IOnInitListener

Чтобы действие могло преобразовать текст в речь, нужно реализовать метод интерфейса `OnInit` (это второй параметр, который указывается для создания экземпляра класса `TextToSpeech`). Он инициализирует прослушиватель и проверяет результат.

Прослушиватель должен проверять по меньшей мере `OperationResult.Success` и `OperationResult.Failure`.
В примере ниже именно это и показано.

```csharp
void TextToSpeech.IOnInitListener.OnInit(OperationResult status)
{
    // if we get an error, default to the default language
    if (status == OperationResult.Error)
        textToSpeech.SetLanguage(Java.Util.Locale.Default);
    // if the listener is ok, set the lang
    if (status == OperationResult.Success)
        textToSpeech.SetLanguage(lang);
}
```

## <a name="summary"></a>Сводка

В этом руководстве мы рассмотрели основные моменты преобразование текста в речь и речи в текст, а также возможные методы добавления этих технологий в приложения. Хотя мы и не охватили все возможные ситуации, у вас уже есть базовое представление о механизмах интерпретации речи, установке новых языков и повышении инклюзивности приложений.

## <a name="related-links"></a>Связанные ссылки

- [Xamarin.Forms DependencyService](/samples/xamarin/xamarin-forms-samples/dependencyservice//)
- [Преобразование текста в речь (пример)](/samples/xamarin/monodroid-samples/platformfeatures-texttospeech)
- [Преобразование речи в текст (пример)](/samples/xamarin/monodroid-samples/platformfeatures-speechtotext)
- [Пространство имен Android.Speech](xref:Android.Speech)
- [Пространство имен Android.Speech.Tts](xref:Android.Speech.Tts)