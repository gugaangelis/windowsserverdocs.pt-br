---
title: Implantar o Cache hospedado servidores (opcional)
description: Este tópico faz parte do BranchCache implantação guia para Windows Server 2016, que demonstra como implantar BranchCache nos modos de cache hospedado e distribuídos para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 96d03b42-6cd9-4905-b6a2-dc36130dd24f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d7345b9acf5ef5003cc2a811569083d7c12894a1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-hosted-cache-servers-optional"></a>Implantar o Cache hospedado servidores (opcional)

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para instalar e configurar os servidores de cache BranchCache hospedado que estão localizados em filiais onde você deseja implantar o modo de cache BranchCache hospedado. Com BranchCache no Windows Server 2016, você pode implantar vários servidores de cache hospedado em uma filial.  
  
> [!IMPORTANT]  
> Esta etapa é opcional, porque o modo de cache distribuído não exige um computador do servidor de cache hospedado em filiais. Se você não planeja implantar hospedado modo cache em qualquer filiais, você não precisa implantar um servidor de cache hospedado e você não precisa executar as etapas neste procedimento.  
  
Você deve ser um membro do **administradores**, ou equivalente a executar este procedimento.  
  
### <a name="to-install-and-configure-a-hosted-cache-server"></a>Instalar e configurar um servidor de cache hospedado  
  
1.  No computador em que você deseja configurar como um servidor de cache hospedado, execute o seguinte comando em um prompt do Windows PowerShell para instalar o recurso BranchCache.  
  
    `Install-WindowsFeature BranchCache -IncludeManagementTools`  
  
2.  Configure o computador como um servidor de cache hospedado usando um dos seguintes comandos:  
  
    -   Para configurar um computador que ingressou sem domínio como um servidor de cache hospedado, digite o seguinte comando no prompt do Windows PowerShell e pressione ENTER.  
  
        `Enable-BCHostedServer`  
  
    -   Para configurar um domínio ingressar o computador como um servidor de cache hospedado e para registrar um ponto de conexão de serviço no Active Directory para descoberta de servidor de cache hospedado automática por computadores cliente, digite o seguinte comando no prompt do Windows PowerShell e pressione ENTER.  
  
        `Enable-BCHostedServer -RegisterSCP`  
  
3.  Para verificar a configuração correta do servidor cache hospedado, digite o seguinte comando no prompt do Windows PowerShell e pressione ENTER.  
  
    `Get-BCStatus`  
  
    > [!NOTE]  
    > Depois de executar esse comando, na seção **HostedCacheServerConfiguration**, o valor para **HostedCacheServerIsEnabled** é **True **. Se você configurou um servidor de cache hospedado participantes de domínio para registrar um ponto de conexão de serviço (SCP) no Active Directory, o valor para **HostedCacheScpRegistrationEnabled** é **True **.  
  

