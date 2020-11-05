---
title: Visual Basic в Xamarin. Android и Xamarin. iOS
description: В этом пошаговом руководстве показано, как создавать собственные приложения Xamarin. iOS и Xamarin. Android, использующие бизнес-логику, написанную на языке Visual Basic.NET.
ms.prod: xamarin
ms.assetid: 455fda67-3879-4299-8036-b12840e6a498
author: davidortinau
ms.author: daortin
ms.date: 04/24/2019
ms.openlocfilehash: 96f8a8c2fa49e1ff59621908ef89d08d302aaffa
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93367976"
---
# <a name="visual-basic-in-xamarin-android-and-ios"></a>Visual Basic в Xamarin Android и iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/mobile-samples/visualbasic-taskyvb/)

Пример приложения [таскивб](/samples/xamarin/mobile-samples/visualbasic-taskyvb/) демонстрирует, как Visual Basic код, скомпилированный в библиотеку .NET Standard, можно использовать с Xamarin. Ниже приведены некоторые снимки экрана приложений, работающих в Android и iOS.

 [![Android и iOS, на которых работает приложение, созданное с помощью Visual Basic](native-apps-images/simulators-sml.png)](native-apps-images/simulators.png#lightbox)

Проекты Android и iOS в примере написаны на языке C#. Пользовательский интерфейс для каждого приложения строится с помощью собственных технологий, а `TodoItem` Управление обеспечивается библиотекой Visual Basic .NET Standard с помощью XML-файла (для демонстрационных целей, а не для полной базы данных).

## <a name="sample-walkthrough"></a>Пример пошагового руководства

В этом руководство объясняется, как Visual Basic реализован в образце Xamarin для iOS и Android в [таскивб](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyVB) .

> [!NOTE]
> Ознакомьтесь с инструкциями по [Visual Basic и .NET Standard](index.md) , прежде чем продолжить работу с этим руководством.
>
> Сведения о создании приложения с общим пользовательским интерфейсом Visual Basic кода см. в статье о [Xamarin. Forms с помощью Visual Basic](xamarin-forms.md) инструкций.

## <a name="visualbasicnetstandard"></a>висуалбасикнетстандард

Visual Basic библиотеки .NET Standard можно создавать только в Visual Studio в Windows.
Пример библиотеки содержит основные сведения о приложении в следующих Visual Basic файлах:

- TodoItem. vb
- TodoItemManager. vb
- Тодоитемрепоситориксмл. vb
- Ксмлстораже. vb

### <a name="todoitemvb"></a>TodoItem. vb

Этот класс содержит бизнес-объект, используемый во всем приложении. Он будет определен в Visual Basic и использоваться в проектах Android и iOS, написанных на языке C#.

Определение класса показано ниже:

```vb
Public Class TodoItem
    Property ID() As Integer
    Property Name() As String
    Property Notes() As String
    Property Done() As Boolean
End Class
```

В примере используется сериализация XML и десериализация для загрузки и сохранения объектов TodoItem.

### <a name="todoitemmanagervb"></a>TodoItemManager. vb

Класс Manager представляет "API" для переносимого кода. Он предоставляет базовые операции CRUD для `TodoItem` класса, но не реализует эти операции.

```vb
Public Class TodoItemManager
    Private _repository As TodoItemRepositoryXML
    Public Sub New(filename As String)
        _repository = New TodoItemRepositoryXML(filename, storage)
    End Sub
    Public Function GetTask(id As Integer) As TodoItem
        Return _repository.GetTask(id)
    End Function
    Public Function GetTasks() As List(Of TodoItem)
        Return New List(Of TodoItem)(_repository.GetTasks())
    End Function
    Public Function SaveTask(item As TodoItem) As Integer
        Return _repository.SaveTask(item)
    End Function
    Public Function DeleteTask(item As TodoItem) As Integer
        Return _repository.DeleteTask(item.ID)
    End Function
End Class
```

Конструктор принимает экземпляр Иксмлстораже в качестве параметра. Это позволяет каждой платформе предоставлять собственную рабочую реализацию, одновременно позволяя переносимому коду описывать другие функциональные возможности, которые можно использовать совместно.

### <a name="todoitemrepositoryvb"></a>Тодоитемрепоситори. vb

Класс репозитория содержит логику для управления списком объектов TodoItem. Полный код показан ниже: логика существует, главным образом, для управления уникальным значением идентификатора в TodoItems по мере их добавления и удаления из коллекции.

```vb
Public Class TodoItemRepositoryXML
    Private _filename As String
    Private _storage As IXmlStorage
    Private _tasks As List(Of TodoItem)

    ''' <summary>Constructor</summary>
    Public Sub New(filename As String)
        _filename = filename
        _storage = New XmlStorage
        _tasks = _storage.ReadXml(filename)
    End Sub
    ''' <summary>Inefficient search for a Task by ID</summary>
    Public Function GetTask(id As Integer) As TodoItem
        For t As Integer = 0 To _tasks.Count - 1
            If _tasks(t).ID = id Then
                Return _tasks(t)
            End If
        Next
        Return New TodoItem() With {.ID = id}
    End Function
    ''' <summary>List all the Tasks</summary>
    Public Function GetTasks() As IEnumerable(Of TodoItem)
        Return _tasks
    End Function
    ''' <summary>Save a Task to the Xml file
    ''' Calculates the ID as the max of existing IDs</summary>
    Public Function SaveTask(item As TodoItem) As Integer
        Dim max As Integer = 0
        If _tasks.Count > 0 Then
            max = _tasks.Max(Function(t As TodoItem) t.ID)
        End If
        If item.ID = 0 Then
            item.ID = ++max
            _tasks.Add(item)
        Else
            Dim j = _tasks.Where(Function(t) t.ID = item.ID).First()
            j = item
        End If
        _storage.WriteXml(_tasks, _filename)
        Return max
    End Function
    ''' <summary>Removes the task from the XMl file</summary>
    Public Function DeleteTask(id As Integer) As Integer
        For t As Integer = 0 To _tasks.Count - 1
            If _tasks(t).ID = id Then
                _tasks.RemoveAt(t)
                _storage.WriteXml(_tasks, _filename)
                Return 1
            End If
        Next
        Return -1
    End Function
End Class
```

> [!NOTE]
> Этот код является примером очень простого механизма хранения данных.
> Он предназначен для демонстрации того, как библиотека .NET Standard может программировать интерфейс для доступа к функциональным возможностям платформы (в данном случае загрузка и сохранение XML-файла). Он не предназначен для альтернативного качества базы данных.

## <a name="android-and-ios-application-projects"></a>Проекты приложений для Android и iOS

### <a name="ios"></a>iOS

В приложении iOS приложения `TodoItemManager` и `XmlStorageImplementation` создаются в файле **AppDelegate.CS** , как показано в этом фрагменте кода. Первые четыре строки — это просто создание пути к файлу, в котором будут храниться данные; в последних двух строках показаны два создаваемых класса.

```csharp
var xmlFilename = "TodoList.xml";
string documentsPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine(documentsPath, "..", "Library"); // Library folder
var path = Path.Combine(libraryPath, xmlFilename);

TaskMgr = new TodoItemManager(path);
```

### <a name="android"></a>Android

В приложении Android приложения `TodoItemManager` и `XmlStorageImplementation` создаются в файле **Application.CS** , как показано в этом фрагменте кода. Первые три строки — это просто создание пути к файлу, в котором будут храниться данные. в последних двух строках показаны два создаваемых класса.

```csharp
var xmlFilename = "TodoList.xml";
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
var path = Path.Combine(libraryPath, xmlFilename);

TaskMgr = new TodoItemManager(path);
```

Остальная часть кода приложения в первую очередь касается пользовательского интерфейса и использования `TaskMgr` класса для загрузки и сохранения `TodoItem` классов.

## <a name="visual-studio-2019-for-mac"></a>Visual Studio 2019 для Mac

> [!WARNING]
> Visual Studio для Mac не поддерживает редактирование языка Visual Basic — нет пунктов меню для создания Visual Basic проектов или файлов. При открытии **. vb** отсутствует выделение синтаксиса языка, Автозаполнение или IntelliSense.

Visual Studio 2019 для Mac _может_ компилировать проекты .NET Standard Visual Studio, созданные в Windows, поэтому приложения iOS могут ссылаться на эти проекты.

Visual Studio 2017 _не может_ создавать проекты Visual Basic.

## <a name="summary"></a>Сводка

В этой статье показано, как использовать код Visual Basic в приложениях Xamarin с помощью Visual Studio и библиотек .NET Standard. Несмотря на то, что Xamarin не поддерживает Visual Basic напрямую, компиляция Visual Basic в библиотеку .NET Standard позволяет включать код, написанный с Visual Basic, в приложения iOS и Android.

## <a name="related-links"></a>Связанные ссылки

- [Таскивб (пример .NET Standard)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyVB)
- [Новые возможности .NET Standard](/dotnet/standard/whats-new/whats-new-in-dotnet-standard?tabs=csharp)