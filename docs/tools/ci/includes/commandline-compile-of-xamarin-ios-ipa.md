---
ms.openlocfilehash: 31c8d1b781648f2d9c69062c52e71478fc7a496b
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "69529117"
---

Следующая командная строка позволяет указать сборку выпуска решения **SOLUTION_FILE. sln** для iPhone. Расположение IPA можно задать, указав `IpaPackageDir` свойство в командной строке:

- На компьютере Mac с помощью **Xbuild**:

  ```
  xbuild /p:Configuration="Release" \ 
         /p:Platform="iPhone" \ 
         /p:IpaPackageDir="$HOME/Builds" \
         /t:Build MyProject.sln
  ```

Команда **Xbuild** обычно находится в каталоге **/либрари/фрамеворкс/моно.фрамеворк/коммандс**.

- В Windows с помощью **MSBuild**:

  ```
  msbuild /p:Configuration="Release" 
          /p:Platform="iPhone" 
          /p:IpaPackageDir="%USERPROFILE%\Builds" 
          /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser"  
          /t:Build MyProject.sln
  ```

**MSBuild** не будет автоматически развертывать `$( )` выражения, переданные в командной строке. По этой причине рекомендуется использовать полный путь при задании в `IpaPackageDir` командной строке.

Дополнительные сведения о свойстве см. в [заметках о выпуске iOS 9,8](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/xamarin.ios_9/xamarin.ios_9.8.md#new-msbuild-property-ipapackagedir-to-customize-ipa-output-location) `IpaPackageDir` .
