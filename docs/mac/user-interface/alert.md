---
title: Оповещения в Xamarin. Mac
description: В этой статье рассматривается работа с предупреждениями в приложении Xamarin. Mac. Здесь описывается создание и отображение предупреждений из кода C# и реагирование на взаимодействия пользователей.
ms.prod: xamarin
ms.assetid: F1DB93A1-7549-4540-AD5E-D7605CCD8435
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 00e5b2a2238763822172a1b7d7a7c3090634ed17
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938090"
---
# <a name="alerts-in-xamarinmac"></a>Оповещения в Xamarin. Mac

_В этой статье рассматривается работа с предупреждениями в приложении Xamarin. Mac. Здесь описывается создание и отображение предупреждений из кода C# и реагирование на взаимодействия пользователей._

При работе с C# и .NET в приложении Xamarin. Mac у вас есть доступ к тем же предупреждениям, что и разработчик, работающий на *уровне цели-C* и *Xcode* . 

Предупреждение — это особый тип диалогового окна, которое появляется при возникновении серьезной проблемы (например, ошибка) или в виде предупреждения (например, при подготовке к удалению файла). Поскольку оповещение является диалоговым окном, оно также требует ответа пользователя, прежде чем его можно будет закрыть.

[![Пример оповещения](alert-images/alert06.png)](alert-images/alert06.png#lightbox)

В этой статье рассматриваются основные принципы работы с предупреждениями в приложении Xamarin. Mac. 

<a name="Introduction_to_Alerts"></a>

## <a name="introduction-to-alerts"></a>Общие сведения о предупреждениях

Предупреждение — это особый тип диалогового окна, которое появляется при возникновении серьезной проблемы (например, ошибка) или в виде предупреждения (например, при подготовке к удалению файла). Поскольку предупреждения нарушают пользователя, так как они должны быть отклонены, прежде чем пользователь сможет продолжить работу с задачей, не следует отображать оповещение, если оно совершенно не требуется.

Apple рекомендует следующие рекомендации:

- Не используйте оповещение только для предоставления сведений о пользователях.
- Не отображать оповещение для распространенных, отменяемых действий. Даже если такая ситуация может привести к потере данных.
- Если ситуация достойной предупреждения, старайтесь не использовать какой-либо другой элемент пользовательского интерфейса или метод для его вывода.
- Значок предупреждения следует использовать осторожно.
- Опишите ситуацию с предупреждениями четко и кратко в сообщении с предупреждением.
- Имя кнопки по умолчанию должно соответствовать действию, описанному в сообщении с предупреждением.

Дополнительные сведения см. в разделе " [оповещения](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAlerts.html#//apple_ref/doc/uid/20000957-CH44-SW1) [" руководства по использованию интерфейса](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) пользователя в ОС Apple.

<a name="Anatomy_of_an_Alert"></a>

## <a name="anatomy-of-an-alert"></a>Анатомия оповещения

Как упоминалось выше, оповещения должны отображаться для пользователя приложения при возникновении серьезной проблемы или в виде предупреждения о возможной потере данных (например, при закрытии несохраненного файла). В Xamarin. Mac в коде C# создается предупреждение, например:

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Critical,
  InformativeText = "We need to save the document here...",
  MessageText = "Save Document",
};
alert.RunModal ();
```

Приведенный выше код отображает предупреждение с значком приложения, нажимаемым на значке предупреждения, заголовок, предупреждающее сообщение и одну кнопку **ОК** :

[![Оповещение с кнопкой "ОК"](alert-images/alert01.png)](alert-images/alert01.png#lightbox)

Apple предоставляет несколько свойств, которые можно использовать для настройки оповещений.

- **Алертстиле** определяет тип предупреждения как одно из следующих:
  - **Предупреждение** — используется для предупреждения пользователя о текущем или неожидающем событии, которое не является критическим. Этот стиль используется по умолчанию.
  - **Информация** — используется для предупреждения пользователя о текущем или ожидающем событии. В настоящее время нет видимых различий между **предупреждением** и **информационным**
  - **Критический** — используется для предупреждения пользователя о серьезных последствиях незавершенного события (например, при удалении файла). Этот тип предупреждения следует использовать с осторожностью.
- **MessageText** — это основное сообщение или заголовок оповещения, которое должно быстро определить ситуацию для пользователя.
- **Информативетекст** — это текст оповещения, в котором следует четко определить ситуацию и предоставить пользователю возможные варианты.
- **Значок** — позволяет пользователю отображать пользовательский значок.
- **Хелпанчор**  &  **Шовшелп** — позволяет привязывать предупреждение к хелпбуку приложения и отображать справку по этому оповещению.
- **Кнопки** . по умолчанию оповещение содержит только кнопку **ОК** , но коллекция **Buttons** позволяет добавлять дополнительные варианты по мере необходимости.
- **Шовссуппрессионбуттон** — если `true` отображается флажок, который пользователь может использовать для подавления предупреждения при последующих событиях события, которое его инициировало.
- **Акцессоривиев** — позволяет присоединить к предупреждению другое Подпредставление, чтобы предоставить дополнительные сведения, например добавить **текстовое поле** для ввода данных. Если задать новый **акцессоривиев** или изменить существующий, необходимо вызвать `Layout()` метод, чтобы настроить видимый макет предупреждения.

<a name="Displaying_an_Alert"></a>

## <a name="displaying-an-alert"></a>Отображение предупреждения

Существует два разных способа отображения предупреждения: свободно-плавающая или как лист. Приведенный ниже код отображает предупреждение как свободный с плавающей запятой.

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Informational,
  InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.RunModal ();
```

