---
title: Android Beam
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/06/2017
ms.openlocfilehash: 8132be3e709e6cfe519934ce6c16ae16ac7054c6
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91454225"
---
# <a name="android-beam"></a>Android Beam

Android Beam — это технология Near Field Communication (NFC), представленная в Android 4.0, которая позволяет приложениям совместно использовать информацию через NFC, если они находятся поблизости друг от друга.

[![Схема, иллюстрирующая совместное использование двух устройств, которые находятся поблизости друг от друга](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

Android Beam поддерживает передачу сообщений через NFC, если два устройства находятся в диапазоне действия. Устройства, которые находятся друг от друга на расстоянии 4 см, могут обмениваться данными с помощью Android Beam. Действие на одном устройстве создает сообщение и указывает действие (или действия), которое может выполнять его принудительную отправку. Если указанное действие находится на переднем плане, а устройства находятся в диапазоне действия, Android Beam отправляет сообщение на второе устройство. На принимающем устройстве вызывается намерение, содержащее данные сообщения.

Android поддерживает два способа настройки сообщений с помощью Android Beam.

- `SetNdefPushMessage` — перед инициацией Android Beam приложение может вызвать SetNdefPushMessage, чтобы указать NdefMessage выполнять отправку через NFC, и действие, которое отправляет его. Этот механизм лучше использовать, если сообщение не изменяется во время использования приложения.

- `SetNdefPushMessageCallback` — когда Android Beam инициируется, приложение может реализовать обратный вызов для создания NdefMessage. Этот механизм позволяет отложить создание сообщений, пока устройства не будут находиться в диапазоне действия. Он поддерживает сценарии, в которых сообщение может изменяться в зависимости от того, что происходит в приложении.

В любом случае, чтобы отправлять данные с помощью Android Beam, приложение отправляет `NdefMessage`, упаковывая данные в несколько `NdefRecords`. Рассмотрим ключевые моменты, которые необходимо устранить, прежде чем активировать Android Beam. Во первых, мы будем работать со стилем обратного вызова создания `NdefMessage`.

## <a name="creating-a-message"></a>Создание сообщения

Обратные вызовы можно зарегистрировать с помощью `NfcAdapter` в методе `OnCreate` действия. Например, если `NfcAdapter` с именем `mNfcAdapter` объявляется в действии как переменная класса, для создания обратного вызова можно написать следующий код, который будет формировать сообщение:

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

Действие, которое реализует `NfcAdapter.ICreateNdefMessageCallback`, передается методу `SetNdefPushMessageCallback`, предоставленному выше. При инициации Android Beam система вызывает команду `CreateNdefMessage`, из которой действие может создать `NdefMessage`, как показано ниже:

```csharp
public NdefMessage CreateNdefMessage (NfcEvent evt)
{
    DateTime time = DateTime.Now;
    var text = ("Beam me up!\n\n" + "Beam Time: " +
        time.ToString ("HH:mm:ss"));
    NdefMessage msg = new NdefMessage (
        new NdefRecord[]{ CreateMimeRecord (
            "application/com.example.android.beam",
            Encoding.UTF8.GetBytes (text)) });
        } };
    return msg;
}

public NdefRecord CreateMimeRecord (String mimeType, byte [] payload)
{
    byte [] mimeBytes = Encoding.UTF8.GetBytes (mimeType);
    NdefRecord mimeRecord = new NdefRecord (
        NdefRecord.TnfMimeMedia, mimeBytes, new byte [0], payload);
    return mimeRecord;
}
```

## <a name="receiving-a-message"></a>Получение сообщения

На принимающей стороне система вызывает намерение с действием `ActionNdefDiscovered`, из которого NdefMessage можно извлечь следующим образом:

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

Полный пример кода, в котором используется Android Beam, показанный на снимке экрана ниже, см. в Коллекции образцов [Xamarin.Android. Демонстрационная версия Android Beam](/samples/xamarin/monodroid-samples/androidbeamdemo).

[![Пример снимков экрана с демонстрационной версией Android Beam](android-beam-images/24.png)](android-beam-images/24.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [Xamarin.Android. Демонстрационная версия Android Beam](/samples/xamarin/monodroid-samples/androidbeamdemo)