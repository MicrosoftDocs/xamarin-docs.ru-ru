---
title: Распознавание речи с помощью API службы распознавания речи
description: В этой статье объясняется, как использовать API службы распознавания речи Azure для транскрипция речи в текст в Xamarin.Forms приложении.
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 01/14/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 45cf62354eb54007b9380ee06b1d93ee49000909
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93365857"
---
# <a name="speech-recognition-using-azure-speech-service"></a>Распознавание речи с помощью служб речи Azure

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/webservices-cognitivespeechservice)

Служба распознавания речи Azure — это облачный API, который предоставляет следующие функциональные возможности:

- Преобразование **речи в текст** расшифровывает звуковых файлов или потоков в текст.
- Преобразование **текста в речь** приводит к преобразованию входного текста в речь, похожая на синтезированную.
- **Перевод речи** обеспечивает многоязычное преобразование в реальном времени для преобразования речи в текст и речи в речь.
- **Голосовые помощники** могут создавать для приложений интерфейсы общения, схожие с людьми.

В этой статье объясняется, как речь-to-Text реализуется в примере Xamarin.Forms приложения с помощью службы распознавания речи Azure. На следующих снимках экрана показан пример приложения на iOS и Android:

[![Снимки экрана примера приложения в iOS и Android](speech-recognition-images/speech-recognition-cropped.png)](speech-recognition-images/speech-recognition.png#lightbox "Снимки экрана примера приложения в iOS и Android")

## <a name="create-an-azure-speech-service-resource"></a>Создание ресурса службы распознавания речи Azure

Служба распознавания речи Azure входит в состав Azure Cognitive Services, которая предоставляет облачные API для таких задач, как распознавание изображений, распознавание речи, перевод и поиск Bing. Дополнительные сведения см. в статье [что такое Azure Cognitive Services?](/azure/cognitive-services/welcome).

Для примера проекта требуется создание ресурса Azure Cognitive Services в портал Azure. Ресурс Cognitive Services может быть создан для одной службы, например службы речи, или как ресурс с несколькими службами. Ниже приведены действия по созданию ресурса службы речи.

1. Войдите в [портал Azure](https://portal.azure.com).
1. Создайте ресурс с несколькими службами или одной службой.
1. Получите ключ API и сведения о регионе для ресурса.
1. Обновите пример файла **Constants.CS** .

Пошаговое пошаговое инструкции по созданию ресурса см. в разделе [создание Cognitive Services ресурса](/azure/cognitive-services/cognitive-services-apis-create-account).

> [!NOTE]
> Если у вас еще нет [подписки Azure](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing), создайте [бесплатную учетную запись Azure](https://aka.ms/azfree-docs-mobileapps), прежде чем начать работу. После создания учетной записи на уровне Free можно создать ресурс с одной службой, чтобы испытать службу.

## <a name="configure-your-app-with-the-speech-service"></a>Настройка приложения с помощью службы речи

После создания ресурса Cognitive Services файл **Constants.CS** можно обновить с помощью региона и ключа API из ресурса Azure:

```csharp
public static class Constants
{
    public static string CognitiveServicesApiKey = "YOUR_KEY_GOES_HERE";
    public static string CognitiveServicesRegion = "westus";
}
```

## <a name="install-nuget-speech-service-package"></a>Установка пакета службы распознавания речи NuGet

Пример приложения использует пакет NuGet **Microsoft. CognitiveServices. Speech** для подключения к службе распознавания речи Azure. Установите этот пакет NuGet в общем проекте и в каждом проекте платформы.

## <a name="create-an-imicrophoneservice-interface"></a>Создание интерфейса Имикрофонесервице

Для каждой платформы требуется разрешение на доступ к микрофону. Пример проекта предоставляет `IMicrophoneService` интерфейс в общем проекте и использует Xamarin.Forms `DependencyService` для получения реализаций платформы интерфейса.

```csharp
public interface IMicrophoneService
{
    Task<bool> GetPermissionAsync();
    void OnRequestPermissionResult(bool isGranted);
}
```

## <a name="create-the-page-layout"></a>Создание макета страницы

Образец проекта определяет базовый макет страницы в файле **MainPage. XAML** . Ключевыми элементами макета являются `Button` , которые начинают процесс отслеживания, а — `Label` для включения текста расшифрованной, а также `ActivityIndicator` для показа, когда выполняется транскрипция.

```xaml
<ContentPage ...>
    <StackLayout>
        <Frame ...>
            <ScrollView x:Name="scroll"
                        ...>
                <Label x:Name="transcribedText"
                       ... />
            </ScrollView>
        </Frame>

        <ActivityIndicator x:Name="transcribingIndicator"
                           IsRunning="False" />
        <Button x:Name="transcribeButton"
                ...
                Clicked="TranscribeClicked"/>
    </StackLayout>
</ContentPage>
```

## <a name="implement-the-speech-service"></a>Реализация службы речи

Файл кода программной части **MainPage.XAML.CS** содержит всю логику отправки аудио и получение текста расшифрованной из службы распознавания речи Azure.

`MainPage`Конструктор получает экземпляр `IMicrophoneService` интерфейса из `DependencyService` :

```csharp
public partial class MainPage : ContentPage
{
    SpeechRecognizer recognizer;
    IMicrophoneService micService;
    bool isTranscribing = false;

    public MainPage()
    {
        InitializeComponent();

        micService = DependencyService.Resolve<IMicrophoneService>();
    }

    // ...
}
```

`TranscribeClicked`Метод вызывается при `transcribeButton` касании экземпляра:

```csharp
async void TranscribeClicked(object sender, EventArgs e)
{
    bool isMicEnabled = await micService.GetPermissionAsync();

    // EARLY OUT: make sure mic is accessible
    if (!isMicEnabled)
    {
        UpdateTranscription("Please grant access to the microphone!");
        return;
    }

    // initialize speech recognizer 
    if (recognizer == null)
    {
        var config = SpeechConfig.FromSubscription(Constants.CognitiveServicesApiKey, Constants.CognitiveServicesRegion);
        recognizer = new SpeechRecognizer(config);
        recognizer.Recognized += (obj, args) =>
        {
            UpdateTranscription(args.Result.Text);
        };
    }

    // if already transcribing, stop speech recognizer
    if (isTranscribing)
    {
        try
        {
            await recognizer.StopContinuousRecognitionAsync();
        }
        catch(Exception ex)
        {
            UpdateTranscription(ex.Message);
        }
        isTranscribing = false;
    }

    // if not transcribing, start speech recognizer
    else
    {
        Device.BeginInvokeOnMainThread(() =>
        {
            InsertDateTimeRecord();
        });
        try
        {
            await recognizer.StartContinuousRecognitionAsync();
        }
        catch(Exception ex)
        {
            UpdateTranscription(ex.Message);
        }
        isTranscribing = true;
    }
    UpdateDisplayState();
}
```

Метод `TranscribeClicked` выполняет следующие действия:

1. Проверяет, имеет ли приложение доступ к микрофону, и завершает работу на раннем этапе, если нет.
1. Создает экземпляр класса, `SpeechRecognizer` если он еще не существует.
1. Останавливает непрерывную транскрипцию, если она выполняется.
1. Вставляет отметку времени и начинает непрерывную транскрипцию, если она не выполняется.
1. Уведомляет приложение о том, что его внешний вид будет обновляться на основе нового состояния приложения.

Оставшаяся часть `MainPage` методов класса — это вспомогательные методы для отображения состояния приложения:

```csharp
void UpdateTranscription(string newText)
{
    Device.BeginInvokeOnMainThread(() =>
    {
        if (!string.IsNullOrWhiteSpace(newText))
        {
            transcribedText.Text += $"{newText}\n";
        }
    });
}

void InsertDateTimeRecord()
{
    var msg = $"=================\n{DateTime.Now.ToString()}\n=================";
    UpdateTranscription(msg);
}

void UpdateDisplayState()
{
    Device.BeginInvokeOnMainThread(() =>
    {
        if (isTranscribing)
        {
            transcribeButton.Text = "Stop";
            transcribeButton.BackgroundColor = Color.Red;
            transcribingIndicator.IsRunning = true;
        }
        else
        {
            transcribeButton.Text = "Transcribe";
            transcribeButton.BackgroundColor = Color.Green;
            transcribingIndicator.IsRunning = false;
        }
    });
}
```

`UpdateTranscription`Метод записывает предоставленный объект `newText` `string` в `Label` элемент с именем `transcribedText` . Он заставляет это обновление выполняться в потоке пользовательского интерфейса, чтобы его можно было вызывать из любого контекста, не вызывая исключений. Объект `InsertDateTimeRecord` записывает текущую дату и время в `transcribedText` экземпляр, чтобы отметить начало нового транскрипции. Наконец, `UpdateDisplayState` метод обновляет `Button` элементы и, `ActivityIndicator` чтобы отразить, выполняется ли запись.

## <a name="create-platform-microphone-services"></a>Создание служб микрофона платформы

Приложение должно иметь доступ к микрофону для получения данных речи. Для `IMicrophoneService` работы приложения интерфейс должен быть реализован и зарегистрирован в `DependencyService` на каждой платформе.

### <a name="android"></a>Android

В примере проекта определяется `IMicrophoneService` реализация для Android с именем `AndroidMicrophoneService` :

```csharp
[assembly: Dependency(typeof(AndroidMicrophoneService))]
namespace CognitiveSpeechService.Droid.Services
{
    public class AndroidMicrophoneService : IMicrophoneService
    {
        public const int RecordAudioPermissionCode = 1;
        private TaskCompletionSource<bool> tcsPermissions;
        string[] permissions = new string[] { Manifest.Permission.RecordAudio };

        public Task<bool> GetPermissionAsync()
        {
            tcsPermissions = new TaskCompletionSource<bool>();

            if ((int)Build.VERSION.SdkInt < 23)
            {
                tcsPermissions.TrySetResult(true);
            }
            else
            {
                var currentActivity = MainActivity.Instance;
                if (ActivityCompat.CheckSelfPermission(currentActivity, Manifest.Permission.RecordAudio) != (int)Permission.Granted)
                {
                    RequestMicPermissions();
                }
                else
                {
                    tcsPermissions.TrySetResult(true);
                }

            }

            return tcsPermissions.Task;
        }

        public void OnRequestPermissionResult(bool isGranted)
        {
            tcsPermissions.TrySetResult(isGranted);
        }

        void RequestMicPermissions()
        {
            if (ActivityCompat.ShouldShowRequestPermissionRationale(MainActivity.Instance, Manifest.Permission.RecordAudio))
            {
                Snackbar.Make(MainActivity.Instance.FindViewById(Android.Resource.Id.Content),
                        "Microphone permissions are required for speech transcription!",
                        Snackbar.LengthIndefinite)
                        .SetAction("Ok", v =>
                        {
                            ((Activity)MainActivity.Instance).RequestPermissions(permissions, RecordAudioPermissionCode);
                        })
                        .Show();
            }
            else
            {
                ActivityCompat.RequestPermissions((Activity)MainActivity.Instance, permissions, RecordAudioPermissionCode);
            }
        }
    }
}
```

`AndroidMicrophoneService`Компонент имеет следующие возможности.

1. `Dependency`Атрибут регистрирует класс с помощью `DependencyService` .
1. `GetPermissionAsync`Метод проверяет, требуются ли разрешения на основе версии пакет SDK для Android, и вызывает, `RequestMicPermissions` Если разрешение еще не предоставлено.
1. `RequestMicPermissions`Метод использует `Snackbar` класс для запроса разрешений у пользователя, если требуется обоснование, в противном случае он напрямую запрашивает разрешения записи звука.
1. `OnRequestPermissionResult`Метод вызывается с результатом, `bool` когда пользователь ответил на запрос разрешений.

`MainActivity`Класс настраивается для обновления `AndroidMicrophoneService` экземпляра при завершении запросов разрешений:

```csharp
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
{
    IMicrophoneService micService;
    internal static MainActivity Instance { get; private set; }
    
    protected override void OnCreate(Bundle savedInstanceState)
    {
        Instance = this;
        // ...
        micService = DependencyService.Resolve<IMicrophoneService>();
    }
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Android.Content.PM.Permission[] grantResults)
    {
        // ...
        switch(requestCode)
        {
            case AndroidMicrophoneService.RecordAudioPermissionCode:
                if (grantResults[0] == Permission.Granted)
                {
                    micService.OnRequestPermissionResult(true);
                }
                else
                {
                    micService.OnRequestPermissionResult(false);
                }
                break;
        }
    }
}
```

`MainActivity`Класс определяет статическую ссылку с именем `Instance` , которая необходима `AndroidMicrophoneService` объекту при запросе разрешений. Он переопределяет `OnRequestPermissionsResult` метод для обновления `AndroidMicrophoneService` объекта при утверждении или отклонении запроса разрешений пользователем.

Наконец, приложение Android должно включать разрешение на запись звука в файл **AndroidManifest.xml** :

```xml
<manifest ...>
    ...
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
</manifest>
```

### <a name="ios"></a>iOS

В примере проекта определяется `IMicrophoneService` реализация для iOS с именем `iOSMicrophoneService` :

```csharp
[assembly: Dependency(typeof(iOSMicrophoneService))]
namespace CognitiveSpeechService.iOS.Services
{
    public class iOSMicrophoneService : IMicrophoneService
    {
        TaskCompletionSource<bool> tcsPermissions;

        public Task<bool> GetPermissionAsync()
        {
            tcsPermissions = new TaskCompletionSource<bool>();
            RequestMicPermission();
            return tcsPermissions.Task;
        }

        public void OnRequestPermissionResult(bool isGranted)
        {
            tcsPermissions.TrySetResult(isGranted);
        }

        void RequestMicPermission()
        {
            var session = AVAudioSession.SharedInstance();
            session.RequestRecordPermission((granted) =>
            {
                tcsPermissions.TrySetResult(granted);
            });
        }
    }
}
```

`iOSMicrophoneService`Компонент имеет следующие возможности.

1. `Dependency`Атрибут регистрирует класс с помощью `DependencyService` .
1. `GetPermissionAsync`Метод вызывает `RequestMicPermissions` для запроса разрешений у пользователя устройства.
1. `RequestMicPermissions`Метод использует общий `AVAudioSession` экземпляр для запроса разрешений на запись.
1. `OnRequestPermissionResult`Метод обновляет `TaskCompletionSource` экземпляр с указанным `bool` значением.

Наконец, приложение iOS **info. plist** должно включать сообщение, сообщающее пользователю, почему приложение запрашивает доступ к микрофону. Измените файл info. plist, чтобы включить следующие теги в `<dict>` элемент:

```xml
<plist>
    <dict>
        ...
        <key>NSMicrophoneUsageDescription</key>
        <string>Voice transcription requires microphone access</string>
    </dict>
</plist>
```

### <a name="uwp"></a>UWP

В примере проекта определяется `IMicrophoneService` реализация для UWP с именем `UWPMicrophoneService` :

```csharp
[assembly: Dependency(typeof(UWPMicrophoneService))]
namespace CognitiveSpeechService.UWP.Services
{
    public class UWPMicrophoneService : IMicrophoneService
    {
        public async Task<bool> GetPermissionAsync()
        {
            bool isMicAvailable = true;
            try
            {
                var mediaCapture = new MediaCapture();
                var settings = new MediaCaptureInitializationSettings();
                settings.StreamingCaptureMode = StreamingCaptureMode.Audio;
                await mediaCapture.InitializeAsync(settings);
            }
            catch(Exception ex)
            {
                isMicAvailable = false;
            }

            if(!isMicAvailable)
            {
                await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-microphone"));
            }

            return isMicAvailable;
        }

        public void OnRequestPermissionResult(bool isGranted)
        {
            // intentionally does nothing
        }
    }
}
```

`UWPMicrophoneService`Компонент имеет следующие возможности.

1. `Dependency`Атрибут регистрирует класс с помощью `DependencyService` .
1. `GetPermissionAsync`Метод пытается инициализировать `MediaCapture` экземпляр. В случае сбоя он запускает запрос пользователя на включение микрофона.
1. `OnRequestPermissionResult`Метод существует для удовлетворения интерфейса, но не является обязательным для реализации UWP.

Наконец, пакет UWP **. appxmanifest** должен указывать, что приложение использует микрофон. Дважды щелкните файл Package. appxmanifest и выберите параметр **Microphone** на вкладке **возможности** в Visual Studio 2019:

[![Снимок экрана манифеста в Visual Studio 2019](speech-recognition-images/package-manifest-cropped.png)](speech-recognition-images/package-manifest.png#lightbox "Снимок экрана манифеста в Visual Studio 2019")

## <a name="test-the-application"></a>Тестирование приложения

Запустите приложение и нажмите кнопку **транскрипция** . Приложение должно запрашивать доступ к микрофону и начинать процесс транскрипции. `ActivityIndicator`Будет выполняться анимация, показывающая, что запись активна. Во время диктовки приложение будет выполнять потоковую передачу звуковых данных в ресурс служб распознавания речи Azure, который будет отвечать на расшифрованной текст. Текст расшифрованной будет отображаться в `Label` элементе при его получении.

> [!NOTE]
> Эмуляторам Android не удается загрузить и инициализировать библиотеки речевых служб. Для платформы Android рекомендуется тестирование на физическом устройстве.

## <a name="related-links"></a>Связанные ссылки

- [Пример службы распознавания речи Azure](/samples/xamarin/xamarin-forms-samples/webservices-cognitivespeechservice)
- [Обзор службы распознавания речи Azure](/azure/cognitive-services/speech-service/overview)
- [Создание ресурса Cognitive Services](/azure/cognitive-services/cognitive-services-apis-create-account)
- [Краткое руководство. по распознаванию речи с микрофона](/azure/cognitive-services/speech-service/quickstarts/speech-to-text-from-microphone)