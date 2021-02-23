---
title: Хранение данных в локальной базе данных SQLite.NET
description: В этой статье описывается, как хранить данные в локальной базе данных SQLite.NET из приложения Оболочки в Xamarin.Forms.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: F669CE4E-1E1B-409F-BC1C-8364633EB7EF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.custom: contperf-fy21q3
ms.date: 01/28/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cfc479e6211692f3de7d78e2fd52cd80a2630660
ms.sourcegitcommit: 1f391667869a4541dd9b42d78862dc01d69ed160
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99818408"
---
# <a name="store-data-in-a-local-sqlitenet-database"></a>Хранение данных в локальной базе данных SQLite.NET

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)

В этом кратком руководстве рассматриваются следующие темы:

- Локальное хранение данных в базе данных SQLite.NET.

В этом кратком руководстве объясняется, как хранить данные в локальной базе данных SQLite.NET из приложения Оболочки в Xamarin.Forms. Ниже показано итоговое приложение:

[![Страница заметок](database-images/screenshots1-sml.png)](database-images/screenshots1.png#lightbox)
[![Страница ввода заметки](database-images/screenshots2-sml.png)](database-images/screenshots2.png#lightbox)

## <a name="prerequisites"></a>Предварительные требования

Прежде чем приступать к этому краткому руководству, необходимо успешно завершить [предыдущее](navigation.md). Также вы можете скачать [пример из предыдущего краткого руководства](/samples/xamarin/xamarin-forms-samples/getstarted-notes-navigation/) и использовать его в качестве отправной точки для работы с этим руководством.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Обновление приложения с помощью Visual Studio

1. Запустите Visual Studio и откройте решение Notes.

2. В **обозревателе решений** щелкните правой кнопкой мыши решение **Notes** и выберите **Manage NuGet Packages for Solution...** (Управление пакетами NuGet для решения...):

    ![Управление пакетами NuGet](database-images/vs/manage-nuget-packages.png)    

3. В разделе **Диспетчер пакетов NuGet** выберите вкладку **Обзор** и найдите пакет NuGet **sqlite-net-pcl**.

    > [!WARNING]
    > Существует несколько пакетов NuGet с похожими названиями. Правильный пакет имеет следующие атрибуты:
    > - **Авторы:** SQLite-net
    > - **Ссылка NuGet:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > Несмотря на название, этот пакет NuGet можно использовать в проектах .NET Standard.

    В разделе **Диспетчер пакетов NuGet** выберите правильный пакет **sqlite-net-pcl**, установите флажок **Проект** и нажмите кнопку **Установить**, чтобы добавить его в решение.

    ![Выбор sqlite-net-pcl](database-images/vs/select-package.png)

    Этот пакет будет использоваться для включения в приложение операций с базами данных и будет добавлен в каждый проект решения.

    Закройте **Диспетчер пакетов NuGet**.

4. В **обозревателе решений** выберите проект **Notes** и откройте файл **Note.cs** в папке **Models**, а затем замените существующий код следующим:

    ```csharp
    using System;
    using SQLite;

    namespace Notes.Models
    {
        public class Note
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    Этот класс определяет модель `Note`, где будут храниться данные о каждой заметке в этом приложении. Свойство `ID` помечено атрибутами `PrimaryKey` и `AutoIncrement`, чтобы каждый экземпляр `Note` в базе данных SQLite.NET имел уникальный идентификатор, предоставленный SQLite.NET.

    Сохраните изменения в **Note.cs**, нажав клавиши **CTRL+S**.

    > [!WARNING]
    > В данный момент сборка приложения не будет выполнена из-за ошибок, которые будут исправлены в последующих шагах.

5. В **обозревателе решений** добавьте новую папку с именем **Data** в проект **Notes**.

6. В **обозревателе решений** выберите проект **Notes** и добавьте новый класс с именем **NoteDatabase** в папку **Data**.

7. Замените содержимое файла **NoteDatabase.cs** следующим кодом:

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;
    using Notes.Models;

    namespace Notes.Data
    {
        public class NoteDatabase
        {
            readonly SQLiteAsyncConnection database;

            public NoteDatabase(string dbPath)
            {
                database = new SQLiteAsyncConnection(dbPath);
                database.CreateTableAsync<Note>().Wait();
            }

            public Task<List<Note>> GetNotesAsync()
            {
                //Get all notes.
                return database.Table<Note>().ToListAsync();
            }

            public Task<Note> GetNoteAsync(int id)
            {
                // Get a specific note.
                return database.Table<Note>()
                                .Where(i => i.ID == id)
                                .FirstOrDefaultAsync();
            }

            public Task<int> SaveNoteAsync(Note note)
            {
                if (note.ID != 0)
                {
                    // Update an existing note.
                    return database.UpdateAsync(note);
                }
                else
                {
                    // Save a new note.
                    return database.InsertAsync(note);
                }
            }

            public Task<int> DeleteNoteAsync(Note note)
            {
                // Delete a note.
                return database.DeleteAsync(note);
            }
        }
    }
    ```

    Этот класс содержит код, чтобы создать базу данных, считывать и записывать данные в ней, а также удалять данные из нее. В коде используются асинхронные API-интерфейсы SQLite.NET, которые перемещают операции базы данных в фоновые потоки. Кроме того конструктор `NoteDatabase` принимает путь файла базы данных в качестве аргумента. Этот путь будет предоставлен классом `App` в следующем шаге.

    Сохраните изменения в **NoteDatabase.cs**, нажав клавиши **CTRL+S**.  

    > [!WARNING]
    > В данный момент сборка приложения не будет выполнена из-за ошибок, которые будут исправлены в последующих шагах.

8. В **обозревателе решений** в проекте **Notes** разверните **App.xaml** и дважды щелкните файл **App.xaml.cs**, чтобы открыть его: Затем замените существующий код следующим:

    ```csharp
    using System;
    using System.IO;
    using Notes.Data;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class App : Application
        {
            static NoteDatabase database;

            // Create the database connection as a singleton.
            public static NoteDatabase Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new NoteDatabase(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "Notes.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();
                MainPage = new AppShell();
            }

            protected override void OnStart()
            {
            }

            protected override void OnSleep()
            {
            }

            protected override void OnResume()
            {
            }
        }
    }
    ```

    Этот код определяет свойство `Database`, которое создает экземпляр `NoteDatabase` в качестве отдельной базы данных, передавая имя файла базы данных в качестве аргумента в конструктор `NoteDatabase`. Преимущество использования отдельной базы данных в том, что создается отдельное подключение к базе данных, которое остается открытым, пока работает приложение. Это позволяет избежать затрат, связанных с открытием и закрытием файла базы данных каждый раз, когда выполняется операция с ней.

    Сохраните изменения в файле **App.xaml.cs**, нажав клавиши **CTRL+S**.  

    > [!WARNING]
    > В данный момент сборка приложения не будет выполнена из-за ошибок, которые будут исправлены в последующих шагах.

9. В **обозревателе решений** в проекте **Notes** разверните **NotesPage.xaml** в папке **Views** и откройте **NotesPage.xaml.cs**. Затем замените методы `OnAppearing` и `OnSelectionChanged` следующим кодом:

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        // Retrieve all the notes from the database, and set them as the
        // data source for the CollectionView.
        collectionView.ItemsSource = await App.Database.GetNotesAsync();
    }

    async void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        if (e.CurrentSelection != null)
        {
            // Navigate to the NoteEntryPage, passing the ID as a query parameter.
            Note note = (Note)e.CurrentSelection.FirstOrDefault();
            await Shell.Current.GoToAsync($"{nameof(NoteEntryPage)}?{nameof(NoteEntryPage.ItemId)}={note.ID.ToString()}");
        }
    }    
    ```    

    Метод `OnAppearing` заполняет [`CollectionView`](xref:Xamarin.Forms.CollectionView) любыми заметками, хранящимися в базе данных. Метод `OnSelectionChanged` переходит к объекту `NoteEntryPage`, передавая свойство `ID` выбранного объекта `Note` в качестве параметра запроса.

    Сохраните изменения в файле **NotesPage.xaml.cs**, нажав клавиши **CTRL+S**.  

    > [!WARNING]
    > В данный момент сборка приложения не будет выполнена из-за ошибок, которые будут исправлены в последующих шагах.

10. В **обозревателе решений** разверните **NoteEntryPage.xaml** в папке **Views** и откройте **NoteEntryPage.xaml.cs**. Затем замените методы `LoadNote`, `OnSaveButtonClicked` и `OnDeleteButtonClicked` следующим кодом:

      ```csharp
      async void LoadNote(string itemId)
      {
          try
          {
              int id = Convert.ToInt32(itemId);
              // Retrieve the note and set it as the BindingContext of the page.
              Note note = await App.Database.GetNoteAsync(id);
              BindingContext = note;
          }
          catch (Exception)
          {
              Console.WriteLine("Failed to load note.");
          }
      }

      async void OnSaveButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          note.Date = DateTime.UtcNow;
          if (!string.IsNullOrWhiteSpace(note.Text))
          {
              await App.Database.SaveNoteAsync(note);
          }

          // Navigate backwards
          await Shell.Current.GoToAsync("..");
      }

      async void OnDeleteButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          await App.Database.DeleteNoteAsync(note);

          // Navigate backwards
          await Shell.Current.GoToAsync("..");
      }
      ```    

      `NoteEntryPage` использует метод `LoadNote` для получения заметки из базы данных, идентификатор которой был передан на страницу в качестве параметра запроса, и сохраняет его в виде объекта `Note` в[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы. При выполнении обработчика событий `OnSaveButtonClicked` экземпляр `Note` сохраняется в базе данных, и приложение возвращается на предыдущую страницу. При выполнении обработчика событий `OnDeleteButtonClicked` экземпляр `Note` удаляется из базы данных, и приложение возвращается на предыдущую страницу.

      Сохраните изменения в файле **NoteEntryPage.xaml.cs**, нажав клавиши **CTRL+S**.  

11. Создайте и запустите проект на каждой соответствующей платформе. Дополнительные сведения см. в разделе [Сборка примера из краткого руководства](app.md#building-the-quickstart).

    На странице **NotesPage** нажмите кнопку **Добавить**, чтобы перейти к странице **NoteEntryPage** и ввести заметку. После сохранения заметки приложение вернется на страницу **NotesPage**.

    Введите несколько заметок разной длины, чтобы понаблюдать за поведением приложения. Закройте приложение и повторно запустите его, чтобы проверить, сохранены ли в базе данных введенные заметки.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Обновление приложения с помощью Visual Studio для Mac

1. Запустите Visual Studio для Mac и откройте решение Notes.

2. На **Панели решения** щелкните правой кнопкой мыши решение **Notes** и выберите **Manage NuGet Packages...** (Управление пакетами NuGet...):

    ![Управление пакетами NuGet](database-images/vsmac/manage-nuget-packages.png)    

3. В разделе **Manage NuGet Packages** (Управление пакетами NuGet) выберите вкладку **Обзор** и найдите пакет NuGet **sqlite-net-pcl**.

    > [!WARNING]
    > Существует несколько пакетов NuGet с похожими названиями. Правильный пакет имеет следующие атрибуты:
    > - **Авторы:** SQLite-net
    > - **Ссылка NuGet:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > Несмотря на название, этот пакет NuGet можно использовать в проектах .NET Standard.

    В диалоговом окне **Manage NuGet Packages** (Управление пакетами NuGet) выберите пакет **sqlite-net-pcl** и нажмите кнопку **Добавить пакет**, чтобы добавить его в решение:

      ![Выбор sqlite-net-pcl](database-images/vsmac/select-package.png)

    Этот пакет будет использоваться для включения операций базы данных в приложение.

4. В диалоговом окне **Выбор проектов** установите все флажки и нажмите кнопку **ОК**:

    ![Добавление пакета во все проекты](database-images/vsmac/add-package.png)

    В результате этого пакет NuGet будет добавлен в каждый проект в решении.

5. На **Панели решения** выберите проект **Notes** и откройте файл **Note.cs** в папке **Models**, а затем замените существующий код следующим:

    ```csharp
    using System;
    using SQLite;

    namespace Notes.Models
    {
        public class Note
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    Этот класс определяет модель `Note`, где будут храниться данные о каждой заметке в этом приложении. Свойство `ID` помечено атрибутами `PrimaryKey` и `AutoIncrement`, чтобы каждый экземпляр `Note` в базе данных SQLite.NET имел уникальный идентификатор, предоставленный SQLite.NET.

    Сохраните изменения в файле **Note.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

    > [!WARNING]
    > В данный момент сборка приложения не будет выполнена из-за ошибок, которые будут исправлены в последующих шагах.

6. На **Панели решения** добавьте новую папку с именем **Data** в проект **Notes**.

7. На **Панели решения** выберите проект **Notes** и добавьте новый класс с именем **NoteDatabase** в папку **Data**.

8. Замените содержимое файла **NoteDatabase.cs** следующим кодом:

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;
    using Notes.Models;

    namespace Notes.Data
    {
        public class NoteDatabase
        {
            readonly SQLiteAsyncConnection database;

            public NoteDatabase(string dbPath)
            {
                database = new SQLiteAsyncConnection(dbPath);
                database.CreateTableAsync<Note>().Wait();
            }

            public Task<List<Note>> GetNotesAsync()
            {
                //Get all notes.
                return database.Table<Note>().ToListAsync();
            }

            public Task<Note> GetNoteAsync(int id)
            {
                // Get a specific note.
                return database.Table<Note>()
                                .Where(i => i.ID == id)
                                .FirstOrDefaultAsync();
            }

            public Task<int> SaveNoteAsync(Note note)
            {
                if (note.ID != 0)
                {
                    // Update an existing note.
                    return database.UpdateAsync(note);
                }
                else
                {
                    // Save a new note.
                    return database.InsertAsync(note);
                }
            }

            public Task<int> DeleteNoteAsync(Note note)
            {
                // Delete a note.
                return database.DeleteAsync(note);
            }
        }
    }
    ```

    Этот класс содержит код, чтобы создать базу данных, считывать и записывать данные в ней, а также удалять данные из нее. В коде используются асинхронные API-интерфейсы SQLite.NET, которые перемещают операции базы данных в фоновые потоки. Кроме того конструктор `NoteDatabase` принимает путь файла базы данных в качестве аргумента. Этот путь будет предоставлен классом `App` в следующем шаге.

    Сохраните изменения в файле **NoteDatabase.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

    > [!WARNING]
    > В данный момент сборка приложения не будет выполнена из-за ошибок, которые будут исправлены в последующих шагах.

9. На **Панели решения** в проекте **Notes** разверните **App.xaml** и дважды щелкните файл **App.xaml.cs**, чтобы открыть его: Затем замените существующий код следующим:

    ```csharp
    using System;
    using System.IO;
    using Notes.Data;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class App : Application
        {
            static NoteDatabase database;

            // Create the database connection as a singleton.
            public static NoteDatabase Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new NoteDatabase(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "Notes.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();
                MainPage = new AppShell();
            }

            protected override void OnStart()
            {
            }

            protected override void OnSleep()
            {
            }

            protected override void OnResume()
            {
            }
        }
    }
    ```

    Этот код определяет свойство `Database`, которое создает экземпляр `NoteDatabase` в качестве отдельной базы данных, передавая имя файла базы данных в качестве аргумента в конструктор `NoteDatabase`. Преимущество использования отдельной базы данных в том, что создается отдельное подключение к базе данных, которое остается открытым, пока работает приложение. Это позволяет избежать затрат, связанных с открытием и закрытием файла базы данных каждый раз, когда выполняется операция с ней.

    Сохраните изменения в файле **App.xaml.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

    > [!WARNING]
    > В данный момент сборка приложения не будет выполнена из-за ошибок, которые будут исправлены в последующих шагах.

10. На **Панели решения** в проекте **Notes** разверните **NotesPage.xaml** в папке **Views** и откройте **NotesPage.xaml.cs**. Затем замените методы `OnAppearing` и `OnSelectionChanged` следующим кодом:

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        // Retrieve all the notes from the database, and set them as the
        // data source for the CollectionView.
        collectionView.ItemsSource = await App.Database.GetNotesAsync();
    }

    async void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        if (e.CurrentSelection != null)
        {
            // Navigate to the NoteEntryPage, passing the ID as a query parameter.
            Note note = (Note)e.CurrentSelection.FirstOrDefault();
            await Shell.Current.GoToAsync($"{nameof(NoteEntryPage)}?{nameof(NoteEntryPage.ItemId)}={note.ID.ToString()}");
        }
    }    
    ```    

    Метод `OnAppearing` заполняет [`CollectionView`](xref:Xamarin.Forms.CollectionView) любыми заметками, хранящимися в базе данных. Метод `OnSelectionChanged` переходит к объекту `NoteEntryPage`, передавая свойство `ID` выбранного объекта `Note` в качестве параметра запроса.

    Сохраните изменения в файле **NotesPage.xaml.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

    > [!WARNING]
    > В данный момент сборка приложения не будет выполнена из-за ошибок, которые будут исправлены в последующих шагах.

