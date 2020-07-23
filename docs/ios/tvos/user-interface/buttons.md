---
title: Работа с кнопками tvOS в Xamarin
description: В этом документе описывается работа с кнопками в приложении tvOS, созданном с помощью Xamarin. В нем рассматривается работа с кнопками в коде и в раскадровках, а также рассматривается применение стиля для кнопки.
ms.prod: xamarin
ms.assetid: DA6EF400-A4E3-4245-A0D4-F2398CAE2C9B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/07/2017
ms.openlocfilehash: 0de41379800d3f8fcb26e2bc58497010b8e3bf95
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86931382"
---
# <a name="working-with-tvos-buttons-in-xamarin"></a>Работа с кнопками tvOS в Xamarin

Используйте экземпляр `UIButton` класса для создания фокуса, выбираемой в окне tvOS кнопки. Когда пользователь нажимает кнопку, он отправляет сообщение о действии в целевой объект, позволяя приложению Xamarin. tvOS реагировать на входные данные пользователя.

[![Примеры кнопок](buttons-images/buttons01.png)](buttons-images/buttons01.png#lightbox)

Дополнительные сведения о работе с фокусом и навигации по Siri Remote см. в документации по [работе с навигацией](~/ios/tvos/app-fundamentals/navigation-focus.md) и [Siri удаленными устройствами и контроллерами Bluetooth](~/ios/tvos/platform/remote-bluetooth.md) .

<a name="About-Buttons"></a>

## <a name="about-buttons"></a>О кнопках

В tvOS кнопки используются для действий конкретного приложения и могут содержать заголовок, значок или и то, и другое. По мере того, как пользователь переходит по пользовательскому интерфейсу приложения с помощью [Siri Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), фокус перемещается на заданную кнопку, что делает изменение цвета текста и фона. Тень также применяется к кнопке, которая позволяет добавить трехмерный эффект, чтобы он отображался над остальной частью пользовательского интерфейса.

[![Примеры кнопок](buttons-images/buttons01.png)](buttons-images/buttons01.png#lightbox)

У Apple есть следующие рекомендации по работе с кнопками:

- **Используйте заголовок или значок** . Хотя в кнопку можно добавить как значок, так и заголовок, пространство ограничено, поэтому старайтесь не объединять оба.
- **Ясно помечайте разрушающие кнопки** . Если кнопка выполняет разрушительное действие (например, удаление файла), четко помечайте его, используя текст и/или значок. Разрушительные действия всегда должны представлять [предупреждение](~/ios/tvos/user-interface/alerts.md) , предлагающее пользователю ограничить действие.
- **Не использовать кнопки "назад** " — кнопка меню на удаленном Siri используется для возврата к предыдущему экрану. Единственным исключением из этого правила являются покупки в приложении или разрушительные действия, в которых должна отображаться кнопка **Отмена** .

Дополнительные сведения о работе с фокусом и навигации см. в документации по [работе с навигацией и фокусом](~/ios/tvos/app-fundamentals/navigation-focus.md) .

<a name="Button-Icons"></a>

### <a name="button-icons"></a>Значки кнопок

Компания Apple предлагает использовать для значков кнопок простые, хорошо распознаваемые изображения. Чрезмерно сложные значки трудно распознать на экране телевизора в комнате на диване, поэтому попробуйте использовать простейшее представление для получения идеи. Везде, где это возможно, используйте стандартные, хорошо известные изображения для значков (например, лупа для поиска).

<a name="Button-Titles"></a>

### <a name="button-titles"></a>Заголовки кнопок

При создании названий для кнопок компания Apple предлагает следующие рекомендации:

- **Показывать описательный текст под кнопками со значками** . там, где это возможно, поместите флажок четкий текст с описанием ниже значка только кнопки, чтобы получить назначение кнопки.
- **Используйте глаголы или фразы глагола для заголовка** — ясно укажите действие, которое будет выполняться при нажатии пользователем кнопки.
- **Используйте прописные буквы в стиле заголовка** — за исключением статей, комбинаций или предварительных позиций (не более четырех букв) каждое слово названия кнопки должно быть прописным.
- **Используйте краткий заголовок, чтобы** описать действие кнопки, используйте кратчайшие возможности формулировках.

<a name="Buttons-and-Storyboards"></a>

## <a name="buttons-and-storyboards"></a>Кнопки и раскадровки

Самым простым способом работы с кнопками в приложении Xamarin. tvOS является добавление их в пользовательский интерфейс приложения с помощью Xamarin Designer для iOS.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. В **Обозреватель решений**дважды щелкните `Main.storyboard` файл и откройте его для редактирования.
1. Перетащите **кнопку** из **библиотеки** и поместите ее в представление: 

    [![Кнопка A](buttons-images/storyboard01.png)](buttons-images/storyboard01.png#lightbox)
1. В **обозревателе свойств**можно настроить несколько свойств кнопки, например ее **заголовок** и **Цвет текста**: 

    [![Свойства кнопки](buttons-images/storyboard02.png)](buttons-images/storyboard02.png#lightbox)
1. Затем перейдите на **вкладку События** и назовите **событие** с помощью **кнопки** и позвоните в него `ButtonPressed` : 

    [![Вкладка "события"](buttons-images/storyboard03.png)](buttons-images/storyboard03.png#lightbox)
1. Вы будете автоматически переключаться на `ViewController.cs` представление, где можно разместить новое действие в коде с помощью клавиш со стрелками **вверх** и **вниз** : 

    [![Размещение нового действия в коде](buttons-images/storyboard04.png)](buttons-images/storyboard04.png#lightbox)
1. Нажмите клавишу **Ввод** , чтобы выбрать расположение: 

    [![Редактор кода](buttons-images/storyboard05.png)](buttons-images/storyboard05.png#lightbox)
1. Сохраните изменения во всех файлах.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. В **Обозреватель решений**дважды щелкните `Main.storyboard` файл и откройте его для редактирования.
1. Перетащите **кнопку** из **библиотеки** и поместите ее в представление: 

    [![Кнопка A](buttons-images/storyboard01vs.png)](buttons-images/storyboard01vs.png#lightbox)
1. В **обозревателе свойств**можно настроить несколько свойств кнопки, например ее **заголовок** и **Цвет текста**: 

    [![Обозреватель свойств](buttons-images/storyboard02vs.png)](buttons-images/storyboard02vs.png#lightbox)
1. Затем перейдите на **вкладку События** и назовите **событие** с помощью **кнопки** и позвоните в него `ButtonPressed` : 

    [![Вкладка "события"](buttons-images/storyboard03vs.png)](buttons-images/storyboard03vs.png#lightbox)
1. Сохраните изменения во всех файлах.

Измените файл контроллера представления (example `ViewController.cs` ) и добавьте следующий код, чтобы обрабатывал выбранную кнопку:

```

using System;
using UIKit;

namespace tvRemote
{
    public partial class ViewController : UIViewController
    {
        ...

        partial void ButtonPressed (UIButton sender) {
            // Handle click here
            ...
        }
    }
}

```

-----

При условии, что `Enabled` свойство кнопки не `true` охватывается другим элементом управления или представлением, можно сделать элемент с фокусом с помощью Siri Remote. Если пользователь нажимает кнопку и щелкает сенсорную поверхность, `ButtonPressed` будет выполнено определенное выше действие.

> [!IMPORTANT]
> Хотя `TouchUpInside` `UIButton` при создании **обработчика событий**можно назначать такие действия, как в конструкторе iOS, он никогда не будет вызываться, так как Apple TV не имеет сенсорного экрана или поддерживает события касания. При создании **действий** для элементов пользовательского интерфейса tvOS всегда следует использовать **тип действия** по умолчанию.

Дополнительные сведения о работе с раскадровками см. в статье [Hello, tvOS краткое руководство по началу работы](~/ios/tvos/get-started/hello-tvos.md).

<a name="Buttons-and-Code"></a>

## <a name="buttons-and-code"></a>Кнопки и код

При необходимости `UIButton` можно создать в коде C# и добавить в представление приложения tvOS. Пример:

```csharp
var button = new UIButton(UIButtonType.System);
button.Frame = new CGRect (25, 25, 300, 150);
button.SetTitle ("Hello", UIControlState.Normal);
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
View.AddSubview (button);
```

При создании нового `UIButton` в коде вы указываете его `UIButtonType` как одно из следующих:

- **System** — это стандартный тип кнопки, представленный tvOS, а — тип, который будет использоваться чаще всего.
- **Детаилдисклосуре** — представляет тип кнопки "раскрутка", используемый для скрытия или отображения подробных сведений.
- **Инфодарк** — кнопка темной подробной информации, отображающая "i" в окружности.
- **Инфолигхт** — кнопка с подробными сведениями, отображающая "i" в окружности.
- **Аддконтакт** — отображение кнопки в виде кнопки Добавить контакт.
- **Custom** — позволяет настроить несколько характеристик кнопки.

Далее вы определите размер и расположение кнопки на экране. Пример.

```csharp
button.Frame = new CGRect (25, 25, 300, 150);
```

Затем задайте заголовок для кнопки. `UIButtons`отличаются от большинства `UIKit` элементов управления тем, что они имеют состояние, поэтому нельзя просто изменить название, его необходимо изменить для определенного `UIControlState` . Пример:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

Затем используйте событие, `AllEvents` чтобы увидеть, когда пользователь нащелкнул кнопку. Пример.

```csharp
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
```

Наконец, добавьте кнопку в представление, чтобы отобразить ее:

```csharp
View.AddSubview (button);
```

> [!IMPORTANT]
> Хотя можно назначать такие действия `TouchUpInside` `UIButton` , как, он никогда не будет вызываться, так как Apple TV не имеет сенсорного экрана или не поддерживает сенсорные события. Следует всегда использовать такие события, как **AllEvents** или **примаряктионтригжеред**.

<a name="Styling-a-Button"></a>

## <a name="styling-a-button"></a>Стилизация кнопки

tvOS предоставляет несколько свойств `UIButton` , которые можно использовать для предоставления его заголовка и стиля с такими объектами, как цвет фона и изображения.

<a name="Button-Titles"></a>

### <a name="button-titles"></a>Заголовки кнопок

Как было показано выше, они `UIButtons` отличаются от большинства `UIKit` элементов управления тем, что они имеют состояние, поэтому нельзя просто изменить название, его необходимо изменить для определенного `UIControlState` . Пример:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

Цвет заголовка для кнопки можно задать с помощью `SetTitleColor` метода. Пример:

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
```

Можно настроить тень заголовка с помощью `SetTitleShadowColor` . Пример:

```csharp
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Можно задать для тени заголовка значение от *утопленного* к *рельефному* , когда кнопка будет выделена с помощью следующего кода:

```csharp
button.ReverseTitleShadowWhenHighlighted = true;
```

Кроме того, в качестве заголовка кнопки можно использовать текст с атрибутами. Пример:

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

### <a name="button-images"></a>Изображения кнопок

`UIButton`Может иметь присоединенный к нему образ и может использовать изображение в качестве фона.

Чтобы задать фоновое изображение кнопки для заданного объекта `UIControlState` , используйте следующий код:

```csharp
button.SetBackgroundImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Задайте для `AdjustsImageWhenHiglighted` свойства значение `true` , чтобы изображение было светлее выделять нажатием кнопки (это значение по умолчанию). Задайте для `AdjustsImageWhenDisabled` свойства значение `true` , чтобы изображение было темнее, когда кнопка отключена (опять же, это значение по умолчанию).

Чтобы задать изображение, отображаемое на кнопке, используйте следующий код:

```csharp
button.SetImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Используйте `TintColor` свойство для задания цветового оттенка, который применяется как к названию, так и к изображению кнопки. Для кнопок `Custom` типа это свойство не оказывает никакого влияния, необходимо реализовать `TintColor` поведение самостоятельно.

<a name="Summary"></a>

## <a name="summary"></a>Итоги

В этой статье рассматривается проектирование и работа с кнопками внутри приложения Xamarin. tvOS. В нем было показано, как работать с кнопками в конструкторе iOS и как создавать кнопки в коде C#. Наконец, было показано, как изменить заголовок кнопки и изменить его стиль и внешний вид.

## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [Руководства по tvOSму интерфейсу](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Руководством по программированию приложений для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
