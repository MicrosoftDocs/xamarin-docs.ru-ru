---
title: Устранение неполадок с привязками
description: В этой статье перечислены типичные ошибки сервера, которые могут возникать при создании привязок, возможные причины этих ошибок и способы их устранения.
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 3ec01d9322d4b6706fccd0959ae5944f71e82664
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853024"
---
# <a name="troubleshooting-bindings"></a>Устранение неполадок с привязками

> [!IMPORTANT]
> В настоящее время рассматривается возможность использования настраиваемых привязок на платформе Xamarin. Примите участие в [**этом опросе**](https://www.surveymonkey.com/r/KKBHNLT), чтобы помочь определить дальнейшие направления разработки.

_В этой статье перечислены типичные ошибки сервера, которые могут возникать при создании привязок, возможные причины этих ошибок и способы их устранения._

## <a name="overview"></a>Обзор

Привязка библиотеки Android (файл **AAR** или **JAR**) редко проходит просто. Обычно требуются дополнительные усилия для устранения проблем, возникающих из-за различий между Java и .NET.
Эти проблемы не позволяют Xamarin.Android привязать библиотеку Android и проявляются как сообщения об ошибках в журнале сборки. В этом руководстве представлены некоторые советы по устранению неполадок, перечислены самые типичные проблемы и сценарии, а также предложены возможные решения для успешной привязки библиотеки Android.

При привязке существующей библиотеки Android необходимо учитывать следующие моменты:

- **Внешние зависимости для библиотеки** &ndash; все необходимые библиотеке Android зависимости Java должны быть включены в проект Xamarin.Android в виде **ReferenceJar** или **EmbeddedReferenceJar**.

- **Уровень API в Android, который предстоит использовать библиотеке Android** &ndash; возможности "понизить" уровень API в Android не существует, поэтому сразу проследите за тем, чтобы проект Xamarin.Android использовал тот же (или более высокий) уровень API, что и подключаемая библиотека Android.

- **Версия пакета JDK для Android, которая использовалась при упаковке библиотеки Android** &ndash; ошибки привязки могут быть связаны с тем, что библиотека Android компилировалась не с той версией JDK, которую использует Xamarin.Android. Если есть возможность, перекомпилируйте библиотеку Android, используя ту же версию пакета JDK, которая используется в вашей установке Xamarin.Android.

Первым этапом в устранении неполадок с привязкой библиотеки Xamarin.Android должно быть включение [диагностического вывода MSBuild](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).
Включив диагностический вывод, перекомпилируйте проект Xamarin.Android и проверьте журнал сборки на предмет ошибок, позволяющих предположить причину проблем.

Также можно декомпилировать библиотеку Android и изучить типы и методы, к которым пытается создать привязку Xamarin.Android. Более подробно этот процесс рассматривается далее в этом руководстве.

## <a name="decompiling-an-android-library"></a>Декомпиляция библиотеки Android

Изучение классов и методов Java может дать ценную информацию, которая поможет привязать библиотеку.
Служебная программа с графическим интерфейсом [JD-GUI](http://jd.benow.ca/) умеет отображать исходный код Java из файлов **CLASS**, включенных в пакет JAR. Ее можно использовать в виде самостоятельного приложения или как подключаемый модуль для IntelliJ или Eclipse.

Чтобы декомпилировать библиотеку Android, откройте файл **JAR** в декомпиляторе Java. Если библиотека оформлена как файл **AAR**, вам придется извлечь из архива файл **classes.jar**. На снимке экрана ниже демонстрируется применение JD-GUI для анализа JAR-файла [Picasso](https://square.github.io/picasso/).

![Использование декомпилятора Java для анализа файла picasso-2.5.2.jar](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

Декомпилировав файл библиотеки Android, изучите ее исходный код. Особое внимание следует обращать на следующие признаки:

- **Классы с признаками обфускации** &ndash; признаками обфускации могут быть, среди прочего, следующие:

  - имя класса содержит символ **$** , например **a$.class**;
  - имя класса написано полностью строчными буквами, например **a.class**.      

- **Инструкции `import` для библиотек, на которые нет ссылок** &ndash; определите, на какие библиотеки нет ссылок, и добавьте эти зависимости в проект Xamarin.Android с помощью **Build Action** из **ReferenceJar** или **EmbedddedReferenceJar**.

> [!NOTE]
> Декомпиляция библиотеки Java может быть запрещена или юридически ограничена в соответствии с местным законодательством или условиями лицензии, по которой эта библиотека была опубликована. Если потребуется, обратитесь за консультацией к профессиональным юристам перед декомпиляцией библиотеки Java и изучением ее исходного кода.

## <a name="inspect-apixml"></a>Изучение API.XML

В ходе привязки в исходного проекта Xamarin.Android создает XML-файл с именем **obj/Debug/api.xml**.

![Автоматически созданный файл api.xml в папке obj/Debug](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

Этот файл содержит список всех API-интерфейсов Java, которые пытается привязать Xamarin.Android. Содержимое этого файла может помочь найти недостающие типы или методы, а также повторные привязки. Хотя изучение этого файла может быть сложным и долгим, оно может дать полезные подсказки о причинах проблем с привязкой. Например, из **api.xml** можно узнать, что свойство возвращает недопустимый тип или существуют два типа с одним управляемым именем.

## <a name="known-issues"></a>Известные проблемы

Этот раздел содержит список самых распространенных сообщений об ошибках и симптомов, которые могут проявляться при попытке привязать библиотеку Android.

### <a name="problem-java-version-mismatch"></a>Проблема. Несоответствие версий Java

Иногда могут не создаваться типы или возникать непредвиденные сбои. Причиной может быть то, что вы используете более новую или более раннюю версию Java по сравнению с той, которая использовалась для компиляции библиотеки. Перекомпилируйте библиотеку Android, используя ту же версию пакета JDK, которую использует ваш проект Xamarin.Android.

### <a name="problem-at-least-one-java-library-is-required"></a>Проблема. Требуется по крайней мере одна библиотека Java

Вы получаете ошибку о том, что требуется по крайней мере одна библиотека Java, даже если JAR-файл уже добавлен.

#### <a name="possible-causes"></a>Возможные причины

Убедитесь, что указано действие сборки `EmbeddedJar`. Так как для JAR-файлов существует несколько действий сборки (например, `InputJar`, `EmbeddedJar`, `ReferenceJar` или `EmbeddedReferenceJar`), генератор привязок не всегда может правильно выбрать, какую из них нужно использовать. Дополнительные сведения о действиях сборки см. в [этой статье](~/android/platform/binding-java-library/index.md).

### <a name="problem-binding-tools-cannot-load-the-jar-library"></a>Проблема. Средства привязки не могут загрузить JAR-файл библиотеки

Генератору библиотек привязки не удается загрузить JAR-файл библиотеки.

#### <a name="possible-causes"></a>Возможные причины

Некоторые библиотеки в формате JAR-файлов, в которых используется обфускация кода (с помощью Proguard или аналогичных средств), не могут быть загружены средствами Java. Так как в нашем средстве используется отражение Java и библиотека инженерного кода ASM, эти зависимые средства могут отклонять библиотеки с обфускацией кода, даже если те признаются средствами среды выполнения Android. В качестве возможного решения можно вручную привязать эти библиотеки, не используя генератор привязок.

### <a name="problem-missing-c-types-in-generated-output"></a>Проблема. В создаваемых выходных данных отсутствуют типы C#

Файл **DLL** привязки создается, но в нем отсутствуют некоторые типы Java, или созданный исходный код C# не компилируется из-за ошибки с сообщением об отсутствии типов.

#### <a name="possible-causes"></a>Возможные причины

Эта ошибка может быть связана с любой из перечисленных ниже причин.

- Привязываемая библиотека может ссылаться на другую библиотеку Java. Если открытый API для привязываемой библиотеки использует типы из второй библиотеки, вам необходимо создать ссылку на управляемую привязку и для второй библиотеки.

- Возможно, что библиотека была внедрена отражением Java, как и в описанной выше проблеме с загрузкой библиотеки, что привело к непредвиденной загрузке метаданных. Сейчас решить эту проблему средствами Xamarin.Android невозможно. В этом случае библиотеку нужно привязать вручную.

- В среде выполнения .NET 4.0 существовала ошибка, из-за которой она иногда не могла загружать сборки без очевидных причин. Эта проблема устранена в среде выполнения .NET 4.5.

- Java позволяет наследовать открытый класс от закрытого класса, но в .NET это не поддерживается. Поскольку генератор привязки не создает привязки для закрытых классов, производные классы не могут быть сформированы правильно. Чтобы устранить эту проблему, удалите запись метаданных для производных классов из **Metadata.xml** с помощью remove-node или исправьте метаданные так, чтобы закрытый класс стал открытым. Несмотря на то, что последнее решение правильно создаст привязку и компиляция исходного кода C# пройдет успешно, не следует использовать закрытый класс.

  Пример:

  ```xml
  <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
      name="visibility">public</attr>
  ```

- Средства обфускации кода в библиотеках Java могут конфликтовать с генератором привязок Xamarin.Android и помешать ему создать классы-оболочки C#. В следующем фрагменте кода показано, как изменить **Metadata.xml**, чтобы убрать обфускацию имени класса.

  ```xml
  <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
      name="obfuscated">false</attr>
  ```

### <a name="problem-generated-c-source-does-not-build-due-to-parameter-type-mismatch"></a>Проблема. Созданный исходный код C# не компилируется из-за несоответствия типов параметров

Созданный исходный код C# не компилируется. Типы параметров переопределенного метода не совпадают.

#### <a name="possible-causes"></a>Возможные причины

В Xamarin.Android есть несколько полей Java, которые сопоставляются с перечислениями в привязках C#. Это может приводить к несовместимости типов в созданных привязках. Чтобы устранить такую проблему, нужно исправить сигнатуры методов, созданные генератором привязок, и перейти на использование перечислений. Дополнительные сведения см. в статье [об исправлении перечислений](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md).

### <a name="problem-noclassdeffounderror-in-packaging"></a>Проблема. Ошибка NoClassDefFoundError при упаковке

На этапе упаковки возникает исключение `java.lang.NoClassDefFoundError`.

#### <a name="possible-causes"></a>Возможные причины

Наиболее вероятной причиной такой ошибки является необходимость добавить обязательную библиотеку Java в проект приложения ( **.csproj**). JAR-файлы не разрешаются автоматически. Привязки библиотеки Java не всегда успешно создаются для пользовательских сборок, которые не существуют на целевом устройстве или в эмуляторе (например, проблема с **maps.jar** для Карт Google). Такого не происходит при поддержке проекта библиотеки Android, так как JAR-файл библиотеки всегда включается в DLL-файл библиотеки. Пример: [ошибка 4288](https://bugzilla.xamarin.com/show_bug.cgi?id=4288).

### <a name="problem-duplicate-custom-eventargs-types"></a>Проблема. Дублирование пользовательских типов EventArgs

Сборка завершается ошибкой из-за дублирования пользовательских типов EventArgs. Ошибка будет выглядеть примерно так:

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### <a name="possible-causes"></a>Возможные причины

Это может быть связано с конфликтом между типами событий из-за нескольких типов прослушивателя интерфейса, которые используют методы с одинаковыми именами. Например, для двух представленных в примере ниже интерфейсов Java генератор создаст `DismissScreenEventArgs` как для `MediationBannerListener`, так и для `MediationInterstitialListener`, что приведет к ошибке.

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

Это ожидаемое поведение, которое позволяет избежать слишком длинных имен для типов аргументов событий. Чтобы избежать таких конфликтов, потребуется преобразовать метаданные. Откройте файл **Transforms\Metadata.xml** и добавьте атрибут `argsType` для любого из интерфейсов (или в метод интерфейса).

```xml
<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationBannerListener']/method[@name='onDismissScreen']"
        name="argsType">BannerDismissScreenEventArgs</attr>

<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationInterstitialListener']/method[@name='onDismissScreen']"
        name="argsType">IntersitionalDismissScreenEventArgs</attr>

<attr path="/api/package[@name='android.content']/
        interface[@name='DialogInterface.OnClickListener']"
        name="argsType">DialogClickEventArgs</attr>
```

### <a name="problem-class-does-not-implement-interface-method"></a>Проблема. Класс не реализует метод интерфейса

Появляется ошибка с сообщением о том, что созданный класс не реализует метод, который является обязательным для реализуемого этим классом интерфейса. Однако, просмотрев созданный код, вы обнаруживаете нужный метод.

Вот пример такой ошибки:

```shell
obj\Debug\generated\src\Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.cs(8,23):
error CS0738: 'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter' does not
implement interface member 'Oauth.Signpost.Http.IHttpRequest.Unwrap()'.
'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.Unwrap()' cannot implement
'Oauth.Signpost.Http.IHttpRequest.Unwrap()' because it does not have the matching
return type of 'Java.Lang.Object'
```

#### <a name="possible-causes"></a>Возможные причины

Такая проблема возникает при привязке методов Java с ковариантными возвращаемыми типами. В нашем примере метод `Oauth.Signpost.Http.IHttpRequest.UnWrap()` должен возвращать `Java.Lang.Object`. Но метод `Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()` имеет тип возвращаемого значения `HttpURLConnection`. Устранить эту проблему можно двумя способами:

- Объявите разделяемый класс для `HttpURLConnectionRequestAdapter` и явным образом реализуйте `IHttpRequest.Unwrap()`.

  ```csharp
  namespace Oauth.Signpost.Basic {
      partial class HttpURLConnectionRequestAdapter {
          Java.Lang.Object OauthSignpost.Http.IHttpRequest.Unwrap() {
              return Unwrap();
          }
      }
  }
  ```

- Удалите ковариантность из созданного кода C#. Для этого нужно добавить следующее преобразование в файл **Transforms\Metadata.xml**, чтобы созданный код C# мог иметь тип возвращаемого значения `Java.Lang.Object`.

  ```xml
  <attr
      path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
      name="managedReturn">Java.Lang.Object
  </attr>
  ```

### <a name="problem-name-collisions-on-inner-classes--properties"></a>Проблема. Конфликты имен во внутренних классах или свойствах

Конфликт видимости для наследуемых объектов.

В Java производный класс не обязан иметь ту же область видимости, что и родительский класс. Java автоматически исправляет это. Но в C# такие объявления должны быть явными, поэтому вам нужно обеспечить правильную область видимости для всех классов в иерархии. В следующем примере показано, как изменить имя пакета Java с `com.evernote.android.job` на `Evernote.AndroidJob`.

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### <a name="problem-a-so-library-required-by-the-binding-is-not-loading"></a>Проблема. Нужная для привязки библиотека **SO** не загружается

Некоторые проекты с привязкой могут зависеть от библиотеки **SO**. Может случиться так, что Xamarin.Android не сможет автоматически загрузить библиотеку **SO**. При выполнении кода Java из оболочки Xamarin.Android не сможет вызвать JNI и выдаст ошибку _java.lang.UnsatisfiedLinkError: Native method not found:_ (встроенный метод не обнаружен) в журнале LogCat для приложения.

Чтобы исправить это, вручную загрузите нужную библиотеку **SO** с помощью вызова `Java.Lang.JavaSystem.LoadLibrary`. Предположим для примера, что проект Xamarin.Android использует общую библиотеку **libpocketsphinx_jni.so**, включенную в проект с помощью действия привязки **EmbeddedNativeLibrary**. В этом случае следующий фрагмент кода (который нужно добавить перед использованием общей библиотеки) загрузит нужную библиотеку **SO**.

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="summary"></a>Сводка

В этой статье мы описали симптомы и методы устранения типичных неполадок, которые могут возникать с привязками Java.

## <a name="related-links"></a>Связанные ссылки

- [Проекты библиотеки](https://developer.android.com/tools/projects/index.html#LibraryProjects)
- [Работа с JNI](~/android/platform/java-integration/working-with-jni.md)
- [Включение выходных данных диагностики](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [Xamarin для разработчиков Android](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
