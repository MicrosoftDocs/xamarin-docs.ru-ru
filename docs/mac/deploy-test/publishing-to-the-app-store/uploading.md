---
title: Отправка в Mac App Store
description: Этот документ описывает использование iTunes Connect для отправки приложения Xamarin.Mac в Mac App Store. Он содержит сведения, необходимые iTunes Connect для завершения этого процесса.
ms.prod: xamarin
ms.assetid: 30cd0e47-1b2e-47ef-93f6-4bed20b15c03
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 57591a6e9fcaa3c8271ab27756160ee1c46a4fa3
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86935932"
---
# <a name="upload-to-mac-app-store"></a>Отправка в Mac App Store

_В этом руководстве описано, как отправить приложение Xamarin.Mac для публикации в Mac App Store._

Приложения отправляются на утверждение команде Mac App Store через [iTunes Connect](https://itunesconnect.apple.com/). Кроме того, вам понадобится средство [**Transporter**](https://apps.apple.com/us/app/transporter/id1450874784?mt=12) из App Store.

1. Выберите **приложение macOS**, которое требуется создать:

    [![iTunes Connect](uploading-images/image65.png)](uploading-images/image65.png#lightbox)

2. Введите имя приложения и укажите другие сведения. Можно выбрать только значения существующего параметра **Идентификатор пакета**, который был создан ранее:

    [![Выбор идентификатора пакета](uploading-images/image66.png)](uploading-images/image66.png#lightbox)

3. Выберите дату доступности и цену. Независимо от выбранной даты приложение станет доступно для продажи только после его утверждения. Если вам требуется больший контроль над фактической датой доступности, это значение можно задать далеко в будущем.

    [![Задание даты доступности и цены](uploading-images/image67.png)](uploading-images/image67.png#lightbox)

4. Введите сведения о приложении, включая категорию Магазина приложений, в которую оно входит.

    [![Ввод данных о приложении](uploading-images/image68.png)](uploading-images/image68.png#lightbox)

    Выберите применимые оценки:

    [![Установка оценок приложения](uploading-images/image69.png)](uploading-images/image69.png#lightbox)

    Описание, ключевые слова и контактные URL-адреса:

    [![Редактирование описания, ключевых слов и контактных URL-адресов](uploading-images/image70.png)](uploading-images/image70.png#lightbox)

    Контактные данные и информация для рецензентов Магазина приложений:

    [![Редактирование контактных данных и информации для рецензентов App Store](uploading-images/image71.png)](uploading-images/image71.png#lightbox)

    И, наконец, снимки экранов:

    [![Добавление необходимых снимков экрана](uploading-images/image72.png)](uploading-images/image72.png#lightbox)

    Снимки экрана должны иметь формат JPG, TIF или PNG и размер 1280 x 800, 1440 x 900, 2880 x 1800 или 2560 x 1600 пикселей. Нажмите клавишу **Сохранить** для завершения.

5. Сведения о приложении доступны для просмотра. Нажмите кнопку **Показать сведения**, чтобы изменить состояние:

    [![Просмотр сведений о приложении](uploading-images/image73.png)](uploading-images/image73.png#lightbox)

6. В представлении сведений нажмите кнопку "Ready to Upload Binary" (Готово для отправки двоичного файла), чтобы отправить файл пакета приложения:

    [![Кнопка "Ready to Upload Binary" (Готово для отправки двоичного файла)](uploading-images/image74.png)](uploading-images/image74.png#lightbox)

7. Ответьте на вопрос шифрования:

    [![Ответ на вопрос шифрования](uploading-images/image75.png)](uploading-images/image75.png#lightbox)

8. Появится сообщение о том, когда сайт будет готов принять файл пакета приложения:

    [![Уведомление о принятии](uploading-images/image76.png)](uploading-images/image76.png#lightbox)

9. Запустите **Transporter** и войдите с помощью идентификатора Apple ID, а затем выберите **ADD APP** (Добавить приложение):

    [![Интерфейс загрузчика приложений](uploading-images/transporter01-sml.png)](uploading-images/transporter01.png#lightbox)

    Следуйте инструкциям, чтобы отправить пакет приложения в iTunes Connect.

    > [!NOTE]
    > [**Transporter**](https://apps.apple.com/us/app/transporter/id1450874784?mt=12) заменяет средство **Загрузчик приложений**, которое использовалось с Xcode 10 или более ранними версиями.
    > Загрузчик приложений больше недоступен в Xcode 11 или более поздней версии.

После утверждения приложение будет доступно для скачивания или приобретения в Mac App Store.

## <a name="related-links"></a>Связанные ссылки

- [Установка](~//mac/get-started/installation.md)
- [Пример кода приложения "Привет, Mac"](~/mac/get-started/hello-mac.md)
- [Распространение приложений в Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Руководство по работе с инструментами. Подписывание кода приложения](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Идентификатор разработчика и привратник](https://developer.apple.com/developer-id/)
