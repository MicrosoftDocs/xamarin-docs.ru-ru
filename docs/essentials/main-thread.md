---
title: ''Xamarin.Essentials: MainThread'' description: ms.assetid: author: ms.custom: ms.author: ms.date: no-loc:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---

# <a name="xamarinessentials-mainthread"></a>Xamarin.Essentials. MainThread

Класс **MainThread** позволяет выполнять код в основном потоке приложения, а также проверять, выполняется ли конкретный блок кода в основном потоке в конкретный момент.

## <a name="background"></a>Фон

Большинство операционных систем, включая iOS, Android и универсальную платформу Windows, используют однопоточную модель для кода, имеющего отношение к пользовательскому интерфейсу. Такая модель необходима для правильной сериализации событий пользовательского интерфейса, например нажатий клавиш и сенсорного ввода. Этот поток может упоминаться как _основной потоком_, _поток, обрабатывающий события от пользовательского интерфейса,_ или _поток пользовательского интерфейса_. Недостатком этой модели является то, что весь код с доступом к элементам пользовательского интерфейса должен выполняться в основном потоке приложения.

Иногда приложениям приходится использовать события, которые вызывают обработчик событий во второстепенном потоке выполнения. (Классы Xamarin.Essentials [`Accelerometer`](accelerometer.md), [`Compass`](compass.md), [`Gyroscope`](gyroscope.md), [`Magnetometer`](magnetometer.md) и [`OrientationSensor`](orientation-sensor.md) могут возвращать сведения во второстепенном потоке, если работают с высокой скоростью.) Если обработчику событий нужен доступ к элементам пользовательского интерфейса, он должен выполнять соответствующий код в основном потоке. Класс **MainThread** позволяет приложениям выполнять код в основном потоке.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="running-code-on-the-main-thread"></a>Выполнение кода в основном потоке

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Чтобы запустить код в основном потоке, вызовите статический метод `MainThread.BeginInvokeOnMainThread`. Он принимает в качестве аргумента объект [`Action`](xref:System.Action), который представляет собой метод без аргументов и возвращаемых значений:

```csharp
MainThread.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

Также есть возможность определить отдельный метод для кода, который должен выполняться в основном потоке:

```csharp
void MyMainThreadCode()
{
    // Code to run on the main thread
}
```

Вы сможете запустить этот метод в основном потоке, передав ссылку на него в метод `BeginInvokeOnMainThread`:

```csharp
MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms содержит метод с именем [`Device.BeginInvokeOnMainThread(Action)`](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread)
> который делает то же, что и `MainThread.BeginInvokeOnMainThread(Action)`.
> Вы можете использовать в приложении Xamarin.Forms любой из этих методов, но при выборе желательно учитывать, есть ли у вызывающего кода любая другая зависимость от Xamarin.Forms. Если такой зависимости нет, обычно лучше использовать `MainThread.BeginInvokeOnMainThread(Action)`.

## <a name="determining-if-code-is-running-on-the-main-thread"></a>Определение того, выполняется ли код в основном потоке

Класс `MainThread` позволяет приложению определить, выполняется ли конкретный блок кода в основном потоке. Свойство `IsMainThread` возвращает значение `true`, если обращающийся к нему код выполняется в основном потоке. Программа может использовать это свойство, чтобы условно запускать другой код в основном или второстепенном потоке:

```csharp
if (MainThread.IsMainThread)
{
    // Code to run if this is the main thread
}
else
{
    // Code to run if this is a secondary thread
}
```

Возможно, у вас возникло желание проверять поток выполнения кода перед вызовом `BeginInvokeOnMainThread`, например так:

```csharp
if (MainThread.IsMainThread)
{
    MyMainThreadCode();
}
else
{
    MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
}
```

Можно предположить, что такая проверка может повысить производительность, если блок кода уже выполняется в основном потоке.

_Но на самом деле эта проверка не нужна._ Все реализации платформ `BeginInvokeOnMainThread` самостоятельно проверяют, выполняется ли вызов из основного потока. Если вы будете вызывать метод `BeginInvokeOnMainThread` без реальной необходимости, это приведет к небольшому снижению производительности.

## <a name="additional-methods"></a>Дополнительные методы

Класс `MainThread` включает следующие дополнительные методы `static`, которые можно использовать для взаимодействия с элементами пользовательского интерфейса из фоновых потоков.

| Метод | Аргументы | Возвращает | Цель |
|---|---|---|---|
| `InvokeOnMainThreadAsync<T>` | `Func<T>` | `Task<T>` | Вызывает объект `Func<T>` в основном потоке и ожидает его завершения. |
| `InvokeOnMainThreadAsync` | `Action` | `Task` | Вызывает объект `Action` в основном потоке и ожидает его завершения. |
| `InvokeOnMainThreadAsync<T>`| `Func<Task<T>>` | `Task<T>` | Вызывает объект `Func<Task<T>>` в основном потоке и ожидает его завершения. |
| `InvokeOnMainThreadAsync` | `Func<Task>` | `Task` | Вызывает объект `Func<Task>` в основном потоке и ожидает его завершения. |
| `GetMainThreadSynchronizationContextAsync` | | `Task<SynchronizationContext>` | Возвращает `SynchronizationContext` для основного потока. |

## <a name="api"></a>API

- [Исходный код MainThread](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/MainThread)
- [Документация по API для MainThread](xref:Xamarin.Essentials.MainThread)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Main-Thread-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
