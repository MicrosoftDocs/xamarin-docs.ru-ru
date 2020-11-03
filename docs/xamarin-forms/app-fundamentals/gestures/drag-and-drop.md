---
title: Добавление распознавателя жестов перетаскивания
description: В этой статье объясняется, как распознать жесты перетаскивания с помощью Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 4CB2F270-908A-4A89-B852-70BC04066E8C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/27/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: af56e84598f73693a8cb0e93573b789a716c194a
ms.sourcegitcommit: 1550019cd1e858d4d13a4ae6dfb4a5947702f24b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/28/2020
ms.locfileid: "92897472"
---
# <a name="add-drag-and-drop-gesture-recognizers"></a>Добавление распознавателя жестов перетаскивания

![Предварительный выпуск API](~/media/shared/preview.png)

[![Скачать пример](~/media/shared/download.png) Скачайте пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-draganddropgesture/)

Жест перетаскивания позволяет перетаскивать элементы и связанные с ними пакеты данных из одного расположения на экране в другое, используя непрерывный жест. Перетаскивание можно выполнять в одном приложении или запустить в одном приложении и закончить в другом.

> [!IMPORTANT]
> Распознаватель жестов перетаскивания Xamarin.Forms в настоящее время является экспериментальным и может использоваться только при установке флага `DragAndDrop_Experimental`. Дополнительные сведения см. в статье [Флаги экспериментального режима Xamarin.Forms](~/xamarin-forms/internals/experimental-flags.md).
>
> Распознавание жестов перетаскивания поддерживается в iOS, Android и универсальной платформе Windows (UWP). Однако в iOS требуется минимальная версия платформы iOS 11.

*Источник перетаскивания* , являющийся элементом, на котором инициируется жест перетаскивания, может обеспечить передачу данных путем заполнения объекта пакета данных. Когда источник перетаскивания освобождается, происходит отпускание. *Целевой объект перетаскивания* , который является элементом источника перетаскивания, обрабатывает пакет данных.

Процесс включения перетаскивания в приложение выглядит следующим образом:

