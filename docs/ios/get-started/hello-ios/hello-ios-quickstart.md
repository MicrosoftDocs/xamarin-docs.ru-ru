---
title: Краткое руководство по приложению "Привет, iOS"
description: Это пошаговое руководство показывает, как создать простое приложение Xamarin.iOS под названием "Привет, iOS". При этом оно знакомит читателя с ключевыми средствами и концепциями, в которых нужно разобраться для создания приложений Xamarin.iOS.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: D3868F3A-4EED-BDDF-45AA-665102C39634
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
ms.openlocfilehash: bde05cfeecaeba389224bbbdb7ae01fcb7bcb56c
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91434108"
---
# <a name="hello-ios--quickstart"></a>Краткое руководство по приложению "Привет, iOS"

Это руководство описывает создание приложения, которое преобразует введенный пользователем буквенно-цифровой телефонный номер в числовой телефонный номер и затем набирает его. Окончательный вариант приложения выглядит примерно так:

 [![Приложение краткого руководства по Hello.iOS](hello-ios-quickstart-images/image1.png)](hello-ios-quickstart-images/image1.png#lightbox)

## <a name="requirements"></a>Требования

Для разработки на iOS с помощью Xamarin требуется следующее:

- Компьютер Mac под управлением macOS High Sierra 10.13 или последующей версии.
- Последняя версия Xcode и пакета SDK iOS из [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12).

::: zone pivot="macos"

Xamarin.iOS работает со следующими конфигурациями:

- Последняя версия Visual Studio для Mac, удовлетворяющая указанным выше требованиям.

Пошаговые инструкции по установке см. в [руководстве по установке Xamarin.iOS на Mac](~/ios/get-started/installation/mac.md).

::: zone-end
::: zone pivot="windows"

Xamarin.iOS работает со следующими конфигурациями:

- Последняя версия Visual Studio 2019 или Visual Studio 2017 Community, Professional или Enterprise на базе Windows 10 или более поздней версии, связанная с узлом сборки Mac, удовлетворяющим указанным выше требованиям.

Пошаговые инструкции по установке см. в [руководстве по установке Xamarin.iOS в Windows](~/ios/get-started/installation/windows/index.md).

::: zone-end

Перед началом работы скачайте [набор значков приложения Xamarin](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true).

::: zone pivot="macos"

## <a name="visual-studio-for-mac-walkthrough"></a>Пошаговое руководство по Visual Studio для Mac

Это пошаговое руководство описывает создание приложения Phoneword, которое преобразует буквенно-цифровой телефонный номер в числовой.

1. Запустите Visual Studio для Mac из папки **Applications** (Приложения) или из **Spotlight**:

    ![Экран запуска](hello-ios-quickstart-images/image2new.png)

    На экране запуска нажмите кнопку **Новый проект...** , чтобы создать новое решение Xamarin.iOS:

    ![Решение iOS](hello-ios-quickstart-images/image3new.png)

2. В диалоговом окне **Новое решение** выберите шаблон **iOS > Приложение > Приложение одного представления**, убедившись, что выбран C#. Нажмите кнопку **Далее**:

    ![Выбор приложения одного представления](hello-ios-quickstart-images/image4new.png)

3. Настройте приложение. Присвойте ему **имя** `Phoneword_iOS`, а для остальных параметров сохраните значения по умолчанию. Нажмите кнопку **Далее**:

    ![Ввод имени приложения](hello-ios-quickstart-images/image5new.png)

4. Оставьте имеющееся имя для проекта и решения. Выберите расположение проекта или оставьте значение по умолчанию:

    ![Выбор расположения проекта](hello-ios-quickstart-images/image6new.png)

5. Нажмите кнопку **Создать**, чтобы создать **Решение**.

6. Откройте файл **Main.storyboard**, дважды щелкнув его на **Панели решения**. Это позволяет создавать пользовательский интерфейс визуально:

    ![Конструктор iOS](hello-ios-quickstart-images/image7new.png)

    Обратите внимание на то, что _классы размера_ включены по умолчанию. Чтобы узнать о них подробнее, см. руководство по [унифицированным раскадровкам](~/ios/user-interface/storyboards/unified-storyboards.md).

7. Откройте **панель элементов**, введите "метка" в поле поиска и перетащите элемент **Метка** в область конструктора (в центре):

    ![Перетаскивание элемента "Метка" в область конструктора в центре](hello-ios-quickstart-images/image8new.png)

    > [!NOTE]
    > Вы можете открыть **Панель свойств** или **Панель элементов** в любое время, перейдя в раздел **Вид > Панели**.

8. Используя маркеры *перетаскивания элементов управления* (кружки вокруг элемента управления), сделайте метку шире:

    ![Увеличение ширины метки](hello-ios-quickstart-images/image9.png)

9. Выбрав элемент **Метка** в области конструктора, используйте **Панель свойств**, чтобы изменить свойство **Текст** элемента **Метка** на "Enter a Phoneword" (Введите слово-номер):

    ![Установка метки для ввода слова-номера](hello-ios-quickstart-images/image10.png)

10. Выполните поиск строки "текстовое поле" на панели элементов и перетащите элемент **Текстовое поле** с **панели элементов** в область конструктора, поместив его под элементом **Метка**. Измените ширину, чтобы элемент **Текстовое поле** был такой же ширины, что и **Метка**:

    ![Выравнивание ширины текстового поля и метки](hello-ios-quickstart-images/image12new.png)

11. Выбрав элемент **Текстовое поле** в области конструктора, измените для **текстового поля** свойство **Имя** в разделе "Удостоверение" на **панели свойств**, указав новое значение `PhoneNumberText`, а для свойства **Текст** введите "1-855-XAMARIN":

    ![Изменение значения свойства "Заголовок" на 1-855-XAMARIN](hello-ios-quickstart-images/image13new.png)

12. Перетащите элемент **Кнопка** из **панели элементов** в область конструктора и поместите его под элементом **Текстовое поле**. Настройте ширину элемента **Кнопка**, чтобы выровнять его с элементами **Текстовое поле** и **Метка**:

    ![Настройка ширины элемента "Кнопка", чтобы выровнять его с элементами "Текстовое поле" и "Метка"](hello-ios-quickstart-images/image14new.png)

13. Выбрав элемент **Кнопка** в области конструктора, измените свойство **Имя** в разделе **Удостоверение** **Панели свойств** на `TranslateButton`. Измените значение свойства **Заголовок** на "Translate" (Преобразовать):

    ![Изменение значения свойства "Заголовок" на Translate (Преобразовать)](hello-ios-quickstart-images/image15new.png)

14. Повторите два предыдущих действия, перетащите элемент **Кнопка** из **панели элементов** в область конструктора и поместите его под первым элементом **Кнопка**. Настройте ширину элемента **Кнопка**, чтобы выровнять его с первым элементом **Кнопка**:

    ![Настройка ширины элемента "Кнопка", чтобы выровнять его с первым элементом "Кнопка"](hello-ios-quickstart-images/image16new.png)

15. Выбрав второй элемент **Кнопка** в области конструктора, измените свойство **Имя** в разделе **Удостоверение** **Панели свойств** на `CallButton`. Измените значение свойства **Заголовок** на "Call" (Вызов):

    ![Изменение значения свойства "Заголовок" на Call (Вызов)](hello-ios-quickstart-images/image17new.png)

    Сохраните изменения, выбрав **Файл > Сохранить** или нажав клавиши **⌘+S**.

16. В приложение нужно добавить логику для преобразования телефонных номеров из буквенно-цифровых в цифровые. Добавьте новый файл в проект, щелкнув правой кнопкой мыши проект **Phoneword_iOS** на **Панели решения** и выбрав **Добавить > Новый файл...** или нажав клавиши **⌘+N**:

    ![Добавление нового файла в проект](hello-ios-quickstart-images/image18.png)

17. В диалоговом окне **Новый файл** выберите **Общие > Пустой класс** и назовите новый файл `PhoneTranslator`:

    ![Выбор параметра "Пустой класс" и присвоение имени PhoneTranslator новому файлу](hello-ios-quickstart-images/image19.png)

18. Создается пустой класс C#. Удалите код шаблона и замените его следующим:

    ```csharp
    using System.Text;
    using System;

    namespace Phoneword_iOS
    {
        public static class PhoneTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw)) {
                    return "";
                } else {
                    raw = raw.ToUpperInvariant();
                }

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c)) {
                        newNumber.Append(c);
                    } else {
                        var result = TranslateToNumber(c);
                        if (result != null) {
                            newNumber.Append(result);
                        }
                    }
                    // otherwise we've skipped a non-numeric char
                }
                return newNumber.ToString();
            }

            static bool Contains (this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static int? TranslateToNumber(char c)
            {
                if ("ABC".Contains(c)) {
                    return 2;
                } else if ("DEF".Contains(c)) {
                    return 3;
                } else if ("GHI".Contains(c)) {
                    return 4;
                } else if ("JKL".Contains(c)) {
                    return 5;
                } else if ("MNO".Contains(c)) {
                    return 6;
                } else if ("PQRS".Contains(c)) {
                    return 7;
                } else if ("TUV".Contains(c)) {
                    return 8;
                } else if ("WXYZ".Contains(c)) {
                    return 9;
                }
                return null;
            }
        }
    }
    ```

    Сохраните файл **PhoneTranslator.cs** и закройте его.

19. Добавьте код для подключения пользовательского интерфейса. Для этого дважды щелкните файл **ViewController.cs** на **Панели решения**, чтобы открыть его:

    ![Добавление кода для подключения пользовательского интерфейса](hello-ios-quickstart-images/image20new.png)

20. Начните с подключения `TranslateButton`. Найдите метод `ViewDidLoad` в классе **ViewController** и добавьте следующий код под вызовом `base.ViewDidLoad()`:

    ```csharp
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
        // Convert the phone number with text to a number
        // using PhoneTranslator.cs
        translatedNumber = PhoneTranslator.ToNumber(
            PhoneNumberText.Text);

        // Dismiss the keyboard if text field was tapped
        PhoneNumberText.ResignFirstResponder ();

        if (translatedNumber == "") {
            CallButton.SetTitle ("Call ", UIControlState.Normal);
            CallButton.Enabled = false;
        } else {
            CallButton.SetTitle ("Call " + translatedNumber,
                UIControlState.Normal);
            CallButton.Enabled = true;
        }
    };
    ```

    Включите `using Phoneword_iOS;`, если пространство имен файла отличается.

21. Добавьте код, реагирующий на нажатие пользователем второй кнопки, которая называется `CallButton`. Поместите следующий код после кода для `TranslateButton` и добавьте `using Foundation;` в начало файла:

    ```csharp
        CallButton.TouchUpInside += (object sender, EventArgs e) => {
            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl ("tel:" + translatedNumber);

            // ...otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl (url)) {
                var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                PresentViewController (alert, true, null);
            }
        };
    ```

22. Сохраните изменения и выполните сборку приложения, выбрав **Сборка > Собрать все** или нажав клавиши **⌘+B**.  Если приложение компилируется, в верхней части интегрированной среды разработки отображается сообщение об успешном выполнении:

    ![Отображение сообщения об успешном выполнении в верхней части интегрированной среды разработки](hello-ios-quickstart-images/image21.png)

    При наличии ошибок просмотрите предыдущие шаги и исправьте все ошибки, пока сборка приложения не будет проходить успешно.

23. Наконец, протестируйте приложение в **симуляторе iOS**. В верхнем левом углу интегрированной среды разработки выберите значение **Отладка** в первом раскрывающемся списке и **iPhone XR iOS 12.0** или другой доступный симулятор во втором, а затем нажмите кнопку **Запустить** (треугольная кнопка, которая напоминает кнопку воспроизведения):

    ![Выберите симулятор и нажмите "Запустить"](hello-ios-quickstart-images/image27.png)

    > [!NOTE]
    > Сейчас, в связи с требованиями Apple, при создании кода для устройства или симулятора может потребоваться сертификат разработки или *удостоверение подписывания*. Чтобы настроить их, следуйте указаниям в руководстве по [подготовке устройств](~/ios/get-started/installation/device-provisioning/manual-provisioning.md).

24. В результате приложение запускается в симуляторе iOS:

    ![Выполнение приложения в симуляторе iOS](hello-ios-quickstart-images/image28.png)

    Симулятор iOS не поддерживает телефонные звонки; при попытке выполнить вызов отображается диалоговое окно предупреждения:

    ![Диалоговое окно предупреждения при попытке выполнения вызова](hello-ios-quickstart-images/image29.png)

::: zone-end
::: zone pivot="windows"

## <a name="visual-studio-walkthrough"></a>Пошаговое руководство по Visual Studio

Это пошаговое руководство описывает создание приложения Phoneword, которое преобразует буквенно-цифровой телефонный номер в числовой.

> [!NOTE]
> В этом пошаговом руководстве используется Visual Studio Enterprise 2017 на виртуальной машине Windows 10. Ваша конфигурация может отличаться при условии соблюдения приведенных выше требований, но имейте в виду, что некоторые снимки экрана могут отличаться от вашей конфигурации.

> [!NOTE]
> Прежде чем продолжить работу с пошаговым руководством, вам нужно подключиться к компьютеру Mac из Visual Studio. Это вызвано тем, что Xamarin.iOS использует инструменты Apple при сборке и запуске конструктора и приложений iOS. Настройка описана в инструкциях из руководства по [связыванию с компьютером Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Запустите Visual Studio из меню **Пуск**:

    ![Начальный экран](hello-ios-quickstart-images/image001-.png)

    Создайте решение Xamarin.iOS, выбрав **Файл > Создать > Проект... > Visual C# > 	iPhone и iPad > Приложение iOS (Xamarin)** :

    ![Выберите тип проекта приложения iOS (Xamarin)](hello-ios-quickstart-images/image002.w157.png "Выберите тип проекта приложения iOS (Xamarin)")

    В следующем диалоговом окне выберите шаблон **Приложение с одним представлением** и нажмите **OK**, чтобы создать проект:

    ![Выберите шаблон проекта с одним представлением](hello-ios-quickstart-images/image002-2.w157.png "Выберите шаблон проекта с одним представлением")

1. Убедитесь, что значок агента Mac Xamarin на панели инструментов отображается зеленым цветом.

    ![Убедитесь, что значок агента Mac Xamarin на панели инструментов отображается зеленым цветом](hello-ios-quickstart-images/vs-image4.png)

    Если это не так, то есть отсутствует подключение к компьютеру сборки Mac, выполните инструкции из [руководства по выбору конфигурации](~/ios/get-started/installation/windows/connecting-to-mac/index.md), чтобы настроить подключение.

1. Откройте файл **Main.storyboard** в конструкторе iOS, дважды щелкнув его в **обозревателе решений**:

    ![Конструктор iOS](hello-ios-quickstart-images/vs-image7.png)

1. Откройте вкладку **Панель элементов**, введите "метка" в поле поиска и перетащите элемент **Метка** в область конструктора (в центре):

    ![Перетаскивание элемента "Метка" в область конструктора в центре](hello-ios-quickstart-images/vs-image8.png)

1. Захватите *маркеры перетаскивания* и растяните метку в стороны:

    ![Увеличение ширины метки](hello-ios-quickstart-images/vs-image9.png)

1. Выбрав элемент **Метка** в области конструктора, используйте **окна свойств**, чтобы изменить свойство **Текст** элемента **Метка** на "Enter a Phoneword" (Введите слово-номер):

    ![Изменение свойства "Текст" метки на "Enter a Phoneword" (Введите слово-номер)](hello-ios-quickstart-images/vs-image10.png)

    > [!NOTE]
    > Вы можете открыть **Свойства** или **Панель элементов** в любое время. Для этого перейдите в меню **Вид**.

1. Выполните поиск строки "текстовое поле" на панели элементов и перетащите элемент **Текстовое поле** с **панели элементов** в область конструктора, поместив его под элементом **Метка**. Измените ширину, чтобы элемент **Текстовое поле** был такой же ширины, что и **Метка**:

    ![Настройка ширины элемента "Текстовое поле", чтобы выровнять его с элементом "Метка"](hello-ios-quickstart-images/vs-image12.png)

1. Выбрав элемент **Текстовое поле** в области конструктора, измените его**свойство** **Имя** в разделе "Удостоверение" **свойств** на `PhoneNumberText`, а также измените свойство **Текст** на "1-855-XAMARIN":

    ![Изменение значения свойства "Текст" на 1-855-XAMARIN](hello-ios-quickstart-images/vs-image13.png)

1. Перетащите элемент **Кнопка** из **панели элементов** в область конструктора и поместите его под элементом **Текстовое поле**. Настройте ширину элемента **Кнопка**, чтобы выровнять его с элементами **Текстовое поле** и **Метка**:

    ![Настройка ширины элемента "Кнопка", чтобы выровнять его с элементами "Текстовое поле" и "Метка"](hello-ios-quickstart-images/vs-image14.png)

1. Выбрав элемент **Кнопка** в области конструктора, измените свойство **Имя** в разделе **Удостоверение** **свойств** на `TranslateButton`. Измените значение свойства **Заголовок** на "Translate" (Преобразовать):

    ![Изменение значения свойства "Заголовок" на Translate (Преобразовать)](hello-ios-quickstart-images/vs-image15.png)

1. Повторите два предыдущих действия, перетащите элемент **Кнопка** из **панели элементов** в область конструктора и поместите его под первым элементом **Кнопка**. Настройте ширину элемента **Кнопка**, чтобы выровнять его с первым элементом **Кнопка**:

    ![Настройка ширины элемента "Кнопка", чтобы выровнять его с первым элементом "Кнопка"](hello-ios-quickstart-images/vs-image16.png)

1. Выбрав второй элемент **Кнопка** в области конструктора, измените свойство **Имя** в разделе **Удостоверение** **свойств** на `CallButton`. Измените значение свойства **Заголовок** на "Call" (Вызов):

    ![Изменение значения свойства "Заголовок" на Call (Вызов)](hello-ios-quickstart-images/vs-image17.png)

    Сохраните изменения, выбрав **Файл > Сохранить все** или нажав клавиши **CTRL+S**.

1. Добавьте код, который преобразует телефонные номера из буквенно-цифровой записи в цифровую. Для этого сначала добавьте новый файл в проект, щелкнув правой кнопкой мыши проект **Phoneword** в **обозревателе решений** и выбрав **Добавить > Новый элемент...** или нажав клавиши **CTRL+SHIFT+A**:

    ![Добавьте код, который преобразует телефонные номера из буквенно-цифровой записи в цифровую](hello-ios-quickstart-images/vs-image18.png)

1. В диалоговом окне **Добавить новый элемент** (щелкните правой кнопкой мыши проект, выберите "Добавить > Новый элемент..."), выберите **Apple > Класс** и назовите новый файл `PhoneTranslator`:

    ![Добавление нового класса с именем PhoneTranslator](hello-ios-quickstart-images/vs-image19.w157.png)

    > [!IMPORTANT]
    > Убедитесь, что выбран шаблон класса, в значке которого указан C#. В противном случае ссылка на этот класс может быть невозможна.

1. Создается класс C#. Удалите код шаблона и замените его следующим:

    ```csharp
    using System.Text;
    using System;

    namespace Phoneword
    {
        public static class PhoneTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw)) {
                    return "";
                } else {
                    raw = raw.ToUpperInvariant();
                }

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c)) {
                        newNumber.Append(c);
                    } else {
                        var result = TranslateToNumber(c);
                        if (result != null) {
                            newNumber.Append(result);
                        }
                    }
                    // otherwise we've skipped a non-numeric char
                }
                return newNumber.ToString();
            }

            static bool Contains (this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static int? TranslateToNumber(char c)
            {
                if ("ABC".Contains(c)) {
                    return 2;
                } else if ("DEF".Contains(c)) {
                    return 3;
                } else if ("GHI".Contains(c)) {
                    return 4;
                } else if ("JKL".Contains(c)) {
                    return 5;
                } else if ("MNO".Contains(c)) {
                    return 6;
                } else if ("PQRS".Contains(c)) {
                    return 7;
                } else if ("TUV".Contains(c)) {
                    return 8;
                } else if ("WXYZ".Contains(c)) {
                    return 9;
                }
                return null;
            }
        }
    }
    ```

    Сохраните файл **PhoneTranslator.cs** и закройте его.

1. Дважды щелкните файл **ViewController.cs** в **обозревателе решений**, чтобы открыть его и добавить логику, обрабатывающую взаимодействие с кнопками:

    ![Добавление логики для обработки взаимодействия с кнопками](hello-ios-quickstart-images/vs-image20.png)

1. Начните с подключения `TranslateButton`. В классе **ViewController** найдите метод `ViewDidLoad`. Добавьте следующий код кнопки внутрь `ViewDidLoad`, под вызовом `base.ViewDidLoad()`:

    ```csharp
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {

        // Convert the phone number with text to a number
        // using PhoneTranslator.cs
        translatedNumber = PhoneTranslator.ToNumber(PhoneNumberText.Text);

        // Dismiss the keyboard if text field was tapped
        PhoneNumberText.ResignFirstResponder ();

        if (translatedNumber == "") {
            CallButton.SetTitle ("Call", UIControlState.Normal);
            CallButton.Enabled = false;
            }
        else {
            CallButton.SetTitle ("Call " + translatedNumber, UIControlState.Normal);
            CallButton.Enabled = true;
            }
    };
    ```

    Включите `using Phoneword;`, если пространство имен файла отличается.

1. Добавьте код, реагирующий на нажатие пользователем второй кнопки, которая называется `CallButton`. Поместите следующий код после кода для `TranslateButton` и добавьте `using Foundation;` в начало файла:

    ```csharp
    CallButton.TouchUpInside += (object sender, EventArgs e) => {
        var url = new NSUrl ("tel:" + translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app,
            // otherwise show an alert dialog

        if (!UIApplication.SharedApplication.OpenUrl (url)) {
                        var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                        alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                        PresentViewController (alert, true, null);
                    }
    };
    ```

1. Сохраните изменения и выполните сборку приложения, выбрав **Сборка > Построить решение** или нажав клавиши **CTRL+SHIFT+B**.  Если приложение компилируется, в нижней части интегрированной среды разработки отображается сообщение об успешном выполнении:

    ![Отображение сообщения об успешном выполнении в нижней части интегрированной среды разработки](hello-ios-quickstart-images/vs-image21.png)

    При наличии ошибок просмотрите предыдущие шаги и исправьте все ошибки, пока сборка приложения не будет проходить успешно.

1. Наконец, протестируйте приложение в **удаленном симуляторе iOS**. На панели инструментов интегрированной среды разработки выберите **Отладка** и **iPhone 8 Plus iOS x.x** в раскрывающихся меню и щелкните значок **Запустить** (зеленый треугольник, похожий на кнопку воспроизведения):

    ![Нажатие кнопки "Запустить"](hello-ios-quickstart-images/vs-image27.png)

1. В результате приложение запускается в симуляторе iOS:

    ![Выполнение приложения в симуляторе iOS](hello-ios-quickstart-images/vs-image28.png)

    Симулятор iOS не поддерживает телефонные звонки; при попытке выполнить вызов отображается диалоговое окно предупреждения:

    ![Диалоговое окно предупреждения, отображаемое при попытке выполнения вызова](hello-ios-quickstart-images/vs-image29.png)

::: zone-end

Поздравляем! Вы создали свое первое приложение Xamarin.iOS!

Пришло время закрепить и углубить приобретенные знания и навыки с помощью раздела [Привет, iOS: теперь подробнее](~/ios/get-started/hello-ios/hello-ios-deepdive.md).

## <a name="related-links"></a>Связанные ссылки

- [Значки приложения Xamarin и изображения при запуске (пример)](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true)
- [Привет, iOS (пример)](/samples/xamarin/ios-samples/hello-ios)
- [Рекомендации по работе с человеческим интерфейсом iOS](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [Портал подготовки iOS](https://developer.apple.com/ios/manage/overview/index.action)