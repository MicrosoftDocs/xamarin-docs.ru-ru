---
title: "Навигация в многостраничном приложении Xamarin.Forms" description: "Эта статья поясняет, как на основе одностраничного приложения, которое может хранить одну заметку, создать многостраничное, позволяющее хранить несколько заметок".
zone_pivot_groups: platform ms.topic: quickstart ms.prod: xamarin ms.assetid: 9DC3B3D6-6CBC-4705-BE80-3D86A9E65F92 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 04/01/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="perform-navigation-in-a-multi-page-xamarinforms-application"></a>Навигация в многостраничном приложении Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)

В этом кратком руководстве рассматриваются следующие темы:

- Добавление дополнительных страниц в решение Xamarin.Forms.
- Навигация между страницами.
- Использование привязки данных для синхронизации данных между элементами пользовательского интерфейса и их источником данных.

В этом кратком руководстве описано, как на основе одностраничного кроссплатформенного приложения Xamarin.Forms, которое может хранить одну заметку, создать многостраничное приложение, позволяющее хранить несколько заметок. Ниже показано итоговое приложение:

[![](multi-page-images/screenshots1-sml.png "Notes Page")](multi-page-images/screenshots1.png#lightbox "Notes Page")
[![](multi-page-images/screenshots2-sml.png "Note Entry Page")](multi-page-images/screenshots2.png#lightbox "Note Entry Page")

### <a name="prerequisites"></a>Предварительные требования

Прежде чем приступать к этому краткому руководству, необходимо успешно завершить [предыдущее](single-page.md). Также вы можете скачать [пример из предыдущего краткого руководства](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-singlepage/) и использовать его в качестве отправной точки для работы с этим руководством.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Обновление приложения с помощью Visual Studio

1. Запустите Visual Studio. В начальном окне щелкните решение **Notes** в списке недавних проектов/решений или элемент **Открыть проект или решение**, а затем в диалоговом окне **Открыть проект/решение** выберите файл решения для проекта Notes:

    ![](multi-page-images/vs/open-solution.png "Open Project")

2. В **обозревателе решений** щелкните проект **Notes** правой кнопкой мыши и выберите **Добавить > Новая папка**.

    ![](multi-page-images/vs/add-new-item.png "Add New Item")

3. В **обозревателе решений** присвойте новой папке имя **Models**:

    ![](multi-page-images/vs/name-folder.png "Models Folder")

4. В **обозревателе решений** выберите папку **Models**, щелкните правой кнопкой мыши и выберите **Добавить > Новый элемент...** :

    ![](multi-page-images/vs/add-new-models-file.png "Add New File")

5. В диалоговом окне **Добавление нового элемента** выберите **Элементы Visual C# > Класс**, назовите новый файл **Note** и нажмите кнопку **Добавить**:

    ![](multi-page-images/vs/add-note-class.png "Add Note Class")

    Это приведет к добавлению класса с именем **Note** в папку **Models** проекта **Notes**.

6. Удалите в файле **Note.cs** весь код шаблона и замените его приведенным ниже:

    ```csharp
    using System;

    namespace Notes.Models
    {
        public class Note
        {
            public string Filename { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    Этот класс определяет модель `Note`, где будут храниться данные о каждой заметке в этом приложении.    

    Сохраните изменения в **Note.cs**, нажав клавиши **CTRL+S**, и закройте файл.

7. В **обозревателе решений** щелкните проект **Notes** правой кнопкой мыши и выберите **Добавить > Новый элемент**. В диалоговом окне **Добавление нового элемента** выберите **Элементы Visual C# > Xamarin.Forms > Страница содержимого**, присвойте новому файлу имя **NoteEntryPage** и нажмите кнопку **Добавить**.

    ![](multi-page-images/vs/add-note-entry-page.png "Add Xamarin.Forms ContentPage")

    Это приведет к добавлению новой страницы с именем **NoteEntryPage** в корневую папку проекта. Эта страница будет второй страницей в приложении.

8. В **NoteEntryPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом:

      ```xaml
      <?xml version="1.0" encoding="UTF-8"?>
      <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                   x:Class="Notes.NoteEntryPage"
                   Title="Note Entry">
          <StackLayout Margin="20">
              <Editor Placeholder="Enter your note"
                      Text="{Binding Text}"
                      HeightRequest="100" />
              <Grid>
                  <Grid.ColumnDefinitions>
                      <ColumnDefinition Width="*" />
                      <ColumnDefinition Width="*" />
                  </Grid.ColumnDefinitions>
                  <Button Text="Save"
                          Clicked="OnSaveButtonClicked" />
                  <Button Grid.Column="1"
                          Text="Delete"
                          Clicked="OnDeleteButtonClicked"/>
              </Grid>
          </StackLayout>
      </ContentPage>
      ```

      Этот код декларативно определяет пользовательский интерфейс для страницы, которая состоит из [`Editor`](xref:Xamarin.Forms.Editor) для ввода текста, а также двух экземпляров [`Button`](xref:Xamarin.Forms.Button), которые предписывают приложению сохранить или удалить файл. Два экземпляра `Button` располагаются по горизонтали в [`Grid`](xref:Xamarin.Forms.Grid), а `Editor` и `Grid` — по вертикали в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Кроме того, `Editor` использует привязку данных для привязки к свойству `Text` модели `Note`. Дополнительные сведения о привязке данных см. в разделе [Привязка данных](deepdive.md#data-binding) статьи [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

      Сохраните изменения в файле **NoteEntryPage.xaml**, нажав клавиши **CTRL+S**, и закройте файл.

9. Удалите из **NoteEntryPage.xaml.cs** весь шаблонный код и замените его приведенным ниже.

      ```csharp
      using System;
      using System.IO;
      using Xamarin.Forms;
      using Notes.Models;

      namespace Notes
      {
          public partial class NoteEntryPage : ContentPage
          {
              public NoteEntryPage()
              {
                  InitializeComponent();
              }

              async void OnSaveButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (string.IsNullOrWhiteSpace(note.Filename))
                  {
                      // Save
                      var filename = Path.Combine(App.FolderPath, $"{Path.GetRandomFileName()}.notes.txt");
                      File.WriteAllText(filename, note.Text);
                  }
                  else
                  {
                      // Update
                      File.WriteAllText(note.Filename, note.Text);
                  }

                  await Navigation.PopAsync();
              }

              async void OnDeleteButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (File.Exists(note.Filename))
                  {
                      File.Delete(note.Filename);
                  }

                  await Navigation.PopAsync();
              }
          }
      }
      ```

      Этот код сохраняет экземпляр `Note`, представляющий одну заметку, в [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы. При нажатии кнопки **Сохранить** [`Button`](xref:Xamarin.Forms.Button) выполняется обработчик событий `OnSaveButtonClicked`, который сохраняет содержимое `Editor` либо в новый файл со случайно сформированным именем, либо в существующий файл, если заметка обновляется. В обоих случаях файл хранится в локальной папке данных для этого приложения. Затем метод осуществляет возврат на предыдущую страницу. При нажатии кнопки **Удалить** `Button` выполняется обработчик событий `OnDeleteButtonClicked`, который удаляет файл, если он существует, и осуществляет возврат на предыдущую страницу. Дополнительные сведения о навигации см. в разделе [Навигация](deepdive.md#navigation) статьи [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

      Сохраните изменения в файле **NoteEntryPage.xaml.cs**, нажав клавиши **CTRL+S**, и закройте файл.

      > [!WARNING]
      > Попытка выполнить сборку приложения на этом этапе приведет к ошибкам, которые будут исправлены позже.

10. В **обозревателе решений** щелкните проект **Notes** правой кнопкой мыши и выберите **Добавить > Новый элемент**. В диалоговом окне **Добавление нового элемента** выберите **Элементы Visual C# > Xamarin.Forms > Страница содержимого**, присвойте новому файлу имя **NotesPage** и нажмите кнопку **Добавить**.

      Это приведет к добавлению страницы с именем **NotesPage** в корневую папку проекта. Эта страница будет корневой страницей приложения.

11. Удалите из **NotesPage.xaml** весь шаблонный код и замените его приведенным ниже.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>
        <ListView x:Name="listView"
                  Margin="20"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`ListView`](xref:Xamarin.Forms.ListView) и [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem). `ListView` использует привязку данных для вывода всех заметок, извлекаемых приложением, а при выборе заметки осуществляется переход к `NoteEntryPage`, где ее можно изменить. Кроме того, можно создать заметку, нажав кнопку `ToolbarItem`. Дополнительные сведения о привязке данных см. в разделе [Привязка данных](deepdive.md#data-binding) статьи [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **NotesPage.xaml**, нажав клавиши **CTRL+S**, и закройте файл.

12. Удалите из **NotesPage.xaml.cs** весь шаблонный код и замените его приведенным ниже.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Xamarin.Forms;
    using Notes.Models;

    namespace Notes
    {
        public partial class NotesPage : ContentPage
        {
            public NotesPage()
            {
                InitializeComponent();
            }

            protected override void OnAppearing()
            {
                base.OnAppearing();

                var notes = new List<Note>();

                var files = Directory.EnumerateFiles(App.FolderPath, "*.notes.txt");
                foreach (var filename in files)
                {
                    notes.Add(new Note
                    {
                        Filename = filename,
                        Text = File.ReadAllText(filename),
                        Date = File.GetCreationTime(filename)
                    });
                }

                listView.ItemsSource = notes
                    .OrderBy(d => d.Date)
                    .ToList();
            }

            async void OnNoteAddedClicked(object sender, EventArgs e)
            {
                await Navigation.PushAsync(new NoteEntryPage
                {
                    BindingContext = new Note()
                });
            }

            async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
            {
                if (e.SelectedItem != null)
                {
                    await Navigation.PushAsync(new NoteEntryPage
                    {
                        BindingContext = e.SelectedItem as Note
                    });
                }
            }
        }
    }
    ```    

    Этот код определяет функциональные возможности для `NotesPage`. При отображении страницы выполняется метод `OnAppearing`, который заполняет [`ListView`](xref:Xamarin.Forms.ListView) всеми заметками, полученными из локальной папки данных приложений. При нажатии кнопки [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) запускается обработчик событий `OnNoteAddedClicked`. Этот метод переходит к `NoteEntryPage`, задавая для [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы `NoteEntryPage` новый экземпляр `Note`. При выборе элемента в `ListView` запускается обработчик событий `OnListViewItemSelected`. Этот метод переходит к `NoteEntryPage`, задавая для [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы `NoteEntryPage` выбранный экземпляр `Note`. Дополнительные сведения о навигации см. в разделе [Навигация](deepdive.md#navigation) статьи [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **NotesPage.xaml.cs**, нажав клавиши **CTRL+S**, и закройте файл.

    > [!WARNING]
    > Попытка выполнить сборку приложения на этом этапе приведет к ошибкам, которые будут исправлены позже.

13. В **обозревателе решений** дважды щелкните файл **App.xaml.cs**, чтобы открыть его. Затем замените существующий код следующим:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class App : Application
        {
            public static string FolderPath { get; private set; }

            public App()
            {
                InitializeComponent();
                FolderPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData));
                MainPage = new NavigationPage(new NotesPage());
            }
            // ...
        }
    }
    ```

    Этот код добавляет объявление пространства имен `System.IO` и объявление для статического свойства `FolderPath` типа `string`. Свойство `FolderPath` используется для хранения пути на устройстве, где будут храниться данные заметки. Кроме того, код инициализирует свойство `FolderPath` в конструкторе `App`, а также инициализирует свойство [`MainPage`](xref:Xamarin.Forms.Application.MainPage) как [`NavigationPage`](xref:Xamarin.Forms.NavigationPage), где размещается экземпляр `NotesPage`. Дополнительные сведения о навигации см. в разделе [Навигация](deepdive.md#navigation) статьи [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **App.xaml.cs**, нажав клавиши **CTRL+S**, и закройте файл.

14. В **обозревателе решений** выберите проект **Notes**, щелкните правой кнопкой мыши **MainPage.xaml** и выберите пункт **Удалить**. В появившемся диалоговом окне нажмите кнопку **ОК**, чтобы удалить файл с жесткого диска.

    При этом страница, которая больше не используется, удаляется.

15. Создайте и запустите проект на каждой соответствующей платформе. Дополнительные сведения см. в разделе [Сборка примера из краткого руководства](single-page.md#building-the-quickstart).

    На странице **NotesPage** нажмите кнопку **+** , чтобы перейти к странице **NoteEntryPage** и ввести заметку. После сохранения заметки приложение вернется на страницу **NotesPage**.

    Введите несколько заметок разной длины, чтобы понаблюдать за поведением приложения.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Обновление приложения с помощью Visual Studio для Mac

1. Запуск Visual Studio для Mac В начальном окне щелкните **Открыть**, а затем в диалоговом окне выберите файл решения проекта Notes:

    ![](multi-page-images/vsmac/open-solution.png "Open Solution")

2. На **Панели решения** выберите проект **Notes**, щелкните правой кнопкой мыши и выберите **Добавить > Новая папка**:

    ![](multi-page-images/vsmac/add-new-folder.png "Add New Folder")

3. На **Панели решения** присвойте новой папке имя **Models**:

    ![](multi-page-images/vsmac/name-folder.png "Models Folder")

4. На **Панели решения** выберите папку **Models**, щелкните правой кнопкой мыши и выберите **Добавить > Новый файл...** :

    ![](multi-page-images/vsmac/add-new-models-file.png "Add New File")

5. В диалоговом окне **Новый файл** выберите **Общие > Пустой класс**, присвойте новому файлу имя **Note** и нажмите кнопку **Создать**:

    ![](multi-page-images/vsmac/add-note-class.png "Add Note Class")

    Это приведет к добавлению класса с именем **Note** в папку **Models** проекта **Notes**.

6. Удалите в файле **Note.cs** весь код шаблона и замените его приведенным ниже:

    ```csharp
    using System;

    namespace Notes.Models
    {
        public class Note
        {
            public string Filename { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    Этот класс определяет модель `Note`, где будут храниться данные о каждой заметке в этом приложении.

    Сохраните изменения в **Note.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**, и закройте файл.

7. На **Панели решения** выберите проект **Notes**, щелкните правой кнопкой мыши и выберите **Добавить > Новый файл...** . В диалоговом окне **Новый файл** выберите **Forms > Forms ContentPage XAML**, назовите новый файл **NoteEntryPage** и нажмите кнопку **Создать**:

    ![](multi-page-images/vsmac/add-note-entry-page.png "Add Xamarin.Forms ContentPage")

    Это приведет к добавлению новой страницы с именем **NoteEntryPage** в корневую папку проекта. Эта страница будет второй страницей в приложении.

8. В **NoteEntryPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом:

      ```xaml
      <?xml version="1.0" encoding="UTF-8"?>
      <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                   x:Class="Notes.NoteEntryPage"
                   Title="Note Entry">
          <StackLayout Margin="20">
              <Editor Placeholder="Enter your note"
                      Text="{Binding Text}"
                      HeightRequest="100" />
              <Grid>
                  <Grid.ColumnDefinitions>
                      <ColumnDefinition Width="*" />
                      <ColumnDefinition Width="*" />
                  </Grid.ColumnDefinitions>
                  <Button Text="Save"
                          Clicked="OnSaveButtonClicked" />
                  <Button Grid.Column="1"
                          Text="Delete"
                          Clicked="OnDeleteButtonClicked"/>
              </Grid>
          </StackLayout>
      </ContentPage>
      ```

      Этот код декларативно определяет пользовательский интерфейс для страницы, которая состоит из [`Editor`](xref:Xamarin.Forms.Editor) для ввода текста, а также двух экземпляров [`Button`](xref:Xamarin.Forms.Button), которые предписывают приложению сохранить или удалить файл. Два экземпляра `Button` располагаются по горизонтали в [`Grid`](xref:Xamarin.Forms.Grid), а `Editor` и `Grid` — по вертикали в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Кроме того, `Editor` использует привязку данных для привязки к свойству `Text` модели `Note`. Дополнительные сведения о привязке данных см. в разделе [Привязка данных](deepdive.md#data-binding) статьи [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

      Сохраните изменения в **NoteEntryPage.xaml**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**, и закройте файл.

9. Удалите из **NoteEntryPage.xaml.cs** весь шаблонный код и замените его приведенным ниже.

      ```csharp
      using System;
      using System.IO;
      using Xamarin.Forms;
      using Notes.Models;

      namespace Notes
      {
          public partial class NoteEntryPage : ContentPage
          {
              public NoteEntryPage()
              {
                  InitializeComponent();
              }

              async void OnSaveButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (string.IsNullOrWhiteSpace(note.Filename))
                  {
                      // Save
                      var filename = Path.Combine(App.FolderPath, $"{Path.GetRandomFileName()}.notes.txt");
                      File.WriteAllText(filename, note.Text);
                  }
                  else
                  {
                      // Update
                      File.WriteAllText(note.Filename, note.Text);
                  }

                  await Navigation.PopAsync();
              }

              async void OnDeleteButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (File.Exists(note.Filename))
                  {
                      File.Delete(note.Filename);
                  }

                  await Navigation.PopAsync();
              }
          }
      }
      ```

      Этот код сохраняет экземпляр `Note`, представляющий одну заметку, в [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы. При нажатии кнопки **Сохранить** [`Button`](xref:Xamarin.Forms.Button) выполняется обработчик событий `OnSaveButtonClicked`, который сохраняет содержимое `Editor` либо в новый файл со случайно сформированным именем, либо в существующий файл, если заметка обновляется. В обоих случаях файл хранится в локальной папке данных для этого приложения. Затем метод осуществляет возврат на предыдущую страницу. При нажатии кнопки **Удалить** `Button` выполняется обработчик событий `OnDeleteButtonClicked`, который удаляет файл, если он существует, и осуществляет возврат на предыдущую страницу. Дополнительные сведения о навигации см. в разделе [Навигация](deepdive.md#navigation) статьи [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

      Сохраните изменения в **NoteEntryPage.xaml.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**, и закройте файл.

      > [!WARNING]
      > Попытка выполнить сборку приложения на этом этапе приведет к ошибкам, которые будут исправлены позже.

10. На **Панели решения** выберите проект **Notes**, щелкните правой кнопкой мыши и выберите **Добавить > Новый файл...** . В диалоговом окне **Новый файл** выберите **Forms > Forms ContentPage XAML**, назовите новый файл **NotesPage** и нажмите кнопку **Создать**.

      Это приведет к добавлению страницы с именем **NotesPage** в корневую папку проекта. Эта страница будет корневой страницей приложения.

11. Удалите из **NotesPage.xaml** весь шаблонный код и замените его приведенным ниже.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>
        <ListView x:Name="listView"
                  Margin="20"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`ListView`](xref:Xamarin.Forms.ListView) и [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem). `ListView` использует привязку данных для вывода всех заметок, извлекаемых приложением, а при выборе заметки осуществляется переход к `NoteEntryPage`, где ее можно изменить. Кроме того, можно создать заметку, нажав кнопку `ToolbarItem`. Дополнительные сведения о привязке данных см. в разделе [Привязка данных](deepdive.md#data-binding) статьи [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в **NotesPage.xaml**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**, и закройте файл.

12. Удалите из **NotesPage.xaml.cs** весь шаблонный код и замените его приведенным ниже.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Xamarin.Forms;
    using Notes.Models;

    namespace Notes
    {
        public partial class NotesPage : ContentPage
        {
            public NotesPage()
            {
                InitializeComponent();
            }

            protected override void OnAppearing()
            {
                base.OnAppearing();

                var notes = new List<Note>();

                var files = Directory.EnumerateFiles(App.FolderPath, "*.notes.txt");
                foreach (var filename in files)
                {
                    notes.Add(new Note
                    {
                        Filename = filename,
                        Text = File.ReadAllText(filename),
                        Date = File.GetCreationTime(filename)
                    });
                }

                listView.ItemsSource = notes
                    .OrderBy(d => d.Date)
                    .ToList();
            }

            async void OnNoteAddedClicked(object sender, EventArgs e)
            {
                await Navigation.PushAsync(new NoteEntryPage
                {
                    BindingContext = new Note()
                });
            }

            async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
            {
                if (e.SelectedItem != null)
                {
                    await Navigation.PushAsync(new NoteEntryPage
                    {
                        BindingContext = e.SelectedItem as Note
                    });
                }
            }
        }
    }
    ```    

    Этот код определяет функциональные возможности для `NotesPage`. При отображении страницы выполняется метод `OnAppearing`, который заполняет [`ListView`](xref:Xamarin.Forms.ListView) всеми заметками, полученными из локальной папки данных приложений. При нажатии кнопки [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) запускается обработчик событий `OnNoteAddedClicked`. Этот метод переходит к `NoteEntryPage`, задавая для [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы `NoteEntryPage` новый экземпляр `Note`. При выборе элемента в `ListView` запускается обработчик событий `OnListViewItemSelected`. Этот метод переходит к `NoteEntryPage`, задавая для [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы `NoteEntryPage` выбранный экземпляр `Note`. Дополнительные сведения о навигации см. в разделе [Навигация](deepdive.md#navigation) статьи [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в **NotesPage.xaml.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**, и закройте файл.

    > [!WARNING]
    > Попытка выполнить сборку приложения на этом этапе приведет к ошибкам, которые будут исправлены позже.

13. На **Панели решения** дважды щелкните файл **App.xaml.cs**, чтобы открыть его. Затем замените существующий код следующим:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class App : Application
        {
            public static string FolderPath { get; private set; }

            public App()
            {
                InitializeComponent();
                FolderPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData));
                MainPage = new NavigationPage(new NotesPage());
            }
            // ...
        }
    }
    ```

    Этот код добавляет объявление пространства имен `System.IO` и объявление для статического свойства `FolderPath` типа `string`. Свойство `FolderPath` используется для хранения пути на устройстве, где будут храниться данные заметки. Кроме того, код инициализирует свойство `FolderPath` в конструкторе `App`, а также инициализирует свойство [`MainPage`](xref:Xamarin.Forms.Application.MainPage) как [`NavigationPage`](xref:Xamarin.Forms.NavigationPage), где размещается экземпляр `NotesPage`. Дополнительные сведения о навигации см. в разделе [Навигация](deepdive.md#navigation) статьи [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **App.xaml.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**, и закройте файл.

14. На **Панели решения** выберите проект **Notes**, щелкните правой кнопкой мыши **MainPage.xaml** и выберите пункт **Удалить**. В появившемся диалоговом окне нажмите кнопку **Удалить**, чтобы удалить файл с жесткого диска.

    При этом страница, которая больше не используется, удаляется.

15. Создайте и запустите проект на каждой соответствующей платформе. Дополнительные сведения см. в разделе [Сборка примера из краткого руководства](single-page.md#building-the-quickstart).

    На странице **NotesPage** нажмите кнопку **+** , чтобы перейти к странице **NoteEntryPage** и ввести заметку. После сохранения заметки приложение вернется на страницу **NotesPage**.

    Введите несколько заметок разной длины, чтобы понаблюдать за поведением приложения.

::: zone-end

## <a name="next-steps"></a>Следующие шаги

В этом кратком руководстве рассматривались следующие темы:

- Добавление дополнительных страниц в решение Xamarin.Forms.
- Навигация между страницами.
- Использование привязки данных для синхронизации данных между элементами пользовательского интерфейса и их источником данных.

Чтобы изменить приложение таким образом, чтобы оно сохраняло свои данные в локальной базе данных SQLite.NET, перейдите к следующему краткому руководству.

> [!div class="nextstepaction"]
> [Вперед](database.md)

## <a name="related-links"></a>Связанные ссылки

- [Заметки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)
- [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md)