1. Включите перетаскивание элемента, добавив объект `DragGestureRecognizer` в его коллекцию `GestureRecognizers`, и выберите для свойства `DragGestureRecognizer.CanDrag` значение `true`. Дополнительные сведения см. в разделе [Включение перетаскивания](#enable-drag).
1. [Необязательно] Создайте пакет данных. Xamarin.Forms автоматически заполняет пакет данных для элементов управления изображением и текстом, но для другого содержимого потребуется создать собственный пакет данных. Дополнительные сведения см. в разделе [Создание пакета данных](#build-a-data-package).
1. Включите отпускание элемента, добавив объект `DropGestureRecognizer` в его коллекцию `GestureRecognizers`, и выберите для свойства `DropGestureRecognizer.AllowDrop` значение `true`. Дополнительные сведения см. в разделе [Включение отпускания](#enable-drop).
1. [Необязательно] Обработайте событие `DropGestureRecognizer.DragOver`, чтобы указать тип операции, разрешенной целью отпускания. Дополнительные сведения см. в разделе [Обработка события DragOver](#handle-the-dragover-event).
1. [Необязательно] Обработайте пакет данных для получения отпускаемого содержимого. Xamarin.Forms будет автоматически получать изображения и текстовые данные из пакета данных, но для другого содержимого потребуется обработать пакет данных. Дополнительные сведения см. в разделе [Обработка пакета данных](#process-the-data-package).

> [!NOTE]
> Перетаскивание элементов в [`CollectionView`](xref:Xamarin.Forms.CollectionView) в настоящее время не поддерживается.

## <a name="enable-drag"></a>Включение перетаскивания

В Xamarin.Forms распознавание жестов перетаскивания обеспечивается классом `DragGestureRecognizer`. Этот класс определяет приведенные ниже свойства.

- `CanDrag` с типом `bool`, который указывает, может ли элемент, к которому прикреплен распознаватель жестов, быть источником перетаскивания. Значение по умолчанию этого свойства равно `false`.
- `DragStartingCommand` с типом `ICommand`, который выполняется при первом распознавании жеста перетаскивания.
- `DragStartingCommandParameter` с типом `object`, который передается как параметр в `DragStartingCommand`.
- `DropCompletedCommand` с типом `ICommand`, который выполняется при отпускании источника перетаскивания.
- `DropCompletedCommandParameter` с типом `object`, который передается как параметр в `DropCompletedCommand`.

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

Класс `DragGestureRecognizer` также определяет события `DragStarting` и `DropCompleted`. Когда объект `DragGestureRecognizer` обнаруживает жест перетаскивания, он выполняет `DragStartingCommand` и вызывает событие `DragStarting`. Далее, когда объект `DragGestureRecognizer` обнаруживает завершение жеста отпускания, он выполняет `DropCompletedCommand` и вызывает событие `DropCompleted`.

Объект `DragStartingEventArgs`, который прилагается к событию `DragStarting`, содержит приведенные ниже свойства.

- `Handled` с типом `bool` — указывает, обрабатывал ли обработчик событие или следует ли продолжать обработку Xamarin.Forms.
- `Cancel` с типом `bool` — указывает, следует ли отменять событие.
- `Data` с типом `DataPackage` — указывает пакет данных, сопровождающий источник перетаскивания. Это свойство доступно только для чтения.

Объект `DropCompletedEventArgs`, сопровождающий событие `DropCompleted`, имеет свойство `DropResult` только для чтения с типом `DataPackageOperation`. Дополнительные сведения о перечислении `DataPackageOperation` см. в разделе [Обработка события DragOver](#handle-the-dragover-event).

В следующем примере кода XAML показан `DragGestureRecognizer`, присоединенный к [`Image`](xref:Xamarin.Forms.Image):

```xaml
<Image Source="monkeyface.png">
    <Image.GestureRecognizers>
        <DragGestureRecognizer CanDrag="True" />
    </Image.GestureRecognizers>
</Image>
```

В этом примере можно инициировать жест перетаскивания в [`Image`](xref:Xamarin.Forms.Image).

> [!TIP]
> В iOS, Android и UWP жест перетаскивания инициируется с помощью длительного нажатия, за которым следует перетаскивание.

Пример использования команд `DragGestureRecognizer` см. в [образце](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-draganddropgesture/).

## <a name="build-a-data-package"></a>Создание пакета данных

Xamarin.Forms автоматически создаст пакет данных при инициировании перетаскивания для приведенных ниже элементов управления.

- Текстовые элементы управления. Текстовые значения можно перетаскивать из объектов [`CheckBox`](xref:Xamarin.Forms.CheckBox), [`DatePicker`](xref:Xamarin.Forms.DatePicker), [`Editor`](xref:Xamarin.Forms.Editor), [`Entry`](xref:Xamarin.Forms.Entry), [`Label`](xref:Xamarin.Forms.Label), [`RadioButton`](xref:Xamarin.Forms.RadioButton), [`Switch`](xref:Xamarin.Forms.Switch) и [`TimePicker`](xref:Xamarin.Forms.TimePicker).
- Элементы управления изображениями. Изображения можно перетаскивать из элементов управления [`Button`](xref:Xamarin.Forms.Button), [`Image`](xref:Xamarin.Forms.Image) и [`ImageButton`](xref:Xamarin.Forms.ImageButton).

В следующей таблице показаны свойства, которые считываются, и все попытки преобразования, которые выполняются при инициировании перетаскивания текстовых элементов управления.

| Control | Property (Свойство) | Преобразование |
| --- | --- | --- |
| `CheckBox` | `IsChecked` | Параметр `bool`, преобразованный в `string`. |
| `DatePicker` | `Date` | Параметр `DateTime`, преобразованный в `string`. |
| `Editor` | `Text` ||
| `Entry` | `Text` ||
| `Label` | `Text` ||
| `RadioButton` | `IsChecked` | Параметр `bool`, преобразованный в `string`. |
| `Switch` | `IsToggled` | Параметр `bool`, преобразованный в `string`. |
| `TimePicker` | `Time` | Параметр `TimeSpan`, преобразованный в `string`. |

Для содержимого, отличного от текста и изображений, необходимо самостоятельно создать пакет данных.

Пакеты данных представлены классом `DataPackage`, который определяет приведенные ниже свойства.

- `Properties` с типом `DataPackagePropertySet`, который представляет собой коллекцию свойств, составляющих данные, содержащиеся в `DataPackage`. Это свойство доступно только для чтения.
- `Image` с типом [`ImageSource`](xref:Xamarin.Forms.ImageSource), который является изображением, содержащимся в `DataPackage`.
- `Text` с типом `string`, который является текстом, содержащимся в `DataPackage`.
- `View` с типом `DataPackageView`, который является версией `DataPackage` только для чтения.

Класс `DataPackagePropertySet` представляет контейнер свойств, хранящийся как `Dictionary<string,object>`. Дополнительные сведения о классе `DataPackageView` см. в разделе [Обработка пакета данных](#process-the-data-package).

### <a name="store-image-or-text-data"></a>Хранение изображений или текстовых данных

Изображения или текстовые данные могут быть связаны с источником перетаскивания путем сохранения данных в свойстве `DataPackage.Image` или `DataPackage.Text`. Это можно сделать в обработчике события `DragStarting`.

В следующем примере XAML показан `DragGestureRecognizer`, регистрирующий обработчик для события `DragStarting`:

```xaml
<Path Stroke="Black"
      StrokeThickness="4">
    <Path.GestureRecognizers>
        <DragGestureRecognizer CanDrag="True"
                               DragStarting="OnDragStarting" />
    </Path.GestureRecognizers>
    <Path.Data>
        <!-- PathGeometry goes here -->
    </Path.Data>
</Path>
```

В этом примере `DragGestureRecognizer` присоединяется к объекту `Path`. Событие `DragStarting` срабатывает при обнаружении жеста перетаскивания в `Path`, который выполняет обработчик событий `OnDragStarting`:

```csharp
void OnDragStarting(object sender, DragStartingEventArgs e)
{
    e.Data.Text = "My text data goes here";
}
```

Объект `DragStartingEventArgs`, сопровождающий событие `DragStarting`, имеет свойство `Data` с типом `DataPackage`. В этом примере свойству `Text` объекта `DataPackage` присваивается `string`. Затем к `DataPackage` можно получить доступ при отпускании, чтобы получить `string`.

### <a name="store-data-in-the-property-bag"></a>Хранение данных в контейнере свойств

Любые данные, включая изображения и текст, могут быть связаны с источником перетаскивания путем сохранения данных в коллекции `DataPackage.Properties`. Это можно сделать в обработчике события `DragStarting`.

В следующем примере XAML показан `DragGestureRecognizer`, регистрирующий обработчик для события `DragStarting`:

```xaml
<Rectangle Stroke="Red"
           Fill="DarkBlue"
           StrokeThickness="4"
           HeightRequest="200"
           WidthRequest="200">
    <Rectangle.GestureRecognizers>
        <DragGestureRecognizer CanDrag="True"
                               DragStarting="OnDragStarting" />
    </Rectangle.GestureRecognizers>
</Rectangle>
```

В этом примере `DragGestureRecognizer` присоединяется к объекту `Rectangle`. Событие `DragStarting` срабатывает при обнаружении жеста перетаскивания в `Rectangle`, который выполняет обработчик событий `OnDragStarting`:

```csharp
void OnDragStarting(object sender, DragStartingEventArgs e)
{
    Shape shape = (sender as Element).Parent as Shape;
    e.Data.Properties.Add("Square", new Square(shape.Width, shape.Height));
}
```

Объект `DragStartingEventArgs`, сопровождающий событие `DragStarting`, имеет свойство `Data` с типом `DataPackage`. Коллекция `Properties` объекта `DataPackage`, который является коллекцией `Dictionary<string, object>`, можно изменить для хранения необходимых данных. В этом примере словарь `Properties` изменяется для хранения объекта `Square`, который представляет размер `Rectangle` для ключа Square.

## <a name="enable-drop"></a>Включение отпускания

В Xamarin.Forms распознавание жестов отпускания обеспечивается классом `DropGestureRecognizer`. Этот класс определяет приведенные ниже свойства.

- `AllowDrop` с типом `bool`, который указывает, может ли элемент, к которому прикреплен распознаватель жестов, быть источником отпускания. Значение по умолчанию этого свойства равно `false`.
- `DragOverCommand` с типом `ICommand`, который выполняется при перетаскивании источника перетаскивания над целевым объектом отпускания.
- `DragOverCommandParameter` с типом `object`, который передается как параметр в `DragOverCommand`.
- `DropCommand` с типом `ICommand`, который выполняется при отпускании источника перетаскивания над целевым объектом отпускания.
- `DropCommandParameter` с типом `object`, который передается как параметр в `DropCommand`.

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

Класс `DropGestureRecognizer` также определяет события `DragOver` и `Drop`. Когда `DropGestureRecognizer` распознает источник перетаскивания для цели перетаскивания, он выполняет `DragOverCommand` и вызывает событие `DragOver`. Затем, когда `DropGestureRecognizer` распознает источник перетаскивания для цели отпускания, он выполняет `DropCommand` и вызывает событие `Drop`.

Класс `DragEventArgs`, который прилагается к событию `DragOver`, содержит приведенные ниже свойства.

- `Data` с типом `DataPackage`, который содержит данные, связанные с источником перетаскивания. Это свойство доступно только для чтения.
- `AcceptedOperation` с типом `DataPackageOperation`, который указывает, какие операции разрешены целевым объектом отпускания.

Дополнительные сведения о перечислении `DataPackageOperation` см. в разделе [Обработка события DragOver](#handle-the-dragover-event).

Класс `DropEventArgs`, который прилагается к событию `Drop`, содержит приведенные ниже свойства.

- `Data` с типом `DataPackageView`, который является версией пакета данных только для чтения.
- `Handled` с типом `bool` — указывает, обрабатывал ли обработчик событие или следует ли продолжать обработку Xamarin.Forms.

В следующем примере кода XAML показан `DropGestureRecognizer`, присоединенный к [`Image`](xref:Xamarin.Forms.Image):

```xaml
<Image BackgroundColor="Silver"
       HeightRequest="300"
       WidthRequest="250">
    <Image.GestureRecognizers>
        <DropGestureRecognizer AllowDrop="True" />
    </Image.GestureRecognizers>
</Image>
```

В этом примере, когда источник перетаскивания отпускается на целевом объекте отпускания [`Image`](xref:Xamarin.Forms.Image), источник перетаскивания копируется в целевой объект отпускания при условии, что источником перетаскивания является [`ImageSource`](xref:Xamarin.Forms.ImageSource). Это происходит потому, что Xamarin.Forms автоматически копирует перетаскиваемые изображения и текст в совместимые целевые объекты отпускания.

Пример использования команд `DropGestureRecognizer` см. в [образце](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-draganddropgesture/).

## <a name="handle-the-dragover-event"></a>Обработка события DragOver

Событие `DropGestureRecognizer.DragOver` можно дополнительно обработать, чтобы указать, какие типы операций разрешены целевым объектом отпускания. Это можно сделать, задав свойство `AcceptedOperation` с типом `DataPackageOperation` для объекта `DragEventArgs`, сопровождающего событие `DragOver`.

Перечисление `DataPackageOperation` определяет следующие члены:

- `None` указывает, что действие не будет выполнено.
- `Copy`указывает, что содержимое источника перетаскивания будет скопировано в целевой объект отпускания.

> [!IMPORTANT]
> При создании объекта `DragEventArgs` свойству `AcceptedOperation` по умолчанию присвоено значение `DataPackageOperation.Copy`.

В следующем примере XAML показан `DropGestureRecognizer`, регистрирующий обработчик для события `DragOver`:

```xaml
<Image BackgroundColor="Silver"
       HeightRequest="300"
       WidthRequest="250">
    <Image.GestureRecognizers>
        <DropGestureRecognizer AllowDrop="True"
                               DragOver="OnDragOver" />
    </Image.GestureRecognizers>
</Image>
```

В этом примере `DropGestureRecognizer` присоединяется к объекту [`Image`](xref:Xamarin.Forms.Image). Событие `DragOver` срабатывает, когда источник перетаскивания перетаскивается на целевой объект отпускания, но не отпускается, что приводит к выполнению обработчика событий `OnDragOver`:

```csharp
void OnDragOver(object sender, DragEventArgs e)
{
    e.AcceptedOperation = DataPackageOperation.None;
}
```

В этом примере свойству `AcceptedOperation` объекта `DragEventArgs` присваивается `DataPackageOperation.None`. Таким образом, при перемещении источника перетаскивания на целевой объект отпускания никакие действия не выполняются.

## <a name="process-the-data-package"></a>Обработка пакета данных

Событие `Drop` срабатывает при отпускании источника перетаскивания над целевым объектом отпускания. В этом случае Xamarin.Forms автоматически попытается получить данные из пакета данных, когда источник перетаскивания отпускается над приведенными ниже элементами управления.

- Текстовые элементы управления. Текстовые значения можно отпустить над объектами [`CheckBox`](xref:Xamarin.Forms.CheckBox), [`DatePicker`](xref:Xamarin.Forms.DatePicker), [`Editor`](xref:Xamarin.Forms.Editor), [`Entry`](xref:Xamarin.Forms.Entry), [`Label`](xref:Xamarin.Forms.Label), [`RadioButton`](xref:Xamarin.Forms.RadioButton), [`Switch`](xref:Xamarin.Forms.Switch) и [`TimePicker`](xref:Xamarin.Forms.TimePicker).
- Элементы управления изображениями. Изображения можно отпускать над элементами управления [`Button`](xref:Xamarin.Forms.Button), [`Image`](xref:Xamarin.Forms.Image) и [`ImageButton`](xref:Xamarin.Forms.ImageButton).

В следующей таблице показаны свойства, которые указываются, и все попытки преобразования, когда текстовый источник отпускания удаляется в текстовом элементе управления.

| Control | Property (Свойство) | Преобразование |
| --- | --- | --- |
| `CheckBox` | `IsChecked` | `string` преобразуется в `bool`. |
| `DatePicker` | `Date` | `string` преобразуется в `DateTime`. |
| `Editor` | `Text` ||
| `Entry` | `Text` ||
| `Label` | `Text` ||
| `RadioButton` | `IsChecked` | `string` преобразуется в `bool`. |
| `Switch` | `IsToggled` | `string` преобразуется в `bool`. |
| `TimePicker` | `Time` | `string` преобразуется в `TimeSpan`. |

Для содержимого, отличного от текста и изображений, необходимо самостоятельно обработать пакет данных.

Класс `DropEventArgs`, который прилагается к событию `Drop`, указывает свойство `Data` с типом `DataPackageView`. Это свойство является версией пакета данных только для чтения.

### <a name="retrieve-image-or-text-data"></a>Получение изображений или текстовых данных

Изображения или текстовые данные можно получить из пакета данных в обработчике события `Drop` с помощью методов, определенных в классе `DataPackageView`.

Класс `DataPackageView` включает в себя методы `GetImageAsync` и `GetTextAsync`. Метод `GetImageAsync` извлекает изображение из пакета данных, хранимого в свойстве `DataPackage.Image`, и возвращает `Task<ImageSource>`. Метод `GetTextAsync` также извлекает текст из пакета данных, хранимого в свойстве `DataPackage.Text`, и возвращает `Task<string>`.

В следующем примере показан обработчик событий `Drop`, который извлекает текст из пакета данных для `Path`:

```csharp
async void OnDrop(object sender, DropEventArgs e)
{
    string text = await e.Data.GetTextAsync();

    // Perform logic to take action based on the text value.
}
```

В этом примере текстовые данные извлекаются из пакета данных с помощью метода `GetTextAsync`. Затем можно выполнить действие, основанное на текстовом значении.

### <a name="retrieve-data-from-the-property-bag"></a>Извлечение данных из контейнера свойств

Любые данные можно получить из пакета данных в обработчике события `Drop`, обратившись к коллекции `Properties` пакета данных.

Класс `DataPackageView` определяет свойство `Properties` с типом `DataPackagePropertySetView`. Класс `DataPackagePropertySetView` представляет контейнер свойств только для чтения, хранящийся как `Dictionary<string, object>`.

В следующем примере показан обработчик событий `Drop`, который извлекает данные из контейнера свойств пакета данных для `Rectangle`:

```csharp
void OnDrop(object sender, DropEventArgs e)
{
    Square square = (Square)e.Data.Properties["Square"];

    // Perform logic to take action based on retrieved value.
}
```

В этом примере объект `Square` извлекается из контейнера свойств пакета данных путем указания ключа Square словаря. Затем можно выполнить действие, основанное на извлеченном значении.

## <a name="related-links"></a>Связанные ссылки

- [Распознаватель жестов перетаскивания Xamarin.Forms](/samples/xamarin/xamarin-forms-samples/workingwithgestures-draganddropgesture/)
