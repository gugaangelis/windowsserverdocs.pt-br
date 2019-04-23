---
title: Criar um DVD de recuperação de servidor para suporte a vários idiomas
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c7da0f6c-9732-4784-9c28-7dad72c4071d
4author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ac547f97b48e4cd0ebf87e0935cadc2c539b4d0b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854997"
---
# <a name="create-a-server-recovery-dvd-for-multi-language-support"></a>Criar um DVD de recuperação de servidor para suporte a vários idiomas

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_MLHeadedRecovery"></a> Criar uma instalação de servidor e DVD de recuperação de servidor para suporte a vários idiomas em servidores administrados localmente  
  
> [!NOTE]
>  Você deve primeiro criar uma imagem multilíngue do Windows, conforme descrito no [passo a passo: Criação de imagem multilíngue do Windows](https://technet.microsoft.com/library/jj126995) antes de adicionar o pacote de idiomas do Windows Server Essentials em Install. wim.  
  
 Há duas fases de instalação: o Ambiente de Pré-Instalação do Windows (Windows PE) e a configuração inicial. Por padrão, a página de seleção de idioma na configuração inicial não será exibida.  
  
-   Para uma instalação administrada remotamente de OEM ou um cenário de pré-instalação de OEM, é preciso adicionar uma chave de registro usando o seguinte comando para exibir a página de seleção de idioma na configuração inicial.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
    > [!IMPORTANT]
    >  Quando os OEMs criam uma imagem no laboratório, eles devem escolher o **inglês** como idioma durante a fase de configuração do Windows PE.  
  
-   Para um cenário de Kit de Opção de Revendedor (Reseller Option Kit - ROK), os clientes recebem um DVD, e, talvez, algum hardware. O cliente deve poder selecionar o idioma durante a instalação do Windows PE, e a página de seleção de idioma não é mais exibida durante a configuração inicial.  
  
 Você pode escolher enviar um único DVD de duas camadas contendo vários idiomas.  
  
 Esta seção descreve como adicionar suporte a idiomas à Instalação do Windows. A principal ferramenta para personalizar o Windows PE 3.0 é DISM (Gerenciamento e Manutenção de Imagens de Implantação), uma ferramenta de linha de comando. Essa solução possibilita os seguintes cenários:  
  
1.  Criar instalações multilíngues  
  
2.  Criar uma mídia de distribuição  
  
### <a name="prerequisites"></a>Pré-requisitos  
 Para adicionar suporte multilíngue à Instalação do Windows, é preciso:  
  

-   Um computador do técnico que forneça todas as ferramentas e arquivos de origem necessários para criar uma imagem personalizada do WinPE. Para obter mais informações, consulte [Prepare the Technician Computer](Prepare-the-Technician-Computer.md).  

-   Um computador do técnico que forneça todas as ferramentas e arquivos de origem necessários para criar uma imagem personalizada do WinPE. Para obter mais informações, consulte [Prepare the Technician Computer](../install/Prepare-the-Technician-Computer.md).  

  
-   Um DVD do Windows Server Essentials.  
  
-   Um Windows Server Essentials Language Pack DVD.  
  
###  <a name="BKMK_Steps"></a> Adicionando suporte a vários idiomas  
 Para adicionar suporte a vários idiomas à instalação do Windows, você atualiza o Install. wim adicionando-se o Windows Server 2012 e pacotes de idioma do Windows Server Essentials.  
  
#### <a name="update-installwim"></a>Atualizar Install.wim  
 Nesta etapa, você adiciona o Windows Server 2012 e pacotes de idiomas do Windows Server Essentials em Install. wim.  
  
> [!NOTE]
>  Verifique se que você instale pacotes de idiomas para o Windows Server 2012. Isso garante que você obtenha a identidade visual correta. O Windows Server 2012 Multilingual User Interface Language Packs estão disponíveis no [Microsoft.com](https://www.microsoft.com/OEM/en/installation/downloads/Pages/technical-downloads.aspx). Siga as instruções conforme descrito no [passo a passo: Criação de imagem multilíngue do Windows sobre como criar um multilíngue](https://technet.microsoft.com/library/jj126995.aspx) sobre como criar uma imagem multilíngue do Windows antes de adicionar o pacote de idiomas do Windows Server Essentials ao Install. wim.  
>   
>  Pacotes de idiomas do Windows Server Essentials estão disponíveis na mídia de pacote de idiomas em \Language Packs\\< CultureName\>.  
  
> [!NOTE]
>  Nem todos os pacotes de idioma talvez não está disponível antes do lançamento do Windows Server 2012.  
  
###### <a name="to-add-language-packs-to-installwim"></a>Para adicionar pacotes de idioma a Install.wim  
  
1.  Adicione o sistema operacional e os pacotes de idioma do produto ao Install.wim como segue (neste exemplo, usamos francês):  
  
    ```  
    Dism /Mount-Wim /WimFile:C:\my_distribution\sources\install.wim /index:1 /MountDir:C:\InstallMount  
    Mkdir c:\temp_Scratch  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<path_to_OS_language_packs>\fr-fr\lp.cab  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<OCD\Language Packs\FR-FR\lp.cab  
  
    ```  
  

2.  Adicione arquivos específicos do idioma ao suporte criando USB de restauração de Backup do cliente unidade flash, usando o procedimento descrito em [criar mídia de restauração de cliente de vários idiomas](Build-Multi-Language-Client-Restore-Media.md).  

2.  Adicione arquivos específicos do idioma ao suporte criando USB de restauração de Backup do cliente unidade flash, usando o procedimento descrito em [criar mídia de restauração de cliente de vários idiomas](../install/Build-Multi-Language-Client-Restore-Media.md).  

  
3.  Recrie o arquivo Lang.ini na mídia separada para refletir o suporte a idiomas adicional usando o comando `DISM /Gen-LangINI` , por exemplo:  
  
    ```  
    Dism /image:C:\InstallMount /Gen-LangINI /distribution:C:\my_distribution  
  
    ```  
  
4.  Salve suas alterações de volta na imagem usando o comando `DISM /unmount /commit` , por exemplo:  
  
    ```  
    Dism /Unmount-Wim /MountDir:C:\InstallMount /Commit  
    ```  
  
## <a name="see-also"></a>Consulte também  

 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do usuário](Testing-the-Customer-Experience.md)

 [Criando e personalizando a imagem](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](../install/Additional-Customizations.md)   
 [Preparando a imagem para implantação](../install/Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do usuário](../install/Testing-the-Customer-Experience.md)

