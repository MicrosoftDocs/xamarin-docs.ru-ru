---
title: Сводка по главе 20. Асинхронные и файловые операции ввода-вывода
description: Создание мобильных приложений с помощью Xamarin.Forms. Сводка по главе 20. Асинхронные и файловые операции ввода-вывода
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D595862D-64FD-4C0D-B0AD-C1F440564247
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 75c79c7a5300cf5708bb46740bec11f84b59c786
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374027"
---
# <a name="summary-of-chapter-20-async-and-file-io"></a>Сводка по главе 20. Асинхронные и файловые операции ввода-вывода

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)

> [!NOTE]
> Эта книга была опубликована весной 2016 года и с тех пор не обновлялась. Многое в этой книге остается ценным, но некоторые материалы устарели, а некоторые разделы перестали быть полностью верными или полными.

Графический пользовательский интерфейс должен последовательно реагировать на события пользовательского ввода. Это означает, что вся обработка пользовательских событий должна выполняться в одном потоке, который часто называют *основным потоком* или *потоком пользовательского интерфейса*.

Пользователи ожидают, что графический интерфейс будет реагировать без задержек. Это означает, что программа должна быстро обрабатывать события пользовательского ввода. Если это невозможно, обработку следует передать на выполнение во вторичные потоки.

В нескольких примерах программ в этой книге уже использовался класс [`WebRequest`](xref:System.Net.WebRequest). В этом классе метод [`BeginGetResponse`](xref:System.Net.WebRequest.BeginGetResponse(System.AsyncCallback,System.Object)) запускает рабочий поток, который после завершения вызывает функцию обратного вызова. Но эта функция обратного вызова выполняется в рабочем потоке, а значит программа должна вызвать метод [`Device.BeginInvokeOnMainThread`](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) для доступа к пользовательскому интерфейсу.

> [!NOTE]
> Программы Xamarin.Forms для обращения к файлам через Интернет должны использовать [`HttpClient`](xref:System.Net.Http.HttpClient), а не [`WebRequest`](xref:System.Net.WebRequest). `HttpClient` поддерживает асинхронные операции.

Более современный подход к асинхронной обработке доступен в .NET и C#. Сюда относятся классы [`Task`](xref:System.Threading.Tasks.Task) и [`Task<TResult>`](xref:System.Threading.Tasks.Task`1), а также другие типы в пространствах имен [`System.Threading`](xref:System.Threading) и [`System.Threading.Tasks`](xref:System.Threading.Tasks) и ключевые слова `async` и `await` из C# 5.0. Именно их мы рассматриваем в этой главе.

## <a name="from-callbacks-to-await"></a>От обратных вызовов до ожидания

Сам класс `Page` содержит три асинхронных метода для отображения окон с предупреждениями:

- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) возвращает объект `Task`;
- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String,System.String)) возвращает объект `Task<bool>`.
- [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])) возвращает объект `Task<string>`.

Объекты `Task` указывают, что эти методы реализуют асинхронную модель на основе задач (TAP). Эти объекты `Task` быстро возвращаются методом. Возвращаемые значения `Task<T>` составляют так называемое обещание того, что после завершения задачи будет доступно значение типа `TResult`. Возвращаемое значение `Task` указывает на асинхронное действие, которое будет выполнено, но не возвращает значения.

Во всех этих случаях `Task` завершается, когда пользователь закрывает окно предупреждения.  

### <a name="an-alert-with-callbacks"></a>Предупреждение с обратными вызовами

В примере [**AlertCallbacks**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertCallbacks) показано, как управлять возвращаемыми объектами `Task<bool>` и вызовами `Device.BeginInvokeOnMainThread` с помощью методов обратного вызова.

### <a name="an-alert-with-lambdas"></a>Объявление с лямбда-выражениями

В примере [**AlertLambdas**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertLambdas) показано, как использовать анонимные лямбда-функции для обработки вызовов `Task` и `Device.BeginInvokeOnMainThread`.  

### <a name="an-alert-with-await"></a>Объявление с ожиданием

