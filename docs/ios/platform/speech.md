---
title: Распознавание речи в Xamarin. iOS
description: В этой статье представлен новый API распознавания речи и показано, как реализовать его в приложении Xamarin. iOS для поддержки непрерывного распознавания речи и транскрипция речи (из активных или записанных звуковых потоков) в текст.
ms.prod: xamarin
ms.assetid: 64FED50A-6A28-4833-BEAE-63CEC9A09010
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 98cbcd333d223d741602786643ef1948915d7dfc
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436463"
---
# <a name="speech-recognition-in-xamarinios"></a>Распознавание речи в Xamarin. iOS

_В этой статье представлен новый API распознавания речи и показано, как реализовать его в приложении Xamarin. iOS для поддержки непрерывного распознавания речи и транскрипция речи (из активных или записанных звуковых потоков) в текст._

Начиная с iOS 10, компания Apple выпустила API распознавания речи, который позволяет приложению iOS поддерживать непрерывное распознавание речи и транскрипция речи (из активных или записанных звуковых потоков) в текст.

В соответствии с Apple API распознавания речи обладает следующими возможностями и преимуществами.

- Высокая точность
- Состояние изображения
- Простота в использовании
- быстрый;
- Поддержка нескольких языков
- Уважает конфиденциальность пользователей

## <a name="how-speech-recognition-works"></a>Принцип работы распознавания речи

Распознавание речи реализовано в приложении iOS путем получения прямых или предварительно записанных аудио (в любом из речевых языков, поддерживаемых API) и передачи его распознавателю речи, который возвращает текстовые транскрипции слов.

