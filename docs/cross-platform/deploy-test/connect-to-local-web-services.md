---
title: Подключение к локальным веб-службам из iOS Simulator и Android Emulator
description: В этой статье объясняется, как мобильное приложение Xamarin, выполняемое в iOS Simulator или Android Emulator, может использовать веб-службу ASP.NET Core, запущенную в локальной среде.
ms.prod: xamarin
ms.assetid: FD8FE199-898B-4841-8041-CC9CA1A00917
author: davidbritch
ms.author: dabritch
ms.date: 02/04/2021
ms.openlocfilehash: 6c9e91d8c434a0deea8c419def7dc3f1800b1d06
ms.sourcegitcommit: 3b6eec7841868f50827271105577ecdc6766c162
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/06/2021
ms.locfileid: "99606580"
---
# <a name="connect-to-local-web-services-from-ios-simulators-and-android-emulators"></a>Подключение к локальным веб-службам из iOS Simulator и Android Emulator

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/webservices-todorest/)

Многие мобильные приложения используют веб-службы. На этапе разработки веб-службу обычно развертывают локально и подключаются к ней из мобильного приложения, выполняемого в iOS Simulator или Android Emulator. Это избавляет от необходимости развертывать веб-службу в размещенной конечной точке и позволяет упростить процесс отладки, так как и мобильное приложение, и веб-служба выполняются локально.

Мобильные приложения, выполняемые в iOS Simulator или Android Emulator, могут использовать веб-службы ASP.NET Core, запущенные в локальной среде и предоставляемые по протоколу HTTP следующим образом:

- Приложения, выполняемые в iOS Simulator, могут подключаться к локальным веб-службам HTTP, используя IP-адрес компьютера или имя узла `localhost`. Например, если имеется локальная веб-служба HTTP, которая предоставляет операцию GET по относительному URI `/api/todoitems/`, то приложение, работающее в iOS Simulator, может использовать операцию путем отправки запроса GET к `http://localhost:<port>/api/todoitems/`.
- Приложения, работающие в Android Emulator, могут подключаться к локальной веб-службе HTTP по адресу `10.0.2.2`, который является псевдонимом для интерфейса замыкания узла на себя (`127.0.0.1` на компьютере разработки). Например, если имеется локальная веб-служба HTTP, которая предоставляет операцию GET по относительному URI `/api/todoitems/`, то приложение, работающее в Android Emulator, может использовать операцию путем отправки запроса GET к `http://10.0.2.2:<port>/api/todoitems/`.

Но приложению, выполняемому в iOS Simulator или Android Emulator, нужна доработка, чтобы оно могло использовать локальную веб-службу, предоставляемую по протоколу HTTPS. В этом сценарии процесс выглядит следующим образом:

