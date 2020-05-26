---
title: 'Xamarin.Essentials: Защищенное хранилище'
description: В этом документе описан класс SecureStorage в Xamarin.Essentials, который организует безопасное хранение пар "ключ-значение". Здесь рассматривается использование класса, особенности реализации для платформ и существующие ограничения.
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.custom: video
ms.openlocfilehash: 41d9efa66318f4c3f5315351d3c1f51b4e503521
ms.sourcegitcommit: 44c44ad60c5c880a39006493aedd2d7aa834a27e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/18/2020
ms.locfileid: "83550904"
---
# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials: Защищенное хранилище

Класс **SecureStorage** помогает обеспечить безопасное хранение пар "ключ-значение".

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

Чтобы проверить функциональность класса **SecureStorage**, нужно создать описанную ниже конфигурацию для конкретной платформы.

# <a name="android"></a>[Android](#tab/android)

> [!TIP]
> [Auto Backup for Apps](https://developer.android.com/guide/topics/data/autobackup) (Автоматическое резервное копирование для приложений) — это функция Android 6.0 (API уровня 23) и более поздних версий, которая создает резервные копии пользовательских данных из приложения (общие параметры, файлы во внутреннем хранилище приложения, некоторые другие файлы). Эти данные восстанавливаются при повторной установке приложения на том же или на новом устройстве. Это может повлиять на класс `SecureStorage`, который использует сохраненные общие параметры и не может быть расшифрован при восстановлении. Xamarin.Essentials автоматически учитывает этот случай и удаляет этот ключ, чтобы его можно было сбросить. Но вы можете принять дополнительные меры предосторожности и отключить автоматическое резервное копирование.

### <a name="enable-or-disable-backup"></a>Включение и отключение резервного копирования
Вы можете отключить автоматическое резервное копирование для всего приложения, присвоив значение false параметру `android:allowBackup` в файле `AndroidManifest.xml`. Мы рекомендуем использовать такой метод, только если вы планируете организовать другой способ восстановления данных.

```xml
<manifest ... >
    ...
    <application android:allowBackup="false" ... >
        ...
    </application>
</manifest>
```

### <a name="selective-backup"></a>Выборочные резервные копии
Вы можете исключить из автоматического резервного копирования определенное содержимое. Можно создать настраиваемое правило, которое исключает элементы `SecureStore` из процесса резервного копирования.

1. Настройте атрибут `android:fullBackupContent` в файле **AndroidManifest.xml**:

    ```xml
    <application ...
        android:fullBackupContent="@xml/auto_backup_rules">
    </application>
    ```

2. Создайте файл XML с именем **auto_backup_rules.xml** в каталоге **Resources/xml** с действием сборки **AndroidResource**. Затем настройте следующее содержимое, которое включает все общие параметры, кроме `SecureStorage`:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <full-backup-content>
        <include domain="sharedpref" path="."/>
        <exclude domain="sharedpref" path="${applicationId}.xamarinessentials.xml"/>
    </full-backup-content>
    ```

# <a name="ios"></a>[iOS](#tab/ios)

Если разработка ведется на **симуляторе iOS**, включите право **Keychain** (Цепочка ключей) и добавьте группу доступа к цепочке ключей для идентификатора пакета приложения. 

Откройте **Entitlements.plist** из проекта iOS, найдите в нем право **Keychain** (Цепочка ключей) и включите его. Это действие автоматически позволяет автоматически добавить идентификатор приложения как группу.

В свойствах проекта найдите раздел **Подписывание пакета iOS** и укажите для параметра **Настраиваемые объемы обслуживания** значение **Entitlements.plist**.

> [!TIP]
> При развертывании на устройство iOS это назначение не является обязательным, и его лучше удалить.

# <a name="uwp"></a>[UWP](#tab/uwp)

Дополнительная настройка не требуется.

-----

## <a name="using-secure-storage"></a>Использование защищенного хранилища

Добавьте в свой класс ссылку на Xamarin.Essentials:

```csharp
using Xamarin.Essentials;
```

Чтобы сохранить значение определенного _ключа_ в защищенное хранилище, используйте следующий код:

```csharp
try
{
  await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
}
catch (Exception ex)
{
  // Possible that device doesn't support secure storage on device.
}
```

Чтобы извлечь значение из защищенного хранилища, используйте следующий код:

```csharp
try
{
  var oauthToken = await SecureStorage.GetAsync("oauth_token");
}
catch (Exception ex)
{
  // Possible that device doesn't support secure storage on device.
}
```

> [!NOTE]
> Если для запрошенного ключа нет сохраненного значения, команда `GetAsync` вернет значение `null`.

Чтобы удалить один конкретный ключ, используйте следующий вызов:

```csharp
SecureStorage.Remove("oauth_token");
```

Чтобы удалить все ключи, используйте следующий вызов:

```csharp
SecureStorage.RemoveAll();
```

## <a name="platform-implementation-specifics"></a>Особенности реализации для платформ

# <a name="android"></a>[Android](#tab/android)

[Хранилище ключей Android](https://developer.android.com/training/articles/keystore.html) используется для хранения ключа шифрования, с помощью которого значения шифруются перед сохранением в [общие параметры](https://developer.android.com/training/data-storage/shared-preferences.html) с именем файла **[идентификатор_пакета_приложения].xamarinessentials**.  Ключ (не криптографический, а _ключ_ для _значения_), используемый в файле общих параметров, представляет собой _хэш MD5_ ключа, передаваемого в API `SecureStorage`.

**API уровня 23 и более поздних версий**

В API новых уровней ключ **AES** извлекается из хранилища ключей Android и применяется с шифром **AES/GCM/NoPadding** для шифрования значения перед сохранением в файл общих параметров.

**API уровня 22 и более ранних версий**

В API более старых уровней хранилище ключей Android поддерживает только хранение ключей **RSA**, которые применяются с шифром **RSA/ECB/PKCS1Padding** для шифрования ключа **AES** (созданного случайным образом во время выполнения) и сохранения полученного значения в файле общих параметров в разделе _SecureStorageKey_, если такой ключ еще не был создан.

**SecureStorage** использует API-интерфейс [предпочтений](preferences.md) и применяет правила сохраняемости данных, которые описаны в документации по классу [Preferences](preferences.md#persistence). При обновлении устройств с API уровня 22 или ниже до API уровня 23 или выше сохраняется указанный тип шифрования, если приложение не было удалено и (или) не был вызван метод **RemoveAll**.

# <a name="ios"></a>[iOS](#tab/ios)

Для безопасного хранения значений на устройствах iOS используется [цепочка ключей](xref:Security.SecKeyChain).  Для хранения значения используется `SecRecord` со значением `Service`, которое настраивается в файле **[идентификатор_пакета_приложения].xamarinessentials**.

В некоторых случаях данные цепочки ключей синхронизируется с iCloud, и тогда при удалении приложения сохраненные значения из iCloud и с других устройств пользователя не всегда удаляются.

# <a name="uwp"></a>[UWP](#tab/uwp)

Для безопасного шифрования значений на устройствах универсальной платформы Windows применяется [DataProtectionProvider](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider).

Зашифрованные значения хранятся в `ApplicationData.Current.LocalSettings` внутри контейнера с именем **[идентификатор_приложения].xamarinessentials**.

**SecureStorage** использует API-интерфейс [предпочтений](preferences.md) и применяет правила сохраняемости данных, которые описаны в документации по классу [Preferences](preferences.md#persistence). При этом также используется `LocalSettings` со следующим ограничением: длина имени каждого параметра не может превышать 255 символов. Каждый параметр может иметь размер до 8 КБ, а каждый составной параметр — до 64 КБ.

-----

## <a name="limitations"></a>Ограничения

Этот API предназначен для хранения небольших объемов текста.  Производительность может быть низкой, если сохранять большие объемы текста. 

## <a name="api"></a>API

- [Исходный код SecureStorage](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/SecureStorage)
- [Документация по API SecureStorage](xref:Xamarin.Essentials.SecureStorage)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Secure-Storage-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
