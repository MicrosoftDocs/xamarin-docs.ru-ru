---
title: Обзор поддержки асинхронного выполнения
description: В этом документе описывается программирование с использованием Async и await, основные понятия, представленные в C# 5, облегчающие написание асинхронного кода.
ms.prod: xamarin
ms.assetid: F87BF587-AB64-4C60-84B1-184CAE36ED65
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 9821519c680de61767792c8122938bf826e457f5
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91453250"
---
# <a name="async-support-overview"></a>Обзор поддержки асинхронного выполнения

_В C# 5 появились два ключевых слова для упрощения асинхронной программы: async и await. Эти ключевые слова позволяют написать простой код, использующий библиотеку параллельных задач для выполнения длительных операций (таких как сетевой доступ) в другом потоке и простого доступа к результатам по завершении. Последние версии Xamarin. iOS и Xamarin. Android поддерживают Async и await — этот документ содержит объяснения и пример использования нового синтаксиса с Xamarin._

Поддержка асинхронной работы Xamarin основана на моно 3,0 Foundation и обновляет профиль API с помощью совместимой с мобильными версиями Silverlight, которая является удобной для мобильных устройств версией .NET 4,5.

## <a name="overview"></a>Обзор

В этом документе представлены новые ключевые слова Async и await, а затем приведены некоторые простые примеры реализации асинхронных методов в Xamarin. iOS и Xamarin. Android.

Более подробное обсуждение новых асинхронных функций C# 5 (включая множество примеров и различных сценариев использования) см. в статье [Асинхронное программирование](/dotnet/csharp/async).

