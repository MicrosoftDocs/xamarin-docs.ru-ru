---
title: Использование собственных библиотек
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 1b24648f70041abb092c93f41e999b68e8f26a26
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91453263"
---
# <a name="using-native-libraries"></a>Использование собственных библиотек

Xamarin.Android поддерживает использование собственных библиотек с помощью стандартного механизма PInvoke. Вы также можете привязать к APK-файлу дополнительные собственные библиотеки, которые не являются частью операционной системы.

Чтобы развернуть собственную библиотеку с приложением Xamarin.Android, добавьте в проект двоичный файл библиотеки и задайте для его **действия сборки** значение **AndroidNativeLibrary**.

Чтобы развернуть собственную библиотеку с проектом библиотеки Xamarin.Android, добавьте в проект двоичный файл библиотеки и задайте для его **действия сборки** значение **EmbeddedNativeLibrary**.

Так как Android поддерживает несколько бинарных интерфейсов приложений (ABI), Xamarin.Android должно знать, для какого ABI создана собственная библиотека.
Это можно сделать двумя способами:

1. Сканирование пути
1. Использование элемента `AndroidNativeLibrary/Abi` в файле проекта

При сканировании пути имя родительского каталога собственной библиотеки используется для указания целевого ABI библиотеки. Таким образом при добавлении `lib/armeabi/libfoo.so` к проекту ABI будет сканироваться как `armeabi`.

Или же вы можете отредактировать файл вашего проекта, чтобы явно указать используемый ABI:

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

Дополнительные сведения об использовании собственных библиотек см. в статье [Взаимодействие с собственными библиотеками](https://www.mono-project.com/docs/advanced/pinvoke/).

## <a name="debugging-native-code-with-visual-studio"></a>Отладка машинного кода в Visual Studio

Если вы используете *Visual Studio 2019* или *Visual Studio 2017*, вам не нужно изменять файлы проекта, как описано выше.
Вы можете выполнять сборку и отладку C++ в решении Xamarin.Android, добавив ссылку на проект на C++ **Динамическая общая библиотека (Android)** .

Чтобы выполнить отладку машинного кода C++ в вашем проекте, выполните указанные ниже действия.

1. Дважды щелкните пункт **Свойства** проекта и выберите страницу **Параметры Android**.
2. Прокрутите вниз до пункта **Параметры отладки**.
3. В раскрывающемся меню **Отладчик** выберите **C++** (вместо **.NET (Xamarin)** , который задан по умолчанию).

Разработчики Visual Studio C++ могут просмотреть пример [SanAngeles_NativeDebug](/samples/xamarin/monodroid-samples/sanangeles-ndk), чтобы попробовать выполнить отладку C++ из Visual Studio 2019 или Visual Studio 2017 с помощью Xamarin. Дополнительные сведения см. в [записи блога](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/).

## <a name="related-links"></a>Связанные ссылки

- [SanAngeles_NativeDebug (пример)](/samples/xamarin/monodroid-samples/sanangeles-ndk)
- [Разработка собственных приложений Xamarin Android](https://blogs.msdn.microsoft.com/vcblog/2015/02/23/developing-xamarin-android-native-applications/)