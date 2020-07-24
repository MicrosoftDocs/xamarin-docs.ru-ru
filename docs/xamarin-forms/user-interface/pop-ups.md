---
title: Отображать всплывающие окна
description: 'Xamarin.Formsв предусмотрено три всплывающих элемента пользовательского интерфейса: предупреждение, лист действий и запрос. В этой статье демонстрируется использование API предупреждений, листов действий и интерфейсов командной строки для вывода диалоговых окон, предлагающих пользователям простые вопросы, помогающие пользователям выполнять задачи и выводить запросы.'
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 75cc3070f552ef05c3e8702d27caf7c353ac0a8f
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86931876"
---
# <a name="display-pop-ups"></a>Отображать всплывающие окна

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-pop-ups)

Отображение предупреждения, предоставление пользователю возможности выбора или отображение запроса — это обычная задача пользовательского интерфейса. Xamarin.Formsимеет три метода в [`Page`](xref:Xamarin.Forms.Page) классе для взаимодействия с пользователем через всплывающее окно: [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) , [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) и `DisplayPromptAsync` . Эти элементы визуализируются на каждой платформе с помощью соответствующих собственных элементов управления.

## <a name="display-an-alert"></a>Отображение предупреждения

Все Xamarin.Forms Поддерживаемые платформы имеют модальное всплывающее окно для оповещения пользователя или запроса простых вопросов. Чтобы отобразить эти предупреждения в Xamarin.Forms , используйте [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) метод для любого из них [`Page`](xref:Xamarin.Forms.Page) . Следующая строка отображает простое сообщение:

```csharp
await DisplayAlert ("Alert", "You have been alerted", "OK");
```

![Диалоговое окно предупреждения с одной кнопкой](pop-ups-images/alert.png)

В этом примере не предполагается получение сведений от пользователя. Предупреждение отображается в модальном режиме, и после его закрытия пользователь продолжает работать с приложением.

[`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*)Метод также можно использовать для записи ответа пользователя путем представления двух кнопок и возврата `boolean` . Для получения ответа на предупреждение предоставьте надписи для обеих кнопок и примените к методу оператор `await`. После того как пользователь выберет один из вариантов, ответ возвращается в код. Обратите внимание на ключевые слова `async` и `await` в примере кода ниже.

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  bool answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![дисплайалерт](pop-ups-images/alert2-sml.png "Диалоговое окно предупреждения с двумя кнопками")](pop-ups-images/alert2.png#lightbox "Диалоговое окно предупреждения с двумя кнопками")

## <a name="guide-users-through-tasks"></a>Рекомендации для пользователей по задачам

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) — это стандартный элемент пользовательского интерфейса в iOS. Xamarin.Forms [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) Метод позволяет включать этот элемент управления в межплатформенные приложения, выменяя собственные альтернативы в Android и UWP.

Для вывода листа действий `await` [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) в любом случае передайте [`Page`](xref:Xamarin.Forms.Page) сообщение и метки кнопок в виде строк. Этот метод возвращает надпись кнопки, нажатой пользователем. Вот простой пример.

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  string action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![Диалоговое окно с листом действий](pop-ups-images/action.png)

Кнопка `destroy` отрисовывается не так, как другие. Ее можно оставить со значением `null` или указать в качестве третьего строкового параметра. В следующем примере используется кнопка `destroy`.

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  string action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![дисплайактионшит](pop-ups-images/action2-sml.png "Диалоговое окно листа действий с кнопкой "уничтожить"")](pop-ups-images/action2.png#lightbox "Диалоговое окно листа действий с кнопкой "уничтожить"")

## <a name="display-a-prompt"></a>Отображение запроса

Чтобы отобразить запрос, вызовите метод `DisplayPromptAsync` в Any [`Page`](xref:Xamarin.Forms.Page) , передав заголовок и сообщение в качестве `string` аргументов:

```csharp
string result = await DisplayPromptAsync("Question 1", "What's your name?");
```

Запрос отображается в модальном виде:

[![Снимок экрана: модальная строка в iOS и Android](pop-ups-images/simple-prompt.png "Модальная строка")](pop-ups-images/simple-prompt-large.png#lightbox "Модальная строка")

Если нажата кнопка ОК, то возвращаемый ответ возвращается в виде `string` . Если нажата кнопка Отмена, `null` возвращается значение.

Полный список аргументов для `DisplayPromptAsync` метода:

- `title`Тип `string` — заголовок, отображаемый в запросе.
- `message`Тип — `string` сообщение, отображаемое в командной строке.
- `accept`, типа `string` — это текст кнопки Accept. Это необязательный аргумент, значение по умолчанию которого — ОК.
- `cancel`, типа `string` — это текст для кнопки Отмена. Это необязательный аргумент, значение по умолчанию которого — Cancel.
- `placeholder`Тип — `string` текст заполнителя, отображаемый в запросе. Это необязательный аргумент, значение по умолчанию которого — `null` .
- `maxLength`, типа `int` — Максимальная длина ответа пользователя. Это необязательный аргумент, значение по умолчанию которого равно-1.
- `keyboard`Тип `Keyboard` — это тип клавиатуры, используемый для ответа пользователя. Это необязательный аргумент, значение по умолчанию которого — `Keyboard.Default` .
- `initialValue`тип — это `string` предварительно определенный ответ, который будет отображаться и который можно изменить. Это необязательный аргумент, значение по умолчанию которого — Empty `string` .

В следующем примере показано задание некоторых необязательных аргументов:

```csharp
string result = await DisplayPromptAsync("Question 2", "What's 5 + 5?", initialValue: "10", maxLength: 2, keyboard: Keyboard.Numeric);
```

Этот код отображает предопределенный ответ 10, ограничивает число символов, которое может быть введено равным 2, и отображает цифровую клавиатуру для ввода данных пользователем:

[![Снимок экрана: модальная строка в iOS и Android](pop-ups-images/keyboard-prompt.png "Модальная строка")](pop-ups-images/keyboard-prompt-large.png#lightbox "Модальная строка")

## <a name="related-links"></a>Связанные ссылки

- [PopupsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-pop-ups)
