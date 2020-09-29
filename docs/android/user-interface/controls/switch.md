---
title: Параметр Xamarin. Android
description: Использование мини-приложения коммутатора в приложении Xamarin. Android
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/29/2018
ms.openlocfilehash: dabd4b5bd7d6dbd118314e94fe04bc4623cf11e1
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457722"
---
# <a name="xamarinandroid-switch"></a>Параметр Xamarin. Android

Мини-приложение `Switch` (показанное ниже) позволяет пользователю переключаться между двумя состояниями, например включать или ВЫключать. `Switch`Значение по умолчанию — OFF. Мини-приложение отображается в обоих состояниях:

[![Снимки экрана мини-приложения переключателя в состоянии OFF и ON](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)

## <a name="creating-a-switch"></a>Создание переключателя

Чтобы создать переключатель, просто объявите `Switch` элемент в XML следующим образом:

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

При этом создается базовый коммутатор, как показано ниже.

[![Снимок экрана демонстрационного приложения, отображающего параметр в состоянии OFF](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)

## <a name="changing-default-values"></a>Изменение значений по умолчанию

Текст, отображаемый элементом управления для состояний включения и ВЫКЛЮЧЕНия, и значение по умолчанию можно настроить. Например, чтобы параметру по умолчанию было присвоено значение ON, а для свойства Read/YES — значение OFF/ON, можно задать `checked` `textOn` атрибуты, и `textOff` в следующем коде XML.

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

## <a name="providing-a-title"></a>Указание заголовка

`Switch`Мини-приложение также поддерживает включение текстовой метки, настроив `text` атрибут следующим образом:

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

Эта разметка создает следующий снимок экрана во время выполнения:

[![Снимок экрана демонстрационного приложения с текстом по горизонтали перед мини-приложением-переключателем](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

При `Switch` изменении значения возникает `CheckedChange` событие.
Например, в следующем коде мы Сообразим это событие и представим `Toast` мини-приложение с сообщением на основе `isChecked` значения `Switch` , которое передается в обработчик событий как часть `CompoundButton.CheckedChangeEventArg` аргумента.

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```

## <a name="related-links"></a>Связанные ссылки

- [Свитчдемо (пример)](/samples/xamarin/monodroid-samples/switchdemo)
- [Учебник по макету вкладок](~/android/user-interface/layouts/tab-layout/index.md)