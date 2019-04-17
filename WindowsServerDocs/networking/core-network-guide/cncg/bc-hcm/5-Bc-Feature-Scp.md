---
title: Instalar o recurso BranchCache e configurar o servidor de Cache hospedado pelo ponto de Conexão de serviço
description: Este guia fornece instruções sobre como implantar BranchCache em modo de cache hospedado em computadores que executam o Windows Server 2016 e o Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 9adf420b-5a58-4e59-9906-71bd58f757fd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 854ff9f80a2221a857fab4e6ea7f7c8e6d51f1ef
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-branchcache-feature-and-configure-the-hosted-cache-server-by-service-connection-point"></a>Instalar o recurso BranchCache e configurar o servidor de Cache hospedado pelo ponto de Conexão de serviço

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar este procedimento para instalar o recurso BranchCache no seu servidor de cache hospedado, HCS1 e para configurar o servidor para registrar um ponto de Conexão de serviço \(SCP\) no Active Directory Domain Services \(AD DS\).

Quando você registra o cache hospedado servidores com um SCP no AD DS, o SCP permite que computadores cliente que estão configurados corretamente para detectar automaticamente o cache hospedado servidores consultando a AD DS para o SCP. Instruções sobre como configurar computadores cliente para realizar essa ação são fornecidas mais adiante neste guia.

>[!IMPORTANT]
>Antes de executar este procedimento, você deve ingressar o computador ao domínio e configurar o computador com um endereço IP estático.

Para executar este procedimento, você deve ser um membro do grupo Administradores.

## <a name="to-install-the-branchcache-feature-and-configure-the-hosted-cache-server"></a>Para instalar o recurso BranchCache e configurar o servidor de cache hospedado  

1. No computador servidor, execute o Windows PowerShell como administrador. Digite o seguinte comando e pressione ENTER.

    ``` 
    Install-WindowsFeature BranchCache
    ```

2.  Para configurar o computador como um servidor de cache hospedado depois que o recurso BranchCache está instalado e registrar um ponto de Conexão de serviço no AD DS, digite o seguinte comando no Windows PowerShell e pressione ENTER.

    ```  
    Enable-BCHostedServer -RegisterSCP
    ```  

3. Para verificar a configuração do servidor de cache hospedado, digite o seguinte comando e pressione ENTER.

    ```  
    Get-BCStatus  
    ```  
  
    Os resultados do comando de exibem o status para todos os aspectos da instalação do BranchCache. A seguir estão algumas das configurações do BranchCache e o valor correto para cada item:  
  
    -   BranchCacheIsEnabled: True

    -   HostedCacheServerIsEnabled: True

    -   HostedCacheScpRegistrationEnabled: True

4. Para preparar para a etapa de copiar seus pacotes de dados de seus servidores de conteúdo para os servidores de cache hospedado, identifique um compartilhamento no servidor de cache hospedado existente ou crie uma nova pasta e compartilhá-la para que ele fique acessível a partir de seus servidores de conteúdo. Depois de criar seus pacotes de dados em seus servidores de conteúdo, você irá copiar os pacotes de dados para essa pasta compartilhada no servidor de cache hospedado.
  
5. Se você estiver implantando mais de um servidor de cache hospedado, repita este procedimento em cada servidor.

Para continuar com este guia, consulte [mover e redimensionar o Cache hospedado #40 opcional &; 41; ](6-Bc-Move-Resize-Cache.md).
