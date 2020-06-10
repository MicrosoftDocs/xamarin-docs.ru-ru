---
title: Работа с оповещениями tvOS в Xamarin
description: В этом документе описывается работа с оповещениями tvOS в Xamarin. Здесь рассматривается отображение предупреждения, Добавление текстовых полей и вспомогательного класса.
ms.prod: xamarin
ms.assetid: F969BB28-FF2C-4A7D-88CA-F8076AD48538
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: ed58694073f8d04d16cf19840a07f5210f0afb91
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574071"
---
# <a name="working-with-tvos-alerts-in-xamarin"></a>Работа с оповещениями tvOS в Xamarin

_В этой статье рассматривается работа с Уиалертконтроллер для вывода на экран сообщения о предупреждении пользователю в Xamarin. tvOS._

Если необходимо уделить внимание пользователю tvOS или запросить разрешение на выполнение разрушительного действия (например, удаление файла), вы можете предоставить предупреждение с помощью `UIAlertViewController` :

[![](alerts-images/alert01.png "An example UIAlertViewController")](alerts-images/alert01.png#lightbox)

Если отображается сообщение, можно добавить в оповещение кнопки и текстовые поля, чтобы пользователь мог отвечать на действия и отправлять отзывы.

<a name="About-Alerts"></a>

## <a name="about-alerts"></a>Об оповещениях

Как упоминалось выше, оповещения используются для получения внимания пользователя и уведомления о состоянии приложения или запроса на отзыв. Оповещения должны представлять заголовок, при необходимости может быть сообщение и одна или несколько кнопок или текстовых полей.

[![](alerts-images/alert04.png "An example alert")](alerts-images/alert04.png#lightbox)

В Apple имеются следующие рекомендации по работе с оповещениями.

- **Использование оповещений с осторожностью** . предупреждения мешают потоку пользователя в приложении и прерывают взаимодействие с пользователем, поэтому его следует использовать только для важных ситуаций, таких как уведомления об ошибках, покупки в приложении и обратимые действия.
- **Предоставляет полезные варианты** . Если в предупреждении представлены параметры для пользователя, необходимо убедиться, что каждый вариант предлагает важные сведения и предоставляет полезные действия, которые пользователь должен выполнить.

<a name="Alert-Titles-and-Messages"></a>

### <a name="alert-titles-and-messages"></a>Заголовки и сообщения оповещений

Компания Apple предлагает следующие рекомендации для представления названия и необязательного сообщения.

- **Использование многословных названий** . Заголовок предупреждения должен быть достаточно четким, пока все еще остается простым. Один заголовок слова редко предоставляет достаточно информации.
- **Используйте описательные названия, которые не требуют сообщения** — везде, где это возможно, рекомендуется сделать заголовок предупреждения достаточно описательным, что необязательный текст сообщения не требуется.
- **Сделать сообщение коротким, завершающим предложением** . Если для получения точки оповещения требуется необязательное сообщение, сделайте его как можно более простым и сделайте его полным предложением с правильными прописными буквами и знаками препинания.

<a name="Alert-Buttons"></a>

### <a name="alert-buttons"></a>Кнопки предупреждений

Компания Apple имеет следующие рекомендации по добавлению кнопок в оповещение:

- **Ограничить две кнопки** . везде, где это возможно, ограничьте оповещение максимум двумя кнопками. Оповещения одной кнопки предоставляют сведения, но не действия. Два оповещения для кнопок обеспечивают простой выбор действий "Да" и "нет".
- **Используйте сжатые, логические заголовки кнопок** — простое одно-два названия кнопок Word, которые четко описывают действие кнопки. Дополнительные сведения см. в документации по [работе с кнопками](~/ios/tvos/user-interface/buttons.md) .
- **Ясно помечайте разрушающие кнопки** . кнопки, выполняющие разрушительное действие (например, удаление файла), четко помечают их `UIAlertActionStyle.Destructive` стилем.

<a name="Displaying-an-Alert"></a>

## <a name="displaying-an-alert"></a>Отображение предупреждения

Чтобы отобразить предупреждение, необходимо создать экземпляр компонента `UIAlertViewController` и настроить его, добавив действия (кнопки) и выбрав стиль оповещения. Например, следующий код отображает предупреждение ОК/Отмена:

```csharp
const string title = "A Short Title is Best";
const string message = "A message should be a short, complete sentence.";
const string acceptButtonTitle = "OK";
const string cancelButtonTitle = "Cancel";
const string deleteButtonTitle = "Delete";
...

var alertController = UIAlertController.Create (title, message, UIAlertControllerStyle.Alert);

// Create the action.
var acceptAction = UIAlertAction.Create (acceptButtonTitle, UIAlertActionStyle.Default, _ =>
    Console.WriteLine ("The \"OK/Cancel\" alert's other action occurred.")
);

var cancelAction = UIAlertAction.Create (cancelButtonTitle, UIAlertActionStyle.Cancel, _ =>
    Console.WriteLine ("The \"OK/Cancel\" alert's other action occurred.")
);

// Add the actions.
alertController.AddAction (acceptAction);
alertController.AddAction (cancelAction);
PresentViewController (alertController, true, null);
```

Давайте подробно рассмотрим этот код. Сначала мы создадим новое оповещение с заданным названием и сообщением:

```csharp
UIAlertController.Create (title, message, UIAlertControllerStyle.Alert)
```

Затем для каждой кнопки, которую нужно отобразить в предупреждении, мы создаем действие, определяющее название кнопки, ее стиль и действие, которое нужно предпринять при нажатии кнопки:

```csharp
UIAlertAction.Create ("Button Title", UIAlertActionStyle.Default, _ =>
    // Do something when the button is pressed
    ...
);
```

`UIAlertActionStyle`Перечисление позволяет задать стиль кнопки как одно из следующих:

- **По умолчанию** — кнопка будет выбрана по умолчанию при отображении оповещения.
- **Отмена** — кнопка отменена для оповещения.
- **Разрушение** — эта кнопка выделяется как разрушительное действие, например удаление файла. В настоящее время tvOS отображает кнопку уничтожения с красным фоном.

`AddAction`Метод добавляет заданное действие в и, `UIAlertViewController` наконец, `PresentViewController (alertController, true, null)` метод отображает данное предупреждение пользователю.

<a name="Adding-Text-Fields"></a>

## <a name="adding-text-fields"></a>Добавление текстовых полей

Помимо добавления действий (кнопок) к предупреждению, в оповещение можно добавлять текстовые поля, позволяющие пользователю заполнять такие данные, как идентификаторы пользователей и пароли:

[![](alerts-images/alert02.png "Text Field in an alert")](alerts-images/alert02.png#lightbox)

Если пользователь выберет текстовое поле, будет отображена Стандартная клавиатура tvOS, позволяющая вводить значение для поля:

[![](alerts-images/alert03.png "Entering text")](alerts-images/alert03.png#lightbox)

В следующем коде отображается предупреждение ОК/Отмена с одним текстовым полем для ввода значения:

```csharp
UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);
UITextField field = null;

// Add and configure text field
alert.AddTextField ((textField) => {
    // Save the field
    field = textField;

    // Initialize field
    field.Placeholder = placeholder;
    field.Text = text;
    field.AutocorrectionType = UITextAutocorrectionType.No;
    field.KeyboardType = UIKeyboardType.Default;
    field.ReturnKeyType = UIReturnKeyType.Done;
    field.ClearButtonMode = UITextFieldViewMode.WhileEditing;

});

// Add cancel button
alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
    // User canceled, do something
    ...
}));

// Add ok button
alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
    // User selected ok, do something
    ...
}));

// Display the alert
controller.PresentViewController(alert,true,null);
```

`AddTextField`Метод добавляет в предупреждение новое текстовое поле, которое затем можно настроить, задав такие свойства, как текст заполнителя (текст, отображаемый при пустом поле), текстовое значение по умолчанию и тип клавиатуры. Пример.

```csharp
// Initialize field
field.Placeholder = placeholder;
field.Text = text;
field.AutocorrectionType = UITextAutocorrectionType.No;
field.KeyboardType = UIKeyboardType.Default;
field.ReturnKeyType = UIReturnKeyType.Done;
field.ClearButtonMode = UITextFieldViewMode.WhileEditing;
```

Чтобы мы могли работать со значением текстового поля позже, мы также сохраняем копию с помощью следующего кода:

```csharp
UITextField field = null;
...

// Add and configure text field
alert.AddTextField ((textField) => {
    // Save the field
    field = textField;
    ...
});
```

После того, как пользователь вводит значение в текстовое поле, можно использовать `field` переменную для доступа к этому значению.

<a name="Alert-View-Controller-Helper-Class"></a>

## <a name="alert-view-controller-helper-class"></a>Вспомогательный класс контроллера представления предупреждений

Поскольку отображение простых и распространенных типов оповещений с помощью `UIAlertViewController` может привести к созданию повторяющегося кода, можно использовать вспомогательный класс, чтобы уменьшить количество повторяющихся кодов. Пример.

```csharp
using System;
using Foundation;
using UIKit;
using System.CodeDom.Compiler;

namespace UIKit
{
    /// <summary>
    /// Alert view controller is a reusable helper class that makes working with <c>UIAlertViewController</c> alerts
    /// easier in a tvOS app.
    /// </summary>
    public class AlertViewController
    {
        #region Static Methods
        public static UIAlertController PresentOKAlert(string title, string description, UIViewController controller) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Configure the alert
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(action) => {}));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentOKCancelAlert(string title, string description, UIViewController controller, AlertOKCancelDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false);
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
                // Any action?
                if (action!=null) {
                    action(true);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentDestructiveAlert(string title, string description, string destructiveAction, UIViewController controller, AlertOKCancelDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false);
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create(destructiveAction,UIAlertActionStyle.Destructive,(actionOK) => {
                // Any action?
                if (action!=null) {
                    action(true);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentTextInputAlert(string title, string description, string placeholder, string text, UIViewController controller, AlertTextInputDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);
            UITextField field = null;

            // Add and configure text field
            alert.AddTextField ((textField) => {
                // Save the field
                field = textField;

                // Initialize field
                field.Placeholder = placeholder;
                field.Text = text;
                field.AutocorrectionType = UITextAutocorrectionType.No;
                field.KeyboardType = UIKeyboardType.Default;
                field.ReturnKeyType = UIReturnKeyType.Done;
                field.ClearButtonMode = UITextFieldViewMode.WhileEditing;

            });

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false,"");
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
                // Any action?
                if (action!=null && field !=null) {
                    action(true, field.Text);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }
        #endregion

        #region Delegates
        public delegate void AlertOKCancelDelegate(bool OK);
        public delegate void AlertTextInputDelegate(bool OK, string text);
        #endregion
    }
}
```

С помощью этого класса можно выполнять отображение простых оповещений и реагировать на них следующим образом.

```csharp
#region Custom Actions
partial void DisplayDestructiveAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentDestructiveAlert("A Short Title is Best","The message should be a short, complete sentence.","Delete",this, (ok) => {
        Console.WriteLine("Destructive Alert: The user selected {0}",ok);
    });
}

partial void DisplayOkCancelAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentOKCancelAlert("A Short Title is Best","The message should be a short, complete sentence.",this, (ok) => {
        Console.WriteLine("OK/Cancel Alert: The user selected {0}",ok);
    });
}

partial void DisplaySimpleAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentOKAlert("A Short Title is Best","The message should be a short, complete sentence.",this);
}

partial void DisplayTextInputAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentTextInputAlert("A Short Title is Best","The message should be a short, complete sentence.","placeholder", "", this, (ok, text) => {
        Console.WriteLine("Text Input Alert: The user selected {0} and entered `{1}`",ok,text);
    });
}
#endregion
```

<a name="Summary"></a>

## <a name="summary"></a>Сводка

Эта статья посвящена работе с `UIAlertController` для вывода на экран сообщения о предупреждении пользователю в Xamarin. tvOS. Во-первых, было показано, как отобразить простое оповещение и добавить кнопки. Далее показано, как добавить текстовые поля в предупреждение. Наконец, было показано, как использовать вспомогательный класс, чтобы уменьшить количество повторяющихся кода, необходимого для отображения предупреждения.

## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [Руководства по tvOSму интерфейсу](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Руководством по программированию приложений для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
