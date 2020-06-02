---
title: Повышение производительности приложения Xamarin.Forms
description: Существует множество методов повышения производительности приложений Xamarin.Forms. Вместе они могут значительно снизить загрузку ЦП и сократить объем памяти, используемой приложением.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 96b5939fd1f8448d45d1398fd56770f9032de083
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139117"
---
# <a name="improve-xamarinforms-app-performance"></a>Повышение производительности приложения Xamarin.Forms

> [!VIDEO https://youtube.com/embed/RZvdql3Ev0E]

**Усовершенствовано в 2016 г. Оптимизация производительности приложения с помощью Xamarin.Forms**

Низкая производительность приложения проявляется по-разному. Из-за нее приложение может переставать отвечать на запросы, могут возникать задержки при прокрутке и сокращаться время работы батареи устройства. Однако оптимизация производительности предусматривает не только правильную реализацию кода. Необходимо также учитывать эффективность работы пользователей. Например, чтобы повысить удобство работы, необходимо сделать так, чтобы выполнение операций не мешало пользователю выполнять другие действия.

Существует ряд методов повышения производительности приложений Xamarin.Forms, в том числе воспринимаемой пользователем. Вместе они могут значительно снизить загрузку ЦП и сократить объем памяти, используемой приложением.

> [!NOTE]
> Прежде чем прочитать эту статью, ознакомьтесь со статьей [Кроссплатформенная производительность](~/cross-platform/deploy-test/memory-perf-best-practices.md), в которой описываются универсальные для всех платформ способы оптимизации использования памяти и повышения производительности приложений, построенных с помощью платформы Xamarin.

## <a name="enable-the-xaml-compiler"></a>Включение компилятора XAML

При необходимости можно воспользоваться компилятором XAML (XAMLC) и скомпилировать XAML напрямую в промежуточный язык (IL). Компилятор XAMLC предлагает ряд преимуществ:

- Он проверяет XAML во время компиляции, уведомляя пользователя об ошибках.
- Он сокращает часть времени загрузки и создания элементов XAML.
- Он позволяет сократить размер файла окончательной сборки, так как больше не добавляет XAML-файлы.

XAMLC по умолчанию включен в новых решениях Xamarin.Forms. Однако может потребоваться включить его в старых решениях. Дополнительные сведения см. в разделе [Компиляция XAML](~/xamarin-forms/xaml/xamlc.md).

## <a name="use-compiled-bindings"></a>Использование скомпилированных привязок

Использование скомпилированных привязок позволяет оптимизировать производительность привязки данных в приложениях Xamarin.Forms за счет разрешения выражений привязки во время компиляции, а не во время выполнения при помощи отражения. При компиляции выражения привязки создается скомпилированный код, который обычно разрешает привязку в 8–20 раз быстрее, чем при использовании классической привязки. Дополнительные сведения см. в статье [Скомпилированные привязки](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

## <a name="reduce-unnecessary-bindings"></a>Сокращение количества ненужных привязок

Не используйте привязки для содержимого, которое можно легко задать статически. В привязке данных, которые не должны быть привязаны, нет никакой выгоды, так как привязки нерентабельны с точки зрения затрат. Например, установка `Button.Text = "Accept"` связана с меньшими издержками, чем привязка [`Button.Text`](xref:Xamarin.Forms.Button.Text) к свойству `string` компонента ViewModel со значением Accept.

## <a name="use-fast-renderers"></a>Использование быстрых отрисовщиков

Быстрые отрисовщики сокращают затраты на отрисовку элементов управления Xamarin.Forms на платформе Android путем сведения итоговой иерархии собственных элементов управления. Это еще больше способствует повышению производительности, так как создается меньше объектов, в результате чего формируется менее сложное визуальное дерево, а также сокращается использование памяти.

Начиная с Xamarin.Forms 4.0 все приложения, нацеленные на `FormsAppCompatActivity`, используют быстрые отрисовщики по умолчанию. Дополнительные сведения см. в статье [Быстрые отрисовщики](~/xamarin-forms/internals/fast-renderers.md).

## <a name="enable-startup-tracing-on-android"></a>Включение трассировки запуска на Android

Компиляция до времени (AOT) в Android минимизирует затраты на запуск приложения JIT и использование памяти за счет создания гораздо большего APK-файла. Альтернативой является использование трассировки запуска, которая обеспечивает компромисс между размером Android APK и временем запуска по сравнению с обычной компиляцией AOT.

Вместо того чтобы компилировать как можно большую часть приложения в виде неуправляемого кода, трассировка запуска компилирует только набор управляемых методов, представляющих самые ресурсоемкие части запуска приложения, в пустом приложении Xamarin.Forms. Такой подход приводит к уменьшению размера APK по сравнению с обычной компиляцией AOT, обеспечивая при этом те же улучшения с точки зрения запуска.

## <a name="enable-layout-compression"></a>Включение сжатия макета

Сжатие макета позволяет удалить указанные макеты из визуального дерева в целях повышения скорости отрисовки страниц. В этом случае рост производительности зависит от сложности страницы, версии используемой операционной системы и устройства, на котором выполняется приложение. Однако наиболее заметное повышение производительности будет наблюдаться на старых устройствах. Дополнительные сведения см. в статье [Сжатие макета](~/xamarin-forms/user-interface/layouts/layout-compression.md).

## <a name="choose-the-correct-layout"></a>Выбор подходящего макета

Макет, который может отображать несколько дочерних элементов, но имеет лишь один такой элемент, связан с дополнительными издержками. Например, в следующем примере кода показан макет [`StackLayout`](xref:Xamarin.Forms.StackLayout) с одним дочерним элементом:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <StackLayout>
        <Image Source="waterfront.jpg" />
    </StackLayout>
</ContentPage>
```

Он нерационален, поэтому элемент [`StackLayout`](xref:Xamarin.Forms.StackLayout) должен быть удален, как показано в следующем примере кода:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <Image Source="waterfront.jpg" />
</ContentPage>
```

Кроме того, не пытайтесь воспроизвести внешний вид определенного макета с помощью сочетаний других макетов, так как это приводит к выполнению ненужных вычислений макета. Например, не пытайтесь воспроизвести макет [`Grid`](xref:Xamarin.Forms.Grid) с помощью сочетания экземпляров [`StackLayout`](xref:Xamarin.Forms.StackLayout). В следующем коде показан пример такого нерекомендуемого действия:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Name:" />
            <Entry Placeholder="Enter your name" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Age:" />
            <Entry Placeholder="Enter your age" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Occupation:" />
            <Entry Placeholder="Enter your occupation" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Address:" />
            <Entry Placeholder="Enter your address" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Такой метод неэффективен, так как выполняются ненужные вычисления макета. Вместо этого для формирования нужного макета следует использовать [`Grid`](xref:Xamarin.Forms.Grid), как показано в следующем примере кода:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="100" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
        </Grid.RowDefinitions>
        <Label Text="Name:" />
        <Entry Grid.Column="1" Placeholder="Enter your name" />
        <Label Grid.Row="1" Text="Age:" />
        <Entry Grid.Row="1" Grid.Column="1" Placeholder="Enter your age" />
        <Label Grid.Row="2" Text="Occupation:" />
        <Entry Grid.Row="2" Grid.Column="1" Placeholder="Enter your occupation" />
        <Label Grid.Row="3" Text="Address:" />
        <Entry Grid.Row="3" Grid.Column="1" Placeholder="Enter your address" />
    </Grid>
</ContentPage>
```

## <a name="optimize-layout-performance"></a>Оптимизация производительности макета

Чтобы добиться максимально возможной производительности, соблюдайте следующие правила:

- Сократите глубину иерархий макетов, указав значения свойств [`Margin`](xref:Xamarin.Forms.View.Margin), что позволит создавать макеты с меньшим количеством представлений-оболочек. Дополнительные сведения см. в статье [Внешние и внутренние поля](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).
- При использовании [`Grid`](xref:Xamarin.Forms.Grid) постарайтесь сделать так, чтобы размер [`Auto`](xref:Xamarin.Forms.GridLength.Auto) был задан как можно меньшему количеству строк и столбцов. Из-за каждой строки или столбца с автоматическим размером обработчик макета будет выполнять дополнительные вычисления макета. Если возможно, используйте строки и столбцы фиксированного размера. Кроме того, с помощью значения перечисления [`GridUnitType.Star`](xref:Xamarin.Forms.GridUnitType.Star) можно указать, чтобы строки и столбцы занимали пропорциональную часть пространства при условии, что родительское дерево соответствует этим правилам макета.
- Не задавайте свойства макета [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) и [`HorizontalOptions`](xref:Xamarin.Forms.View.VerticalOptions), если этого не требуется. Значения по умолчанию для свойств [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) и [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) обеспечивают наилучшую оптимизацию макета. Изменение этих свойств связано с издержками и потреблением памяти даже в том случае, если свойствам задаются значения по умолчанию.
- По возможности избегайте использования [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout). В противном случае ЦП будет испытывать значительно большую нагрузку.
- При использовании [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) по возможности избегайте использования свойства [`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize).
- При использовании [`StackLayout`](xref:Xamarin.Forms.StackLayout) убедитесь, что свойство [`LayoutOptions.Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) задано только одному дочернему элементу. В этом случае указанный дочерний элемент будет занимать максимальное пространство, предоставляемое ему макетом `StackLayout`. Выполнять эти вычисления несколько раз слишком затратно.
- Не вызывайте методы класса [`Layout`](xref:Xamarin.Forms.Layout), так как они связаны с дорогостоящими вычислениями макета. Скорее всего, нужного поведения макета можно добиться путем установки свойств [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) и [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY). Кроме того, для этого можно вывести подкласс из класса [`Layout<View>`](xref:Xamarin.Forms.Layout`1).
- Не обновляйте экземпляры [`Label`](xref:Xamarin.Forms.Label) чаще, чем требуется, так как изменение размера метки может привести к пересчету всего макета экрана.
- Не задавайте свойство [`Label.VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment), если это не требуется.
- По возможности задайте свойству [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) любых экземпляров [`Label`](xref:Xamarin.Forms.Label) значение [`NoWrap`](xref:Xamarin.Forms.LineBreakMode.NoWrap).

## <a name="use-asynchronous-programming"></a>Использование асинхронного программирования

С помощью асинхронного программирования можно увеличить общую скорость реагирования приложения и избежать появления узких мест производительности. В .NET [асинхронная модель на основе задач](/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap) является рекомендуемым конструктивным шаблоном для реализации асинхронных операций. Однако неправильное использование этой модели может привести к созданию приложений с низкой производительностью. Поэтому при применении асинхронной модели на основе задач следует соблюдать приведенные ниже рекомендации.

### <a name="fundamentals"></a>Основы

- Разберитесь в жизненном цикле задачи, представленном перечислением `TaskStatus`. Дополнительные сведения см. в разделах [Значение TaskStatus](https://devblogs.microsoft.com/pfxteam/the-meaning-of-taskstatus/) и [Состояние задачи](/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap#task-status).
- Используйте метод `Task.WhenAll` для асинхронного ожидания завершения нескольких асинхронных операций вместо ожидания каждой из ряда асинхронных операций по-отдельности с помощью `await`. Дополнительные сведения см. в разделе [Task.WhenAll](/dotnet/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern#taskwhenall).
- Используйте метод `Task.WhenAny` для асинхронного ожидания завершения одной из нескольких асинхронных операций. Дополнительные сведения см. в разделе [Task.WhenAny](/dotnet/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern#taskwhenall).
- Используйте метод `Task.Delay` для создания объекта `Task`, который завершается после определенного времени. Это полезно в различных ситуациях, включая опрос данных и задержку обработки введенных пользователем данных на заданный период времени. Дополнительные сведения см. в разделе [Task.Delay](/dotnet/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern#taskdelay).
- Выполняйте синхронные операции, интенсивно использующие ЦП, в пуле потоков с помощью метода `Task.Run`. Это ускоренный вариант метода `TaskFactory.StartNew` с оптимальным набором аргументов. Дополнительные сведения см. в разделе [Task.Run](/dotnet/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern#taskrun).
- Старайтесь не создавать асинхронные конструкторы. Вместо этого используйте события жизненного цикла или отдельную логику инициализации для правильного ожидания (`await`) любой инициализации. Дополнительные сведения см. в записи блога [Async Constructors](https://blog.stephencleary.com/2013/01/async-oop-2-constructors.html) (Асинхронные конструкторы) на сайте blog.stephencleary.com.
- Используйте шаблон отложенной задачи, чтобы избежать ожидания завершения асинхронных операций во время запуска приложения. Дополнительные сведения см. в разделе [AsyncLazy](https://devblogs.microsoft.com/pfxteam/asynclazyt/).
- Создайте оболочку задачи для существующих асинхронных операций, которые не используют асинхронную модель на основе задач, путем создания объектов `TaskCompletionSource<T>`. Эти объекты обеспечивают возможности программирования `Task` и позволяют контролировать время существования и завершение связанной задачи `Task`. Дополнительные сведения см. в записи блога [The Nature of TaskCompletionSource](https://devblogs.microsoft.com/pfxteam/the-nature-of-taskcompletionsourcetresult/) (Суть типа TaskCompletionSource).
 
- Когда нет необходимости обрабатывать результат асинхронной операции, следует возвращать объект `Task` вместо ожидаемого объекта `Task`. Это более производительный подход, так как переключать контекст приходится реже.
- Используйте библиотеку потоков данных "Библиотека параллельных задач" (TPL) в ситуациях, когда, например, необходимо обрабатывать данные по мере того, как они становятся доступными, или когда несколько операций должны взаимодействовать друг с другом асинхронно. Дополнительные сведения см. в статье [Поток данных (библиотека параллельных задач)](/dotnet/standard/parallel-programming/dataflow-task-parallel-library).

### <a name="ui"></a>ИП

- Вызывайте асинхронную версию API, если она доступна. Это позволит избежать блокирования потока пользовательского интерфейса, что повысит удобство работы пользователя с приложением.
- Обновляйте элементы пользовательского интерфейса данными из асинхронных операций в потоке пользовательского интерфейса, чтобы избежать возникновения исключений. Однако изменения свойства `ListView.ItemsSource` будут автоматически маршалироваться в поток пользовательского интерфейса. Сведения о том, как определить, выполняется ли код в потоке пользовательского интерфейса, см. в разделе [Xamarin.Essentials: MainThread](~/essentials/main-thread.md?content=xamarin/xamarin-forms).

    > [!IMPORTANT]
    > Все свойства элементов управления, которые изменяются путем привязки данных, будут автоматически маршалироваться в поток пользовательского интерфейса.

### <a name="error-handling"></a>Обработка ошибок

- Ознакомьтесь с асинхронной обработкой исключений. Необработанные исключения, создаваемые асинхронно выполняемым кодом, распространяются обратно в вызывающий поток, за исключением отдельных сценариев. Дополнительные сведения см. в статье [Обработка исключений (библиотека параллельных задач)](/dotnet/standard/parallel-programming/exception-handling-task-parallel-library).
- Избегайте создания методов `async void`. Вместо этого создавайте методы `async Task`. Это упрощает обработку ошибок, компоновку и тестирование. Исключением из этого правила являются асинхронные обработчики событий, которые должны возвращать `void`. Дополнительные сведения см. в разделе [Избегайте async void](/archive/msdn-magazine/2013/march/async-await-best-practices-in-asynchronous-programming#avoid-async-void).
- Не смешивайте блокирующий и асинхронный код путем вызова методов `Task.Wait`, `Task.Result` или `GetAwaiter().GetResult`, так как это может привести к возникновению взаимоблокировки. Однако если это правило необходимо нарушить, предпочтительным подходом является вызов метода `GetAwaiter().GetResult`, так как он сохраняет исключения задачи. Дополнительные сведения см. в разделах [Соблюдайте асинхронность от начала до конца](/archive/msdn-magazine/2013/march/async-await-best-practices-in-asynchronous-programming#async-all-the-way) и [Обработка исключений задач в .NET 4.5](https://devblogs.microsoft.com/pfxteam/task-exception-handling-in-net-4-5/).
- По возможности используйте метод `ConfigureAwait` для создания кода, не зависящего от контекста. Код, не зависящий от контекста, имеет более высокую производительность в мобильных приложениях и позволяет предотвращать взаимоблокировки при работе с частично асинхронной базой кода. Дополнительные сведения см. в разделе [Конфигурируйте контекст](/archive/msdn-magazine/2013/march/async-await-best-practices-in-asynchronous-programming#configure-context).
- Используйте *задачи продолжения* для таких функций, как обработка исключений, созданных предыдущей асинхронной операцией, и отмена продолжения перед его запуском либо во время его выполнения. Дополнительные сведения см. в статье [Создание цепочки задач с помощью задач продолжения](/dotnet/standard/parallel-programming/chaining-tasks-by-using-continuation-tasks).
- Используйте асинхронную реализацию `ICommand`, когда асинхронные операции вызываются из `ICommand`. Это позволит обрабатывать все исключения в логике асинхронных команд. Дополнительные сведения см. в статье [Асинхронное программирование. Шаблоны для асинхронных MVVM-приложений: команды](/archive/msdn-magazine/2014/april/async-programming-patterns-for-asynchronous-mvvm-applications-commands).

## <a name="choose-a-dependency-injection-container-carefully"></a>Тщательный выбор контейнера внедрения зависимостей

Контейнеры внедрения зависимостей налагают дополнительные ограничения производительности для мобильных приложений. Регистрация и разрешение типов в контейнере влечет затраты с точки зрения производительности из-за использования отражения в контейнере для создания каждого типа, особенно если зависимости перестраиваются при каждом переходе по страницам в приложении. При наличии большого числа зависимостей или глубоких зависимостей стоимость создания может значительно возрасти. Кроме того, регистрация типов, которая обычно происходит во время запуска приложения, может оказать заметное воздействие на время запуска в зависимости от используемого контейнера.

В качестве альтернативы можно сделать внедрение зависимостей более производительным, реализовав его вручную с помощью фабрик.

## <a name="create-shell-applications"></a>Создание приложения оболочки

Приложения оболочки Xamarin.Forms предоставляют специализированный пользовательский интерфейс навигации на основе всплывающих элементов и вкладок. Если взаимодействие с пользователем приложения можно реализовать с помощью оболочки, это полезно сделать. Преимуществом этого подхода является ускорение запуска приложения, так как страницы создаются только по запросу в ответ на действия навигации, а не сразу при запуске приложения, как в приложениях, использующих [TabbedPage](xref:Xamarin.Forms.TabbedPage). Дополнительные сведения см. в разделе [Оболочка Xamarin.Forms](~/xamarin-forms/app-fundamentals/shell/index.md).

## <a name="use-collectionview-instead-of-listview"></a>Использование CollectionView вместо ListView

Представление [`CollectionView`](xref:Xamarin.Forms.CollectionView) служит для вывода списков данных с различными спецификациями макета. Оно предоставляет более гибкую и производительную альтернативу [`ListView`](xref:Xamarin.Forms.ListView). Дополнительные сведения см. в разделе [Xamarin.Forms CollectionView](~/xamarin-forms/user-interface/collectionview/index.md).

## <a name="optimize-listview-performance"></a>Оптимизация производительности ListView

При использовании элемента управления [`ListView`](xref:Xamarin.Forms.ListView) необходимо оптимизировать несколько механизмов взаимодействия с пользователем.

- **Инициализация** — интервал времени, который начинается с момента создания элемента управления, и заканчивается при отображении элементов на экране.
- **Прокрутка** — возможность прокрутки списка и проверка того, что пользовательский интерфейс не отстает от сенсорных жестов.
- **Взаимодействие** для добавления, удаления и выбора элементов.

Для использования элемента управления [`ListView`](xref:Xamarin.Forms.ListView) требуется предоставить шаблоны данных и ячеек. От выполнения этого условия будет в значительной степени зависеть производительность элемента управления. Дополнительные сведения см. в статье [Производительность элемента управления ListView](~/xamarin-forms/user-interface/listview/performance.md).

## <a name="optimize-image-resources"></a>Оптимизация графических ресурсов

Отображение графических ресурсов может значительно увеличивать объем памяти, занимаемой приложением. Поэтому их следует создавать только при необходимости и сразу же освобождать, если они больше не нужны приложению. Например, если приложение отображает изображение, считывая его данные из потока, убедитесь, что поток создается только в том случае, если он необходим. Когда поток больше не нужен, его следует освободить. Это можно сделать, создав поток при создании страницы или при возникновении события [`Page.Appearing`](xref:Xamarin.Forms.Page.Appearing), а затем удалив его при возникновении события [`Page.Disappearing`](xref:Xamarin.Forms.Page.Disappearing).

Изображение, скачанное с помощью метода [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)), необходимо кэшировать, задав свойству [`UriImageSource.CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled) значение `true`. Дополнительные сведения см. в статье о [работе с изображениями](~/xamarin-forms/user-interface/images.md).

Дополнительные сведения см. в разделе [Оптимизация графических ресурсов](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages).

## <a name="reduce-the-visual-tree-size"></a>Уменьшение размера визуального дерева

Сокращение количества элементов на странице способствует ускорению отрисовки страницы. Существует два основных способа решения этой задачи. Первый заключается в скрытии элементов, которые не отображаются. Свойство [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) каждого элемента определяет, должен ли элемент быть частью визуального дерева. Таким образом, если элемент не отображается, поскольку он скрыт другими элементами, удалите элемент или задайте его свойству `IsVisible` значение `false`.

Второй метод предполагает удаление ненужных элементов. Например, в следующем примере кода показан макет страницы, отображающий ряд объектов [`Label`](xref:Xamarin.Forms.Label):

```xaml
<StackLayout>
    <StackLayout Padding="20,20,0,0">
        <Label Text="Hello" />
    </StackLayout>
    <StackLayout Padding="20,20,0,0">
        <Label Text="Welcome to the App!" />
    </StackLayout>
    <StackLayout Padding="20,20,0,0">
        <Label Text="Downloading Data..." />
    </StackLayout>
</StackLayout>
```

Этот же макет страницы можно оставить с меньшим количеством элементов, как показано в следующем примере кода:

```xaml
<StackLayout Padding="20,35,20,20" Spacing="25">
  <Label Text="Hello" />
  <Label Text="Welcome to the App!" />
  <Label Text="Downloading Data..." />
</StackLayout>
```

## <a name="reduce-the-application-resource-dictionary-size"></a>Уменьшение размера словаря ресурсов приложения

Во избежание дублирования все ресурсы, используемые в приложении, должны храниться в словаре ресурсов приложения. Это позволит сократить объем кода XAML, подлежащий анализу в приложении. В следующем примере кода показан ресурс `HeadingLabelStyle`, который используется во всем приложении и, таким образом, определен в словаре ресурсов приложении:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Resources.App">
     <Application.Resources>
         <ResourceDictionary>
            <Style x:Key="HeadingLabelStyle" TargetType="Label">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
         </ResourceDictionary>
     </Application.Resources>
</Application>
```

Тем не менее XAML, соответствующий странице, не следует включать в словарь ресурсов приложения, так как анализ ресурсов будет осуществляться при запуске приложения, а не когда это необходимо для страницы. Если ресурс используется страницей, не являющейся стартовой, его необходимо поместить в словарь ресурсов для этой страницы, что способствует сокращению объема XAML, анализируемого при запуске приложения. В следующем примере кода показан ресурс `HeadingLabelStyle`, который используется только на одной странице и, таким образом, определен в словаре ресурсов страницы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Test.HomePage"
             Padding="0,20,0,0">
    <ContentPage.Resources>
        <ResourceDictionary>
          <Style x:Key="HeadingLabelStyle" TargetType="Label">
              <Setter Property="HorizontalOptions" Value="Center" />
              <Setter Property="FontSize" Value="Large" />
              <Setter Property="TextColor" Value="Red" />
          </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Дополнительные сведения о ресурсах приложений см. в статье [Стили XAML](~/xamarin-forms/user-interface/styles/xaml/index.md).

## <a name="use-the-custom-renderer-pattern"></a>Использование шаблона настраиваемого отрисовщика

Большинство классов отрисовщиков Xamarin.Forms предоставляют метод `OnElementChanged`, который вызывается при создании пользовательского элемента управления Xamarin.Forms для отрисовки соответствующего собственного элемента управления. Затем классы настраиваемых отрисовщиков в проекте для каждой платформы переопределяют этот метод, чтобы создать и настроить собственный элемент управления. Метод `SetNativeControl` используется для построения собственного элемента управления и присваивает свойству `Control` ссылку на элемент управления.

Однако в некоторых случаях метод `OnElementChanged` может вызываться несколько раз. Поэтому в целях предотвращения утечек памяти, которые могут негативно повлиять на производительность, необходимо с осторожностью создавать собственный элемент управления. В следующем примере кода показан подход, используемый при создании собственного элемента управления в пользовательском отрисовщике:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null)
  {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null)
  {
    if (Control == null)
    {
      // Instantiate the native control with the SetNativeControl method
    }
    // Configure the control and subscribe to event handlers
  }
}
```

Собственный элемент управления должен создаваться только один раз, когда свойству `Control` задано значение `null`. Кроме того, элемент управления следует создавать, настраивать и подписывать на обработчики событий только при присоединении пользовательского отрисовщика к новому элементу Xamarin.Forms. Аналогично, от всех обработчиков событий, на которые были созданы подписки, следует отписываться только при изменении элемента с подключенным отрисовщиком. Реализовав этот подход, вы сможете создать эффективно работающий настраиваемый отрисовщик, на который не влияют утечки памяти.

> [!IMPORTANT]
> Метод `SetNativeControl` следует вызывать только в том случае, если свойство `e.NewElement` не имеет значение `null`, а свойство `Control` имеет значение `null`.

Дополнительные сведения о настраиваемых отрисовщиках см. в статье [Настройка элементов управления на каждой платформе](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## <a name="related-links"></a>Связанные ссылки

- [Кроссплатформенная производительность](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Компиляция XAML](~/xamarin-forms/xaml/xamlc.md)
- [Скомпилированные привязки](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)
- [Быстрые отрисовщики](~/xamarin-forms/internals/fast-renderers.md)
- [Сжатие макета](~/xamarin-forms/user-interface/layouts/layout-compression.md)
- [Оболочка Xamarin.Forms](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Xamarin.Forms CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)
- [Производительность элемента управления ListView](~/xamarin-forms/user-interface/listview/performance.md)
- [Оптимизация графических ресурсов](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages)
- [Стили XAML](~/xamarin-forms/user-interface/styles/xaml/index.md)
- [Настройка элементов управления на каждой платформе](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
