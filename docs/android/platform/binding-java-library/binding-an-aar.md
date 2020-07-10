---
title: Привязка AAR-файла
description: Это пошаговое руководство содержит инструкции по созданию библиотеки привязок Java для Xamarin.Android из AAR-файла на платформе Android.
ms.prod: xamarin
ms.assetid: 380413B8-6A99-4BB8-B64C-3EAF9F359C22
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/11/2018
ms.openlocfilehash: 514157873412b3af31e43ba211aa636c9b53c112
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853112"
---
# <a name="binding-an-aar"></a>Привязка AAR-файла

> [!IMPORTANT]
> В настоящее время рассматривается возможность использования настраиваемых привязок на платформе Xamarin. Примите участие в [**этом опросе**](https://www.surveymonkey.com/r/KKBHNLT), чтобы помочь определить дальнейшие направления разработки.

_Это пошаговое руководство содержит инструкции по созданию библиотеки привязок Java для Xamarin.Android из AAR-файла на платформе Android._

## <a name="overview"></a>Обзор

Формат файлов *архива Android (.AAR)* используется для библиотек Android.
AAR-файл представляет собой ZIP-архив со следующим содержимым:

- скомпилированный код Java;
- идентификаторы ресурсов;
- Ресурсы
- метаданные (например, объявления действий, разрешения).

В этом пошаговом руководстве мы рассмотрим основные концепции создания библиотеки привязок из одного AAR-файла. Общие сведения (с простым примером кода) о создании библиотек привязок для Java см. в разделе [Привязка библиотеки Java](~/android/platform/binding-java-library/index.md).

> [!IMPORTANT]
> Проект привязки может включать только один AAR-файл. Если AAR-файл имеет зависимости от другого AAR-файла, такие зависимости должны содержаться в отдельном проекте привязки, на который будет указана ссылка. См. статью [об ошибке 44573](https://bugzilla.xamarin.com/show_bug.cgi?id=44573).

## <a name="walkthrough"></a>Пошаговое руководство

Мы создадим библиотеку привязок для примера файла с архивом Android [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true), который мы создали в Android Studio. Этот AAR-файл содержит класс `TextCounter` со статическими методами, которые подсчитывают число гласных и согласных в строке. Кроме того, **textanalyzer.aar** содержит ресурс изображения, чтобы наглядно отображать результаты подсчета.

Чтобы создать библиотеку привязок из AAR-файла, мы выполним следующую процедуру.

1. Создайте проект библиотеки привязок Java.

2. Добавьте в этот проект один AAR-файл. Проект привязки может содержать только один AAR-файл.

3. Задайте нужное действие сборки для этого AAR-файла.

4. Выберите целевую платформу, которую поддерживает AAR-файл.

5. Выполните сборку библиотеки привязок.

После создания библиотеки привязок мы разработаем небольшое приложение Android, которое предлагает пользователю ввести текстовую строку, вызывает из AAR-файла методы анализа текста, извлекает из AAR-файла изображение и отображает его вместе с результатами.

Пример приложения будет обращаться к классу `TextCounter` файла **textanalyzer.aar**:

```java
package com.xamarin.textcounter;

public class TextCounter
{
    ...
    public static int numVowels (String text) { ... };
    ...
    public static int numConsonants (String text) { ... };
    ...
}
```

Кроме того, этот пример приложения будет получать и отображать ресурс изображения, упакованный в файле **textanalyzer.aar**:

[![Изображение обезьяны Xamarin](binding-an-aar-images/00-monkey-sml.png)](binding-an-aar-images/00-monkey.png#lightbox)

Этот ресурс изображения сохранен по адресу **res/drawable/monkey.png** в файле **textanalyzer.aar**.

### <a name="creating-the-bindings-library"></a>Создание библиотеки привязок

Прежде чем переходить к следующим шагам, скачайте файл архива Android с примером [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true).

1. Создайте новый проект библиотеки привязок на основе шаблона библиотеки привязок Android. Вы можете использовать Visual Studio для Mac или Visual Studio (на снимках экрана ниже показан интерфейс Visual Studio, но Visual Studio для Mac мало чем отличается). Присвойте решению имя **AarBinding**.

    [![Создание проекта AarBindings](binding-an-aar-images/01-new-bindings-library-vs-sml.w160.png)](binding-an-aar-images/01-new-bindings-library-vs.w160.png#lightbox)

2. Шаблон включает папку **Jars**, в которую вы добавите AAR-файл для проекта библиотеки привязок. Щелкните папку **Jars** правой кнопкой мыши и выберите **Добавить > Существующий элемент**.

    [![Добавление существующего элемента](binding-an-aar-images/02-add-existing-item-vs-sml.png)](binding-an-aar-images/02-add-existing-item-vs.png#lightbox)

3. Перейдите к ранее скачанному файлу **textanalyzer.aar**, выберите его и щелкните **Добавить**:

    [![Добавление textanalyzer.aar](binding-an-aar-images/03-select-aar-file-vs-sml.png)](binding-an-aar-images/03-select-aar-file-vs.png#lightbox)

4. Убедитесь, что файл **textanalyzer.aar** успешно добавлен в проект:

    [![Файл textanalyzer.aar успешно добавлен](binding-an-aar-images/04-aar-added-vs-sml.png)](binding-an-aar-images/04-aar-added-vs.png#lightbox)

5. Задайте для **textanalyzer.aar** действие сборки `LibraryProjectZip`. В Visual Studio для Mac щелкните **textanalyzer.aar** правой кнопкой мыши, чтобы задать действие сборки. В Visual Studio действие сборки выбирается на панели **Свойства**.

    [![Настройка действия сборки LibraryProjectZip для файла textanalyzer.aar](binding-an-aar-images/05-embedded-aar-vs-sml.png)](binding-an-aar-images/05-embedded-aar-vs.png#lightbox)

6. В разделе "Свойства" для проекта задайте значение для параметра *Целевая платформа*. Если AAR-файл использует API-интерфейсы Android, для целевой платформы нужно указать тот уровень API который ожидает этот AAR-файл. (Дополнительную информацию о настройке целевой платформы и уровней API Android см. в [этой статье](~/android/app-fundamentals/android-api-levels.md).)

    Настройте целевой уровень API для библиотеки привязок. В этом примере мы можем свободно выбрать последнюю версию API для платформы (уровень API 23), так как наш файл **textanalyzer** не имеет зависимостей от интерфейсов API Android:

    [![Задание целевого уровня API 23](binding-an-aar-images/06-set-target-framework-vs-sml.png)](binding-an-aar-images/06-set-target-framework-vs.png#lightbox)

7. Выполните сборку библиотеки привязок. Проект библиотеки привязок должен успешно создать готовый DLL-файл по следующему адресу: **AarBinding/bin/Debug/AarBinding.dll**

### <a name="using-the-bindings-library"></a>Использование библиотеки привязок

Чтобы использовать этот DLL-файл в приложении Xamarin.Android, необходимо сначала добавить ссылку на библиотеку привязок. Выполните указанные ниже действия.

1. Чтобы упростить это пошаговое руководство, мы создаем приложение в том же решении, что и библиотеку привязок. (Приложение, использующее библиотеку привязок, может размещаться и в другом решении.) Создайте приложение Xamarin.Android, щелкнув "Решение" правой кнопкой мыши и выбрав пункт **Добавить новый проект**. Присвойте проекту имя **BindingTest**.

    [![Создание проекта BindingTest](binding-an-aar-images/07-add-new-project-vs-sml.w157.png)](binding-an-aar-images/07-add-new-project-vs.w157.png#lightbox)

2. Щелкните правой кнопкой мыши узел **Ссылки** для проекта **BindingTest** и выберите команду **Добавить ссылку...** .

    [![Щелкните "Добавить ссылку"](binding-an-aar-images/08-add-reference-vs-sml.png)](binding-an-aar-images/08-add-reference-vs.png#lightbox)

3. Выберите ранее созданный проект **AarBinding** и щелкните **ОК**.

    [![Проверка проекта привязки к AAR](binding-an-aar-images/09-choose-aar-binding-vs-sml.png)](binding-an-aar-images/09-choose-aar-binding-vs.png#lightbox)

4. Откройте узел **Ссылки** для проекта **BindingTest**, чтобы убедиться в наличии ссылки на **AarBinding**.

    [![AarBinding отображается в списке ссылок](binding-an-aar-images/10-references-shows-aarbinding-vs-sml.png)](binding-an-aar-images/10-references-shows-aarbinding-vs.png#lightbox)

Если вы хотите просмотреть содержимое проекта библиотеки привязки, можно дважды щелкнуть ссылку, чтобы открыть ее в **обозревателе объектов**. Здесь вы увидите содержимое пространства имен `Com.Xamarin.Textcounter`, сопоставленное с пакетом Java `com.xamarin.textanalyzezr`, и сможете просмотреть элементы класса `TextCounter`:

[![Просмотр окна обозревателя объектов](binding-an-aar-images/11-object-browser-vs-sml.png)](binding-an-aar-images/11-object-browser-vs.png#lightbox)

На приведенном выше снимке экрана выделены два метода `TextAnalyzer`, которые будут вызываться из примера приложения: `NumConsonants` (оболочка для базового метода Java `numConsonants`) и `NumVowels` (оболочка для базового метода Java `numVowels`).

### <a name="accessing-aar-types"></a>Доступ к типам из AAR-файла

Добавив в приложение ссылку, которая указывает на библиотеку привязки, вы сможете использовать типы Java из AAR-файла так же, как обычные типы C# (благодаря оболочкам C#). Код приложения C# может вызывать методы `TextAnalyzer`, как показано в следующем примере:

```csharp
using Com.Xamarin.Textcounter;
...
int numVowels = TextCounter.NumVowels (myText);
int numConsonants = TextCounter.NumConsonants (myText);
```

В приведенном выше примере мы вызываем статические методы из класса `TextCounter`. Но вы также можете создать экземпляры классов и вызвать методы экземпляра. Например, если AAR-файл является оболочкой для класса с именем `Employee`, в котором есть метод экземпляра `buildFullName`, вы можете создать экземпляр `MyClass` и использовать его, как показано ниже:

```csharp
var employee = new Com.MyCompany.MyProject.Employee();
var name = employee.BuildFullName ();
```

В следующих шагах описано, как добавить в приложение код, который запрашивает у пользователя текст, применяет `TextCounter` для анализа этого текста и выводит результаты.

Замените макет **BindingTest** (**Main.axml**) следующим XML-кодом. Этот макет содержит `EditText` для ввода текста и две кнопки для запуска подсчета гласных и согласных:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation             ="vertical"
    android:layout_width            ="fill_parent"
    android:layout_height           ="fill_parent" >
    <TextView
        android:text                ="Text to analyze:"
        android:textSize            ="24dp"
        android:layout_marginTop    ="30dp"
        android:layout_gravity      ="center"
        android:layout_width        ="wrap_content"
        android:layout_height       ="wrap_content" />
    <EditText
        android:id                  ="@+id/input"
        android:text                ="I can use my .AAR file from C#!"
        android:layout_marginTop    ="10dp"
        android:layout_gravity      ="center"
        android:layout_width        ="300dp"
        android:layout_height       ="wrap_content"/>
    <Button
        android:id                  ="@+id/vowels"
        android:layout_marginTop    ="30dp"
        android:layout_width        ="240dp"
        android:layout_height       ="wrap_content"
        android:layout_gravity      ="center"
        android:text                ="Count Vowels" />
    <Button
        android:id                  ="@+id/consonants"
        android:layout_width        ="240dp"
        android:layout_height       ="wrap_content"
        android:layout_gravity      ="center"
        android:text                ="Count Consonants" />
</LinearLayout>
```

Замените все содержимое файла **MainActivity.cs** приведенным ниже кодом. Как показано в этом примере, обработчики событий кнопки вызывают упакованные методы `TextCounter`, которые находятся в AAR-файле и используют всплывающие уведомления для вывода результатов. Обратите внимание на оператор `using` в пространстве имен привязанной библиотеки (в нашем примере это `Com.Xamarin.Textcounter`):

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using Android.Views.InputMethods;
using Com.Xamarin.Textcounter;

namespace BindingTest
{
    [Activity(Label = "BindingTest", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        InputMethodManager imm;

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            SetContentView(Resource.Layout.Main);

            imm = (InputMethodManager)GetSystemService(Context.InputMethodService);

            var vowelsBtn = FindViewById<Button>(Resource.Id.vowels);
            var consonBtn = FindViewById<Button>(Resource.Id.consonants);
            var edittext = FindViewById<EditText>(Resource.Id.input);
            edittext.InputType = Android.Text.InputTypes.TextVariationPassword;

            edittext.KeyPress += (sender, e) =>
            {
                imm.HideSoftInputFromWindow(edittext.WindowToken, HideSoftInputFlags.NotAlways);
                e.Handled = true;
            };

            vowelsBtn.Click += (sender, e) =>
            {
                int count = TextCounter.NumVowels(edittext.Text);
                string msg = count + " vowels found.";
                Toast.MakeText (this, msg, ToastLength.Short).Show ();
            };

            consonBtn.Click += (sender, e) =>
            {
                int count = TextCounter.NumConsonants(edittext.Text);
                string msg = count + " consonants found.";
                Toast.MakeText (this, msg, ToastLength.Short).Show ();
            };

        }
    }
}
```

Скомпилируйте и запустите проект **BindingTest**. Приложение запустится и отобразит страницу, как на снимке экрана слева (элемент `EditText` содержит некоторый текст, но вы можете коснуться его, чтобы изменить). Если нажать кнопку **COUNT VOWELS**, появится всплывающее уведомление с количеством гласных, как показано справа:

[![Снимки экрана выполняемого приложения BindingTest](binding-an-aar-images/12-count-vowels.png)](binding-an-aar-images/12-count-vowels.png#lightbox)

Попробуйте нажать кнопку **COUNT CONSONANTS**. Кроме того, вы можете изменить строку текста и снова нажать любую из этих кнопок, чтобы получить новое значение количества гласных и согласных.

### <a name="accessing-aar-resources"></a>Доступ к ресурсам из AAR-файла

Средства Xamarin включают данные **R** из AAR-файла в класс **Resourse** вашего приложения. Это позволяет обращаться к ресурсам AAR-файла из макета (или кода программной части) так же, как к любым ресурсам в пути **Resources** этого проекта.

Для доступа к ресурсу изображения укажите имя **Resource.Drawable**, по которому это изображение размещается в упакованном AAR-файле. Например, к файлу **image.png** в AAR-файле можно обратиться по адресу `@drawable/image`:

```xml
<ImageView android:src="@drawable/image" ... />
```

Вы также можете использовать макеты ресурсов, размещенные в AAR-файле. Для этого укажите имя **Resource.Layout** для макета, упакованного в AAR-файле. Пример:

```csharp
var a = new ArrayAdapter<string>(this, Resource.Layout.row_layout, ...);
```

Пример **textanalyzer.aar** содержит ресурс изображения, который сохранен по адресу **res/drawable/monkey.png**. Давайте обратимся к этому ресурсу изображения и используем его в нашем примере приложения:

Измените макет **BindingTest** (**Main.axml**) и добавьте `ImageView` в конец контейнера `LinearLayout`. Этот `ImageView` отображает изображение из расположения **\@drawable/monkey**, которое будет загружено из раздела ресурсов в **textanalyzer.aar**:

```xml
    ...
    <ImageView
        android:src                 ="@drawable/monkey"
        android:layout_marginTop    ="40dp"
        android:layout_width        ="200dp"
        android:layout_height       ="200dp"
        android:layout_gravity      ="center" />

</LinearLayout>
```

Скомпилируйте и запустите проект **BindingTest**. Приложение запустится и отобразит страницу, как на снимке экрана слева. При нажатии кнопки **COUNT CONSONANTS** результаты будут представлены справа, вот так:

[![BindingTest отображает число согласных](binding-an-aar-images/13-count-consonants.png)](binding-an-aar-images/13-count-consonants.png#lightbox)

Поздравляем! Итак, вы успешно создали библиотеку привязки Java на основе AAR-файла.

## <a name="summary"></a>Сводка

В этом пошаговом руководстве мы создали библиотеку привязок для AAR-файла, добавили эту библиотеку привязок в простейшее тестовое приложение и запустили это приложение для проверки того, как наш код C# вызывает код Java из AAR-файла.
Кроме того, мы дополнили это приложение кодом доступа к ресурсу изображения, который размещен в AAR-файле.

## <a name="related-links"></a>Связанные ссылки

- [Создание библиотеки привязок Java (видео)](https://university.xamarin.com/classes#10090)
- [Привязка JAR-файла](~/android/platform/binding-java-library/binding-a-jar.md)
- [Привязка библиотеки Java](~/android/platform/binding-java-library/index.md)
- [AarBinding (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/javaintegration-aarbinding)
- [Ошибка 44573 — один проект не может привязать несколько AAR-файлов](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)
