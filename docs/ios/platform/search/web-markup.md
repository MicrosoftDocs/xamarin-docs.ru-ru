---
title: Поиск в веб-разметке в Xamarin. iOS
description: В этом документе описывается создание результатов поиска на основе веб-узла, которые связываются с приложением Xamarin. iOS. В нем обсуждается включение индексации веб-содержимого, обеспечение возможности обнаружения веб-сайта приложения, использование баннеров смарт-приложений, универсальные ссылки и многое другое.
ms.prod: xamarin
ms.assetid: 876315BA-2EF9-4275-AE33-A3A494BBF7FD
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: cd9e5e3fc604fc6e6993b10424a209aa6c382a10
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86935061"
---
# <a name="search-with-web-markup-in-xamarinios"></a>Поиск в веб-разметке в Xamarin. iOS

Для приложений, предоставляющих доступ к их содержимому через веб-сайт (не только в пределах приложения), веб-содержимое может быть отмечено специальными ссылками, которые будут обходиться Apple и обеспечить глубокую компоновку приложения на устройстве iOS 9 пользователя.

Если приложение iOS уже поддерживает мобильное связывание и веб-сайт предложит ссылки на содержимое в приложении, то веб-обходчик Apple _апплебот_ будет индексировать это содержимое и автоматически добавит его в свой облачный индекс:

