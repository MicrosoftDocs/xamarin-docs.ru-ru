---
title: "Хранение данных в локальной базе данных SQLite.NET" description: "Эта статья описывает, как хранить данные в локальной базе данных SQLite.NET".
zone_pivot_groups: platform ms.topic: quickstart ms.prod: xamarin ms.assetid: 5BF901BD-FDE8-4B74-B4AB-418E81745A3B ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 04/01/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="store-data-in-a-local-sqlitenet-database"></a>Хранение данных в локальной базе данных SQLite.NET

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)

В этом кратком руководстве рассматриваются следующие темы:

- Использование диспетчер пакетов NuGet для добавления пакета NuGet в проект.
- Локальное хранение данных в базе данных SQLite.NET.

Это краткое руководство описывает, как хранить данные в локальной базе данных SQLite.NET. Ниже показано итоговое приложение:

[![](database-images/screenshots1-sml.png "Notes Page")](database-images/screenshots1.png#lightbox "Notes Page")
[![](database-images/screenshots2-sml.png "Note Entry Page")](database-images/screenshots2.png#lightbox "Note Entry Page")

## <a name="prerequisites"></a>Предварительные требования

Прежде чем приступать к этому краткому руководству, необходимо успешно завершить [предыдущее](multi-page.md). Также вы можете скачать [пример из предыдущего краткого руководства](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/) и использовать его в качестве отправной точки для работы с этим руководством.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Обновление приложения с помощью Visual Studio

1. Запустите Visual Studio и откройте решение Notes.

2. В **обозревателе решений** выберите проект **Notes**, щелкните правой кнопкой мыши и выберите **Управление пакетами NuGet...** :

    ![](database-images/vs/add-nuget-packages.png "Add NuGet Packages")    

3. В разделе **Диспетчер пакетов NuGet** выберите вкладку **Обзор**, выполните поиск пакета NuGet **sqlite-net-pcl**, выберите его и нажмите кнопку **Установить**, чтобы добавить его в проект.

    ![](database-images/vs/add-package.png "Add Package")

    > [!NOTE]
    > Существует ряд пакетов NuGet с похожими названиями. Правильный пакет имеет следующие атрибуты:
    > - **Автор(ы)** : Фрэнк А. Крюгер (Frank A. Krueger)
    > - **Идентификатор:** sqlite-net-pcl
    > - **Ссылка NuGet:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > Несмотря на название, этот пакет NuGet можно использовать в проектах .NET Standard.

    Этот пакет будет использоваться для включения операций базы данных в приложение.

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

    Сохраните изменения в **Note.cs**, нажав клавиши **CTRL+S**, и закройте файл.

    > [!WARNING]
    > Попытка выполнить сборку приложения на этом этапе приведет к ошибкам, которые будут исправлены позже.

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
            readonly SQLiteAsyncConnection _database;

            public NoteDatabase(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Note>().Wait();
            }

            public Task<List<Note>> GetNotesAsync()
            {
                return _database.Table<Note>().ToListAsync();
            }

            public Task<Note> GetNoteAsync(int id)
            {
                return _database.Table<Note>()
                                .Where(i => i.ID == id)
                                .FirstOrDefaultAsync();
            }

            public Task<int> SaveNoteAsync(Note note)
            {
                if (note.ID != 0)
                {
                    return _database.UpdateAsync(note);
                }
                else
                {
                    return _database.InsertAsync(note);
                }
            }

            public Task<int> DeleteNoteAsync(Note note)
            {
                return _database.DeleteAsync(note);
            }
        }
    }
    ```

    Этот класс содержит код, чтобы создать базу данных, считывать и записывать данные в ней, а также удалять данные из нее. В коде используются асинхронные API-интерфейсы SQLite.NET, которые перемещают операции базы данных в фоновые потоки. Кроме того конструктор `NoteDatabase` принимает путь файла базы данных в качестве аргумента. Этот путь будет предоставлен классом `App` в следующем шаге.

    Сохраните изменения в **NoteDatabase.cs**, нажав клавиши **CTRL+S**, и закройте файл.

    > [!WARNING]
    > Попытка выполнить сборку приложения на этом этапе приведет к ошибкам, которые будут исправлены позже.

8. В **обозревателе решений** дважды щелкните файл **App.xaml.cs** в проекте **Notes**, чтобы открыть его. Затем замените существующий код следующим:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;
    using Notes.Data;

    namespace Notes
    {
        public partial class App : Application
        {
            static NoteDatabase database;

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
                MainPage = new NavigationPage(new NotesPage());
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    Этот код определяет свойство `Database`, которое создает экземпляр `NoteDatabase` в качестве отдельной базы данных, передавая имя файла базы данных в качестве аргумента в конструктор `NoteDatabase`. Преимущество использования отдельной базы данных в том, что создается отдельное подключение к базе данных, которое остается открытым, пока работает приложение. Это позволяет избежать затрат, связанных с открытием и закрытием файла базы данных каждый раз, когда выполняется операция с ней.

    Сохраните изменения в файле **App.xaml.cs**, нажав клавиши **CTRL+S**, и закройте файл.

    > [!WARNING]
    > Попытка выполнить сборку приложения на этом этапе приведет к ошибкам, которые будут исправлены позже.

9. В **обозревателе решений** дважды щелкните файл **NotesPage.xaml.cs** в проекте **Notes**, чтобы открыть его. Замените метод `OnAppearing` следующим кодом:

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    Этот код заполняет [`ListView`](xref:Xamarin.Forms.ListView) заметками, которые хранятся в базе данных.

    Сохраните изменения в файле **NotesPage.xaml.cs**, нажав клавиши **CTRL+S**, и закройте файл.

    > [!WARNING]
    > Попытка выполнить сборку приложения на этом этапе приведет к ошибкам, которые будут исправлены позже.

10. В **обозревателе решений** дважды щелкните файл **NoteEntryPage.xaml.cs**, чтобы открыть его. Затем замените методы `OnSaveButtonClicked` и `OnDeleteButtonClicked` следующим кодом:

      ```csharp
      async void OnSaveButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          note.Date = DateTime.UtcNow;
          await App.Database.SaveNoteAsync(note);
          await Navigation.PopAsync();
      }

      async void OnDeleteButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          await App.Database.DeleteNoteAsync(note);
          await Navigation.PopAsync();
      }
      ```    

      `NoteEntryPage` сохраняет экземпляр `Note`, представляющий одну заметку, в [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы. При выполнении обработчика событий `OnSaveButtonClicked` экземпляр `Note` сохраняется в базе данных, и приложение возвращается на предыдущую страницу. При выполнении обработчика событий `OnDeleteButtonClicked` экземпляр `Note` удаляется из базы данных, и приложение возвращается на предыдущую страницу.

      Сохраните изменения в файле **NoteEntryPage.xaml.cs**, нажав клавиши **CTRL+S**, и закройте файл.

11. Создайте и запустите проект на каждой соответствующей платформе. Дополнительные сведения см. в разделе [Сборка примера из краткого руководства](single-page.md#building-the-quickstart).

    На странице **NotesPage** нажмите кнопку **+** , чтобы перейти к странице **NoteEntryPage** и ввести заметку. После сохранения заметки приложение вернется на страницу **NotesPage**.

    Введите несколько заметок разной длины, чтобы понаблюдать за поведением приложения.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Обновление приложения с помощью Visual Studio для Mac

1. Запустите Visual Studio для Mac и откройте проект Notes.

2. На **Панели решения** выберите проект **Notes**, щелкните правой кнопкой мыши и выберите **Добавить > Добавить пакеты NuGet...** :

    ![](database-images/vsmac/add-nuget-packages.png "Add NuGet Packages")    

3. В окне **Добавление пакетов** выполните поиск пакета NuGet **sqlite-net-pcl**, выберите его и нажмите кнопку **Добавить пакет**, чтобы добавить его в проект.

    ![](database-images/vsmac/add-package.png "Add Package")

    > [!NOTE]
    > Существует ряд пакетов NuGet с похожими названиями. Правильный пакет имеет следующие атрибуты:
    > - **Автор**: Фрэнк А. Крюгер (Frank A. Krueger)
    > - **Идентификатор:** sqlite-net-pcl
    > - **Ссылка NuGet:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > Несмотря на название, этот пакет NuGet можно использовать в проектах .NET Standard.

    Этот пакет будет использоваться для включения операций базы данных в приложение.

4. На **Панели решения** выберите проект **Notes** и откройте файл **Note.cs** в папке **Models**, а затем замените существующий код следующим:

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

    Сохраните изменения в **Note.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**, и закройте файл.

    > [!WARNING]
    > Попытка выполнить сборку приложения на этом этапе приведет к ошибкам, которые будут исправлены позже.

5. На **Панели решения** добавьте новую папку с именем **Data** в проект **Notes**.

6. На **Панели решения** выберите проект **Notes** и добавьте новый класс с именем **NoteDatabase** в папку **Data**.

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
            readonly SQLiteAsyncConnection _database;

            public NoteDatabase(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Note>().Wait();
            }

            public Task<List<Note>> GetNotesAsync()
            {
                return _database.Table<Note>().ToListAsync();
            }

            public Task<Note> GetNoteAsync(int id)
            {
                return _database.Table<Note>()
                                .Where(i => i.ID == id)
                                .FirstOrDefaultAsync();
            }

            public Task<int> SaveNoteAsync(Note note)
            {
                if (note.ID != 0)
                {
                    return _database.UpdateAsync(note);
                }
                else
                {
                    return _database.InsertAsync(note);
                }
            }

            public Task<int> DeleteNoteAsync(Note note)
            {
                return _database.DeleteAsync(note);
            }
        }
    }
    ```

    Этот класс содержит код, чтобы создать базу данных, считывать и записывать данные в ней, а также удалять данные из нее. В коде используются асинхронные API-интерфейсы SQLite.NET, которые перемещают операции базы данных в фоновые потоки. Кроме того конструктор `NoteDatabase` принимает путь файла базы данных в качестве аргумента. Этот путь будет предоставлен классом `App` в следующем шаге.

    Сохраните изменения в **NoteDatabase.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**, и закройте файл.

    > [!WARNING]
    > Попытка выполнить сборку приложения на этом этапе приведет к ошибкам, которые будут исправлены позже.

