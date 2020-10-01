---
title: Пошаговое руководство. Привязка библиотеки Kotlin для Android
description: В этой статье приводятся пошаговые инструкции по созданию привязки существующей библиотеки Kotlin BubblePicker к приложению Xamarin.Android. В ней рассматриваются такие темы, как компиляция библиотеки Kotlin, ее привязка и использование этой привязки в приложении Xamarin.Android.
ms.prod: xamarin
ms.assetid: 5E45451F-514F-4F2B-A6E8-599ED2EFDA2C
ms.technology: xamarin-android
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: d75ec48bf9736297c31e0bb5af5a71de4332c66b
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91454277"
---
# <a name="walkthrough-bind-an-android-kotlin-library"></a>Пошаговое руководство. Привязка библиотеки Kotlin для Android

> [!IMPORTANT]
> В настоящее время рассматривается возможность использования настраиваемых привязок на платформе Xamarin. Примите участие в [**этом опросе**](https://www.surveymonkey.com/r/KKBHNLT), чтобы помочь определить дальнейшие направления разработки.

Xamarin позволяет разработчикам мобильных приложений создавать собственные кросс-платформенные мобильные приложения с использованием Visual Studio и C#. Вы можете использовать готовые компоненты пакета SDK для платформы Android, но во многих случаях вам также может понадобиться использовать сторонние пакеты SDK, написанные для этой платформы, и Xamarin позволяет сделать это с помощью привязок. Чтобы внедрить стороннюю платформу Android в приложение Xamarin.Android, необходимо создать привязку Xamarin.Android, чтобы использовать ее в приложениях.

Платформа Android, а также ее машинные языки и средства постоянно развиваются. К примеру, недавно введен язык Kotlin, который, в конечном итоге, заменяет Java. Существует ряд сторонних пакетов SDK, которые уже перенесены с Java на Kotlin, и это ставит перед нами новые задачи. Несмотря на то, что процесс привязки Kotlin аналогичен этому процессу на Java, он требует дополнительных шагов и параметров конфигурации для успешной сборки и запуска в рамках приложения Xamarin.Android.  

Цель этого документа — описать общий подход для подготовки такого сценария и предоставить подробные пошаговые инструкции с простым примером.

## <a name="background"></a>Фон

Kotlin выпущен в феврале 2016 г. и представлен в качестве альтернативы стандартному компилятору Java в Android Studio к 2017 г. Позднее в 2019 г. корпорация Google объявила о том, что язык Kotlin стал предпочтительным языком для разработчиков приложений Android. Общий подход на основе привязки аналогичен [процессу привязки регулярных библиотек Java](../binding-java-library/index.md), но при использовании в Kotlin необходимы несколько важных специальных шагов.

## <a name="prerequisites"></a>Предварительные требования

Для выполнения данного пошагового руководства требуется:

- [Android Studio](https://developer.android.com/studio)
- [Visual Studio для Mac](https://visualstudio.microsoft.com/downloads)
- [Декомпилятор Java](http://java-decompiler.github.io/)

## <a name="build-a-native-library"></a>Сборка собственной библиотеки

Первым шагом является сборка собственной библиотеки Kotlin с помощью Android Studio. Библиотека обычно предоставляется сторонним разработчиком или доступна в [репозитории Google Maven](https://maven.google.com/web/index.html) и других удаленных репозиториях. Например, в этом учебнике создается привязка для библиотеки Kotlin Bubble Picker:

![Демонстрация BubblePicker в GitHub](walkthrough-images/github-bubblepicker-demo.png)

1. Скачайте [исходный код](https://github.com/igalata/Bubble-Picker/archive/develop.zip) из GitHub для библиотеки и распакуйте его в локальной папке **Bubble-Picker**.

1. Запустите Android Studio, выберите пункт меню **Open an existing Android Studio project** (Открыть существующий проект Android Studio) и укажите локальную папку Bubble-Picker:

    ![Открытие проекта в Android Studio](walkthrough-images/android-studio-open-project.png)

1. Убедитесь, что Android Studio не устарела, в том числе Gradle. Исходный код можно успешно собрать в Android Studio версии 3.5.3, Gradle версии 5.4.1. Инструкции по обновлению Gradle до последней версии [можно найти здесь](https://gradle.org/install/).

1. Убедитесь, что установлен обязательный пакет SDK для Android. Исходный код требует пакет SDK для Android версии 25. Откройте пункт меню **Сервис > Диспетчер пакетов SDK**, чтобы установить компоненты пакета SDK.

1. Обновите и синхронизируйте главный файл конфигурации **build.gradle**, расположенный в корне папки проекта:

    - Задайте параметру версии Kotlin значение 1.3.10.

        ```gradle
        buildscript {
            ext.kotlin_version = '1.3.10'
        }
        ```

    - Зарегистрируйте репозиторий Google Maven по умолчанию, чтобы можно было разрешить зависимость библиотеки поддержки:

        ```gradle
        allprojects {
            repositories {
                jcenter()
                maven {
                    url "https://maven.google.com"
                }
            }
        }
        ```

    - После обновления файла конфигурации он не синхронизирован, и в Gradle будет отображаться кнопка **Sync Now** (Синхронизировать сейчас), нажмите ее и дождитесь завершения процесса:

        ![Кнопка Sync Now (Синхронизировать сейчас) в Gradle в Android Studio](walkthrough-images/android-studio-gradle-syncnow.png)

        > [!TIP]
        > Кэш зависимостей Gradle может быть поврежден, это иногда происходит после истечения времени ожидания сетевого подключения. Повторно скачайте зависимости и синхронизируйте проект (требуется подключение к сети).

        > [!TIP]
        > У процесса сборки Gradle (управляющей программ) может быть поврежденное состояние. Остановка всех управляющих программ Gradle может решить эту проблему. Остановите процессы сборки Gradle (требуется перезапуск). В случае повреждения процессов Gradle можно также закрыть интегрированную среду разработки, а затем завершить все процессы Java.

        > [!TIP]
        > Возможно, ваш проект использует сторонний подключаемый модуль, который несовместим с другими подключаемыми модулями в проекте или версии Gradle, запрошенной проектом.

1. Откройте меню Gradle справа, перейдите в меню **bubblepicker > Tasks** (Задачи), выполните задачу **build**, дважды коснувшись ее, и дождитесь завершения сборки:

    ![Выполнение задачи в Gradle в Android Studio](walkthrough-images/android-studio-gradle-execute-task.png)

1. Откройте корневую папку в обозревателе файлов и перейдите к папке build: **Bubble-Picker -> bubblepicker -> build -> outputs  -> aar**, сохраните файл **bubblepicker-release.aar** с именем **bubblepicker-v1.0.aar**. Этот файл будет использоваться позже в процессе привязки:

    ![Выходные данные AAR в Android Studio](walkthrough-images/android-studio-aar-output.png)

Файл AAR — это архив Android, который содержит скомпилированный исходный код и ресурсы Kotlin, необходимые Android для выполнения приложения с использованием этого пакета SDK.  

## <a name="prepare-metadata"></a>Подготовка метаданных

Вторым шагом является подготовка файла преобразования метаданных, который используется Xamarin.Android для создания соответствующих классов C#. Проект привязки Xamarin.Android будет обнаруживать все собственные классы и члены из заданного архива Android с последующим созданием файла XML с соответствующими метаданными. Затем созданный вручную файл преобразования метаданных необходимо применить к ранее созданному базовому плану, чтобы создать окончательный файл XML определения, используемый для создания кода C#.

Метаданные используют синтаксис  [XPath](https://www.w3.org/TR/xpath/)  и используются генератором привязок, чтобы повлиять на создание сборки привязки. В статье [Метаданные привязок Java](../binding-java-library/customizing-bindings/java-bindings-metadata.md) приведены дополнительные сведения о преобразованиях, которые могут быть применены:

1. Создайте пустой файл **Metadata.xml**:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <metadata>
    </metadata>
    ```

1. Определите файл XML преобразований:

- У собственной библиотеки Kotlin есть две зависимости, которые вам не нужно предоставлять в коде C#, задайте два преобразования, чтобы полностью их игнорировать. Важно отметить, что собственные члены не будут удалены из результирующего двоичного файла, только не будут создаваться классы C#. [Декомпилятор Java](http://java-decompiler.github.io/) можно использовать для определения зависимостей. Запустите средство и откройте созданный ранее файл AAR, в результате чего будет показана структура архива Android со всеми зависимостями, значениями, ресурсами, манифестами и классами:  

    ![Зависимости декомпилятора Java](walkthrough-images/java-decompiler-dependencies.png)

    Преобразования для пропуска обработки этих пакетов определяются с помощью инструкций XPath:

    ```xml
    <remove-node path="/api/package[starts-with(@name,'org.jbox2d')]" />
    <remove-node path="/api/package[starts-with(@name,'org.slf4j')]" />
    ```

- У собственного класса `BubblePicker` есть два метода: `getBackgroundColor` и `setBackgroundColor`, а следующее преобразование изменит это на свойство `BackgroundColor` C#:

    ```xml
    <attr path="/api/package[@name='com.igalata.bubblepicker.rendering']/class[@name='BubblePicker']/method[@name='getBackground' and count(parameter)=0]" name="propertyName">BackgroundColor</attr>
    <attr path="/api/package[@name='com.igalata.bubblepicker.rendering']/class[@name='BubblePicker']/method[@name='setBackground' and count(parameter)=1 and parameter[1][@type='int']]" name="propertyName">BackgroundColor</attr>
    ```

- Для неподписанных типов `UInt, UShort, ULong, UByte` требуется специальная обработка. Для этих типов Kotlin автоматически изменяет имена методов и типы параметров, что отражается в созданном коде:

    ```Kotlin
    public open fun fooUIntMethod(value: UInt) : String {
        return "fooUIntMethod${value}"
    }
    ```

    Этот код компилируется в следующий байтовый код Java:

    ```Java byte code
    @NotNull
    public String fooUIntMethod-WZ4Q5Ns(int value) {
    return "fooUIntMethod" + UInt.toString-impl(value);
    }
    ```

    Более того, связанные типы, такие как `UIntArray, UShortArray, ULongArray, UByteArray`, также затрагиваются Kotlin. Имя метода изменяется и включает в себя дополнительный суффикс, а параметры изменяются на массив элементов подписанных версий тех же типов. В примере ниже параметр типа `UIntArray` преобразуется автоматически в `int[]`, а имя метода изменяется с `fooUIntArrayMethod` на `fooUIntArrayMethod--ajY-9A`. Имя метода обнаруживается средствами Xamarin.Android и формируется как допустимое имя метода:

    ```Kotlin
    public open fun fooUIntArrayMethod(value: UIntArray) : String {
        return "fooUIntArrayMethod${value.size}"
    }
    ```

    Этот код компилируется в следующий байтовый код Java:

    ```Java byte code
    @NotNull
    public String fooUIntArrayMethod--ajY-9A(@NotNull int[] value) {
        Intrinsics.checkParameterIsNotNull(value, "value");
        return "fooUIntArrayMethod" + UIntArray.getSize-impl(value);
    }
    ```

    Чтобы дать ему понятное имя, в **Metadata.xml** можно добавить следующие метаданные, которые обновят имя на исходное, определенное в коде Kotlin:

    ```xml
    <attr path="/api/package[@name='com.microsoft.simplekotlinlib']/class[@name='FooClass']/method[@name='fooUIntArrayMethod--ajY-9A']" name="managedName">fooUIntArrayMethod</attr>
    ```

    В примере BubblePicker нет членов, использующих неподписанные типы, поэтому дополнительные изменения не требуются.

- Члены Kotlin с универсальными параметрами по умолчанию преобразуются в параметры типа `Lang.Object` Java. Например, метод Kotlin имеет универсальный параметр \<T>:

    ```Kotlin
    public open fun <T>fooGenericMethod(value: T) : String {
    return "fooGenericMethod${value}"
    }
    ```

    После создания привязки Xamarin.Android метод предоставляется в C# следующим образом:

    ```csharp
    [Register ("fooGenericMethod", "(Ljava/lang/Object;)Ljava/lang/String;", "GetFooGenericMethod_Ljava_lang_Object_Handler")]
    [JavaTypeParameters (new string[] {
        "T"
    })]

    public virtual string FooGenericMethod (Java.Lang.Object value);
    ```

    Универсальные шаблоны Java и Kotlin не поддерживаются привязками Xamarin.Android, поэтому создается обобщенный метод C# для доступа к универсальному API. В качестве обходного решения можно создать библиотеку-оболочку Kotlin и предоставить необходимые API строго типизированным способом без универсальных шаблонов. Кроме того, можно создавать вспомогательные приложения на C# для решения проблемы таким же образом с помощью строго типизированных API.

    > [!TIP]
    > При преобразовании метаданных к созданной привязке можно применить любые изменения. В статье [Привязка библиотеки Java](../binding-java-library/index.md) подробно объясняется, как создаются и обрабатываются метаданные.

## <a name="build-a-binding-library"></a>Сборка библиотеки привязки

Следующим шагом является создание проекта привязки Xamarin.Android с помощью шаблона привязки Visual Studio, добавления необходимых метаданных, собственных ссылок и последующая сборка проекта для создания готовой к использованию библиотеки:

1. Откройте Visual Studio для Mac и создайте проект библиотеки привязки Xamarin.Android, присвойте ему имя, в этом случае — **testBubblePicker.Binding**, и завершите работу мастера. Шаблон привязки Xamarin.Android расположен по следующему пути: **Android > Библиотека > Binding Library** (Библиотека привязки):

    ![Создание привязки в Visual Studio](walkthrough-images/visual-studio-create-binding.png)

    В папке Transformations есть три основных файла преобразования:

    - **Metadata.xml** — позволяет вносить изменения в окончательный API, например изменять пространство имен созданной привязки.
    - **EnumFields.xml** — содержит сопоставление константами int Java и перечислениями C#.
    - **EnumMethods.xml** — позволяет изменять параметры метода и типы возвращаемых значений с целочисловых констант Java на перечисления C#.

    Сохраните пустые файлы **EnumFields.xml** и **EnumMethods.xml**, а затем обновите **Metadata.xml** для определения преобразований.

1. Замените существующий файл **Metadata.xml** в папке Transformations файлом **Metadata.xml**, созданным на предыдущем шаге. В окне свойств убедитесь, что для файла **Build Action** задано значение **TransformationFile**:

    ![Метаданные в Visual Studio](walkthrough-images/visual-studio-metadata.png)

1. Добавьте файл **bubblepicker-v1.0.aar**, созданный на шаге 1 для проекта привязки в качестве собственной ссылки. Чтобы добавить ссылки на собственную библиотеку, откройте систему поиска и перейдите в папку с архивом Android. Перетащите архив в папку Jars в обозревателе решений. Кроме того, можно использовать команду контекстного меню **Добавить** для папки Jars и выбрать пункт **Существующие файлы...** . Скопируйте файл в каталог для дальнейшего использования в этом пошаговом руководстве. Не забудьте убедиться, что **Build Action** задано значение **LibraryProjectZip**:

    ![Собственная ссылка в Visual Studio](walkthrough-images/visual-studio-native-reference.png)

1. Добавьте ссылку на пакет [Xamarin.Kotlin.StdLib NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/). Этот пакет является привязкой стандартной библиотеки Kotlin. Без этого пакета привязка будет работать только в том случае, если библиотека Kotlin не использует специфические типы Kotlin, иначе все эти члены не будут предоставлены в коде C#, а любое приложение, пытающееся использовать привязку, завершится сбоем во время выполнения.

    > [!TIP]
    > Из-за ограничения Xamarin.Android средства привязки можно добавить только в один архив Android (AAR) для каждого проекта привязки. Если необходимо добавить несколько файлов AAR, требуется несколько проектов Xamarin.Android, по одному на каждый AAR. Если бы мы столкнулись с этим в этом пошаговом руководстве, то предыдущие четыре действия этого шага надо было бы повторить для каждого архива. В качестве альтернативного варианта можно вручную объединить несколько архивов Android в один, и в результате можно использовать один проект привязки Xamarin.Android.

1. Завершающим действием является сборка библиотеки без ошибок компиляции. В случае ошибок компиляции они могут быть устранены и обработаны с помощью файла Metadata.xml, созданного ранее путем добавления метаданных преобразования XML, которые будут добавлять, удалять или переименовывать элементы библиотеки.

## <a name="consume-the-binding-library"></a>Использование библиотеки привязки

Последним шагом является использование библиотеки привязки Xamarin.Android в приложении Xamarin.Android. Создайте проект Xamarin.Android, добавьте ссылку на библиотеку привязки и визуализируйте пользовательский интерфейс Bubble Picker:

1. Создайте проект Xamarin.Android. Используйте **Android > Приложение > Приложение Android** как отправную точку и выберите **Самое новое и интересное** в качестве целевой платформы, чтобы избежать проблем совместимости. Все последующие шаги предназначены для этого проекта:

    ![Создание приложения в Visual Studio](walkthrough-images/visual-studio-create-app.png)

1. Добавьте ссылку на проект в проект привязки или добавьте ссылку на созданную ранее библиотеку DLL:

    ![Изображение PNG добавление ссылки в привязку в Visual Studio](walkthrough-images/visual-studio-add-binding-reference.png)

1. Добавьте ссылку на пакет [Xamarin.Kotlin.StdLib NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/), который вы ранее добавили в проект привязки Xamarin.Android. Этот пакет поддерживает любые специальные типы Kotlin, которые должны обрабатываться в среде выполнения.  Без этого пакета приложение можно скомпилировать, но произойдет сбой во время выполнения:

    ![Добавление StdLib NuGet в Visual Studio](walkthrough-images/visual-studio-add-stdlib-nuget.png)

1. Добавьте элемент управления `BubblePicker` в макет Android для `MainActivity`. Откройте файл **testBubblePicker/Resources/layout/content_main.xml** и добавьте узел элементов управления BubblePicker в виде последнего элемента корневого элемента управления RelativeLayout:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout …>
        …
        <com.igalata.bubblepicker.rendering.BubblePicker
            android:id="@+id/picker"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:backgroundColor="@android:color/white" />
    </RelativeLayout>
    ```

1. Обновите исходный код приложения и добавьте логику инициализации в `MainActivity`, которая активирует пакет SDK для Bubble Picker:

    ```csharp
    protected override void OnCreate(Bundle savedInstanceState)
    {
        ...
        var picker = FindViewById<BubblePicker>(Resource.Id.picker);
        picker.BubbleSize = 20;
        picker.Adapter = new BubblePickerAdapter();
        picker.Listener = new BubblePickerListener(picker);
        ...
    }
    ```

    `BubblePickerAdapter` и `BubblePickerListener` — это два класса, создаваемые с нуля. Они обрабатывают данные пузырьков и управляют взаимодействием:

    ```csharp
    public class BubblePickerAdapter : Java.Lang.Object, IBubblePickerAdapter
    {
        private List<string> _bubbles = new List<string>();
        public int TotalCount => _bubbles.Count;
        public BubblePickerAdapter()
        {
            for (int i = 0; i < 10; i++)
            {
                _bubbles.Add($"Item {i}");
            }
        }

        public PickerItem GetItem(int itemIndex)
        {
            if (itemIndex < 0 || itemIndex >= _bubbles.Count)
                return null;

            var result = _bubbles[itemIndex];
            var item = new PickerItem(result);
            return item;
        }
    }

    public class BubblePickerListener : Java.Lang.Object, IBubblePickerListener
    {
        public View Picker { get; }
        public BubblePickerListener(View picker)
        {
            Picker = picker;
        }

        public void OnBubbleDeselected(PickerItem item)
        {
            Snackbar.Make(Picker, $"Deselected: {item.Title}", Snackbar.LengthLong)
                .SetAction("Action", (Android.Views.View.IOnClickListener)null)
                .Show();
        }

        public void OnBubbleSelected(PickerItem item)
        {
            Snackbar.Make(Picker, $"Selected: {item.Title}", Snackbar.LengthLong)
            .SetAction("Action", (Android.Views.View.IOnClickListener)null)
            .Show();
        }
    }
    ```

1. Запустите приложение, которое должно визуализировать пользовательский интерфейс Bubble Picker:

    ![Демонстрация в BubblePicker](walkthrough-images/bubble-picker-demo.png)

    Для примера требуется дополнительный код для визуализации стиля элементов и обработки взаимодействий, но элемент управления `BubblePicker` успешно создан и активирован.

Поздравляем! Вы успешно создали приложение Xamarin.Android и библиотеку привязок, которая использует библиотеку Kotlin.  

Теперь у вас должно быть базовое приложение Xamarin.Android, которое использует собственную библиотеку Kotlin через библиотеку привязки Xamarin.Android. В этом пошаговом руководстве намеренно используется простой пример для выделения основных понятий. В реальных сценариях, скорее всего, потребуется предоставить большее количество API и применить к ним преобразования метаданных.

## <a name="related-links"></a>Связанные ссылки

- [Android Studio](https://developer.android.com/studio)
- [Установка Gradle](https://gradle.org/install/)
- [Visual Studio для Mac](https://visualstudio.microsoft.com/downloads)
- [Декомпилятор Java](http://java-decompiler.github.io/)
- [Библиотека Kotlin BubblePicker](https://github.com/igalata/Bubble-Picker)
- [Привязка библиотеки Java](../binding-java-library/index.md)
- [XPath](https://www.w3.org/TR/xpath/)
- [Метаданные привязки Java](../binding-java-library/customizing-bindings/java-bindings-metadata.md)
- [Xamarin.Kotlin.StdLib NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)
- [Репозиторий примеров проектов](https://github.com/alexeystrakh/xamarin-binding-kotlin-framework)