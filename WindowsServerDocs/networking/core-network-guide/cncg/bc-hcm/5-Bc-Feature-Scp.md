---
title: Instalar o recurso BranchCache e configurar o servidor de cache hospedado por ponto de conexão de serviço
description: Este guia fornece instruções sobre como implantar o BranchCache no modo de cache hospedado em computadores que executam o Windows Server 2016 e o Windows 10
manager: brianlic
ms.topic: article
ms.assetid: 9adf420b-5a58-4e59-9906-71bd58f757fd
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7aca1cc28086761b18e559210aeaf21e87612ed3
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87937461"
---
# <a name="install-the-branchcache-feature-and-configure-the-hosted-cache-server-by-service-connection-point"></a>Instalar o recurso BranchCache e configurar o servidor de cache hospedado por ponto de conexão de serviço

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar este procedimento para instalar o recurso do BranchCache em seu servidor de cache hospedado, HCS1, e para configurar o servidor para registrar um SCP de ponto \( \) de conexão de serviço no Active Directory Domain Services \( AD DS \) .

Quando você registra servidores de cache hospedados com um SCP no AD DS, o SCP permite que os computadores cliente configurados corretamente para descobrir automaticamente os servidores de cache hospedados consultando AD DS para o SCP. As instruções sobre como configurar computadores cliente para executar essa ação são fornecidas posteriormente neste guia.

>[!IMPORTANT]
>Antes de executar esse procedimento, você deve ingressar o computador no domínio e configurar o computador com um endereço IP estático.

Para executar esse procedimento, é necessário ser membro do grupo Administradores.

## <a name="to-install-the-branchcache-feature-and-configure-the-hosted-cache-server"></a>Para instalar o recurso BranchCache e configurar o servidor de cache hospedado

1. No computador servidor, execute o Windows PowerShell como administrador. Digite o comando a seguir e pressione ENTER.

    ```
    Install-WindowsFeature BranchCache
    ```

2.  Para configurar o computador como um servidor de cache hospedado após a instalação do recurso do BranchCache e registrar um ponto de conexão de serviço no AD DS, digite o seguinte comando no Windows PowerShell e pressione ENTER.

    ```
    Enable-BCHostedServer -RegisterSCP
    ```

3. Para verificar a configuração do servidor de cache hospedado, digite o comando a seguir e pressione ENTER.

    ```
    Get-BCStatus
    ```

    Os resultados do status de exibição do comando para todos os aspectos da instalação do BranchCache. A seguir estão algumas das configurações do BranchCache e o valor correto para cada item:

    -   BranchCacheIsEnabled: true

    -   HostedCacheServerIsEnabled: true

    -   HostedCacheScpRegistrationEnabled: true

4. Para se preparar para a etapa de copiar seus pacotes de dados dos seus servidores de conteúdo para seus servidores de cache hospedados, identifique um compartilhamento existente no servidor de cache hospedado ou crie uma nova pasta e compartilhe a pasta para que ela possa ser acessada dos seus servidores de conteúdo. Depois de criar seus pacotes de dados em seus servidores de conteúdo, você copiará os pacotes de dados para essa pasta compartilhada no servidor de cache hospedado.

5. Se você estiver implantando mais de um servidor de cache hospedado, repita esse procedimento em cada servidor.

Para continuar com este guia, consulte [mover e redimensionar o cache hospedado &#40;&#41;opcional ](6-Bc-Move-Resize-Cache.md).