В рамках более прямого подхода используются ключевые слова `async` и `await`, которые появились в C# 5. Их использование демонстрируется в примере [**AlertAwait**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertAwait).

### <a name="an-alert-with-nothing"></a>Оповещение без дополнительных элементов

Если асинхронный метод возвращает `Task`, а не `Task<TResult>`, программе не нужно использовать любые дополнительные методы, если не требуется знать время завершения асинхронной задачи. Этот подход демонстрируется в примере [**NothingAlert**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/NothingAlert).

### <a name="saving-program-settings-asynchronously"></a>Асинхронное сохранение параметров программы

Пример [**SaveProgramChanges**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/SaveProgramSettings) демонстрирует применение метода [`SavePropertiesAsync`](xref:Xamarin.Forms.Application.SavePropertiesAsync) из `Application` для сохранения параметров программы при их изменении без переопределения метода `OnSleep`.

### <a name="a-platform-independent-timer"></a>Таймер, независимый от платформы

С помощью [`Task.Delay`](xref:System.Threading.Tasks.Task.Delay(System.Int32)) вы можете создать независимый от платформы таймер. Этот подход демонстрируется в примере [**TaskDelayClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TaskDelayClock).

## <a name="file-inputoutput"></a>Файловый ввод-вывод

Традиционно в .NET пространство имен [`System.IO`](xref:System.IO) использовалось для поддержки файлового ввода-вывода. Некоторые методы в этом пространстве имен поддерживают асинхронные операции, но их немного. Пространство имен также поддерживает несколько простых вызовов методов, выполняющих сложные функции файлового ввода-вывода.

### <a name="good-news-and-bad-news"></a>Что в этом хорошего и что плохого

Все платформы, которые поддерживаются в Xamarin.Forms, поддерживают локальное хранилище приложения &mdash; хранилище, которое является частным для приложения.

Библиотеки Xamarin.iOS и Xamarin.Android включают версию .NET, которая специально адаптирована в Xamarin для этих двух платформ. Например, классы из `System.IO` можно использовать на этих двух платформах для выполнения файлового ввода-вывода в локальном хранилищем приложения.

Но вы не сможете найти эти классы `System.IO` в библиотеке PCL Xamarin.Forms. Проблема заключается в том, что корпорация Майкрософт полностью переработала механизм файлового ввода-вывода для API среды выполнения Windows. Программы, предназначенные для использования с Windows 8.1, Windows Phone 8.1 и универсальной платформы Windows, не используют `System.IO` для файлового ввода-вывода.

Это означает, что вам нужно будет использовать [`DependencyService`](xref:Xamarin.Forms.DependencyService) (см. главу 9 [ **Вызовы API конкретных платформ**](chapter09.md) как пример реализации файлового ввода-вывода).

> [!NOTE]
> Переносимые библиотеки классов заменены библиотеками .NET Standard 2.0, а .NET Standard 2.0 поддерживает типы [`System.IO`](xref:System.IO) для всех платформ Xamarin.Forms. Вам больше не нужно использовать `DependencyService` для большинства задач файлового ввода-вывода. Более современный подход к операциям файлового ввода-вывода см. в главе [Обработка файлов в Xamarin.Forms](~/xamarin-forms/data-cloud/data/files.md).

### <a name="a-first-shot-at-cross-platform-file-io"></a>Подход к организации файлового ввода-вывода для нескольких платформ

