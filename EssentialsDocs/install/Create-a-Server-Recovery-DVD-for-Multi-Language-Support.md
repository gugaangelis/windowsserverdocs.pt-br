---
title: "Criar uma DVD de recuperação do servidor para suporte a vários idiomas"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-server-recovery-dvd-for-multi-language-support"></a>Criar uma DVD de recuperação do servidor para suporte a vários idiomas

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_MLHeadedRecovery"></a>Criar uma instalação do servidor e o DVD de recuperação de servidor para suporte a vários idiomas em servidores administrados localmente  
  
> [!NOTE]
>  Você deve primeiro criar uma imagem multilíngue do Windows, conforme descrito no [passo a passo: criação de imagens multilíngues do Windows](https://technet.microsoft.com/library/jj126995) antes de adicionar o pacote de langauage do Windows Server Essentials em install.wim.  
  
 Há duas fases da instalação: o ambiente de pré-instalação do Windows (Windows PE) e a configuração inicial. Por padrão, a página de seleção de idioma na configuração inicial não será exibida.  
  
-   Para uma instalação de OEM administrado remotamente ou um cenário de pré-instalação OEM, você precisa adicionar um registro chave usando o seguinte comando para exibir a página de seleção de idioma na configuração inicial.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
    > [!IMPORTANT]
    >  Quando os OEMs cria uma imagem no laboratório, deve escolher **inglês** como o idioma durante a fase de Windows PE da instalação.  
  
-   Para um cenário de revendedor opção Kit (ROK), os clientes recebem um DVD e talvez alguns itens de hardware. O cliente deve capaz de selecionar o idioma durante a instalação do Windows PE e a página de seleção de idioma não é mais exibida durante a configuração inicial.  
  
 Você pode optar por enviar um único DVD de camada dupla que contém vários idiomas.  
  
 Esta seção descreve como adicionar suporte a idiomas à instalação do Windows. A ferramenta principal para personalizar o Windows PE 3.0 é o DISM (gerenciamento), uma ferramenta de linha de comando e manutenção de imagens de implantação. Essa solução permite os seguintes cenários:  
  
1.  Criar instalações multilíngue  
  
2.  Criar a mídia distribuível  
  
### <a name="prerequisites"></a>Pré-requisitos  
 Para adicionar suporte multilíngue à instalação do Windows, você precisa do seguinte:  
  

-   Um computador do técnico que fornece todas as ferramentas e os arquivos de origem necessários para criar uma imagem personalizada do WinPE. Para obter mais informações, consulte [preparar o computador do técnico](Prepare-the-Technician-Computer.md).  

-   Um computador do técnico que fornece todas as ferramentas e os arquivos de origem necessários para criar uma imagem personalizada do WinPE. Para obter mais informações, consulte [preparar o computador do técnico](../install/Prepare-the-Technician-Computer.md).  

  
-   Um DVD do Windows Server Essentials.  
  
-   Um servidor de Windows Essentials ao pacote de idiomas DVD.  
  
###  <a name="BKMK_Steps"></a>Adicionando suporte a vários idiomas  
 Para adicionar suporte a vários idiomas à instalação do Windows você atualizar o Install.wim, adicionando o Windows Server 2012 e pacotes de idiomas do Windows Server Essentials para ele.  
  
#### <a name="update-installwim"></a>Atualizar Install.wim  
 Nesta etapa, você adiciona Windows Server 2012 e pacotes de idiomas do Windows Server Essentials em Install.wim.  
  
> [!NOTE]
>  Verifique se que você instale pacotes de idiomas para o Windows Server 2012. Isso garante que você obtenha a identidade visual apropriado. O Windows Server 2012 multilíngue usuário Interface pacotes de idiomas estão disponíveis em [Microsoft.com](https://www.microsoft.com/OEM/en/installation/downloads/Pages/technical-downloads.aspx). Siga as instruções conforme descrito no [passo a passo: criação de imagens multilíngues do Windows sobre como criar um multilíngue](https://technet.microsoft.com/library/jj126995.aspx) sobre como criar uma imagem multilíngue do Windows antes de adicionar o pacote de idiomas no Windows Server Essentials Install.wim.  
>   
>  Pacotes de idiomas do Windows Server Essentials estão disponíveis na mídia do pacote de idioma em \Language Packs\\ < CultureName\ >.  
  
> [!NOTE]
>  Nem todos os pacotes de idiomas podem não está disponível antes do lançamento do Windows Server 2012.  
  
###### <a name="to-add-language-packs-to-installwim"></a>Para adicionar pacotes de idiomas para Install.wim  
  
1.  Adicione pacotes de idiomas do sistema operacional e do produto em Install.wim como a seguir (Este exemplo usa francês):  
  
    ```  
    Dism /Mount-Wim /WimFile:C:\my_distribution\sources\install.wim /index:1 /MountDir:C:\InstallMount  
    Mkdir c:\temp_Scratch  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<path_to_OS_language_packs>\fr-fr\lp.cab  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<OCD\Language Packs\FR-FR\lp.cab  
  
    ```  
  

2.  Adicionar arquivos específicos do idioma para dar suporte à criação do cliente Backup Restaurar USB flash drive, usando o procedimento descrito [criar mídia de restauração de cliente de vários idiomas](Build-Multi-Language-Client-Restore-Media.md).  

2.  Adicionar arquivos específicos do idioma para dar suporte à criação do cliente Backup Restaurar USB flash drive, usando o procedimento descrito [criar mídia de restauração de cliente de vários idiomas](../install/Build-Multi-Language-Client-Restore-Media.md).  

  
3.  Recriar o arquivo Lang.ini na mídia flexível para refletir o suporte a idiomas adicionais usando o `DISM /Gen-LangINI`comando, por exemplo:  
  
    ```  
    Dism /image:C:\InstallMount /Gen-LangINI /distribution:C:\my_distribution  
  
    ```  
  
4.  Salve suas alterações de volta na imagem usando o `DISM /unmount /commit`comando, por exemplo:  
  
    ```  
    Dism /Unmount-Wim /MountDir:C:\InstallMount /Commit  
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