[![Обзор облачного индекса](web-markup-images/webmarkup01.png)](web-markup-images/webmarkup01.png#lightbox)

Apple получит эти результаты в результатах поиска Spotlight и Safari.
Если пользователь отменяет один из этих результатов (и приложение установлено), они будут передаваться в содержимое приложения:

[![Глубокая компоновка веб-сайта в результатах поиска](web-markup-images/webmarkup02.png)](web-markup-images/webmarkup02.png#lightbox)

## <a name="enabling-web-content-indexing"></a>Включение индексации веб-содержимого

Для обеспечения поиска содержимого приложения с помощью веб-разметки необходимо выполнить четыре шага.

1. Убедитесь, что компания Apple может обнаружить и индексировать веб-сайт вашего приложения, определив его в качестве веб-сайта **поддержки** или **маркетинга** в iTunes Connect.
2. Убедитесь, что веб-сайт вашего приложения содержит необходимую разметку для реализации глубокой компоновки для мобильных устройств. Дополнительные сведения см. в следующих разделах.
3. Включите обработку глубокой ссылки в приложении iOS.
4. Добавьте разметку для структурированных данных, попадающего на веб-сайт приложения, чтобы предоставить пользователю богатый и привлекательный результат. Хотя этот шаг не является обязательным, настоятельно рекомендуется использовать Apple.

В следующих разделах подробно рассматриваются эти действия.

## <a name="make-your-apps-website-discoverable"></a>Как сделать веб-сайт приложения обнаруживаемым

Самый простой способ найти веб-сайт приложения в Apple — это использовать его как веб-сайт **поддержки** или **маркетинга** при отправке приложения в Apple через iTunes Connect.

## <a name="using-smart-app-banners"></a>Использование баннеров смарт-приложений

Предоставьте баннер Smart App на веб-сайте, чтобы предоставить четкую ссылку на приложение. Если приложение еще не установлено, Safari автоматически выдаст пользователю запрос на установку приложения. В противном случае можно коснуться ссылки **View** , чтобы запустить приложение с веб-сайта. Например, чтобы создать баннер смарт-приложения, можно использовать следующий код:

```html
<meta name="AppName" content="app-id=123456, app-argument=http://company.com/AppName">
```

Дополнительные сведения см. [в статье продвижение приложений Apple с помощью смарт-приложений](https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html) .

## <a name="using-universal-links"></a>Использование универсальных ссылок

В iOS 9 универсальные ссылки обеспечивают лучшую альтернативу созданию баннеров смарт-приложений или существующих пользовательских схем URL-адресов, предоставляя следующие возможности:

- **Уникальный** — один и тот же URL-адрес не может быть заявлен несколькими веб-сайтами.
- **Secure** — для веб-сайта требуется подписанный сертификат, который гарантирует, что веб-сайт принадлежит вам и правильно связан с вашим приложением.
- **Гибкая** — конечный пользователь может управлять запуском веб-сайта или приложения.
- **Universal** — один и тот же URL-адрес можно использовать для определения содержимого веб-сайта и вашего приложения.

## <a name="using-twitter-cards"></a>Использование карточек Twitter

Вы можете предоставить глубокие ссылки на содержимое вашего приложения с помощью карточки Twitter. Например.

```html
<meta name="twitter:app:name:iphone" content="AppName">
<meta name="twitter:app:id:iphone" content="AppNameID">
<meta name="twitter:app:url:iphone" content="AppNameURL">
```

Дополнительные сведения см. в документации по [протоколу карты Twitter](https://developer.twitter.com/en/docs/tweets/optimize-with-cards/overview/abouts-cards) в Twitter.

## <a name="using-facebook-app-links"></a>Использование ссылок на приложения Facebook

Ссылки на содержимое приложения можно предоставить с помощью ссылки на приложение Facebook. Например.

```html
<meta property="al:ios:app_name" content="AppName">
<meta property="al:ios:app_store_id" content="AppNameID">
<meta property="al:ios:url" content="AppNameURL">
```

Дополнительные сведения см. в документации по [ссылкам для приложений](https://developers.facebook.com/docs/applinks) Facebook.

## <a name="opening-deep-links"></a>Открытие ссылок с глубокими ссылками

Необходимо добавить поддержку для открытия и отображения ссылок в приложении Xamarin. iOS. Измените файл **AppDelegate.CS** и переопределите `OpenURL` метод, чтобы он обрабатывал пользовательский формат URL-адреса. Например.

```csharp
public override bool OpenUrl (UIApplication application, NSUrl url, string sourceApplication, NSObject annotation)
{

  // Handling a URL in the form http://company.com/appname/?123
  try {
    var components = new NSUrlComponents(url,true);
    var path = components.Path;
    var query = components.Query;

    // Is this a known format?
    if (path == "/appname") {
      // Display the view controller for the content
      // specified in query (123)
      return ContentViewController.LoadContent(query);
    }
  } catch {
    // Ignore issue for now
  }

  return false;
}
```

В приведенном выше коде мы ищем URL-адрес, содержащий `/appname` и передающий значение `query` ( `123` в этом примере), в пользовательский контроллер представления в приложении для отображения запрошенного содержимого пользователю.

## <a name="providing-rich-results-with-structured-data"></a>Предоставление расширенных результатов структурированным данным

Включив разметку структурированных данных, можно предоставить конечному пользователю расширенные результаты поиска, которые выходят за рамки просто заголовка и описания. Включите изображения, данные приложения (например, оценки) и действия в результаты с помощью разметки структурированных данных.

Расширенные результаты являются более привлекательными и могут помочь улучшить ранжирование в облачном индексе поиска, заманчив больше пользователей на взаимодействие с ними.

Один из вариантов предоставления структурированных данных — с помощью Open Graph. Например.

```html
<meta property="og:image" content="http://company.com/appname/icon.jpg">
<meta property="og:audio" content="http://company.com/appname/theme.m4a">
<meta property="og:video" content="http://company.com/appname/tutorial.mp4">
```

Дополнительные сведения см. на веб-сайте [Open Graph](https://ogp.me) .

Другим распространенным форматом разметки структурированных данных является формат Schema. org микроданные. Например.

```html
<div itemprop="aggregateRating" itemscope itemtype="http://schema.org/AggregateRating">
  <span itemprop="ratingValue">4** stars -
  <span itemprop="reviewCount">255** reviews
```

Те же сведения можно представить в формате JSON-LD схемы. org:

```html
<script type="application/ld+json">
  "@content":"http://schema.org",
  "@type":"AggregateRating",
  "ratingValue":"4",
  "reviewCount":"255"
</script>
```

Ниже приведен пример метаданных с веб-сайта, предоставляющих пользователям подробные результаты поиска:

[![Расширенные результаты поиска через разметку структурированных данных](web-markup-images/deeplink01.png)](web-markup-images/deeplink01.png#lightbox)

В настоящее время Apple поддерживает следующие типы схем из schema.org:

- аггрегатератинг
- имажеобжект
- интерактионкаунт
- Предложения
- Организация
- прицеранже
- Рецепт
- сеарчактион

Дополнительные сведения об этих типах схем см. в разделе [Schema.org](https://schema.org).

## <a name="providing-actions-with-structured-data"></a>Предоставление действий с структурированными данными

Определенные типы структурированных данных позволят конечному пользователю выполнять действия по поиску. В настоящее время поддерживаются следующие действия:

- Набор номера телефона.
- Получение направления карт по заданному адресу.
- Воспроизведение аудио или видео файла.

Например, определение действия для набора номера телефона может выглядеть следующим образом:

```html
<div itemscope itemtype="http://schema.org/Organization">
  <span itemprop="telephone">(408) 555-1212**
```

Когда результат поиска будет представлен конечному пользователю, в результате отобразится маленький значок телефона. Если пользователь касается значка, будет вызвано указанное число.

Следующий HTML-код добавляет действие для воспроизведения звукового файла из результатов поиска:

```html
<div itemscope itemtype="http://schema.org/AudioObject">
  <span itemprop="contentUrl">http://company.com/appname/greeting.m4a**
```

Наконец, следующий HTML-код добавит действие для получения направлений из результата поиска:

```html
<div itemscope itemtype="http://schema.org/PostalAddress">
  <span itemprop="streetAddress">1 Infinite Loop**
  <span itemprop="addressLocality">Cupertino**
  <span itemprop="addressRegion">CA**
  <span itemprop="postalCode">95014**
```

Дополнительные сведения см. в разделе [веб-сайт разработчика поиска приложений](https://developer.apple.com/ios/search/)Apple.

## <a name="related-links"></a>Связанные ссылки

- [Примеры для iOS 9](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [iOS 9 для разработчиков](https://developer.apple.com/ios/pre-release/)
- [iOS 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Инструкции по программированию поиска приложений](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
