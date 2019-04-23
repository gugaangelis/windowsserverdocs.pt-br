---
title: Etapa 2 configurar o servidor RADIUS
description: Este tópico faz parte do guia de implantação de acesso remoto com autenticação OTP no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0326818f-9144-496c-b946-f82be4eefbd3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 219c333745d28bdedb9027c1dd46148ac80998f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840867"
---
# <a name="step-2-configure-the-radius-server"></a>Etapa 2 configurar o servidor RADIUS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Antes de configurar o servidor de acesso remoto para dar suporte ao suporte de DirectAccess com a OTP, configure o servidor RADIUS.  
  
|Tarefa|Descrição|  
|----|--------|  
|[2.1. Configurar os tokens de distribuição de software do RADIUS](#BKMK_1.1)|No servidor RADIUS configure tokens de distribuição de software.|  
|[2.2. Configure as informações de segurança RADIUS](#BKMK_1.2)|No servidor RADIUS, configure as portas e o segredo compartilhado a ser usado.|  
|[2.3 adicionando a conta de usuário para a investigação de OTP](#BKMK_Probe)|No servidor RADIUS, crie uma nova conta de usuário para a investigação de OTP.|  
|[2.4 sincronizar com o Active Directory](#BKMK_Active)|No servidor RADIUS crie contas de usuário sincronizadas com contas do Active Directory.|  
|[2.5 configurar o agente de autenticação RADIUS](#BKMK_AuthAgent)|Configure o servidor de acesso remoto como um agente de autenticação de RADIUS.|  
  
## <a name="BKMK_1.1"></a>2.1 configurar os tokens de distribuição de software do RADIUS  
O servidor RADIUS deve ser configurado com a licença necessária e tokens de distribuição de software e/ou hardware a ser usado pelo DirectAccess com a OTP. Esse processo será específico para cada implementação do fornecedor RADIUS.  
  
## <a name="BKMK_1.2"></a>2.2 configurar as informações de segurança RADIUS  
O servidor RADIUS usa portas UDP para fins de comunicação, e cada fornecedor RADIUS tem seus próprio as portas UDP padrão para a comunicação de entrada e saída. Para o servidor RADIUS trabalhar com o servidor de acesso remoto, certifique-se de que todos os firewalls no ambiente são configurados para permitir o tráfego UDP entre os servidores DirectAccess e OTP nas portas necessárias conforme necessário.  
  
O servidor RADIUS usa um segredo compartilhado para fins de autenticação. Configurar o servidor RADIUS com uma senha forte para o segredo compartilhado e observe que isso será usado ao configurar a configuração do computador cliente do servidor DirectAccess para uso com o DirectAccess com OTP.  
  
## <a name="BKMK_Probe"></a>2.3 adicionando a conta de usuário para a investigação de OTP  
No servidor RADIUS, crie uma nova conta de usuário chamada **DAProbeUser** e dê a ele a senha **DAProbePass**.  
  
## <a name="BKMK_Active"></a>2.4 sincronizar com o Active Directory  
O servidor RADIUS deve ter contas de usuário que correspondem aos usuários no Active Directory que estiver usando o DirectAccess com a OTP.  
  
#### <a name="to-synchronize-the-radius-and-active-directory-users"></a>Para sincronizar os usuários do RADIUS e o Active Directory  
  
1.  Registre as informações de usuário do Active Directory para o DirectAccess todos com usuários OTP.  
  
2.  Use o procedimento específico do fornecedor para criar usuário idêntico **domínio \ nomedeusuário** contas que foram registradas no servidor RADIUS.  
  
## <a name="BKMK_AuthAgent"></a>2.5 configurar o agente de autenticação RADIUS  
O servidor de acesso remoto deve ser configurado como um agente de autenticação de RADIUS para o DirectAccess com a implementação de OTP. Siga as instruções do fornecedor RADIUS para configurar o servidor de acesso remoto como um agente de autenticação de RADIUS.  
  


