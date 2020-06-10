---
Title: "почему конструктор XAML Visual Studio не работает для Xamarin.Forms файлов XAML?"
MS. Topic: Устранение неполадок MS. произв. Xamarin MS. AssetID: cab2eefb-c52f-4d81-866e-8f1feabbdd64 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч MS. Дата: 04/25/2017 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-files"></a>Почему конструктор XAML Visual Studio не работает с Xamarin.Forms файлами XAML?

Xamarin.FormsСейчас не поддерживает визуальные конструкторы для файлов XAML. Поэтому при попытке открыть файл XAML форм в *конструкторе пользовательского интерфейса XAML* Visual Studio или *конструкторе пользовательского интерфейса XAML с кодировкой*выдается следующее сообщение об ошибке:

> "Не удается открыть файл в выбранном редакторе. Выберите другой редактор ".

Это ограничение описано в разделе Руководство по [ Xamarin.Forms основам XAML](~/xamarin-forms/xaml/xaml-basics/index.md) :

> «Пока нет визуального конструктора для создания XAML в Xamarin.Forms приложениях, поэтому все XAML должны быть написаны вручную».

Однако Xamarin.Forms Предварительный просмотр XAML можно отобразить, выбрав пункт **Просмотр > другие окна Windows > Xamarin.Forms предварительного просмотра** .
