---
title: Привязка JAR-файла
description: Это пошаговое руководство содержит инструкции по созданию библиотеки привязок Java для Xamarin.Android из JAR-файла на платформе Android.
ms.prod: xamarin
ms.assetid: 93F1D5C5-E2AF-46EA-8460-485A0860C176
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/11/2018
ms.openlocfilehash: 336462176813b9985e2c269273d08c4aacbea9b5
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853098"
---
# <a name="binding-a-jar"></a>Привязка JAR-файла

> [!IMPORTANT]
> В настоящее время рассматривается возможность использования настраиваемых привязок на платформе Xamarin. Примите участие в [**этом опросе**](https://www.surveymonkey.com/r/KKBHNLT), чтобы помочь определить дальнейшие направления разработки.

_Это пошаговое руководство содержит инструкции по созданию библиотеки привязок Java для Xamarin.Android из JAR-файла на платформе Android._

## <a name="overview"></a>Обзор

Сообщество Android предлагает много библиотек Java, которые можно использовать в приложении. Эти библиотеки Java часто упаковываются в виде JAR-файла (архив Java), но вы можете создать для этого JAR-файла привязку в *библиотеке привязок Java*, чтобы его функции стали доступны для приложений Xamarin.Android. Библиотека привязок Java предназначена для того, чтобы сделать API-интерфейсы из JAR-файла доступными в коде C# через автоматически создаваемые оболочки кода.

Средства Xamarin могут создавать библиотеку привязок из одного или нескольких JAR-файлов. Библиотека привязок (сборка в виде DLL-файла) содержит следующие элементы. 

- Содержимое исходных JAR-файлов.

- Вызываемые оболочки управляемого кода (MCW), которые являются типами C# для переноса соответствующих типов Java из JAR-файлов.

Созданный код MCW использует нативный интерфейс Java (JNI) для переадресации вызовов API в исходный JAR-файл. Вы можете создать библиотеки привязок для любого JAR-файла, который предназначен для использования с Android (обратите внимание, что в настоящее время средства Xamarin не поддерживают привязку библиотек Java для платформ, отличных от Android). Вы также можете выполнить сборку библиотеку привязок, не включая содержимое JAR-файла, и тогда библиотека DLL будет иметь зависимость от JAR-файла во время выполнения.

В этом руководстве мы рассмотрим основные концепции создания библиотеки привязок из одного JAR-файла. Мы продемонстрируем пример, в котором все прошло удачно, то есть не потребовалась настройка или отладка привязок. 
В примере [создания привязок на основе метаданных](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) показан более сложный сценарий, в котором процесс привязки невозможно выполнить полностью автоматически и требуется вмешательство пользователя. Общие сведения (с простым примером кода) о создании библиотек привязок для Java см. в разделе [Привязка библиотеки Java](~/android/platform/binding-java-library/index.md). 

## <a name="walkthrough"></a>Пошаговое руководство

В следующем пошаговом руководстве мы создадим библиотеку привязок для популярного пакета Android [Picasso](https://square.github.io/picasso/), который предоставляет функции загрузки и кэширования изображений. Выполните следующие действия, чтобы привязать файл **picasso-2.x.x.jar** и создать новую сборку .NET, которую можно использовать в проекте Xamarin.Android: 

1. Создайте проект библиотеки привязок Java.

2. Добавьте JAR-файл в этот проект.

3. Задайте нужное действие сборки для этого JAR-файла.

4. Выберите целевую платформу, которую поддерживает JAR-файл.

5. Выполните сборку библиотеки привязок.

После создания библиотеки привязок мы создадим небольшое приложение Android, которое демонстрирует возможность вызова API через библиотеку привязок. В этом примере мы хотим получить доступ к методам из **picasso-2.x.x.jar**:

```java
package com.squareup.picasso

public class Picasso
{ 
    ...
    public static Picasso with (Context context) { ... };
    ...
    public RequestCreator load (String path) { ... };
    ...
}
```

После создания библиотеки привязок для **picasso-2.x.x.jar** вы сможете вызывать эти методы в коде C#. Пример:

```csharp
using Com.Squareup.Picasso;
...
Picasso.With (this)
    .Load ("http://mydomain.myimage.jpg")
    .Into (imageView);

```

### <a name="creating-the-bindings-library"></a>Создание библиотеки привязок

Прежде чем переходить к следующим шагам, скачайте файл [picasso-2.x.x.jar](http://repo1.maven.org/maven2/com/squareup/picasso/picasso/2.5.2/picasso-2.5.2.jar).

Для начала создайте проект библиотеки привязок. В Visual Studio для Mac или Visual Studio создайте новое решение и выберите шаблон *Библиотека привязок (Android)* . (Снимки экрана в этом пошаговом руководстве взяты из Visual Studio, но Visual Studio для Mac мало чем отличается.) Присвойте решению имя **JarBinding**. 

[![Создание проекта библиотеки JarBinding](binding-a-jar-images/01-new-bindings-library-sml.w157.png)](binding-a-jar-images/01-new-bindings-library.w157.png#lightbox)

Шаблон включает папку **Jars**, в которую вы добавите JAR-файлы для проекта библиотеки привязок. Щелкните папку **Jars** правой кнопкой мыши и выберите **Добавить > Существующий элемент**. 

[![Добавление существующего элемента](binding-a-jar-images/02-add-existing-item-sml.png)](binding-a-jar-images/02-add-existing-item.png#lightbox)

Перейдите к ранее скачанному файлу **picasso-2.x.x.jar**, выберите его и щелкните **Добавить**. 

[![Выбор файла и нажатие кнопки "Добавить"](binding-a-jar-images/03-select-jar-file-sml.png)](binding-a-jar-images/03-select-jar-file.png#lightbox)

Убедитесь, что файл **picasso-2.x.x.jar** успешно добавлен в проект. 

[![JAR-файл добавлен в проект](binding-a-jar-images/04-jar-added-sml.png)](binding-a-jar-images/04-jar-added.png#lightbox)

При создании проекта библиотеки привязок Java необходимо указать, будет ли JAR-файл внедрен в библиотеку привязок или упакован отдельно. Для этого укажите одно из следующих *действий сборки*: 

- **EmbeddedJar** — JAR-файл будет внедрен в библиотеку привязок.

- **InputJar** — JAR-файл будет храниться отдельно от библиотеки привязок.

Обычно используется действие сборки **EmbeddedJar**, чтобы JAR-файл автоматически добавлялся в библиотеку привязок. Это самый простой вариант, при котором байт-код Java из JAR-файла преобразуется в байтовый код DEX и внедряется (вместе с вызываемыми оболочками управляемого кода) в созданный APK. Если вы хотите, чтобы JAR-файл хранился отдельно от библиотеки привязок, можно использовать вариант **InputJar**, но при этом вам придется обеспечить наличие этого JAR-файла на устройстве, на котором выполняется приложение. 

Выберите действие сборки **EmbeddedJar**. 

[![Выбор действия сборки EmbeddedJar](binding-a-jar-images/05-embeddedjar-sml.png)](binding-a-jar-images/05-embeddedjar.png#lightbox)

Теперь в разделе "Свойства" для проекта задайте значение для параметра *Целевая платформа*. Если JAR-файл использует API-интерфейсы Android, для целевой платформы нужно указать тот уровень API который ожидает этот JAR-файл. Как правило, разработчик JAR-файла указывает один или несколько уровней API, с которыми совместим этот JAR-файл. (Дополнительную информацию о настройке целевой платформы и уровней API Android см. в [этой статье](~/android/app-fundamentals/android-api-levels.md).)

Настройте целевой уровень API для библиотеки привязок (в этом примере мы используем уровень API 19). 

[![Выбор целевого уровня API 19](binding-a-jar-images/06-set-target-framework-sml.png)](binding-a-jar-images/06-set-target-framework.png#lightbox)

Наконец, выполните сборку библиотеки привязок. Возможно, отобразятся некоторые предупреждения, но проект библиотеки привязок должен успешно пройти сборку и создать готовый DLL-файл по следующему адресу: **JarBinding/bin/Debug/JarBinding.dll**

### <a name="using-the-bindings-library"></a>Использование библиотеки привязок

Чтобы использовать этот DLL-файл в приложении Xamarin.Android, выполните следующие действия:

1. Добавьте ссылку на библиотеку привязок.

2. Выполните вызовы к JAR-файлу через вызываемые оболочки управляемого кода. 

На следующих шагах мы создадим простейшее приложение, которое использует библиотеку привязок для скачивания и отображения изображения из `ImageView`. Вся основная работа выполняется кодом, который находится в JAR-файле. 

Для начала создайте приложение Xamarin.Android, которое использует библиотеку привязок. Щелкните "Решение" правой кнопкой мыши и выберите **Добавить новый проект**, затем присвойте проекту имя **BindingTest**. Мы создаем это приложение в том же решении, что и библиотеку привязок, чтобы упростить это пошаговое руководство. Но приложение, использующее библиотеку привязок, может находиться в другом решении: 

[![Добавление нового проекта BindingTest](binding-a-jar-images/07-add-new-project-sml.w157.png)](binding-a-jar-images/07-add-new-project.w157.png#lightbox)

Щелкните правой кнопкой мыши узел **Ссылки** для проекта **BindingTest** и выберите команду **Добавить ссылку...** .

[![Добавление ссылки через контекстное меню](binding-a-jar-images/08-add-reference.png)](binding-a-jar-images/08-add-reference.png#lightbox)

Выберите ранее созданный проект **JarBinding** и щелкните **ОК**.

[![Выбор проекта JarBinding](binding-a-jar-images/09-choose-jar-binding-sml.png)](binding-a-jar-images/09-choose-jar-binding.png#lightbox)

Откройте узел **Ссылки** для проекта **BindingTest**, чтобы убедиться в наличии ссылки на **JarBinding**. 

[![JarBinding отображается в списке ссылок](binding-a-jar-images/10-references-shows-jarbinding-sml.png)](binding-a-jar-images/10-references-shows-jarbinding.png#lightbox)

Измените макет **BindingTest** (**Main.axml**), чтобы в нем был один `ImageView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <ImageView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/imageView" />
</LinearLayout>
```

Добавьте следующую инструкцию `using` в файл **MainActivity.cs**, чтобы было проще обращаться к методам класса `Picasso` на основе кода Java, включенного в библиотеку привязок:

```csharp
using Com.Squareup.Picasso;
```

Измените метод `OnCreate`, чтобы в нем использовался класс `Picasso` для загрузки изображение по URL-адресу и отображения в `ImageView`: 

```csharp
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);
        ImageView imageView = FindViewById<ImageView>(Resource.Id.imageView);

        // Use the Picasso jar library to load and display this image:
        Picasso.With (this)
            .Load ("http://i.imgur.com/DvpvklR.jpg")
            .Into (imageView);
    }
}
```

Скомпилируйте и запустите проект **BindingTest**. Приложение откроется и после небольшой задержки (в зависимости от условий сети) скачает и отобразит изображение, примерно как на следующем снимке экрана:

[![Снимок экрана с запущенным приложением BindingTest](binding-a-jar-images/11-result-sml.png)](binding-a-jar-images/11-result.png#lightbox)

Поздравляем! Вы успешно привязали библиотеку Java из JAR-файла и применили ее в приложении Xamarin.Android.

## <a name="summary"></a>Сводка

Из этого пошагового руководства вы узнали, как создали библиотеку привязок для стороннего JAR-файла, добавить эту библиотеку привязок в простейшее тестовое приложение и запустили это приложение для проверки того, как наш код C# вызывает код Java из JAR-файла. 

## <a name="related-links"></a>Связанные ссылки

- [Создание библиотеки привязок Java (видео)](https://university.xamarin.com/classes#10090)
- [Привязка библиотеки Java](~/android/platform/binding-java-library/index.md)