[![Принцип работы распознавания речи](speech-images/speech01.png)](speech-images/speech01.png#lightbox)

### <a name="keyboard-dictation"></a>Диктовка с клавиатуры

Когда большинство пользователей думают о распознавании речи на устройстве iOS, они считают встроенным голосовым помощником Siri, который был выпущен вместе с диктовкой с клавиатуры в iOS 5 с помощью iPhone 4S.

Диктовка с клавиатуры поддерживается любым элементом интерфейса, который поддерживает Тексткит (например, `UITextField` или `UITextArea` ) и активируется пользователем нажатием кнопки диктовки (непосредственно слева от пробела) на виртуальной клавиатуре iOS.

Компания Apple выпустила следующую статистику диктовки клавиатуры (собранную с момента 2011):

- Диктовка с клавиатуры широко использовалась, так как она была выпущена в iOS 5.
- Примерно 65 000 приложений используют его в день.
- Третья из всех операций диктовки iOS выполняется в стороннее приложение.

Диктовка с помощью клавиатуры чрезвычайно проста в использовании, так как не требует усилий в части разработчика, кроме использования элемента интерфейса Тексткит в разработке пользовательского интерфейса приложения. Диктовка с клавиатуры также имеет преимущество не требует никаких специальных запросов прав из приложения, прежде чем его можно будет использовать.

Для приложений, использующих новые API распознавания речи, потребуются специальные разрешения, предоставляемые пользователем, так как для распознавания речи требуется передача и временное хранение данных на серверах Apple. Дополнительные сведения см. в документации по [улучшению безопасности и конфиденциальности](~/ios/app-fundamentals/security-privacy.md) .

Хотя диктовку с клавиатуры легко реализовать, она имеет несколько ограничений и недостатков:

- Он требует использования поля ввода текста и вывода клавиатуры.
- Он работает только с входными звуковыми потоками, и приложение не контролирует процесс записи звука.
- Он не позволяет управлять языком, используемым для интерпретации речи пользователя.
- В приложении нет способа выяснить, доступна ли кнопка Диктовка для пользователя.
- Приложение не может настроить процесс записи звука.
- Он предоставляет очень неглубокий набор результатов, в котором отсутствуют такие сведения, как время и уверенность.

### <a name="speech-recognition-api"></a>API распознавания речи

Впервые с iOS 10 компания Apple выпустила API распознавания речи, который предоставляет более мощный способ реализации распознавания речи в приложении iOS. Этот API аналогичен тому, который Apple использует для включения в Siri и диктовки клавиатуры и может обеспечить быструю транскрипцию с состоянием точности изображения.

Результаты, предоставляемые API распознавания речи, прозрачно настроены для отдельных пользователей, без необходимости получать данные о закрытых пользователях или получать к ним доступ.

API распознавания речи предоставляет результаты обратно в вызывающее приложение практически в реальном времени, так как пользователь говорит о нем и предоставляет дополнительные сведения о результатах перевода, чем просто текст. К ним относятся следующие объекты.

- Несколько интерпретаций того, что сказал пользователь.
- Уровни достоверности для отдельных переводов.
- Сведения о времени.

Как упоминалось выше, звук для перевода может предоставляться интерактивным каналом или из предварительно записанного источника и любого из более чем 50 языков и диалектов, поддерживаемых iOS 10.

API распознавания речи можно использовать на любом устройстве iOS под управлением iOS 10, и в большинстве случаев требуется динамическое подключение к Интернету, так как основная часть переводов выполняется на серверах Apple. С другой стороны, некоторые более новые устройства iOS поддерживают преобразование на устройстве для определенных языков с поддержкой Always on.

Компания Apple включила API доступности, чтобы определить, доступен ли данный язык для перевода в текущий момент. Приложение должно использовать этот API вместо проверки непосредственного подключения к Интернету.

Как отмечалось выше в разделе диктовки клавиатуры, распознавание речи требует передачи и временного хранения данных на серверах Apple через Интернет, поэтому приложение _должно_ запросить разрешение пользователя на распознавание, включив `NSSpeechRecognitionUsageDescription` ключ в `Info.plist` файл и вызвав `SFSpeechRecognizer.RequestAuthorization` метод. 

В зависимости от источника звука, используемого для распознавания речи, могут потребоваться другие изменения в `Info.plist` файле приложения. Дополнительные сведения см. в документации по [улучшению безопасности и конфиденциальности](~/ios/app-fundamentals/security-privacy.md) .

## <a name="adopting-speech-recognition-in-an-app"></a>Внедрение распознавания речи в приложение

Существует четыре основных шага, которые необходимо предпринять разработчику для внедрения распознавания речи в приложение iOS:

- Укажите описание использования в `Info.plist` файле приложения с помощью `NSSpeechRecognitionUsageDescription` ключа. Например, приложение камеры может включать следующее описание: _«это позволяет получить фотографию, произнося слово «Мак»._
- Запросите авторизацию, вызвав `SFSpeechRecognizer.RequestAuthorization` метод для представления пояснения (приведенного `NSSpeechRecognitionUsageDescription` выше ключа) о том, почему приложение хочет получить доступ к распознаванию речи для пользователя в диалоговом окне и позволить им принять или отклонить.
- Создайте запрос распознавания речи:
  - Для предварительно записанного звука на диске используйте `SFSpeechURLRecognitionRequest` класс.
  - Для звукового аудио (или звука из памяти) используйте `SFSPeechAudioBufferRecognitionRequest` класс.
- Передайте запрос распознавания речи распознавателю речи (), `SFSpeechRecognizer` чтобы начать распознавание. Приложение может при необходимости удерживаться на возвращенном `SFSpeechRecognitionTask` для отслеживания и отслеживания результатов распознавания.

Эти действия будут подробно рассмотрены ниже.

### <a name="providing-a-usage-description"></a>Предоставление описания использования

Чтобы предоставить необходимый `NSSpeechRecognitionUsageDescription` ключ в `Info.plist` файле, выполните следующие действия.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Дважды щелкните `Info.plist` файл, чтобы открыть его для редактирования.
2. Переключитесь в представление **исходного кода** : 

    [![Представление источника](speech-images/speech02.png)](speech-images/speech02.png#lightbox)
3. Щелкните **Добавить новую запись**, введите `NSSpeechRecognitionUsageDescription` для **Свойства** `String` **тип** и **Описание использования** в качестве **значения**. Пример: 

    [![Добавление Нсспичрекогнитионусажедескриптион](speech-images/speech03.png)](speech-images/speech03.png#lightbox)
4. Если приложение будет обрабатывать голосовую транскрипцию, для него также потребуется описание использования микрофона. Щелкните **Добавить новую запись**, введите `NSMicrophoneUsageDescription` для **Свойства** `String` **тип** и **Описание использования** в качестве **значения**. Пример: 

    [![Добавление Нсмикрофонеусажедескриптион](speech-images/speech04.png)](speech-images/speech04.png#lightbox)
5. Сохраните изменения в файле.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Дважды щелкните `Info.plist` файл, чтобы открыть его для редактирования.
2. Щелкните **Добавить новую запись**, введите `NSSpeechRecognitionUsageDescription` для **Свойства** `String` **тип** и **Описание использования** в качестве **значения**. Пример: 

    [![Добавление Нсспичрекогнитионусажедескриптион](speech-images/speech03w.png)](speech-images/speech03w.png#lightbox)
3. Если приложение будет обрабатывать голосовую транскрипцию, для него также потребуется описание использования микрофона. Щелкните **Добавить новую запись**, введите `NSMicrophoneUsageDescription` для **Свойства** `String` **тип** и **Описание использования** в качестве **значения**. Пример: 

    [![Добавление Нсмикрофонеусажедескриптион](speech-images/speech04w.png)](speech-images/speech04w.png#lightbox)
4. Сохраните изменения в файле.

-----

> [!IMPORTANT]
> Если не предоставить ни один из указанных выше `Info.plist` ключей ( `NSSpeechRecognitionUsageDescription` или `NSMicrophoneUsageDescription` ), приложение может завершиться сбоем без предупреждения при попытке получить доступ к распознаванию речи или микрофону для живого аудио.

### <a name="requesting-authorization"></a>Запрос авторизации

Чтобы запросить необходимую авторизацию пользователя, которая позволяет приложению получить доступ к распознаванию речи, измените класс основного представления контроллера и добавьте следующий код:

```csharp
using System;
using UIKit;
using Speech;

namespace MonkeyTalk
{
    public partial class ViewController : UIViewController
    {
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Request user authorization
            SFSpeechRecognizer.RequestAuthorization ((SFSpeechRecognizerAuthorizationStatus status) => {
                // Take action based on status
                switch (status) {
                case SFSpeechRecognizerAuthorizationStatus.Authorized:
                    // User has approved speech recognition
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.Denied:
                    // User has declined speech recognition
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.NotDetermined:
                    // Waiting on approval
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.Restricted:
                    // The device is not permitted
                    ...
                    break;
                }
            });
        }
    }
}
```

`RequestAuthorization`Метод `SFSpeechRecognizer` класса будет запрашивать у пользователя разрешение на доступ к распознаванию речи с помощью причины, указанной разработчиком в `NSSpeechRecognitionUsageDescription` ключе `Info.plist` файла.

`SFSpeechRecognizerAuthorizationStatus`Результат возвращается в `RequestAuthorization` подпрограммы обратного вызова метода, которую можно использовать для выполнения действий в зависимости от разрешения пользователя. 

> [!IMPORTANT]
> Компания Apple предлагает подождать, пока пользователь не запустит действие в приложении, которое требует распознавания речи, прежде чем запрашивать это разрешение.

### <a name="recognizing-pre-recorded-speech"></a>Распознавание предварительно записанных речевых функций

Если приложению нужно распознать речь из предварительно записанного WAV-или MP3-файла, он может использовать следующий код:

```csharp
using System;
using UIKit;
using Speech;
using Foundation;
...

public void RecognizeFile (NSUrl url)
{
    // Access new recognizer
    var recognizer = new SFSpeechRecognizer ();

    // Is the default language supported?
    if (recognizer == null) {
        // No, return to caller
        return;
    }

    // Is recognition available?
    if (!recognizer.Available) {
        // No, return to caller
        return;
    }

    // Create recognition task and start recognition
    var request = new SFSpeechUrlRecognitionRequest (url);
    recognizer.GetRecognitionTask (request, (SFSpeechRecognitionResult result, NSError err) => {
        // Was there an error?
        if (err != null) {
            // Handle error
            ...
        } else {
            // Is this the final translation?
            if (result.Final) {
                Console.WriteLine ("You said, \"{0}\".", result.BestTranscription.FormattedString);
            }
        }
    });
}
```

Рассмотрим этот код подробно, сначала он попытается создать распознаватель речи ( `SFSpeechRecognizer` ). Если язык по умолчанию не поддерживается для распознавания речи, `null` возвращается функция и функции завершаются.

Если распознаватель речи доступен для языка по умолчанию, приложение проверяет, доступно ли в настоящее время распознавание с помощью `Available` Свойства. Например, распознавание может быть недоступно, если у устройства нет активного подключения к Интернету.

Объект `SFSpeechUrlRecognitionRequest` создается из `NSUrl` расположения предварительно записанного файла на устройстве iOS и передается распознавателю речи для обработки с помощью подпрограммы обратного вызова.

При вызове функции обратного вызова, если `NSError` не `null` существовала ошибка, которая должна быть обработана. Так как распознавание речи выполняется постепенно, подпрограммы обратного вызова могут вызываться более одного раза, поэтому `SFSpeechRecognitionResult.Final` свойство проверяется на предмет завершения перевода и выводится лучшая версия перевода ( `BestTranscription` ).

### <a name="recognizing-live-speech"></a>Распознавание речи в реальном времени

Если приложение хочет распознать речь, процесс очень похож на распознавание предварительно записанных речевых функций. Пример:

```csharp
using System;
using UIKit;
using Speech;
using Foundation;
using AVFoundation;
...

#region Private Variables
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
#endregion
...

public void StartRecording ()
{
    // Setup audio session
    var node = AudioEngine.InputNode;
    var recordingFormat = node.GetBusOutputFormat (0);
    node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
        // Append buffer to recognition request
        LiveSpeechRequest.Append (buffer);
    });

    // Start recording
    AudioEngine.Prepare ();
    NSError error;
    AudioEngine.StartAndReturnError (out error);

    // Did recording start?
    if (error != null) {
        // Handle error and return
        ...
        return;
    }

    // Start recognition
    RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
        // Was there an error?
        if (err != null) {
            // Handle error
            ...
        } else {
            // Is this the final translation?
            if (result.Final) {
                Console.WriteLine ("You said \"{0}\".", result.BestTranscription.FormattedString);
            }
        }
    });
}

public void StopRecording ()
{
    AudioEngine.Stop ();
    LiveSpeechRequest.EndAudio ();
}

public void CancelRecording ()
{
    AudioEngine.Stop ();
    RecognitionTask.Cancel ();
}
```

Просматривая этот код подробно, он создает несколько частных переменных для обработки процесса распознавания:

```csharp
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
```

Он использует AV Foundation для записи звука, который будет передан в, `SFSpeechAudioBufferRecognitionRequest` чтобы обрабатывать запрос на распознавание:

```csharp
var node = AudioEngine.InputNode;
var recordingFormat = node.GetBusOutputFormat (0);
node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
    // Append buffer to recognition request
    LiveSpeechRequest.Append (buffer);
});
```

Приложение пытается начать запись, а все ошибки обрабатываются, если запись не может быть запущена:

```csharp
AudioEngine.Prepare ();
NSError error;
AudioEngine.StartAndReturnError (out error);

// Did recording start?
if (error != null) {
    // Handle error and return
    ...
    return;
}
```

Начнется задача распознавания, и обработчик будет сохранен в задаче распознавания ( `SFSpeechRecognitionTask` ):

```csharp
RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
    ...
});
```

Обратный вызов используется аналогично тому, который использовался выше в предварительно записанном речевом формате.

Если запись останавливается для пользователя, уведомляется и звуковой обработчик, и запрос распознавания речи:

```csharp
AudioEngine.Stop ();
LiveSpeechRequest.EndAudio ();
```

Если пользователь отменяет распознавание, то уведомляется о механизме аудио и распознавании:

```csharp
AudioEngine.Stop ();
RecognitionTask.Cancel ();
```

Важно вызвать, `RecognitionTask.Cancel` Если пользователь отменит перевод, чтобы освободить память и процессор устройства.

> [!IMPORTANT]
> Если не предоставить `NSSpeechRecognitionUsageDescription` ключи или, это `NSMicrophoneUsageDescription` `Info.plist` может привести к сбою приложения без предупреждения при попытке получить доступ к распознаванию речи или микрофону для Live Audio ( `var node = AudioEngine.InputNode;` ). Дополнительные сведения см. в разделе **предоставление описания использования** выше.

## <a name="speech-recognition-limits"></a>Ограничения распознавания речи

При работе с распознаванием речи в приложении для iOS Apple накладывает следующие ограничения:

- Распознавание речи предоставляется бесплатно для всех приложений, но его использование не ограничено:
  - Отдельные устройства iOS имеют ограниченное количество поощрений, которые могут выполняться в день.
  - Приложения будут регулироваться глобально на основе запросов в день.
- Приложение должно быть подготовлено для обработки ошибок сетевого подключения распознавания речи и ограничения скорости использования.
- В связи с этим распознавание речи может иметь высокую стоимость в случае очистки от аккумулятора и интенсивного сетевого трафика на устройстве iOS пользователя, поэтому компания Apple накладывает ограничение длительности звука примерно на одну минуту от максимального числа речи.

Если приложение последовательно достигает ограничений по частоте регулирования скорости, Apple запрашивает у разработчика связь с ними.

## <a name="privacy-and-usability-considerations"></a>Вопросы конфиденциальности и удобства использования

При включении распознавания речи в приложение iOS в Apple предусмотрено следующее предложение для прозрачности и соблюдения конфиденциальности пользователя.

- При записи речи пользователя необходимо четко указать, что запись выполняется в пользовательском интерфейсе приложения. Например, приложение может воспроизвести звук "запись" и отображать индикатор записи.
- Не используйте распознавание речи для конфиденциальных сведений о пользователе, таких как пароли, данные о работоспособности или финансовая информация.
- _Перед началом_ работы отобразите результаты распознавания. Это не только предоставляет отзыв о том, что делает приложение, но позволяет пользователю обменять ошибки распознавания по мере их выполнения.

## <a name="summary"></a>Сводка

В этой статье представлен новый API распознавания речи и показано, как реализовать его в приложении Xamarin. iOS для поддержки непрерывного распознавания речи и транскрипция речи (из активных или записанных звуковых потоков) в текст. 

## <a name="related-links"></a>Связанные ссылки

- [Спеактоме (пример)](/samples/xamarin/ios-samples/ios10-speaktome)