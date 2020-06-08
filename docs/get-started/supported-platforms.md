---
название: "Поддерживаемые платформы для Xamarin.Forms"; описание: "Системные требования к платформе и разработке для Xamarin.Forms."
ms.prod: xamarin ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 22.01.2020 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinforms-supported-platforms"></a>Поддерживаемые платформы для Xamarin.Forms

Приложения Xamarin.Forms могут быть написаны для следующих операционных систем:

- iOS 9 или более поздние версии;
- Android 4.4 (API 19) или более поздние версии ([подробнее](#android-platform-support)); Однако в качестве минимального API рекомендуется использовать Android 5.0 (API 21). Это обеспечит полную совместимость со всеми библиотеками поддержки Android, при этом сохранив ориентацию на большинство устройств Android.
- Универсальная платформа Windows для Windows 10.

Приложения Xamarin.Forms для iOS, Android и универсальной платформы Windows (UWP) можно разрабатывать в Visual Studio. Однако для разработки на iOS требуется подключенный к сети компьютер Mac с последней версией Xcode и минимальной версией macOS, указанной Apple. Дополнительные сведения см. в разделе [Требования к Windows](~/cross-platform/get-started/requirements.md#windows-requirements).

Приложения Xamarin.Forms для iOS и Android можно разрабатывать в Visual Studio для Mac. Дополнительные сведения см. в разделе [Требования к macOS](~/cross-platform/get-started/requirements.md#macos-requirements).

> [!NOTE]
> Для разработки приложений с помощью Xamarin.Forms требуется знакомство с [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md).

## <a name="additional-platform-support"></a>Поддержка дополнительных платформ

Кроме iOS, Android и Windows Xamarin.Forms поддерживает дополнительные платформы:

- Samsung Tizen;
- macOS 10.13 или более поздних версий
- GTK#
- WPF

Сведения о состоянии этих платформ можно найти на [странице вики-сайта о поддержке платформ Xamarin.Forms в GitHub](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support).

## <a name="android-platform-support"></a>Поддержка платформы Android

Необходимо установить последнюю версию Android SDK Tools и платформы API Android. Для обновления до последних версий можно воспользоваться [диспетчером пакетов SDK Android](~/android/get-started/installation/android-sdk.md).

Кроме того, в целевой версии или версии компиляции для проектов Android **должен** быть выбран параметр *Использовать самую новую установленную платформу*. Однако минимальной версией может быть API 19, что позволяет поддерживать устройства, использующие Android 4.4 и более поздние версии. Эти значения задаются в разделе **Параметры проекта**.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

**Параметры проекта > Приложение > Свойства приложения**

![Параметры сборки Android в Visual Studio](requirements-images/options-android-vs-sml.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

**Сборка > Общие**

![Выбор последней целевой платформы](requirements-images/options-general-sml.png)

**Сборка > Приложение Android**

![Выбор минимальной и целевой версий Android для приложения](requirements-images/options-android-sml.png)

-----

## <a name="deprecated-platforms"></a>Нерекомендуемые платформы

Эти платформы не поддерживаются при использовании Xamarin.Forms 3.0 или более поздних версий:

- *Windows 8.1 или Windows Phone 8.1 WinRT;*
- *Windows Phone 8 Silverlight.*
