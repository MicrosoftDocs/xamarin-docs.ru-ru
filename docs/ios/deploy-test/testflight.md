---
title: Использование TestFlight для распространения приложений Xamarin.iOS
description: Теперь сервис TestFlight принадлежит компании Apple и является основным способом бета-тестирования приложений Xamarin.iOS. В этой статье приводятся все этапы процесса TestFlight — от отправки приложения до работы с iTunes Connect.
ms.prod: xamarin
ms.assetid: BA880768-2BC8-41E4-B57E-A56F8EED4690
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 86b887b7aa913572d5fb6093fe682cd2933b282e
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573187"
---
# <a name="using-testflight-to-distribute-xamarinios-apps"></a>Использование TestFlight для распространения приложений Xamarin.iOS

_Теперь сервис TestFlight принадлежит компании Apple и является основным способом бета-тестирования приложений Xamarin.iOS. В этой статье приводятся все этапы процесса TestFlight — от отправки приложения до работы с iTunes Connect._

Бета-тестирование является неотъемлемой частью цикла разработки программного обеспечения, и существует множество кроссплатформенных предложений для упрощения этого процесса, таких как [HockeyApp](https://hockeyapp.net/features/), [Applause](https://www.applause.com/mobile-app-testing) и, конечно, встроенный инструмент бета-тестирования Google Play для приложений Android. Этот документ посвящен TestFlight компании Apple.

TestFlight — служба бета-тестирования Apple для приложений iOS, она доступна только через [iTunes Connect](https://itunesconnect.apple.com/). В настоящее время служба доступна для приложений iOS 8.0 и более поздних версий. TestFlight позволяет участвовать в бета-тестировании как внутренним, так и внешним пользователям, а проверка бета-версии приложения, необходимая для внешнего тестирования, значительно упрощает процесс финальной проверки при публикации в магазине.

Ранее двоичный файл создавался в среде Visual Studio для Mac и отправлялся на веб-сайт TestFlightApp для распространения среди тестировщиков. Новая процедура принесла с собой ряд улучшений, позволяющих публиковать в магазине приложений качественные и хорошо протестированные приложения. Пример:

- Проверка бета-версии приложения, необходимая для внешнего тестирования, значительно повышает вероятность успеха финальной проверки в App Store, поскольку в соответствии с правилами Apple обе проверки необходимы.
- Перед отправкой приложения его необходимо зарегистрировать в iTunes Connect. Это гарантирует отсутствие несоответствий профилей подготовки, имен и сертификатов.
- TestFlight теперь является настоящим приложением iOS, поэтому оно стало работать быстрее.
- После завершения бета-тестирования передать приложение на проверку просто и быстро, достаточно нажатия на одну кнопку.

## <a name="requirements"></a>Требования

Через TestFlight можно протестировать только приложения для iOS 8.0 и более поздних версий.

Все тестировщики должны тестировать приложение как минимум на устройстве с iOS 8. Тем не менее рекомендуется проводить тестирование во всех версиях iOS

## <a name="provisioning"></a>Подготовка

Чтобы протестировать сборку в TestFlight, необходимо создать *Профиль распространения App Store* с новым назначением для бета-тестирования. Это назначение разрешает бета-тестирование в TestFlight, все **новые** профили распространения App Store автоматически содержат это назначение. Для создания нового профиля выполните пошаговую инструкцию в руководстве [Создание профиля распространения](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile).

Можно указать, что профиль распространения содержит назначение для бета-тестирования, в процессе [проверки сборки в Xcode](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md), как показано ниже:

[![](testflight-images/validate-build.png "Submitting the App to Apple")](testflight-images/validate-build.png#lightbox)

## <a name="testflight-workflow"></a>Работа с TestFlight

Следующая инструкция описывает шаги, необходимые для начала использования TestFlight для бета-тестирования приложения:

1. Для новых приложений создайте [запись iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md).
2. [Заархивируйте и опубликуйте](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) приложения в iTunes Connect.
3. Подготовьте бета-тестирование:
    - добавьте метаданные.
    - Добавьте внутренних пользователей:
      - максимум 25 пользователей.
    - Добавьте внешних пользователей:
      - максимум 1000 пользователей.
      - Требуется проверка бета-версии в соответствии с рекомендациями Apple.
4. Получить отзывы от пользователей, проанализировать их и вернуться к шагу 2.

## <a name="create-an-itunes-connect-record"></a>Создание записи iTunes Connect

1. Войдите на [Портал iTunes Connect](https://itunesconnect.apple.com/), используя учетную запись разработчика Apple.
2. Выберите **Мои приложения**:

    [![](testflight-images/my-apps.png "Select My Apps")](testflight-images/my-apps.png#lightbox)

3. Чтобы добавить новое приложение, в окне **Мои приложения** щелкните на кнопку **+** в левом верхнем углу экрана. Если у вас есть учетные записи разработчика iOS и Mac, будет предложено выбрать тип нового приложения.

Откроется окно отправки данных **Новое приложение iOS**, которое должно содержать в точности те же сведения, что и файл Info.plist вашего приложения

Дополнительные сведения о создании новой записи iTunes Connect см. в руководстве [Создание записи iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md).

### <a name="completing-the-new-ios-app-submission-form"></a>Заполнение формы отправки для нового приложения iOS

Форма должна в точности отражать данные в файле Info.plist, как показано ниже:

[![](testflight-images/infoplist.png "The app's Info.plist")](testflight-images/infoplist.png#lightbox)
[![](testflight-images/newiosapp.png "The form on iTunes Connect")](testflight-images/newiosapp.png#lightbox)

- **Имя** — описательное имя, используемое при настройке пакета приложений. Параметр должен точно совпадать с записью **Application name** в `Info.plist`.
- **Основной язык** — базовый язык, используемый в приложении. Значение этого поля обычно не зависит от языка, на котором говорит разработчик.
- **ИД пакета** — в раскрывающемся меню отображается список идентификаторов всех приложений, созданных в вашей учетной записи разработчика.
  - **Суффикс ИД пакета** — при указании идентификатора пакета с подстановочным знаком (например, заканчивающегося знаком *, как показано в приведенном выше примере) отобразится дополнительное поле для ввода суффикса идентификатора пакета. В нашем примере **ИД пакетом** является `mobi.chkn.*`, суффиксом является **PageView**. Вместе они составляют параметр **Bundle Identifier** в файле `Info.plist`.
- **Версия** — номер версии отправляемого приложения. Значение параметра выбирается разработчиком.
- **SKU** — SKU представляет собой уникальный идентификатор приложения, который не будет доступен пользователями. Этот параметр можно считать аналогом идентификатора продукта. В приведенном выше примере в этом параметре мы совместили дату и номер версии.

## <a name="upload-your-app"></a>Отправка приложения

После создания записи iTunes Connect можно отправить новые сборки. Помните, что сборки должны иметь новое назначение для бета-тестирования.

Сначала нужно создать [финальную сборку](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) в среде разработки, затем [отправить приложение в Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) через приложение Application Loader или функцию архивирования в Xcode.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

### <a name="create-an-archive"></a>Создание архива

 Чтобы выполнить сборку двоичного файла в Visual Studio для Mac, необходимо использовать функцию _Архивирование_. Щелкните правой кнопкой мыши на проекте и выберите **Архивирование для публикации**, как показано ниже:

 [![](testflight-images/new-archive.png "Select Archive for Publishing")](testflight-images/new-archive.png#lightbox)

 Дополнительные сведения см. в руководстве [Сборка для распространения](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md).

### <a name="sign-and-distribute-your-app"></a>Подписывание и распространение приложения

 После создания архива будет автоматически открыто окно **Архивы**, в котором отображаются все заархивированные проекты, сгруппированные по решениям. Чтобы подписать приложение и подготовить его к распространению, выберите **Подписать и распространить...** , как показано ниже:

[![](testflight-images/archive-view.png "Creating an archive will automatically open the Archives View")](testflight-images/archive-view.png#lightbox)

 Откроется мастер публикации. Выберите **App Store** в качестве канала распространения, чтобы создать пакет, и откройте приложение Application Loader. На экране профиля подготовки выберите удостоверение подписывания и профиль подготовки или подпишите повторно другим удостоверением. Проверьте сведения о пакете и затем нажмите кнопку **Опубликовать** для сохранения файла `.ipa`

[![](testflight-images/group.png "Select your signing identity and provisioning profile, or re-sign with another identity")](testflight-images/group.png#lightbox)

 Дополнительные сведения об этой процедуре см. в разделе [Отправка приложения в Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md).

### <a name="submitting-your-build"></a>Отправка сборки
 Мастер публикации запустит программу Application Loader для загрузки сборки в iTunes Connect. Выберите **Доставить приложение** и затем отправьте файл `.ipa`, созданный ранее. Application Loader проверит и отправит сборку в iTunes Connect.

 Дополнительные сведения об этой процедуре см. в разделе [Отправка приложения в Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md).

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

### <a name="building-your-final-distributable"></a>Финальная сборка
 Поскольку подключаемый модуль Xamarin для Visual Studio не поддерживает архивирования приложений Xamarin.iOS для публикации в App Store, существует два варианта для публикации приложения iOS из Visual Studio. Эти особые значения приведены ниже.

1. Отправить IPA-файл, созданный при помощи команды сборки IPA для прямого распространения.
1. Отправить ZIP-архив пакета `.app`.

 Оба этих варианта описаны в руководстве [Сборка для распространения](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md).

### <a name="submitting-your-build"></a>Отправка сборки
 Для отправки приложения в Apple необходимо переместить узел сборки и воспользоваться программой Application Loader, которая устанавливается как часть Xcode. Дополнительные сведения о доступе к программе Application Loader см. в руководстве Apple [Доступ к Application Loader](https://help.apple.com/itc/apploader/#/apdATD1E927-D1E1A1303-D1E927A1126).

После открытия выберите **Доставить приложение** и отправьте ZIP-архив или файл `.ipa`, созданный ранее. Application Loader проверит и отправит сборку в iTunes Connect.

 Дополнительные сведения об этой процедуре см. в разделе [Отправка приложения в Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md).

-----

В руководстве [Публикация в App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) эти шаги описаны более подробно, обратитесь к этому руководству, если хотите глубже понять процесс отправки приложения в App Store.

При возвращении к разделу **Мои приложения** в iTunes Connect вы должны увидеть, что приложение успешно отправлено. Теперь вы готовы к проведению бета-тестирования!

## <a name="manage-beta-testing"></a>Проведение бета-тестирования

### <a name="add-metadata"></a>Добавление метаданных

Чтобы начать использовать TestFlight, перейдите к вкладке **Предварительный выпуск** приложения. Появится три вкладки со списком сборок, внутренних тестировщиков и внешних тестировщиков, как показано ниже:

[![](testflight-images/app-uploaded.png "Builds, Internal Testers, and External Testers tabs")](testflight-images/app-uploaded.png#lightbox)

Чтобы добавить метаданные к приложению, нажмите номер сборки и затем TestFlight:

[![](testflight-images/metadata.png "Add Metadata")](testflight-images/metadata.png#lightbox)

В разделе **Сведения о тестировании** вы можете предоставить тестировщикам важные сведения, касающиеся приложения, например:

- Что следует проверить
- Описание приложения.
- URL-адрес рекламных материалов — это позволит получить сведения о вашем приложении.
- URL-адрес политики конфиденциальности — URL-адрес сведений о политике конфиденциальности вашей организации.
- Адрес электронной почты для обратной связи.

Обратите внимание, что эти метаданные **не** требуются для внутренних тестировщиков, но **требуются** для внешних тестировщиков.

<a name="beta-testing"></a>

### <a name="enable-beta-testing"></a>Запуск бета-тестирования

Когда будете готовы к тестированию приложения, включите переключатель **Бета-тестирование TestFlight** для требуемой версии:

[![](testflight-images/turn-on-testing.png "Turn on the TestFlight Beta Testing switch")](testflight-images/turn-on-testing.png#lightbox)

Тестирование каждой сборки остается активным в течение **60 дней** с даты включения переключателя "Бета-тестирование TestFlight". На странице **Сведения о тестировании** отображается количество оставшихся дней для каждой сборки:

[![](testflight-images/daysleft.png "The Test Information page")](testflight-images/daysleft.png#lightbox)

Тестирование можно прервать в любое время.

### <a name="internal-testers"></a>Внутренние тестировщики

Внутренние тестировщики — это члены команды разработчиков, которым в iTunes Connect была назначена одна из следующих ролей:

- **Admin (Администратор)** — администратор отвечает за добавление и управление новыми пользователями в iTunes Connect.
- **Legal (Законный представитель)** — представитель команды разработчиков, назначается из числа администраторов, на эту роль можно назначить только одного человека. Эта роль позволяет подписывать юридически значимые документы.
- **Technical (Технический специалист)** — технический специалист может изменять большинство свойств, относящихся к приложению. Например, изменять сведения о приложении, загружать двоичные файлы и отправлять приложение на проверку.

Каждую сборку можно предоставить для тестирования не более чем 25 пользователям.

Чтобы добавить тестировщиков, перейдите к **Пользователи и роли** на начальном экране iTunes Connect:

[![](testflight-images/users-and-roles.png "Users and Roles on the main iTunes Connect screen")](testflight-images/users-and-roles.png#lightbox)

Откроется список существующих пользователей iTunes Connect. Чтобы выбрать пользователя, нажмите на его имя, включите переключатель **Внутренний тестировщик** и затем нажмите кнопку **Сохранить**:

[![](testflight-images/internal-tester.png "Turn on the Internal Tester switch")](testflight-images/internal-tester.png#lightbox)

Чтобы добавить пользователя, которого нет в списке, нажмите **+** рядом с надписью *Пользователи* и укажите имя, фамилию и адрес электронной почты для создания учетной записи. Для активации учетной записи необходимо, чтобы пользователь подтвердил свой адрес электронной почты:

[![](testflight-images/add-new-user.png "Adding a user")](testflight-images/add-new-user.png#lightbox)

Если вернуться в **Мои приложения > Предварительный выпуск > Внутренние тестировщики**, то там отобразятся пользователи, добавленные для внутреннего бета-тестирования в TestFlight:

[![](testflight-images/select-users.png "A list of users that have been added for TestFlight Internal beta testing")](testflight-images/select-users.png#lightbox)

Вы можете пригласить этих тестировщиков, выбрав имя пользователя и нажав на кнопку **Пригласить**. Они получат на электронную почту приглашение для тестирования приложения.

Можно просмотреть состояние приглашения в столбце "Состояние" на странице "Внутренние тестировщики":

[![](testflight-images/status-added.png "The invitation status")](testflight-images/status-added.png#lightbox)

### <a name="external-testers"></a>Внешние тестировщики

Перед приглашением внешних тестировщиков для бета-тестирования приложения оно должно пройти проверку бета-версии и, таким образом, соответствовать [ Руководству по публикации приложений в App Store ](https://developer.apple.com/app-store/review/guidelines/).

Чтобы отправить приложение на проверку, нажмите **Отправить на проверку бета-версии** напротив номера версии, как показано на рисунке ниже:

[![](testflight-images/beta-app-review.png "Submit For Beta App Review")](testflight-images/beta-app-review.png#lightbox)

Для передачи на проверку необходимо ввести все необходимые метаданные на странице "TestFlight бета-версия".

Теперь можно приступить к подготовке приглашений и добавлению до 2000 внешних тестировщиков на вкладке "Внешние тестировщики". Для этого необходимо ввести их адрес электронной почты, имя и фамилию, как показано на рисунке ниже. Указываемый адрес электронной почты необязательно должен совпадать с Apple ID, это только адрес, на который тестировщик получит приглашение.

[![](testflight-images/add-external.png "Invite testers")](testflight-images/add-external.png#lightbox)

Если у вас большое количество внешних тестировщиков, можно, нажав на ссылку **Импортировать файл**, импортировать файл `CSV` со следующим форматом строки:

``` 
first name, last name, email address
```

Также для упорядочивания можно распределить внешних тестировщиков по группам.

После того как вы открыли сведения о внешних тестировщиках, нажмите кнопку **Добавить** и подтвердите, что у вас есть согласие пользователей на их приглашение:

[![](testflight-images/confirm-consent.png "Confirm you have the users consent to invite them")](testflight-images/confirm-consent.png#lightbox)

Только после успешной проверки бета-версии приложения вы сможете отправлять приглашения внешним тестировщикам. На этом этапе текст в столбце **Внешнее** на странице сборки изменится на **Отправить приглашения**. Нажмите на эту ссылку, чтобы отправить приглашения всем тестировщикам, которых вы добавили.

Если приложение было отклонено, необходимо исправить проблемы, перечисленные **Центре решения проблем**, и отправить полностью обновленный двоичный файл на повторную проверку.

## <a name="as-a-beta-tester"></a>Действия тестировщика

После приглашения тестировщиков они получат приглашения по электронной почте, пример приглашения представлен на рисунке ниже:

[![](testflight-images/tester-email.png "An example invite email")](testflight-images/tester-email.png#lightbox)

После нажатия кнопки **Открыть в TestFlight** приложение будет открыто в TestFlight. Если TestFlight еще не установлен, то произойдет перенаправление в App Store для скачивания этого приложения.

После открытия вашего приложения в TestFlight на экране появятся подробные сведения о том, что именно необходимо протестировать, и будет предложено установить приложение на устройство тестировщика с iOS 8.0 (или более поздней версией):

[![](testflight-images/install-app.png "TestFlight will show details of what to test for")](testflight-images/install-app.png#lightbox)

Оранжевая точка перед именем приложения на начальном экране устройства будет указывать на то, что это тестовая сборка.

Тестировщики могут оставлять отзывы через приложение TestFlight, и вы будете получать их на электронную почту, адрес которой указан в метаданных.

### <a name="beta-testing-complete"></a>Завершение бета-тестирования

После завершения бета-тестирования можно отправить приложение на проверку компанией Apple для публикации его в магазине App Store. Этот процесс выполняется очень просто: в iTunes Connect щелкните на кнопку **Отправить на проверку**, как показано ниже:

[![](testflight-images/submit-for-review.png "Click the Submit for Review button")](testflight-images/submit-for-review.png#lightbox)

## <a name="summary"></a>Сводка

В этой статье рассмотрен процесс использования службы бета-тестирования Apple TestFlight через iTunes Connect. Статья описывает процесс отправки новой сборки в iTunes Connect и процесс приглашения внутренних и внешних тестировщиков бета-версий для тестирования вашего приложения.

## <a name="related-links"></a>Связанные ссылки

- [Создание записи iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [Публикация в App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Подготовка приложения для распространения через App Store](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#provisioning)
- [Использование TestFlight Beta](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/BetaTestingTheApp.html#//apple_ref/doc/uid/TP40011225-CH35-SW2)
