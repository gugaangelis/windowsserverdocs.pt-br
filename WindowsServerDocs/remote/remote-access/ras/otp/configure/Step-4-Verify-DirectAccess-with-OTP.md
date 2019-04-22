---
title: Etapa 4 verificar o DirectAccess com OTP
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
ms.assetid: ed49a0a3-1c45-42e5-8f13-cad20c1c1d68
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bcc152723ee339c8652c9480647bfb8f438c87d6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813237"
---
# <a name="step-4-verify-directaccess-with-otp"></a>Etapa 4 verificar o DirectAccess com OTP

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve como verificar se configurou corretamente seu DirectAccess com a implantação da OTP.
  
### <a name="to-verify-otp-health-on-the-remote-access-server"></a>Para verificar a integridade da OTP no servidor de acesso remoto

1. No servidor de acesso remoto aberto a **gerenciamento de acesso remoto** console.  

2. Sob **servidores de acesso remoto** clique no servidor de acesso remoto que tenha sido configurado para dar suporte a OTP.  

3. Clique em **Status de operações**.  

4. Verifique se o status de OTP exibe o ícone verde e está funcionando.  
  
    > [!NOTE]  
    > O intervalo de atualização de status de integridade será a soma dos valores da chave do registro HKLM\SYSTEM\CCS\Services\Ramgmtsvc\parameters\HealthRefreshTimeout máximo e o **intervalo de tempo de atividade do servidor** que foi definida no Configuração do acesso remoto.  
  
### <a name="to-verify-access-to-internal-resources-using-otp-authentication"></a>Para verificar o acesso a recursos internos usando a autenticação de OTP  
  
1.  Conectar um computador cliente DirectAccess à rede corporativa e execute **gpupdate /force** do prompt de comando para obter a política de grupo.  
  
2.  Desconecte o computador cliente da rede corporativa, conecte-se à rede externa e tentar acessar os recursos internos. Você não deve ter acesso aos recursos internos.  
  
3.  No caso de um token de software, acessar o token OTP do cliente usando as instruções do fornecedor e observe o código de token atual. Quando é usado um token de hardware, siga as instruções do fornecedor para autenticação.  
  
4.  Clique no ícone **Conexões de rede** na área de notificação para acessar o Gerenciador de Mídia do DA.  
  
5.  Clique o **Conexão do DirectAccess**e clique em **continuar**.  
  
6.  Insira o código de token observado anteriormente e clique em **Okey**. Aguarde a autenticação seja concluída. O status de Conexão do DirectAccess no local de trabalho agora será **Connected**.  
  
7.  Tentativa de acessar recursos internos. Você deve conseguir acessar todos os recursos corporativos.  
  


