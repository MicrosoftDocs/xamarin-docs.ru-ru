---
title: Подписывание пакета APK вручную
ms.prod: xamarin
ms.assetid: 08549E1C-7F04-4D20-9E7A-794B9D09FD12
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 5dee7e18b7f9f47493293bc552cd4f44da56ceec
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571432"
---
# <a name="manually-signing-the-apk"></a>Подписывание пакета APK вручную

После сборки приложения для выпуска и до распространения необходимо подписать пакет APK, чтобы его можно было запускать на устройстве Android. Как правило, этот процесс обрабатывается в интегрированной среде разработки, однако существуют ситуации, когда пакет APK нужно подписать вручную с использованием командной строки. В ходе процесса подписывания пакета APK выполняются следующие действия:

1. **Создание закрытого ключа** &ndash; это действие должно выполняться только один раз. Закрытый ключ необходим для подписывания пакета APK цифровой подписью.
    После подготовки закрытого ключа это действие можно пропустить для будущих сборок выпуска.

2. **Оптимизация пакета APK** &ndash; *Zipalign* представляет собой процесс оптимизации, выполняемый в приложении. Он позволяет Android более эффективно взаимодействовать с пакетом APK во время выполнения. Xamarin.Android проводит проверку во время выполнения и запретит запуск приложения, если пакет APK не был оптимизирован.

3. **Подписывание пакета APK** &ndash; в этом действии используется служебная программа **apksigner** из пакета SDK для Android и выполняется подписывание пакета APK с помощью закрытого ключа, созданного ранее. Приложения, разработанные с применением средств сборки пакета SDK для Android, вышедших до v24.0.3, будут использовать приложение **jarsigner** из пакета JDK. Оба эти средства обсуждаются более подробно ниже.

Важно соблюдать порядок действий, который зависит от средства, применяемого для подписывания пакета APK. При использовании **apksigner** важно сначала оптимизировать приложение с помощью **zipalign**, а затем подписать его с помощью **apksigner**.  Если для подписывания пакета APK необходимо использовать **jarsigner**, то важно сначала подписать пакет APK, а затем запустить **zipalign**.

## <a name="prerequisites"></a>Предварительные требования

В этом руководстве будет рассматриваться использование **apksigner** из средств сборки пакета SDK для Android v24.0.3 или более поздней версии. Предполагается, что пакет APK уже собран.

