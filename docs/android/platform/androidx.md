---
title: AndroidX
description: Начните разрабатывать приложения с помощью AndroidX и Xamarin.Android.
ms.assetid: CC21BD28-EF67-4132-8C0D-CF25B78BA78B
author: JonDouglas
ms.author: jodou
ms.date: 02/20/2020
ms.openlocfilehash: 05736fda26e5fd391d977481b340b3744f224c2f
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91454212"
---
# <a name="androidx-with-xamarin"></a>Разработка приложений с помощью AndroidX и Xamarin

_Начните разрабатывать приложения с помощью AndroidX и Xamarin.Android._

AndroidX — это значительно усовершенствованная библиотека поддержки Android, исходная версия которой больше не поддерживается. Пакеты **AndroidX** полностью заменяют библиотеку поддержки Android, обеспечивая равенство функций и предоставляя новые библиотеки, которые можно использовать в приложениях Android.

AndroidX содержит следующие возможности:

- Все пакеты в AndroidX теперь имеют согласованное пространство имен, начинающееся с `androidx`. Это означает, что все пакеты библиотеки поддержки Android сопоставляются с соответствующим пакетом `androidx.*`.
- Пакеты `androidx` обслуживаются и обновляются отдельно. Это означает, что библиотеки AndroidX можно обновлять независимо друг от друга.
- Начиная с версии 28 библиотека поддержки Android больше не будет выпускаться. Все новые разработки будут включены в `androidx`.

![Логотип AndroidX](~/android/platform/androidx-images/AndroidXLogo.png)

## <a name="requirements"></a>Требования

Компоненты из следующего списка необходимы для использования возможностей AndroidX в приложениях на основе Xamarin.

- **Visual Studio**. В Windows выполните обновление до Visual Studio 2019 версии 16.4 или более поздней. В macOS выполните обновление до Visual Studio 2019 для Mac версии 8.4 или более поздней.
- **Xamarin.Android**. Вместе с Visual Studio нужно установить Xamarin.Android 10.0 или более поздней версии (Xamarin.Android автоматически устанавливается в составе рабочей нагрузки **Разработка мобильных приложений на .NET** в ОС Windows или в составе **установщика Visual Studio для Mac**).
- Пакет **Java Developer Kit**. Для разработки на Xamarin.Android 10.0 требуется JDK 8. Дистрибутив OpenJDK от корпорации Майкрософт автоматически устанавливается в составе Visual Studio.
- **Пакет SDK для Android**. Необходимо установить пакет SDK для Android (API 28) или более поздней версии с помощью Диспетчера SDK Android.

## <a name="get-started"></a>Начало работы

Вы можете приступить к работе с AndroidX, включив в свой проект Android любой [пакет NuGet AndroidX](https://www.nuget.org/packages?q=Tags%3A%22AndroidX%22+Authors%3A%22Microsoft%22). Подробнее об установке и использовании пакета в [Visual Studio](/nuget/quickstart/install-and-use-a-package-in-visual-studio) и [Visual Studio для Mac](/nuget/quickstart/install-and-use-a-package-in-visual-studio-mac)

## <a name="behavior-changes"></a>Изменения в поведении

AndroidX представляет собой переработанную библиотеку поддержки Android. Она включает этапы миграции, которые повлияют на приложения Android, созданные с помощью библиотеки поддержки Android.

### <a name="package-name-change"></a>Изменение названий пакетов
Имена старых и новых пакетов отличаются. Ниже приведены примеры этих различий.

| Прежний вариант                    | Оператор new                    |
| ---------------------- | ---------------------- |
| android.support.**     | androidx.@             |
| android.design.**      | com.google.android.material.@ |
| android.support.test.** | androidx.test.@       |
| android.arch.**        | androidx.@             |
| android.arch.persistence.room.** | androidx.room.@ |
| android.arch.persistence.** | androidx.sqlite.@ |

Дополнительные сведения об именовании пакетов [см. в этой документации](https://developer.android.com/jetpack/androidx/migrate#artifact_mappings).

## <a name="migration-tooling"></a>Средства миграции

Миграция состоит из трех этапов, которые нужно учитывать для вашего приложения.

1. Если приложение включает **пространства имен библиотеки поддержки Android и вы хотите перейти с них на пространства имен AndroidX**, для выполнения большинства сценариев с пространствами имен можно использовать средства IDE **миграции в AndroidX**. 

Чтобы включить **средство миграции в AndroidX**, в Visual Studio 2019 щелкните **Сервис > Параметры > Xamarin > Параметры Android** (в Visual Studio для Mac этот шаг можно пропустить).

![Включение средства миграции в AndroidX](~/android/platform/androidx-images/EnableAndroidXMigrator.png)

Щелкните проект правой кнопкой мыши и выберите **Миграция на AndroidX**.

![Пункт "Миграция на AndroidX"](~/android/platform/androidx-images/MigrateToAndroidX.png)

> [!NOTE] 
> Для сценариев, с которыми это средство не работает, вам нужно будет вручную внести изменения в пространство имен. Правильный пакет сопоставляется автоматически, но мы рекомендуем ознакомиться с официальным [сопоставлением артефактов](https://developer.android.com/jetpack/androidx/migrate/artifact-mappings) и [классов](https://developer.android.com/jetpack/androidx/migrate/class-mappings), чтобы упростить миграцию проектов.

2. Если приложение включает **зависимости, которые не были перенесены в пространство имен AndroidX**, необходимо использовать [библиотеку поддержки Android для пакета миграции AndroidX.](https://www.nuget.org/packages/Xamarin.AndroidX.Migration)
3. Если приложение **не содержит зависимостей, требующих миграции в пространство имен AndroidX**, вы можете прямо сейчас использовать [библиотеки AndroidX в NuGet](https://www.nuget.org/packages?q=Tags%3A%22AndroidX%22+Authors%3A%22Microsoft%22).

## <a name="troubleshooting"></a>Устранение неполадок

- Некоторые пакеты архитектуры в AndroidX могут конфликтовать с версиями библиотеки поддержки. Чтобы устранить эту проблему, следует использовать версию AndroidX этих пакетов и удалить версию библиотеки поддержки. Например, если вы ссылаетесь в проекте на `Xamarin.Android.Arch.Work.Runtime`, возникнет конфликт с типами нового добавленного пакета `AndroidX.Work`.

## <a name="summary"></a>Сводка

Из этой статьи вы узнали о том, что такое AndroidX, а также об установке и настройке новейших средств и пакетов для разработки приложений Xamarin.Android с помощью AndroidX. Статья содержит общие сведения об AndroidX. В ней содержатся ссылки на документацию по API и разделы для разработчиков Android, которые помогут вам приступить к созданию приложений с помощью AndroidX. В статье также описаны самые важные изменения в поведении AndroidX, которые могут повлиять на существующие приложения, и приведен раздел об устранении неполадок.

## <a name="related-links"></a>Связанные ссылки

- [Введение в AndroidX | Xamarin Show](https://www.youtube.com/watch?v=M_l3RjTev5A)
- [AndroidX](https://developer.android.com/jetpack/androidx)
- [Репозиторий Xamarin AndroidX в GitHub](https://github.com/xamarin/AndroidX)
- [Репозиторий GitHub для миграции в Xamarin AndroidX](https://github.com/xamarin/XamarinAndroidXMigration)