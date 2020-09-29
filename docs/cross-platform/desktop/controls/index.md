---
ms.assetid: 4D47185C-8998-4903-AE64-7E2A67F9DF7A
title: Сравнение элементов управления ИП
description: В этом документе представлено сравнение элементов управления пользовательского интерфейса между Xamarin. Forms, Windows Forms и WPF. Он также содержит ссылки на другую документацию, которая сравнивает WPF с Xamarin. Forms.
author: davidortinau
ms.author: daortin
ms.date: 04/26/2017
ms.openlocfilehash: b4cffd9e95f24dea9fc5fed5a6badeec624a4e25
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91453094"
---
# <a name="ui-controls-comparison"></a>Сравнение элементов управления ИП

Ниже приведено сравнение элементов управления Xamarin. Forms с Windows Forms и WPF на основе [этой таблицы](/dotnet/framework/wpf/advanced/windows-forms-controls-and-equivalent-wpf-controls).

Узнайте больше о [сходствах и различиях между WPF и Xamarin. Forms](wpf.md) , помогающими обновить знания о рабочем столе для разработки мобильных приложений.

|Windows Forms|WPF|Xamarin.Forms|
|--- |--- |--- |
|[BindingNavigator](/dotnet/api/system.windows.forms.bindingnavigator)|-|-|
|[BindingSource](/dotnet/api/system.windows.forms.bindingsource)|[CollectionViewSource](/dotnet/api/system.windows.data.collectionviewsource)|Свойство Binding, например. BindingContext|
|[Кнопка](/dotnet/api/system.windows.forms.button)|[Кнопка](/dotnet/api/system.windows.controls.button)|Кнопка|
|[CheckBox](/dotnet/api/system.windows.forms.checkbox)|[CheckBox](/dotnet/api/system.windows.controls.checkbox)|Коммутатор|
|[CheckedListBox](/dotnet/api/system.windows.forms.checkedlistbox)|[ListBox](/dotnet/api/system.windows.controls.listbox) с композицией.|ListView с композицией.|
|[ColorDialog](/dotnet/api/system.windows.forms.colordialog)|-|-|
|[ComboBox](/dotnet/api/system.windows.forms.combobox)|[ComboBox](/dotnet/api/system.windows.controls.combobox) (не поддерживает автозавершение)|Средство выбора|
|[ContextMenuStrip](/dotnet/api/system.windows.forms.contextmenustrip)|[ContextMenu](/dotnet/api/system.windows.controls.contextmenu)|-|
|[DataGridView](/dotnet/api/system.windows.forms.datagridview)|[DataGrid](/dotnet/api/system.windows.controls.datagrid)|-|
|[DateTimePicker](/dotnet/api/system.windows.forms.datetimepicker)|[DatePicker](/dotnet/api/system.windows.controls.datepicker)|DatePicker & TimePicker|
|[DomainUpDown](/dotnet/api/system.windows.forms.domainupdown)|[Текстовое поле](/dotnet/api/system.windows.controls.textbox) и два элемента управления [RepeatButton](/dotnet/api/system.windows.controls.primitives.repeatbutton) .|Шаговый переключатель|
|[ErrorProvider](/dotnet/api/system.windows.forms.errorprovider)|-|-|
|[FlowLayoutPanel](/dotnet/api/system.windows.forms.flowlayoutpanel)|[WrapPanel](/dotnet/api/system.windows.controls.wrappanel) или [StackPanel](/dotnet/api/system.windows.controls.stackpanel)|StackLayout или Флекслайаут|
|[FolderBrowserDialog](/dotnet/api/system.windows.forms.folderbrowserdialog)|-|-|
|[FontDialog](/dotnet/api/system.windows.forms.fontdialog)|-|-|
|[Form](/dotnet/api/system.windows.forms.form)|[Окно](/dotnet/api/system.windows.window)|Страница|
|[GroupBox](/dotnet/api/system.windows.forms.groupbox)|[GroupBox](/dotnet/api/system.windows.controls.groupbox)|-|
|[HelpProvider](/dotnet/api/system.windows.forms.helpprovider)|Нет эквивалентного элемента управления (Используйте подсказки).|-|
|[HScrollBar](/dotnet/api/system.windows.forms.hscrollbar)|[ScrollBar](/dotnet/api/system.windows.controls.primitives.scrollbar) (прокрутка встроена в контейнерные элементы управления)|Использование Скроллвиев|
|[Рисунк](/dotnet/api/system.windows.forms.imagelist)|-|-|
|[Label](/dotnet/api/system.windows.forms.label)|[Label](/dotnet/api/system.windows.controls.label)|Метка|
|[LinkLabel](/dotnet/api/system.windows.forms.linklabel)|Нет эквивалентного элемента управления (можно использовать класс [Hyperlink](/dotnet/api/system.windows.documents.hyperlink) для размещения гиперссылок в содержимом нефиксированного формата).|-|
|[ListBox](/dotnet/api/system.windows.forms.listbox)|[ListBox](/dotnet/api/system.windows.controls.listbox)|Использовать ListView|
|[ListView](/dotnet/api/system.windows.forms.listview)|[ListView](/dotnet/api/system.windows.controls.listview)|ListView|
|[MaskedTextBox](/dotnet/api/system.windows.forms.maskedtextbox)|-|-|
|[MenuStrip](/dotnet/api/system.windows.forms.menustrip)|[Меню](/dotnet/api/system.windows.controls.menu)|Рассмотрим Мастердетаилпаже или Таббедпаже|
|[MonthCalendar](/dotnet/api/system.windows.forms.monthcalendar)|[Календарь](/dotnet/api/system.windows.controls.calendar)|-|
|[NotifyIcon](/dotnet/api/system.windows.forms.notifyicon)|-|-|
|[NumericUpDown](/dotnet/api/system.windows.forms.numericupdown)|[Текстовое поле](/dotnet/api/system.windows.controls.textbox) и два элемента управления [RepeatButton](/dotnet/api/system.windows.controls.primitives.repeatbutton) .|Шаговый переключатель|
|[OpenFileDialog](/dotnet/api/system.windows.forms.openfiledialog)|[OpenFileDialog](/dotnet/api/microsoft.win32.openfiledialog)|-|
|[PageSetupDialog](/dotnet/api/system.windows.forms.pagesetupdialog)|-|-|
|[Panel](/dotnet/api/system.windows.forms.panel)|[Элемент Canvas](/dotnet/api/system.windows.controls.canvas)|Просмотреть или Абсолутелайаут|
|[PictureBox](/dotnet/api/system.windows.forms.picturebox)|[Изображение](/dotnet/api/system.windows.controls.image)|Образ —|
|[PrintDialog](/dotnet/api/system.windows.forms.printdialog)|[PrintDialog](/dotnet/api/system.windows.controls.printdialog)|-|
|[PrintDocument](/dotnet/api/system.drawing.printing.printdocument)|-|-|
|[PrintPreviewControl](/dotnet/api/system.windows.forms.printpreviewcontrol)|[DocumentViewer](/dotnet/api/system.windows.controls.documentviewer)|-|
|[PrintPreviewDialog](/dotnet/api/system.windows.forms.printpreviewdialog)|-|-|
|[ProgressBar](/dotnet/api/system.windows.forms.progressbar)|[ProgressBar](/dotnet/api/system.windows.controls.progressbar)|ProgressBar|
|[PropertyGrid](/dotnet/api/system.windows.forms.propertygrid)|-|-|
|[RadioButton](/dotnet/api/system.windows.forms.radiobutton)|[RadioButton](/dotnet/api/system.windows.controls.radiobutton)|-|
|[RichTextBox](/dotnet/api/system.windows.forms.richtextbox)|[RichTextBox](/dotnet/api/system.windows.controls.richtextbox)|Редактор не поддерживает форматированный (форматированный) текст, запись для однострочного текста|
|[SaveFileDialog](/dotnet/api/system.windows.forms.savefiledialog)|[SaveFileDialog](/dotnet/api/microsoft.win32.savefiledialog)|-|
|[ScrollableControl](/dotnet/api/system.windows.forms.scrollablecontrol)|[ScrollViewer](/dotnet/api/system.windows.controls.scrollviewer)|ScrollView|
|[SoundPlayer](/dotnet/api/system.media.soundplayer)|[MediaPlayer](/dotnet/api/system.windows.media.mediaplayer)|-|
|[SplitContainer](/dotnet/api/system.windows.forms.splitcontainer)|[GridSplitter](/dotnet/api/system.windows.controls.gridsplitter)|Рассмотрите возможность Мастердетаилпаже|
|[StatusStrip](/dotnet/api/system.windows.forms.statusstrip)|[StatusBar](/dotnet/api/system.windows.controls.primitives.statusbar)|-|
|[TabControl](/dotnet/api/system.windows.forms.tabcontrol)|[TabControl](/dotnet/api/system.windows.controls.tabcontrol)|TabbedPage|
|[TableLayoutPanel](/dotnet/api/system.windows.forms.tablelayoutpanel)|[Grid](/dotnet/api/system.windows.controls.grid)|Макет Grid|
|[TextBox](/dotnet/api/system.windows.forms.textbox)|[TextBox](/dotnet/api/system.windows.controls.textbox)|Редактор не поддерживает форматированный текст (форматированный)|
|[Таймер](/dotnet/api/system.windows.forms.timer)|[DispatcherTimer](/dotnet/api/system.windows.threading.dispatchertimer)|Device. StartTime ()|
|[ToolStrip](/dotnet/api/system.windows.forms.toolstrip)|[Панелей](/dotnet/api/system.windows.controls.toolbar)|Page. Тулбаритемс и Тулбаритем|
|[ToolStripContainer](/dotnet/api/system.windows.forms.toolstripcontainer), [ToolStripDropDown](/dotnet/api/system.windows.forms.toolstripdropdown), [ToolStripDropDownMenu](/dotnet/api/system.windows.forms.toolstripdropdownmenu), [ToolStripPanel](/dotnet/api/system.windows.forms.toolstrippanel)|[Панель инструментов](/dotnet/api/system.windows.controls.toolbar) с композицией.|Page. Тулбаритемс и Тулбаритем с композицией|
|[ToolTip](/dotnet/api/system.windows.forms.tooltip)|[ToolTip](/dotnet/api/system.windows.controls.tooltip)|Использование специальных возможностей|
|[TrackBar](/dotnet/api/system.windows.forms.trackbar)|[Ползунок](/dotnet/api/system.windows.controls.slider)|Ползунок|
|[TreeView](/dotnet/api/system.windows.forms.treeview)|[TreeView](/dotnet/api/system.windows.controls.treeview)|Рассмотрите возможность иерархического ListView в Навигатионпаже|
|[UserControl](/dotnet/api/system.windows.forms.usercontrol)|[UserControl](/dotnet/api/system.windows.controls.usercontrol)|Просмотр и пользовательские модули подготовки отчетов|
|[VScrollBar](/dotnet/api/system.windows.forms.vscrollbar)|[Элемента](/dotnet/api/system.windows.controls.primitives.scrollbar)|Использование Скроллвиев|
|[WebBrowser](/dotnet/api/system.windows.forms.webbrowser)|[WebBrowser](/dotnet/api/system.windows.controls.webbrowser)|WebView|