Пример [**TextFileTryout**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileTryout) определяет интерфейс [`IFileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/TextFileTryout/TextFileTryout/TextFileTryout/IFileHelper.cs) для файлового ввода-вывода и содержит реализации этого интерфейса на всех платформах. Но реализации для среды выполнения Windows не работают с методами в этом интерфейсе, так как методы ввода-вывода в среде выполнения Windows являются асинхронными.

### <a name="accommodating-windows-runtime-file-io"></a>Адаптация к файловому вводу-выводу для среды выполнения Windows

Программы, которые работают в среде выполнения Windows, используют для файлового ввода-вывода классы из пространств имен [`Windows.Storage`](/uwp/api/Windows.Storage) и [`Windows.Storage.Streams`](/uwp/api/Windows.Storage.Streams), в том числе для работы с локальным хранилищем приложения. Специалисты Майкрософт определили, что любая операция длительностью более 50 миллисекунд должна быть асинхронной, чтобы избежать блокировки в потоке пользовательского интерфейса, поэтому почти все эти методы файлового ввода-вывода являются асинхронными.

Код, демонстрирующий этот новый подход, будет размещен в библиотеке и доступен для других приложений.

## <a name="platform-specific-libraries"></a>Библиотеки для конкретных платформ

Многократно используемый код удобнее всего хранить в библиотеках. Конечно же, это становится намного сложнее, если разные фрагменты повторно используемого кода предназначены для совершенно разных операционных систем.

Один из возможных подходов демонстрируется в решении [ **Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform). Это решение содержит семь разных проектов:

- [ **Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform) — обычная библиотека Xamarin.Forms PCL;
- [ **Xamarin.FormsBook.Platform.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS) — библиотека классов для iOS;
- [ **Xamarin.FormsBook.Platform.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android) — библиотека классов для Android;
- [ **Xamarin.FormsBook.Platform.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.UWP) — библиотека классов для Универсальной платформы Windows;
- [ **Xamarin.FormsBook.Platform.WinRT**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT) — общий проект для кода, которые используется на всех платформах Windows.

Все проекты для отдельных платформ (кроме **Xamarin.FormsBook.Platform.WinRT**) содержат ссылки на **Xamarin.FormsBook.Platform**. Три проекта для Windows содержат ссылки на **Xamarin.FormsBook.Platform.WinRT**.

Все эти проекты содержат статический метод `Toolkit.Init`, который гарантирует загрузку библиотеки, если он не указан напрямую в проекте, включенном в решение приложения для Xamarin.Forms.

Проект **Xamarin.FormsBook.Platform** содержит новый интерфейс [`IFileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/IFileHelper.cs). Имена всех методов теперь дополняются суффиксами `Async` и возвращают объекты `Task`.

Проект **Xamarin.FormsBook.Platform.WinRT** содержит класс [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) для среды выполнения Windows.

Проект **Xamarin.FormsBook.Platform.iOS** содержит класс [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/FileHelper.cs) для iOS. Эти методы теперь должны быть асинхронными. Некоторые из них используют асинхронные версии методов, которые определены в `StreamWriter` и `StreamReader`, то есть [`WriteAsync`](xref:System.IO.StreamWriter.WriteAsync(System.String)) и [`ReadToEndAsync`](xref:System.IO.StreamReader.ReadToEndAsync). Другие преобразуют результат в объект `Task` с помощью метода [`FromResult`](xref:System.Threading.Tasks.Task.FromResult*).

Проект **Xamarin.FormsBook.Platform.Android** содержит аналогичный класс [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/FileHelper.cs) для Android.

Проект **Xamarin.FormsBook.Platform** также содержит класс [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/FileHelper.cs), который упрощает использование объекта `DependencyService`.

Чтобы использовать эти библиотеки, решение приложения должно включать все проекты в решении **Xamarin.FormsBook.Platform**, и каждый из проектов приложений должен иметь ссылку на соответствующую библиотеку из **Xamarin.FormsBook.Platform**.

В решении [**TextFileAsync**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileAsync) демонстрируется использование библиотек **Xamarin.FormsBook.Platform**. Каждый из этих проектов вызывает `Toolkit.Init`. Приложение использует асинхронные функции файлового ввода-вывода.

### <a name="keeping-it-in-the-background"></a>Оставляем работать в фоновом режиме

Методы в библиотеках, которые выполняют вызовы нескольких асинхронных методов &mdash;, например методы `WriteFileAsync` и `ReadFileASync` из класса &mdash; в [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) для среды выполнения Windows, можно сделать более эффективными с помощью метода [`ConfigureAwait`](xref:System.Threading.Tasks.Task`1.ConfigureAwait(System.Boolean)), который позволяет избежать переключения обратно в поток пользовательского интерфейса.

### <a name="dont-block-the-ui-thread"></a>Никогда не блокируйте поток пользовательского интерфейса.

Иногда есть соблазн обойтись без `ContinueWith` или `await`, просто указав для методов свойство [`Result`](xref:System.Threading.Tasks.Task`1.Result). Но этого следует избегать, так как есть риск заблокировать поток пользовательского интерфейса, а также работу всего приложения.