Пример приложения выполняет простой асинхронный веб-запрос (без блокировки основного потока), а затем обновляет пользовательский интерфейс с загруженным кодом HTML и числом символов.

 [![Пример приложения выполняет простой асинхронный веб-запрос без блокировки основного потока, а затем обновляет пользовательский интерфейс с загруженным кодом HTML и числом символов.](async-images/AsyncAwait_427x368.png)](async-images/AsyncAwait.png#lightbox)

Поддержка асинхронной работы Xamarin основана на моно 3,0 Foundation и обновляет профиль API с помощью совместимой с мобильными версиями Silverlight, которая является удобной для мобильных устройств версией .NET 4,5.

## <a name="requirements"></a>Требования

Для компонентов C# 5 требуется моно 3,0, включенный в Xamarin. iOS 6,4 и Xamarin. Android 4,8. Вам будет предложено обновить Mono, Xamarin. iOS, Xamarin. Android и Xamarin. Mac, чтобы воспользоваться его преимуществами.

## <a name="using-async-amp-await"></a>Использование Async &amp; await

 `async` и `await` являются новыми функциями языка C#, которые работают совместно с библиотекой параллельных задач, чтобы упростить написание потокового кода для выполнения длительных задач без блокировки основного потока приложения.

## <a name="async"></a>async

### <a name="declaration"></a>Объявление

`async`Ключевое слово помещается в объявление метода (или в лямбда-или анонимном методе), чтобы указать, что он содержит код, который может выполняться асинхронно, IE. не блокируйте поток вызывающего объекта.

Метод, помеченный как, `async` должен содержать по крайней мере одно выражение или оператор await. Если `await` в методе отсутствуют операторы, они будут выполняться синхронно (то же, что и при отсутствии `async` модификатора). Это также приведет к появлению предупреждения компилятора (но не ошибки).

### <a name="return-types"></a>Типы возвращаемых данных

Асинхронный метод должен возвращать `Task` , `Task<TResult>` или `void` .

Укажите `Task` тип возвращаемого значения, если метод не возвращает никаких других значений.

Укажите `Task<TResult>` , должен ли метод возвращать значение, где `TResult` — возвращаемый тип (например `int` ,).

`void`Тип возвращаемого значения используется главным образом для обработчиков событий, которым он необходим. Код, вызывающий асинхронные методы, возвращающие значение void, не может быть `await` получен в результате.

### <a name="parameters"></a>Параметры

Асинхронные методы не могут объявлять `ref` `out` Параметры или.

## <a name="await"></a>await

Оператор await можно применить к задаче внутри метода, помеченного как async. Это приводит к тому, что метод останавливает выполнение в этой точке и ждет завершения задачи.

Использование await не блокирует поток вызывающего объекта, а элемент управления возвращается вызывающему. Это означает, что вызывающий поток не блокируется, поэтому, например, поток пользовательского интерфейса не будет заблокирован при ожидании задачи.

По завершении задачи метод возобновляет выполнение в той же точке кода. Сюда входит возврат к области try блока try-catch-finally (если таковой имеется). await нельзя использовать в блоке catch или finally.

Дополнительные сведения о [ожидании](/dotnet/csharp/language-reference/keywords/await) в документация Майкрософт.

## <a name="exception-handling"></a>Обработка исключений

Исключения, возникающие в асинхронном методе, сохраняются в задаче и вызываются, когда задача является `await` ED. Эти исключения могут быть перехвачены и обработаны внутри блока try-catch.

## <a name="cancellation"></a>Отмена

Асинхронные методы, выполнение которых занимает много времени, должны поддерживать отмену. Как правило, отмена вызывается следующим образом:

- `CancellationTokenSource`Создается объект.
- `CancellationTokenSource.Token`Экземпляр передается асинхронному методу, допускающему отмену.
- Отмена запрашивается путем вызова `CancellationTokenSource.Cancel` метода.

Затем задача отменяет саму себя и подтверждает отмену.

Дополнительные сведения об отмене см. в разделе [Настройка асинхронного приложения (C#)](/dotnet/csharp/programming-guide/concepts/async/fine-tuning-your-async-application).

## <a name="example"></a>Пример

Скачайте [пример решения Xamarin](/samples/xamarin/mobile-samples/asyncawait/) (для iOS и Android), чтобы просмотреть рабочий пример `async` и `await` в мобильных приложениях. Пример кода более подробно рассматривается в этом разделе.

### <a name="writing-an-async-method"></a>Написание асинхронного метода

Следующий метод демонстрирует создание кода `async` метода с помощью `await` задачи Ed:

```csharp
public async Task<int> DownloadHomepage()
{
    var httpClient = new HttpClient(); // Xamarin supports HttpClient!

    Task<string> contentsTask = httpClient.GetStringAsync("https://visualstudio.microsoft.com/xamarin"); // async method!

    // await! control returns to the caller and the task continues to run on another thread
    string contents = await contentsTask;

    ResultEditText.Text += "DownloadHomepage method continues after async call. . . . .\n";

    // After contentTask completes, you can calculate the length of the string.
    int exampleInt = contents.Length;

    ResultEditText.Text += "Downloaded the html and found out the length.\n\n\n";

    ResultEditText.Text += contents; // just dump the entire HTML

    return exampleInt; // Task<TResult> returns an object of type TResult, in this case int
}
```

Обратите внимание на следующие моменты:

- Объявление метода включает  `async` ключевое слово.
- Тип возвращаемого значения заключается в том,  `Task<int>` что вызывающий код может получить доступ к  `int` значению, вычисленному в этом методе.
- Оператор Return представляет собой  `return exampleInt;` целочисленный объект — тот факт, что возвращаемый метод  `Task<int>` является частью улучшений языка.

### <a name="calling-an-async-method-1"></a>Вызов асинхронного метода 1

Этот обработчик событий нажатия кнопки можно найти в примере приложения Android для вызова описанного выше метода:

```csharp
GetButton.Click += async (sender, e) => {

    Task<int> sizeTask = DownloadHomepage();

    ResultTextView.Text = "loading...";
    ResultEditText.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await sizeTask;

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultTextView.Text = "Length: " + intResult ;
    // "returns" void, since it's an event handler
};
```

Примечания.

- Анонимный делегат имеет префикс ключевого слова Async.
- Асинхронный метод Довнлоадхомепаже возвращает задачу \<int> , которая хранится в переменной сизетаск.
- Код ожидает переменную Сизетаск.  *Это* расположение, в котором метод приостанавливается, и управление возвращается вызывающему коду до тех пор, пока асинхронная задача не завершится в собственном потоке.
- Выполнение  *не* приостанавливается при создании задачи в первой строке метода, несмотря на то, что в ней создается задача. Ключевое слово await обозначает место приостановки выполнения.
- По завершении асинхронной задачи устанавливается параметр tResult, и выполнение остается в исходном потоке из строки await.

### <a name="calling-an-async-method-2"></a>Вызов асинхронного метода 2

В примере приложения iOS пример написан несколько иначе, чтобы продемонстрировать альтернативный подход. Вместо использования анонимного делегата в этом примере объявляется `async` обработчик событий, который назначается как обычный обработчик событий:

```csharp
GetButton.TouchUpInside += HandleTouchUpInside;
```

Затем метод обработчика событий определяется, как показано ниже:

```csharp
async void HandleTouchUpInside (object sender, EventArgs e)
{
    ResultLabel.Text = "loading...";
    ResultTextView.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await DownloadHomepage();

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultLabel.Text = "Length: " + intResult ;
}
```

Некоторые важные моменты:

- Метод помечается как,  `async` но возвращает  `void` . Обычно это делается только для обработчиков событий (в противном случае возвращается  `Task` или  `Task<TResult>` ).
- `await`Ключевое слово в `DownloadHomepage` методе непосредственно присваивает переменной () в `intResult` отличие от предыдущего примера, где мы использовали промежуточную `Task<int>` переменную для ссылки на задачу.  *Это* расположение, где управление возвращается вызывающему объекту до тех пор, пока асинхронный метод не завершит работу в другом потоке.
- Когда асинхронный метод завершает работу и возвращает значение, выполнение возобновляется с,  `await` что означает возврат целочисленного результата и последующее его отображение в мини-приложении пользовательского интерфейса.

## <a name="summary"></a>Сводка

Использование Async и await значительно упрощает код, необходимый для инициирования длительных операций в фоновых потоках, не блокируя основной поток. Они также упрощают доступ к результатам после завершения задачи.

В этом документе представлен обзор новых ключевых слов языка и примеров для Xamarin. iOS и Xamarin. Android.

## <a name="related-links"></a>Связанные ссылки

- [Асинкаваит (пример)](/samples/xamarin/mobile-samples/asyncawait/)
- [Обратные вызовы в качестве оператора Go в поколениях](https://tirania.org/blog/archive/2013/Aug-15.html)
- [Данные (iOS) (пример)](/samples/xamarin/ios-samples/data/)
- [HttpClient (iOS) (пример)](/samples/xamarin/ios-samples/httpclient/)
- [Мапкитсеарч (iOS) (пример)](https://github.com/xamarin/monotouch-samples/tree/master/MapKitSearch)
- [Асинхронное программирование](/dotnet/csharp/async)
- [Fine-Tuning Your Async Application (C#)](/dotnet/csharp/programming-guide/concepts/async/fine-tuning-your-async-application) (Тонкая настройка асинхронного приложения в C#)
- [Ожидание, Пользовательский интерфейс и взаимоблокировки! Вот это да!](https://devblogs.microsoft.com/pfxteam/await-and-ui-and-deadlocks-oh-my/)
- [Обработка задач по мере их завершения)](https://devblogs.microsoft.com/pfxteam/processing-tasks-as-they-complete/)
- [Task-based Asynchronous Pattern (TAP)](/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap) (Асинхронный шаблон, основанный на задачах (TAP))
- [Асинхронность в C# 5 (блог «Липперта») — о вводе ключевых слов](/archive/blogs/ericlippert/asynchrony-in-c-5-part-six-whither-async)