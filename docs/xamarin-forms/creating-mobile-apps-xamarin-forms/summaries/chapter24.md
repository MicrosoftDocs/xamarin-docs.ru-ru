---
title: "Сводная информация о главе 24 (Переход по страницам)" description: "Создание мобильных приложений с помощью Xamarin.Forms: Сводная информация о главе 24 (Переход по страницам)" ms.prod: xamarin ms.technology: xamarin-forms ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23 author: davidbritch ms.author: dabritch ms.date: 11/07/2017 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="summary-of-chapter-24-page-navigation"></a>Сводная информация о главе 24. Переход по страницам

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)

Многие приложения состоят из нескольких страниц, по которым переходит пользователь. У приложения всегда есть *главная* или *домашняя* страница, с которой пользователь переходит на другие страницы, которые помещаются в стек для переходов назад. Дополнительные варианты навигации описаны в главе 25 [ **Виды страниц**](chapter25.md).

## <a name="modal-pages-and-modeless-pages"></a>Модальные страницы и немодальные страницы

`VisualElement` определяет свойство [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) с типом [`INavigation`](xref:Xamarin.Forms.INavigation), который включает следующие два метода для перехода на новую страницу:

- [`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page))
- [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page))

Оба метода принимают в качестве аргумента экземпляр `Page` и возвращают объект `Task`. Следующие два метода возвращают пользователя на предыдущую страницу.

- [`PopAsync`](xref:Xamarin.Forms.INavigation.PopAsync)
- [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync)

Если в пользовательском интерфейсе есть собственная кнопка **Назад** (например, на смартфонах Android и Windows), вызывать эти методы в приложении нет необходимости.

Хотя эти методы доступны в любом классе `VisualElement`, обычно они вызываются из свойства `Navigation` текущего экземпляра `Page`.

Приложения обычно используют модальные страницы, если пользователь должен предоставить на странице некоторую информацию, прежде чем вернуться на предыдущую. Страницы, которые не являются модальными, иногда называют *иерархическими*. По самой странице нет возможности определить, является ли она модальной, ведь эта разница полностью определяется методом, вызванным для перехода к странице. Чтобы правильно работать на всех платформах, модальная страница должна предоставлять собственный пользовательский интерфейс для перехода на предыдущую страницу.

Пример [**ModelessAndModal**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal) позволяет изучить разницу между модальными и немодальными страницами. Любое приложение со страничной системой навигации должно передавать свою домашнюю страницу конструктору [`NavigationPage`](xref:Xamarin.Forms.NavigationPage), обычно в классе `App` программы. Это удобно хотя бы тем, что теперь не нужно задавать `Padding` на странице для iOS.

Вы обнаружите, что для немодальных страниц отображается свойство страницы [`Title`](xref:Xamarin.Forms.Page.Title). Платформы iOS, Android и Windows для планшетов и настольных компьютеров предоставляют элемент пользовательского интерфейса для возврата на предыдущую страницу. Кроме того, на устройствах Android и Windows Phone для возврата есть стандартная кнопка **Назад**.

Для модальных страниц страница `Title` не отображается, и элемент пользовательского интерфейса для возврата на предыдущую страницу отсутствует. На смартфонах Android и Windows вы можете использовать стандартную кнопку **Назад**, чтобы вернуться на предыдущую страницу, но для других платформ модальная страница должна предоставить собственный механизм возврата.

### <a name="animated-page-transitions"></a>Анимация переходов по страницам

Альтернативные версии методов навигации предоставляются с дополнительным логическим аргументом, которому вы присваиваете значение `true`, если хотите применить анимацию к переходу между страницами:

- [PushAsync](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page,System.Boolean))
- [PushModalAsync](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page,System.Boolean))
- [PopAsync](xref:Xamarin.Forms.INavigation.PopAsync(System.Boolean));
- [PopModalAsync](xref:Xamarin.Forms.INavigation.PopModalAsync(System.Boolean)).

Но стандартные методы навигации по страницам по умолчанию всегда используют анимацию, так что эта возможность полезна только для перехода к определенной странице при запуске приложения (как описано в конце этой главы) или для реализации собственной анимации входа (как описано в главе 22 [ **Анимация**](chapter22.md)).

### <a name="visual-and-functional-variations"></a>Визуальные и функциональные варианты

`NavigationPage` содержит два свойства, которые можно задать при создании экземпляра класса в методе `App`:

- [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)
- [`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor)

Кроме того, `NavigationPage` включает четыре присоединенных привязываемых свойства, которые влияют на определенную страницу, для которой они заданы:

