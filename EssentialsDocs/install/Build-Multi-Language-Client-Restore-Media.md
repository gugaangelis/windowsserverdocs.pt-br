---
title: "Criar mídia de restauração do cliente de vários idiomas"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fdbc016-d464-43cb-bd75-8a63e61588a2
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1ad934d297c3092050bd6adbb6bb0f50d1ec6f36
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="build-multi-language-client-restore-media"></a>Criar mídia de restauração do cliente de vários idiomas

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

> [!NOTE]
>  Você deve primeiro criar uma imagem multilíngue do Windows, conforme descrito no [passo a passo: criação de imagens multilíngues do Windows](https://technet.microsoft.com/library/jj126995) antes de adicionar o pacote de langauage do Windows Server Essentials em install.wim.  
  
 Ao criar o DVD de instalação do servidor de vários idiomas, os pacotes de idiomas serão instalados para install.wim do servidor. Os recursos localizados para o Assistente para restauração serão instalados como parte do pacote de idiomas.  
  
### <a name="to-build-a-multi-language-client-restore-media"></a>Para criar mídia de restauração de um cliente de vários idiomas  
  
1.  Monte install.wim na c:\mount, chamamos c:\mount\Program Server\bin\ClientRestore de programas\Windows pasta como a raiz da mídia de restauração do cliente: [RestoreMediaRoot] abaixo:  
  
    ```  
    dism /mount-wim /wimfile:install.wim /index:1 /mountdir:c:\mount  
    ```  
  
2.  Monte o arquivo WIM de restauração do cliente em [RestoreMediaRoot]\Sources\Boot.wim (mesmas etapas que precisam ser executadas para boot_x86.wim também). A linha de comando é:  
  
    ```  
    dism /mount-wim /wimfile:boot.wim /index:1 /mountdir:c:\mountRestore  
    ```  
  
3.  Adicione o pacote WinPE-Setup.cab à mídia de restauração, executando:  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:WinPE-Setup.cab  
    ```  
  
4.  Use o bloco de notas para editar c:\mountRestore\windows\system32\winpeshl.ini, preencher com conteúdo seguinte:  
  
    ```  
    [LaunchApp]  
    AppPath = %SYSTEMDRIVE%\sources\SelectLanguage.exe  
    ```  
  
5.  Adicione pacotes de idiomas à mídia de restauração. Adicionando pacotes de idiomas pode ser feito pelo comando a seguir em execução:  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:[language pack path]  
    ```  
  
     Pacotes de idiomas a seguir precisam ser adicionadas:  
  
    1.  Pacote de idiomas do WinPE (lp.cab)  
  
    2.  Pacote de idiomas do WinPE-Setup (WinPE-Setup _ [lang]. cab, por exemplo, o WinPE-Setup_en-us.cab)  
  
    3.  Para fontes da Ásia, incluindo zh-cn, zh-tw, zh-hk, ko-kr, ja-jp, pacote de fontes adicionais precisam ser instaladas (winpe - fontsupport-[lang]. cab, por exemplo, o winpe-fontsupport-zh-cn.cab)  
  
6.  Gere um novo arquivo Lang.ini, executando:  
  
    ```  
    dism /image:c:\mountRestore /Gen-LangINI /distribution:mount  
    ```  
  
7.  Confirme e desmonte a imagem e executando:  
  
    ```  
    dism /unmount-wim /mountdir:c:\mountRestore /commit  
    ```  
  
8.  Repita a etapa 2 para a etapa 7 para [RestoreMediaroot]\Sources\Boot_x86.wim.  
  
9. Confirme e desmonte a imagem e executando:  
  
    ```  
    dism /unmount-wim /mountdir:c:\mount /commit  
    ```  
  
## <a name="see-also"></a>Consulte também  

 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)

 [Criar e personalizar a imagem](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](../install/Additional-Customizations.md)   
 [Preparando a imagem para implantação](../install/Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](../install/Testing-the-Customer-Experience.md)

