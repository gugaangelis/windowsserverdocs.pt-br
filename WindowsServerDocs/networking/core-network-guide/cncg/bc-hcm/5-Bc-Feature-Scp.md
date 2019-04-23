---
title: Instalar o recurso BranchCache e configurar o servidor de cache hospedado por ponto de conexão de serviço
description: Este guia fornece instruções sobre como implantar o BranchCache no modo de cache hospedado em computadores que executam o Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 9adf420b-5a58-4e59-9906-71bd58f757fd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6619b09df0d4c161148d22091337a5039c7ea3af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849647"
---
# <a name="install-the-branchcache-feature-and-configure-the-hosted-cache-server-by-service-connection-point"></a>Instalar o recurso BranchCache e configurar o servidor de cache hospedado por ponto de conexão de serviço

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar este procedimento para instalar o recurso BranchCache no seu servidor de cache hospedado, HCS1 e para configurar o servidor para registrar um ponto de Conexão de serviço \(SCP\) nos serviços de domínio do Active Directory \(doADDS\).

Quando você registra os servidores de cache hospedado com um SCP no AD DS, o SCP permite que computadores cliente que estão configuradas corretamente para descobrir automaticamente os servidores de cache hospedado, consultando o AD DS para o SCP. Instruções sobre como configurar computadores cliente para executar esta ação são fornecidas mais adiante neste guia.

>[!IMPORTANT]
>Antes de executar este procedimento, você deve ingressar o computador no domínio e configure o computador com um endereço IP estático.

Para executar esse procedimento, é necessário ser membro do grupo Administradores.

## <a name="to-install-the-branchcache-feature-and-configure-the-hosted-cache-server"></a>Para instalar o recurso BranchCache e configurar o servidor de cache hospedado  

1. No computador do servidor, execute o Windows PowerShell como administrador. Digite o comando a seguir e pressione ENTER.

    ``` 
    Install-WindowsFeature BranchCache
    ```

2.  Para configurar o computador como um servidor de cache hospedado, depois que o recurso BranchCache está instalado e para registrar um ponto de Conexão de serviço no AD DS, digite o seguinte comando no Windows PowerShell e pressione ENTER.

    ```  
    Enable-BCHostedServer -RegisterSCP
    ```  

3. Para verificar a configuração do servidor de cache hospedado, digite o seguinte comando e pressione ENTER.

    ```  
    Get-BCStatus  
    ```  
  
    Os resultados do comando exibem o status de todos os aspectos de sua instalação do BranchCache. A seguir estão algumas das configurações do BranchCache e o valor correto para cada item:  
  
    -   BranchCacheIsEnabled: True

    -   HostedCacheServerIsEnabled: True

    -   HostedCacheScpRegistrationEnabled: True

4. Para preparar para a etapa de copiar seus pacotes de dados de seus servidores de conteúdo para seus servidores de cache hospedado, identificar um compartilhamento existente no servidor de cache hospedado ou crie uma nova pasta e compartilhe a pasta para que ele seja acessível a partir de seus servidores de conteúdo. Depois de criar os pacotes de dados nos servidores de conteúdo, você copiará os pacotes de dados para essa pasta compartilhada no servidor de cache hospedado.
  
5. Se você estiver implantando mais de um servidor de cache hospedado, repita esse procedimento em cada servidor.

Para continuar com este guia, consulte [mova e redimensione o Cache hospedado do &#40;opcional&#41;](6-Bc-Move-Resize-Cache.md).