- [`SetHasBackButton`](xref:Xamarin.Forms.NavigationPage.SetHasBackButton(Xamarin.Forms.Page,System.Boolean)) и [`GetHasBackButton`](xref:Xamarin.Forms.NavigationPage.GetHasBackButton(Xamarin.Forms.Page))
- [`SetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.SetHasNavigationBar(Xamarin.Forms.BindableObject,System.Boolean)) и [`GetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.GetHasNavigationBar(Xamarin.Forms.BindableObject))
- [`SetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.SetBackButtonTitle(Xamarin.Forms.BindableObject,System.String)) and [`GetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.GetBackButtonTitle(Xamarin.Forms.BindableObject)) (только для iOS)
- [`SetTitleIcon`](xref:Xamarin.Forms.NavigationPage.SetTitleIcon(Xamarin.Forms.BindableObject,Xamarin.Forms.FileImageSource)) и [`GetTitleIcon`](xref:Xamarin.Forms.NavigationPage.GetTitleIcon(Xamarin.Forms.BindableObject)) (только для iOS и Android)

### <a name="exploring-the-mechanics"></a>Изучение механизма работы

Все методы навигации по страницам являются асинхронными и должны использоваться только в сочетании с `await`. Завершение метода не означает, что переход на новую страницу завершен, а лишь разрешает безопасно изучать стек навигации по страницам.

При переходе с одной страницы на другую первая страница обычно получает вызов метода [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing), а вторая — метода [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing). Аналогичным образом, при возврате с одной страницы на другую первая страница обычно получает вызов метода `OnDisappearing`, а вторая — метода `OnAppearing`. Порядок этих вызовов и время завершения методов, которые вызвали эту навигацию, зависят от платформы. Слово "обычно" в двух предшествующих заявлениях появилось из-за навигации по модальным страницам в Android, где эти методы не вызываются.

Кроме того, вызов методов `OnAppearing` и `OnDisappearing` не всегда связан с навигацией между страницами.

Интерфейс `INavigation` содержит два свойства коллекции, которые позволяют изучать стек навигации:

- [`NavigationStack`](xref:Xamarin.Forms.INavigation.NavigationStack) с типом `IReadOnlyList<Page>` для немодального стека;
- [`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack) с типом `IReadOnlyList<Page>` для модального стека.

Самый безопасный вариант доступа к стекам — из свойства `Navigation` в классе `NavigationPage` (в котором должно находиться свойство [`MainPage`](xref:Xamarin.Forms.Application.MainPage) класса `App`). Изучение стеков считается безопасным только после завершения работы асинхронных методов навигации между страницами. Свойство [`CurrentPage`](xref:Xamarin.Forms.NavigationPage.CurrentPage) класса `NavigationPage` не соответствует текущей странице, если текущая страница является модальной. В такой ситуации это свойство соответствует последней немодальной странице.

Пример [**SinglePageNavigation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation) позволяет изучить работу со стеками и навигацией по страницам, а также допустимые типы переходов:

- с немодальной страницы можно переходить на другую немодальную или модальную страницу;
- с модальной страницы можно переходить только на другую модальную страницу.

### <a name="enforcing-modality"></a>Принудительное применение модальности

Приложение использует модальную страницу, когда ему требуется некоторая информация от пользователя. У пользователя не должно быть возможности вернуться на предыдущую страницу, пока эта информация не будет предоставлена. На iOS очень легко создать кнопку **Назад** и включать ее только после завершения работы на текущей странице. Но на смартфонах Android и Windows приложение должно переопределить метод [`OnBackButtonPressed`](xref:Xamarin.Forms.Page.OnBackButtonPressed) и возвращать `true`, если программа самостоятельно обработала нажатие кнопки **Назад**, как показано в примере [**ModalEnforcement**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement).

Пример [**MvvmEnforcement**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) демонстрирует работу этого механизма для сценария MVVM.

## <a name="navigation-variations"></a>Варианты навигации

Если на определенную модальную страницу можно перейти несколько раз, она должна сохранять введенную ранее информацию, чтобы пользователь мог изменить ее, не вводя все заново. Чтобы реализовать такое поведение, вы можете сохранять конкретный экземпляр модальной страницы, но гораздо лучше (особенно на платформе iOS) сохранять информацию в модели представления.

### <a name="making-a-navigation-menu"></a>Создание меню навигации

Пример [**ViewGalleryType**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType) демонстрирует использование `TableView` для перечисления элементов меню. Каждый элемент списка связан с объектом `Type`, который представляет определенную страницу. При выборе определенного элемента программа создает экземпляр соответствующей страницы и переходит к ней.

[![Три снимка экрана с представлением типа коллекции](images/ch24fg21-small.png "TableView со списком элементов меню")](images/ch24fg21-large.png#lightbox "TableView со списком элементов меню")

Пример [**ViewGalleryInst**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst) немного отличается тем, что меню содержит не типы, а экземпляры каждой страницы. Это помогает сохранять информацию на каждой странице, но требует создавать экземпляры всех страниц при запуске программы.

### <a name="manipulating-the-navigation-stack"></a>Управление стеком навигации

Пример [**StackManipulation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation) демонстрирует несколько функций, которые определены в `INavigation` и позволяют вам упорядоченно управлять стеком навигации:

- [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page))
- [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page))
- [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync) и [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync(System.Boolean)) с необязательной анимацией.

### <a name="dynamic-page-generation"></a>Динамическое создание страниц

Пример [**BuildAPage**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage) демонстрирует создание страницы в среде выполнения в ответ на ввод данных пользователем.

## <a name="patterns-of-data-transfer"></a>Шаблоны передачи данных

Часто бывает необходимо передавать данные между страницами, например на вызываемую страницу или обратно на ту страницу, с которой была вызвана текущая. Для этого есть несколько способов.

### <a name="constructor-arguments"></a>Аргументы конструктора

При переходе на новую страницу можно создать экземпляр класса страницы с помощью аргумента конструктора, который позволяет странице инициализировать значения параметров. Этот подход демонстрируется в примере [**SchoolAndStudents**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents). Кроме того, для вызываемой страницы можно задать класс `BindingContext` из той страницы, которая ее вызывает.

### <a name="properties-and-method-calls"></a>Свойства и вызовы методов

Остальные примеры передачи данных посвящены изучению проблемы с передачей данных между страницами в тех случаях, когда пользователь переходит с одной страницы на другую и обратно. В этих обсуждениях страница *home* выполняет переход на страницу *info* и должна передавать инициализированную информацию на страницу *info*. Страница *info* получает дополнительную информацию от пользователя и передает эту информацию на страницу *home*.

Страница *home* может легко применить общие методы и свойства страницы *info* после того, как создаст ее экземпляр. Страница *info* также может применить общие методы и свойства страницы *home*, но здесь сложнее выбрать правильный момент. Пример [**DateTransfer1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1) выполняет эти действия в переопределении `OnDisappearing`. Это плохо тем, что страница *info* должна иметь сведения о типе страницы *home*.

### <a name="messagingcenter"></a>MessagingCenter

Класс [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) в Xamarin.Forms дает еще одну возможность организовать взаимодействие между двумя страницами. Сообщения идентифицируются по текстовой строке и могут дополняться объектами.

Программа, которая намерена получать сообщения определенного типа, должна подписаться на них с помощью [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) и указать функцию обратного вызова. Позже она может отменить подписку, вызвав [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*). Функция обратного вызова будет получать все сообщения, отправленные из указанного типа с указанным именем через метод [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*).

Программа [**DateTransfer2**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2) демонстрирует процесс передачи данных через центр обмена сообщениями, но и в этом сценарии страница *info* должна знать тип страницы *home*.

### <a name="events"></a>События

События уже давно считаются общепризнанным способом передачи информации от одного класса другому, не выясняя тип второго класса. В примере [**DateTransfer3**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3) класс *info* определяет событие, которое срабатывает при наличии готовой информации. Однако, не существует удобного места, где страница *info* могла бы отсоединить обработчик событий.

### <a name="the-app-class-intermediary"></a>Класс-посредник App

В примере [**DateTransfer4**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4) демонстрируется, как страницы *home* и *info* используют свойства, которые определены в классе `App`. Это хорошее решение, но в следующем разделе есть вариант еще лучше.

### <a name="switching-to-a-viewmodel"></a>Переход на ViewModel

Использование ViewModel для хранения информации позволяет страницам *home* и *info* совместно использовать один экземпляр класса information. Это показано в примере [**DateTransfer5**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5).

### <a name="saving-and-restoring-page-state"></a>Сохранение и восстановление состояния страницы

Класс-посредник `App` в архитектуре ViewModel идеально подходит для ситуаций, когда приложению нужно сохранять информацию при переходе программы в спящий режим при активной странице *info*. Пример [**DateTransfer6**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6) демонстрирует такой сценарий.

## <a name="saving-and-restoring-the-navigation-stack"></a>Сохранение и восстановление стека навигации

В общем случае многостраничная программа, которая переходит в спящий режим, должна при восстановлении возвращаться на ту же страницу. Это означает, что такой программе нужно сохранять содержимое стека навигации. В этом разделе показано, как автоматизировать этот процесс в специально разработанном для таких целей классе. Этот класс также вызывает отдельные страницы, чтобы они могли сохранить и восстановить свое состояние.

В библиотеке [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) определен интерфейс с именем [`IPersistantPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs), который классы могут реализовать для сохранения данных в словаре `Properties` и восстановления их оттуда.

Класс [`MultiPageRestorableApp`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs) в библиотеке **Xamarin.FormsBook.Toolkit** наследует от `Application`. Вы можете наследовать свой класс `App` от `MultiPageRestorableApp` и выполнять в нем действия по обслуживанию.

В примере [**StackRestoreDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo) демонстрируется использование `MultiPageRestorableApp`.

### <a name="something-like-a-real-life-app"></a>Что-то похожее на настоящее приложение

Пример [**NoteTaker**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker) также использует `MultiPageRestorableApp` и позволяет вводить и редактировать заметки, которые сохраняются в словаре `Properties`.

## <a name="related-links"></a>Связанные ссылки

- [Глава 24, полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [Примеры для главы 24](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [Иерархическая навигация](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [Модальные страницы](~/xamarin-forms/app-fundamentals/navigation/modal.md)
