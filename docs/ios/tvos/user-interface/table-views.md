---
title: Работа с табличными представлениями tvOS в Xamarin
description: В этой статье рассматривается проектирование и работа с табличными представлениями и контроллерами представлений таблиц в приложении Xamarin. tvOS.
ms.prod: xamarin
ms.assetid: D8F80FA9-6400-4DB7-AFC9-A28A54AD04E8
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: eb95970066f85bb62eb207f23cfc135dc1625a11
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937102"
---
# <a name="working-with-tvos-table-views-in-xamarin"></a>Работа с табличными представлениями tvOS в Xamarin

_В этой статье рассматривается проектирование и работа с табличными представлениями и контроллерами представлений таблиц в приложении Xamarin. tvOS._

В tvOS представление таблицы представляется в виде одного столбца прокручиваемых строк, который при необходимости может быть организован в группы или разделы. Табличные представления следует использовать, когда необходимо эффективно отображать большой объем данных для пользователя, чтобы понять, как это удобно.

Табличные представления обычно отображаются на одной стороне [разделенного представления](~/ios/tvos/user-interface/split-views.md) в виде навигации с подробными сведениями о выбранном элементе, отображаемом на противоположной стороне:

[![Пример представления таблицы](table-views-images/intro01.png)](table-views-images/intro01.png#lightbox)

<a name="About-Table-Views"></a>

## <a name="about-table-views"></a>Сведения о табличных представлениях

`UITableView`Отображает один столбец прокручиваемых строк в виде иерархического списка данных, которые при необходимости можно объединить в группы или разделы: 

[![Выбранный элемент](table-views-images/table01.png)](table-views-images/table01.png#lightbox)

У Apple есть следующие рекомендации по работе с таблицами:

- **Обратите внимание, что ширина** — попробуйте задержать правильный баланс в ширине таблицы. Если таблица слишком широкая, ее может быть трудно сканировать с расстояния и убрать из области доступные материалы. Если таблица является слишком узким, она может привести к усечению или переносу информации, что может быть трудной задачей для чтения данных из комнаты.
- **Быстрое отображение содержимого таблицы** . для больших списков данных следует выполнить отложенную загрузку содержимого и начать отображение информации сразу после представления таблицы пользователю. Если загрузка таблицы занимает много времени, пользователь может потерять интерес в приложении или подумать, что он заблокирован.
- **Информировать пользователя о длительных загрузках содержимого** . Если время загрузки таблицы не позволяет избежать, представьте [индикатор выполнения или индикатора активности](~/ios/tvos/user-interface/progress-indicators.md) , чтобы убедиться, что приложение не заблокировано.

<a name="Table-Cell-Types"></a>

## <a name="table-view-cell-types"></a>Типы ячеек табличного представления

Объект `UITableViewCell` используется для представления отдельных строк данных в табличном представлении. Компания Apple определила несколько типов ячеек таблицы по умолчанию:

- **По умолчанию** . Этот тип представляет изображение параметра в левой части ячейки, а заголовок с левой границей справа. 
- **Подзаголовок** . Этот тип представляет заголовок, выравниваемая по левому краю, в первой строке и меньшее правое подзаголовок на следующей строке.
- **Значение 1** . Этот тип представляет заголовок с горизонтальным вычеркиванием с более светлой линией подзаголовка на той же строке.
- **Значение 2** . Этот тип представляет заголовок с горизонтальным вычеркиванием в той же строке.

Все типы ячеек табличного представления по умолчанию также поддерживают графические элементы, такие как индикаторы раскрытия или галочки. 

Кроме того, можно определить тип ячейки **пользовательского** представления таблицы и предоставить _ячейку прототипа_, созданную в конструкторе интерфейса или с помощью кода.

У Apple есть следующие рекомендации по работе с ячейками табличного представления:

- **Избегайте обрезки текста** — Сократите отдельные строки текста, чтобы они не были обрезаны. Обрезанные слова или фразы трудно проанализировать из комнаты.
- **Обратите внимание на состояние строки с фокусом** — так как строка больше, а с более скругленными углами при фокусе, необходимо протестировать внешний вид ячейки во всех состояниях. Изображения или текст могут стать обрезанными или выглядеть неправильными в состоянии "с упором".
- **Использование изменяемых таблиц с осторожностью** — перемещение или удаление строк таблицы занимает больше времени, чем в tvOS, чем в iOS. Необходимо тщательно решить, если эта функция будет добавляться или отвлекаться от приложения tvOS.
- **Создавайте пользовательские типы ячеек, где это уместно** , хотя встроенные типы ячеек табличного представления отлично подходят для многих ситуаций, рассмотрите возможность создания пользовательских типов ячеек для нестандартных сведений, чтобы обеспечить более полный контроль и лучше представлять информацию пользователю.

<a name="Working-With-Table-Views"></a>

## <a name="working-with-table-views"></a>Работа с табличными представлениями

Самым простым способом работы с табличными представлениями в приложении Xamarin. tvOS является создание и изменение их внешнего вида в конструкторе интерфейсов.

Чтобы начать работу, выполните следующие действия:

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. В Visual Studio для Mac запустите новый проект приложения tvOS и выберите приложение **tvOS**  >  **приложение**с  >  **единым представлением** и нажмите кнопку **Далее** : 

    [![Выбор приложения с одним представлением](table-views-images/table02.png)](table-views-images/table02.png#lightbox)
1. Введите **имя** приложения и нажмите кнопку **Далее**: 

    [![Введите имя приложения](table-views-images/table03.png)](table-views-images/table03.png#lightbox)
1. Измените **имя проекта** и **имя решения** или примите значения по умолчанию и нажмите кнопку **создать** , чтобы создать новое решение. 

    [![Имя проекта и имя решения](table-views-images/table04.png)](table-views-images/table04.png#lightbox)
1. В **панель решения**дважды щелкните `Main.storyboard` файл, чтобы открыть его в конструкторе iOS: 

    [![Главный файл. Storyboard](table-views-images/table05.png)](table-views-images/table05.png#lightbox)
1. Выберите и удалите **контроллер представления по умолчанию**: 

    [![Выбор и удаление контроллера представления по умолчанию](table-views-images/table06.png)](table-views-images/table06.png#lightbox)
1. Выберите **контроллер разделенного представления** из **панели элементов** и перетащите его на область конструктора.
1. По умолчанию вы получите [разделенное представление](~/ios/tvos/user-interface/split-views.md) с **контроллером представления навигации** и **контроллером представления таблицы** в левой части и **контроллером представления** в правой части. Это рекомендуемое использование представления таблицы в tvOS в Apple: 

    [![Добавление разделенного представления](table-views-images/table08.png)](table-views-images/table08.png#lightbox)
1. Необходимо будет выбрать каждую часть табличного представления и назначить ей **имя пользовательского класса** на вкладке **мини** -приложение **обозревателя свойств** , чтобы к нему можно было обращаться позже в коде C#. Например, **контроллер представления таблицы**: 

    [![Назначение имени класса](table-views-images/table09.png)](table-views-images/table09.png#lightbox)
1. Убедитесь, что создан пользовательский класс для **контроллера представления таблицы**, **представления таблицы** и всех **ячеек прототипа**. Visual Studio для Mac добавит пользовательские классы в дерево проекта при их создании: 

    [![Пользовательские классы в дереве проекта](table-views-images/table10.png)](table-views-images/table10.png#lightbox)
1. Затем выберите табличное представление в область конструктора и при необходимости измените его свойства. Например, число **ячеек прототипа** и **стиль** (обычный или сгруппированный): 

    [![Вкладка мини-приложения](table-views-images/table11.png)](table-views-images/table11.png#lightbox)
1. Для каждой **ячейки прототипа**выберите ее и назначьте уникальный **идентификатор** на вкладке **мини** -приложение **обозревателя свойств**. Этот шаг _очень важен_ , так как этот идентификатор потребуется позже при заполнении таблицы. Например, `AttrCell`: 

    [![Вкладка мини-приложения](table-views-images/table12.png)](table-views-images/table12.png#lightbox)
1. Можно также выбрать отображение ячейки в виде одного из [типов ячеек табличного представления по умолчанию](#table-view-cell-types) через раскрывающийся список **стилей** или задать его как **Настраиваемый** и использовать область конструктора для разметки ячейки, ПЕРЕТАЩИВ другие мини-приложения пользовательского интерфейса из **панели элементов**: 

    [![Макет ячейки](table-views-images/table13.png)](table-views-images/table13.png#lightbox)
1. Назначьте уникальное **имя** каждому ЭЛЕМЕНТУ пользовательского интерфейса в макете ячеек прототипа на вкладке **мини** -приложение **обозревателя свойств** , чтобы к ним можно было обращаться позже в коде C#: 

    [![Присвоение имени](table-views-images/table14.png)](table-views-images/table14.png#lightbox)
1. Повторите описанный выше шаг для всех ячеек прототипа в представлении таблицы.
1. Затем назначьте пользовательские классы остальным элементам пользовательского интерфейса, разработайте представление подробностей и назначьте уникальные **имена** каждому ЭЛЕМЕНТУ пользовательского интерфейса в представлении Details, чтобы иметь возможность обращаться к ним в C#. Пример: 

    [![Макет пользовательского интерфейса](table-views-images/table15.png)](table-views-images/table15.png#lightbox)
1. Сохраните изменения в раскадровке.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. В Visual Studio запустите новый проект приложения tvOS и выберите **tvOS**  >  **Single View App (приложение с одним представлением** ) и введите имя приложения. Нажмите кнопку " **ОК** ", чтобы создать новое решение: 

    [![Выбор приложения с одним представлением](table-views-images/table02-vs.png)](table-views-images/table02-vs.png#lightbox)
1. В **Обозреватель решений**дважды щелкните `Main.storyboard` файл, чтобы открыть его в конструкторе iOS: 

    [![Главный файл. Storyboard](table-views-images/table05-vs.png)](table-views-images/table05-vs.png#lightbox)
1. Выберите и удалите **контроллер представления по умолчанию**: 

    [![Выбор и удаление контроллера представления по умолчанию](table-views-images/table06-vs.png)](table-views-images/table06-vs.png#lightbox)
1. Выберите **контроллер разделенного представления** из **панели элементов** и перетащите его на область конструктора: 

    [![Контроллер разделенного представления](table-views-images/table07-vs.png)](table-views-images/table07-vs.png#lightbox)
1. По умолчанию вы получите [разделенное представление](~/ios/tvos/user-interface/split-views.md) с **контроллером представления навигации** и **контроллером представления таблицы** в левой части и **контроллером представления** в правой части. Это рекомендуемое использование представления таблицы в tvOS в Apple: 

    [![Макет пользовательского интерфейса](table-views-images/table08-vs.png)](table-views-images/table08-vs.png#lightbox)
1. Необходимо будет выбрать каждую часть табличного представления и назначить ей **имя пользовательского класса** на вкладке **мини** -приложение **обозревателя свойств** , чтобы к нему можно было обращаться позже в коде C#. Например, **контроллер представления таблицы**: 

    [![Вкладка мини-приложения](table-views-images/table09-vs.png)](table-views-images/table09-vs.png#lightbox)
1. Убедитесь, что создан пользовательский класс для **контроллера представления таблицы**, **представления таблицы** и всех **ячеек прототипа**. Visual Studio для Mac добавит пользовательские классы в дерево проекта при их создании: 

    [![Пользовательские классы в дереве проекта](table-views-images/table10-vs.png)](table-views-images/table10-vs.png#lightbox)
1. Затем выберите табличное представление в область конструктора и при необходимости измените его свойства. Например, число **ячеек прототипа** и **стиль** (обычный или сгруппированный): 

    [![Вкладка мини-приложения](table-views-images/table11-vs.png)](table-views-images/table11-vs.png#lightbox)
1. Для каждой **ячейки прототипа**выберите ее и назначьте уникальный **идентификатор** на вкладке **мини** -приложение **обозревателя свойств**. Этот шаг _очень важен_ , так как этот идентификатор потребуется позже при заполнении таблицы. Например, `AttrCell`: 

    [![Назначение идентификатора](table-views-images/table12-vs.png)](table-views-images/table12-vs.png#lightbox)
1. Можно также выбрать отображение ячейки в виде одного из [типов ячеек табличного представления по умолчанию](#table-view-cell-types) через раскрывающийся список **стилей** или задать его как **Настраиваемый** и использовать область конструктора для разметки ячейки, ПЕРЕТАЩИВ другие мини-приложения пользовательского интерфейса из **панели элементов**: 

    [![Раскрывающийся список стилей](table-views-images/table13-vs.png)](table-views-images/table13-vs.png#lightbox)
1. Назначьте уникальное **имя** каждому ЭЛЕМЕНТУ пользовательского интерфейса в макете ячеек прототипа на вкладке **мини** -приложение **обозревателя свойств** , чтобы к ним можно было обращаться позже в коде C#: 

    [![Вкладка мини-приложения](table-views-images/table14-vs.png)](table-views-images/table14-vs.png#lightbox)
1. Повторите описанный выше шаг для всех ячеек прототипа в представлении таблицы.
1. Затем назначьте пользовательские классы остальным элементам пользовательского интерфейса, разработайте представление подробностей и назначьте уникальные **имена** каждому ЭЛЕМЕНТУ пользовательского интерфейса в представлении Details, чтобы иметь возможность обращаться к ним в C#. Пример: 

    [![Макет пользовательского интерфейса](table-views-images/table15.png)](table-views-images/table15.png#lightbox)
1. Сохраните изменения в раскадровке.

-----

<a name="Designing-a-Data-Model"></a>

## <a name="designing-a-data-model"></a>Проектирование модели данных

Чтобы упростить работу с данными, которые будут отображаться в табличном представлении, и упростить представление подробных сведений (по мере выбора или выделения строк в табличном представлении пользователем), создайте пользовательский класс или классы, которые будут использоваться в качестве модели данных для представленной информации.

Возьмем пример приложения для резервирования поездок, содержащего список **городов**, каждый из которых содержит уникальный список **аттрактионс** , которые пользователь может выбрать. Пользователь сможет пометить притяжения как *избранный*, выбрать, чтобы получить *направления* к притяжения и поставлять *бюллетень о перелете* в заданный город.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Чтобы создать модель данных для **притяжения**, щелкните правой кнопкой мыши имя проекта в **панель решения** и выберите **добавить**  >  **новый файл...**. Введите `AttractionInformation` в поле **имя** и нажмите кнопку **New (создать** ): 

[![Введите Аттрактионинформатион для имени](table-views-images/data01.png)](table-views-images/data01.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Чтобы создать модель данных для **притяжения**, щелкните правой кнопкой мыши имя проекта в **Обозреватель решений** и выберите команду **добавить**  >  **новый элемент...**. Выберите **класс** и введите `AttractionInformation` в поле **имя** и нажмите кнопку **Добавить** : 

[![Выберите класс и введите Аттрактионинформатион в поле имя.](table-views-images/data01-vs.png)](table-views-images/data01-vs.png#lightbox)

-----

Измените `AttractionInformation.cs` файл и сделайте его следующим:

```csharp
using System;
using Foundation;

namespace tvTable
{
    public class AttractionInformation : NSObject
    {
        #region Computed Properties
        public CityInformation City { get; set;}
        public string Name { get; set;}
        public string Description { get; set;}
        public string ImageName { get; set;}
        public bool IsFavorite { get; set;}
        public bool AddDirections { get; set;}
        #endregion

        #region Constructors
        public AttractionInformation (string name, string description, string imageName)
        {
            // Initialize
            this.Name = name;
            this.Description = description;
            this.ImageName = imageName;
        }
        #endregion
    }
}
```

Этот класс предоставляет свойства для хранения сведений о заданном **притяжения**.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Затем щелкните правой кнопкой мыши имя проекта в **панель решения** и выберите команду **Добавить**  >  **новый файл...**. Введите `CityInformation` в поле **имя** и нажмите кнопку **New (создать** ): 

[![Введите Цитинформатион для имени](table-views-images/data02.png)](table-views-images/data02.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Затем щелкните правой кнопкой мыши имя проекта в **Обозреватель решений** и выберите команду **Добавить**  >  **новый элемент...**. Введите `CityInformation` для **имени** и нажмите кнопку **добавить** : 

[![Введите Цитинформатион для имени](table-views-images/data02-vs.png)](table-views-images/data02-vs.png#lightbox)

-----

Измените `CityInformation.cs` файл и сделайте его следующим:

```csharp
using System;
using System.Collections.Generic;
using Foundation;

namespace tvTable
{
    public class CityInformation : NSObject
    {
        #region Computed Properties
        public string Name { get; set; }
        public List<AttractionInformation> Attractions { get; set;}
        public bool FlightBooked { get; set;}
        #endregion

        #region Constructors
        public CityInformation (string name)
        {
            // Initialize
            this.Name = name;
            this.Attractions = new List<AttractionInformation> ();
        }
        #endregion

        #region Public Methods
        public void AddAttraction (AttractionInformation attraction)
        {
            // Mark as belonging to this city
            attraction.City = this;

            // Add to collection
            Attractions.Add (attraction);
        }

        public void AddAttraction (string name, string description, string imageName)
        {
            // Create attraction
            var attraction = new AttractionInformation (name, description, imageName);

            // Mark as belonging to this city
            attraction.City = this;

            // Add to collection
            Attractions.Add (attraction);
        }
        #endregion
    }
}
```

Этот класс содержит всю информацию о конечном **городе**, коллекцию **аттрактионс** для этого города и предоставляет два вспомогательных метода (), облегчающих `AddAttraction` Добавление аттрактионс в город.

<a name="The-Table-Data-Source"></a>

## <a name="the-table-view-data-source"></a>Источник данных табличного представления

Для каждого табличного представления необходимо, чтобы источник данных ( `UITableViewDataSource` ) предоставил данные для таблицы и создавал необходимые строки в соответствии с требованиями табличного представления.

В приведенном выше примере щелкните правой кнопкой мыши имя проекта в **Обозреватель решений**, выберите команду **Добавить**  >  **новый файл...** и вызовите его `AttractionTableDatasource` и нажмите кнопку **создать** . Затем измените `AttractionTableDatasource.cs` файл и сделайте его следующим:

```csharp
using System;
using System.Collections.Generic;
using UIKit;

namespace tvTable
{
    public class AttractionTableDatasource : UITableViewDataSource
    {
        #region Constants
        const string CellID = "AttrCell";
        #endregion

        #region Computed Properties
        public AttractionTableViewController Controller { get; set;}
        public List<CityInformation> Cities { get; set;}
        #endregion

        #region Constructors
        public AttractionTableDatasource (AttractionTableViewController controller)
        {
            // Initialize
            this.Controller = controller;
            this.Cities = new List<CityInformation> ();
            PopulateCities ();
        }
        #endregion

        #region Public Methods
        public void PopulateCities ()
        {
            // Clear existing
            Cities.Clear ();

            // Define cities and attractions
            var Paris = new CityInformation ("Paris");
            Paris.AddAttraction ("Eiffel Tower", "Is a wrought iron lattice tower on the Champ de Mars in Paris, France.", "EiffelTower");
            Paris.AddAttraction ("Musée du Louvre", "is one of the world's largest museums and a historic monument in Paris, France.", "Louvre");
            Paris.AddAttraction ("Moulin Rouge", "French for 'Red Mill', is a cabaret in Paris, France.", "MoulinRouge");
            Paris.AddAttraction ("La Seine", "Is a 777-kilometre long river and an important commercial waterway within the Paris Basin.", "RiverSeine");
            Cities.Add (Paris);

            var SanFran = new CityInformation ("San Francisco");
            SanFran.AddAttraction ("Alcatraz Island", "Is located in the San Francisco Bay, 1.25 miles (2.01 km) offshore from San Francisco.", "Alcatraz");
            SanFran.AddAttraction ("Golden Gate Bridge", "Is a suspension bridge spanning the Golden Gate strait between San Francisco Bay and the Pacific Ocean", "GoldenGateBridge");
            SanFran.AddAttraction ("San Francisco", "Is the cultural, commercial, and financial center of Northern California.", "SanFrancisco");
            SanFran.AddAttraction ("Telegraph Hill", "Is primarily a residential area, much quieter than adjoining North Beach.", "TelegraphHill");
            Cities.Add (SanFran);

            var Houston = new CityInformation ("Houston");
            Houston.AddAttraction ("City Hall", "It was constructed in 1938-1939, and is located in Downtown Houston.", "CityHall");
            Houston.AddAttraction ("Houston", "Is the most populous city in Texas and the fourth most populous city in the US.", "Houston");
            Houston.AddAttraction ("Texas Longhorn", "Is a breed of cattle known for its characteristic horns, which can extend to over 6 ft.", "LonghornCattle");
            Houston.AddAttraction ("Saturn V Rocket", "was an American human-rated expendable rocket used by NASA between 1966 and 1973.", "Rocket");
            Cities.Add (Houston);
        }
        #endregion

        #region Override Methods
        public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            // Get cell
            var cell = tableView.DequeueReusableCell (CellID) as AttractionTableCell;

            // Populate cell
            cell.Attraction = Cities [indexPath.Section].Attractions [indexPath.Row];

            // Return new cell
            return cell;
        }

        public override nint NumberOfSections (UITableView tableView)
        {
            // Return number of cities
            return Cities.Count;
        }

        public override nint RowsInSection (UITableView tableView, nint section)
        {
            // Return the number of attractions in the given city
            return Cities [(int)section].Attractions.Count;
        }

        public override string TitleForHeader (UITableView tableView, nint section)
        {
            // Get the name of the current city
            return Cities [(int)section].Name;
        }
        #endregion
    }
}
```

Давайте подробно рассмотрим несколько разделов класса.

Во-первых, мы определили константу для хранения уникального идентификатора ячейки прототипа (это тот же идентификатор, который был назначен в конструкторе интерфейсов выше), добавил ярлык обратно в контроллер представления таблицы и создал хранилище для наших данных:

```csharp
const string CellID = "AttrCell";
public AttractionTableViewController Controller { get; set;}
public List<CityInformation> Cities { get; set;}
```

Далее мы сохраняем контроллер представления таблицы, затем создаем и заполняем наш источник данных (используя модели данных, определенные выше) при создании класса:

```csharp
public AttractionTableDatasource (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
    this.Cities = new List<CityInformation> ();
    PopulateCities ();
}
```

В частности, `PopulateCities` метод просто создает объекты модели данных в памяти, однако их можно легко прочитать из базы данных или веб-службы в реальном приложении:

```csharp
public void PopulateCities ()
{
    // Clear existing
    Cities.Clear ();

    // Define cities and attractions
    var Paris = new CityInformation ("Paris");
    Paris.AddAttraction ("Eiffel Tower", "Is a wrought iron lattice tower on the Champ de Mars in Paris, France.", "EiffelTower");
    ...
}
```

`NumberOfSections`Метод возвращает число разделов в таблице:

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    // Return number of cities
    return Cities.Count;
}
```

Для представлений таблиц с **обычным** стилем всегда возвращает 1.

`RowsInSection`Метод возвращает число строк в текущем разделе:

```csharp
public override nint RowsInSection (UITableView tableView, nint section)
{
    // Return the number of attractions in the given city
    return Cities [(int)section].Attractions.Count;
}
```

Опять же, для **простых** табличных представлений возвращается общее число элементов в источнике данных.

`TitleForHeader`Метод возвращает заголовок для данного раздела:

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    // Get the name of the current city
    return Cities [(int)section].Name;
}
```

Для **простого** типа табличного представления оставьте заголовок пустым ( `""` ).

Наконец, по запросу табличного представления создайте и заполните ячейку прототипа с помощью `GetCell` метода: 

```csharp
public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    // Get cell
    var cell = tableView.DequeueReusableCell (CellID) as AttractionTableCell;

    // Populate cell
    cell.Attraction = Cities [indexPath.Section].Attractions [indexPath.Row];

    // Return new cell
    return cell;
}
```

Дополнительные сведения о работе с `UITableViewDatasource` см. в документации Apple [уитаблевиевдатасаурце](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40006941) .

<a name="The-Table-View-Delegate"></a>

## <a name="the-table-view-delegate"></a>Делегат представления таблицы

Для каждого табличного представления требуется делегат ( `UITableViewDelegate` ), отвечающий за взаимодействие с пользователем или другие системные события в таблице.

В приведенном выше примере щелкните правой кнопкой мыши имя проекта в **Обозреватель решений**, выберите команду **Добавить**  >  **новый файл...** и вызовите его `AttractionTableDelegate` и нажмите кнопку **создать** . Затем измените `AttractionTableDelegate.cs` файл и сделайте его следующим:

```csharp
using System;
using System.Collections.Generic;
using UIKit;

namespace tvTable
{
    public class AttractionTableDelegate : UITableViewDelegate
    {
        #region Computed Properties
        public AttractionTableViewController Controller { get; set;}
        #endregion

        #region Constructors
        public AttractionTableDelegate (AttractionTableViewController controller)
        {
            // Initializw
            this.Controller = controller;
        }
        #endregion

        #region Override Methods
        public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
            attraction.IsFavorite = (!attraction.IsFavorite);

            // Update UI
            Controller.TableView.ReloadData ();
        }

        public override bool CanFocusRow (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            // Inform caller of highlight change
            RaiseAttractionHighlighted (Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row]);
            return true;
        }
        #endregion

        #region Events
        public delegate void AttractionHighlightedDelegate (AttractionInformation attraction);
        public event AttractionHighlightedDelegate AttractionHighlighted;

        internal void RaiseAttractionHighlighted (AttractionInformation attraction)
        {
            // Inform caller
            if (this.AttractionHighlighted != null) this.AttractionHighlighted (attraction);
        }
        #endregion
    }
}
```

Подробнее рассмотрим несколько разделов этого класса.

Сначала мы создадим ярлык для контроллера представления таблицы при создании класса:

```csharp
public AttractionTableViewController Controller { get; set;}
...

public AttractionTableDelegate (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
}
```

Затем при выборе строки (пользователь щелкает на сенсорной поверхности удаленного Apple) мы хотим пометить **притяжения** , представленную выбранной строкой, в качестве избранной:

```csharp
public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
    attraction.IsFavorite = (!attraction.IsFavorite);

    // Update UI
    Controller.TableView.ReloadData ();
}
```

После того, как пользователь выделит строку (подавая ей фокус на пульте дистанционного управления Apple), мы хотим представить подробные сведения о **притяжения** , представленные этой строкой, в разделе подробностей нашего контроллера разделенного представления.

```csharp
public override bool CanFocusRow (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    // Inform caller of highlight change
    RaiseAttractionHighlighted (Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row]);
    return true;
}
...

public delegate void AttractionHighlightedDelegate (AttractionInformation attraction);
public event AttractionHighlightedDelegate AttractionHighlighted;

internal void RaiseAttractionHighlighted (AttractionInformation attraction)
{
    // Inform caller
    if (this.AttractionHighlighted != null) this.AttractionHighlighted (attraction);
}
```

`CanFocusRow`Метод вызывается для каждой строки, которая собирается получить фокус в табличном представлении. Возвращает `true` значение, если строка может получить фокус, иначе возвращается `false` . В этом примере мы создали пользовательское `AttractionHighlighted` событие, которое будет создано в каждой строке при получении фокуса.

Дополнительные сведения о работе с `UITableViewDelegate` см. в документации Apple [уитаблевиевделегате](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40006942) .

<a name="The-Table-View-Cell"></a>

## <a name="the-table-view-cell"></a>Ячейка табличного представления

Для каждой ячейки прототипа, добавленной в представление таблицы в конструкторе интерфейсов, также создается пользовательский экземпляр ячейки представления таблицы ( `UITableViewCell` ), позволяющий заполнить новую ячейку (строку) по мере ее создания.

Для примера приложения дважды щелкните `AttractionTableCell.cs` файл, чтобы открыть его для редактирования, и сделайте так, чтобы он выглядел следующим образом:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionTableCell : UITableViewCell
    {
        #region Private Variables
        private AttractionInformation _attraction = null;
        #endregion

        #region Computed Properties
        public AttractionInformation Attraction {
            get { return _attraction; }
            set {
                _attraction = value;
                UpdateUI ();
            }
        }
        #endregion

        #region Constructors
        public AttractionTableCell (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Private Methods
        private void UpdateUI ()
        {
            // Trap all errors
            try {
                Title.Text = Attraction.Name;
                Favorite.Hidden = (!Attraction.IsFavorite);
            } catch {
                // Since the UI might not be fully loaded, ignore
                // all errors at this point
            }
        }
        #endregion
    }
}
```

Этот класс предоставляет хранилище для объекта модели данных притяжения ( `AttractionInformation` как определено выше), отображаемого в заданной строке:

```csharp
private AttractionInformation _attraction = null;
...

public AttractionInformation Attraction {
    get { return _attraction; }
    set {
        _attraction = value;
        UpdateUI ();
    }
}
```

`UpdateUI`Метод заполняет мини-приложения пользовательского интерфейса (которые были добавлены в прототип ячейки в конструкторе интерфейсов) по мере необходимости:

```csharp
private void UpdateUI ()
{
    // Trap all errors
    try {
        Title.Text = Attraction.Name;
        Favorite.Hidden = (!Attraction.IsFavorite);
    } catch {
        // Since the UI might not be fully loaded, ignore
        // all errors at this point
    }
}
```

Дополнительные сведения о работе с `UITableViewCell` см. в документации Apple [уитаблевиевцелл](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewCell_Class/index.html#//apple_ref/doc/uid/TP40006938) .

<a name="The-Table-View-Controller"></a>

## <a name="the-table-view-controller"></a>Контроллер представления таблицы

Контроллер представления таблицы ( `UITableViewController` ) управляет табличным представлением, добавленным в раскадровку с помощью конструктора интерфейсов.

Для примера приложения дважды щелкните `AttractionTableViewController.cs` файл, чтобы открыть его для редактирования, и сделайте так, чтобы он выглядел следующим образом:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionTableViewController : UITableViewController
    {
        #region Computed Properties
        public AttractionTableDatasource Datasource {
            get { return TableView.DataSource as AttractionTableDatasource; }
        }

        public AttractionTableDelegate TableDelegate {
            get { return TableView.Delegate as AttractionTableDelegate; }
        }
        #endregion

        #region Constructors
        public AttractionTableViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Setup table
            TableView.DataSource = new AttractionTableDatasource (this);
            TableView.Delegate = new AttractionTableDelegate (this);
            TableView.ReloadData ();
        }
        #endregion
    }
}
```

Давайте подробнее рассмотрим этот класс. Сначала мы создали ярлыки, чтобы упростить доступ к таблицам `DataSource` и `TableDelegate` . Эти данные будут использоваться позже для взаимодействия между табличным представлением в левой части разделенного представления и подробным представлением сведений в правой части.

Наконец, когда табличное представление загружается в память, создаются экземпляры класса `AttractionTableDatasource` и `AttractionTableDelegate` (оба созданы выше) и присоединяются их к табличному представлению.

Дополнительные сведения о работе с `UITableViewController` см. в документации Apple [уитаблевиевконтроллер](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523) .

<a name="Pulling-it-All-Together"></a>

## <a name="pulling-it-all-together"></a>Совместное извлечение

Как указано в начале этого документа, представления таблиц обычно отображаются в одной стороне [разделенного представления](~/ios/tvos/user-interface/split-views.md) в виде навигации с подробными сведениями о выбранном элементе, отображаемом на противоположной стороне. Например. 

[![Запуск примера приложения](table-views-images/intro01.png)](table-views-images/intro01.png#lightbox)

Так как это стандартный шаблон в tvOS, давайте взглянем на заключительные шаги, чтобы объединить все вместе и обеспечить взаимодействие левой и правой сторон представления с разделением друг с другом.

<a name="The-Detail-View"></a>

### <a name="the-detail-view"></a>Подробное представление

В приведенном выше примере приложения для командировок для `AttractionViewController` стандартного контроллера представления, представленного в правой части разделенного представления, в качестве подробного представления определен пользовательский класс ().

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionViewController : UIViewController
    {
        #region Private Variables
        private AttractionInformation _attraction = null;
        #endregion

        #region Computed Properties
        public AttractionInformation Attraction {
            get { return _attraction; }
            set {
                _attraction = value;
                UpdateUI ();
            }
        }

        public MasterSplitView SplitView { get; set;}
        #endregion

        #region Constructors
        public AttractionViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Public Methods
        public void UpdateUI ()
        {
            // Trap all errors
            try {
                City.Text = Attraction.City.Name;
                Title.Text = Attraction.Name;
                SubTitle.Text = Attraction.Description;

                IsFlighBooked.Hidden = (!Attraction.City.FlightBooked);
                IsFavorite.Hidden = (!Attraction.IsFavorite);
                IsDirections.Hidden = (!Attraction.AddDirections);
                BackgroundImage.Image = UIImage.FromBundle (Attraction.ImageName);
                AttractionImage.Image = BackgroundImage.Image;
            } catch {
                // Since the UI might not be fully loaded, ignore
                // all errors at this point
            }
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Ensure the UI Updates
            UpdateUI ();
        }
        #endregion

        #region Actions
        partial void BookFlight (NSObject sender)
        {
            // Ask user to book flight
            AlertViewController.PresentOKCancelAlert ("Book Flight",
                                                      string.Format ("Would you like to book a flight to {0}?", Attraction.City.Name),
                                                      this,
                                                      (ok) => {
                Attraction.City.FlightBooked = ok;
                IsFlighBooked.Hidden = (!Attraction.City.FlightBooked);
            });
        }

        partial void GetDirections (NSObject sender)
        {
            // Ask user to add directions
            AlertViewController.PresentOKCancelAlert ("Add Directions",
                                                     string.Format ("Would you like to add directions to {0} to you itinerary?", Attraction.Name),
                                                     this,
                                                     (ok) => {
                                                         Attraction.AddDirections = ok;
                                                         IsDirections.Hidden = (!Attraction.AddDirections);
                                                     });
        }

        partial void MarkFavorite (NSObject sender)
        {
            // Flip favorite state
            Attraction.IsFavorite = (!Attraction.IsFavorite);
            IsFavorite.Hidden = (!Attraction.IsFavorite);

            // Reload table
            SplitView.Master.TableController.TableView.ReloadData ();
        }
        #endregion
    }
}
```

Здесь мы предуказали, что **притяжения** ( `AttractionInformation` ) отображается как свойство и создал `UpdateUI` метод, который заполняет мини-приложения пользовательского интерфейса, добавленные в представление в конструкторе интерфейсов.

Мы также определили ярлык к контроллеру разделенного представления ( `SplitView` ), который будет использоваться для передачи изменений обратно в представление таблицы ( `AcctractionTableView` ).

Наконец, в три экземпляра, созданные в конструкторе интерфейсов, были добавлены пользовательские действия (события), `UIButton` позволяющие пользователю помечать притяжения в качестве _избранного_, получать _указания_ по притяжения и _Book рейс_ в заданный город.

<a name="The-Navigation-View-Controller"></a>

### <a name="the-navigation-view-controller"></a>Контроллер представления навигации

Поскольку контроллер представления таблицы вложен в контроллер представления навигации в левой части разделенного представления, контроллеру представления навигации был назначен пользовательский класс ( `MasterNavigationController` ) в конструкторе интерфейсов, который определяется следующим образом:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class MasterNavigationController : UINavigationController
    {
        #region Computed Properties
        public MasterSplitView SplitView { get; set;}
        public AttractionTableViewController TableController {
            get { return TopViewController as AttractionTableViewController; }
        }
        #endregion

        #region Constructors
        public MasterNavigationController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

Опять же, этот класс просто определяет несколько сочетаний клавиш, чтобы упростить взаимодействие между двумя сторонами контроллера разделенного представления:

- `SplitView`— Ссылка на контроллер разделенного представления ( `MainSpiltViewController` ), к которому относится контроллер представления навигации.
- `TableController`— Получает контроллер представления таблицы ( `AttractionTableViewController` ), представленный в качестве верхнего представления в контроллере представления навигации.

<a name="The-Split-View-Controller"></a>

### <a name="the-split-view-controller"></a>Контроллер разделенного представления

Поскольку контроллер разделенного представления является основой нашего приложения, мы создали пользовательский класс ( `MasterSplitViewController` ) для него в конструкторе интерфейсов и определили его следующим образом:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class MasterSplitView : UISplitViewController
    {
        #region Computed Properties
        public AttractionViewController Details {
            get { return ViewControllers [1] as AttractionViewController; }
        }

        public MasterNavigationController Master {
            get { return ViewControllers [0] as MasterNavigationController; }
        }
        #endregion

        #region Constructors
        public MasterSplitView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            Master.SplitView = this;
            Details.SplitView = this;

            // Wire-up events
            Master.TableController.TableDelegate.AttractionHighlighted += (attraction) => {
                // Display new attraction
                Details.Attraction = attraction;
            };
        }
        #endregion
    }
}
```

Во-первых, мы создаем ярлыки для области **сведений** в разделенном представлении ( `AttractionViewController` ) и на стороне **главного** ( `MasterNavigationController` ). Опять же, это упрощает обмен данными между двумя сторонами позже.

Затем, когда представление с разделением загружается в память, мы подключим контроллер разделенного представления к обеим сторонам разделенного представления и реагируем на пользователя, выделяющий притяжения в табличном представлении ( `AttractionHighlighted` ), отображая новый притяжения в области **сведений** в представлении с разделением.

Ознакомьтесь с примером приложения [твтаблес](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvtable) для полной реализации табличных представлений в представлении с разделением.

## <a name="table-views-in-detail"></a>Подробное представление таблицы

Так как tvOS основано на iOS, представления таблиц и контроллеров представлений таблиц спроектированы и работают аналогичным образом. Более подробные сведения о работе с табличным представлением в приложении Xamarin см. в документации по [работе с таблицами и ячейками](~/ios/user-interface/controls/tables/index.md) в iOS.

<a name="Summary"></a>

## <a name="summary"></a>Сводка

В этой статье рассматривается проектирование и работа с табличными представлениями в приложении Xamarin. tvOS. И представил пример работы с табличным представлением в представлении с разделением, которое является типичным использованием табличного представления в приложении tvOS.

## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [уитаблевиевконтроллер](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523)
- [tvOS](https://developer.apple.com/tvos/)
- [Руководства по tvOSму интерфейсу](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Руководством по программированию приложений для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