Для приложений, созданных с помощью более ранней версии средств сборки пакета SDK для Android, следует использовать средство **jarsigner**, как описано в разделе [Подписывание пакета APK с помощью jarsigner](#Sign_the_APK_with_jarsigner).

## <a name="create-a-private-keystore"></a>Создание закрытого хранилища ключей (keystore)

*keystore* — это база данных сертификатов безопасности, которая создается с помощью программы [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) из пакета SDK для Java. Хранилище ключей крайне важно для публикации приложения Xamarin.Android, так как Android не будет запускать приложения, не имеющие цифровой подписи.

Во время разработки для подписывания приложения Xamarin.Android использует хранилище ключей, что позволяет развернуть приложение непосредственно в эмуляторе на устройствах, настроенных для использования отлаживаемых приложений.
Однако это хранилище ключей не распознается как допустимое для распространения приложений.

Поэтому для подписывания приложений необходимо создать и использовать закрытое хранилище ключей. Это действие выполняется только один раз, так как один и тот же ключ будет использоваться для публикации обновлений и для подписывания других приложений.

Очень важно обеспечить безопасность этого хранилища ключей. В случае его потери будет невозможно публиковать изменения для приложения в Google Play.
Единственное решение проблемы, связанной с потерянным хранилищем ключей, заключается в создании нового хранилища ключей, повторном подписывании пакета APK новым ключом и отправке нового приложения. Старое приложение потребуется удалить из Google Play. Аналогично, в случае нарушения безопасности нового хранилища ключей или его публичного распространения в широкую доступность могут выйти неофициальные или вредоносные версии приложения.

### <a name="create-a-new-keystore"></a>Создание нового хранилища ключей

Для создания нового хранилища ключей требуется средство командной строки [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) из пакета SDK для Java. Ниже приведен пример использования **keytool** (замените `<my-filename>` именем файла хранилища ключей, а `<key-name>` — именем ключа в хранилище ключей):

```shell
$ keytool -genkeypair -v -keystore <filename>.keystore -alias <key-name> -keyalg RSA \
          -keysize 2048 -validity 10000
```

Сначала **keytool** запросит пароль для хранилища ключей. Затем он запросит определенные сведения, которые помогут в создании ключа. Ниже приведен пример создания ключа `publishingdoc`, который будет храниться в файле `xample.keystore`:

```shell
$ keytool -genkeypair -v -keystore xample.keystore -alias publishingdoc -keyalg RSA -keysize 2048 -validity 10000
Enter keystore password:
Re-enter new password:
What is your first and last name?
  [Unknown]:  Ham Chimpanze
What is the name of your organizational unit?
  [Unknown]:  NASA
What is the name of your organization?
  [Unknown]:  NASA
What is the name of your City or Locality?
  [Unknown]:  Cape Canaveral
What is the name of your State or Province?
  [Unknown]:  Florida
What is the two-letter country code for this unit?
  [Unknown]:  US
Is CN=Ham Chimpanze, OU=NASA, O=NASA, L=Cape Canaveral, ST=Florida, C=US correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA1withRSA) with a validity of 10,000 days
        for: CN=Ham Chimpanze, OU=NASA, O=NASA, L=Cape Canaveral, ST=Florida, C=US
Enter key password for <publishingdoc>
        (RETURN if same as keystore password):
Re-enter new password:
[Storing xample.keystore]
```

Чтобы получить список ключей, которые хранятся в хранилище ключей, используйте **keytool** с параметром &ndash; `list`:

```shell
$ keytool -list -keystore xample.keystore
```

## <a name="zipalign-the-apk"></a>Оптимизация пакета APK

Перед подписыванием пакета APK с помощью **apksigner** сначала необходимо оптимизировать файл с помощью средства **zipalign** из пакета SDK для Android. **zipalign** выравнивает ресурсы в пакете APK по 4-байтовым границам. Благодаря выравниванию Android может быстро загружать ресурсы из пакета APK, повышая производительность приложения и потенциально сокращая использование памяти. Чтобы определить, прошел ли пакет APK оптимизацию, Xamarin.Android будет проводить проверку во время выполнения. Если пакет APK не оптимизирован, приложение не запустится.

Следующая команда использует подписанный пакет APK, а ее результатом будет подписанный, оптимизированный пакет APK **helloworld.apk**, готовый к распространению.

```shell
$ zipalign -f -v 4 mono.samples.helloworld-unsigned.apk helloworld.apk
```

## <a name="sign-the-apk"></a>Подписывание пакета APK

Оптимизированный пакет APK необходимо подписать с использованием хранилища ключей. Для этого предназначено средство **apksigner**, находящееся в каталоге **build-tools** версии средств сборки пакета SDK.  Например, если установлены средства сборки пакета SDK для Android v25.0.3, **apksigner** можно найти в следующем каталоге:

```bash
$ ls $ANDROID_HOME/build-tools/25.0.3/apksigner
/Users/tom/android-sdk-macosx/build-tools/25.0.3/apksigner*
```

В следующем фрагменте предполагается, что средство **apksigner** доступно для переменной среды `PATH`. Пакет APK подписывается с помощью псевдонима ключа `publishingdoc`, содержащегося в файле **xample.keystore**.

```shell
$ apksigner sign --ks xample.keystore --ks-key-alias publishingdoc mono.samples.helloworld.apk
```

При выполнении этой команды при необходимости **apksigner** запросит пароль для хранилища ключей.

Дополнительные сведения об использовании **apksigner** см. в [документации Google](https://developer.android.com/studio/command-line/apksigner.html).

> [!NOTE]
> Согласно [проблеме Google 62696222](https://issuetracker.google.com/issues/62696222), средство **apksigner** "отсутствует" в пакете SDK для Android. Решением этой проблемы является установка средств сборки пакета SDK для Android v25.0.3 и использование этой версии **apksigner**.  

<a name="Sign_the_APK_with_jarsigner"></a>

### <a name="sign-the-apk-with-jarsigner"></a>Подписывание пакета APK с помощью jarsigner

> [!WARNING]
> Сведения в этом разделе применимы, если пакет APK необходимо подписать с помощью программы **jarsigner**. Для подписывания пакета APK разработчикам рекомендуется использование **apksigner**.

Эта методика предполагает подписывание APK-файла с помощью **[jarsigner](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jarsigner.html)** из пакета SDK для Java.  Средство **jarsigner** входит в состав пакета SDK для Java.

Далее показано, как подписать пакет APK с помощью **jarsigner** и ключа `publishingdoc`, содержащегося в файле хранилища ключей **xample.keystore**:

```shell
$ jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore xample.keystore mono.samples.helloworld.apk publishingdoc
```

> [!NOTE]
> При использовании **jarsigner** важно _сначала_ подписать пакет APK, а затем использовать средство **zipalign**.  

## <a name="related-links"></a>Связанные ссылки

- [Подписывание приложений](https://source.android.com/security/apksigning/)
- [jarsigner](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jarsigner.html)
- [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html)
- [zipalign](https://developer.android.com/studio/command-line/zipalign.html)
- [Средства сборки 26.0.0: куда пропало средство apksigner?](https://issuetracker.google.com/issues/62696222)