11. На **Панели решения** разверните **NoteEntryPage.xaml** в папке **Views** и откройте **NoteEntryPage.xaml.cs**. Затем замените методы `LoadNote`, `OnSaveButtonClicked` и `OnDeleteButtonClicked` следующим кодом:

      ```csharp
      async void LoadNote(string itemId)
      {
          try
          {
              int id = Convert.ToInt32(itemId);
              // Retrieve the note and set it as the BindingContext of the page.
              Note note = await App.Database.GetNoteAsync(id);
              BindingContext = note;
          }
          catch (Exception)
          {
              Console.WriteLine("Failed to load note.");
          }
      }

      async void OnSaveButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          note.Date = DateTime.UtcNow;
          if (!string.IsNullOrWhiteSpace(note.Text))
          {
              await App.Database.SaveNoteAsync(note);
          }

          // Navigate backwards
          await Shell.Current.GoToAsync("..");
      }

      async void OnDeleteButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          await App.Database.DeleteNoteAsync(note);

          // Navigate backwards
          await Shell.Current.GoToAsync("..");
      }
      ```    

      `NoteEntryPage` использует метод `LoadNote` для получения заметки из базы данных, идентификатор которой был передан на страницу в качестве параметра запроса, и сохраняет его в виде объекта `Note` в[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы. При выполнении обработчика событий `OnSaveButtonClicked` экземпляр `Note` сохраняется в базе данных, и приложение возвращается на предыдущую страницу. При выполнении обработчика событий `OnDeleteButtonClicked` экземпляр `Note` удаляется из базы данных, и приложение возвращается на предыдущую страницу.

      Сохраните изменения в файле **NoteEntryPage.xaml.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

12. Создайте и запустите проект на каждой соответствующей платформе. Дополнительные сведения см. в разделе [Сборка примера из краткого руководства](app.md#building-the-quickstart).

    На странице **NotesPage** нажмите кнопку **Добавить**, чтобы перейти к странице **NoteEntryPage** и ввести заметку. После сохранения заметки приложение вернется на страницу **NotesPage**.

    Введите несколько заметок разной длины, чтобы понаблюдать за поведением приложения. Закройте приложение и повторно запустите его, чтобы проверить, сохранены ли в базе данных введенные заметки.

::: zone-end

## <a name="next-steps"></a>Дальнейшие действия

В этом кратком руководстве рассматривались следующие темы:

- Локальное хранение данных в базе данных SQLite.NET.

Перейдите к следующему краткому руководству, чтобы стилизовать приложение с помощью стилей XAML.

> [!div class="nextstepaction"]
> [Вперед](styling.md)

## <a name="related-links"></a>Связанные ссылки

- [Заметки (пример)](/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)
- [Подробное изучение кратких руководств по Оболочке в Xamarin.Forms](deepdive.md)
