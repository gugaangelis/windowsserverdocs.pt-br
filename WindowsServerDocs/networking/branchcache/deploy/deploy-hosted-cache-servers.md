---
title: Implantar servidores de cache hospedado (opcional)
description: Este tópico faz parte do BranchCache implantação guia para o Windows Server 2016, que demonstra como implantar o BranchCache nos modos de cache hospedado e distribuído para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 96d03b42-6cd9-4905-b6a2-dc36130dd24f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b19680e933e7a33871816578b63c5a141db0ce00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826207"
---
# <a name="deploy-hosted-cache-servers-optional"></a>Implantar servidores de cache hospedado (opcional)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para instalar e configurar servidores de cache hospedado do BranchCache que estão localizados em filiais em que você deseja implantar o modo de cache hospedado do BranchCache. Com o BranchCache no Windows Server 2016, você pode implantar vários servidores de cache hospedado em uma filial.  
  
> [!IMPORTANT]  
> Esta etapa é opcional, como o modo de cache distribuído não exige um computador do servidor de cache hospedado em filiais. Se você não estiver planejando implantar o cache hospedado em qualquer filiais, não é necessário implantar um servidor de cache hospedado e não é necessário executar as etapas neste procedimento.  
  
Você deve ser um membro da **administradores**, ou equivalente a executar este procedimento.  
  
### <a name="to-install-and-configure-a-hosted-cache-server"></a>Para instalar e configurar um servidor de cache hospedado  
  
1.  No computador que você deseja configurar como um servidor de cache hospedado, execute o seguinte comando em um prompt do Windows PowerShell para instalar o recurso BranchCache.  
  
    `Install-WindowsFeature BranchCache -IncludeManagementTools`  
  
2.  Configure o computador como um servidor de cache hospedado usando um dos seguintes comandos:  
  
    -   Para configurar um computador que ingressou fora do domínio como um servidor de cache hospedado, digite o seguinte comando no prompt do Windows PowerShell e pressione ENTER.  
  
        `Enable-BCHostedServer`  
  
    -   Para configurar um computador de ingressado no domínio como um servidor de cache hospedado e registrar um ponto de conexão de serviço no Active Directory para descoberta de servidor automática de cache hospedado por computadores cliente, digite o seguinte comando no prompt do Windows PowerShell e, em seguida, Pressione ENTER.  
  
        `Enable-BCHostedServer -RegisterSCP`  
  
3.  Para verificar a configuração correta do servidor de cache hospedado, digite o seguinte comando no prompt do Windows PowerShell e pressione ENTER.  
  
    `Get-BCStatus`  
  
    > [!NOTE]  
    > Depois de executar esse comando, na seção **HostedCacheServerConfiguration**, o valor para **HostedCacheServerIsEnabled** está **verdadeiro**. Se você tiver configurado um servidor de cache hospedado ingressado no domínio para registrar um ponto de conexão de serviço (SCP) no Active Directory, o valor para **HostedCacheScpRegistrationEnabled** é **verdadeiro**.  
  

