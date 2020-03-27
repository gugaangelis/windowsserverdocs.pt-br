---
title: Implantar servidores de cache hospedado (opcional)
description: Este tópico faz parte do guia de implantação do BranchCache para o Windows Server 2016, que demonstra como implantar o BranchCache em modos de cache distribuídos e hospedados para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 96d03b42-6cd9-4905-b6a2-dc36130dd24f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f66e7e728f8e5ca657d002c3c5afcd2d9d23e9ff
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319156"
---
# <a name="deploy-hosted-cache-servers-optional"></a>Implantar servidores de cache hospedado (opcional)

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para instalar e configurar servidores de cache hospedados do BranchCache localizados em filiais em que você deseja implantar o modo de cache hospedado do BranchCache. Com o BranchCache no Windows Server 2016, você pode implantar vários servidores de cache hospedados em uma filial.  
  
> [!IMPORTANT]  
> Essa etapa é opcional porque o modo de cache distribuído não requer um computador de servidor de cache hospedado em filiais. Se você não estiver planejando implantar o modo de cache hospedado em qualquer filial, não será necessário implantar um servidor de cache hospedado e não precisará executar as etapas neste procedimento.  
  
Você deve ser membro de **Administradores**ou equivalente para executar este procedimento.  
  
### <a name="to-install-and-configure-a-hosted-cache-server"></a>Para instalar e configurar um servidor de cache hospedado  
  
1.  No computador que você deseja configurar como um servidor de cache hospedado, execute o comando a seguir em um prompt do Windows PowerShell para instalar o recurso BranchCache.  
  
    `Install-WindowsFeature BranchCache -IncludeManagementTools`  
  
2.  Configure o computador como um servidor de cache hospedado usando um dos seguintes comandos:  
  
    -   Para configurar um computador não ingressado no domínio como um servidor de cache hospedado, digite o seguinte comando no prompt do Windows PowerShell e pressione ENTER.  
  
        `Enable-BCHostedServer`  
  
    -   Para configurar um computador ingressado no domínio como um servidor de cache hospedado e registrar um ponto de conexão de serviço no Active Directory para descoberta automática de servidor de cache hospedado por computadores cliente, digite o seguinte comando no prompt do Windows PowerShell e, em seguida, pressione ENTER.  
  
        `Enable-BCHostedServer -RegisterSCP`  
  
3.  Para verificar a configuração correta do servidor de cache hospedado, digite o seguinte comando no prompt do Windows PowerShell e pressione ENTER.  
  
    `Get-BCStatus`  
  
    > [!NOTE]  
    > Depois de executar esse comando, na seção **HostedCacheServerConfiguration**, o valor de **HostedCacheServerIsEnabled** será **true**. Se você tiver configurado um servidor de cache hospedado associado ao domínio para registrar um SCP (ponto de conexão de serviço) em Active Directory, o valor de **HostedCacheScpRegistrationEnabled** será **true**.  
  

