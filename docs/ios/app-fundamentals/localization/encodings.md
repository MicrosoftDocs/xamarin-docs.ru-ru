---
title: Кодировки интернационализации в Xamarin. iOS
description: В этом документе описываются кодировки интернационализации в Xamarin. iOS, обсуждаются Доступные кодировки и способы их добавления в приложение.
ms.prod: xamarin
ms.assetid: F5117294-28BB-4583-B6A0-A339B050FDE1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/28/2017
ms.openlocfilehash: 88f95f797d1230c818a635376f7b17a2264afadf
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930342"
---
# <a name="internationalization-encodings-in-xamarinios"></a>Кодировки интернационализации в Xamarin. iOS

По умолчанию в библиотеку классов Xamarin. iOS включены не все кодировки.

Чтобы уменьшить размер приложения, Xamarin. iOS не включает в себя какую бы то ни было определенную кодировку, и необходимо указать mtouch включить сборки, содержащие поддержку необходимой кодировки.

Это можно сделать, выбрав дополнительные кодировки в области сборка iOS/дополнительно в Visual Studio для Mac или Visual Studio.

 [![Выбор дополнительных кодировок](encodings-images/00.png)](encodings-images/00.png#lightbox)

 [![Выбор дополнительных кодировок](encodings-images/00a.png)](encodings-images/00a.png#lightbox)

Можно выбрать один из следующих элементов:

- ККЯ: для Чинисе, японского и корейского языков
- Ближний Восток: арабский, иврит, Турецкий и Latin5.
- другое: кириллица, Балтийская, вьетнамский, украинский и тайский
- редкие: кодировки EBCDIC и другие редкие кодовые страницы
- Западная часть: языки латинского алфавита, Пасха и Западная Европа
- все

 <a name="cjk"></a>

## <a name="cjk"></a>ККЯ

- CP51932
- CP932
- CP936
- CP949
- CP950
- CP54936

 <a name="mideast"></a>

## <a name="mideast"></a>Ближний Восток

- CP1254
- CP1255
- CP1256
- CP28596
- CP28598
- CP28599
- CP38598

 <a name="other"></a>

## <a name="other"></a>др.

- CP1251
- CP1257
- CP1258
- CP20866
- CP21866
- CP28594
- CP28595
- CP57002
- CP874

 <a name="rare"></a>

## <a name="rare"></a>маловероятны

- CP1026
- CP1047
- CP1140
- CP1141
- CP1142
- CP1143
- CP1144
- CP1145
- CP1146
- CP1147
- CP1148
- CP1149
- CP20273
- CP20277
- CP20278
- CP20280
- CP20284
- CP20285
- CP20290
- CP20297
- CP20420
- CP20424
- CP20871
- CP21025
- CP37
- CP500
- CP708
- CP852
- CP855
- CP857
- CP858
- CP862
- CP864
- CP866
- CP869
- CP870
- CP875

 <a name="west"></a>

## <a name="west"></a>запад

- CP10000
- CP10079
- CP1250
- CP1252
- CP1253
- CP28592
- CP28593
- CP28597
- CP28605
- CP437
- CP850
- CP860
- CP861
- CP863
- CP865
