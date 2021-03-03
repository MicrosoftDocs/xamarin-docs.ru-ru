---
title: Отображать всплывающие окна
description: 'Xamarin.Forms в предусмотрено три всплывающих элемента пользовательского интерфейса: предупреждение, лист действий и запрос. В этой статье демонстрируется использование API предупреждений, листов действий и интерфейсов командной строки для вывода диалоговых окон, предлагающих пользователям простые вопросы, помогающие пользователям выполнять задачи и выводить запросы.'
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/10/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 05ad6af1b094a980f5e226bcde22b43764576652
ms.sourcegitcommit: 322e7bcf9fb8c1ad52ab8e929bea95d45e280834
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101751521"
---
# <a name="display-pop-ups"></a>Отображать всплывающие окна

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/navigation-pop-ups)

Отображение предупреждения, предоставление пользователю возможности выбора или отображение запроса — это обычная задача пользовательского интерфейса. Xamarin.Forms имеет три метода в [`Page`](xref:Xamarin.Forms.Page) классе для взаимодействия с пользователем через всплывающее окно: [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) , [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) и `DisplayPromptAsync` . Эти элементы визуализируются на каждой платформе с помощью соответствующих собственных элементов управления.

## <a name="display-an-alert"></a>Отображение предупреждения

Все Xamarin.Forms Поддерживаемые платформы имеют модальное всплывающее окно для оповещения пользователя или запроса простых вопросов. Чтобы отобразить эти предупреждения в Xamarin.Forms , используйте [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) метод для любого из них [`Page`](xref:Xamarin.Forms.Page) . Следующая строка отображает простое сообщение:

```csharp
await DisplayAlert ("Alert", "You have been alerted", "OK");
```

[![Диалоговое окно предупреждения с одной кнопкой на iOS и Android](pop-ups-images/simple-alert.png)](pop-ups-images/simple-alert-large.png#lightbox)

В этом примере не предполагается получение сведений от пользователя. Предупреждение отображается в модальном режиме, и после его закрытия пользователь продолжает работать с приложением.

[`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*)Метод также можно использовать для записи ответа пользователя путем представления двух кнопок и возврата `boolean` . Для получения ответа на предупреждение предоставьте надписи для обеих кнопок и примените к методу оператор `await`. После того как пользователь выберет один из вариантов, ответ возвращается в код. Обратите внимание на ключевые слова `async` и `await` в примере кода ниже.

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  bool answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![Диалоговое окно предупреждения с двумя кнопками](pop-ups-images/two-button-alert.png)](pop-ups-images/two-button-alert.png#lightbox)

[`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*)Метод также имеет перегрузки, принимающие [`FlowDirection`](xref:Xamarin.Forms.FlowDirection) аргумент, указывающий направление, в котором элементы пользовательского интерфейса поступают в пределах предупреждения. Дополнительные сведения о направлении потока см. [в разделе локализация справа налево](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

> [!WARNING]
> По умолчанию в UWP при отображении оповещения какие-либо ключи доступа, определенные на странице за предупреждением, могут быть активированы. Дополнительные сведения см. [в разделе ключи доступа висуалелемент в Windows](~/xamarin-forms/platform/windows/visualelement-access-keys.md).

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

[![Диалоговое окно Актионшит в iOS и Android](pop-ups-images/simple-actionsheet.png)](pop-ups-images/simple-actionsheet-large.png#lightbox)

Кнопка отображается по `destroy` -разному для других кнопок в iOS и может быть оставлена `null` или указана в качестве третьего строкового параметра. В следующем примере используется кнопка `destroy`.

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  string action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![Диалоговое окно Актионшит с кнопкой "уничтожить" в iOS и Android](pop-ups-images/actionsheet-destroy-button.png)](pop-ups-images/actionsheet-destroy-button-large.png#lightbox)

[`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)Метод также имеет перегрузку, которая принимает [`FlowDirection`](xref:Xamarin.Forms.FlowDirection) аргумент, указывающий направление потока элементов пользовательского интерфейса на листе действий. Дополнительные сведения о направлении потока см. [в разделе локализация справа налево](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

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

[![Снимок экрана необязательной модальной строки в iOS и Android](pop-ups-images/keyboard-prompt.png "Модальная строка")](pop-ups-images/keyboard-prompt-large.png#lightbox "Модальная строка")

> [!WARNING]
> По умолчанию в UWP при отображении запроса все ключи доступа, определенные на странице, расположенной за запросом, могут быть активированы. Дополнительные сведения см. [в разделе ключи доступа висуалелемент в Windows](~/xamarin-forms/platform/windows/visualelement-access-keys.md).

## <a name="related-links"></a>Связанные ссылки

- [PopupsSample](/samples/xamarin/xamarin-forms-samples/navigation-pop-ups)
- [Локализация справа налево](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