## <a name="your-own-awaitable-methods"></a>Собственные методы для ожидания

Вы можете выполнить часть кода асинхронно, передав его в один из методов [`Task.Run`](xref:System.Threading.Tasks.Task.Run(System.Action)). `Task.Run` можно вызвать в асинхронном методе, который берет на себя часть дополнительной работы.

Ниже обсуждаются некоторые подходы к использованию `Task.Run`.

### <a name="the-basic-mandelbrot-set"></a>Базовая реализация множества Мандельброта

Чтобы визуализировать множество Мандельброта в реальном времени, в библиотеке [ **Xamarin.Forms.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) предоставляется структура [`Complex`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/Complex.cs), аналогичная структуре из пространства имен `System.Numerics`.

Пример [**MandelbrotSet**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotSet) содержит в файле кода программной части метод `CalculateMandeblotAsync`, который вычисляет базовое черно-белое отображение множества Мандельброта и с помощью [`BmpMaker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) размещает его в растровом изображении.

### <a name="marking-progress"></a>Отображение хода выполнения

Чтобы отслеживать ход выполнения асинхронного метода, вы можете создать экземпляр класса [`Progress<T>`](xref:System.Progress`1) и определить в асинхронном методе аргумент с типом [`IProgress<T>`](xref:System.IProgress`1). Этот подход демонстрируется в примере [**MandelbrotProgress**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotProgress).

### <a name="cancelling-the-job"></a>Отмена задания

Можно также реализовать возможность отмены асинхронного метода. Для начала создайте класс с именем [`CancellationTokenSource`](xref:System.Threading.CancellationTokenSource). Свойство [`Token`](xref:System.Threading.CancellationTokenSource.Token) имеет значение с типом [`CancellationToken`](xref:System.Threading.CancellationToken). Оно передается в асинхронную функцию. Программа вызывает метод [`Cancel`](xref:System.Threading.CancellationTokenSource.Cancel) из `CancellationTokenSource` (обычно в ответ на действие пользователя), чтобы отменить выполнение асинхронной функции.

Асинхронный метод может периодически проверять свойство [`IsCancellationRequested`](xref:System.Threading.CancellationToken.IsCancellationRequested) в `CancellationToken` и завершать работу, если это свойство имеет значение `true`, или просто вызвать метод [`ThrowIfCancellationRequested`](xref:System.Threading.CancellationToken.ThrowIfCancellationRequested), чтобы завершить работу с вызовом исключения [`OperationCancelledException`](xref:System.OperationCanceledException).

В примере [**MandelbrotCancellation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotCancellation) показано использование функции с поддержкой отмены.

### <a name="an-mvvm-mandelbrot"></a>MVVM для множества Мандельброта

Пример [**MandelbrotXF**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotXF) предоставляет более сложный пользовательский интерфейс и почти полностью основан на классах [`MandelbrotModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotModel.cs) и [`MandelbrotViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotViewModel.cs).

[![Снимок экрана с тремя изображениями X F множества Мандельброта](images/ch20fg13-small.png "MVVM для множества Мандельброта")](images/ch20fg13-large.png#lightbox "MVVM для множества Мандельброта")

## <a name="back-to-the-web"></a>И опять о веб-решениях

Класс [`WebRequest`](xref:System.Net.WebRequest), который используется в нескольких примерах, применяет старомодный протокол асинхронной работы APM. Вы можете перевести этот класс на использование более современного протокола TAP, применив любой из методов `FromAsync` класса [`TaskFactory`](xref:System.Threading.Tasks.TaskFactory`1). Пример [**ApmToTap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/ApmToTap) демонстрирует такой сценарий.

## <a name="related-links"></a>Связанные ссылки

- [Глава 20, полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)
- [Примеры для Главы 20](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)
- [Работа с файлами](~/xamarin-forms/data-cloud/data/files.md)