8. На **Панели решения** дважды щелкните файл **App.xaml.cs** в проекте **Notes**, чтобы открыть его. Затем замените существующий код следующим:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;
    using Notes.Data;

    namespace Notes
    {
        public partial class App : Application
        {
            static NoteDatabase database;

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
                MainPage = new NavigationPage(new NotesPage());
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    Этот код определяет свойство `Database`, которое создает экземпляр `NoteDatabase` в качестве отдельной базы данных, передавая имя файла базы данных в качестве аргумента в конструктор `NoteDatabase`. Преимущество использования отдельной базы данных в том, что создается отдельное подключение к базе данных, которое остается открытым, пока работает приложение. Это позволяет избежать затрат, связанных с открытием и закрытием файла базы данных каждый раз, когда выполняется операция с ней.

    Сохраните изменения в файле **App.xaml.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**, и закройте файл.

    > [!WARNING]
    > Попытка выполнить сборку приложения на этом этапе приведет к ошибкам, которые будут исправлены позже.

9. На **Панели решения** дважды щелкните файл **NotesPage.xaml.cs** в проекте **Notes**, чтобы открыть его. Замените метод `OnAppearing` следующим кодом:

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    Этот код заполняет [`ListView`](xref:Xamarin.Forms.ListView) заметками, которые хранятся в базе данных.

    Сохраните изменения в **NotesPage.xaml.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**, и закройте файл.

    > [!WARNING]
    > Попытка выполнить сборку приложения на этом этапе приведет к ошибкам, которые будут исправлены позже.

10. На **Панели решения** дважды щелкните файл **NoteEntryPage.xaml.cs**, чтобы открыть его. Затем замените методы `OnSaveButtonClicked` и `OnDeleteButtonClicked` следующим кодом:

      ```csharp
      async void OnSaveButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          note.Date = DateTime.UtcNow;
          await App.Database.SaveNoteAsync(note);
          await Navigation.PopAsync();
      }

      async void OnDeleteButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          await App.Database.DeleteNoteAsync(note);
          await Navigation.PopAsync();
      }
      ```    

      `NoteEntryPage` сохраняет экземпляр `Note`, представляющий одну заметку, в [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы. При выполнении обработчика событий `OnSaveButtonClicked` экземпляр `Note` сохраняется в базе данных, и приложение возвращается на предыдущую страницу. При выполнении обработчика событий `OnDeleteButtonClicked` экземпляр `Note` удаляется из базы данных, и приложение возвращается на предыдущую страницу.

      Сохраните изменения в **NoteEntryPage.xaml.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**, и закройте файл.

11. Создайте и запустите проект на каждой соответствующей платформе. Дополнительные сведения см. в разделе [Сборка примера из краткого руководства](single-page.md#building-the-quickstart).

    На странице **NotesPage** нажмите кнопку **+** , чтобы перейти к странице **NoteEntryPage** и ввести заметку. После сохранения заметки приложение вернется на страницу **NotesPage**.

    Введите несколько заметок разной длины, чтобы понаблюдать за поведением приложения.

::: zone-end

## <a name="next-steps"></a>Следующие шаги

В этом кратком руководстве рассматривались следующие темы:

- Использование диспетчер пакетов NuGet для добавления пакета NuGet в проект.
- Локальное хранение данных в базе данных SQLite.NET.

Чтобы стилизовать приложение с помощью стилей XAML, перейдите к следующему краткому руководству.

> [!div class="nextstepaction"]
> [Вперед](styling.md)

## <a name="related-links"></a>Связанные ссылки

- [Заметки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)
- [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md)