При выполнении этого кода отображается следующее:

[![Простое оповещение](alert-images/alert02.png)](alert-images/alert02.png#lightbox)

Следующий код отображает то же самое предупреждение, что и лист:

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Informational,
  InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.BeginSheet (this);
```

При выполнении этого кода будут отображены следующие:

[![Предупреждение, отображаемое в виде листа](alert-images/alert03.png)](alert-images/alert03.png#lightbox)

<a name="Working_with_Alert_Buttons"></a>

## <a name="working-with-alert-buttons"></a>Работа с кнопками предупреждений

По умолчанию в предупреждении отображается только кнопка **ОК** . Тем не менее, вы можете создать дополнительные кнопки, добавив их в коллекцию **Buttons** . В следующем коде создается оповещение с произвольным плавающей точкой с помощью кнопки **ОК**, **Отмена** и, **возможно** ,:

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Informational,
  InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
var result = alert.RunModal ();
```

Первая добавленная кнопка будет _кнопкой по умолчанию_ , которая будет активирована, если пользователь нажмет клавишу ВВОД. Возвращаемое значение будет целым числом, представляющим нажатую пользователем кнопку. В нашем случае будут возвращены следующие значения:

- **ОК** — 1000.
- **Отмена** — 1001.
- **Возможно** , 1002.

Если выполнить код, будут отображены следующие фрагменты кода:

[![Предупреждение с тремя параметрами кнопки](alert-images/alert04.png)](alert-images/alert04.png#lightbox)

Ниже приведен код для того же оповещения, что и лист:

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Informational,
  InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.BeginSheetForResponse (this, (result) => {
  Console.WriteLine ("Alert Result: {0}", result);
});
```

При выполнении этого кода будут отображены следующие:

[![Предупреждение с тремя кнопками, отображаемое в виде листа](alert-images/alert05.png)](alert-images/alert05.png#lightbox)

> [!IMPORTANT]
> В оповещение никогда не следует добавлять более трех кнопок.

<a name="Showing_the_Suppress_Button"></a>

## <a name="showing-the-suppress-button"></a>Отображение кнопки "подавлять"

Если `ShowSuppressButton` свойство предупреждения имеет значение `true` , то в предупреждении отображается флажок, который пользователь может использовать для подавления предупреждения о последующих событиях события, которое его инициировало. В следующем коде отображается оповещение с произвольным плавающей точкой с кнопкой "подавлять":

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Informational,
  InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

Если значение параметра `alert.SuppressionButton.State` равно, то `NSCellStateValue.On` пользователь установил флажок подавлять, а в противном случае — нет.

Если код выполняется, будет выведено следующее:

[![Предупреждение с кнопкой "подавлять"](alert-images/alert06.png)](alert-images/alert06.png#lightbox)

Ниже приведен код для того же оповещения, что и лист:

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Informational,
  InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.BeginSheetForResponse (this, (result) => {
  Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

При выполнении этого кода будут отображены следующие:

[![Предупреждение с кнопкой "подавлять" отображается как лист](alert-images/alert07.png)](alert-images/alert07.png#lightbox)

<a name="Adding_a_Custom_SubView"></a>

## <a name="adding-a-custom-subview"></a>Добавление пользовательского подпредставления

Оповещения имеют `AccessoryView` свойство, которое можно использовать для дальнейшей настройки оповещения и добавления таких элементов, как **текстовое поле** для ввода данных пользователем. Следующий код создает оповещение с произвольным плавающей точкой с добавленным текстовым полем ввода:

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
AlertStyle = NSAlertStyle.Informational,
InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

Ключевые строки здесь `var input = new NSTextField (new CGRect (0, 0, 300, 20));` создают новое **текстовое поле** , в которое мы добавим предупреждение. `alert.AccessoryView = input;`При этом **текстовое поле** подключается к предупреждению и вызывается `Layout()` метод, который требуется для изменения размера предупреждения в соответствии с новым подпредставлением.

Если выполнить код, будут отображены следующие фрагменты кода:

[![При выполнении кода будут отображены следующие](alert-images/alert08.png)](alert-images/alert08.png#lightbox)

Это то же самое предупреждение, что и лист:

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Informational,
  InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
alert.BeginSheetForResponse (this, (result) => {
  Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

Если выполнить этот код, отобразятся следующие фрагменты кода:

[![Оповещение с пользовательским представлением](alert-images/alert09.png)](alert-images/alert09.png#lightbox)

<a name="Summary"></a>

## <a name="summary"></a>Сводка

В этой статье подробно рассматривается работа с оповещениями в приложении Xamarin. Mac. Мы узнали о различных типах и использовании оповещений, о том, как создавать и настраивать оповещения, а также как работать с предупреждениями в коде C#.

## <a name="related-links"></a>Связанные ссылки

- [Маквиндовс (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/macwindows)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Работа с Windows](~/mac/user-interface/window.md)
- [Рекомендации по работе с человеческим интерфейсом OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Введение в Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [нсалерт](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSAlert_Class/index.html#//apple_ref/doc/uid/TP40004001)
