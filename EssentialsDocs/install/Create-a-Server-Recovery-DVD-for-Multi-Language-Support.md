---
title: Criar um DVD de recuperação de servidor para suporte a vários idiomas
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: c7da0f6c-9732-4784-9c28-7dad72c4071d
author: daveba
ms.author: daveba
ms.openlocfilehash: e00bf2db8216489787ba3a476a79d7567d4d0d78
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181412"
---
# <a name="create-a-server-recovery-dvd-for-multi-language-support"></a>Criar um DVD de recuperação de servidor para suporte a vários idiomas

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="create-a-server-setup-and-server-recovery-dvd-for-multiple-language-support-on-locally-administered-servers"></a><a name="BKMK_MLHeadedRecovery"></a>Criar um DVD de instalação do servidor e de recuperação do servidor para suporte a vários idiomas em servidores administrados localmente

> [!NOTE]
>  Primeiro, você deve criar uma imagem multilíngue do Windows, conforme descrito no [passo a passo: criação de imagem multilíngüe do Windows](https://technet.microsoft.com/library/jj126995) antes de adicionar o Windows Server Essentials langauage Pack ao install. wim.

 Há duas fases de instalação: o Ambiente de Pré-Instalação do Windows (Windows PE) e a configuração inicial. Por padrão, a página de seleção de idioma na configuração inicial não será exibida.

- Para uma instalação administrada remotamente de OEM ou um cenário de pré-instalação de OEM, é preciso adicionar uma chave de registro usando o seguinte comando para exibir a página de seleção de idioma na configuração inicial.

  ```
  %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f
  ```

  > [!IMPORTANT]
  >  Quando os OEMs criam uma imagem no laboratório, eles devem escolher o **inglês** como idioma durante a fase de configuração do Windows PE.

- Para um cenário de Kit de Opção de Revendedor (Reseller Option Kit - ROK), os clientes recebem um DVD, e, talvez, algum hardware. O cliente deve poder selecionar o idioma durante a instalação do Windows PE, e a página de seleção de idioma não é mais exibida durante a configuração inicial.

  Você pode escolher enviar um único DVD de duas camadas contendo vários idiomas.

  Esta seção descreve como adicionar suporte a idiomas à Instalação do Windows. A principal ferramenta para personalizar o Windows PE 3.0 é DISM (Gerenciamento e Manutenção de Imagens de Implantação), uma ferramenta de linha de comando. Essa solução possibilita os seguintes cenários:

1.  Criar instalações multilíngues

2.  Criar uma mídia de distribuição

### <a name="prerequisites"></a>Pré-requisitos
 Para adicionar suporte multilíngue à Instalação do Windows, é preciso:


-   Um computador do técnico que forneça todas as ferramentas e arquivos de origem necessários para criar uma imagem personalizada do WinPE. Para obter mais informações, consulte [Preparar o Computador do Técnico](Prepare-the-Technician-Computer.md).

-   Um computador do técnico que forneça todas as ferramentas e arquivos de origem necessários para criar uma imagem personalizada do WinPE. Para obter mais informações, consulte [Preparar o Computador do Técnico](../install/Prepare-the-Technician-Computer.md).


-   Um DVD do Windows Server Essentials.

-   Um DVD do pacote de idiomas do Windows Server Essentials.

###  <a name="adding-multiple-language-support"></a><a name="BKMK_Steps"></a>Adicionando suporte a vários idiomas
 Para adicionar suporte a vários idiomas ao Instalação do Windows você atualiza o install. wim adicionando os pacotes de idiomas do Windows Server 2012 e do Windows Server Essentials a ele.

#### <a name="update-installwim"></a>Atualizar Install.wim
 Nesta etapa, você adiciona os pacotes de idiomas do Windows Server 2012 e do Windows Server Essentials ao install. wim.

> [!NOTE]
>  Verifique se você instalou os pacotes de idiomas para o Windows Server 2012. Isso garante que você obtenha a identidade visual correta. Os pacotes de idiomas da interface do usuário multilíngüe do Windows Server 2012 estão disponíveis no [Microsoft.com](https://www.microsoft.com/OEM/en/installation/downloads/Pages/technical-downloads.aspx). Siga as instruções descritas no passo a [passo: criação de imagem multilíngüe do Windows na criação de um multilíngue](https://technet.microsoft.com/library/jj126995.aspx) ao criar uma imagem multilíngue do Windows antes de adicionar o pacote de idiomas do Windows Server Essentials ao install. wim.
>
>  Os pacotes de idiomas do Windows Server Essentials estão disponíveis na mídia do pacote de idiomas em \Language packs \\<CultureName \> .

> [!NOTE]
>  Nem todos os pacotes de idiomas podem não estar disponíveis antes do lançamento do Windows Server 2012.

###### <a name="to-add-language-packs-to-installwim"></a>Para adicionar pacotes de idioma a Install.wim

1.  Adicione o sistema operacional e os pacotes de idioma do produto ao Install.wim como segue (neste exemplo, usamos francês):

    ```
    Dism /Mount-Wim /WimFile:C:\my_distribution\sources\install.wim /index:1 /MountDir:C:\InstallMount
    Mkdir c:\temp_Scratch
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<path_to_OS_language_packs>\fr-fr\lp.cab
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<OCD\Language Packs\FR-FR\lp.cab

    ```


2.  Adicione arquivos específicos de idioma para dar suporte à criação da unidade flash USB de restauração de backup do cliente, usando o procedimento descrito em [criar mídia de restauração de cliente em vários idiomas](Build-Multi-Language-Client-Restore-Media.md).

2.  Adicione arquivos específicos de idioma para dar suporte à criação da unidade flash USB de restauração de backup do cliente, usando o procedimento descrito em [criar mídia de restauração de cliente em vários idiomas](../install/Build-Multi-Language-Client-Restore-Media.md).


3.  Recrie o arquivo Lang.ini na mídia separada para refletir o suporte a idiomas adicional usando o comando `DISM /Gen-LangINI` , por exemplo:

    ```
    Dism /image:C:\InstallMount /Gen-LangINI /distribution:C:\my_distribution

    ```

4.  Salve suas alterações de volta na imagem usando o comando `DISM /unmount /commit` , por exemplo:

    ```
    Dism /Unmount-Wim /MountDir:C:\InstallMount /Commit
    ```

## <a name="see-also"></a>Consulte Também

 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md) [personalizações adicionais](Additional-Customizations.md) [preparando a imagem para](Preparing-the-Image-for-Deployment.md) [testar a implantação da experiência do cliente](Testing-the-Customer-Experience.md)