1. Создайте самозаверяющий сертификат разработки на своем компьютере. Дополнительные сведения см. в разделе [Создание сертификата разработки](#create-a-development-certificate).
1. Настройте проект на использование соответствующего сетевого стека `HttpClient` для отладочной сборки. Дополнительные сведения см. в разделе [Настройка проекта](#configure-your-project).
1. Укажите адрес своего локального компьютера. Дополнительные сведения см. в разделе [Указание адреса локального компьютера](#specify-the-local-machine-address).
1. Выполните обход проверки безопасности сертификата разработки. Дополнительные сведения см. в разделе [Обход проверки безопасности сертификата](#bypass-the-certificate-security-check).

Далее последовательно рассматриваются все эти этапы.

## <a name="create-a-development-certificate"></a>Создание сертификата разработки

При установке пакета SDK для .NET Core в локальное хранилище сертификатов пользователя устанавливается сертификат разработки HTTPS ASP.NET Core. Но устанавливаемый сертификат не является доверенным. Чтобы сделать его доверенным, выполните следующее однократное действие для запуска средства .NET `dev-certs`.

```dotnetcli
dotnet dev-certs https --trust
```

Следующая команда вызывает справку по средству `dev-certs`.

```dotnetcli
dotnet dev-certs https --help
```

Кроме того, при запуске проекта ASP.NET Core 2.1 (или следующих версий), использующего протокол HTTPS, Visual Studio проверяет наличие сертификата разработки и предлагает установить его и сделать доверенным.

> [!NOTE]
> Сертификат разработки HTTPS ASP.NET Core является самозаверяющим.

Дополнительные сведения о включении протокола HTTPS в локальной среде на компьютере см. в разделе [Включение локального HTTPS](/aspnet/core/getting-started#enable-local-https).

## <a name="configure-your-project"></a>Настройка проекта

В приложении Xamarin для iOS и Android с помощью класса `HttpClient` можно указать, какой сетевой стек использовать: управляемый сетевой стек или собственный сетевой стек. Управляемый стек обеспечивает высокий уровень совместимости с существующим кодом .NET, но ограничен протоколом TLS 1.0, может выполняться медленнее и стать причиной большого размера исполняемого файла. Собственный стек может выполнятся быстрее и обеспечивать лучшую безопасность, но при этом не предоставлять все функциональные возможности класса `HttpClient`.

### <a name="ios"></a>iOS

Приложения Xamarin под управлением iOS могут использовать управляемый сетевой стек или собственные сетевые стеки `CFNetwork` или `NSUrlSession`. По умолчанию новые проекты для платформы iOS используют сетевой стек `NSUrlSession` для поддержки протокола TLS 1.2 и собственные API для повышения производительности и уменьшения размера исполняемого файла. Дополнительные сведения см. в статье о [селекторе реализации HttpClient и SSL/TLS для iOS и macOS](~/cross-platform/macios/http-stack.md).

### <a name="android"></a>Android

Приложения Xamarin под управлением Android могут использовать управляемый сетевой стек `HttpClient` или собственный сетевой стек `AndroidClientHandler`. По умолчанию новые проекты для платформы Android используют сетевой стек `AndroidClientHandler` для поддержки протокола TLS 1.2 и собственные API для повышения производительности и уменьшения размера исполняемого файла. Дополнительные сведения о сетевых стеках Android см. в статье [Стек HttpClient и селектор реализации SSL/TLS для Android](~/android/app-fundamentals/http-stack.md).

## <a name="specify-the-local-machine-address"></a>Указание адреса локального компьютера

Как iOS Simulator, так и Android Emulator предоставляют доступ к защищенным веб-службам на локальном компьютере. Но адреса локального компьютера при этом будут разными.

### <a name="ios"></a>iOS

iOS Simulator использует сеть главного компьютера. Таким образом, приложения, выполняемые в симуляторе, могут подключаться к веб-службам, работающим на локальном компьютере, используя IP-адрес компьютера или имя узла `localhost`. Например, если имеется локальная защищенная веб-служба, которая предоставляет операцию GET по относительному URI `/api/todoitems/`, то приложение, работающее в iOS Simulator, может использовать операцию путем отправки запроса GET к `https://localhost:<port>/api/todoitems/`.

> [!NOTE]
> При запуске мобильного приложения в iOS Simulator из Windows приложение отображается в [Remoted iOS Simulator для Windows](~/tools/ios-simulator/index.md). Но приложение выполняется на связанном компьютере Mac. Таким образом, у приложения iOS на компьютере Mac отсутствует доступ localhost к веб-службе, выполняющейся в Windows.

### <a name="android"></a>Android

Каждый экземпляр Android Emulator изолирован от сетевых интерфейсов компьютера разработки с помощью виртуального маршрутизатора. Таким образом, эмулируемое устройство не может видеть компьютер разработки или другие экземпляры эмулятора в сети.

Виртуальный маршрутизатор каждого эмулятора управляет специализированным сетевым пространством, которое имеет предварительно выделенные адреса, а адрес `10.0.2.2` является псевдонимом для интерфейса замыкания узла на себя (127.0.0.1 на компьютере разработки). Таким образом, если имеется локальная защищенная веб-служба, которая предоставляет операцию GET по относительному URI `/api/todoitems/`, то приложение, работающее в Android Emulator, может использовать операцию путем отправки запроса GET к `https://10.0.2.2:<port>/api/todoitems/`.

### <a name="detect-the-operating-system"></a>Определение операционной системы

С помощью класса [`DeviceInfo`](xref:Xamarin.Essentials.DeviceInfo) можно определить платформу, на которой запущено приложение. Соответствующее имя узла, предоставляющего доступ к локальной защищенной веб-службе, можно задать следующим образом:

```csharp
public static string BaseAddress =
    DeviceInfo.Platform == DevicePlatform.Android ? "https://10.0.2.2:5001" : "https://localhost:5001";
public static string TodoItemsUrl = $"{BaseAddress}/api/todoitems/";
```

Дополнительные сведения о классе `DeviceInfo` см. в разделе [Xamarin. Essentials. Сведения об устройстве](~/essentials/device-information.md).

## <a name="bypass-the-certificate-security-check"></a>Обход проверки безопасности сертификата

Попытка вызвать локальную защищенную веб-службу из приложения, выполняемого в iOS Simulator или Android Emulator, приведет к исключению `HttpRequestException` даже при использовании управляемого сетевого стека на любой из платформ. Это обусловлено тем, что локальный сертификат разработки HTTPS является самозаверяющим, а самозаверяющие сертификаты не являются доверенными в iOS и Android. Таким образом, необходимо игнорировать ошибки SSL, когда приложение использует локальную защищенную веб-службу. Это можно сделать, используя управляемый и собственный сетевые стеки в iOS и Android и задав в качестве значения свойства `ServerCertificateCustomValidationCallback` объекта `HttpClientHandler` обратный вызов, который игнорирует результат проверки безопасности локального сертификата разработки HTTPS.

```csharp
// This method must be in a class in a platform project, even if
// the HttpClient object is constructed in a shared project.
public HttpClientHandler GetInsecureHandler()
{
    HttpClientHandler handler = new HttpClientHandler();
    handler.ServerCertificateCustomValidationCallback = (message, cert, chain, errors) =>
    {
        if (cert.Issuer.Equals("CN=localhost"))
            return true;
        return errors == System.Net.Security.SslPolicyErrors.None;
    };
    return handler;
}
```

В этом примере кода результат проверки сертификата сервера возвращается тогда, когда прошедший проверку сертификат не является сертификатом `localhost`. Для такого сертификата результат проверки игнорируется и возвращается значение `true`, подтверждающее, что сертификат действителен. Полученный объект `HttpClientHandler` должен передаваться в качестве аргумента в конструктор `HttpClient` для отладочных сборок:

```csharp
#if DEBUG
    HttpClientHandler insecureHandler = GetInsecureHandler();
    HttpClient client = new HttpClient(insecureHandler);
#else
    HttpClient client = new HttpClient();
#endif
```

## <a name="enable-http-clear-text-traffic"></a>Включение HTTP-трафика с открытым текстом

При необходимости для проектов iOS и Android можно настроить разрешение HTTP-трафика с открытым текстом. Если для серверной службы настроено разрешение HTTP-трафика, в базовых URL-адресах можно указать HTTP, а затем настроить для проектов разрешение трафика с открытым текстом:

```csharp
public static string BaseAddress =
    DeviceInfo.Platform == DevicePlatform.Android ? "http://10.0.2.2:5000" : "http://localhost:5000";
public static string TodoItemsUrl = $"{BaseAddress}/api/todoitems/";
```

### <a name="ios-ats-opt-out"></a>Отказ от ATS в iOS

Чтобы включить в iOS локальный трафик с открытым текстом, следует [отказаться от ATS](~/ios/app-fundamentals/ats.md#optout), добавив следующий текст в файл **Info.plist**:

```xml
<key>NSAppTransportSecurity</key>    
<dict>
    <key>NSAllowsLocalNetworking</key>
    <true/>
</dict>
```

### <a name="android-network-security-configuration"></a>Конфигурация сетевой безопасности в Android

Чтобы включить в Android локальный трафик с открытым текстом, необходимо создать конфигурацию сетевой безопасности, добавив новый XML-файл с именем **network_security_config.xml** в папку **Resources/xml**. XML-файл должен содержать следующую конфигурацию:

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
  <domain-config cleartextTrafficPermitted="true">
    <domain includeSubdomains="true">10.0.2.2</domain>
  </domain-config>
</network-security-config>
```

Затем настройте свойство **networkSecurityConfig** в узле **приложения** в манифесте Android:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest>
    <application android:networkSecurityConfig="@xml/network_security_config">
        ...
    </application>
</manifest>
```

## <a name="related-links"></a>Связанные ссылки

- [TodoREST (пример)](/samples/xamarin/xamarin-forms-samples/webservices-todorest/)
- [Включение локального HTTPS](/aspnet/core/getting-started#enable-local-https)
- [HttpClient and SSL/TLS implementation selector for iOS/macOS](~/cross-platform/macios/http-stack.md) (Селектор реализации HttpClient и SSL/TLS для iOS и macOS)
- [HttpClient Stack and SSL/TLS Implementation selector for Android](~/android/app-fundamentals/http-stack.md) (Селектор реализации HttpClient и SSL/TLS для Android)
- [Конфигурация сетевой безопасности в Android](https://devblogs.microsoft.com/xamarin/cleartext-http-android-network-security/)
- [Защита транспорта приложения в iOS](~/ios/app-fundamentals/ats.md)
- [Xamarin.Essentials. Сведения об устройстве](~/essentials/device-information.md)
