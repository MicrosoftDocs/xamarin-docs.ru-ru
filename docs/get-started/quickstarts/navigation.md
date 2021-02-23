---
title: Навигация в приложении Xamarin.Forms
description: В этой статье объясняется, как на основе приложения Оболочки в Xamarin.Forms, которое может хранить одну заметку, создать приложение, позволяющее хранить несколько заметок.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 030204F7-E9E4-4AE3-B9F7-915B697D0171
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.custom: contperf-fy21q3
ms.date: 01/28/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c218c7108ec70ab1ccd1b048e1655dba02c48660
ms.sourcegitcommit: 66ca2680c8fde18a55ce5daa3818edeeb9ba219b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/17/2021
ms.locfileid: "100569711"
---
# <a name="perform-navigation-in-a-xamarinforms-application"></a>Навигация в приложении Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/getstarted-notes-navigation/)

В этом кратком руководстве рассматриваются следующие темы:

- Добавление дополнительных страниц в приложение Оболочки в Xamarin.Forms.
- Навигация между страницами.
- Использование привязки данных для синхронизации данных между элементами пользовательского интерфейса и их источником данных.

В этом кратком руководстве описано, как на основе кроссплатформенного приложения Оболочки в Xamarin.Forms, которое может хранить одну заметку, создать приложение, позволяющее хранить несколько заметок. Ниже показано итоговое приложение:

[![Страница заметок](navigation-images/screenshots1-sml.png)](navigation-images/screenshots1.png#lightbox "Страница заметок")
[![Страница ввода заметки](navigation-images/screenshots2-sml.png)](navigation-images/screenshots2.png#lightbox "Страница ввода заметки")

### <a name="prerequisites"></a>Предварительные требования

Прежде чем приступать к этому краткому руководству, необходимо успешно завершить [предыдущее](app.md). Также вы можете скачать [пример из предыдущего краткого руководства](/samples/xamarin/xamarin-forms-samples/getstarted-notes-app/) и использовать его в качестве отправной точки для работы с этим руководством.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Обновление приложения с помощью Visual Studio

1. Запустите Visual Studio. В начальном окне щелкните решение **Notes** в списке недавних проектов/решений или элемент **Открыть проект или решение**, а затем в диалоговом окне **Открыть проект/решение** выберите файл решения для проекта Notes:

    ![Открытие решения](navigation-images/vs/open-solution.png)

2. В **обозревателе решений** щелкните проект **Notes** правой кнопкой мыши и выберите **Добавить > Новая папка**.

    ![Добавление новой папки](navigation-images/vs/add-new-folder.png)

3. В **обозревателе решений** присвойте новой папке имя **Models**:

    ![Папка Models](navigation-images/vs/name-folder.png)

4. В **обозревателе решений** выберите папку **Models**, щелкните правой кнопкой мыши и выберите **Добавить > Класс...** :

    ![Добавление нового файла](navigation-images/vs/add-new-models-file.png)

5. В диалоговом окне **Добавление нового элемента** выберите **Элементы Visual C# > Класс**, назовите новый файл **Note** и нажмите кнопку **Добавить**:

    ![Добавление класса Note](navigation-images/vs/add-note-class.png)

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

    Сохраните изменения в **Note.cs**, нажав клавиши **CTRL+S**.  

7. В **обозревателе решений** в проекте **Notes** выберите папку **Views**, щелкните правой кнопкой мыши и выберите **Добавить > Новый элемент...** : В диалоговом окне **Добавление нового элемента** выберите **Элементы Visual C# > Xamarin.Forms > Страница содержимого**, присвойте новому файлу имя **NoteEntryPage** и нажмите кнопку **Добавить**.

    ![Добавление Xamarin.Forms ContentPage](navigation-images/vs/add-note-entry-page.png)

    В результате этого будет добавлена новая страница с именем **NoteEntryPage** в папку **Views** проекта. Эта страница будет использоваться для записи заметки.

8. В **NoteEntryPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом:

      ```xaml
      <?xml version="1.0" encoding="UTF-8"?>
      <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                   x:Class="Notes.Views.NoteEntryPage"
                   Title="Note Entry">
          <!-- Layout children vertically -->
          <StackLayout Margin="20">
              <Editor Placeholder="Enter your note"
                      Text="{Binding Text}"
                      HeightRequest="100" />
              <!-- Layout children in two columns -->
              <Grid ColumnDefinitions="*,*">
                  <Button Text="Save"
                          Clicked="OnSaveButtonClicked" />
                  <Button Grid.Column="1"
                          Text="Delete"
                          Clicked="OnDeleteButtonClicked"/>
              </Grid>
          </StackLayout>
      </ContentPage>
      ```

      Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Editor`](xref:Xamarin.Forms.Editor) для ввода текста, а также двух объектов [`Button`](xref:Xamarin.Forms.Button), которые предписывают приложению сохранить или удалить файл. Два экземпляра `Button` располагаются по горизонтали в [`Grid`](xref:Xamarin.Forms.Grid), а `Editor` и `Grid` — по вертикали в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Кроме того, `Editor` использует привязку данных для привязки к свойству `Text` модели `Note`. Дополнительные сведения о привязке данных см. в разделе [Привязка данных](deepdive.md#data-binding) статьи [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

      Сохраните изменения в файле **NoteEntryPage.xaml**, нажав клавиши **CTRL+S**.

9. Удалите из **NoteEntryPage.xaml.cs** весь шаблонный код и замените его приведенным ниже.

      ```csharp
      using System;
      using System.IO;
      using Notes.Models;
      using Xamarin.Forms;

      namespace Notes.Views
      {
          [QueryProperty(nameof(ItemId), nameof(ItemId))]
          public partial class NoteEntryPage : ContentPage
          {
              public string ItemId
              {
                  set
                  {
                      LoadNote(value);
                  }
              }

              public NoteEntryPage()
              {
                  InitializeComponent();

                  // Set the BindingContext of the page to a new Note.
                  BindingContext = new Note();
              }

              void LoadNote(string filename)
              {
                  try
                  {
                      // Retrieve the note and set it as the BindingContext of the page.
                      Note note = new Note
                      {
                          Filename = filename,
                          Text = File.ReadAllText(filename),
                          Date = File.GetCreationTime(filename)
                      };
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

                  if (string.IsNullOrWhiteSpace(note.Filename))
                  {
                      // Save the file.
                      var filename = Path.Combine(App.FolderPath, $"{Path.GetRandomFileName()}.notes.txt");
                      File.WriteAllText(filename, note.Text);
                  }
                  else
                  {
                      // Update the file.
                      File.WriteAllText(note.Filename, note.Text);
                  }

                  // Navigate backwards
                  await Shell.Current.GoToAsync("..");
              }

              async void OnDeleteButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  // Delete the file.
                  if (File.Exists(note.Filename))
                  {
                      File.Delete(note.Filename);
                  }

                  // Navigate backwards
                  await Shell.Current.GoToAsync("..");
              }
          }
      }
      ```

      Этот код сохраняет экземпляр `Note`, представляющий одну заметку, в [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы. Класс снабжен атрибутом `QueryPropertyAttribute`, что позволяет передавать данные на страницу во время навигации с помощью параметров запроса. Первый аргумент в `QueryPropertyAttribute` указывает имя свойства, которое будет получать данные, а второй аргумент задает идентификатор параметра запроса. Таким образом, атрибут `QueryParameterAttribute` в коде выше указывает, что свойство `ItemId` будет получать данные, переданные в параметр запроса `ItemId` из URI, указанного в вызове метода `GoToAsync`. Затем свойство `ItemId` вызывает метод `LoadNote` для создания объекта `Note` из файла на устройстве и задает для `BindingContext` страницы объект `Note`.

      При нажатии кнопки **Сохранить** [`Button`](xref:Xamarin.Forms.Button) выполняется обработчик событий `OnSaveButtonClicked`, который сохраняет содержимое `Editor` либо в новый файл со случайно сформированным именем, либо в существующий файл, если заметка обновляется. В обоих случаях файл хранится в локальной папке данных для этого приложения. Затем метод осуществляет возврат на предыдущую страницу. При нажатии кнопки **Удалить** `Button` выполняется обработчик событий `OnDeleteButtonClicked`, который удаляет файл, если он существует, и осуществляет возврат на предыдущую страницу. Дополнительные сведения о навигации см. в разделе [Навигация](deepdive.md#navigation) статьи [Подробное изучение кратких руководств по Оболочке в Xamarin.Forms](deepdive.md).

      Сохраните изменения в файле **NoteEntryPage.xaml.cs**, нажав клавиши **CTRL+S**.

      > [!WARNING]
      > В данный момент сборка приложения не будет выполнена из-за ошибок, которые будут исправлены в последующих шагах.

10. В **обозревателе решений** в проекте **Notes** откройте **NotesPage.xaml** в папке **Views**.

11. Удалите из **NotesPage.xaml** весь шаблонный код и замените его приведенным ниже.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NotesPage"
                 Title="Notes">
        <!-- Add an item to the toolbar -->
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="Add"
                         Clicked="OnAddClicked" />
        </ContentPage.ToolbarItems>

        <!-- Display notes in a list -->
        <CollectionView x:Name="collectionView"
                        Margin="20"
                        SelectionMode="Single"
                        SelectionChanged="OnSelectionChanged">
            <CollectionView.ItemsLayout>
                <LinearItemsLayout Orientation="Vertical"
                                   ItemSpacing="10" />
            </CollectionView.ItemsLayout>
            <!-- Define the appearance of each item in the list -->
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <StackLayout>
                        <Label Text="{Binding Text}"
                               FontSize="Medium"/>
                        <Label Text="{Binding Date}"
                               TextColor="Silver"
                               FontSize="Small" />
                    </StackLayout>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`CollectionView`](xref:Xamarin.Forms.CollectionView) и [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem). `CollectionView` использует привязку данных для отображения заметок, полученных приложением. При выборе заметки будет выполнен переход к `NoteEntryPage`, в которой можно изменить заметку. Кроме того, можно создать заметку, нажав кнопку `ToolbarItem`. Дополнительные сведения о привязке данных см. в разделе [Привязка данных](deepdive.md#data-binding) статьи [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **NotesPage.xaml**, нажав клавиши **CTRL+S**.

12. В **обозревателе решений** в проекте **Notes** разверните **NotesPage.xaml** в папке **Views** и откройте **NotesPage.xaml.cs**.

13. Удалите из **NotesPage.xaml.cs** весь шаблонный код и замените его приведенным ниже.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Notes.Models;
    using Xamarin.Forms;

    namespace Notes.Views
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

                // Create a Note object from each file.
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

                // Set the data source for the CollectionView to a
                // sorted collection of notes.
                collectionView.ItemsSource = notes
                    .OrderBy(d => d.Date)
                    .ToList();
            }

            async void OnAddClicked(object sender, EventArgs e)
            {
                // Navigate to the NoteEntryPage, without passing any data.
                await Shell.Current.GoToAsync(nameof(NoteEntryPage));
            }

            async void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
            {
                if (e.CurrentSelection != null)
                {
                    // Navigate to the NoteEntryPage, passing the filename as a query parameter.
                    Note note = (Note)e.CurrentSelection.FirstOrDefault();
                    await Shell.Current.GoToAsync($"{nameof(NoteEntryPage)}?{nameof(NoteEntryPage.ItemId)}={note.Filename}");
                }
            }
        }
    }
    ```

    Этот код определяет функциональные возможности для `NotesPage`. При отображении страницы выполняется метод `OnAppearing`, который заполняет [`CollectionView`](xref:Xamarin.Forms.CollectionView) всеми заметками, полученными из локальной папки данных приложений. При нажатии кнопки [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) запускается обработчик событий `OnAddClicked`. Этот метод переходит к `NoteEntryPage`. При выборе элемента в `CollectionView` запускается обработчик событий `OnSelectionChanged`. Этот метод переходит к `NoteEntryPage` при условии, что выбран элемент [`CollectionView`](xref:Xamarin.Forms.CollectionView), передавая свойство `Filename` выбранного `Note` в качестве параметра запроса на страницу. Дополнительные сведения о навигации см. в разделе [Навигация](deepdive.md#navigation) статьи [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **NotesPage.xaml.cs**, нажав клавиши **CTRL+S**.

    > [!WARNING]
    > В данный момент сборка приложения не будет выполнена из-за ошибок, которые будут исправлены в последующих шагах.

14. В **обозревателе решений** в проекте **Notes** разверните **AppShell.xaml** и откройте **AppShell.xaml.cs**. Затем замените существующий код следующим:

    ```csharp
    using Notes.Views;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class AppShell : Shell
        {
            public AppShell()
            {
                InitializeComponent();
                Routing.RegisterRoute(nameof(NoteEntryPage), typeof(NoteEntryPage));
            }
        }
    }
    ```

    Этот код регистрирует маршрут для `NoteEntryPage`, который не представлен в визуальной иерархии Оболочки (**AppShell.xaml**). Затем на этой странице можно перейти к использованию навигации на основе URI с помощью метода `GoToAsync`.

    Сохраните изменения в файле **AppShell.xaml.cs**, нажав клавиши **CTRL+S**.

15. В **обозревателе решений** разверните **App.xaml** и откройте **App.xaml.cs** в проекте **Notes**. Затем замените существующий код следующим:

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

    Этот код добавляет объявление пространства имен `System.IO` и объявление для статического свойства `FolderPath` типа `string`. Свойство `FolderPath` используется для хранения пути на устройстве, где будут храниться данные заметки. Кроме того, код инициализирует свойство `FolderPath` в конструкторе `App`, а также инициализирует свойство [`MainPage`](xref:Xamarin.Forms.Application.MainPage) в производном объекте `Shell`.

    Сохраните изменения в файле **App.xaml.cs**, нажав клавиши **CTRL+S**.

16. Создайте и запустите проект на каждой соответствующей платформе. Дополнительные сведения см. в разделе [Сборка примера из краткого руководства](app.md#building-the-quickstart).

    На странице **NotesPage** нажмите кнопку **Добавить**, чтобы перейти к странице **NoteEntryPage** и ввести заметку. После сохранения заметки приложение вернется на страницу **NotesPage**.

    Введите несколько заметок разной длины, чтобы понаблюдать за поведением приложения. Закройте приложение и повторно запустите его, чтобы проверить, сохранены ли на устройстве введенные заметки.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Обновление приложения с помощью Visual Studio для Mac

1. Запуск Visual Studio для Mac В начальном окне щелкните **Открыть**, а затем в диалоговом окне выберите файл решения проекта Notes:

    ![Открытие решения](navigation-images/vsmac/open-solution.png)

2. На **Панели решения** щелкните проект **Notes** правой кнопкой мыши и выберите **Добавить > Новая папка**.

    ![Добавление новой папки](navigation-images/vsmac/add-new-folder.png)

3. В диалоговом окне **Новая папка** присвойте новой папке имя **Models**:

    ![Папка Models](navigation-images/vsmac/name-folder.png)

4. На **Панели решения** выберите папку **Models**, щелкните правой кнопкой мыши и выберите **Добавить > Новый класс...** :

    ![Добавление нового файла](navigation-images/vsmac/add-new-models-file.png)

5. В диалоговом окне **Новый файл** выберите **Общие > Пустой класс**, присвойте новому файлу имя **Note** и нажмите кнопку **Создать**:

    ![Добавление класса Note](navigation-images/vsmac/add-note-class.png)

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

    Сохраните изменения в файле **Note.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

7. На **Панели решения** выберите проект **Notes**, щелкните правой кнопкой мыши и выберите **Добавить > Новый файл...** . В диалоговом окне **Новый файл** выберите **Forms > Forms ContentPage XAML**, назовите новый файл **NoteEntryPage** и нажмите кнопку **Создать**:

    ![Добавление Xamarin.Forms ContentPage](navigation-images/vsmac/add-note-entry-page.png)

    В результате этого будет добавлена новая страница с именем **NoteEntryPage** в папку **Views** проекта. Эта страница будет использоваться для записи заметки.

8. В **NoteEntryPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом:

      ```xaml
      <?xml version="1.0" encoding="UTF-8"?>
      <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                   x:Class="Notes.Views.NoteEntryPage"
                   Title="Note Entry">
          <!-- Layout children vertically -->
          <StackLayout Margin="20">
              <Editor Placeholder="Enter your note"
                      Text="{Binding Text}"
                      HeightRequest="100" />
              <!-- Layout children in two columns -->
              <Grid ColumnDefinitions="*,*">
                  <Button Text="Save"
                          Clicked="OnSaveButtonClicked" />
                  <Button Grid.Column="1"
                          Text="Delete"
                          Clicked="OnDeleteButtonClicked"/>
              </Grid>
          </StackLayout>
      </ContentPage>
      ```

      Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Editor`](xref:Xamarin.Forms.Editor) для ввода текста, а также двух объектов [`Button`](xref:Xamarin.Forms.Button), которые предписывают приложению сохранить или удалить файл. Два экземпляра `Button` располагаются по горизонтали в [`Grid`](xref:Xamarin.Forms.Grid), а `Editor` и `Grid` — по вертикали в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Кроме того, `Editor` использует привязку данных для привязки к свойству `Text` модели `Note`. Дополнительные сведения о привязке данных см. в разделе [Привязка данных](deepdive.md#data-binding) статьи [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

      Сохраните изменения в файле **NoteEntryPage.xaml**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

9. Удалите из **NoteEntryPage.xaml.cs** весь шаблонный код и замените его приведенным ниже.

      ```csharp
      using System;
      using System.IO;
      using Notes.Models;
      using Xamarin.Forms;

      namespace Notes.Views
      {
          [QueryProperty(nameof(ItemId), nameof(ItemId))]
          public partial class NoteEntryPage : ContentPage
          {
              public string ItemId
              {
                  set
                  {
                      LoadNote(value);
                  }
              }

              public NoteEntryPage()
              {
                  InitializeComponent();

                  // Set the BindingContext of the page to a new Note.
                  BindingContext = new Note();
              }

              void LoadNote(string filename)
              {
                  try
                  {
                      // Retrieve the note and set it as the BindingContext of the page.
                      Note note = new Note
                      {
                          Filename = filename,
                          Text = File.ReadAllText(filename),
                          Date = File.GetCreationTime(filename)
                      };
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

                  if (string.IsNullOrWhiteSpace(note.Filename))
                  {
                      // Save the file.
                      var filename = Path.Combine(App.FolderPath, $"{Path.GetRandomFileName()}.notes.txt");
                      File.WriteAllText(filename, note.Text);
                  }
                  else
                  {
                      // Update the file.
                      File.WriteAllText(note.Filename, note.Text);
                  }

                  // Navigate backwards
                  await Shell.Current.GoToAsync("..");
              }

              async void OnDeleteButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  // Delete the file.
                  if (File.Exists(note.Filename))
                  {
                      File.Delete(note.Filename);
                  }

                  // Navigate backwards
                  await Shell.Current.GoToAsync("..");
              }
          }
      }
      ```

      Этот код сохраняет экземпляр `Note`, представляющий одну заметку, в [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы. Класс снабжен атрибутом `QueryPropertyAttribute`, что позволяет передавать данные на страницу во время навигации с помощью параметров запроса. Первый аргумент в `QueryPropertyAttribute` указывает имя свойства, которое будет получать данные, а второй аргумент задает идентификатор параметра запроса. Таким образом, атрибут `QueryParameterAttribute` в коде выше указывает, что свойство `ItemId` будет получать данные, переданные в параметр запроса `ItemId` из URI, указанного в вызове метода `GoToAsync`. Затем свойство `ItemId` вызывает метод `LoadNote` для создания объекта `Note` из файла на устройстве и задает для `BindingContext` страницы объект `Note`.

      При нажатии кнопки **Сохранить** [`Button`](xref:Xamarin.Forms.Button) выполняется обработчик событий `OnSaveButtonClicked`, который сохраняет содержимое `Editor` либо в новый файл со случайно сформированным именем, либо в существующий файл, если заметка обновляется. В обоих случаях файл хранится в локальной папке данных для этого приложения. Затем метод осуществляет возврат на предыдущую страницу. При нажатии кнопки **Удалить** `Button` выполняется обработчик событий `OnDeleteButtonClicked`, который удаляет файл, если он существует, и осуществляет возврат на предыдущую страницу. Дополнительные сведения о навигации см. в разделе [Навигация](deepdive.md#navigation) статьи [Подробное изучение кратких руководств по Оболочке в Xamarin.Forms](deepdive.md).

      Сохраните изменения в файле **NoteEntryPage.xaml.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

      > [!WARNING]
      > В данный момент сборка приложения не будет выполнена из-за ошибок, которые будут исправлены в последующих шагах.

10. На **Панели решения** в проекте **Notes** откройте **NotesPage.xaml** в папке **Views**.

11. Удалите из **NotesPage.xaml** весь шаблонный код и замените его приведенным ниже.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NotesPage"
                 Title="Notes">
        <!-- Add an item to the toolbar -->
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="Add"
                         Clicked="OnAddClicked" />
        </ContentPage.ToolbarItems>

        <!-- Display notes in a list -->
        <CollectionView x:Name="collectionView"
                        Margin="20"
                        SelectionMode="Single"
                        SelectionChanged="OnSelectionChanged">
            <CollectionView.ItemsLayout>
                <LinearItemsLayout Orientation="Vertical"
                                   ItemSpacing="10" />
            </CollectionView.ItemsLayout>
            <!-- Define the appearance of each item in the list -->
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <StackLayout>
                        <Label Text="{Binding Text}"
                               FontSize="Medium"/>
                        <Label Text="{Binding Date}"
                               TextColor="Silver"
                               FontSize="Small" />
                    </StackLayout>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`CollectionView`](xref:Xamarin.Forms.CollectionView) и [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem). `CollectionView` использует привязку данных для отображения заметок, полученных приложением. При выборе заметки будет выполнен переход к `NoteEntryPage`, в которой можно изменить заметку. Кроме того, можно создать заметку, нажав кнопку `ToolbarItem`. Дополнительные сведения о привязке данных см. в разделе [Привязка данных](deepdive.md#data-binding) статьи [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **NotesPage.xaml**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

12. На **Панели решения** в проекте **Notes** разверните **NotesPage.xaml** в папке **Views** и откройте **NotesPage.xaml.cs**.

13. Удалите из **NotesPage.xaml.cs** весь шаблонный код и замените его приведенным ниже.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Notes.Models;
    using Xamarin.Forms;

    namespace Notes.Views
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

                // Create a Note object from each file.
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

                // Set the data source for the CollectionView to a
                // sorted collection of notes.
                collectionView.ItemsSource = notes
                    .OrderBy(d => d.Date)
                    .ToList();
            }

            async void OnAddClicked(object sender, EventArgs e)
            {
                // Navigate to the NoteEntryPage, without passing any data.
                await Shell.Current.GoToAsync(nameof(NoteEntryPage));
            }

            async void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
            {
                if (e.CurrentSelection != null)
                {
                    // Navigate to the NoteEntryPage, passing the filename as a query parameter.
                    Note note = (Note)e.CurrentSelection.FirstOrDefault();
                    await Shell.Current.GoToAsync($"{nameof(NoteEntryPage)}?{nameof(NoteEntryPage.ItemId)}={note.Filename}");
                }
            }
        }
    }
    ```

    Этот код определяет функциональные возможности для `NotesPage`. При отображении страницы выполняется метод `OnAppearing`, который заполняет [`CollectionView`](xref:Xamarin.Forms.CollectionView) всеми заметками, полученными из локальной папки данных приложений. При нажатии кнопки [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) запускается обработчик событий `OnAddClicked`. Этот метод переходит к `NoteEntryPage`. При выборе элемента в `CollectionView` запускается обработчик событий `OnSelectionChanged`. Этот метод переходит к `NoteEntryPage` при условии, что выбран элемент [`CollectionView`](xref:Xamarin.Forms.CollectionView), передавая свойство `Filename` выбранного `Note` в качестве параметра запроса на страницу. Дополнительные сведения о навигации см. в разделе [Навигация](deepdive.md#navigation) статьи [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **NotesPage.xaml.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

    > [!WARNING]
    > В данный момент сборка приложения не будет выполнена из-за ошибок, которые будут исправлены в последующих шагах.

14. На **Панели решения** в проекте **Notes** разверните **AppShell.xaml** и откройте **AppShell.xaml.cs**. Затем замените существующий код следующим:

    ```csharp
    using Notes.Views;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class AppShell : Shell
        {
            public AppShell()
            {
                InitializeComponent();
                Routing.RegisterRoute(nameof(NoteEntryPage), typeof(NoteEntryPage));
            }
        }
    }
    ```

    Этот код регистрирует маршрут для `NoteEntryPage`, который не представлен в визуальной иерархии Оболочки. Затем на этой странице можно перейти к использованию навигации на основе URI с помощью метода `GoToAsync`.

    Сохраните изменения в файле **AppShell.xaml.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

15. На **Панели решения** разверните **App.xaml** и откройте **App.xaml.cs** в проекте **Notes**. Затем замените существующий код следующим:

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

    Этот код добавляет объявление пространства имен `System.IO` и объявление для статического свойства `FolderPath` типа `string`. Свойство `FolderPath` используется для хранения пути на устройстве, где будут храниться данные заметки. Кроме того, код инициализирует свойство `FolderPath` в конструкторе `App`, а также инициализирует свойство [`MainPage`](xref:Xamarin.Forms.Application.MainPage) в производном объекте `Shell`.

    Сохраните изменения в файле **App.xaml.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

16. Создайте и запустите проект на каждой соответствующей платформе. Дополнительные сведения см. в разделе [Сборка примера из краткого руководства](app.md#building-the-quickstart).

    На странице **NotesPage** нажмите кнопку **Добавить**, чтобы перейти к странице **NoteEntryPage** и ввести заметку. После сохранения заметки приложение вернется на страницу **NotesPage**.

    Введите несколько заметок разной длины, чтобы понаблюдать за поведением приложения. Закройте приложение и повторно запустите его, чтобы проверить, сохранены ли на устройстве введенные заметки.

::: zone-end

## <a name="next-steps"></a>Дальнейшие действия

В этом кратком руководстве рассматривались следующие темы:

- Добавление дополнительных страниц в приложение Оболочки в Xamarin.Forms.
- Навигация между страницами.
- Использование привязки данных для синхронизации данных между элементами пользовательского интерфейса и их источником данных.

Перейдите к следующему краткому руководству, чтобы изменить приложение таким образом, чтобы оно сохраняло свои данные в локальной базе данных SQLite.NET.

> [!div class="nextstepaction"]
> [Вперед](database.md)

## <a name="related-links"></a>Связанные ссылки

- [Заметки (пример)](/samples/xamarin/xamarin-forms-samples/getstarted-notes-navigation/)
- [Подробное изучение кратких руководств по Оболочке в Xamarin.Forms](deepdive.md)
