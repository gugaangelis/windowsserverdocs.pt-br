---
title: Criar a mídia de restauração do cliente de vários idiomas
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 2fdbc016-d464-43cb-bd75-8a63e61588a2
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 8440fe0a32128182b86d26190c9b0448c64807d8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623914"
---
# <a name="build-multi-language-client-restore-media"></a>Criar a mídia de restauração do cliente de vários idiomas

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

> [!NOTE]
>  Primeiro, você deve criar uma imagem multilíngue do Windows, conforme descrito no [passo a passo: criação de imagem multilíngüe do Windows](/previous-versions/windows/it-pro/windows-8.1-and-8/jj126995(v=win.10)) antes de adicionar o Windows Server Essentials langauage Pack ao install. wim.

 Ao criar o DVD de instalação do servidor de vários idiomas, os pacotes de idiomas serão instalados no Servidor install.wim. Os recursos localizados para o assistente de restauração serão instalados como parte do pacote de idiomas.

### <a name="to-build-a-multi-language-client-restore-media"></a>Para criar uma mídia de restauração do cliente de vários idiomas.

1.  Monte install.wim em c:\mount, chamamos a pasta c:\mount\Program Files\Windows Server\bin\ClientRestore como a raiz da mídia de restauração do cliente: [RestoreMediaRoot] abaixo:

    ```
    dism /mount-wim /wimfile:install.wim /index:1 /mountdir:c:\mount
    ```

2.  Monte o arquivo WIM de restauração do cliente em [RestoreMediaRoot]\Sources\Boot.wim (As mesmas etapas precisam ser realizadas para boot_x86.wim também). A linha de comando é:

    ```
    dism /mount-wim /wimfile:boot.wim /index:1 /mountdir:c:\mountRestore
    ```

3.  Adicione o pacote WinPE-Setup.cab à mídia de restauração executando:

    ```
    dism /image:c:\mountRestore /add-package /packagepath:WinPE-Setup.cab
    ```

4.  Use o bloco de notas para editar c:\mountRestore\windows\system32\winpeshl.ini, preencha com o seguinte conteúdo:

    ```
    [LaunchApp]
    AppPath = %SYSTEMDRIVE%\sources\SelectLanguage.exe
    ```

5.  Adicionar pacotes de idiomas à mídia de restauração. Pacotes de idiomas podem ser adicionados executando o seguinte comando:

    ```
    dism /image:c:\mountRestore /add-package /packagepath:[language pack path]
    ```

     Os seguintes pacotes de idiomas precisam ser adicionais:

    1.  Pacote de idioma WinPE (lp.cab)

    2.  Pacote do idioma WinPE-Setup (WinPE-Setup_[lang].cab, por exemplo, WinPE-Setup_en-us.cab)

    3.  Para fontes asiáticas, incluindo zh-cn, zh-tw, zh-hk, ko-kr, ja-jp, um pacote de fontes adicional precisa ser instalado (winpe-fontsupport-[lang].cab, por exemplo, winpe-fontsupport-zh-cn.cab)

6.  Gere o novo arquivo Lang.ini executando:

    ```
    dism /image:c:\mountRestore /Gen-LangINI /distribution:mount
    ```

7.  Confirme e desmonte a imagem executando:

    ```
    dism /unmount-wim /mountdir:c:\mountRestore /commit
    ```

8.  Repita as etapas de 2 até 7 para [RestoreMediaroot]\Sources\Boot_x86.wim.

9. Confirme e desmonte a imagem executando:

    ```
    dism /unmount-wim /mountdir:c:\mount /commit
    ```

## <a name="see-also"></a>Consulte Também

 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md) [personalizações adicionais](Additional-Customizations.md) [preparando a imagem para](Preparing-the-Image-for-Deployment.md) [testar a implantação da experiência do cliente](Testing-the-Customer-Experience.md)